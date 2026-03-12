# Marp スライド

Markdown でスライドを作成する [Marp](https://marp.app/) 用リポジトリです。

---

## Marp の主要な構文

### フロントマター（必須）

スライド先頭で Marp を有効にします。

```yaml
---
marp: true
---
```

### スライドの区切り

`---` （水平線）でスライドを分割します。

```markdown
# 1枚目

内容

---

# 2枚目

内容
```

### 見出し・本文

- `#` 〜 `######` で見出し（`#` がタイトル、`##` がセクションなど）。
- 通常の Markdown でリスト・強調・リンクなどが使えます。

```markdown
## タイトル

- 箇条書き
- **太字** / *斜体*
- [リンク](https://example.com)
```

### 画像

```markdown
![代替テキスト](画像パス.png)
```

画像の高さは `global.css` で共通指定しています（後述）。

### 映像

HTML5 の `<video>` タグで動画を埋め込めます。**HTML 出力でのみ再生**され、PDF・PPTX では動画は再生されません。

1. **設定**: `marp.json` に `"html": true` を追加するか、CLI で `--html` オプションを付けて実行します。
2. **記述**: Markdown 内に HTML で `<video>` タグを書きます。

```markdown
<video controls width="100%">
  <source src="動画ファイル.mp4" type="video/mp4">
</video>
```

- 動画ファイルは `slide.md` と同じディレクトリに置くか、相対パスで指定します。
- ローカルファイルを使う場合は `allowLocalFiles: true` が必要です（本リポジトリでは設定済み）。

### コードブロック

````markdown
```言語名
コード
```
````

### スライドにクラスを付ける（ディレクティブ）

そのスライドだけレイアウトやスタイルを変えたいときに使います。

```markdown
---
<!-- _class: multi-img-2 -->
## 2枚の画像

![a](a.png)
![b](b.png)
---
```

よく使うディレクティブの例:

| ディレクティブ | 意味 |
|----------------|------|
| `<!-- _class: クラス名 -->` | スライド（section）にクラスを付与 |
| `<!-- _backgroundColor: white -->` | 背景色 |
| `<!-- _paginate: true -->` | ページ番号を表示 |

### スタイルの読み込み

`<style>` 内で `@import` すると共通CSSを読み込めます。

```markdown
---
marp: true
---

<style>
@import url('../global.css');
</style>

# スライド本文
```

---

## global.css について

複数ディレクトリのスライドで共通して使うスタイルを **`global.css`** にまとめています。

### 読み込み方

各 `slide.md` のフロントマター直後に、**自分のディレクトリから見た相対パス**で読み込みます。

| slide.md の場所 | 記述例 |
|-----------------|--------|
| `marp/slide.md` | `@import url('global.css');` |
| `marp/track/slide.md` | `@import url('../global.css');` |
| `marp/○○/△△/slide.md` | `@import url('../../global.css');` |

例:

```markdown
---
marp: true
---

<style>
@import url('../global.css');
</style>
```

### 含まれるスタイル

- **1枚の画像**  
  - スライド内の `img` の高さを `420px` に統一（アスペクト比は維持）。

- **複数画像用クラス**  
  - 1スライドに複数画像を並べるときは、スライドにクラスを付けてレイアウトを指定します。

| クラス | 使い方 | レイアウト |
|--------|--------|------------|
| `multi-img-2` | `<!-- _class: multi-img-2 -->` | 2枚を横並び（高さ 320px） |
| `multi-img-3` | `<!-- _class: multi-img-3 -->` | 3枚を1行（高さ 260px） |
| `multi-img-4` | `<!-- _class: multi-img-4 -->` | 4枚を2×2グリッド（高さ 220px） |

複数画像スライドの例:

```markdown
---
<!-- _class: multi-img-4 -->
## 検出例

![trash](trash_box.png)
![human](human.png)
![connector](connector.png)
![cable](cable.png)
---
```

### スタイルを変えたいとき

- 画像の高さや余白を変えたい → **`global.css`** を編集すると、このファイルを読み込んでいるすべてのスライドに反映されます。
- 1スライドだけ変えたい → そのスライドの `<style>` で上書きするか、別の `_class` を付けて `global.css` にクラスを追加できます。

---

## ディレクトリ構成の例

```
marp/
├── README.md          # 本ドキュメント
├── global.css         # 共通スタイル
├── marp.json          # Marp CLI 設定（allowLocalFiles 等）
├── track/
│   └── slide.md       # @import url('../global.css')
└── profile/
    └── slide.md       # @import url('../global.css')
```

---

## プレビュー・エクスポート

### VS Code

[Marp for VS Code](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode) でプレビュー・エクスポートが可能です。

- **プレビュー**: エディタ右上のプレビューボタン、または `Ctrl+K V`（Mac: `Cmd+K V`）
- **エクスポート**: `Ctrl+Shift+P`（Mac: `Cmd+Shift+P`) → 「**Marp: Export Slide Deck**」を選択 → 形式（PDF / **PPTX** / HTML）を選んで保存

### Marp CLI

[@marp-team/marp-cli](https://github.com/marp-team/marp-cli) でコマンドから出力できます。ルートの `marp.json` が参照されます。

| 形式 | コマンド例 |
|------|------------|
| PDF | `marp track/slide.md --pdf -o slide.pdf` |
| **PPTX**（スライドが画像として出力） | `marp track/slide.md --pptx -o slide.pptx` |
| **PPTX（編集可能）** | `marp track/slide.md --pptx-editable -o slide.pptx` |

- **PPTX（通常）**: `--pptx` で出力。スライドは画像として埋め込まれ、PowerPoint 上での文字編集はできません。
- **PPTX（編集可能）**: `--pptx-editable` を使うと、テキストやレイアウトを PowerPoint で編集できる形式で出力できます。**LibreOffice のインストールが必要**です（[Marp CLI のドキュメント](https://github.com/marp-team/marp-cli#editable-pptx-export)参照）。

ローカル画像を含むスライドを出力する場合は、`marp.json` で `allowLocalFiles: true` を設定してください（本リポジトリでは設定済み）。
# marp
