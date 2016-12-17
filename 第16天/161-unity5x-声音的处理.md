##Unity5.x 声音的处理

####AudioMixer

    AudioMixerGroup
        把AudioSource中的声音通过AudioMixerGroup来处理一下

    AudioMixerSnapShot
        保存的是所有可以使用的AudioMixerGroup的状态
        根据不同的场景对于音效的控制做单独的处理

    举例：
        驾驶舱：发动机的声音的音量可以大些，外部的战斗声音小些
        射击仓：发动机的声音的音量可以小些，战斗的生意大些
        投弹仓：发动机的声音更小些，战斗的声音更小，炸弹爆炸的声音大些
        
        
        
![](/assets/AudioMixer01.png)


![](/assets/AudioMixer02.png)

```
S：将停止除自己以外的其他音效的播放(添加当前组到一个独奏的组集合中)
M：将当前组静音
B：不播放当前组的效果
```

运行后，AudioMixer面板将出现 `Edit in play mode` 按钮，点击 AudioMixer 面板的 Editor in Play Mode 这样我们在运行状态下编辑的 Group，在停止运行时将保存。

---

####AudioSource
        output
            指定哪个AudioMixerGroup来处理这个AudioSource中播放的音效。

####新版与旧版的区别
    AudioListener
        AudioSource

    AudioListener
        AudioMixer
            AudioSource
        AudioSource

####Effect
    Receive
    Send
        发送声音信号
        多个Send可以发送给同一个Receive
    DuckVolume