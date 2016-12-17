##Unity5.x 声音的处理

####AudioMixer 音效混合器

    AudioMixerGroup
        把AudioSource中的声音通过AudioMixerGroup来处理一下

    AudioMixerSnapShot
        快照会捕获AudioMixer内所有参数的状态。
        保存的是所有可以使用的AudioMixerGroup的状态
        根据不同的场景对于音效的控制做单独的处理

    举例：
        驾驶舱：发动机的声音的音量可以大些，外部的战斗声音小些
        射击仓：发动机的声音的音量可以小些，战斗的生意大些
        投弹仓：发动机的声音更小些，战斗的声音更小，炸弹爆炸的声音大些
        
        
        
![](/assets/AudioMixer01.png)


![](/assets/AudioMixer02.png)

```
S：添加当前组到一个独奏的组集合中
M：将当前组静音
B：不播放当前组的效果
```

运行后，AudioMixer面板将出现 `Edit in Play Mode` 按钮，点击 AudioMixer 面板的 `Editor in Play Mode` 这样我们在运行状态下编辑的 Group，在停止运行时将保存。

---

####Effect

（1）Send 会将这个Group 的信号传递出去，多个Send可以发送给同一个Recevier。 

（2）Receive 接收其他 Group 发出的信号

（3）Duck Volume ，接收其他Group发送的信号，改变自己的一些属性，比如音量大小。

（4） Low Pass 低通滤波器，即只允许低频率的音效播放出来，而高频率的音效不播放出来

（5）High Pass 高通滤波器，即只允许高频率的音效播放出来，而低频率的音效不播放出来 

（6）Echo 回声，即添加该效果，该 Grop 播放的声音将会产生回音 

（7）Flang镶边效果，用来产生奇怪的音效 


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