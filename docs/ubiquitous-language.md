# ユビキタス言語

ゆびサッカー Ver.3.0 で使用するドメイン用語の定義一覧。
コード・ドキュメント・会話で表現を統一するために参照する。

出典: Meetings/2026-06-02〜2026-06-19

---

## プレイヤー・チーム

| 用語 | 定義 |
|---|---|
| `Player` | ゲーム内の操作単位。`Finger`（モーション入力）とは切り離した純粋なゲーム内概念 |
| `Team` | v1 は2チーム固定 |
| `GuestPlayer` | Apple Sign In 前の状態のプレイヤー |
| `AuthenticatedPlayer` | Apple Sign In 後のプレイヤー |
| `Skin` | キャラクターの見た目カスタマイズ要素。サーバーフラグで公開制御 |
| `Finger` | MediaPipe から取得する指の入力。ゲーム内概念ではなく、モーション入力層の概念 |

---

## 試合・ゲームフロー

| 用語 | 定義 |
|---|---|
| `Match` | 試合全体の単位 |
| `GameMode` | 試合の種類。`SoloPractice` / `1v1` / `2v2` / `3v3` |
| `SoloPractice` | 1人用トレーニングモード |
| `PrivateMatch` | 招待制マッチ。v0 はこれのみ（パブリックマッチングなし） |
| `Matching` | マッチング待機〜試合開始までのフロー |
| `Kickoff` | ゴール後・試合開始時の初期配置・UIアニメーション・関連処理をすべて含む概念 |
| `MatchStart` | 試合最初の `Kickoff` のトリガー |
| `Goal` | 得点イベント |
| `MatchEnd` | 試合終了。`TimeUp` またはスコア上限到達で発火 |

---

## ゲーム状態・スコア

| 用語 | 定義 |
|---|---|
| `MatchState` | 試合の状態。`Ready` / `Playing` / `Paused` / `Finished` |
| `Score` | チームごとの得点数 |
| `MatchTime` | 試合の残り時間（秒） |
| `TimeUp` | 残り時間が0になる試合終了トリガー |

---

## 座標・物理・操作

| 用語 | 定義 |
|---|---|
| `PlayerPosition` | プレイヤー座標。Client-side Prediction の対象 |
| `BallPosition` | ボール座標 |
| `PositionInterpolation` | クライアント側の座標補間処理 |
| `ServerRollback` | サーバーオーソリティによる座標ズレの修正 |
| `Kick` | プレイヤーのキック操作アクション |
| `Charge` | キックの溜め動作。矢印（ベクトル）で強さと方向を表示 |
| `MagnetZone` | ボール近接時にボールとプレイヤーを引き寄せる吸いつき判定エリア |

---

## ステージ・ギミック

| 用語 | 定義 |
|---|---|
| `Stage` | ステージ。Addressable Assets でサーバー配信 |
| `GoalArea` | ゴールエリア |
| `BoostFloor` | 加速床ギミック |
| `WarpZone` | ワープエリアギミック |
| `Pitfall` | 落とし穴ギミック。落下時は `Kickoff` で初期位置に戻す |

---

## 通信・セッション

| 用語 | 定義 |
|---|---|
| `Room` | マッチングの単位。マッチング成立〜解散まで存在する |
| `Session` | 試合中の接続状態。試合終了で破棄（Redis 管理） |
| `ServerAuthority` | ゴール・スコア判定の最終権限をサーバーが持つ方式 |
| `ClientPrediction` | プレイヤー座標のクライアント先行処理 |

---

## 画面遷移

| 用語 | 定義 |
|---|---|
| `TitleScreen` | タイトル画面 |
| `MatchingScreen` | マッチング待機画面 |
| `GameScreen` | 試合中画面 |
| `ResultScreen` | リザルト画面 |
| `SettingScreen` | 設定画面 |

---

## 変更履歴

| 日付 | 変更者 | 内容 |
|---|---|---|
| 2026-06-22 | — | 初版作成（Meetings 2026-06-02〜19 をもとに策定） |
