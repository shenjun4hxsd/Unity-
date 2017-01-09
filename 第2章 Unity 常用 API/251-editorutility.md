##EditorUtility

Static Functions

|静态方法|说明|
|:--|:--|
|ClearProgressBar|	Removes progress bar.|
|CollectDeepHierarchy|	Collect all objects in the hierarchy rooted at each of the given objects.|
|CollectDependencies|	Calculates and returns a list of all assets the assets listed in roots depend on.|
|CompressTexture|	Compress a texture.压缩图片到相应格式|
|CopySerialized	|Copy all settings of a Unity Object.拷贝一个Object所有的属性设置等到另外一个Object|
|CopySerializedIfDifferent|	Copy all settings of a Unity Object to a second Object if they differ.|
|CreateGameObjectWithHideFlags|	Creates a game object with HideFlags and specified components.|
|DisplayCancelableProgressBar|	Displays or updates a progress bar that has a cancel button.|
|**DisplayDialog**|	Displays a modal dialog.显示一个对话框，类似于MessageBox，有Yes、No按钮|
|DisplayDialogComplex|	Displays a modal dialog with three buttons.|
|DisplayPopupMenu|	Displays a popup menu.|
|DisplayProgressBar|	Displays or updates a progress bar.|
|ExtractOggFile|	Saves an AudioClip or MovieTexture to a file.|
|FocusProjectWindow|	Brings the project window to the front and focuses it.|
|FormatBytes|	Returns a text for a number of bytes.|
|GetObjectEnabled|	Is the object enabled (0 disabled, 1 enabled, -1 has no enabled button).|
|InstanceIDToObject|	Translates an instance ID to a reference to an object.|
|IsPersistent|	Determines if an object is stored on disk.这个函数用于判定是否对象是被保存到硬盘中的。|
|NaturalCompare|	Human-like sorting.|
|OpenFilePanel|	Displays the "open file" dialog and returns the selected path name.选择/保存一个文件的文件摄取窗口。|
|OpenFilePanelWithFilters|	Displays the "open file" dialog and returns the selected path name.|
|**OpenFolderPanel**|	Displays the "open folder" dialog and returns the selected path name.选择/保存一个文件夹的文件夹摄取窗口|
|SaveFilePanel|	Displays the "save file" dialog and returns the selected path name.选择/保存一个文件的文件摄取窗口。|
|SaveFilePanelInProject|	Displays the "save file" dialog in the Assets folder of the project and returns the selected path name.|
|SaveFolderPanel|	Displays the "save folder" dialog and returns the selected path name.选择/保存一个文件夹的文件夹摄取窗口|
|SetDirty|	Marks target object as dirty. (Only suitable for non-scene objects).这个函数告诉引擎，相关对象所属于的Prefab已经发生了更改。方便，当我们更改了自定义对象的属性的时候，自动更新到所属的Prefab中。|
|SetObjectEnabled|	Set the enabled state of the object.|
|SetSelectedRenderState|	Set the Scene View selected display mode for this Renderer.|
|UnloadUnusedAssetsImmediate|	Unloads assets that are not used.|


🔚