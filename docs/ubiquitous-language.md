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
| `TeamLeader` | チームを代表するプレイヤー。ホスト側チームはホストが、相手チームはホストが指名したゲストが務める。チーム名・チームアイコン・各メンバーの背番号を設定できる |
| `GuestPlayer` | Apple Sign In 前の状態のプレイヤー |
| `AuthenticatedPlayer` | Apple Sign In 後のプレイヤー |
| `Skin` | キャラクターの見た目カスタマイズ要素。サーバーフラグで公開制御 |
| `Finger` | MediaPipe から取得する指の入力。ゲーム内概念ではなく、モーション入力層の概念 |
| `PlayerState` | プレイヤーの行動状態。`Normal`（待機）/ `Run`（走り）/ `Charge`（溜め）/ `Kick`（蹴り） |
| `TeamName` | チームの名称。未設定時はテンプレート（Alpha / Bravo 等）を使用する |
| `JerseyNumber` | 各プレイヤーの背番号。未設定時はテンプレート値（1, 2, 3…）を使用する |

---

## 試合・ゲームフロー

| 用語 | 定義 |
|---|---|
| `Match` | 試合全体の単位 |
| `MatchFormat` | 試合の人数形式。`1v1` / `2v2` / `3v3` 等（旧称 `GameMode` の一部。モード選択肢とは区別する） |
| `GameMode` | プレイヤーがモード選択画面で選ぶ種類。`TutorialMission` / `TutorialFreePlay` / `Practice` / `MatchingHost` / `MatchingGuest` |
| `PrivateMatch` | 招待制マッチ。v0 はこれのみ（パブリックマッチングなし） |
| `Matching` | マッチング待機〜試合開始までのフロー |
| `MatchIntro` | マッチング完了後・試合開始前の入場演出。チーム紹介・VS 演出・カウントダウンを含む |
| `KickoffReset` | ゴール後に行うリスタート処理。ボール・プレイヤーを初期配置に戻してプレイ再開を待つ |
| `KickoffCountdown` | `KickoffReset` 完了後（および試合最初）に表示する「3…2…1…Start」カウントダウン演出。この演出が終わるとプレイ開始となる |
| `MatchStart` | 試合の最初の `KickoffCountdown` 完了により発火するイベント |
| `Goal` | 得点イベント。`GoalEvent`（得点チーム + `BallType`）として通知される |
| `MatchEnd` | 試合終了。`TimeUp` またはスコア上限到達で発火 |

---

## ゲームルール

操作方法は共通のまま、勝利条件・スコア計算・終了判定を差し替えられる仕組み。サッカー / ビリヤード / 100m走など。パラメータ層（SO）とロジック層（Strategy）の2層で構成する。

| 用語 | 定義 |
|---|---|
| `GameRule` | ゲームのルール。`GameRuleSet`（パラメータ）と `IGameRule`（ロジック）の組で表現する |
| `GameRuleSet` | ルールのパラメータ集合。試合時間・スコア上限・チーム数・ゴール数・使用ボール/アイテム/ギミックを保持（`GameRuleSetSO` で管理） |
| `IGameRule` | ルールロジックの差し替え単位（Strategy）。勝利条件・スコア計算・終了判定を担う |
| `StandardSoccerRule` | v1 の標準サッカールール。`BilliardsRule` / `DashRule` は将来追加予定 |
| `RuleContext` | 実行時に有効な `GameRuleSet` と `IGameRule` を保持する実行時コンテキスト |

---

## ゲーム状態・スコア

| 用語 | 定義 |
|---|---|
| `MatchPhase` | 試合のライフサイクル状態。`Ready` / `Playing` / `Paused` / `Finished`（旧称 `MatchState` の状態定義部分） |
| `MatchSnapshot` | 特定時点の試合データ。チームごとのスコアと経過時間を保持するレコード（旧称 `MatchState` のデータ定義部分） |
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

## ボール

| 用語 | 定義 |
|---|---|
| `BallType` | ボール種別。v1: `Standard` / `HighScore`。将来追加予定: `Bomb` / `Interference` / `RandomFly` |
| `HighScore` | ゴール時の得点が高いボール（スコア係数を SO で設定） |
| `BallSpawner` | `GameRuleSet` のボールリストに基づきボールを自動生成・場外リスポーンする仕組み |

---

## アイテム

| 用語 | 定義 |
|---|---|
| `Item` | フィールドに自動生成され、取得すると一定時間効果を付与するオブジェクト |
| `ItemType` | アイテム種別。v1: `SpeedUp` / `KickPower`。将来追加予定: `LegPower` |
| `SpeedUp` | 取得プレイヤーの移動速度を一定時間上昇させるアイテム |
| `KickPower` | 取得プレイヤーのキック力を一定時間上昇させるアイテム |

---

## ステージ・ギミック

| 用語 | 定義 |
|---|---|
| `Stage` | ステージ。Addressable Assets でサーバー配信 |
| `GoalArea` | ゴールエリア |
| `GimmickType` | ギミック種別。v1: `Pitfall` / `MultiGoal`。将来追加予定: `Warp` / `WindUp` / `AccelFloor` / `MovingGoal` |
| `Pitfall` | 落とし穴ギミック。落下時は `KickoffReset` で初期位置に戻す |
| `MultiGoal` | 複数ゴールを有効化するギミック（スイッチ）。有効時のみ `GoalCount` > 1 が機能する |
| `GoalCount` | 配置するゴールの数。`MultiGoal` 無効時は 1 固定 |
| `AccelFloor` | 加速床ギミック（将来追加予定） |
| `Warp` | ワープエリアギミック（将来追加予定） |

---

## 通信・セッション

| 用語 | 定義 |
|---|---|
| `Room` | マッチングの単位。マッチング成立〜解散まで存在する |
| `Session` | 試合中の接続状態。試合終了で破棄（Redis 管理） |
| `ServerAuthority` | ゴール・スコア判定の最終権限をサーバーが持つ方式 |
| `ClientPrediction` | プレイヤー座標のクライアント先行処理 |

---

## エラーコード

サーバー〜クライアント間で共有するエラー識別子。文字列定数として定義する。

| コード | 意味 |
|---|---|
| `ROOM_FULL` | ルームが満員のため入室不可 |
| `AUTH_FAILED` | 認証失敗 |
| `GAME_ALREADY_STARTED` | 試合が既に開始済みのため入室不可 |
| `INVALID_ACTION` | 不正な操作（試合開始前のキック等） |
| `RECONNECT_EXPIRED` | 再接続の猶予時間が切れた |
| `VERSION_MISMATCH` | クライアントのバージョンが古い。強制アップデートを促す |
| `ROOM_NOT_FOUND` | 指定された `RoomCode` のルームが存在しない |

---

## モード・チュートリアル

| 用語 | 定義 |
|---|---|
| `Tutorial` | 操作を学ぶモード。`TutorialMission` と `TutorialFreePlay` の2形式を持つ |
| `TutorialMission` | ミッション形式のチュートリアル。走る・蹴る・ゴールなどの課題を1つずつクリアする |
| `TutorialFreePlay` | フリープレイ形式のチュートリアル。ゴールとボールのみのステージで自由に動く（カウントなし） |
| `Mission` | `TutorialMission` で1つずつ提示される達成課題（`MissionSO` で定義） |
| `Practice` | サンドボックスモード。ステージ・ルール・ギミック・アイテムを自由に選択して1人でプレイする |
| `Host` | プライベートマッチのルーム作成側。`RoomCode` を発行し、チーム分けと `TeamLeader` 指名を行う |
| `Guest` | プライベートマッチのルーム参加側。`RoomCode` を入力して参加する |
| `RoomCode` | ルーム参加用の6桁の数字コード（本番はサーバー生成） |

---

## 画面遷移

`SceneNavigator` がシーン遷移を一元管理し、遷移先は `SceneId` enum で表す（文字列直打ちを排除）。

| 用語 | 定義 |
|---|---|
| `SceneNavigator` | シーン遷移を一元管理するサービス |
| `SceneId` | 遷移先シーンの識別子。`Splash` / `Title` / `ModeSelect` / `TutorialMission` / `TutorialFreePlay` / `Practice` / `MatchingHost` / `MatchingGuest` / `MatchIntro` / `Game` |
| `SplashScreen` | 起動時スプラッシュ画面。ゲスト認証（Apple Sign In 前）をここで済ませる |
| `TitleScreen` | タイトル画面 |
| `ModeSelectScreen` | モード選択画面 |
| `MatchingScreen` | マッチング待機画面（ホスト / ゲスト共通シーン） |
| `GameScreen` | 試合中画面 |
| `ResultScreen` | リザルト画面 |
| `SettingScreen` | 設定画面 |

---

## 変更履歴

| 日付 | 変更者 | 内容 |
|---|---|---|
| 2026-06-22 | — | 初版作成（Meetings 2026-06-02〜19 をもとに策定） |
| 2026-06-22 | — | spec-01〜14 由来の用語を追加（ゲームルール / ボール / アイテム / モード・チュートリアル）、ギミック・画面遷移を拡充、用語の衝突を整理 |
| 2026-06-22 | — | 衝突解消（`GameMode`→`MatchFormat`/`GameMode` 分離、`MatchState`→`MatchPhase`/`MatchSnapshot` 分離、`AccelFloor`/`Warp` に統一）、`Kickoff` を `KickoffReset`/`KickoffCountdown` に分割、`TeamLeader`/`PlayerState`/`TeamName`/`JerseyNumber` を追加、エラーコード節を新設 |
