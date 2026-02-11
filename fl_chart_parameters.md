# FL Chart - LineChart 全パラメータリファレンス

FL ChartライブラリのLineChartで使用できる全パラメータの詳細なリファレンスです。

## LineChart ウィジェット

LineChartウィジェット自体のパラメータ：

### 主要パラメータ

- **`data`** (`LineChartData`) - グラフの全データと設定を含むオブジェクト（必須）

### アニメーション関連

- **`swapAnimationDuration`** (`Duration`) - グラフの状態変更時のアニメーション時間
- **`swapAnimationCurve`** (`Curve`) - アニメーションのカーブ（イージング）

---

## LineChartData クラス

グラフ全体の設定を管理するクラス。

### データ関連

- **`lineBarsData`** (`List<LineChartBarData>`) - グラフに表示する線のリスト（複数の線を描画可能）
- **`betweenBarsData`** (`List<BetweenBarsData>`) - 2つの線の間の領域を塗りつぶす設定

### 軸の範囲設定

- **`minX`** (`double`) - X軸の最小値（指定しない場合は自動計算）
- **`maxX`** (`double`) - X軸の最大値（指定しない場合は自動計算）
- **`minY`** (`double`) - Y軸の最小値（指定しない場合は自動計算）
- **`maxY`** (`double`) - Y軸の最大値（指定しない場合は自動計算）
- **`baselineX`** (`double`) - X軸のベースライン
- **`baselineY`** (`double`) - Y軸のベースライン

### 表示設定

- **`titlesData`** (`FlTitlesData`) - 軸のタイトルとラベルの設定
- **`gridData`** (`FlGridData`) - グリッド線の表示設定
- **`borderData`** (`FlBorderData`) - グラフの枠線の設定
- **`backgroundColor`** (`Color`) - グラフの背景色
- **`extraLinesData`** (`ExtraLinesData`) - 追加の水平線・垂直線の設定
- **`rangeAnnotations`** (`RangeAnnotations`) - 範囲アノテーション（範囲の強調表示）

### インタラクション

- **`lineTouchData`** (`LineTouchData`) - タッチ操作とツールチップの設定
- **`showingTooltipIndicators`** (`List<ShowingTooltipIndicators>`) - 手動で表示するツールチップの設定

### その他

- **`clipData`** (`FlClipData`) - グラフの描画領域のクリッピング設定
- **`rotationQuarterTurns`** (`int`) - グラフを90度単位で回転（0, 1, 2, 3）

---

## LineChartBarData クラス

個々の線の設定を管理するクラス。

### 基本設定

- **`show`** (`bool`) - この線を表示するかどうか
- **`spots`** (`List<FlSpot>`) - 線が通る点のリスト（X, Y座標）

### 線のスタイル

- **`color`** (`Color`) - 線の色（単色）
- **`gradient`** (`Gradient`) - 線のグラデーション（`color`と排他的）
- **`gradientArea`** (`LineChartGradientArea`) - グラデーションの適用範囲
- **`barWidth`** (`double`) - 線の太さ（デフォルト: 2.0）
- **`dashArray`** (`List<int>`) - 破線のパターン（例: `[5, 5]`で5ピクセルの線と5ピクセルの空白）
- **`shadow`** (`Shadow`) - 線の影

### 曲線設定

- **`isCurved`** (`bool`) - 線を曲線にするかどうか（デフォルト: false）
- **`curveSmoothness`** (`double`) - 曲線の滑らかさ（0.0〜1.0、デフォルト: 0.35）
- **`preventCurveOverShooting`** (`bool`) - 曲線のオーバーシュート（はみ出し）を防ぐ
- **`preventCurveOvershootingThreshold`** (`double`) - オーバーシュート防止の閾値

### 線の端の設定

- **`isStrokeCapRound`** (`bool`) - 線の端を丸くするか（デフォルト: false）
- **`isStrokeJoinRound`** (`bool`) - 線の接合部を丸くするか（デフォルト: false）

### ステップチャート

- **`isStepLineChart`** (`bool`) - ステップラインチャート（階段状）にするか
- **`lineChartStepData`** (`LineChartStepData`) - ステップチャートの詳細設定

### データポイント（ドット）

- **`dotData`** (`FlDotData`) - データポイントの表示設定
  - `show` - ドットを表示するか
  - `getDotPainter` - ドットの描画方法をカスタマイズ

### 領域の塗りつぶし

- **`belowBarData`** (`BarAreaData`) - 線の下側の領域の塗りつぶし設定
  - `show` - 表示するか
  - `color` - 塗りつぶしの色
  - `gradient` - 塗りつぶしのグラデーション
  - `cutOffY` - 塗りつぶしの下限Y座標
  - `applyCutOffY` - cutOffYを適用するか

- **`aboveBarData`** (`BarAreaData`) - 線の上側の領域の塗りつぶし設定
  - 設定項目は`belowBarData`と同じ

### エラーバー

- **`errorIndicatorData`** (`FlErrorIndicatorData`) - エラーバーの表示設定
  - FlSpotに`xError`や`yError`を設定した場合に使用

### インジケーター

- **`showingIndicators`** (`List<int>`) - 特定のインデックスのポイントにインジケーターを表示

---

## 使用例

### 基本的な折れ線グラフ

```dart
LineChart(
  LineChartData(
    lineBarsData: [
      LineChartBarData(
        spots: [
          FlSpot(0, 1),
          FlSpot(1, 3),
          FlSpot(2, 2),
          FlSpot(3, 4),
        ],
        isCurved: true,
        color: Colors.blue,
        barWidth: 3,
        dotData: FlDotData(show: true),
      ),
    ],
    titlesData: FlTitlesData(show: true),
    gridData: FlGridData(show: true),
    borderData: FlBorderData(
      show: true,
      border: Border.all(color: Colors.black),
    ),
  ),
)
```

### 複数の線を表示

```dart
LineChart(
  LineChartData(
    lineBarsData: [
      LineChartBarData(
        spots: [...],
        color: Colors.blue,
      ),
      LineChartBarData(
        spots: [...],
        color: Colors.red,
      ),
    ],
  ),
)
```

### グラデーションと領域の塗りつぶし

```dart
LineChartBarData(
  spots: [...],
  gradient: LinearGradient(
    colors: [Colors.blue, Colors.purple],
  ),
  belowBarData: BarAreaData(
    show: true,
    gradient: LinearGradient(
      colors: [
        Colors.blue.withOpacity(0.3),
        Colors.purple.withOpacity(0.1),
      ],
    ),
  ),
)
```

### 破線

```dart
LineChartBarData(
  spots: [...],
  dashArray: [5, 5], // 5ピクセルの線、5ピクセルの空白
  color: Colors.green,
)
```

---

## 参考リンク

- [FL Chart 公式ドキュメント](https://pub.dev/packages/fl_chart)
- [LineChartData API](https://pub.dev/documentation/fl_chart/latest/fl_chart/LineChartData-class.html)
- [LineChartBarData API](https://pub.dev/documentation/fl_chart/latest/fl_chart/LineChartBarData-class.html)
