# React Tutorial 和訳

かなり自分に都合よく意訳していますので一部参考ということで．
雰囲気はつかめると思います．

原典：
[http://facebook.github.io/react/docs/tutorial.html](http://facebook.github.io/react/docs/tutorial.html)

2014-05-01時点．

===

# Tutorial

We'll be building a simple but realistic comments box that you can drop into a blog, a basic version of the realtime comments offered by Disqus, LiveFyre or Facebook comments.

We'll provide:

- A view of all of the comments
- A form to submit a comment
- Hooks for you to provide a custom backend

It'll also have a few neat features:

- Optimistic commenting: comments appear in the list before they're saved on the server so it feels fast.
- Live updates: other users' comments are popped into the comment view in real time. 
- Markdown formatting: users can use Markdown to format their text.

## （和訳）チュートリアル

簡単で実用的なブログのコメントシステムを作成していきましょう．Disqus，LiveFyreやFacebookのリアルタイムコメントの基本的なシステムです．

作成するもの：

- 全コメントのビュー
- コメント投稿フォーム
- お望みのバックエンドのコメントフック

他にも実装するいくつかの機能：

- コメント投稿の即反映
- ライブ・アップデート
- マークダウン記法対応

===

# Running a server

While it's not necessary to get started with this tutorial, later on we'll be adding functionality that requires POSTing to a running server. If this is something you are intimately familiar with and want to create your own server, please do. For the rest of you who might want to focus on learning about React without having to worry about the server-side aspects, we have written simple servers in a number of languages - JavaScript (using Node.js), Python, Ruby, Go, and PHP. These are all available on GitHub. You can view the source or download a zip file to get started.

To get started using the tutorial, just start editing public/index.html.


## （和訳）サーバの起動

チュートリアルを始める上では必須ではないですが，いつかはサーバにPOSTする機能を追加することでしょう．もしサーバサイドに精通していて，サーバサイドをつくりたいというのであれば，ぜひ作成してください！サーバーサイドの作成に時間を割かず，Reactのみにフォーカスして学びたいという方には，私たちがシンプルなサーバプログラムを用意しました．それらはいくつかの言語で書かれていて，JavaScript（Node.js），Python，Ruby，GoやPHPのものがあります．それらはGitHubにあります．ソースコードが見れるし，zipファイルとしてダウンロードし，すぐ始めることができます．

チュートリアルを開始します，public/index.htmlをエディタで書き始めましょう！

===

# Getting started

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


## （和訳）はじめましょう！

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

===

# Your first component

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

## （和訳）最初のコンポーネント

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

=== 

# JSX Syntax

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

## （和訳）JSX記法

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

===

# What's going on

We pass some methods in a JavaScript object to React.createClass() to create a new React component. The most important of these methods is called render which returns a tree of React components that will eventually render to HTML.

The `<div>` tags are not actual DOM nodes; they are instantiations of React div components. You can think of these as markers or pieces of data that React knows how to handle. React is safe. We are not generating HTML strings so XSS protection is the default.

You do not have to return basic HTML. You can return a tree of components that you (or someone else) built. This is what makes React composable: a key tenet of maintainable frontends.

React.render() instantiates the root component, starts the framework, and injects the markup into a raw DOM element, provided as the second argument.

## （和訳）何が起こっているのでしょうか

新しいReactコンポーネントを生成するために，いくつかのメソッドを生やしたJavaScriptオブジェクトを，React.createClass()に渡します．メソッドの中で最も重要なのは，イベントが発生した時にHTMLを生成するReactコンポーネントのツリーをreturnするrenderメソッドがcallされていることです．

`<div>`タグがありますがこれはいわゆるDOMではありません；これらはReactのdivコンポーネントツリーのインスタンスです．これらは，Reactがデータをどのように扱うかのマーカーや情報だと思ってください．Reactは**安全**なのです．決してHTML文字列を生成しないのでXSSプロテクトが施されているのです．

一般的なHTMLを返す必要はありません．あなたが（もしくは誰かが）構築したコンポーネントツリーを返すべきです．このことがReactを**コンポーサブル（構築しやすく）**しています．これはメンテナンスしやすいフロントエンドのための重要な指針です．

React.render()はルートコンポーネントを生成し，配下のコンポーネント構築も行います，そして２番めの引数である生DOMにマークアップ（HTML）を注入します．

===

# Composing components

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

## （和訳）コンポーネントを組み立てる

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

===

# Using props

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

## （和訳）propsを使う

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

===

# Component Properties

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

## （和訳）コンポーネントのプロパティ

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

===

# Adding Markdown

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


## （和訳）Markdown機能を追加する

Markdownはインライン・テキストを整形するのに簡単な機能です．例えばアスタリスクで囲んだテキストは強調されます．

まず，サードパーティ製*marked*ライブラリを今回のアプリケーションに追加しましょう．これはMarkdownテキストを生のHTMLへコンバートするJavaScriptライブラリです．headタグにスクリプトタグを追加してください（React Playgroundにはすでに追加されています）．

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

===

# Hook up the data model

So far we've been inserting the comments directly in the source code. Instead, let's render a blob of JSON data into the comment list. Eventually this will come from the server, but for now, write it in your source:

```
// tutorial8.js
var data = [
  {author: "Pete Hunt", text: "This is one comment"},
  {author: "Jordan Walke", text: "This is *another* comment"}
];
```

We need to get this data into CommentList in a modular way. Modify CommentBox and the React.render() call to pass this data into the CommentList via props:

```
// tutorial9.js
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.props.data} />
        <CommentForm />
      </div>
    );
  }
});

React.render(
  <CommentBox data={data} />,
  document.getElementById('content')
);
```

Now that the data is available in the CommentList, let's render the comments dynamically:

```
// tutorial10.js
var CommentList = React.createClass({
  render: function() {
    var commentNodes = this.props.data.map(function (comment) {
      return (
        <Comment author={comment.author}>
          {comment.text}
        </Comment>
      );
    });
    
return (
      <div className="commentList">
        {commentNodes}
      </div>
    );
  }
});
```

That's it!


## （和訳）データモデルを渡す

これまではコメントをソースコード中に直接書いていました．今度はJSONデータをコメント・リストの中へレンダリングしてみましょう：

```
// tutorial8.js
var data = [
  {author: "Pete Hunt", text: "This is one comment"},
  {author: "Jordan Walke", text: "This is *another* comment"}
];
```

CommentListへデータを流しこむには組み立てるようにする必要があります．CommentBoxとを React.render() を編集します．そこで， CommentList に props を使ってデータを渡します．

```
// tutorial9.js
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.props.data} />
        <CommentForm />
      </div>
    );
  }
});

React.render(
  <CommentBox data={data} />,
  document.getElementById('content')
);
```

もう CommentListではデータを扱えます．コメント群をダイナミックにレンダリングしてみましょう：

```
// tutorial10.js
var CommentList = React.createClass({
  render: function() {
    var commentNodes = this.props.data.map(function (comment) {
      return (
        <Comment author={comment.author}>
          {comment.text}
        </Comment>
      );
    });
    return (
      <div className="commentList">
        {commentNodes}
      </div>
    );
  }
});
```

こんなかんじです！

===

# Fetching from the server

Let's replace the hard-coded data with some dynamic data from the server. We will remove the data prop and replace it with a URL to fetch:

```
// tutorial11.js
React.render(
  <CommentBox url="comments.json" />,
  document.getElementById('content')
);
```

This component is different from the prior components because it will have to re-render itself. The component won't have any data until the request from the server comes back, at which point the component may need to render some new comments.

## （和訳）サーバからデータを取得する

直接書いた（hard-coded）データを，サーバから取得する動的なデータに置き換えましょう．データ・プロパティを取りのぞきデータを取得するURLへ置き換えます：

```
// tutorial11.js
React.render(
  <CommentBox url="comments.json" />,
  document.getElementById('content')
);
```

このコンポーネントは以前のものとは，自身をレンダリングし直すという点で，違います．コンポーネントはデータを持っていません．サーバからリクエストが戻ってくると，コンポーネントは対象のコメントをレンダリングしなおします．

===

# Reactive state

So far, each component has rendered itself once based on its props. props are immutable: they are passed from the parent and are "owned" by the parent. To implement interactions, we introduce mutable state to the component. this.state is private to the component and can be changed by calling this.setState(). When the state is updated, the component re-renders itself.

render() methods are written declaratively as functions of this.props and this.state. The framework guarantees the UI is always consistent with the inputs.

When the server fetches data, we will be changing the comment data we have. Let's add an array of comment data to the CommentBox component as its state:

```
// tutorial12.js
var CommentBox = React.createClass({
  getInitialState: function() {
    return {data: []};
  },
  
render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm />
      </div>
    );
  }
});
```

getInitialState() executes exactly once during the lifecycle of the component and sets up the initial state of the component.

## （和訳）Reactのstate

それぞれのコンポーネントは，propsを参照し，レンダリングを一度しかしません．propsはイミュータブル（不変）なのです：
データは親コンポーネントから渡され，"親のもの"です．データのやりとりを実装するにはミュータブル（可変の）な*state*をコンポーネントへ用意しました．stateが更新されるとコンポーネントはレンダリングしなおします．

render() は，this.prop と this.state の機能として定義されているメソッドです．

サーバがデータを取得したとき，いまあるコメントでデータを変更します．それではコメントデータ配列をCommentBoxコンポーネントへstate として追加してみましょう：

```
// tutorial12.js
var CommentBox = React.createClass({
  getInitialState: function() {
    return {data: []};
  },
  
render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm />
      </div>
    );
  }
});
```

getInitialState() はそのコンポーネントのライフサイクルにおいて一度しか実行されません．そしてそのコンポーネントの初期stateを設定します．

===

## Updating state

When the component is first created, we want to GET some JSON from the server and update the state to reflect the latest data. In a real application this would be a dynamic endpoint, but for this example, we will use a static JSON file to keep things simple:

```
// tutorial13.json
[
  {"author": "Pete Hunt", "text": "This is one comment"},
  {"author": "Jordan Walke", "text": "This is *another* comment"}
]
```

We'll use jQuery to help make an asynchronous request to the server.

Note: because this is becoming an AJAX application you'll need to develop your app using a web server rather than as a file sitting on your file system. As mentioned above, we have provided several servers you can use on GitHub. They provide the functionality you need for the rest of this tutorial.

```
// tutorial13.js
var CommentBox = React.createClass({
  getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  
render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm />
      </div>
    );
  }
});
```

Here, componentDidMount is a method called automatically by React when a component is rendered. The key to dynamic updates is the call to this.setState(). We replace the old array of comments with the new one from the server and the UI automatically updates itself. Because of this reactivity, it is only a minor change to add live updates. We will use simple polling here but you could easily use WebSockets or other technologies.

```
// tutorial14.js
var CommentBox = React.createClass({
  loadCommentsFromServer: function() {
    
$.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  
getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    this.loadCommentsFromServer();
    setInterval(this.loadCommentsFromServer, this.props.pollInterval);
  
},
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm />
      </div>
    );
  }
});

React.render(
  <CommentBox url="comments.json" pollInterval={2000} />,
  document.getElementById('content')
);
```

All we have done here is move the AJAX call to a separate method and call it when the component is first loaded and every 2 seconds after that. Try running this in your browser and changing the comments.json file; within 2 seconds, the changes will show!

## （和訳）stateを更新する

そのコンポーネントが作成された最初に，わたしたいはサーバからJSONデータをGETしたいです．stateに最新のデータを更新します．実際のアプリケーションではJSONをGETすることはダイナミック（動的に）行われることでしょう．でも，今回の例では，簡単な例に保つために静的なJSONデータファイルを使用していきます．

```
// tutorial13.json
[
  {"author": "Pete Hunt", "text": "This is one comment"},
  {"author": "Jordan Walke", "text": "This is *another* comment"}
]
```

サーバへのAJAX通信にはjQueryを使用します．

Note：ここでAJAXアプリケーションの例になっているのは，ファイルシステムからファイルを読むよりもウェブサーバを使用するアプリケーション開発することの方が求められるであろうからです．**そう言及したのにはGitHubに**あなたがいますぐ使えるサーバプログラムを用意しているからです．このチュートリアルの以降の部分ではそれらの機能をしようします．

```
// tutorial13.js
var CommentBox = React.createClass({
  getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm />
      </div>
    );
  }
});
```

ここでは componentDidMount は，コンポーネントがレンダリングされた時にReactから自動的に呼び出される，メソッドです．ダイナミック・アップデートするための鍵となるのが，this.setState() を呼ぶことです．コメントの配列を使用しているところを，サーバから取得するよう置き換えました．UI（見た目，ユーザ・インターフェース）が自動的に更新されます．このreactivityを見るためにしたことは，ライブ・アップデートを追加する小さな変更を加えただけです．簡単なポーリング（サーバ問い合わせ）を採用しましたが，ここはWebSocketなどの技術に簡単に置き換えられることができます．

```
// tutorial14.js
var CommentBox = React.createClass({
  loadCommentsFromServer: function() {
    
$.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  
getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    this.loadCommentsFromServer();
    setInterval(this.loadCommentsFromServer, this.props.pollInterval);
  
},
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm />
      </div>
    );
  }
});

React.render(
  <CommentBox url="comments.json" pollInterval={2000} />,
  document.getElementById('content')
);
```

今変更を加えたところは，AJAXコールを一つのメソッドにまとめて，コンポーネントが際しにロードされた時と，その後2秒おきにそのメソッドを呼ぶことができるようにしたことです．ブラウザで試してみましょう．ファイルを編集してみてください．2秒としないうちに変更があったことがわかるでしょう！

===

# Adding new comments

Now it's time to build the form. Our CommentForm component should ask the user for their name and comment text and send a request to the server to save the comment.

```
// tutorial15.js
var CommentForm = React.createClass({
  render: function() {
    return (
      <form className="commentForm">
        <input type="text" placeholder="Your name" />
        <input type="text" placeholder="Say something..." />
        <input type="submit" value="Post" />
      </form>
    
);
  }
});
```

Let's make the form interactive. When the user submits the form, we should clear it, submit a request to the server, and refresh the list of comments. To start, let's listen for the form's submit event and clear it.

```
// tutorial16.js
var CommentForm = React.createClass({
  handleSubmit: function(e) {
    e.preventDefault();
    var author = React.findDOMNode(this.refs.author).value.trim();
    var text = React.findDOMNode(this.refs.text).value.trim();
    if (!text || !author) {
      return;
    }
    // TODO: send request to the server
    React.findDOMNode(this.refs.author).value = '';
    React.findDOMNode(this.refs.text).value = '';
    return;
  },
  render: function() {
    return (
      <form className="commentForm" onSubmit={this.handleSubmit}>
        <input type="text" placeholder="Your name" ref="author" />
        <input type="text" placeholder="Say something..." ref="text" />
        <input type="submit" value="Post" />
      </form>
    );
  }
});
```

## Events

React attaches event handlers to components using a camelCase naming convention. We attach an onSubmit handler to the form that clears the form fields when the form is submitted with valid input.

Call preventDefault() on the event to prevent the browser's default action of submitting the form.

## Refs

We use the ref attribute to assign a name to a child component and this.refs to reference the component. We can call React.findDOMNode(component) on a component to get the native browser DOM element.

## Callbacks as props

When a user submits a comment, we will need to refresh the list of comments to include the new one. It makes sense to do all of this logic in CommentBox since CommentBox owns the state that represents the list of comments.

We need to pass data from the child component back up to its parent. We do this in our parent's render method by passing a new callback (handleCommentSubmit) into the child, binding it to the child's onCommentSubmit event. Whenever the event is triggered, the callback will be invoked:

```
// tutorial17.js
var CommentBox = React.createClass({
  loadCommentsFromServer: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  handleCommentSubmit: function(comment) {
    // TODO: submit to the server and refresh the list
  },
  
getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    this.loadCommentsFromServer();
    setInterval(this.loadCommentsFromServer, this.props.pollInterval);
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm onCommentSubmit={this.handleCommentSubmit} />
      </div>
    );
  }
});
```

Let's call the callback from the CommentForm when the user submits the form:

```
// tutorial18.js
var CommentForm = React.createClass({
  handleSubmit: function(e) {
    e.preventDefault();
    var author = React.findDOMNode(this.refs.author).value.trim();
    var text = React.findDOMNode(this.refs.text).value.trim();
    if (!text || !author) {
      return;
    }
    this.props.onCommentSubmit({author: author, text: text});
    
React.findDOMNode(this.refs.author).value = '';
    React.findDOMNode(this.refs.text).value = '';
    return;
  },
  render: function() {
    return (
      <form className="commentForm" onSubmit={this.handleSubmit}>
        <input type="text" placeholder="Your name" ref="author" />
        <input type="text" placeholder="Say something..." ref="text" />
        <input type="submit" value="Post" />
      </form>
    );
  }
});
```

Now that the callbacks are in place, all we have to do is submit to the server and refresh the list:

```
// tutorial19.js
var CommentBox = React.createClass({
  loadCommentsFromServer: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  handleCommentSubmit: function(comment) {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      type: 'POST',
      data: comment,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  
},
  getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    this.loadCommentsFromServer();
    setInterval(this.loadCommentsFromServer, this.props.pollInterval);
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm onCommentSubmit={this.handleCommentSubmit} />
      </div>
    );
  }
});
```

## (和訳)新しいコメントを追加する

それではフォームを作成しましょう．CommentFormコンポーネントは，コメント投稿者の名前とコメント・テキストが必要です．それからサーバへコメントを保存するようリクエストを送信します．

```
// tutorial15.js
var CommentForm = React.createClass({
  render: function() {
    return (
      <form className="commentForm">
        <input type="text" placeholder="Your name" />
        <input type="text" placeholder="Say something..." />
        <input type="submit" value="Post" />
      </form>
    );
  }
});
```

フォームはインタラクティブにします．ユーザがフォームをサブミットしたら，フォームをクリアし，サーバへリクエストを送信し，コメント・リストを更新します．はじめにフォームのサブミットイベントを監視し，フォームをクリアするようにしましょう．

```
// tutorial16.js
var CommentForm = React.createClass({
  handleSubmit: function(e) {
    e.preventDefault();
    var author = React.findDOMNode(this.refs.author).value.trim();
    var text = React.findDOMNode(this.refs.text).value.trim();
    if (!text || !author) {
      return;
    }
    // TODO: send request to the server
    React.findDOMNode(this.refs.author).value = '';
    React.findDOMNode(this.refs.text).value = '';
    return;
  },
  
render: function() {
    return (
      <form className="commentForm" onSubmit={this.handleSubmit}>
        <input type="text" placeholder="Your name" ref="author" />
        <input type="text" placeholder="Say something..." ref="text" />
        <input type="submit" value="Post" />
      </form>
    );
  }
});
```

## Events

Reactはキャメルケースというネーミングルールに従い，イベントハンドラをコンポーネントに割り当てます．フォームで正しい入力が行われsubmitされたならば，フォームフィールドをクリアする onSubmit ハンドラをフォームに割り当てます．
ブラウザのフォームをサブミットするデフォルト動作を抑制するために，イベントで preventDefault() を実行してください．

## Refs

子コンポーネントに対しての変数名として ref 属性をしようします．そして this.refs としてコンポーネントが参照します．コンポーネントではネイティブなブラウザDOM要素を取得するのに React.findDOMNode(component) を呼び出します．

## Callbacks as props

ユーザがコメントをサブミットした時，新しいコメントを含んだコメント・リストを更新しなければなりません．CommentBoxがコメントのリストを代表するstateを持っているので，コメントボックスの中でこのロジックの全てが行われているということがわかるでしょう．

逆に，子コンポーネントから親コンポーネントへデータを渡したい時があります．その時は，親コンポーネントの render メソッドで，子コンポーネントへ対して新しいコールバック関数（例では，handleCommentSubmit）を渡してやることで同じことができます．コールバック関数は子コンポーネントに onCommentSubmit イベントとしてバインドされています．イベントが起こる度にかならずコールバック関数は実行されます：

```
// tutorial17.js
var CommentBox = React.createClass({
  loadCommentsFromServer: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  handleCommentSubmit: function(comment) {
    // TODO: submit to the server and refresh the list
  },
  getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    this.loadCommentsFromServer();
    setInterval(this.loadCommentsFromServer, this.props.pollInterval);
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm onCommentSubmit={this.handleCommentSubmit} />
      </div>
    );
  }
});
```

ユーザがフォームをサブミットした時にコメントフォームからコールバック関数を呼び出してみましょう：

```
// tutorial18.js
var CommentForm = React.createClass({
  handleSubmit: function(e) {
    e.preventDefault();
    var author = React.findDOMNode(this.refs.author).value.trim();
    var text = React.findDOMNode(this.refs.text).value.trim();
    if (!text || !author) {
      return;
    }
    this.props.onCommentSubmit({author: author, text: text});
    React.findDOMNode(this.refs.author).value = '';
    React.findDOMNode(this.refs.text).value = '';
    return;
  },
  render: function() {
    return (
      <form className="commentForm" onSubmit={this.handleSubmit}>
        <input type="text" placeholder="Your name" ref="author" />
        <input type="text" placeholder="Say something..." ref="text" />
        <input type="submit" value="Post" />
      </form>
    );
  }
});
```

コールバック関数の中ですべきことは，サーバへサブミットすることとリストを更新することです：

```
// tutorial19.js
var CommentBox = React.createClass({
  loadCommentsFromServer: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  handleCommentSubmit: function(comment) {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      type: 'POST',
      data: comment,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  
},
  getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    this.loadCommentsFromServer();
    setInterval(this.loadCommentsFromServer, this.props.pollInterval);
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm onCommentSubmit={this.handleCommentSubmit} />
      </div>
    );
  }
});
```

===

# Optimization: optimistic updates

Our application is now feature complete but it feels slow to have to wait for the request to complete before your comment appears in the list. We can optimistically add this comment to the list to make the app feel faster.

```
// tutorial20.js
var CommentBox = React.createClass({
  loadCommentsFromServer: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  handleCommentSubmit: function(comment) {
    var comments = this.state.data;
    var newComments = comments.concat([comment]);
    this.setState({data: newComments});
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      type: 'POST',
      data: comment,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    this.loadCommentsFromServer();
    setInterval(this.loadCommentsFromServer, this.props.pollInterval);
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm onCommentSubmit={this.handleCommentSubmit} />
      </div>
    );
  }
});
```

## （和訳）最適化：最適な更新

アプリケーションは現在機能については完成していますが，リクエストが完了するのを待っているので，投稿したコメントがリストにでるまで，遅く感じます．もっと速く感じられるようにコメントに最適な修正を追加できます．

```
// tutorial20.js
var CommentBox = React.createClass({
  loadCommentsFromServer: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  handleCommentSubmit: function(comment) {
    var comments = this.state.data;
    var newComments = comments.concat([comment]);
    this.setState({data: newComments});
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      type: 'POST',
      data: comment,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    this.loadCommentsFromServer();
    setInterval(this.loadCommentsFromServer, this.props.pollInterval);
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm onCommentSubmit={this.handleCommentSubmit} />
      </div>
    );
  }
});
```

===

# Congrats!

You have just built a comment box in a few simple steps. Learn more about why to use React, or dive into the API reference and start hacking! Good luck!

## （和訳）コングラッツ！

あなたは少量の簡単なステップをこなしてコメントボックスをつくりあげました．「なぜReactなのか」をもっと学ぶのならば，APIリファレンスにとびこんでハッキングを始めましょう！幸運を！


