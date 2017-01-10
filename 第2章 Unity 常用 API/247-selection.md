## Selection

| 变量 | 类型 | 解释 | 描述 | 注意 |
| :--- | :--- | :--- | :--- | :--- |
|  | transforms | Transform\[\] | 变换列表    返回顶层的选择物，不包括预设物。 |  |
| activeTransform | Transform | 激活变换 | 返回激活的变换。（在检查面板中显示） | 这个将绝不返回预设物或者不可修改的物体。 |
| gameObjects | GameObject\[\] | 游戏物体列表 | 返回实际选择的游戏物体。包括预设物，不可修改的物体。 | 当使用场景中的主要物体时，强烈推荐使用Selection.transforms来代替。 |
|  |  |  |  |  |
| activeGameObject | GameObject | 激活游戏物体 | 返回激活的游戏物体。（在检查面板中显示） | 它同样回返的可能是预设的或者不可修改的游戏物体。 |
|  |  |  |  |  |
| activeObject | Object | 激活物体 | 返回选择的激活的物体。包括预设物，不可改动的物体。 | 当使用场景中的主要物体时，强烈推荐使用Selection.activeTransform来代替。 |
| activeInstanceID | int | 激活实例ID | 返回实际选择的激活的物体的实例ID。包括预设物，不可改动的物体。 |  |
| objects | Object\[\] | 物体列表 | 来自场景中实际未过滤的选择物。 | 只有在场景中或者检测面板中的物体将会被返回，在工程试图里的则不能。你也可以赋值一个对象到选择。 |
| instanceIDs | int\[\] | 实例ID列表 | 在场景中激活的过滤选择返回实例的ID代替objects。 | 一个物体的实例ID总是保证是独一无二的。 |
|  |  |  |  |  |
| assetGUIDs | string\[\] |  | Returns the guids of the selected assets. |  |

 

| 函数 | 返回类型 | 解释 | 描述 |
| :--- | :--- | :--- | :--- |
| Contains | bool | 是否包含 | 返回一个物体是否被包含在当前的选择中。 |
| GetTransforms | Transform\[\] | 获取变换列表 | 允许对选择类型进行精细的控制，使用SelectionMode枚举类型。 |
| GetFiltered | Object\[\] | 获取过滤后物体 | 返回通过类型和选择模式过滤的当前选择的物体。 |

---

### SelectionMode

| 值 | 解释 | 描述 |
| :--- | :--- | :--- |
| Unfiltered | 不过滤 | 返回整个选择。 |
| TopLevel | 顶层选择 | 仅返回最顶层选择的变换；其他选择的子对象将被过滤出。 |
| Deep | 深度 | 返回选择和所有选择的子变换。 |
| ExcludePrefab | 排除预置 | 从选择中排除任何预制。 |
| Editable | 可编辑 | 排除任何不得修改的对象。从导入到fbx文件，这将过滤生成的预置，但用户不能创建预置。 |
| Assets | 资源 | 仅返回资源目录中的资源对象。 |
| DeepAssets | 深度资源 | 如果选择包含文件夹，也包含所有资源和子目录。 |

🔚

