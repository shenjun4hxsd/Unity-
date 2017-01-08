## AssetDatabase

|静态函数|返回类型|解释|描述|注意|
|:---|:---|:---|:---|:---|
|Contains|bool|包含|"资源中是否存在该对象？当对象是一个资源(对应资源文件夹中的一个文件)时返回true，否者返回false(例如场景中的对象或者在运行时生成的对象)。"||  
|CreateFolder|string|创建文件夹|创建一个新的文件夹。||
|IsMainAsset|bool|是否主资源|"该资源在Project窗口中是不是主资源？例如导入的模型有一个游戏物体做为它的根物体以及几个网格和扩充的子物体。在这个例子中根物体就是主资源。"||  
|GenerateUniqueAssetPath|string|生成唯一资源路径|所有的路径都是相对于工程文件夹的，例如”Assets/MyTextures/hello.png”||  
|StartAssetEditing|void|开始资源编辑|开始资源导入。它可以让你把多个资源导入组合成一个更大的导入。||  
|StopAssetEditing|void|停止资源编辑|停止资源导入。这可以让你把多个资源导入组合成一个更大的导入。||  
|ValidateMoveAsset|string|验证移动资源|检查一个资源文件是否可以从一个文件夹移动到另外一个。(并不是真的移动这个文件\)|返回String类型 - 如果资源可以移动返回一个空的字符串，否者返回错误字符串。所有的路径都是相对于工程目录文件。例如” Assets/MyTextures/hello.png”。|  
|MoveAsset \|    string\|    移动资源\|    将一个资源文件从一个文件夹移动到另外一个文件夹。\|    返回String类型 - 如果资源成功共移动返回一个空的字符串,否者返回错误字符串。所有的路径都是相对于工程目录文件。例如” Assets/MyTextures/hello.png”。\|  
\|  
RenameAsset\|    string\|    重命名资源\|    重命名资源文件。\|    返回String类型 - 如果资源成功重命名返回一个空的字符串,否者返回错误字符串。所有的路径都是相对于工程目录文件。例如” Assets/MyTextures/hello.png”。\|  
\|  
MoveAssetToTrash\|    bool\|    移动资源到回收站\|    将指定路径的资源移动到回收站。\|    如果资源已经被成功移动则返回true,如果资源文件不存在或者不能成功移动到回收站返回false。所有的路径都是相对于工程目录文件。例如” Assets/MyTextures/hello.png”。\|  
\|  
DeleteAsset \|    bool\|    删除资源\|    删除指定路径的资源文件。\|    如果资源已经被成功删除返回true,如果资源文件不存在或者不能成功移动到回收站返回false。所有的路径都是相对于工程目录文件。例如” Assets/MyTextures/hello.png”。\|

\|  
ImportAsset\|    void\|    导入资源\|    导入指定路径的资源。    所有的路径都是相对于工程目录文件。 例如” Assets/MyTextures/hello.png”。  
CopyAsset     bool    复制资源    复制指定路径的资源文件到一个新的路径。    所有的路径都是相对于工程目录文件。 例如” Assets/MyTextures/hello.png”。  
CreateAsset     void    新建资源    在指定的路径新建资源。    "你必须保证使用的路径是一个被支持的扩展\('.mat' 代表 materials， '.cubemap' 代表 cubemaps， '.GUISkin' 代表 skins， '.anim' 代表 animations and '.asset' 代表任意其他的资源文件。\)  
当资源被创建后，你可以使用AssetDatabase.AddObjectToAsset把更多的资源添加到文件。如果资源已经存在于指定路径，那么这将会删除原有的资源并新建。"  
AddObjectToAsset     void    添加对象到资源    将objectToAdd这个对象添加到指定路径上的现有资源。    请注意，你应该添加资源到”.assets”目录下的资源中，例如导入的模型或纹理将会丢失它们地数据。  
GetAssetPath     string    获取资源路径    返回相对于工程资源位置的路径名。  
GetTextMetaDataPathFromAssetPath     string    从资源路径获取文本Meta数据路径    获取资源有关文本.meta文件的路径。    返回字符串类型，.meta文本文件的路径或空字符串，如果文件不存在。  
LoadAssetAtPath     Object    指定路径加载资源    返回在指定位置assetPath下第一个类型是type的资源对象。    参数assetPaht不需要区分大小写。在Unity中所有的资源名称和路径都需要斜杠/。  
LoadMainAssetAtPath     Object    指定路径加载主资源    返回指定路径assetPath下的主资源。  
LoadAllAssetRepresentationsAtPath     Object\[\]    加载所在路径所有资源说明    返回assetPath所在路径的所有资源说明。    资源说明是资源在项目视图可见的物体。  
LoadAllAssetsAtPath     Object\[\]    指定路径加载所有资源    返回指定路径assetPath下的所有资源对象数组。    有些资源文件可能包含许多对象。\(例如一个maya文件可能包含许多个网格和游戏物体\)。  
Refresh     void    刷新    重新导入有更新的资源。  
OpenAsset     bool    打开资源    用关联的程序打开资源文件。    使用外部编辑器。纹理引用程序或者模型工具来打开不同资源类型的资源，如果是文本文件，行编号会指引文本编辑器找到对应行。  
AssetPathToGUID    string    资源路径转GUID    返回指定路径path下资源的GUID\(全局唯一标识符\)。  
SaveAssets     void    保存资源    将所有未保存的资源更改写入磁盘。  
GetCachedIcon     Texture    获取缓存的图标    为指定路径的资源检索一个图标。  
CompleteLabel     string\[\]    完成的标签    返回符合给定前缀的所有标签。  
SetLabels     void    设置标签列表    替换资源的标签列表。  
GetLabels     void    获取标签列表    返回给定资源拥有的所有标签。  
ClearLabels     void    清除标签    移除资源上所有的标签。  
GetDependencies     string\[\]    获取相关资源列表    已知资源pathName，返回与它有依赖关系的所有资源列表。  
ExportPackage     void    导出包    导出assetPahtNames定义的资源到一个unitypackage里，并命名filename。  
ImportPackage     void    导入包    将packagePath下的资源包导入到当前工程。  
FindAssets    string\[\]        Search the assetdatabase using a searchfilter string.  
GetAssetOrScenePath    string        Returns the path name relative to the project folder where the asset is stored.  
GUIDToAssetPath    string        Translate a GUID to its current asset path.  
IsOpenForEdit    bool        Use IsOpenForEdit to determine if the asset is open for edit by the version control.  
IsSubAsset    bool        Does the asset form part of another asset?  
WriteImportSettingsIfDirty    bool        Writes the import settings to disk.

