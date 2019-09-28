# 禁用 alsa 重采样

内核版本是 5.3.1，alsa 套件版本是 1.1.9，硬件是双木三林 M6 (SMSL M6)。
alsa 默认是将所有输出重采样到 48kHz 再送入声卡，但因为 M6 解码器本身支持多个采样率，所以软件重采样是多余的，可以禁用。

方法是在 ~/.asoundrc 配置：

```
pcm.M6 {
  type hw
  card "M6"
}

ctl.!default {
  type hw
  card "M6"
}

pcm.!default {
  type plug
  slave {
    pcm "M6"
    rate "unchanged"
  }
}
```

其中 M6 是设备 id，可以 `cat /proc/asound/cards` 获得，行首数字后面、方括号里面的字符就是。

`cat /proc/asound/card2/pcm0p/sub0/hw_params` 可以获得声卡当前的状态。
如果使用默认的重采样，rate 会一直是 48000，配置不重采样的话，播放 44.1kHz 音源时就会显示 44100，和音源一致。

当然这样有个缺点，就是多个程序不能同时播放声音了，因为 M6 并不支持硬件混音。
不过我也没这个需求，所以没有影响。

