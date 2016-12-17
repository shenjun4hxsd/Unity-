##AudioMixer

描述：
AudioMixer asset.

This is a singleton representing a specific audio mixer asset in the project.


|变量|说明|
|:--|:--|
|outputAudioMixerGroup|Routing target.|
|updateMode|How time should progress for this AudioMixer. Used during Snapshot transitions.|


|函数|说明|
|:--|:--|
|ClearFloat|Resets an exposed parameter to its initial value.|
|FindMatchingGroups|Connected groups in the mixer form a path from the mixer's master group to the leaves. This path has the format "Master Group/Child of Master Group/Grandchild of Master Group", so to find the grandchild group in this example, a valid search string would be for instance "randchi" which would return exactly one group while "hild" or "oup/" would return 2 different groups.|
|FindSnapshot|The name must be an exact match.|
|GetFloat|Returns the value of the exposed parameter specified. If the parameter doesn't exist the function returns false. Prior to calling SetFloat and after ClearFloat has been called on this parameter the value returned will be that of the current snapshot or snapshot transition.|
|SetFloat|Sets the value of the exposed parameter specified. When a parameter is exposed, it is not controlled by mixer snapshots and can therefore only be changed via this function.|
|TransitionToSnapshots|Transitions to a weighted mixture of the snapshots specified. This can be used for games that specify the game state as a continuum between states or for interpolating snapshots from a triangulated map location.|

```
public bool GetFloat(string name, out float value);
如果暴露的参数不存在，则返回false。
```
