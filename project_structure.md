# プロジェクト構成ドキュメント

## プロジェクト概要
- **プロジェクト名**: p29_taijuu_flutter
- **説明**: 新規Flutterプロジェクト
- **バージョン**: 1.0.0+1
- **Dart SDK**: ^3.10.8

---

## ディレクトリ構成

```
p29_taijuu_flutter/
├── .dart_tool/          # Dartツールの内部ファイル(自動生成)
├── .idea/               # IntelliJ IDEA / Android Studio の設定ファイル
├── android/             # Androidプラットフォーム固有のコード
├── build/               # ビルド成果物(自動生成)
├── docs/                # プロジェクトドキュメント
├── lib/                 # Dartソースコード(メインアプリケーションコード)
├── test/                # テストコード
├── web/                 # Webプラットフォーム固有のファイル
├── .gitignore           # Gitで無視するファイルの設定
├── .metadata            # Flutterプロジェクトのメタデータ
├── analysis_options.yaml # Dart静的解析の設定
├── pubspec.yaml         # パッケージ依存関係と設定
├── pubspec.lock         # 依存関係のロックファイル
├── README.md            # プロジェクトの基本説明
└── p29_taijuu_flutter.iml # IntelliJ IDEAモジュールファイル
```

---

## 主要ディレクトリの詳細

### 📁 `lib/` - アプリケーションコード
Flutterアプリケーションのメインソースコードを格納するディレクトリです。

#### 現在のファイル:
- **[main.dart](file:///d:/myWorks/p29_taijuu_flutter/lib/main.dart)** (4,927 bytes)
  - アプリケーションのエントリーポイント
  - `main()` 関数でアプリを起動
  - `MyApp`: ルートウィジェット(MaterialAppを設定)
  - `MyHomePage`: ホーム画面のStatefulWidget
  - `_MyHomePageState`: カウンターのデモ実装

**主要な機能:**
- Material Designテーマの設定
- カウンターのデモアプリ(ボタンを押すと数字が増える)
- ホットリロード対応のステートフル実装

---

### 📁 `test/` - テストコード
ユニットテストとウィジェットテストを格納するディレクトリです。

#### 現在のファイル:
- **widget_test.dart** (1,099 bytes)
  - MyHomePageウィジェットのテスト
  - カウンター機能の動作確認テスト

---

### 📁 `android/` - Androidプラットフォーム
Android固有の設定とビルドファイルを格納します。

#### 主要な構成:
- **app/** - Androidアプリケーションモジュール
- **build.gradle.kts** - Gradleビルド設定(Kotlin DSL)
- **gradle/** - Gradleラッパーファイル
- **gradle.properties** - Gradleプロパティ設定
- **gradlew / gradlew.bat** - Gradleラッパースクリプト
- **settings.gradle.kts** - Gradleプロジェクト設定
- **local.properties** - ローカル環境設定(SDK パス等)

**用途:**
- Androidアプリとしてビルド・デプロイする際に使用
- Google Play Storeへの公開準備

---

### 📁 `web/` - Webプラットフォーム
Web版アプリケーションのファイルを格納します。

#### 現在のファイル:
- **index.html** (1,272 bytes) - Webアプリのエントリーポイント
- **manifest.json** (967 bytes) - PWA(Progressive Web App)マニフェスト
- **favicon.png** (917 bytes) - ファビコン
- **icons/** - アプリアイコン

**用途:**
- ブラウザで動作するWebアプリとして実行
- PWAとしてインストール可能

---

## 設定ファイルの詳細

### 📄 `pubspec.yaml` - パッケージ管理
Flutterプロジェクトの依存関係と設定を定義します。

#### 現在の依存関係:
**dependencies:**
- `flutter` (SDK) - Flutterフレームワーク
- `cupertino_icons: ^1.0.8` - iOS スタイルアイコン

**dev_dependencies:**
- `flutter_test` (SDK) - テストフレームワーク
- `flutter_lints: ^6.0.0` - コード品質チェック

**Flutter設定:**
- `uses-material-design: true` - Material Iconsを使用

---

### 📄 `analysis_options.yaml` - 静的解析設定
Dartコードの静的解析ルールを設定します。

**現在の設定:**
- `package:flutter_lints/flutter.yaml` をインクルード
- Flutter推奨のリントルールを適用
- カスタムルールは未設定(デフォルト使用)

**用途:**
- コードの品質チェック
- `flutter analyze` コマンドで実行

---

### 📄 `README.md` - プロジェクト説明
プロジェクトの基本情報とFlutter学習リソースへのリンクを記載。

---

## 自動生成ディレクトリ

### 📁 `.dart_tool/`
Dartツールチェーンが使用する内部ファイル。手動編集不要。

### 📁 `build/`
ビルド成果物が格納されます。`.gitignore`で除外されています。

---

## 開発の始め方

### 1. 依存関係のインストール
```bash
flutter pub get
```

### 2. アプリの実行
```bash
# デバッグモードで実行
flutter run

# Webで実行
flutter run -d chrome

# Androidエミュレータで実行
flutter run -d android
```

### 3. テストの実行
```bash
flutter test
```

### 4. コード解析
```bash
flutter analyze
```

---

## 次のステップ

### 推奨されるディレクトリ構成の拡張:
```
lib/
├── main.dart
├── models/          # データモデル
├── screens/         # 画面ウィジェット
├── widgets/         # 再利用可能なウィジェット
├── services/        # APIやデータベース接続
├── utils/           # ユーティリティ関数
└── constants/       # 定数定義
```

### 追加推奨パッケージ:
- **状態管理**: `provider`, `riverpod`, `bloc`
- **ルーティング**: `go_router`
- **HTTP通信**: `http`, `dio`
- **ローカルストレージ**: `shared_preferences`, `sqflite`
- **UI**: `google_fonts`, `flutter_svg`

---

## 参考リンク
- [Flutter公式ドキュメント](https://docs.flutter.dev/)
- [Dart言語ツアー](https://dart.dev/guides/language/language-tour)
- [Flutter Cookbook](https://docs.flutter.dev/cookbook)
- [pub.dev - パッケージリポジトリ](https://pub.dev/)

---

**最終更新**: 2026-02-03
