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

InputManager周辺のコードはだいたい https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/services/inputflinger/ にあります。
native下はたぶんgithub側にはホストされていないので公式の方をリンクします。

- InputReader https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/services/inputflinger/InputReader.cpp
  - InputMapperとそのサブクラスもこちら
- EventHub https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/services/inputflinger/EventHub.cpp
  - デバイスクラスなどはEventHub.hにある https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/services/inputflinger/EventHub.h

### 2.5 InputDispatcherとInputChannel

- InputDispatcher https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/services/inputflinger/InputDispatcher.cpp
- InputChannel https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/libs/input/InputTransport.cpp
  - ヘッダはこちら https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/include/input/InputTransport.h

なお、InputManagerServiceは言語境界を超えて広く分散されてしまうが、例えばregisterInputChannel()を見たいなら、以下のあたりを見る。

- InputManagerServiceのjni https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/services/core/jni/com_android_server_input_InputManagerService.cpp

## 3章

- Looper https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/os/Looper.java
- MessageQueue https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/os/MessageQueue.java
   - nativePollOnceは、https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/jni/android_os_MessageQueue.cpp から
     以下のpollOnceに行き着く https://github.com/android/platform_system_core/blob/android-cts-7.0_r6/libutils/Looper.cpp
- Handler https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/os/Handler.java
- ActivityThread https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/app/ActivityThread.java
   - scheduleLaunchActivity()などの例で本文では出てくる。main()も見てみるとLooperのloop()の呼び方の良い例となっている。詳細は第二巻。


## 4章

- AssetManager https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/content/res/AssetManager.java
   - XmlResourcePraserの取得はopenXmlResourcePraser()のあたりを読む
- LayoutInflater https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/LayoutInflater.java
- PhoneLayoutInflater https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/com/android/internal/policy/PhoneLayoutInflater.java
- XmlBlock https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/content/res/XmlBlock.java
   - XmlResourceParserの実装はこのXmlBlockのParserを見る
   - ネイティブ側はこちらを経由して https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/jni/android_util_XmlBlock.cpp
   - 最終的にResXMLTreeへ https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/libs/androidfw/ResourceTypes.cpp
      - ResXMLTreeのsetTo()がmemcpy()しているあたり(本文の4.3.6に相当)
      - 本文ではResXMLTreeとResXMLParserの関係は詳細過ぎるという事で省いているが、ResXMLTreeがResXMLParserを持っていて、大部分の仕事はResXMLParserがやっています。
- LinearLayout https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/widget/LinearLayout.java

### 4.4
- Activity https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/app/Activity.java
   - setContentView()の実体はPhoneWindow側
- PhoneWindow https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/com/android/internal/policy/PhoneWindow.java
- screen_title.xml https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/res/res/layout/screen_title.xml

### 4.5

- View https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/View.java
- ImageView https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/widget/ImageView.java

### 4.6
- LinearLayout https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/widget/LinearLayout.java
- ViewGroup https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ViewGroup.java
