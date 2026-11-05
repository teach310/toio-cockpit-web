# toio-cockpit-web

スマホを運転席にして toio を操作する Web ページ。ボタンで前後に走り、スマホの傾きで曲がる。
[ToioCockpit](https://github.com/teach310/ToioCockpit)（M5Stack CoreS3 版）のメーターパネルをブラウザで再現している。

**https://teach310.github.io/toio-cockpit-web/**

## 必要なもの

- toio コア キューブ
- Web Bluetooth に対応したブラウザ
  - Android: Chrome
  - iPhone: Bluefy（App Store で入手）（Safari と iPhone 版 Chrome は Web Bluetooth に対応していない）

## 使い方

1. スマホを横向きにしてページを開く
2. 右上の **CONNECT** をタップし、傾きセンサーの許可と toio の選択をする
3. 右の **▲ FWD** / **▼ BACK** を押している間だけ走る。スマホを左右に傾けると曲がる

| 操作 | 動き |
|---|---|
| FWD / BACK | 押している間、前進 / 後退する。加速・減速はなめらか。後退中はバック音が鳴る |
| 傾き | 車のハンドルのように左右に回すと曲がる（平置きで傾けても同じ）。後退中は車のバックと同じ向きに曲がる |
| SPIN（左下） | 押している間、その場で右に速く回る。ほかの操作より優先 |
| HORN（メーター中央） | 押している間クラクションが鳴る |
| 速さのギア（左上） | タップで SLOW → LOW → HIGH を切り替える。次に開いたときも残る |
| 長押しで切断（右上） | 1.5 秒長押しで切断する。子どもが間違えて切らないよう長押しにしている |

メーターの下には toio の電池残量と、今の傾き（STEER）を表示する。

## 調整

走りや音の値は `index.html` の `<script>` の先頭に定数でまとめている。

| 定数 | 内容 |
|---|---|
| `SPEED_GEARS` | ギアごとの前進・後退の速さ（SLOW 40/30、LOW 70/50、HIGH 100/70） |
| `TURN_RATIO` | 最大まで傾けたときの左右の速さの差。走る速さに対する割合 |
| `STEER_GAIN` / `DEADZONE` | 傾きの効き方と、無視する小さな傾き |
| `ACCEL` / `DECEL` | 加速・減速の速さ（1 秒あたりに変わる速度） |
| `SPIN_SPEED` | SPIN で回る速さ |
| `DISCONNECT_HOLD_MS` | 切断に必要な長押しの時間 |
| `HORN_SOUND` / `BACKUP_SOUND` | クラクションとバック音の MIDI データ（ToioCockpit と同じ） |

## 開発メモ

- 1 ファイル（`index.html`）だけで動く。`main` に push すると GitHub Pages に反映される
- GitHub Pages はページを 10 分キャッシュする。更新をすぐ確かめるときは URL に `?v=2` のような値を付けて開く
- URL に `?debug` を付けると、触った位置と反応した要素を画面に出す（タッチが効かないときの調査用）
