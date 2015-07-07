# Force Opetaion Xとは

Force Operation X (以下F.O.X)は、スマートフォンにおける広告効果最適化のためのトータルソリューションプラットフォームです。アプリケーションのダウンロード、ウェブ上でのユーザーアクションの計測はもちろん、スマートフォンユーザーの行動特性に基づいた独自の効果計測基準の元、企業のプロモーションにおける費用対効果を最大化することができます。

本ドキュメントでは、スマートフォンアプリケーションにおける広告効果最大化のためのF.O.X SDK導入手順について説明します。

## F.O.X SDKとは

F.O.X SDKをアプリケーションに導入することで、以下の機能を実現します。

* **インストール計測**

広告流入別にインストール数を計測することができます。

* **LTV計測**

流入元広告別にLife Time Valueを計測します。主な成果地点としては、会員登録、チュートリアル突破、課金などがあります。各広告別に登録率、課金率や課金額などを計測することができます。

* **アクセス解析**

自然流入と広告流入のインストール比較。アプリケーションの起動数やユニークユーザー数(DAU/MAU)。継続率等を計測することができます。

* **プッシュ通知**

F.O.Xで計測された情報を使い、ユーザーに対してプッシュ通知を行うことができます。例えば、特定の広告から流入したユーザーに対してメッセージを送ることができます。

# 1.	概要

本ドキュメントでは、Force Operation X SDK Cocos2d-x プラグインの導入手順について説明します。Force Operation X SDK Cocos2d-xプラグインはiOSおよびAndroidに対応しています。

Force Operation X SDKでは、インストールおよびLTVの計測、アクセス解析を行うことができます。

SDKの実装後は、アプリケーションの動作及び効果測定の結果に問題のないことを確認した後にマーケットへの申請を行ってください。効果測定のテスト手順については、Force Operation X管理者よりご連絡いたします。

## 1.1	SDK仕様


F.O.X SDKをアプリケーションに導入することで、以下の機能を実現します。


|処理|必須|概要|
|:------:|:------:|:------|
|インストール計測|必須|起動時はブラウザが起動し、Cookie計測により広告効果測定を行います。<br>コンバージョン数、CVRなどを測定することができます。<br>メソッド名：sendConversion|
|LTV計測|オプション|任意の成果地点で成果通知を行い、広告別の課金数や入会数の計測を行います。<br>課金金額、退会数などを測定することができます。<br>メソッド名：sendLtv|
|アクセス解析|オプション|アプリの起動時およびバックグラウンドからの復帰時の起動計測を行います。<br>起動数、アクティブユーザー数(DAU)、継続率などを測定することができます。<br>メソッド名：sendStartSession|



> Force Operation X SDK Cocos2d-x プラグインは、ネイティブ版SDKへのブリッジを行うラッパーとして実装しています。SDK導入には、プラグインと、iOS及びAndroidのネイティブ版SDKが必要となります。_


#2.	iOS用 Xcode プロジェクトの設定


##2.1	SDKの展開

ダウンロードしたSDK「FOX_Cocos2dx_SDK_<version>.zip」をOS上に展開します。Classesフォルダ内に同梱されているファイルは以下の通りです。

ファイル名|必須|概要
:--------|:-------------:|:--------:
Cocos2dxFox.h|必須|ヘッダファイル。Cocos2d-xからSDKを使用するためのラッパー
Cocos2dxFox.mm|	iOS専用	|ライブラリファイル。Cocos2d-xからFOX SDKを使用するためのiOS用のラッパー
Cocos2dxFox.cpp|オプション|ヘッダファイル。Cocos2d-xからPush通知を行うためのラッパー
FoxNotifyPlugin.m|オプション|ライブラリファイル。Cocos2d-xからPush通知を行うためのラッパー
FoxVersionPlugin.h|オプション|ヘッダファイル。Cocos2d-xからバンドルバージョンを制御するラッパー
FoxVersionDelegate.m|オプション|ライブラリファイル。Cocos2d-xからバンドルバージョンを制御するラッパー|
FoxVersionPlugin.h|オプション|ヘッダファイル。Cocos2d-xからバンドルバージョンを制御するラッパー|
FoxVersionPlugin.m|オプション|ライブラリファイル。Cocos2d-xからバンドルバージョンを制御するラッパー|
FoxReengagePlugin.h|オプション|ライブラリファイル。Cocos2d-xからURLスキーム経由の起動計測を行うためのラッパー|
FoxReengagePlugin.m|オプション|ヘッダファイル。Cocos2d-xからURLスキーム経由の起動計測を行うためのラッパー|

上記のファイルはラッパーですので、実装ファイルはネイティブSDKを組み合わせます。

以下のファイル群が、iOSのネイティブSDKとなっており「FOX_iOS_SDK_<version>」フォルダに同梱されています。

ファイル名|必須|概要
:--------|:-------------:|:--------:
AdManager.h|必須|ヘッダファイル。広告の効果測定を行う。|
libAppAdForce.a|必須|ライブラリファイル。広告の効果測定を行う。|
Ltv.h|オプション|ヘッダファイル。LTV計測を行う。|
AnalyticsManager.h|オプション|ヘッダファイル。アクセス解析を行う。|
Notify.h|オプション|ヘッダファイル。Push通知を行う。

※「FOX_Cocos2dx_SDK_<version>.zip」に「FOX_iOS_SDK_<version>」フォルダが含まれていない場合は

SDKダウンロードサイトからFOX_iOS_SDK_<version>.zipをダウンロードの上、ご使用ください。


##2.2	ライブラリの組み込み

* Xcodeのメニュー「File」→「Add Files to “XXX”」を選択
* Cocos2dxFox.h、Cocos2dxFox.mmを組み込んでください。Cocos2dxFox.cppはAndroid用ライブラリであるため、追加の必要はありません。
* プッシュ通知機能を使う場合は、FoxNotifyPlugin.h、FoxNotifyPlugin.mの2ファイルを組み込んでください。
* バンドルバージョン判定機能を使う場合は、FoxVersionDelegate.h、FoxVersionDelegate .mmとFoxVersionPlugin.h、FoxVersionPlugin.mmの計4ファイルを組み込んでください。
* リエンゲージメント計測を行う場合はFoxReengagePlugin.hとFoxReengagePlugin.mの2ファイルを組み込んでください。
* 「Copy items into destination group’s folder」にチェック

![ライブラリの組み込み手順](./doc/integration/img01.png)

[ライブラリの組み込み詳細](./doc/integration/)


##2.3 フレームワークの追加

次のフレームワークをプロジェクトにリンクしてください。

<table>
<tr><th>フレームワーク名</th><th>Status</th></tr>
<tr><td>AdSupport.framework</td><td>Optional</td></tr>
<tr><td>iAd.framework </td><td>Required</td></tr>
<tr><td>Security.framework </td><td>Required </td></tr>
<tr><td>StoreKit.framework </td><td>Required </td></tr>
<tr><td>SystemConfiguration.framework </td><td>Required </td></tr>
</table>

> AdSupport.frameworkはiOS 6以降で追加されたフレームワークのため、アプリケーションをiOS 5以前でも動作させる(iOS Deployment Targetを5.1以下に設定する)場合にはweak linkを行うために”Optional”に設定してください。

![フレームワーク設定01](./doc/config_framework/img01.png)

[フレームワーク設定の詳細](./doc/config_framework/README.md)

##2.4 SDKの設定

SDKの動作に必要な設定をplistに追加します。「AppAdForce.plist」というファイルをプロジェクトの任意の場所に作成し、次のキーと値を入力してください。

<table>
<tr>
  <th>Key</th>
  <th>Type</th>
  <th>Value</th>
</tr>
<tr>
  <td>APP_ID</td>
  <td>String</td>
  <td>Force Operation X管理者より連絡しますので、その値を入力してください。</td>
</tr>
<tr>
  <td>SERVER_URL</td>
  <td>String</td>
  <td>Force Operation X管理者より連絡しますので、その値を入力してください。</td>
</tr>
<tr>
  <td>APP_SALT</td>
  <td>String</td>
  <td>Force Operation X管理者より連絡しますので、その値を入力してください。</td>
</tr>
<tr>
  <td>APP_OPTIONS</td>
  <td>String</td>
  <td>何も入力せず、空文字の状態にしてください。</td>
</tr>
<tr>
  <td>CONVERSION_MODE</td>
  <td>String</td>
  <td>1</td>
</tr>
<tr>
  <td>ANALYTICS_APP_KEY</td>
  <td>String</td>
  <td>Force Operation X管理者より連絡しますので、その値を入力してください。<br />アクセス解析を利用しない場合は設定の必要はありません。</td>
</tr>
</table>

![フレームワーク設定01](./doc/config_plist/img05.png)

[SDK設定の詳細](./doc/config_plist/)

[AppAdForce.plistサンプル](./doc/config_plist/AppAdForce.plist)


#3.	Android用 プロジェクトの設定
##3.1	SDKの展開

ダウンロードしたSDK「FOX_Cocos2dx_SDK_<version>.zip」をOS上に展開します。<br>
「FOX_Android_SDK_<version>」フォルダに同梱されている「AppAdForce.jar」を開発環境に導入します。次に、Eclipseプロジェクトへの導入手順について説明します。

|ファイル名|必須|概要|
|:------:|:------:|:------|
|AppAdForce.jar|必須|AndroidのネイティブSDK。通常成果・LTV成果・アクセス解析を計測することができます。|
|gcm.jar|オプション|プッシュ通知機能を利用する際に導入ください。|

※「FOX_Cocos2dx_SDK_<version>.zip」に「FOX_Android_SDK_<version>」フォルダが含まれていない場合はSDKダウンロードサイトからFOX_Android_SDK_<version>.zipをダウンロードの上、ご使用ください。

##3.2	プロジェクトへの導入

既にアプリケーションにSDKが導入されている場合には、[最新バージョンへのアップデートについて](./doc/update/)をご参照ください。

ダウンロードしたSDK「FOX_Android_SDK_<version>.zip」を展開し、「AppAdForce.jar」をアプリケーションのプロジェクトに組み込んでください。

[Eclipseプロジェクトへの導入の方法](./doc/integration/eclipse/)

[AndroidStudioプロジェクトへの導入の方法](./doc/integration/android_studio/)


##3.3 AndroidManifest.xmlの編集
###3.3.1 パーミッションの設定

Force Operation X SDKの実行に必要なパーミッションの設定を<manifest>タグ内に追加します。

	<uses-permission android:name="android.permission.INTERNET" />
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
	<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />	
###3.3.2 メタデータの設定
|パラメータ名|必須|概要|
|:------|:------|:------|
|APPADFORCE_APP_ID|必須|Force Operation X管理者より連絡しますので、その値を入力してください。|
|APPADFORCE_SERVER_URL|必須|Force Operation X管理者より連絡しますので、その値を入力してください。|
|APPADFORCE_CRYPTO_SALT|必須|Force Operation X管理者より連絡しますので、その値を入力してください。|
|ANALYTICS_APP_KEY|必須|Force Operation X管理者より連絡しますので、その値を入力してください。|
|APPADFORCE_ID_DIR|オプション|内(外)部ストレージを利用する際、任意の保存するディレクトリ名を指定できます。<br>未指定の場合、パッケージ名でディレクトリが作成されます。|
|APPADFORCE_ID_FILE|オプション|内(外)部ストレージを利用する際、任意の保存する識別子ファイル名を指定できます。<br>未指定の場合、”__FOX_XUNIQ__”でファイルが作成されます。|
|APPADFORCE_USE_EXTERNAL_STORAGE|オプション|0を指定した場合、内(外)部ストレージへの保存は実行しません。|

Force Operation X SDKの実行に必要な情報を<application>タグ内に追加します。

	<meta-data android:name="APPADFORCE_APP_ID" android:value="1" />
	<meta-data android:name="APPADFORCE_SERVER_URL" android:value="012345ABC" />


###3.3.3 インストールリファラー計測の設定
インストールリファラー計測を行うために必要な設定を<application>タグ内に追加します。

	<receiver android:name="jp.appAdForce.android.InstallReceiver" android:exported="true">
		<intent-filter>
			<action android:name="com.android.vending.INSTALL_REFERRER" />
		</intent-filter>
	</receiver>

既に"com.android.vending.INSTALL_REFERRER"に対するレシーバークラスが定義されている場合には、[二つのINSTALL_REFERRERレシーバーを共存させる場合の設定](https://github.com/cyber-z/public_fox_android_sdk/blob/master/doc/install_referrer/ja/README.md)をご参照ください。

###3.3.4 AndroidManifest.xmlに関連するその他の設定
* [URLスキームの設定](./doc/config_url_scheme/)
* [（オプション）広告IDを利用するためのGoogle Play Services SDKの導入](./doc/google_play_services/)
* [（オプション）外部ストレージを利用した重複排除設定](./doc/external_storage/)
* [AndroidManifest.xmlの設定例](./doc/config_androidManifest/AndroidManifest.xml)
	


##3.4 Cocos2d-xプロジェクトのClassesへの導入

「FOX_Cocos2dx_SDK_<version>.zip」を展開し、以下の操作を行います。

* Android/Classes配下のCocos2dxFox.hとCocos2dxFox.cppをプロジェクト配下のClassesへコピー
* プロジェクト内配下のjni/Android.mkにCocos2dxFox.cppファイルを追記し、ビルドの対象に含める<br>
　→ LOCAL_SRC_FILESにClasses/Cocos2dxFox.cppの指定を追加してください。

　
Cocos2dxFox.cppのJniHelper.hのincludeパスを開発環境に合わせる（以下はその例）


#4 インストール計測の実装
##4.1 インストールの計測

初回起動のインストール計測を実装することで、広告の効果測定を行うことができます。初回起動時に、ブラウザを起動し、広告クリック時に付与されたCookieの情報を照合することで、成果の計測を行います。

初回起動時にブラウザを起動するために、AppDelegate:applicationDidFinishLaunching:メソッド等、アプリケーションの起動時に必ず呼ばれる箇所に効果測定用の処理を追加します。

ヘッダファイルをインクルード

	#include "Cocos2dxFox.h"
	
成果通知のコードを追加

	FoxPlugin::sendConversion(“default”);

sendConversionメソッドの引数には、通常は上記の通り"default"という文字列をそのまま指定してください。

* [sendConversionの詳細](./doc/send_conversion/)
	
また、URLスキーム経由の起動を計測するために、URLスキームが設定されている全てのActivityのonResume()にsendConversionWithUrlSchemeメソッドを実装します。

クラスをインポート

	import jp.appAdForce.android.cocos2dx.Cocos2dxAdManager;
	
アプリケーション起動時の起動計測（MainActivityクラスへの実装例）

	public class MainActivity extends Cocos2dxActivity {
	
		@Override
		protected void onResume() {
			super.onResume();
			Cocos2dxAdManager.sendConversionWithUrlScheme(this);
		}
	}

URLスキームで起動されるActivityのlaunchModeが"singleTask"または"singleInstance"の場合は、URLスキーム経由でパラメータを受け取るためにonNewIntentメソッドをoverrideし、以下のようにsetIntentメソッドをコールしてください。

	@Override
	protected void onNewIntent(Intent intent) {
    	super.onNewIntent(intent);
	    setIntent(intent);
	}


#5 LTV計測の実装


LTV計測により、広告流入別の課金金額や入会数などを計測することができます。計測のために、任意の地点にLTV成果通信を行うコードを追加します。






#6 アクセス解析

アクセス解析を導入することで、自然流入・広告流入別の起動数、アクティブユーザー数(DAU/MAU)や継続率を計測することができます。アクセス解析では、アプリケーションが起動、もしくはバックグラウンドから復帰する際にセッション計測を行うコードを追加します。不要の場合には、本項目の実装を省略できます。

####iPhone プロジェクト
iPhoneの場合、以下の設定が必要です。

アプリ起動地点のapplicationDidFinishLaunchingおよびapplicationWillEnterForegroundの両方に、以下のように記述してください。

	FoxPlugin::sendStartSession();
	
####Android プロジェクト
Androidの場合、以下の設定が必要です。

アプリケーションの起動及び、バックグラウンドからの復帰を計測するために、ActivityのonResume()にコードを追加します。

クラスをインポート

	import jp.appAdForce.android.AnalyticsManager;
	
アプリケーション起動時の起動計測（MainActivityクラスへの実装例）

	public class MainActivity extends Activity {

＜JavaのActivity上でonResume()が使えない場合＞

アプリ起動地点のapplicationDidFinishLaunchingおよび、applicationWillEnterForeground 内で以下のように実行します。

	#include “Cocos2dxFox.h”
	
---
	
	FoxPlugin::sendStartSession();
	
> ※アプリケーションがバックグラウンドから復帰した際に、そのActivityに起動計測の実装がされていない場合など、正確なアクティブユーザー数が計測できなくなります。<br>




3. 弊社より発行したテスト用URLをクリック<br>
   ※ テスト用URLは必ずOSに設定されているデフォルトブラウザでリクエストされるようにしてください。
   ※ テストURLの場合には、遷移先がなくエラーダイアログが表示される場合がありますが、問題ありません。
6. アプリを起動、ブラウザが起動<br>
   ※ ここでブラウザが起動しない場合には、正常に設定が行われていません。
8. アプリを終了し、バックグラウンドからも削除
9. 再度アプリを起動
10. 弊社へ3,6,7,9の時間をお伝えください。正常に計測が行われているか確認致します。
11. 弊社側の確認にて問題がなければテスト完了となります。







* [広告IDを利用するためのGoogle Play Services SDKの導入](./doc/google_play_services/)

