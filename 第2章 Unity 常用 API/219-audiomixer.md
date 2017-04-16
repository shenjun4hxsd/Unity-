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
|ClearFloat|重设一个暴露的参数为初始值。|
|FindMatchingGroups|Connected groups in the mixer form a path from the mixer's master group to the leaves. This path has the format "Master Group/Child of Master Group/Grandchild of Master Group", so to find the grandchild group in this example, a valid search string would be for instance "randchi" which would return exactly one group while "hild" or "oup/" would return 2 different groups.|
|**FindSnapshot**|通过名字精确查找快照。|
|**GetFloat**|Returns the value of the exposed parameter specified. If the parameter doesn't exist the function returns false. Prior to calling SetFloat and after ClearFloat has been called on this parameter the value returned will be that of the current snapshot or snapshot transition.|
|**SetFloat**|Sets the value of the exposed parameter specified. When a parameter is exposed, it is not controlled by mixer snapshots and can therefore only be changed via this function.|
|TransitionToSnapshots|Transitions to a weighted mixture of the snapshots specified. This can be used for games that specify the game state as a continuum between states or for interpolating snapshots from a triangulated map location.|

```csharp
public bool GetFloat(string name, out float value);
如果暴露的参数不存在，则返回false。
```

```csharp
public Audio.AudioMixerSnapshot FindSnapshot(string name);
```

```csharp
public void TransitionToSnapshots(AudioMixerSnapshot[] snapshots, float[] weights, float timeToReach);

Transitions to a weighted mixture of the snapshots specified. This can be used for games that specify the game state as a continuum between states or for interpolating snapshots from a triangulated map location.
转换为指定快照的加权混合。这可以用于游戏，指定游戏状态之间的状态或快照从三角插值地图位置连续。
```

---

##AudioMixerSnapshot

|方法|说明|
|:--|:--|
|TransitionTo|在指定的时间间隔内执行对该快照的内插转换。|
```csharp
public void TransitionTo(float timeToReach);
```




🔚
