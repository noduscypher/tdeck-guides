> ⚠️ Before you start  
> This guide focuses on using the T‑Deck / T‑Deck Plus with Meshtastic (firmware, UI, maps, battery, troubleshooting).  
> It does not define legal limits or network‑wide best practices.  
> For EU868 presets, roles (CLIENT, ROUTER, etc.), conservative range and regulatory notes in PT/EN/ES, always refer to:  
> https://github.com/noduscypher/mesh-guides
>
> 
# T-Deck Plus Meshtastic 完全ガイド

**バージョン 1.0 | 2026年4月**

> 他の言語で読む: [PT](../../pt/technical/complete-guide.md) · [EN](../../en/technical/complete-guide.md) · [ES](../../es/technical/complete-guide.md)

> 📝 **このバージョンに関する注記:** 元のファイルは大幅にフォーマットが失われていた（複数のテーブル、コードブロック、ASCII 図表が消失）。このバージョンはクリーンな Markdown で構造を再構築している。コンテンツが完全に失われた箇所は ⚠️ で印付け、公開前の確認が必要である。✓ マーカーは高信頼度の再構築を示す。本書は技術版のため、日本語の常体（だ・である調）で記述している。**公開前に日本語ネイティブによる校正を推奨する。**

---

## 目次

1. [はじめに](#はじめに)
2. [必要なもの](#必要なもの)
3. [プログラミングモード（DFU/Flash）](#プログラミングモードdfuflash)
4. [ファームウェアのインストール — Erase + Flash](#ファームウェアのインストール--erase--flash)
5. [ファームウェアの更新（通常）](#ファームウェアの更新通常)
6. [初期設定](#初期設定)
7. [Bluetooth — プログラミングモード](#bluetooth--プログラミングモード)
8. [Bluetooth — スタンドアロン使用のため無効化](#bluetooth--スタンドアロン使用のため無効化)
9. [オフラインマップ（任意）](#オフラインマップ任意)
10. [キーボードショートカット](#キーボードショートカット)
11. [トラブルシューティング](#トラブルシューティング)
12. [追加リソース](#追加リソース)
13. [完全インストール・チェックリスト](#完全インストールチェックリスト)

---

## はじめに

LilyGO T-Deck Plus は、画面、QWERTY キーボード、内蔵 GPS、LoRa を備えた完全スタンドアロンの Meshtastic デバイスである。本ガイドは初期インストールから高度な設定までをカバーする。

**主な特徴:**

- ESP32-S3 + SX1262 LoRa
- L76K GPS 内蔵
- 2.8 インチ TFT タッチスクリーン
- 物理 QWERTY キーボード
- 18650 バッテリー（同梱されない）
- オフラインマップ用 MicroSD

---

## 必要なもの

### ハードウェア

- LilyGO T-Deck Plus
- USB-C ケーブル（充電専用ではなくデータ対応）
- 18650 バッテリー（推奨: 3000 mAh 以上）
- LoRa アンテナ 868 MHz（欧州）または 915 MHz（米国）
- MicroSD カード FAT32 ≤32 GB（任意、マップ用）

### ソフトウェア

- Chromium 系ブラウザ（Chrome、Edge、Brave）
- インターネット接続
- Bluetooth ペアリング用 Meshtastic App（Android/iOS）

### 重要な警告

> ⚠️ **アンテナを接続せずにデバイスの電源を入れないこと** — LoRa モジュールが破損する可能性がある。
>
> ⚠️ 必ずデータ対応の USB-C ケーブルを使うこと（充電専用は不可）。
>
> ⚠️ 正しい地域を設定すること（ポルトガルでは `EU_868`）。ANACOM（Autoridade Nacional de Comunicações、ポルトガル国家通信局）の規制に準拠するためである。

---

## プログラミングモード（DFU/Flash）

ファームウェアを書き込む、または erase を行うには、T-Deck Plus を DFU モードに入れる必要がある。

### 手順

1. T-Deck の電源を切る（側面スイッチを OFF）。
2. トラックボール（中央ボタン）を押し続ける。
3. トラックボールを押したまま、T-Deck の電源を入れる（スイッチ ON）。
4. トラックボールを押したまま 2〜3 秒待つ。
5. トラックボールを離す。
6. USB-C ケーブルをコンピュータに接続する。

### DFU モードの確認

- T-Deck の画面が完全に黒くなる（バックライトなし）。
- コンピュータに **「USB JTAG/serial debug unit」** または **「ESP32-S3」** というデバイスが現れる。

> **注:** 一度でうまくいかない場合は手順を繰り返す。USB-C - USB-C ケーブルが原因で問題が発生することがある — USB-A - USB-C ケーブルを試すこと。

---

## ファームウェアのインストール — Erase + Flash

**使用タイミング:** 初回インストール、深刻な問題があるデバイス、ファームウェアタイプの変更（例: alpha → beta）。

### ステップごとの手順

1. **プログラミングモードに入る**
   前のセクションの手順に従う。

2. **Web Flasher にアクセス**
   - Chromium 系ブラウザを開く
   - 移動先: <https://flasher.meshtastic.org>

3. **デバイスを選択**
   - 「Select Target Device」をクリック
   - **「LILYGO T-Deck」** を選ぶ（T-Deck Plus でも動作する）

4. **ファームウェアバージョンを選ぶ**
   - **推奨:** 最新の BETA バージョンを選ぶ（プロダクションでは alpha を避ける）
   - 日常使用には alpha バージョンを使わないこと

5. **Full Erase の設定**
   - ✅ オプションを有効化: **「Full Erase and Install」**
   - これにより古い設定を含むフラッシュ全体が消去される

6. **デバイスを接続**
   - 「Connect」をクリック
   - ポートを選択: 「USB JTAG/serial debug unit」
   - 「Connect」をクリック

7. **Flash**
   - 「Flash」をクリックし、続いて「CONTINUE」をクリック
   - 完了を待つ（1〜3 分）
   - **処理中に USB を切断しないこと**

8. **完了**
   - 「Flash Complete」が表示されたらブラウザを閉じる
   - T-Deck の電源を切る（スイッチ OFF）
   - 再度電源を入れる（スイッチ ON）
   - デバイスは新しいファームウェアと MUI（Meshtastic UI）で起動する

---

## ファームウェアの更新（通常）

**使用タイミング:** 設定を失わずに最新バージョンに更新する。

### ステップごとの手順

1. **プログラミングモードに入る**
   手順に従う: 電源を切る → トラックボール押下 → 電源を入れる → 待つ → 離す → USB を接続。

2. **Web Flasher**
   - 移動先: <https://flasher.meshtastic.org>
   - 選択: 「LILYGO T-Deck」
   - 希望するファームウェアバージョンを選ぶ

3. **Update（Erase なし）**
   - ❌ 「Full Erase」オプションを **無効化** する
   - 「Flash」をクリック

4. **Flash と再起動**
   - 完了を待つ
   - T-Deck の電源を切って入れ直す
   - 設定は保持される（地域、ノード名、チャンネルなど）

> **注:** 更新後に問題が発生した場合は、Full Erase + Flash を行うこと。

---

## 初期設定

初回フラッシュ後、必須パラメータを設定する。

### Meshtastic Web Client 経由

1. **USB シリアル経由で接続**
   - 移動先: <https://client.meshtastic.org>
   - 「Connect」→「USB Serial」をクリック
   - T-Deck のポートを選択

2. **LoRa 地域の設定**
   - サイドメニュー: **「LoRa」**
   - Region: **EU_868** を選択（ポルトガル/欧州）
   - 「Save」をクリック

   > ⚠️ **ポルトガルでは必須:** EU_868（868 MHz、500 mW、デューティサイクル 10%）、ANACOM 規制に準拠。

3. **GPS 位置情報の設定**
   - メニュー: **「Position」**
   - GPS Mode: 「Enabled」
   - GPS Update Interval: 120 秒（必要に応じて調整）
   - Position Broadcast: 位置情報を共有したい場合に有効化
   - 「Save」をクリック

4. **ノード情報（任意）**
   - メニュー: **「Settings」→「Device」**
   - Node Name: 自分のノード名を設定
   - Role: CLIENT（通常使用）または ROUTER（固定ノード）

5. **チャンネル（任意）**
   - メニュー: **「Channels」**
   - 追加チャンネルを設定するか、既存のメッシュに参加する
   - Primary Channel は事前設定済み

### T-Deck 上で MUI 経由

Web/app による初期設定後、T-Deck 上で直接調整できる:

- **Settings → LoRa** → 地域を確認
- **Settings → Position** → GPS の状態
- **Map** → ノードと GPS 位置を表示

---

## Bluetooth — プログラミングモード

**使用タイミング:** スマートフォンとの初回ペアリング、アプリ経由の設定、トラブルシューティング。

### Bluetooth プログラミングモードの有効化

#### MUI 経由（ファームウェア 2.6+）

1. T-Deck で **Settings**（歯車アイコン）に移動
2. **「Reboot/Shutdown」** を選択
3. Bluetooth アイコン（画面中央）をクリック
4. 画面に **「>> Programming Mode <<」** とパスコード（例: 123456）が表示される

#### スマートフォンとのペアリング

1. Meshtastic App（Android/iOS）を開く
2. Bluetooth → Scan に移動
3. 自分の T-Deck（ノード名）を選択
4. T-Deck の画面に表示されたパスコードを入力
5. 接続してアプリ経由で設定

### Bluetooth プログラミングモードの終了

1. T-Deck の画面で Bluetooth アイコンを長押し
2. デバイスは再起動して通常の MUI に戻る

> **重要:**
> - Bluetooth プログラミングモードは初回ペアリング/設定用である。
> - 日常的な Bluetooth 使用には BaseUI に切り替えること（次のセクション）。
> - MUI は Bluetooth の常時稼働をサポートしない（同じ API を使用するため）。

---

## Bluetooth — スタンドアロン使用のため無効化

**使用タイミング:** スマートフォンなしでのスタンドアロン運用、バッテリー節約、マップ付きの MUI 使用。

### オプション 1: MUI を使用（推奨スタンドアロンモード）

MUI（Meshtastic UI）はフル機能のビジュアルインターフェースだが、Bluetooth との同時使用はサポートしない。

- **利点:** マップ、キーボード、メッシュの完全な可視化
- **欠点:** アプリ用の Bluetooth が使えない

Bluetooth が確実にオフであることを確認するには:

**Meshtastic CLI 経由:**

```bash
meshtastic --set bluetooth.enabled false
```

> ✓ 復元済み: `meshtastic --set bluetooth.enabled false` (standard CLI syntax, confident)

**または Web Client 経由:**

- Settings → Bluetooth → Enabled: OFF → Save

### オプション 2: BaseUI（Bluetooth 常時有効）

アプリ用に Bluetooth を 24/7 常時有効にする必要がある場合:

#### BaseUI への切り替え

**CLI 経由:**

> ⚠️ **[要確認]** ソースから CLI コマンドが消失している。Meshtastic の現在のドキュメントに対して正しい構文を確認すること — BaseUI への切り替え用の設定キーはバージョンによって異なる可能性がある。代わりに、下記の Web Client 経由のパスを使用すること。

**または Web Client 経由:**

- Settings → Device → Display Type: BaseUI
- Save して再起動

**BaseUI の特徴:**

- シンプルなインターフェース（ビジュアルマップなし）
- Bluetooth が常時動作
- リソース消費が少ない
- アプリ経由の制御に最適

### 簡易比較

> ⚠️ ドキュメント自身の記述的文脈から再構築されたテーブル。公開前に公式ドキュメントと照合して確認すること。

| 機能 | MUI | BaseUI |
|---|---|---|
| ビジュアルマップ | ✅ | ❌ |
| 完全なキーボードとナビゲーション | ✅ | 部分的 |
| アプリ用の常時 Bluetooth | ❌（プログラミングモードのみ） | ✅ |
| リソース消費 | 高い | 低い |
| ユースケース | マップ付きスタンドアロン | アプリ駆動制御 |

> **推奨:** マップ付きのスタンドアロンには MUI を使用する。スマートフォンのアプリを常時接続する必要がある場合は BaseUI に切り替える。

---

## オフラインマップ（任意）

T-Deck Plus にオフラインマップを追加すると、メッシュネットワークを実際の地図上に表示できる。

### 必要なもの

- MicroSD カード FAT32、≤32 GB
- マップタイルパック（OpenStreetMap、Google Maps、Atlas）

### 正しいフォルダ構造

タイル構造は OpenStreetMap の標準 XYZ 形式に従う:

```
/maps/
└── <パック名>/
    ├── 0/                  ← ズームレベル 0（世界全体）
    ├── 1/
    ├── 2/
    ├── ...
    ├── 8/                  ← ポルトガル用に必須のズームレベル
    ├── 9/
    ├── 10/
    ├── 11/
    ├── 12/                 ← 高ズーム、多数のタイル
    ├── 13/
    ├── 14/
    └── 15/                 ← 最大ズーム
```

**重要:**

- 番号付きフォルダ（0〜15）はズームレベルに対応する。
- 各ズームフォルダ内には `X/` サブフォルダと `Y.png` ファイルがある（XYZ 形式）。
- 低ズームレベル（0〜5）は少数のタイルで世界全体をカバーする。
- 高ズームレベル（8〜15）は多数のタイルを含み、ローカルな詳細に必須である。
- ポルトガルではズーム 8〜14 が実用上重要となる。
- ズーム 1〜5 は空でも問題ない。

### ステップごとのインストール

#### 方法 1: Atlas プリビルドパック（グローバル推奨）

1. **マップパックをダウンロード:**
   - Atlas（グローバル、欧州を含む）: <https://github.com/meshtastic/device-ui/tree/master/maps>
   - ファイル: `atlas.zip` ⚠️ *[正確なファイル名はリポジトリで要確認]*

2. **ZIP を展開:**
   - `atlas.zip` を解凍
   - `0/` から `15/` のサブフォルダを持つ `atlas/` フォルダができる

3. **SD カードにコピー:**
   - SD カードのルートに `/maps/` フォルダを作成
   - `atlas/` フォルダ全体を `/maps/atlas/` にコピー
   - 最終構造: `/maps/atlas/[0-15]/...`

4. **内容を確認:**
   - `/maps/atlas/12/`（または別の高ズームレベル）を開く
   - サブフォルダと `.png` ファイルが含まれているはず
   - 空の場合、パックが破損している

#### 方法 2: Portugal パック（mapsportugal115OSM.zip）

ポルトガル専用パックを使う場合:

1. **ZIP ファイルを展開:**
   - `mapsportugal115OSM.zip` を解凍
   - フォルダ構造ができる

2. 解凍した ZIP 内の **`openstreetmap/` フォルダ** に移動。

3. **`openstreetmap/` フォルダの内容をコピー:**
   - 解凍した ZIP の `openstreetmap/` フォルダに入る
   - すべての番号付きフォルダ（1、2、3... 15 まで）を選択
   - コピー（Ctrl+C / Cmd+C）

4. **SD の「Google Maps」フォルダに貼り付け:**
   - SD をコンピュータに挿入
   - SD の `/maps/Google Maps/` に移動
   - コピーしたコンテンツをすべて貼り付け（Ctrl+V / Cmd+V）

5. **SD 上の最終構造:**

   ```
   /maps/
   └── Google Maps/
       ├── 1/
       ├── 2/
       ├── ...
       └── 15/
   ```

6. **インストールを確認:**
   - SD で `/maps/Google Maps/12/`（または別の高ズームレベル）を開く
   - 番号付きサブフォルダとファイルが含まれているはず
   - 空の場合、展開とコピーをやり直す

7. **T-Deck で使用:**
   - SD を T-Deck に挿入
   - Map に移動
   - マップアイコンを長押し → 「Google Maps」を選択
   - ポルトガルにズームし、GPS の fix を待つ

> **注:** SD 上の「Google Maps」というフォルダ名はラベルにすぎない — 「Portugal」や他の名前に変更しても構わない。重要なのは内部構造（タイルを含む 1〜15 のフォルダ）である。

#### 方法 3: カスタムタイルの生成

**MapDL Python スクリプト:**

```bash
# スクリプトをダウンロード
wget https://gist.githubusercontent.com/droberin/b333a216d860361e329e74f59f4af4ba/raw/MapDL.py

# ポルトガル用の座標を編集
# lat: 37〜42
# lon: -9.5〜-6
# zoom: 8〜14

python3 MapDL.py

# 出力を /maps/portugal/ にコピー
```

**MOBAC（Mobile Atlas Creator）:**

- ダウンロード: <https://mobac.sourceforge.io>
- 地域選択: ポルトガル
- ズームレベル: 8〜14
- 出力フォーマット: OSM Tile format
- エクスポートして SD にコピー

### T-Deck でマップを使用する

1. SD を T-Deck に挿入する（側面スロット）。
2. デバイスの電源を入れる。
3. Map（マップアイコン）に移動する。
4. マップアイコンを長押し（または Settings → Map）。
5. ソースを選択: `atlas`、`Google Maps`、`portugal`、または使用した名前。
6. ポルトガルにズームインする（トラックボール + スクロール）。
7. 屋外で GPS fix — 位置がマップ上に表示される。

### マップのトラブルシューティング

**マップが表示されない:**

- SD は FAT32 でフォーマットされているか?（NTFS/exFAT は不可）
- 構造 `/maps/<名前>/[0-15]/` は正しいか?
- ズーム 8〜14 のフォルダに `.png` ファイルがあるか?

**マップの読み込みが遅い:**

- SD が遅い（class 10 以上を推奨）。
- ズームレベルが多すぎる（使わないなら 1〜7 を削除）。

**座標が正しくない:**

- GPS fix を待つ（コールドスタートで初期 5〜10 分かかる場合あり）。
- 空が開けた屋外に出る。

---

## キーボードショートカット

> ⚠️ **[元のコンテンツ消失 — 公開前に確認]**
>
> T-Deck Plus のキーボードショートカット完全テーブルは破損したソースから復元できなかった。次のいずれかから復元することを推奨する:
>
> - 公式 device-ui リポジトリ: <https://github.com/meshtastic/device-ui>
> - Meshtastic 公式ドキュメント: <https://meshtastic.org/docs>
> - Meshtastic Portugal コミュニティの議論: <https://meshtastic.pt>

### 一般的な注記（ソースから復元）

- **ALT + キー / SYM + キー:** 押して離す（押し続ける必要はない）。
- `SYM + キー` のショートカットは SND および SET タブでは有効でない。
- **Fn** キーはファームウェア 2.7+ で有効化が必要な場合がある。

---

## トラブルシューティング

### デバイスが DFU モードに入らない

**症状:** 画面は通常通り点灯するが、コンピュータがフラッシュデバイスを検出しない。

**解決策:**

1. シーケンスを繰り返す: 電源切 → トラックボール押下 → 電源入 → 3 秒待つ → 離す。
2. ケーブルをテスト: USB-A - USB-C を使用する（C-C ではなく）。
3. ドライバ: Windows では CH340/CP210x ドライバが必要な場合がある。
4. 3〜5 回試す: タイミングが重要。

### Bluetooth が接続しない

**症状:** アプリがデバイスを認識しない、ペアリングが失敗する。

**解決策:**

**MUI の場合:**

- Bluetooth は通常の MUI では動作しない。
- Bluetooth プログラミングモードを使用: Settings → Reboot → Bluetooth アイコン。
- または BaseUI に永続的に切り替える。

**Bluetooth が無効化されている場合:**

```bash
meshtastic --set bluetooth.enabled true
```

> ✓ 復元済み: `meshtastic --set bluetooth.enabled true` (standard CLI syntax, confident)

**ファームウェア更新後の場合:**

- 自動的に無効化されることがある。
- CLI または Web client から再有効化する。
- 解決しない場合、以前のファームウェア（2.5.18 が動作することが知られている）にロールバックする。

**iPhone の問題:**

- 一部のファームウェア（初期 2.6.x）には iOS のバグがある。
- 代替として USB 経由の Web client を使用する。
- 最新の beta ファームウェアに更新する。

### GPS が fix しない

**症状:** 位置が更新されない、「No GPS fix」が継続する。

**解決策:**

1. 屋外に出る: GPS は空が開けた視界が必要。
2. 5〜15 分待つ: 初回 fix（コールドスタート）には時間がかかる。
3. 設定を確認:

   ```bash
   meshtastic --get position
   ```

   > ✓ 復元済み: `meshtastic --get position` (standard CLI syntax, confident)

4. 正しい GPIO ピン（T-Deck Plus）:
   - RX: GPIO 34
   - TX: GPIO 12
   - Web client 経由で設定: Position → GPS Pins

5. GPS アンテナ: 一部のモデルにはセラミックアンテナが内蔵されており、他のモデルは外部アンテナが必要。

### マップが表示されない

[オフラインマップ](#オフラインマップ任意)の[マップのトラブルシューティング](#マップのトラブルシューティング)セクションを参照。

### デバイスの電源が入らない

**解決策:**

1. **バッテリー:** 18650 が正しく挿入されているか確認する（+/- の極性）。
2. **充電:** USB-C を接続し、30 分待つ。
3. **側面スイッチ:** ON の位置を確認する。
4. **リセット:** RESET ボタン（側面の小さい穴）を押す。

### ファームウェア Brick / 起動しない

**解決策:**

1. **Full Erase + Flash:** DFU モード → Full Erase → beta ファームウェアを Flash。
2. **異なるファームウェアを試す:** 2.5.x vs 2.6.x。
3. **Meshtastic CLI noproto:**

   ```bash
   meshtastic --noproto
   ```

   > ✓ 復元済み: `meshtastic --noproto` (standard CLI syntax, confident)

4. **CLI 経由の Factory reset:**

   ```bash
   meshtastic --factory-reset
   ```

   > ✓ 復元済み: `meshtastic --factory-reset` (standard CLI syntax, confident)

### ノードが表示されない / メッシュが空

**解決策:**

1. **正しい地域:** ポルトガルでは EU_868。
2. **アンテナ接続:** アンテナなし = TX/RX なし。
3. **周波数スロット:** ローカルメッシュが異なるスロットを使っていないか確認する。
4. **範囲:** ノードは LoRa 範囲内にある必要がある（都市部 約 5 km、地方 20 km 以上）。
5. **チャンネル:** Primary Channel はローカルメッシュと一致する必要がある。

### バッテリー消費が高い

**解決策:**

1. 使わない場合は **Bluetooth を無効化** する。
2. **GPS update interval:** 300 秒以上に増やす（120 秒の代わりに）。
3. **Screen timeout:** 画面が消えるようにタイムアウトを有効化する。
4. **Role:** 移動する場合は CLIENT（ROUTER ではなく）に設定する。

> 詳細は [battery-energy-guide.md](battery-energy-guide.md) を参照。

---

## 追加リソース

### 公式リンク

- **Meshtastic Web Flasher:** <https://flasher.meshtastic.org>
- **Meshtastic Web Client:** <https://client.meshtastic.org>
- **公式ドキュメント:** <https://meshtastic.org/docs>
- **ファームウェアリリース:** <https://github.com/meshtastic/firmware/releases>

### ポルトガル・コミュニティ

- **Meshtastic PT:** <https://meshtastic.pt>
- **ポルトガル・ノードマップ:** <https://meshtastic.pt/map>
- **Malha PT:** <https://malha.meshtastic.pt>
- **PT FAQ:** <https://meshtastic.pt/faq>
- **Facebook Meshtastic Portugal:** サポート活動が活発なグループ

### マップリソース

- **MUI Maps GitHub:** <https://github.com/meshtastic/device-ui/tree/master/maps>
- **MapDL Script:** <https://gist.github.com/droberin/b333a216d860361e329e74f59f4af4ba>
- **MOBAC:** <https://mobac.sourceforge.io>
- **Wind & Wireless Guide:** <https://windandwireless.com/pages/techlab/meshtasticmaps>

### Python CLI

```bash
# CLI のインストール
pip3 install --upgrade "meshtastic[cli]"

# 便利なコマンド
meshtastic --info                              # ノード情報
meshtastic --nodes                             # メッシュのノード一覧
meshtastic --set lora.region EU_868            # 地域を設定
meshtastic --set bluetooth.enabled false       # Bluetooth を無効化
meshtastic --get device                        # 完全な設定を取得
meshtastic --factory-reset                     # 完全リセット
```

### ポルトガル規制（ANACOM）

- **周波数:** 868 MHz（EU_868）
- **最大電力:** 500 mW（27 dBm）
- **デューティサイクル:** 10%
- **代替:** 433 MHz、10 mW、デューティサイクル 10%

---

## 完全インストール・チェックリスト

- [ ] LoRa アンテナ接続済み
- [ ] 18650 バッテリー挿入済み
- [ ] ファームウェア書き込み済み（beta 推奨）
- [ ] LoRa 地域: EU_868
- [ ] GPS 有効化、ピン設定正しい
- [ ] ノード名設定済み
- [ ] Bluetooth: プログラミングモードをテスト + 無効化（スタンドアロンの場合）
- [ ] オフラインマップインストール済み（任意）
- [ ] GPS fix 取得済み（屋外）
- [ ] 最初のメッシュノードを検出
- [ ] スマートフォンアプリ・ペアリング済み（Bluetooth 使用の場合）

---

## ガイド変更履歴

- **v1.0（2026年4月）:** 完全初版。

---

> **コントリビューション:** このガイドはオープンソースである。改善および修正は Meshtastic PT コミュニティを通じて歓迎する。

---

☥ cypherpunks write code /// nature writes resilience /// we bridge both ☥

CC BY-SA 4.0 でライセンス · <https://creativecommons.org/licenses/by-sa/4.0/>

*walk quietly, but keep the signal alive.*

₿uilt with Love 🧡 in cooperation with Nature 🌿
