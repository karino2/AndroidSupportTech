# ソースコードの参照について

本書を読む時には、実際のソースコードも参照したくなる事があると思います。
そうした時の為に関連するソースコードの場所をリンクしておきます。

## 参照するサイトについて

オフィシャルなソースコードは https://android.googlesource.com/ になると思うのですが、
ブラウザからの見やすさを考えて、ミラーされているものはgithubのリンクを使います。 
https://github.com/android/

タグとしては本書が参考にしたandroid-7.0.0_r6か、それが無いツリーではそこに近いものを適宜使います。

なお、一部ファイルサイズが大きすぎてgithubのWeb UIからは目的のコードが表示されないファイルがあります。
その場合は直接ローカルにダウンロードしてエディタなどで開いてみてください。


## 1章

- 1.3.7 init.rc https://github.com/android/platform_system_core/blob/android-cts-7.0_r6/rootdir/init.rc

## 2章

### 2.2 LinuxのInputサブシステムとinput_event

- 2.2.2 マルチタッチの例は例えば以下 https://android.googlesource.com/kernel/msm.git/+/android-7.0.0_r0.47/drivers/input/touchscreen/synaptics_i2c_rmi4.c

### 2.4 InputReader

- InputReader https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/services/inputflinger/InputReader.cpp
- EventHub https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/services/inputflinger/EventHub.cpp
  - デバイスクラスなどはEventHub.hにある https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/services/inputflinger/EventHub.h



