##SceneManager 场景管理

描述：运行时的场景管理方法。

|静态变量|说明|
|:--|:--|
|sceneCount|场景的总数。|
|sceneCountInBuildSettings|在BuildSettings中的场景数量。|


|静态函数|说明|
|:--|:--|
|CreateScene|创建一个空的新场景并指定名称。|
|GetActiveScene|获取激活的场景。|
|GetSceneAt|获取在场景管理列表添加场景的索引。|
|GetSceneByName|通过场景添加到场景管理器的指定名称查找该场景。|
|GetSceneByPath|查找所有给定资源路径并添加到SceneManager中的场景。|
|LoadScene|通过在Build Settings中它们的名称或索引加载场景。|
|LoadSceneAsync|在背景中异步加载场景。|
|MergeScenes|该属性将场景资源合并到destinationScene。该函数合并场景资源内容到目的场景并删除原场景资源。所有根资源场景下的物体移动到根目的场景目录下。注意：该函数是有破坏性的：当资源合并完成后原有资源场景将会被立刻销毁。|
|MoveGameObjectToScene|移动一个物体从它当前场景到一个新的场景中。它必须要求该物体在当前场景的根目录下。|
|SetActiveScene|设置被激活的场景。|
|UnloadScene|卸载所有和指定场景关联的物体。|