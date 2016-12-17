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
        射击仓：发动机的声音的音量可以小些，战斗的声音大些
        投弹仓：发动机的声音更小些，战斗的声音更小，炸弹爆炸的声音大些
        
        
        
![](/assets/AudioMixer01.png)

&emsp;

![](/assets/AudioMixer02.png)

```
S：添加当前组到一个独奏的组集合中
M：将当前组静音
B：不播放当前组的效果
```
```
peak = 实时的峰值
rms = 方均根, 一定时限内的平均值
rms 使用上比较接近耳朵的感受, peak 就实实在在的电子数值.
```

运行后，AudioMixer面板将出现 `Edit in Play Mode` 按钮，点击 AudioMixer 面板的 `Editor in Play Mode` 这样我们在运行状态下编辑的 Group，在停止运行时将保存。

在播放时，开启Edit in Play Mode之后, 任何做过的改变都会被保留。并且此时无法通过脚本修改参数。(比如通过AudioMixer.SetFloat来进行参数修改，或者通过AudioMixerSnapshot.TransitionTo()来转换SnapShot都会无效, 这也是必然的)。


---

####Effect

（1）Send 会将这个Group 的信号传递出去，多个Send可以将信号发送给同一个Recevie。 

（2）Receive 接收其他 Group 发出的信号

（3）Duck Volume ，接收其他Group发送的信号，改变自己的一些属性，比如音量大小。

（4） Low Pass 低通滤波器，即只允许低频率的音效播放出来，而高频率的音效不播放出来

（5）High Pass 高通滤波器，即只允许高频率的音效播放出来，而低频率的音效不播放出来 

（6）Echo 回声，即添加该效果，该 Grop 播放的声音将会产生回音 

（7）Flange镶边效果器，用于创建迷幻飘渺的声音。

（8）Distortion失真，直接让信号听上去从30mb的MP3变成1mb的渣音质。

（9）Normalize 用了之后音质会变得非常恶心。

（10）ParamEQ，也称Parametric Equalizer。参数均衡器, 是Sonar里面的
![](/assets/AudioMixer03.png)
这种Equalizer的弱化版，一个效果器只允许调节一条曲线。当然可以存在多个这样的效果器来实现Sonar里的Equalizer的效果。比如这样
![](/assets/AudioMixer04.png)
就是降低高频并且提升低频的效果。

（11）Pitch Shifter听上去很有意思。这个更改Pitch的操作与更改Group总体的Pitch的操作的原理不同，因为更改Group总体的Pitch直接等于修改播放速度了，通过加快速度来提升音高或者通过降低速度来减低音高。而Pitch Shifter可以在播放速度不变的情况下改变音高，也不愧需要消耗这么多CPU资源了(选中Group在Inspector视图点击齿轮图标可以看到显示CPU消耗的选项)。一个男声变成女声，或者一个女声变成男声。很好玩。

（12）Chorus虽说是合唱效果器，但是当前版本中完全没有达到正常音序器的合唱效果器的水准。怎么说呢，希望加强吧。

（13）Compressor降低信号中音量最大的部分。

（14）SFX Reverb混响效果，还不错。

（15）Low Pass Simple低通滤波器简化版.

（16）High Pass Simple高通滤波器简化版.

下面大概说一下每种效果器的用途：其实大家完全可以自己尝试每种效果器听起来的感觉。
比较特别的有三个，一个是Send，二是Receive，三是Duck Volume，
Send会将这个Group的信号传递出去，而Receive配合接收，因此Receive和Send是一对多的关系。而Duck Volume需要Send进来一些信号，在信号音量超过一定程度的时候会降低这个Group本身的音量。比如NPC正在说话，此时你想让背景音乐小点声，这个Duck Volume就发挥作用了。

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

🔚

