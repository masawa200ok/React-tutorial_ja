# React Tutorial 和訳

かなり自分に都合よく意訳していますので一部参考ということで．
雰囲気はつかめると思います．

原典：
[http://facebook.github.io/react/docs/tutorial.html](http://facebook.github.io/react/docs/tutorial.html)

2014-05-01時点．

## Tutorial

We'll be building a simple but realistic comments box that you can drop into a blog, a basic version of the realtime comments offered by Disqus, LiveFyre or Facebook comments.

We'll provide:

- A view of all of the comments
- A form to submit a comment
- Hooks for you to provide a custom backend

It'll also have a few neat features:

- Optimistic commenting: comments appear in the list before they're saved on the server so it feels fast.
- Live updates: other users' comments are popped into the comment view in real time. 
- Markdown formatting: users can use Markdown to format their text.

### （訳）チュートリアル

簡単で実用的なブログのコメントシステムを作成していきましょう．Disqus，LiveFyreやFacebookのリアルタイムコメントの基本的なシステムです．

作成するもの：

- 全コメントのビュー
- コメント投稿フォーム
- お望みのバックエンドのコメントフック

他にも実装するいくつかの機能：

- サクサクとコメント投稿：コメントはサーバにセーブする前にリストしちゃいます．
- ライブ・アップデート：他のユーザがコメントを投稿したらリアルタイムでリストされます．
- マークダウン記法対応：ユーザはマークダウン記法を使用できます．


### （訳）調べた単語

- Optimistic  
  楽観的な．

===

## Running a server

While it's not necessary to get started with this tutorial, later on we'll be adding functionality that requires POSTing to a running server. If this is something you are intimately familiar with and want to create your own server, please do. For the rest of you who might want to focus on learning about React without having to worry about the server-side aspects, we have written simple servers in a number of languages - JavaScript (using Node.js), Python, Ruby, Go, and PHP. These are all available on GitHub. You can view the source or download a zip file to get started.

To get started using the tutorial, just start editing public/index.html.


### （訳）サーバの起動

チュートリアルを始める上では必須ではないですが，いつかはサーバにPOSTする機能を追加することでしょう．もしサーバサイドに精通していて，サーバサイドをつくりたいというのであれば，ぜひ作成してください！サーバーサイドの作成に時間を割かず，Reactのみにフォーカスして学びたいという方には，私たちがシンプルなサーバプログラムを用意しました．それらはいくつかの言語で書かれていて，JavaScript（Node.js），Python，Ruby，GoやPHPのものがあります．それらはGitHubにあります．ソースコードが見れるし，zipファイルとしてダウンロードし，すぐ始めることができます．

チュートリアルを開始します，public/index.htmlをエディタで書き始めましょう！


### 調べた単語

- intimately  
  密接に
- familiar with  
  精通している


## Getting started

For this tutorial, we'll use prebuilt JavaScript files on a CDN. Open up your favorite editor and create a new HTML document:

```
<!-- index.html -->
<html>
  <head>
    <title>Hello React</title>
    <script src="https://fb.me/react-0.13.2.js"></script>
    <script src="https://fb.me/JSXTransformer-0.13.2.js"></script>
    <script src="https://code.jquery.com/jquery-2.1.3.min.js"></script>
  </head>
  <body>
    <div id="content"></div>
    <script type="text/jsx">
      // Your code here
    </script>
  </body>
</html>
```

For the remainder of this tutorial, we'll be writing our JavaScript code in this script tag.

Note:
We included jQuery here because we want to simplify the code of our future ajax calls, but it's NOT mandatory for React to work.


### （訳）はじめましょう！

このチュートリアルでは，CDNにアップロードされている，ビルド済みJavaScriptライブラリを使います．お気に入りのエディタを起動して，新しいHTMLドキュメントを書きましょう：

```
<!-- index.html -->
<html>
  <head>
    <title>Hello React</title>
    <script src="https://fb.me/react-0.13.2.js"></script>
    <script src="https://fb.me/JSXTransformer-0.13.2.js"></script>
    <script src="https://code.jquery.com/jquery-2.1.3.min.js"></script>
  </head>
  <body>
    <div id="content"></div>
    <script type="text/jsx">
      // Your code here
    </script>
  </body>
</html>
```

これ以降は，このscriptタグ（Your code here）の中にJavaScriptコードを書いていきます．

Note：
jQueryをインクルードしてますが，それはAjax処理のコード部分を簡単にしたいためです．Reactが動作するためには必要ではありません．

### 調べた単語

- mandatory  
  義務的な

## Your first component

React is all about modular, composable components. For our comment box example, we'll have the following component structure:

```
- CommentBox
  - CommentList
    - Comment
  - CommentForm
```

Let's build the CommentBox component, which is just a simple `<div>`:

```
// tutorial1.js
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        Hello, world! I am a CommentBox.
      </div>
    );
  }
});
React.render(
  <CommentBox />,
  document.getElementById('content')
);
```

### （訳）最初のコンポーネント

Reactはほぼ完全に，組み立て方式であり，構成可能なコンポーネントとなります．作成するCommentBoxの例では次のようなコンポーネントの構造を作ります：

```
- CommentBox
  - CommentList
    - Comment
  - CommentForm
```

では，CommentBoxコンポーネントを作成しましょう．とてもシンプルな`<div>`コンポーネントですね．

```
// tutorial1.js
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        Hello, world! I am a CommentBox.
      </div>
    );
  }
});
React.render(
  <CommentBox />,
  document.getElementById('content')
);
```

### 調べた単語

- modular  
  組み立て式の
  

## JSX Syntax

The first thing you'll notice is the XML-ish syntax in your JavaScript. We have a simple precompiler that translates the syntactic sugar to this plain JavaScript:

```
// tutorial1-raw.js
var CommentBox = React.createClass({displayName: 'CommentBox',
  render: function() {
    return (
      React.createElement('div', {className: "commentBox"},
        "Hello, world! I am a CommentBox."
      )
    );
  }
});
React.render(
  React.createElement(CommentBox, null),
  document.getElementById('content')
);
```

Its use is optional but we've found JSX syntax easier to use than plain JavaScript. Read more on the JSX Syntax article.

### （訳）JSX記法

最初に注視すべきことは，JavaScriptの中にXMLっぽい記法があることでしょう．Reactには，以下のプレーンJavaScriptへ変換するプリコンパイラがあるのです．

```
// tutorial1-raw.js
var CommentBox = React.createClass({displayName: 'CommentBox',
  render: function() {
    return (
      React.createElement('div', {className: "commentBox"},
        "Hello, world! I am a CommentBox."
      )
    );
  }
});
React.render(
  React.createElement(CommentBox, null),
  document.getElementById('content')
);
```

プリコンパイラを使用することはオプションではありますが，プレーンJavaScriptよりも簡単に使えることがわかるかと思います．もっと知りたくなったならJSX記法の記事も読んでください．


## （訳）What's going on

We pass some methods in a JavaScript object to React.createClass() to create a new React component. The most important of these methods is called render which returns a tree of React components that will eventually render to HTML.

The `<div>` tags are not actual DOM nodes; they are instantiations of React div components. You can think of these as markers or pieces of data that React knows how to handle. React is safe. We are not generating HTML strings so XSS protection is the default.

You do not have to return basic HTML. You can return a tree of components that you (or someone else) built. This is what makes React composable: a key tenet of maintainable frontends.

React.render() instantiates the root component, starts the framework, and injects the markup into a raw DOM element, provided as the second argument.

### （訳）何が起こっているのでしょうか

新しいReactコンポーネントを生成するために，いくつかのメソッドを生やしたJavaScriptオブジェクトを，React.createClass()に渡します．メソッドの中で最も重要なのは，イベントが発生した時にHTMLを生成するReactコンポーネントのツリーをreturnするrenderメソッドがcallされていることです．

`<div>`タグがありますがこれはいわゆるDOMではありません；これらはReactのdivコンポーネントツリーのインスタンスです．これらは，Reactがデータをどのように扱うかのマーカーや情報だと思ってください．Reactは**安全**なのです．決してHTML文字列を生成しないのでXSSプロテクトが施されているのです．

一般的なHTMLを返す必要はありません．あなたが（もしくは誰かが）構築したコンポーネントツリーを返すべきです．このことがReactを**コンポーサブル（構築しやすく）**しています．これはメンテナンスしやすいフロントエンドのための重要な指針です．

React.render()はルートコンポーネントを生成し，配下のコンポーネント構築も行います，そして２番めの引数である生DOMにマークアップ（HTML）を注入します．

## Composing components

Let's build skeletons for CommentList and CommentForm which will, again, be simple `<div>`s:

```
// tutorial2.js
var CommentList = React.createClass({
  render: function() {
    return (
      <div className="commentList">
        Hello, world! I am a CommentList.
      </div>
    );
  }
});

var CommentForm = React.createClass({
  render: function() {
    return (
      <div className="commentForm">
        Hello, world! I am a CommentForm.
      </div>
    );
  }
});
```

Next, update the CommentBox component to use these new components:

```
// tutorial3.js
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList />
        <CommentForm />
      </div>
    );
  }
});
```

Notice how we're mixing HTML tags and components we've built. HTML components are regular React components, just like the ones you define, with one difference. The JSX compiler will automatically rewrite HTML tags to React.createElement(tagName) expressions and leave everything else alone. This is to prevent the pollution of the global namespace.

### （訳）コンポーネントを組み立てる

では，再びシンプルな`<div>`で，CommentListとCommentFormのスケルトンを作りましょう：

```
// tutorial2.js
var CommentList = React.createClass({
  render: function() {
    return (
      <div className="commentList">
        Hello, world! I am a CommentList.
      </div>
    );
  }
});

var CommentForm = React.createClass({
  render: function() {
    return (
      <div className="commentForm">
        Hello, world! I am a CommentForm.
      </div>
    );
  }
});
```

次に，新しいコンポーネント（CommentList，CommentForm）を使用するようCommentBoxコンポーネントを編集しましょう：

```
// tutorial3.js
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList />
        <CommentForm />
      </div>
    );
  }
});
```

注目すべきはどのようにHTMLと私たちが作成したコンポーネントをミックスしているか，というところです．HTMLのコンポーネントは，あなたが作成したコンポーネントとおなじように，正式なReactコンポーネントです（ひとつ違いがありますが）．JSXコンパイラは自動的にHTMLタグをReact.createElement(tagName)というように書き直し，それぞれを個別にします．この機能はグローバル名前空間の汚染を防ぐためです．

### 調べた単語

- pollution  
  汚染


## Using props

Let's create the Comment component, which will depend on data passed in from its parent. Data passed in from a parent component is available as a 'property' on the child component. These 'properties' are accessed through this.props. Using props we will be able to read the data passed to the Comment from the CommentList, and render some markup:

```
// tutorial4.js
var Comment = React.createClass({
  render: function() {
    return (
      <div className="comment">
        <h2 className="commentAuthor">
          {this.props.author}
        </h2>
        {this.props.children}
      </div>
    );
  }
});
```

By surrounding a JavaScript expression in braces inside JSX (as either an attribute or child), you can drop text or React components into the tree. We access named attributes passed to the component as keys on this.props and any nested elements as this.props.children.

### （訳）propsを使う

それではCommentコンポーネントを作りましょう．親コンポーネントから渡されるデータに依存します．
親コンポーネントから渡されるデータは，子コンポーネントでは'プロパティ'として利用できます．これらのプロパティ群には，this.propsを通じてアクセスします．CommentListコンポーネントからCommentコンポーネントへ渡されるデータを読むことができるpropsを使うには，マークアップをrenderする時に渡します．

```
// tutorial4.js
var Comment = React.createClass({
  render: function() {
    return (
      <div className="comment">
        <h2 className="commentAuthor">
          {this.props.author}
        </h2>
        {this.props.children}
      </div>
    );
  }
});
```

JSXの中でJavaScriptを使うには（attributeか子コンポーネントとして）ブレース（{}）で囲みます．そのことによってテキストやReactコンポーネントをツリーへ追加できます．this.propsのキーとしてコンポーネントに渡されるattributeのキーを使うか，もしくは，this.props.childrenとしてネストされた要素を使い，アクセスします．


## Component Properties

Now that we have defined the Comment component, we will want to pass it the author name and comment text. This allows us to reuse the same code for each unique comment. Now let's add some comments within our CommentList:

```
// tutorial5.js
var CommentList = React.createClass({
  render: function() {
    return (
      <div className="commentList">
        <Comment author="Pete Hunt">This is one comment</Comment>
        <Comment author="Jordan Walke">This is *another* comment</Comment>
      </div>
    );
  }
});
```

Note that we have passed some data from the parent CommentList component to the child Comment components. For example, we passed Pete Hunt (via an attribute) and This is one comment (via an XML-like child node) to the first Comment. As noted above, the Comment component will access these 'properties' through this.props.author, and this.props.children.

### （訳）コンポーネントのプロパティ

さてCommentコンポーネントを定義したところで，コメントの投稿者とコメント・テキストを渡したいところでしょう．複数のコメントに対して同一コードを使用できます．では，CommentListコンポーネントにいくつかコメントを追加してみよう．

```
// tutorial5.js
var CommentList = React.createClass({
  render: function() {
    return (
      <div className="commentList">
        <Comment author="Pete Hunt">This is one comment</Comment>
        <Comment author="Jordan Walke">This is *another* comment</Comment>
      </div>
    );
  }
});
```

親コンポーネントのCommentListから子コンポーネントのCommentへ幾つかデータを渡しました．'Pete Hunt'（attributeとして）とコメント・テキストを（XMLライクな子ノードとして）を最初のCommentに渡しています．上記のようにして，Commentコンポーネントがthis.props.authorとthis.props.childrenを使って，これらの'プロパティ'にアクセスしています．

## Adding Markdown

Markdown is a simple way to format your text inline. For example, surrounding text with asterisks will make it emphasized.

First, add the third-party library marked to your application. This is a JavaScript library which takes Markdown text and converts it to raw HTML. This requires a script tag in your head (which we have already included in the React playground):

```
<!-- index.html -->
<head>
  <title>Hello React</title>
  <script src="https://fb.me/react-0.13.2.js"></script>
  <script src="https://fb.me/JSXTransformer-0.13.2.js"></script>
  <script src="https://code.jquery.com/jquery-1.10.0.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/marked/0.3.2/marked.min.js"></script>
</head>
```

Next, let's convert the comment text to Markdown and output it:

```
// tutorial6.js
var Comment = React.createClass({
  render: function() {
    return (
      <div className="comment">
        <h2 className="commentAuthor">
          {this.props.author}
        </h2>
        {marked(this.props.children.toString())}
      </div>
    );
  }
});
```

All we're doing here is calling the marked library. We need to convert this.props.children from React's wrapped text to a raw string that marked will understand so we explicitly call toString().

But there's a problem! Our rendered comments look like this in the browser: "`<p>`This is `<em>`another`</em>` comment`</p>`". We want those tags to actually render as HTML.

That's React protecting you from an XSS attack. There's a way to get around it but the framework warns you not to use it:

```
// tutorial7.js
var Comment = React.createClass({
  render: function() {
    var rawMarkup = marked(this.props.children.toString(), {sanitize: true});
    
return (
      <div className="comment">
        <h2 className="commentAuthor">
          {this.props.author}
        </h2>
        <span dangerouslySetInnerHTML={{__html: rawMarkup}} />
      </div>
    );
  }
});
```

This is a special API that intentionally makes it difficult to insert raw HTML, but for marked we'll take advantage of this backdoor.

Remember: by using this feature you're relying on marked to be secure. In this case, we pass sanitize: true which tells marked to escape any HTML markup in the source instead of passing it through unchanged.


### （和訳）Markdown機能を追加する

Markdownはインライン・テキストを整形するのに簡単な機能です．例えばアスタリスクで囲んだテキストは強調されます．

まず，サードパーティ製*marked*ライブラリを今回のアプリケーションに追加しましょう．これはMarkdownテキストを生のHTMLへコンバートするJavaScriptライブラリです．headタグにスクリプトタグを追加してください（React Playground）にはすでに追加されています．

```
<!-- index.html -->
<head>
  <title>Hello React</title>
  <script src="https://fb.me/react-0.13.2.js"></script>
  <script src="https://fb.me/JSXTransformer-0.13.2.js"></script>
  <script src="https://code.jquery.com/jquery-2.1.3.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/marked/0.3.2/marked.min.js"></script>
</head>
```

次に，コメントテキストをMarkdownでフォーマットし，出力してみましょう．

```
// tutorial6.js
var Comment = React.createClass({
  render: function() {
    return (
      <div className="comment">
        <h2 className="commentAuthor">
          {this.props.author}
        </h2>
        {marked(this.props.children.toString())}
      </div>
    );
  }
});
```

ここでしていることはMarkdownライブラリを呼んでいるだけです．ここではthis.props.childrenをReactテキストから生のテキストにコンバートして，markedライブラリが解析できるように明示的にtoString()を呼んでいます．

しかし，ここでひとつ問題があります！いまレンダリングしたコメントはブラウザでは次のように表示されます："`<p>`This is `<em>`another`</em>` comment`</p>`"．今の場合そのタグは実際にHTMLとしてレンダリングしたいはずです．

このことはReactはXSS（クロス・サイト・スクリプション）攻撃から守ってくれていることを表しています．Reactフレームワークは使わないよう警告しますが，実現するのに別の方法があります：

```
// tutorial7.js
var Comment = React.createClass({
  render: function() {
    var rawMarkup = marked(this.props.children.toString(), {sanitize: true});
    
return (
      <div className="comment">
        <h2 className="commentAuthor">
          {this.props.author}
        </h2>
        <span dangerouslySetInnerHTML={{__html: rawMarkup}} />
      </div>
    );
  }
});
```

これは生HTMLを挿入することを意図的に難しくする特別なAPIです．しかしmarkedにはバックドアに対して有効でしょう．

*覚えておいてください*：この機能を使うことによって安全にmarkedを使用できるでしょう．このサンプルでは， marked に sanitize: true を渡して，そのまま渡すのではなく，HTMLマークアップにするよう知らせているのです．


### 調べた単語

- get around it
  There' no way to getting around it. それは避けて通れないよ．



