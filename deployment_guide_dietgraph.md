# DietGraph - Android デプロイガイド (Windows版)

このガイドは、Windows 環境で `DietGraph` を Google Play Store に公開するための手順書です。

## 1. アプリ基本情報の決定

### 開発者アカウント情報

| 項目 | 値 |
|---|---|
| **Google Play 開発者名** | Shonan Craft Club |
| **アカウント ID** | 8234857728229829821 |

### アプリ情報

特に **Application ID** は一度決めると変更できないため重要です。

| 項目 | 設定値 | 備考 |
|---|---|---|
| **アプリ名** | DietGraph | ホーム画面に表示される名前 (設定済) |
| **ストア表示名** | DietGraph - グラフにこだわった体重管理アプリ | Play Store 上での名前 |
| **Application ID** | **com.shonancraft.dietgraph** | `com.example` から変更済みであること |
| **バージョン** | 1.0.0+1 | `pubspec.yaml` で管理 |

---

## 2. プロジェクト設定の変更 (必須)

デプロイ前に、以下の設定を変更してください。

### 2-1. Application ID の変更

`com.example` ドメインはストアで使用できません。独自のドメイン（例: `com.studio.dietgraph`）に変更します。

1.  **build.gradleの設定**:
    `android/app/build.gradle.kts` を開き、`applicationId` を変更します。
    ```kotlin
    defaultConfig {
        applicationId = "com.studio.dietgraph" // ← ここを変更！
        // ...
    }
    ```

2.  **パッケージ名の変更**:
    Android ではフォルダ構成とパッケージ名が一致している必要があります。
    *   `android/app/src/main/kotlin/com/example/p29_taijuu_flutter/MainActivity.kt` を開きます。
    *   1行目の `package com.example.p29_taijuu_flutter` を `package com.studio.dietgraph` に変更します。
    *   フォルダ構成を変更します:
        *   `android/app/src/main/kotlin/com/studio/dietgraph/MainActivity.kt` となるようフォルダを作成・移動し、古い `example` フォルダは削除します。

3.  **AndroidManifest.xmlの確認**:
    `android/app/src/main/AndroidManifest.xml` に `package="com.example..."` という記述がある場合は削除するか、新しいIDに合わせてください（最近のFlutterプロジェクトでは不要な場合もありますが、確認してください）。

---

## 3. 署名鍵 (Keystore) の作成

Google Play Store にアップロードするには、アプリにデジタル署名を行う必要があります。

1.  コマンドプロンプト（またはPowerShell）を開き、プロジェクトのルートディレクトリで以下を実行します。
    ※ 1行で入力してください。カギ括弧内は変更してください。

    ```powershell
    keytool -genkey -v -keystore android/app/upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload
    ```

2.  パスワードの入力などが求められます。
    *   **キーストアのパスワード**: 忘れないようにメモしてください。
    *   **姓名など**: 適当で構いません（例: Taro Yamada）。
    *   **組織単位**: Studio Nameなど
    *   **国コード**: JP

3.  `android/app/upload-keystore.jks` が生成されたことを確認してください。
    **重要**: このファイルは **絶対に公開しないでください**（Gitに含めない）。

---

## 4. 署名情報の登録

署名情報をコードに直接書くのは危険なため、別のファイルで管理します。

1.  `android` フォルダ内に `key.properties` というファイルを作成します。
2.  以下の内容を記述します（パスワードは上記で設定したもの）。

    ```properties
    storePassword=あなたのパスワード
    keyPassword=あなたのパスワード
    keyAlias=upload
    storeFile=upload-keystore.jks
    ```

3.  `android/app/build.gradle.kts` を編集し、署名設定を読み込むようにします。
    （具体的なコードは後述の「build.gradle.kts の修正例」を参照）

---

## 5. リリースビルドの作成

準備ができたら、リリース用の App Bundle (.aab) ファイルを作成します。

```powershell
flutter build appbundle --release
```

成功すると、以下の場所にファイルが生成されます：
`build/app/outputs/bundle/release/app-release.aab`

この `.aab` ファイルを Google Play Console にアップロードします。

---

## 6. Google Play Console 登録情報 (案)

### ストアの掲載情報

*   **アプリ名**: `DietGraph - グラフが見やすい体重管理アプリ` (30文字以内)
*   **簡単な説明** (80文字以内):
    > イベントも記録できる、シンプルで高機能な体重管理アプリ。カレンダーとグラフで変化を一目で把握。
*   **詳しい説明** (4000文字以内):
    > （ここにアプリの詳細な機能を記述します。前回作成したドラフトを参考に、本アプリの機能に合わせて調整してください。）

### グラフィックアセット

*   **アプリアイコン**: 512 x 512 px (PNG)
*   **フィーチャーグラフィック**: 1024 x 500 px (JPG または PNG)
*   **スクリーンショット**:
    *   スマホ用: 2枚以上 (アスペクト比 9:16 推奨)
    *   7インチタブレット用: (任意)
    *   10インチタブレット用: (任意)

### プライバシーポリシー

*   URLが必要です。GitHub Pagesなどで公開している既存のポリシー（`docs/privacy-policy.html`）の内容を今回のアプリに合わせて修正し、そのURLを使用します。

---

## 付録: build.gradle.kts の修正例

`android/app/build.gradle.kts` の署名設定部分の記述例です。

```kotlin
import java.util.Properties
import java.io.FileInputStream

// ... (plugins ブロックなど)

android {
    // ...

    // 署名設定の読み込み
    val keystoreProperties = Properties()
    val keystorePropertiesFile = rootProject.file("key.properties")
    if (keystorePropertiesFile.exists()) {
        keystoreProperties.load(FileInputStream(keystorePropertiesFile))
    }

    signingConfigs {
        create("release") {
            keyAlias = keystoreProperties["keyAlias"] as String
            keyPassword = keystoreProperties["keyPassword"] as String
            storeFile = file(keystoreProperties["storeFile"] as String)
            storePassword = keystoreProperties["storePassword"] as String
        }
    }

    buildTypes {
        release {
            signingConfig = signingConfigs.getByName("release") // ← ここで適用
            // ...
        }
    }
}
```
