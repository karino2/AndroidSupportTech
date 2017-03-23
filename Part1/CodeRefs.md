# ソースコードの参照について

本書を読む時には、実際のソースコードも参照したくなる事があると思います。
そうした時の為に関連するソースコードの場所をリンクしておきます。

## 参照するサイトについて

オフィシャルなソースコードは https://android.googlesource.com/ になると思うのですが、
ブラウザからの見やすさを考えて、ミラーされているものはgithubのリンクを使います。 
https://github.com/android/

また、自分でブラウズする場合は https://sites.google.com/site/devcollaboration/codesearch などのサイトも便利です。
これだけでは追いにくいコードも存在しますが、限界をわきまえて使えばとても便利なサービスです。

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


## 5章

- ViewRootImpl https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ViewRootImpl.java
   - レンダリングのはじまりはViewRootImplのperformTraversals() -> performDraw() -> draw()
   - ThreadedRenderer::draw()呼び出しは以下 https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ViewRootImpl.java#L2796
- ThreadedRenderer https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ThreadedRenderer.java
   - View https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/View.java
   - RenderNode http://tools.oesf.biz/android-7.0.0_r1.0/xref/frameworks/base/core/java/android/view/RenderNode.java
- DisplayListCanvas https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/DisplayListCanvas.java

nSyncAndDrawFrame()は少し大変だが、

- jniのnSyncAndDrawFrame() https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/jni/android_view_ThreadedRenderer.cpp
  - RenderProxy(プロセス間通信) https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/libs/hwui/renderthread/RenderProxy.cpp
  - DrawFrameTask(その時のコマンド) https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/libs/hwui/renderthread/DrawFrameTask.cpp
     - 少しややこしいけど、別スレッド（RenderThread）がDrawFrameTaskのrun()を呼ぶ
     - CanvasContextのdrawRenderNode()が呼ばれる(例えば以下) https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/libs/hwui/renderthread/CanvasContext.cpp#L433
     - mCanvasはOpenGLRendererのdrawRenderNode() https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/libs/hwui/OpenGLRenderer.cpp#L1427

### 5.3

- DisplayListOp https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/libs/hwui/DisplayListOp.h


### 5.4

- View https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/View.java
- ViewGroup https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ViewGroup.java
- android_view_RenderNode_offsetTopAndBottomとSET_AND_DIRTY https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/jni/android_view_RenderNode.cpp
- fling()処理はこちら AbsListView https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/widget/AbsListView.java
    - なお、RecyclerViewのfling()は全く同じコード
    - https://github.com/android/platform_frameworks_support/blob/android-cts-7.0_r6/v7/recyclerview/src/android/support/v7/widget/RecyclerView.java#L1866

## 6章

### 6.2 HALとgralloc

- hw_get_module() https://github.com/android/platform_hardware_libhardware/blob/android-cts-7.0_r6/hardware.c
   - 実際の処理はhw_get_module_by_class()
      - さらにその中で呼ばれるload()でdlopen()が呼ばれる
- hw_moduel_tの定義など https://github.com/android/platform_hardware_libhardware/blob/android-cts-7.0_r6/include/hardware/hardware.h
- hw_module_tのgrallocの定義 https://github.com/android/platform_hardware_libhardware/blob/android-cts-7.0_r6/modules/gralloc/gralloc.cpp
   - HAL_MODULE_INFO_SYMのあたり
- HAL_PIXEL_FORMATの一覧 https://github.com/android/platform_system_core/blob/android-cts-7.0_r6/include/system/graphics.h
- GRALLOC_USAGEの一覧 https://github.com/android/platform_hardware_libhardware/blob/android-cts-7.0_r6/include/hardware/gralloc.h

### 6.3 EGLによるOpenGL ES描画対象の指定

egl呼び出しとgrallocの間は、実際はかなりいろんなクラスが出てきて、コードを全部追うのは大変です。
基本的な構造は本書で説明した通りですが、実際にコードを追いたいなら多くの瑣末な間接クラスを見ていく必要があります。

間接参照が多い類のコードなのでここに全部参照を貼るのは難しくなっています。
この手のコードを理解するのは、実際に頑張ってコードを読んでもらうのが一番と思いますが、その時のヒントを幾つか書いておきます。

1. EGL関連呼び出しはCanvasContextが行うが、これはRenderProxyが呼び出している(RenderProxyについては本書5.2.2などで簡単に解説してあります）
2. RenderProxyはThreadedRendererから呼ばれる（ThreadedRendererは本書5.2を参照）

以上を踏まえて、主要な所だけを見ていきましょう。

#### eglCreateWindowSurface()とeglMakeCurrent()がCanvasContextのどこで呼ばれているかを見る

- eglCreateWindowSurface()はCanvasContextのinitializeで呼ばれる
  - https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/libs/hwui/renderthread/CanvasContext.cpp#L97
  - mEglManager::createSurface()でeglCreateWindowSurface()が呼ばれる
- eglMakeCurrent()はEglManagerのbeginFrame()で呼ばれて、それはCanvasContext::draw()で呼ばれる
  - https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/libs/hwui/renderthread/CanvasContext.cpp#L309
  - https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/libs/hwui/renderthread/EglManager.cpp#L304

こうして、eglCreateWindowSurface()がeglMakeCurrent()される事は分かった。
このCanvasContextのメソッドは以下のRenderProxyから呼ばれる。

- https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/libs/hwui/renderthread/RenderProxy.cpp#L137
    - initializeはここ
    - CREATE_BRIDGE2マクロについては私の以前のブログなども参照のこと http://karino2.livejournal.com/370929.html
- https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/libs/hwui/renderthread/RenderProxy.cpp#L543
    - drawはここのprepareAndDrawから呼ばれる。


### 6.4 SurfaceFlingerとHWC

6.3同様、6.4も間接参照やらBinderによる辿りにくい依存関係があって読んで行くのは大変です。
ここも膨大なリンクの羅列では無く、読んでく上でのヒントを書いていく事にします。

#### BufferQueueProducerとgralloc周辺を追う

- BufferQueueProducerのallocateBuffers()がgrallocを呼んでいる
- これはBinder越しにSurfaceのallocateBuffers()が呼ぶ
- 基本的には以下のフォルダ下
  - https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/libs/gui/
- SurfaceのallocateBuffers()はViewRootImplから呼ばれる
  - https://github.com/android/platform_frameworks_base/blob/master/core/java/android/view/ViewRootImpl.java#L1852

#### HWCのprepare周辺、HWC1

HWCはちょうど現在HWC2という、よりオブジェクト指向的なインターフェースとなっているHALへの置き換えが行われている所になっています。
書籍執筆時点ではHWC1の内容で解説してあります。ここでは本文との対応を追う目的としてHWC1のコードを示しつつ、その後HWC2の話も少し補足しておく事にします。

まずHWC1から。

HWCはHWComposerというクラスに保持されている。
SurfaceFlingerがこのHWComposerを使用する。

- HWC1のprepare()呼び出しはここ https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/services/surfaceflinger/DisplayHardware/HWComposer_hwc1.cpp#688

HWC内のhwc_layer_1がどこからくるのかをちゃんと読んで行くのは大変だが、
基本的にはSurfaceFlinger内のLayerというオブジェクトとだいたい対応づいている。

LayerはBufferQueueProducerを保持しているもの。グラフィックスバッファを持っている物と言い換えても良い。
Layer周辺を全部読むのも大変だが、IGraphicBufferProducerとの対応付けはonFirstRef()のあたりを読むと良い。

- Layer::onFirstRef() https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/services/surfaceflinger/Layer.cpp#152

これらを使っているのは、少し古いSurfaceFlingerのコードを見る必要がある。例えばMarshmallowのコードは以下。

- MarshmallowのSurfaceFlinger::doComposeSurfaces() https://android.googlesource.com/platform/frameworks/native/+/marshmallow-mr1-release/services/surfaceflinger/SurfaceFlinger.cpp#1994

以上が本文で説明しているコードとなります。

#### HWC2の話

Nougatのコードだと基本的にはHWC2が使われています。
細かい所は違いますが、本質的にはフラグの名前がHWC_FRAMEBUFFERからHWC2::Composition::Clientと呼ばれるようになっているだけで、大きくは変わりません。

- NougatのSurfaceFlinger::doComposeSurfaces() https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/services/surfaceflinger/SurfaceFlinger.cpp#2028
- HWC2の公式ドキュメント https://source.android.com/devices/graphics/implement-hwc.html#interface_activities
   - 最後の方にsync回りの変更の記述あり


### 6.5 ViewRootImpl

6.4までに比べると、6.5のコードは普通に読む人も多いと思います。
WindowManagerService側のコードは読むのは大変ですが、ViewRootImplくらいまでなら楽しく読めるのでは無いでしょうか？

#### DecorViewの登録

- DecorViewの登録 https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/app/ActivityThread.java#L3537
   - このaddViewはWindowsManagerGlobalというクラスの物 https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/WindowManagerGlobal.java#L263   
   - ここでViewRootImplが生成されたり、このViewRootImplにViewツリーがセットされたりする。
      - ViewRootImplのWindowManagerへの登録はViewRootImpl側のsetView()
         - https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ViewRootImpl.java#L640
         - 実際はWindowSessionという物に登録している事が分かる
         - InputChannelの登録もここ

#### ThreadedRenderer等の初期化など

ThreadedRendererの初期化なども、上記のViewRootImpl::setView()の中で行われている。

- ViewRootImpl::setView() https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ViewRootImpl.java#L595
   - enableHardwareAcceleration()呼び出しが行われている
- ViewRootImpl::enableHardwareAcceleration()内でThreadedRenderer::create()が呼ばれている
   - https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ViewRootImpl.java#L878
   - ThreadedRenderer::create() https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ThreadedRenderer.java#L252
      - ThreadedRendererのコンストラクタではRenderProxyが作られ、そこからRenderThreadやEGL関連などのさまざまな重要な初期化が行われる
         - https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ThreadedRenderer.java#L351
         - ここからは難しくはないので自分で追ってみてください。

#### ViewRootImplのperformTraversals()

- https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ViewRootImpl.java#L1436

#### ViewRootImplの入力処理全般

まずはInputChannelの登録。少し関連処理が散らばっているが、全てsetView()メソッドの中にある。

- mInputChannelの生成 https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ViewRootImpl.java#L631
- 登録 https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ViewRootImpl.java#L639
- WindowInputReceiverとの関連付け https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ViewRootImpl.java#L726
- WindowInputReceiverはイベントがやってくるとenqueueInputEventする https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/view/ViewRootImpl.java#L6313
- ここから先は割と素直なので読んでみていただけたらと。


## 7章

7章はとりあえずNougatについての記述の所だけ集めておきます。
ICSなどのコードも読むと面白い発見はありますが、さすがに趣味の世界でしょう（このページ自体趣味の世界ですが…）

### 7.5 Nのバイトコード実行環境

- ProfileSaver::Start()などはこちら https://android.googlesource.com/platform/art/+/android-7.0.0_r6/runtime/jit/profile_saver.cc
   - これが呼ばれるのはLoadedApkのVMRuntime.registerAppInfo()呼び出しから追うと分かりやすい
      -  https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/core/java/android/app/LoadedApk.java#L597
   - 実際にプロファイル情報を集めるのは同じファイル（ProfileSaver）のProcessProfilingInfo()を読む
- BackgroundDexOptServiceの登録はこちら https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/services/core/java/com/android/server/pm/BackgroundDexOptService.java#L69
   - profile guidedコンパイルが行われる条件が分かる（充電中とかアイドルとか）
   - そこから呼ばれるperformDexOpt()は、いろいろ通ってperformDexOptInternalWithDependenciesLI()に行き着く
      - https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/services/core/java/com/android/server/pm/PackageManagerService.java#L7453
         - そこからPackageDexOptimizer::perfromDexOpt()を呼んでいるのが分かる
         - 本体はperformDexOptLI() https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/services/core/java/com/android/server/pm/PackageDexOptimizer.java#L152
            - merge_profile呼び出しはここ https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/services/core/java/com/android/server/pm/PackageDexOptimizer.java#L182
            - その結果を受けてdexoptが呼ばれる https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/services/core/java/com/android/server/pm/PackageDexOptimizer.java#L257
               - 渡しているフラグでprofile guided compileが行われるかが決まる。フラグの場所は以下。
               - https://github.com/android/platform_frameworks_base/blob/android-cts-7.0_r6/services/core/java/com/android/server/pm/PackageDexOptimizer.java#L248

#### dexopt呼び出しから

installdはdexoptコマンドのメッセージを受け取ると、dexopt()関数を呼び出す。

- dexopt()はrun_dex2oat()を呼び出す https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/cmds/installd/commands.cpp#1538
- run_dex2oat()の引数のprofile_fdに注意して追う https://android.googlesource.com/platform/frameworks/native/+/android-7.0.0_r6/cmds/installd/commands.cpp#694
- dex2oatより先はこの辺から https://android.googlesource.com/platform/art/+/android-7.0.0_r6/dex2oat/dex2oat.cc#2613
   - イメージファイル周辺もこの辺

