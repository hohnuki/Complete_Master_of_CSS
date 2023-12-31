# CSS設計完全ガイド

**Chapter1 CSSの歴史と問題点**
---

CSSはCascading Style Sheetsの略。<br>
CSSはHTMLのstyle属性を分離したもの。<br>

CSSはすべてがグローバルスコープ。<br>
⇒ ページ数が増えると複雑になり、管理が大変になる。<br>
⇒ <u>分離と抽象化でCSS設計することで乗り越えよう（本書のテーマ）</u><br>

Atomic Designを学習することは抽象思考の観点からおすすめ。

**Chapter2 CSS設計の基本と実践**
---

CSSはカスケーディングという仕組みがある。セレクターにが示す同じ要素の同じプロパティに異なる値が設定されていた時にどれを適応するかの基準。<br>
1.重要度
2.詳細度
3.コードの順序<br>
※詳細度は別途学習したほうがよさそう。

リセットCSSにはハードリセット系CSSとノーマライズ系CSSがある。


よいCSS設計とは。
1. 予測できる(スタイルの影響範囲や期待通りの振る舞いをするか)
2. 再利用できる(スタイルの抽象化)
3. 保守できる(スタイル同士が疎結合)
4. 拡張できる

のポイントを押さえたもの。

また実際にコードを書くときには以下の8つのポイントが大事。
1. 特徴に応じてCSSを分類する
2. コンテンツとスタイリングが疎結合である
3. 影響範囲がみだりに広すぎない
4. 特定のコンテキストにみだりに依存しない
5. 詳細度がみだりに高くない
6. クラス名から影響範囲が想像できる
7. クラス名から見た目・機能・役割が想像できる
8. 拡張しやすい

この後紹介するCSS設計手法のOOCSS/SMACSS/BEM/PRECSSにある規則もこの8つのどれかには該当する。

例えば、<br>
1でベースグループ（サイト全体）・レイアウトグループ・モジュールグループ（汎用物）に分類する。<br>
2ではセレクターの使用を避けHTMLとスタイルを疎結合に。属性セレクターの特定の値もアンチパターン。<br>
3ではスタイル適用は親要素までのセレクターまでにするなど影響範囲を広げすぎないよう注意する。<br>
4はちょっとよくわかんない。<br>
5では詳細度が無駄に高くならないようクラスセレクターを利用することを推奨する。<br>
6ではCSSとはいえ機能や影響範囲にあった命名に気を付ける。クラス名の先頭にモジュール名を付けるなど。<br>
7はtitle1ではなく、page-titleみたいに具体的な命名にする。⇚ 見た目・機能・役割が想像しやすいように。<br>
8はシングルクラスとマルチクラスの特性をみつつ設計するなど。HTMLとCSSのシンプルさのトレードオフ。<br>

モジュールの粒度はばらつきが起きやすい。以下が一般的なモジュールの粒度らしい。Chapter5と6で各々紹介する。
1. 最小モジュール ~ ボタンやラベルなどのシンプルな要素
2. 複合モジュール ~ いくつかの子要素をもつひとかたまりの要素

CSS設計が必要なのは100ページほどあるWebサイトの案件とかで強く出てくる。 ⇒ 1ページのWebサイトではぶっちゃけ不要。<br>
⇒ いつかの大規模プロジェクトのためにいまからCSS設計を頑張ろう。


**Chapter3 様々な設計手法**
---
**OOCSS**

OOCSS = Object Oriented CSS（オブジェクト指向CSS）<br>
Nicole Sullivan氏によって提唱された。<br>
「レゴのように自由に組み合わせが可能なモジュールの集まりをつくろう」や「新規ページを作成するときに追加のCSSは不要」がモットー。<br>

レゴのようなモジュールを実現するための具体案が以下の２つである。
- ストラクチャーとスキンの分離<br>
<u>関連ポイント → 8.拡張しやすい</u><br>
ボタンを例にすると、横幅や高さなどの構造（**ストラクチャー**）とボックスシャドウや背景色などのあしらい（**スキン**）に分類し、分離する。<br>
ストラクチャーはスタイルをまとめて、スキンは個別のスタイルでなどの運用方法がある。
ex.<br>
ストラクチャーにあたるプロパティ → width, height, padding, margin<br>
スキンにあたるプロパティ → color, font, bachground, box-shadow, text-shadow<br>

- コンテナとコンテンツの分離<br>
<u>関連ポイント ⇒ 特定のコンテキストにみだりに依存していない</u><br>
モジュールをなるべく特定のエリアに依存させないこと。<br>
ex. #main.btnみたいなセレクターではなく、.btnみたいなクラスセレクターにする<br>

OOCSSはかなり古く、明確に規則と呼べるものも少ない。あくまで「ひとつの真理」（なんそれ）として扱う。<br>

---
**SMACSS**

SMACSS（スマックス） = Scalable and Modular Architecture for CSS（拡張可能でモジュール的なCSS設計）<br>
Jonathan Snook氏によって提唱された。<br>
CSSのコードを役割に応じてカテゴリ分けしており、以下の5カテゴリについてそれぞれの規則が設けられている。
1. ベース
2. レイアウト
3. モジュール
4. 状態（ステート）
5. テーマ

- ベースルール<br>
<u>関連ポイント → 特性に応じてCSSを分類する</u><br>
「プロジェクトにおいて各要素が標準としてどのように振る舞うか」を定義する → 自分で定義するデフォルトCSSに近い。<br>
bodyの背景色をベースルールで設定することを強く推奨してる。<br>
ex. bodyセレクター、a > imgセレクター、ul liセレクター など

- レイアウトルール<br>
<u>関連ポイント → 特性に応じてCSSを分類する</u><br>
ヘッダーやメインエリア、サイドバー、フッターなどのWebサイトの大枠を構成する大きなモジュールに対するルール。<br>
#header, #main, #footer などページ内で一度しか使用されないものは**IDセレクター**を許容し、.sectionなど複数回使用するものは**クラスセレクター**で記述する。<br>
特定の状況でレイアウトを変更する場合は、.l-narrow #mainなどの子孫セレクターによる上書きを許容している。<br>
レイアウトルールにおいて、クラスセレクターを使用する際は`「l-」`の接頭辞を付けることを推奨している<br>

- モジュールルール<br>
モジュールルールに該当するのは、レイアウトモジュール内に配置されるもの。見出し・ボタンなどが例。<br>
ここは別ページにそのまま配置しても崩れたりしないような、再利用性が求めれらる。<br>
	- なるべく要素型のセレクターを使用しない<br>
	<u>関連ポイント ⇒ 2.HTMLとスタイリングが疎結合である</u><br>
	セマンティックな要素のみ要素型セレクター、そのほかはクラスセレクターを使用する。<br>
	なんやかんやクラスセレクターが安パイ。<br>
		- 要素をセマンティックにする
		SMACSSのセマンティック性の優劣は、div/span要素などの汎用要素 < 見出しなどの意味をもつ要素 < クラス属性がついた要素 <br>
		- 要素型セレクターを使用する際は子セレクターを使用する<br>
		<u>関連ポイント ⇒ 3.影響範囲がみだりに広すぎない</u><br>
		SMACSSでは要素型セレクターを使用するときは、子孫セレクターではなく子セレクターを推奨している。<br>
	- スタイルを上書きするためのサブクラス<br>
	<u>関連ポイント ⇒ 3.影響範囲がみだりに広すぎない</u><br>
	<u>関連ポイント ⇒ 8.拡張しやすい</u><br>
	モジュールに変化がある場合はサブクラスを利用する。ボタンの例が以下。<br>
	`.l-heder .btn:nth-of-type(2)`ではなく、`.btn.btn-small`や`.btn.btn-long`のようなセレクターにする<br>
	HTMLではクラス属性にサブクラスを付与する。今回ならclass="btn btn-small"となる ⇚ 子のセレクターはbtnとbtn-smallをクラスに持つ要素に効く。<br>

- ステートルール<br>
<u>関連ポイント ⇒ 1.特性に応じてCSSを分類する</u><br>
<u>関連ポイント ⇒ 8.拡張しやすい</u><br>
`「is-」`の接頭辞がつき、レイアウトやモジュールに割り当てる。状態スタイルはJavaScriptに依存する。<br>
エラー時や選択時などの挙動を設定するセレクターを指す。⇒ is-error, is-active, is-hoverなど<br>
`is-active > a` はis-activeクラスを持つ要素の子要素のa要素に効く。

- テーマルール<br>
ダークテーマやホワイトテーマなどの設定をする際のルール。`「theme-」`を接頭辞に着けることを推奨。<br>

SMACSSは網羅性があるし、厳格過ぎないCSS設計 → ある程度の柔軟性を持ちたいときにおすすめ。<br>

---
**BEM**

BEM（ベム）= Block, Element, Modifier<br>
Yandex社によって提唱された。<br>
UIを独立したブロックに落とし込んでおくことで、複雑なページも簡単・迅速に開発できちゃうよ。っていうCSS設計手法。<br>
OOCSSのようにモジュールをベースにした方法論だが、他の設計手法と比べ厳格・強力である。世界的にもOOCSSに匹敵するほどに有名。<br>
モジュールを`Block, Element, Modifier`という単位で分割して定義する。⇚ この3つを`BEMエンティティ`という。<br>

BEMにおいてはCSSスタイリングで要素型セレクター・IDセレクターは非推奨。⇚ 詳細度を統一する観点から。<br>
BEMエンティティにおいて、クラス名は半角英数字のハイフンつなぎで宣言する。ex. Global nav ⇒ global-nav<br>

- Blockの基本<br>
<u>関連ポイント ⇒ 7.クラス名から見た目・機能・役割を想像できる</u><br>
「特定のコンテキスト(実行環境や状態)に依存していないどこでも使いまわせるパーツ」が定義である。<br>
スケーラビリティを重視するのでレイアウトに関するスタイリングは行わない。marginとかfloatとか。<br>
Blockの命名規則は単語が1つなら`block`、単語が2つなら`block-name`のようにする。<br>

- Elementの基本<br>
<u>関連ポイント ⇒ 6.クラス名から影響範囲が想像できる</u><br>
<u>関連ポイント ⇒ 7.クラス名から見た目・機能・役割を想像できる</u><br>
定義は「Blockを構成し、Blockの外では独立して使用できないもの」である。Blockの構成要素やね。<br>
Elementの命名規則はBlock名を継承して、単語が1つなら`block__name`、単語が2つなら`block-name__element-name`のようにする。<br>
ネストされた命名規則（X : menu_btn_icon, O : menu_icon）はアンチパターン。⇚ ネストされたElementが悪いわけではない。<br>

- Modifierの基本<br>
<u>関連ポイント ⇒ 7.クラス名から見た目・機能・役割を想像できる</u><br>
<u>関連ポイント ⇒ 8.拡張しやすい</u><br>
定義は「BlockもしくはElementの見た目や状態、振る舞いを定義するもの」。かならずBlockかElementのクラス名がある状態で、2つ目以降のクラス名としてModifierを付けます。<br>
Modifierの命名規則はBlock名またはElement名を継承して、`block_element_modifier`や`block-name_element-name_modifier-name`のようにする。⇚ key:valueのときはアンスコ使うから注意。<br>

- Mix<br>
<u>関連ポイント ⇒ 4.特定のコンテキストにみたりに依存していない</u><br>
<u>関連ポイント ⇒ 5.詳細度がみだりに高くない</u><br>
定義は「1つHTML要素に役割の異なる複数のクラスがついている状態」らしい。<br>
Mixよくわかんない。<br>

BEMは規則の多さとドキュメントの長さが信頼の証！！！<br>

---
**PRECSS**

PRECSS（プレックス） = prefixed CSS<br>
筆者が開発したらしい.......。<br>
CSSのコードを役割に応じてカテゴリ分けしており、以下の6カテゴリについてそれぞれの規則が設けられている。
1. ベース
2. レイアウト
3. モジュール
	1. ブロックモジュール
	2. エレメントモジュール
4. ヘルパー
5. ユニーク
6. プログラム

基本的な命名規則は `_` は階層を表現する。ハイフンは使わずキャメルケースにする。各モジュールの子要素はネストした場合も`親の名前のみ`を継承する<br>
_wrapper, _inner, _header, _body, _footer が汎用的に使用可能な単語。mainVisula = MV のように単語の省略を推奨している。<br>

- ベースグループ<br>
接頭辞：`なし`<br>
リセットCSSなどプロジェクトにおいて標準となるスタイリングを行う。

- レイアウトグループ<br>
接頭辞：`ly_` （layoutの略）<br>
ヘッダー、ボディエリア、ボディエリア、フッターなどの大きなレイアウトを形成する要素に使用。<br>
あくまでコンテンツが入る枠を定義するだけで、コンテンツは後述するモジュールグループで作成する。<br>

- モジュールグループ<br>
再利用性の高いコードをモジュールと呼び、モジュールは大きさによって以下の2つに分類される。<br>
ブロックのほうが大きい、エレメントのほうが小さい。<br>
	- ブロックモジュール<br>
	接頭辞：`bl_`（blockの略）<br>
	よくわからん。<br>

	- エレメントモジュール<br>
	接頭辞：`el_`（elementの略）<br>
	ボタンやラベルなどの最小単位のモジュール。極力汎用的な命名を心掛ける。<br>
	パターンをつけたいときは`__`をつける。⇚ BEMのModifierと同じ。ちなみにModifierはkeyValue形式で書く<br>

- ヘルパーグループ<br>
接頭辞：`hp_`（helperの略）<br>
基本的にひとつのスタイルのみで、「ここのスタイルだけ調整したい！」というときに使用する<br>

- ユニークグループ<br>
接頭辞：`un_`（uniqueの略）<br>
ある特定の箇所でしか利用していないときに目印となるスタイル。あらゆるイレギュラー回避のための万能策<br>

- プログラムグループ<br>
接頭辞：`js_`（JavaScriptの略）と`is_`（be動詞のisから）<br
JavaScriptで要素を取得するときはjs_で、要素の状態を管理するときはjs_を使用する。<br>

**Chapter4 レイアウトの設計**
---

BEM/PRECSSにおけるレイアウト設計のベストプラクティスは「レイアウトに関することは、レイアウト用のクラスに任せる」こと。<br>
divタグの閉じタグの後にコメント付ける習慣大事。<br>

**Chapter5 最小モジュール**
---

**Chapter6 複合モジュール**
---

**Chapter7 モジュールの再利用**
---

**Chapter8 CSS設計をより生かすためのスタイルガイド**
---

**Chapter9 CSS開発に役立つその他の技術**
---
 
 