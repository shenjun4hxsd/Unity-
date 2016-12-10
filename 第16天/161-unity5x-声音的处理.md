##Unity5.x 声音的处理

####AudioMixer

    AudioMixerGroup
        把AudioSource中的声音通过AudioMixerGroup来处理一下

    AudioMixerSnapShot
        保存的是所有可以使用的AudioMixerGroup的状态
        根据不同的场景对于音效的控制做单独的处理

    举例：
        驾驶舱：发动机的声音的音量可以大些，外部的战斗声音小些
        设计仓：发动机的声音的音量可以小些，战斗的生意大些
        投弹仓：发动机的声音更小些，战斗的声音更小，炸弹爆炸的声音大些


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