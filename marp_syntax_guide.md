# Marp エコシステム拡張構文ガイド

このドキュメントは、Marp (Marpit / Marp Core) を使用してスライドを作成する際に利用可能な拡張構文をまとめたものです。AIアシスタントにスライド作成を指示する際のコンテキストとして利用してください。

## 1. 基本構造

### ページ区切り
スライドのページは `---` (水平線) で区切ります。

```markdown
# Slide 1內容

---

# Slide 2内容
```

### ディレクティブ (Directives)
スライド全体または特定のページの設定を制御するための拡張構文です。

#### 定義方法

**1. YAML フロントマター (全体設定)**
ファイルの先頭に記述します。スライド全体に適用されます。

```markdown
---
marp: true
theme: default
paginate: true
---
```

**2. HTML コメント (ローカル設定)**
スライドの途中で設定を変更する場合に使用します。記述したページ以降に適用されます。

```markdown
<!-- paginate: false -->
```

#### 「スポット」ディレクティブ (`_`)
ディレクティブ名の前に `_` を付けると、**そのページのみ**に適用されます。
例: `_class: lead` (このページだけ lead クラスを適用)

---

## 2. 利用可能なディレクティブ一覧

### グローバルディレクティブ (全体設定)
通常は YAML フロントマターで定義します。

| ディレクティブ | 説明 | 例 |
| :--- | :--- | :--- |
| `marp` | `true` に設定すると Marp 互換のエディタ(VS Code等)でプレビューが有効になります。必須推奨。 | `marp: true` |
| `theme` | スライドのテーマ (`default`, `gaia`, `uncover` 等)。 | `theme: gaia` |
| `size` | アスペクト比 (`16:9`, `4:3`)。 | `size: 4:3` |
| `headingDivider` | 指定したレベルの見出しで自動的にページを分割します。 | `headingDivider: 2` |
| `style` | カスタム CSS を定義します。 | `style: "section { color: blue; }"` |

### ローカルディレクティブ (ページ設定)
HTML コメント形式、またはフロントマターで初期値を設定できます。

| ディレクティブ | 説明 | 例 |
| :--- | :--- | :--- |
| `paginate` | ページ番号の表示/非表示 (`true` / `false`)。 | `paginate: true` |
| `header` | ヘッダーに表示するテキスト。 | `header: "Marp Guide"` |
| `footer` | フッターに表示するテキスト。 | `footer: "Page %"` |
| `class` | スライド (`<section>`) に HTML クラスを適用します。 | `class: lead` |
| `backgroundColor` | 背景色を指定します。 | `backgroundColor: #ccc` |
| `color` | テキスト色を指定します。 | `color: black` |
| `backgroundImage` | 背景画像を指定します (CSS の `background-image` と同じ)。 | `backgroundImage: url('img.png')` |

---

## 3. 画像記法 (Image Syntax)

標準の Markdown 画像記法 `![](url)` を拡張しています。`[]` 内にキーワードを指定します。

### リサイズ
- `width` (`w`) と `height` (`h`) を指定可能。
- `![]` (インライン画像) と `![bg]` (背景画像) の両方で使用可能。

```markdown
![width:200px](image.png)
![w:100 h:100](image.png)
```

### 画像フィルター (CSS Filters)
画像の明るさやぼかしなどを調整できます。

```markdown
![blur:10px](image.png)
![brightness:1.5](image.png)
![grayscale:1](image.png)
```

### 背景画像 (`bg`)
スライドの背景として画像を設定します。キーワード `bg` を使用します。

```markdown
![bg](background.jpg)
```

**サイズ調整キーワード:**
- `cover` (デフォルト): 画面いっぱいに表示。
- `contain` / `fit`: 画像全体が収まるように表示。
- `auto`: 元のサイズ。

```markdown
![bg contain](background.jpg)
```

### 分割背景 (Split Backgrounds)
画面を左右に分割して背景画像を表示します。`left` または `right` を指定します。
テキストは反対側のスペースに配置されます。

```markdown
<!-- 基本的な分割 (50%) -->
![bg left](image.jpg)

<!-- 比率を指定 (33%) -->
![bg right:33%](image.jpg)
```

### 複数背景
複数の `![bg]` を並べると、画像を並べて表示できます（実験的機能）。

```markdown
![bg left](img1.jpg)
![bg left](img2.jpg)
```

---

## 4. その他拡張機能

### 断片化リスト (Fragmented List)
プレゼンテーション時にクリックで順番に表示されるリストです。
通常の箇条書き (`-` や `+`) の代わりに、アスタリスク `*` を使用します。

```markdown
* 最初に表示される項目
* 次に表示される項目
* 最後に表示される項目
```

### 数式 (Math Typesetting)
Marp Core は KaTeX をサポートしています。
`$$` で囲むとブロック数式、`$` で囲むとインライン数式になります。

```markdown
$$
ax^2 + bx + c = 0
$$
```

## 5. テーマ固有のクラス (Built-in Themes)

Marp Core に標準搭載されているテーマ (`default`, `gaia`, `uncover`) では、以下のクラスがよく使用されます。

- **`lead`**: タイトルスライド用。テキストを中央寄せし、フォントサイズを大きくします。
  ```markdown
  <!-- _class: lead -->
  # タイトル
  ```
- **`invert`**: 背景色と文字色を反転させます（ダークモード風）。
  ```markdown
  <!-- _class: invert -->
  # 反転スライド
  ```

---

## AI アシスタントへの指示例

Marp 形式のスライドを作成させる際は、以下のフォーマットを参考に指示してください。

> 「以下の情報を元に、Marp 形式の Markdown でスライド資料を作成してください。
>  テーマは `gaia` を使用し、タイトルスライドには `lead` クラスを適用してください。
>  画像は背景画像 (`bg`) として配置し、適宜フィルターを使用してください。」
