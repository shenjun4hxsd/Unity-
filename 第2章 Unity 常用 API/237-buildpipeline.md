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

```javascript
	using UnityEditor;
	 
	public class SimpleBundleBuilder
	{
		[MenuItem("Test/Build Asset Bundles")]
	 
		static void BuildABs() {
			// Create the array of bundle build details.
			AssetBundleBuild[] buildMap = new AssetBundleBuild[2];
	 
			buildMap[0].assetBundleName = "enemybundle";
			string[] enemyAssets = new string[2];
			enemyAssets[0] = "char_enemy_alienShip";
			enemyAssets[1] = "char_enemy_alienShip-damaged";
			buildMap[0].assetNames = enemyAssets;
	 
			buildMap[1].assetBundleName = "herobundle";
			string[] heroAssets = new string[1];
			heroAssets[0] = "char_hero_beanMan";
			buildMap[1].assetNames = heroAssets;
	 
			BuildPipeline.BuildAssetBundles("Assets/ABs", buildMap);
		}
	}
```


---

####分开打包

```javascript
	[MenuItem("Custom Editor/Create AssetBunldes Main")]
	static void CreateAssetBunldesMain ()
	{
        //获取在Project视图中选择的所有游戏对象
		Object[] SelectedAsset = Selection.GetFiltered (typeof(Object), SelectionMode.DeepAssets);
 
        //遍历所有的游戏对象
		foreach (Object obj in SelectedAsset) 
		{
			string sourcePath = AssetDatabase.GetAssetPath (obj);
			//本地测试：建议最后将Assetbundle放在StreamingAssets文件夹下，如果没有就创建一个，因为移动平台下只能读取这个路径
			//StreamingAssets是只读路径，不能写入
			//服务器下载：就不需要放在这里，服务器上客户端用www类进行下载。
			string targetPath = Application.dataPath + "/StreamingAssets/" + obj.name + ".assetbundle";
			if (BuildPipeline.BuildAssetBundle (obj, null, targetPath, BuildAssetBundleOptions.CollectDependencies)) {
  				Debug.Log(obj.name +"资源打包成功");
			} 
			else 
			{
 				Debug.Log(obj.name +"资源打包失败");
			}
		}
		//刷新编辑器
		AssetDatabase.Refresh ();	
 
	}
```

最核心的方法其实就它：

BuildPipeline.BuildAssetBundle (obj, null, targetPath, BuildAssetBundleOptions.CollectDependencies)

参数1：它只能放一个对象，因为我们这里是分别打包，所以通过循环将每个对象分别放在了这里。

参数2：可以放入一个数组对象。

默认情况下打的包只能在电脑上用，如果要在手机上用就要添加一个参数。

Android上：

BuildPipeline.BuildAssetBundle (obj, null, targetPath, BuildAssetBundleOptions.CollectDependencies,BuildTarget.Android)

IOS上:

BuildPipeline.BuildAssetBundle (obj, null, targetPath, BuildAssetBundleOptions.CollectDependencies,BuildTarget.iPhone)

另外，电脑上和手机上打出来的Assetbundle不能混用，不同平台只能用自己的。


####将所有对象打包在一个Assetbundle中


```javascript
	[MenuItem("Custom Editor/Create AssetBunldes ALL")]
	static void CreateAssetBunldesALL ()
	{
 
		Caching.CleanCache ();
 
		string Path = Application.dataPath + "/StreamingAssets/ALL.assetbundle";
 
		Object[] SelectedAsset = Selection.GetFiltered (typeof(Object), SelectionMode.DeepAssets);
 
		foreach (Object obj in SelectedAsset) 
		{
			Debug.Log ("Create AssetBunldes name :" + obj);
		}
 
		//这里注意第二个参数就行
		if (BuildPipeline.BuildAssetBundle (null, SelectedAsset, Path, BuildAssetBundleOptions.CollectDependencies)) {
			AssetDatabase.Refresh ();
		} else {
 
		}
	}
```

---

####读取Assetbundle

🔚