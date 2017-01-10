##AssetBundle

```javascript
	IEnumerator Start () {
		WWW www = new WWW("http://myserver/myBundle.unity3d");
		yield return www;
	
		// Get the designated main asset and instantiate it.
		Instantiate(www.assetBundle.mainAsset);
	}
```


```
isStreamedSceneAssetBundle	Return true if the AssetBundle is a streamed scene AssetBundle.
mainAsset	Main asset that was supplied when building the asset bundle (Read Only).



Public Functions

Contains	Check if an AssetBundle contains a specific object.
GetAllAssetNames	Return all asset names in the AssetBundle.
GetAllScenePaths	Return all the scene asset paths (paths to *.unity assets) in the AssetBundle.
LoadAllAssets	Loads all assets contained in the asset bundle that inherit from type.
LoadAllAssetsAsync	Loads all assets contained in the asset bundle asynchronously.
LoadAsset	Loads asset with name from the bundle.
LoadAssetAsync	Asynchronously loads asset with name from the bundle.
LoadAssetWithSubAssets	Loads asset and sub assets with name from the bundle.
LoadAssetWithSubAssetsAsync	Loads asset with sub assets with name from the bundle asynchronously.
Unload	Unloads all assets in the bundle.
        false 仅删除没有引用的资源
        true 不管bundle中的资源是否在引用，全部删除



Static Functions

LoadFromFile	Synchronously loads an AssetBundle from a file on disk. 必须有UncompressedAssetBundle
LoadFromFileAsync	Asynchronously loads an AssetBundle from a file on disk.
LoadFromMemory	Synchronously create an AssetBundle from a memory region.
LoadFromMemoryAsync	Asynchronously create an AssetBundle from a memory region.
```


🔚