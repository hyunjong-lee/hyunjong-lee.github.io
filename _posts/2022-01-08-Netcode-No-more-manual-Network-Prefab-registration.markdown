---
title: Netcode - No more manual Network Prefab Registration @ Inspector
key:   20220108
date:  2022-01-08 11:00:31
tags:  C# Unity Netcode
---

This post contains the implementation method to easily register `NetworkPrefabs` of [Netcode](https://unity.com/products/netcode) at component view. It is not an official Unity guide, so caution is required when using it.

![Place to register dynamic spawned prefabs](/assets/images/netcode/register_dynamic_spawned_prefabs.png){:.rounded}

# TL;DR

- Modify (Unity) yaml directly to insert `NetworkPrefabs` directly.
- Make one-click button to search and register to `NetworkPrefabs`.
- Or, use [NetworkManager.Singleton.AddNetworkPrefab](https://docs-multiplayer.unity3d.com/netcode/current/api/Unity.Netcode.NetworkManager/index.html#addnetworkprefabgameobject) @runtime instead.
- If you want to register @ Inspector, use the following codes.
  - [NetcodeRegister](https://github.com/hyunjong-lee/NetcodeExample/blob/main/Assets/Scripts/Netcode/NetcodeRegister.cs)
  - [NetcodeRegisterButton](https://github.com/hyunjong-lee/NetcodeExample/blob/main/Assets/Scripts/Editor/NetcodeRegisterButton.cs)

<!--more-->

# Netcode?

Netcode is the new server-client architecture to develop server/client logics like [dedicated server](https://docs.unrealengine.com/5.1/en-US/networking-overview-for-unreal-engine/) in Unreal Engine.
With Netcode, we can easily develop server-client games conceptually.
In my case, I am struggling to use `Netcode` well.
Read [the documents](https://docs-multiplayer.unity3d.com/netcode/current/about) if you want to know more details.

## Install

I created an empty unity project [NetcodeExample](https://github.com/hyunjong-lee/NetcodeExample), and installed the following packages.

- (Required) Install [`com.unity.netcode.gameobjects`](https://docs-multiplayer.unity3d.com/netcode/current/installation/install/index.html).
- (Recommended) Install [`com.unity.multiplayer.tools`](https://docs-multiplayer.unity3d.com/tools/current/install-tools) too.

![Install Netcode package](/assets/images/netcode/netcode_package_install.png){:.rounded}

## Dynamic Spawn

There are roughly two ways to spawn network objects. The first is just place objects in the scene. Then, when the scene is loaded, the objects will be spawned by Netcode. The second is instantiating an object dynamically by [`Instantiate`](https://docs.unity3d.com/ScriptReference/Object.Instantiate.html) method and spawn it by [`GetComponent<NetworkObject>().Spawn()`](https://docs-multiplayer.unity3d.com/netcode/current/basics/object-spawning/index.html).
This post is focusing on the second way.

To use dynamic spawn, we need to register prefabs to `NetworkPrefabs` in `NetworkManager` component of Netcode.

### Network Manager

The following image shows Network Manager component addition.
It is included in Netcode package.

![Add Network Manager Component](/assets/images/netcode/add_network_manager_component.png){:.rounded}

Network Manager component has a field named `NetworkPrefabs` in the following image.
Prefabs to spawn dynamically have to be registered in there.

![Place to register dynamic spawned prefabs](/assets/images/netcode/register_dynamic_spawned_prefabs.png){:.rounded}

If there are just few prefabs, it is okay to register manually by drag & drop. But, if we have tens of? or hundreds of?
I was very frustrated when I registered tens of prefabs.
If it is okay to use APIs instead of explicit registration, [AddNetworkPrefab](https://docs-multiplayer.unity3d.com/netcode/current/api/Unity.Netcode.NetworkManager/index.html#addnetworkprefabgameobject) can be used to register both server and client sides.
But I wanted to show prefabs @ component view.
So I started to develop the codes in [NetcodeExample](https://github.com/hyunjong-lee/NetcodeExample)

I created a dog prefab to show an example.

- [`Network Object`](https://docs-multiplayer.unity3d.com/netcode/current/basics/networkobject/index.html) is required to use Netcode features.
- [`NetworkTransform`](https://docs-multiplayer.unity3d.com/netcode/current/components/networktransform/index.html) synchronizes positions.

![An example prefab - dog](/assets/images/netcode/doc_prefab.png){:.rounded}

# One-click Register

Netcode prefab has `Network Manager` in Netcode.
I dig in to the `Netcode.prefab` file to reveal the structure of the file.
To figure out how can I modify the file to register network prefabs by code instead of drag & drop.

![Make a prefab which contains NetworkManager component](/assets/images/netcode/NetworkManager_as_prefab.png){:.rounded}

I created [`NetcodeRegister`](https://github.com/hyunjong-lee/NetcodeExample/blob/main/Assets/Scripts/Netcode/NetcodeRegister.cs) class to implement one-click register.

## YAML - Netcode.prefab

The first thing is to understand `Netcode.prefab` file.
Prefab files uses (Unity) yaml syntax.
You can find out what is yaml in [here](https://www.tutorialspoint.com/yaml/yaml_introduction.htm).
I recognize yaml as simplified json format.
The following code shows the contens of `Netcode.prefab`.

{% highlight yaml linenos %}

%YAML 1.1
%TAG !u! tag:unity3d.com,2011:
...
  NetworkConfig:
    ProtocolVersion: 0
    NetworkTransport: {fileID: 8661109935012272639}
    PlayerPrefab: {fileID: 0}
    NetworkPrefabs: []
    TickRate: 30
    ClientConnectionBufferTimeout: 10
    ConnectionApproval: 0
...
  DebugSimulator:
    PacketDelayMS: 0
    PacketJitterMS: 0
    PacketDropRate: 0

{% endhighlight %}

`NetworkPrefabs: []` is defined @ line 8 in the above code.
The list will be modified by code.
For that, we have to know what happens when a network prefab is registered.
When I registered the `Dog` prefab, `NetworkPrefabs: []` changed like the following code.

{% highlight yaml linenos %}

...
  NetworkConfig:
    ProtocolVersion: 0
    NetworkTransport: {fileID: 8661109935012272639}
    PlayerPrefab: {fileID: 0}
    NetworkPrefabs:
    - Override: 0
      Prefab: {fileID: 979486745327824490, guid: 3077113299775460f89cbf89f0a0cb56, type: 3}
      SourcePrefabToOverride: {fileID: 0}
      SourceHashToOverride: 0
      OverridingTargetPrefab: {fileID: 0}
    TickRate: 30
    ClientConnectionBufferTimeout: 10
...

{% endhighlight %}

And then, I figured out that I must know how to extract `fileID` and `guid` from prefab files.

## Prefab FileID & guid

In UnityEditor, find out paths of prefab files is easy. Just use `Directory.EnumerateFiles`.
The hard thing in here is that find out `FileID` and `guid`.
With some struggles, I figured out that the ids are belong to the first `GameObject` of each prefab like the following code.

{% highlight c# linenos %}

var assetPath = "Assets/Test/Example.prefab";
var obj = AssetDatabase.LoadAllAssetsAtPath(assetPath).First(e => e.GetType() == typeof(GameObject));
if (AssetDatabase.TryGetGUIDAndLocalFileIdentifier(obj, out var guid, out long fileId))
{
}

{% endhighlight %}

## Inject into NetworkManager

Now, it is time to mix all finds out.
We know prefabs' fileIDs and guids, just write to them `Netcode.prefab`.

{% highlight c# linenos %}

public class NetcodeRegister : MonoBehaviour
{
    [SerializeField] private string RootPath;

    public void RegisterNetworkPrefabs()
    {
        var netcodePrefabPath = Path.Join(Application.dataPath, "Prefab/Netcode.prefab");
        var lines = File.ReadAllLines(netcodePrefabPath);

        var builder = new StringBuilder();

        var pos = 0;
        while (!lines[pos].Trim().StartsWith("NetworkPrefabs"))
        {
            builder.AppendLine(lines[pos]);
            pos++;
        }

        var spaces = lines[pos].Substring(0, lines[pos].IndexOf('N'));
        builder.AppendLine(string.Concat($"{spaces}NetworkPrefabs:"));
        var searchPath = Path.Join(Application.dataPath, RootPath);
        foreach (var prefabPath in Directory.EnumerateFiles(searchPath, "*.prefab", SearchOption.AllDirectories))
        {
            var assetPath = Path.Join("Assets", prefabPath.Replace(Application.dataPath, ""));
            var obj = AssetDatabase.LoadAllAssetsAtPath(assetPath).First(e => e.GetType() == typeof(GameObject));
            if (AssetDatabase.TryGetGUIDAndLocalFileIdentifier(obj, out var guid, out long fileId))
            {
                builder.Append(spaces);
                builder.AppendLine("- Override: 0");
                builder.Append(spaces);
                builder.AppendLine("  Prefab: {fileID: @@FILEID@@, guid: @@GUID@@, type: 3}"
                    .Replace("@@FILEID@@", fileId.ToString())
                    .Replace("@@GUID@@", guid));
                builder.Append(spaces);
                builder.AppendLine("  SourcePrefabToOverride: {fileID: 0}");
                builder.Append(spaces);
                builder.AppendLine("  SourceHashToOverride: 0");
                builder.Append(spaces);
                builder.AppendLine("  OverridingTargetPrefab: {fileID: 0}");
            }
        }

        Debug.Log(builder);

        while (++pos < lines.Length && lines[pos][spaces.Length] is ' ' or '-') ;
        while (pos < lines.Length) builder.AppendLine(lines[pos++]);

        Debug.Log(builder);

        File.WriteAllText(netcodePrefabPath, builder.ToString());
    }
}

{% endhighlight %}

This is all to register network object prefabs by code in UnityEditor.
Now let's add a button @ Inspector to execute `RegisterNetworkPrefabs` method.

## One-click Button

I referenced the following video to implement add button on Inspector.

<div>{% include extensions/youtube.html id='frzTY4iUy-8' %}</div>


The following shows the code.


{% highlight c# linenos %}

[CustomEditor(typeof(NetcodeRegister))]
public class NetcodeRegisterButton : Editor
{
    public override void OnInspectorGUI()
    {
        DrawDefaultInspector();

        var register = (NetcodeRegister) target;
        if (GUILayout.Button("Register"))
        {
            register.RegisterNetworkPrefabs();
        }
    }
}

{% endhighlight %}

The following images show before clicking the button and after clicking the button respectively.

![Before clicking Register Button](/assets/images/netcode/button_click_before.png){:.rounded}

![After clicking Register Button](/assets/images/netcode/button_click_after.png){:.rounded}

# Wrap up

I was trying to figure out how can I register dynamic spawned network prefabs by code instead of drag & drop @ Inspector.
During digging and digging, I could understand Unity YAML syntax and how a prefab is registered as yaml.
The most difficult thing was to figure out `fileID` and `guid`.

The whole experiences helped me a lot to understand Unity.

## Source Code

You can find the entire project source code in [https://github.com/hyunjong-lee/NetcodeExample](https://github.com/hyunjong-lee/NetcodeExample). If there are some tricky cases like this, I will update the repository with new blog postings.
