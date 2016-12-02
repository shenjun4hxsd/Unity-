##AudioSource API

|变量|说明|
|:--|:--|
|bypassEffects|直通效果（应用过滤器组件或全局监听过滤器）。|
|bypassListenerEffects|当在AudioListener设置全局效果，将不会应用到由AudioSource生成的音频信号。如果AudioSource正在混合器组播放，将不会被应用。|
|bypassReverbZones|当设置AudioSource无路由信号到与混响区域关联的全局混响时。|
|**clip**|默认要播放的音频剪辑。|
|dopplerLevel|设置该AudioSource的多普勒大小。|
|ignoreListenerPause|允许AudioListener.pause属性被设置为true的时候依然允许AudioSource播放。这个通常用于菜单项声音或者背景音乐的暂停功能。|
|ignoreListenerVolume|这个使得AudioSource不受AudioListener组件音量值的影响。|
|**isPlaying**|该音频剪辑是否正播放（只读）?|
|**loop**|该音频剪辑是否循环？|
|maxDistance|（对数衰减）最大距离是一个声音停止衰减的距离。|
|minDistance|在最小距离内，AudioSource将停止增大音量。|
|mute|是否静音音频源。Mute是设置音量为0，取消静音是恢复原来的音量。|
|outputAudioMixerGroup|AudioSource应该路由的目标组信号。|
|panStereo|以立体声方式声像一个正播放的声音（左或右），这仅用于单声道或立体声。|
|pitch|音频源的音调。|
|**playOnAwake**|如果设置为true，音频源将在唤醒时自动播放。|
|priority|设置AudioSource的优先级。|
|reverbZoneMix|信号来自AudioSource将混合到有关混响区全局混响的数量。|
|rolloffMode|设置/获取AudioSource随着距离怎样衰减。|
|spatialBlend|设置该音频源由3D空间计算（衰减，多普勒等）被影响多少。0.0使声音完全2D，1.0使声音完全3D。|
|spatialize|启用或禁用空间化。|
|spread|设置3d立体声或多声道扬声器的空间的扩散角度。|
|**time**|以秒为单位的播放位置。|
|timeSamples|在PCM取样的播放位置。|
|velocityUpdateMode|AudioSource是否应该以固定或动态的方式更新？|
|**volume**|音频源的音量（0.0 ~ 1.0）。|

|公有方法|说明|
|:--|:--|
|GetCustomCurve|获取给定AudioSourceCurveType类型的当前自定义曲线。|
|GetSpatializerFloat|读取附加到音频源的自定义空间化效果的一个用户自定义参数。|
|**Pause**|暂停正播放的音频剪辑。|
|**Play**|播放音频剪辑，带有可选延迟参数。|
|PlayDelayed|播放一个带有指定延迟的音频剪辑，单位秒。建议用户替代老函数Play(delay)，接受指定采样相对于44.1 kHz参考速率作为参数延迟。|
|**PlayOneShot**|播放一个音频剪辑，并且通过volumeScale缩放音频源音量。|
|PlayScheduled|在指定的时间播放剪辑，时间是读取AudioSettings.dspTime绝对时间线。|
|SetCustomCurve|设置给定AudioSourceCurveType类型的自定义曲线。|
|SetScheduledEndTime|更改已定时播放的声音将结束的时间。注意取决于时间，不是所有的重定时请求被满足。|
|SetScheduledStartTime|更改已定时播放的声音将开始的时间。|
|SetSpatializerFloat|设置一个附加到音频源的自定义空间化效果的一个用户自定义参数。|
|**Stop**|停止播放该剪辑。|
|**UnPause**|取消该音频源已暂停的播放。|

|静态方法|说明|
|:--|:--|
|**PlayClipAtPoint**|在世界空间给定位置播放一个音频剪辑。|

###示例：

```javascript

 

    // When the audio component has stopped playing, play otherClip

    using UnityEngine;

    using System.Collections;

    public class ExampleClass : MonoBehaviour {

        public AudioClip otherClip;

        AudioSource audio;

        void Start() {

            audio = GetComponent<AudioSource>();

        }

        void Update() {

            if (!audio.isPlaying) {

                audio.clip = otherClip;

                audio.Play();

            }

        }

    }

```

```javascript

    using UnityEngine;

    using System.Collections;

    [RequireComponent(typeof(AudioSource))]

    public class ExampleClass : MonoBehaviour {

        public AudioClip clip;

        void Start() {

            AudioSource.PlayClipAtPoint(clip, new Vector3(5, 1, 2));

        }

    }

```

