##BuildPipeline 编译管线

**Description 描述**

让你可以以编程方式生成程序或资源包， 以便于从网络中加载。

|静态变量|说明|
|:--|:--|
|isBuildingPlayer|当前是否正在编译程序？|

|静态函数|说明|
|:--|:--|
|BuildAssetBundles|在编辑模式编译所有指定的资源。|
|BuildPlayer|编译一个项目。|
|GetCRCForAssetBundle|提取给定资源包的CRC校验。|
|GetHashForAssetBundle|提取指定资源包的哈希值。|

---

####BuildPipeline.BuildAssetBundles 编译资源包

public static AssetBundleManifest BuildAssetBundles(string outputPath, BuildAssetBundleOptions assetBundleOptions = BuildAssetBundleOptions.None, BuildTarget targetPlatform = BuildTarget.WebPlayer);


|参数|说明|
|:--|:--|
|outputPath|资源包的输出路径。|
|assetBundleOptions|资源包编译选项。|
|targetPlatform|目标编译平台|

**Description 描述**

在编辑编译所有指定的资源。

在Unity5.0，在编辑器中让你在包含被命名的资源包中标记资源。这个函数实际上是在编辑器中编译你指定的资源，如果编译成功返回true，否则返回false。此外，错误消息显示说明最常见的编译失败，如不正确的目标文件夹路径。

outputPath是资源要保存的路径，资源将被编译保存到那里（例如，Assets/MyBundleFolder）。该文件夹不会自动创建，如果不存在该函数会简单返回错误。

可选项assetBundleOptions是修改资源包的构建方式，targetPlatform是部署资源要使用的目标平台（如果pc，手机等）。注意，编译为pc平台的资源包，不兼容手机等其他平台，你需要为不同的平台编译不同版本的资源包。

```javascript
	using UnityEditor;
	 
	public class SimpleBundleBuilder
	{
		[MenuItem("Simple Bundles/Build")]
		[MenuItem("Test/Build Asset Bundles")]
		static void BuildABs() {
			// Put the bundles in a folder called "ABs" within the
			// Assets folder.
			BuildPipeline.BuildAssetBundles("Assets/ABs");
		}
	}
```


---

public static AssetBundleManifest BuildAssetBundles(string outputPath, AssetBundleBuild[] builds, BuildAssetBundleOptions assetBundleOptions = BuildAssetBundleOptions.None, BuildTarget targetPlatform = BuildTarget.WebPlayer);

|参数|说明|
|:--|:--|
|outputPath|资源包的输出路径。|
|assetBundleOptions|资源包编译选项。|
|targetPlatform	Target|目标编译平台。|
|builds|资源包编译地图。|







🔚