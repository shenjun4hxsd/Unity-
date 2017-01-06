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