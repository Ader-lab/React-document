---
description: react教學文檔
---

# React

## 一、create-react-app

全域安装create-react-app

```text
$ npm install -g create-react-app
```

創建新項目

```text
$ create-react-app your-app 注意命名方式，全小寫

Creating a new React app in /dir/your-app.

Installing packages. This might take a couple of minutes. 安装過程慢
Installing react, react-dom, and react-scripts... 
```

如果不想全局安装，可以直接使用npx

```text
$ npx create-react-app your-app	
```

這個過程實際上會安裝這三樣東西: 

* react: react的library
* react-dom: 因为react有很多的運行環境，比如app端的react-native，我们要在web上运行就使用react-dom
* react-scripts: 包含執行和打包react應用程序的所有脚本及配置

出現下面的界面，表示創建項目成功:

```text
Success! Created your-app at /dir/your-app
Inside that directory, you can run several commands:

  npm start
    Starts the development server.

  npm run build
    Bundles the app into static files for production.

  npm test
    Starts the test runner.

  npm run eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd your-app
  npm start

Happy hacking!
```

根据上面的提示，通过`cd your-app`命令进入目录并运行`npm start`即可运行项目。

生成项目的目录结构如下：

```text
├── README.md							使用方法的文档
├── node_modules					所有的依赖安装的目录
├── package-lock.json			
├── package.json					
├── public								靜態公共目錄
└── src										開發用的源代碼目錄
```



常見問題：

* npm安裝失敗

  * 請刪除node\_modules及package-lock.json然后重新执行`npm install命令`
  * 再不能解决就删除node\_modules及package-lock.json的同时清除npm缓存`npm cache clean --force`之後再執行`npm install`命令

  \*\*\*\*

## 二、關於React

### **1、React的起源和發展**

React起源於Facebook的內部項目，因為該公司對市場上所有JavaScript MVC框架，都不滿意，就決定自己寫一套，用作架設Instagram的網站。做出來以後，發現這套東西很好用，就在2013年5月開源了。

### **2、React與傳統MVC的關係** 

React不是一個完整的MVC框架，最多可以認為是MVC中的V（View），甚至React並不非常認可MVC開發模式；React 構建頁面 UI 的庫。可以簡單地理解為，React 將界面分成了各個獨立的小塊，每一個塊就是組件，這些組件之間可以組合、嵌套，就成了我們的頁面。

### 3、React高性能的體現：虛擬DOM 

**React高性能的原理：** 

在Web開發中我們總需要將變化的資料實時反應到UI上，這時就需要對DOM進行操作。而復雜或頻繁的DOM操作通常是性能瓶頸產生的原因（如何進行高性能的複雜DOM操作通常是衡量一個前端開發人員技能的重要指標）。 React為此引入了虛擬DOM（Virtual DOM）的機制：在瀏覽器端用Javascript實現了一套DOM API。基於React進行開發時所有的DOM構造都是通過虛擬DOM進行，每當資料變化時，React都會重新構建整個DOM樹，然後React將當前整個DOM樹和上一次的DOM樹進行對比，得到DOM結構的區別，然後僅僅將需要變化的部分進行實際的瀏覽器DOM更新。而且React能夠批處理虛擬DOM的刷新，在一個事件循環（Event Loop）內的兩次資料變化會被合併，例如你連續的先將節點內容從AB, BA，React會認為A變成B，然後又從B變成A UI不發生任何變化，而如果通過手動控制，這種邏輯通常是極其複雜的。

**React Fiber:**

在react 16之後發布的一種react 核心算法，React Fiber是對核心算法的一次重新實現\(官網說法\)。之前用的是diff算法。

在之前React中，更新過程是同步的，這可能會導致性能問題。

當React決定要加載或者更新組件樹時，會做很多事，比如調用各個組件的生命週期函數，計算和比對Virtual DOM，最後更新DOM樹，這整個過程是同步進行的，也就是說只要一個加載或者更新過程開始，中途不會中斷。因為JavaScript單線程的特點，如果組件樹很大的時候，每個同步任務耗時太長，就會出現卡頓。

React Fiber的方法其實很簡單——分片。把一個耗時長的任務分成很多小片，每一個小片的運行時間很短，雖然總時間依然很長，但是在每個小片執行完之後，都給其他任務一個執行的機會，這樣唯一的線程就不會被獨占，其他任務依然有運行的機會。

### 4、React的特點和優勢 

**\(1\) 虛擬DOM**

我們以前操作dom的方式是通過document.getElementById\(\)的方式，這樣的過程實際上是先去讀取html的dom結構，將結構轉換成變量，再進行操作。而reactjs定義了一套變量形式的dom模型，一切操作和換算直接在變量中，這樣減少了操作真實dom，性能真實相當的高，和主流MVC框架有本質的區別，並不和dom打交道

**\(2\) 組件系統**

react最核心的思想是將頁面中任何一個區域或者元素都可以看做一個組件 component

那麼什麼是組件呢？

組件指的就是同時包含了html、css、js、image元素的聚合體

使用react開發的核心就是將頁面拆分成若干個組件，並且react一個組件中同時耦合了css、js、image，這種模式整個顛覆了過去的傳統的方式

**\(3\) 單向資料流**

其實react.js的核心內容就是資料綁定，所謂資料綁定指的是只要將一些服務端的資料和前端頁面綁定好，開發者只關注實現業務就行了

**\(4\) JSX 語法**

在vue中，我們使用render函數來構建組件的dom結構性能較高，因為省去了查找和編譯模板的過程，但是在render中利用createElement創建結構的時候代碼可讀性較低，較為複雜，此時可以利用jsx語法來在render中創建dom，解決這個問題，但是前提是需要使用工具來編譯jsx

## 三、編寫第一個react應用程序 

react開發需要引入多個依賴文件：react.js、react-dom.js，分別又有開發版本和生產版本，create-react-app裡已經幫我們把這些東西都安裝好了。然後打開 index.js. 檔案

```text
// 從 react 的包當中引入了 React。只要你要寫 React.js 組件就必須引入React, 因為react裡有一種語法叫JSX
import React from 'react'
// ReactDOM 可以幫助我們把 React 组件渲染到頁面上去
import ReactDOM from 'react-dom'

// ReactDOM裡有一個render方法，功能就是把組件渲染並且構造 DOM 樹，然後插入到頁面上某個特定的元素上
ReactDOM.render(
// JSX語法，看起來像是純 HTML 代碼寫在 JavaScript 代碼裡面，但是又不需要加上引號。
  <h1>歡迎進入React的世界</h1>,
// 渲染到哪里
  document.getElementById('root')
)
```

## 四、元素與組件 

如果代碼多了之後，不可能一直在render方法裡寫，所以就需要把裡面的代碼提出來，定義一個變量，像這樣：

```text
import React from 'react'
import ReactDOM from 'react-dom'
// 這是在用JSX定義一下react元素
const app = <h1>歡迎進入React的世界</h1>
ReactDOM.render(
  app,
  document.getElementById('root')
)
```

### 1、函數式組件 

由於元素沒有辦法傳遞參數，所以我們就需要把之前定義的變量改為一個方法，讓這個方法去return一個元素:



```text
import React from 'react'
import ReactDOM from 'react-dom'

// 特別注意這裡的寫法，如果要在JSX裡寫js表達式(只能是表達式，不能流程控制)，就需要加 {}，包括註釋也是一樣，並且可以多層嵌套
const app = (props) => <h1>歡迎進入{props.name}的世界</h1>

ReactDOM.render(
  app({
    name: 'react'
  }),
  document.getElementById('root')
)
```

這裡我們定義的方法實際上也是react定義組件的第一種方式-定義函數式組件，這也是無狀態組件。但是這種寫法不符合react的jsx的風格，更好的方式是使用以下方式進行改造



```text
import React from 'react'
import ReactDOM from 'react-dom'

const App = (props) => <h1>歡迎進入{props.name}的世界</h1>

ReactDOM.render(
  // React组件的調用方式
  <App name="react" />,
  document.getElementById('root')
)
```

這樣一個完整的函數式組件就定義好了。但要注意！注意！注意！組件名必須大寫，並且HTML標籤名必需小寫。

### 2、class组件

ES6的加入讓JavaScript直接支持使用class來定義一個類，react的第二種創建組件的方式就是使用的類的繼承，ES6 class是目前官方推薦的使用方式，它使用了ES6標準語法來構建，看以下代碼：

```text
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  render () {
    return (
      // 注意這裡得用this.props.name, 必需用this.props
      <h1>歡迎進入{this.props.name}的世界</h1>
  	)
  }
}
ReactDOM.render(
  <App name="react" />,
  document.getElementById('root')
)
```



運行結果和之前完全一樣，因為JS裡沒有真正的class，這個class只是一個語法糖，但二者的運行機制底層運行機制不一樣。

* 函數式组件是直接调用, 在前面的代碼裡已經有看到
* `es6 class`组件每次使用组件都相当于在實體化组件，例如:

```text
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  render () {
    return (
  		<h1>欢迎进入{this.props.name}的世界</h1>
  	)
  }
}

const app = new App({
  name: 'react'
}).render()

ReactDOM.render(
  app,
  document.getElementById('root')
)
```



###  3、更老的一種方法

現在基本上不用

```text
React.createClass({
  render () {
    return (
      <div>{this.props.xxx}</div>
  	)
  }
})
```

### 4、组件的组合、嵌套

組件嵌套的方式就是將子組件寫入到父組件的模板中，並且React組件只能有一個根節點

```text
import React, { Component, Fragment } from 'react'
import ReactDOM from 'react-dom'

class Title extends Component {
  render () {
    return (
      <h1>歡迎進入React的世界</h1>
  	)
  }
}
class Content extends Component {
  render () {
    return (
      <p>React.js非常好用!!!</p>
  	)
  }
}

// 如果不想生成多餘的一層dom可以使用React提供的Fragment組件在最外層進行包裹
class App extends Component {
  render () {
    return (
      <Fragment>
      	<Title />
        <Content />
      </Fragment>
  	)
  }
}
ReactDOM.render(
  <App/>,
  document.getElementById('root')
)
```

##  五、JSX 原理

要明白JSX的原理，需要先明白如何用 JavaScript 物件來表現一個 DOM 元素的結構? 

看下面的DOM結構

```text
<div class='app' id='appRoot'>
  <h1 class='title'>歡迎進入React的世界</h1>
  <p>
    React.js非常好用!!!
  </p>
</div>
```

上面這個 HTML 所有的信息我們都可以用 JavaScript 物件來表示：

```text
{
  tag: 'div',
  attrs: { className: 'app', id: 'appRoot'},
  children: [
    {
      tag: 'h1',
      attrs: { className: 'title' },
      children: ['歡迎進入React的世界']
    },
    {
      tag: 'p',
      attrs: null,
      children: ['React.js非常好用!!!']
    }
  ]
}
```

但是用 JavaScript 寫起來太長了，結構看起來又不清晰，用 HTML 的方式寫起來就方便很多了。

於是 React.js 就把 JavaScript 的語法擴展了一下，讓 JavaScript 語言能夠支持這種直接在 JavaScript 代碼裡面編寫類似 HTML 標籤結構的語法，這樣寫起來就方便很多了。編譯的過程會把類似 HTML 的 JSX 結構轉換成 JavaScript 的物件結構。

下面代碼:

```text
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  render () {
    return (
      <div className='app' id='appRoot'>
        <h1 className='title'>歡迎進入React的世界</h1>
        <p>
          React.js 是一个构建页面 UI 的库
        </p>
      </div>
    )
  }
}

ReactDOM.render(
	<App />,
  document.getElementById('root')
)
```

編譯之後將得到這樣的代碼:

```text
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  render () {
    return (
      React.createElement(
        "div",
        {
          className: 'app',
          id: 'appRoot'
        },
        React.createElement(
          "h1",
          { className: 'title' },
          "歡迎進入React的世界"
        ),
        React.createElement(
          "p",
          null,
          "React.js非常好用!!!"
        )
      )
    )
  }
}

ReactDOM.render(
	React.createElement(App),
  document.getElementById('root')
)
```

React.createElement 會構建一個 JavaScript 物件來描述你 HTML 結構的信息，包括標籤名、屬性、還有子元素等, 語法為

```text
React.createElement(
  type,
  [props],
  [...children]
)
```

所謂的 JSX 其實就是 JavaScript 物件，所以使用 React 和 JSX 的時候一定要經過編譯的過程:

 JSX —使用react構造组件，bable進行編譯—&gt; JavaScript物件 — `ReactDOM.render()`—&gt;DOM元素 —&gt;插入頁面

##  六、组件中DOM樣式

* 行內樣式

想給虛擬dom添加行內樣式，需要使用表達式傳入樣式對象的方式來實現：

```text
// 注意這裡的兩個括號，第一個表示我們在要JSX裡插入JS了，第二個是物件的括號
 <p style={{color:'red', fontSize:'14px'}}>Hello world</p>
```

* 使用`class`

`class`需要寫成`className`（畢竟是在寫js代碼，會收到js規則的現在，而class是關鍵字）

```text
<p className="hello" style = {this.style}>Hello world</p>
```

## 七、组件的資料傳遞方式

### 1、属性\(props\)

`props`是正常是外部傳入的，組件內部也可以通過一些方式來初始化的設置，屬性不能被組件自己更改，但是你可以通過父組件主動重新渲染的方式來傳入新的 `props`

屬性是描述性質、特點的，組件自己不能隨意更改。 

在使用一個組件的時候，可以把參數放在標籤的屬性當中，所有的屬性都會作為組件 `props` 對象的鍵值。通過箭頭函數創建的組件，需要通過函數的參數來接收`props`:

```text
import React, { Component, Fragment } from 'react'
import ReactDOM from 'react-dom'

class Title extends Component {
  render () {
    return (
  		<h1>欢迎进入{this.props.name}的世界</h1>
  	)
  }
}

const Content = (props) => {
  return (
    <p>{props.name}好實用!!!</p>
  )
}

class App extends Component {
  render () {
    return (
  		<Fragment>
      	<Title name="React" />
        <Content name="React.js" />
      </Fragment>
  	)
  }
}

ReactDOM.render(
	<App/>,
  document.getElementById('root')
)
```

####  \(1\) 設置組件的默認props

```text
import React, { Component, Fragment } from 'react'
import ReactDOM from 'react-dom'

class Title extends Component {
  // 使用類創建的組件，直接在這裡寫static方法，創建defaultProps
  static defaultProps = {
    name: 'React'
  }
  render () {
    return (
  		<h1>入{this.props.name}的世界</h1>
  	)
  }
}

const Content = (props) => {
  return (
    <p>{props.name}真有趣</p>
  )
}

// 使用箭頭函創建的組件，需要在這個組件上直接寫defaultProps屬性
Content.defaultProps = {
  name: 'React.js'
}

class App extends Component {
  render () {
    return (
  		<Fragment>
        {/* 由於設置了defaultProps，不傳props也能正常運行，如果傳遞了就會覆蓋defaultProps的值 */}
      	<Title />
        <Content />
      </Fragment>
  	)
  }
}

ReactDOM.render(
	<App/>,
  document.getElementById('root')
)
```

####  \(2\) props.children

我們知道使用組件的時候，可以嵌套。要在自定義組件的使用嵌套結構，就需要使用 `props.children`。

```text
import React, { Component, Fragment } from 'react'
import ReactDOM from 'react-dom'

class Title extends Component {
  render () {
    return (
  		<h1>入{this.props.children}的世界</h1>
  	)
  }
}

const Content = (props) => {
  return (
    <p>{props.children}</p>
  )
}

class App extends Component {
  render () {
    return (
  		<Fragment>
      	<Title>React</Title>
        <Content><i>React.js</i>是一个构建UI的库</Content>
      </Fragment>
  	)
  }
}

ReactDOM.render(
	<App/>,
  document.getElementById('root')
)
```

####  \(3\) 使用prop-types检查props

React其實是為了構建大型應用程序而生, 在一個大型應用中，根本不知道別人使用你寫的組件的時候會傳入什麼樣的參數，有可能會造成應用程序運行不了，但是不報錯。為了解決這個問題，React提供了一種機制，讓寫組件的人可以給組件的`props`設定參數檢查，需要安裝和使用[**prop-types**](https://www.npmjs.com/package/prop-types):



```text
$ npm i prop-types
```

###  2、狀態\(state\)

狀態就是組件描述某種顯示情況的資料，由組件自己設置和更改，也就是說由組件自己維護，使用狀態的目的就是為了在不同的狀態下使組件的顯示不同\(自己管理\)。

  
\(1\) 定義state

第一種方式

```text
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

class App extends Component {
  state = {
    name: 'React',
    isLiked: false
  }
  render () {
    return (
      <div>
        <h1>欢迎到{this.state.name}的世界</h1>
        <button>
          {
            this.state.isLiked ? '取消' : '收藏'
          }
        </button>
      </div>
  	)
  }
}
ReactDOM.render(
	<App/>,
  document.getElementById('root')
)
```

另一种方式\(推荐\)



```text
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

class App extends Component {
  constructor() {
    super()
    this.state = {
      name: 'React',
      isLiked: false
    }
  }
  render () {
    return (
  		<div>
        <h1>欢迎到{this.state.name}的世界</h1>
        <button>
          {
            this.state.isLiked ? '取消' : '收藏'
          }
        </button>
      </div>
  	)
  }
}
ReactDOM.render(
  <App/>,
  document.getElementById('root')
)
```





