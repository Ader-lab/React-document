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
* react-dom: 因为react有很多的運行環境，比如app端的react-native，我们要在web上運行就使用react-dom
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

`this.props`和`this.state`是純js，如果直接更改的話，react是無法得知的，所以，需要使用特殊的更改狀態的方法setState。

#### \(2\) setState

`isLiked` 存放在實例的 `state` 物件當中，組件的 `render` 函數內，會根據組件的 `state` 的中`的isLiked`不同顯示“取消”或“收藏”內容。下面給 `button` 加上了點擊的事件監聽。

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
  handleBtnClick = () => {
    this.setState({
      isLiked: !this.state.isLiked
    })
  }
  render () {
    return (
      <div>
        <h1>到{this.state.name}的世界</h1>
        <button onClick={this.handleBtnClick}>
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



`setState`有两个参数

第一个参数可以是物件，也可以是方法return一個物件，我們把這個參數叫做`updater`

* 參數是物件

  ```text
  this.setState({
    isLiked: !this.state.isLiked
  })
  ```

* 參數是方法

  ```text
  this.setState((prevState, props) => {
    return {
      isLiked: !prevState.isLiked
    }
  })
  ```

  注意的是這個方法接收兩個參數，第一个是上一次的state，第二個是props

`setState`是異步的，所以想要獲取到最新的state，没有辦法獲取，就有了第二個參數，這是一個可選的回調函數

```text
this.setState((prevState, props) => {
  return {
    isLiked: !prevState.isLiked
  }
}, () => {
  console.log('回裡的',this.state.isLiked)
})
console.log('setState外部的',this.state.isLiked)
```

###  3、屬性vs狀態

相似點：都是純js物件，都會觸發render更新。

`state` 的主要作用是用於組件保存、控制、修改自己的可變狀態。 `state` 在組件內部初始化，可以被組件自身修改，而外部不能訪問也不能修改。你可以認為 `state` 是一個局部的、只能被組件自身控制的資料源。 `state` 中狀態可以通過 `this.setState`方法進行更新，`setState` 會導致組件的重新渲染。

`props` 的主要作用是讓使用該組件的父組件可以傳入參數來配置該組件。它是外部傳進來的配置參數，組件內部無法控制也無法修改。除非外部組件主動傳入新的 `props`，否則組件的 `props` 永遠保持不變。

###  4、狀態提升

如果有多個組件共享一個資料，把這個資料放到共同的父級組件中來管理，例如:登入狀態。

###  5、受控组件與非受控组件

React組件的資料渲染是否被調用者傳遞的`props`完全控制，控制則為受控組件，否則非受控組件。

###  6、渲染資料

* 條件渲染

  ```text
  {
    condition ? '取消' : '收藏'
  }
  ```

* 列表渲染

```text
// 
const people = [{
  id: 1,
  name: 'Leo',
  age: 35
}, {
  id: 2,
  name: 'James',
  age: 16
}]
// 渲染列表
{
  people.map(person => {
    return (
      <dl key={person.id}>
        <dt>{person.name}</dt>
        <dd>age: {person.age}</dd>
      </dl>
    )
  })
}
```

React的高效依賴於所謂的 Virtual-DOM，盡量不碰 DOM。對於列表元素來說會有一個問題：元素可能會在一個列表中改變位置。要實現這個操作，只需要交換一下DOM 位置就行了，但是React並不知道其實我們只是改變了元素的位置，所以它會重新渲染後面兩個元素（再執行Virtual-DOM ），這樣會大大增加DOM操作。但如果給每個元素加上唯一的標識，React 就可以知道這兩個元素只是交換了位置，這個標識就是key，這個 key 必須是每個元素唯一的標識

##  八、事件處理

### 1、绑定事件

採用on+事件名的方式來綁定一個事件，注意，這里和原生的事件是有區別的，原生的事件全是小寫`onclick`，React裡的事件是駝峰`onClick`，React的事件並不是原生事件，而是[`合成事件`](https://www.fooish.com/reactjs/handling-events.html)。

###  2、事件handler的寫法

* 直接在render裡寫行内的箭頭函式\(不推薦\)
* 在组件内使用箭頭函式定義一個方法\(推薦\)
* 直接在组件内定義一个非箭頭函式的方法，然后在render裡直接使用`onClick={this.handleClick.bind(this)}`\(不推薦，因為bind每執行一次會創建一次function，影響效能\)
* 直接在组件内定義一个非箭頭函式的方法，然后在constructor里bind\(this\)\(推薦，只綁定一次\)

### 3、Event 物件

和普通瀏覽器一樣，事件handler會被自動傳入一個 `event` 物件，這個物件和普通的瀏覽器 `event` 物件所包含的方法和屬性都基本一致。不同的是 React中的 `event` 物件並不是瀏覽器提供的，而是它自己內部所構建的。它同樣具有`event.stopPropagation`、`event.preventDefault` 這種常用的方法。

###  4、事件的参数傳遞

* 在`render`裡調用方法的地方外面包一層箭頭函數`onClick{(event) => this.itemClick("參數",event)}`
* 在`render`里通過`this.handleEvent.bind(this, "參數")`這樣的方式來傳遞 通過event傳遞
* 做一個子組件，在父組件中定義方法，通過props傳遞到子組件中，然後在子組件件通過this.props.method來調用

##  九、表单

在React裡，HTML表單元素的工作方式和其他的DOM元素有些不同，這是因為表單元素通常會保持一些內部的狀態。例如這個純HTML表單只接受一個名稱：

```text
<form>
  <label>
    名字:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="提交" />
</form>
```

此表單具有樣式的HTML表單行為，即在用戶提交表單後瀏覽到新頁面。如果你在React中執行相同的代碼，它仍然有效。但大多數情況下，使用JavaScript函數可以很方便的處理表單的 提交，同時還可以訪問用戶填充的表單數據。實現這種效果的標準方式是使用“替代組件”。

###  1、受控组件

在HTML中，表單元素（如，和）通常自己維護狀態，並根據用戶輸入進行更新。而在React中，變量狀態（可變狀態）通常保存在組件的狀態屬性中，並且只能通過 使用`setState（）`來更新。

我們可以把兩個結合起來，使React的狀態成為“唯一資料源”。渲染表單的React組件還控制著用戶輸入過程中表單發生的操作。被React以這種方式控制取值的表單輸入元素就 叫做“預設組件”。

例如，如果我們想讓前一個示例在提交時打印出名稱，我們可以將表單寫入為選擇性組件：

```text
class NameForm extends React.Component {
  constructor(props) {
    super();
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('提交的名字: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          名字:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="提交" />
      </form>
    );
  }
}
```

由於在表單元素上設置了 `value` 屬性，因此顯示的值將始終為 this.state.value，這使得 React 的 state 成為唯一數據源。由於 `handlechange` 在每次按鍵時都會執行並更新 React 的 state，因此顯示的值將隨著用戶輸入而更新。

對於受控組件來說，輸入的值始終由 React 的 state 驅動。你也可以將 value 傳遞給其他 UI 元素，或者通過其他事件處理函數重置，但這意味著你需要編寫更多的代碼。

###  2、textarea 標籤

在 HTML 中, `<textarea>` 元素通過其子元素定義其文本:

```text
<textarea>
  你好，這是在 text area 裡的文本
</textarea>
```

而在 React 中，`<textarea>` 使用 `value` 屬性代替。這樣，可以使得使用 `<textarea>` 的表單和使用單行 input 的表單非常類似：

```text
class EssayForm extends React.Component {
  constructor(props) {
    super();
    this.state = {
      value: '請撰写一篇你喜欢的DOM 元素的文章.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('提交的文章: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          文章:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="提交" />
      </form>
    );
  }
}
```

請注意，`this.state.value` 初始化於構造函數中，因此文本區域默認有初值。

### 3、select 標籤

在 HTML 中， 創建下拉列表標籤。例如，如下 HTML 創建了水果相關的下拉列表：

```text
<select>
  <option value="grapefruit">葡萄柚</option>
  <option value="lime">酸橙</option>
  <option selected value="coconut">椰子</option>
  <option value="mango">芒果</option>
</select>
```

請注意，由於 `selected` 屬性的緣故，椰子選項默認被選中。 React 並不會使用 `selected` 屬性，而是在根 `select` 標籤上使用 `value` 屬性。這在受控組件中更便捷，因為您只需要在根標籤中更新它。例如：

```text
class FlavorForm extends React.Component {
  constructor(props) {
    super();
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('你喜歡是: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          選擇你喜歡的水果:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">葡萄柚</option>
            <option value="lime">酸橙</option>
            <option value="coconut">椰子</option>
            <option value="mango">芒果</option>
          </select>
        </label>
        <input type="submit" value="提交" />
      </form>
    );
  }
}
```

你也可以將陣列傳遞到 `value` 屬性中，以支持在 `select` 標籤中選擇多個選項：

```text
<select multiple={true} value={['B', 'C']}>
```

```text
class MulFlavorForm extends React.Component {
	constructor(props) {
		super();
		this.state = {
			value: "2",
			arr: [],
			options: [
				{ value: "1", label: "葡萄柚" },
				{ value: "2", label: "柳橙" },
				{ value: "3", label: "椰子" },
				{ value: "4", label: "芒果" }
			]
		};
	  	this.handleChange = this.handleChange.bind(this);
	}
	handleChange(e){
		// 
		let idx = this.state.arr.findIndex(item=>{
			return item === e.target.value
		})
		if (idx >= 0) {
			// 
			this.state.arr.splice(idx,1);
		} else {
			// 
			this.state.arr.push(e.target.value);
		}
		let arr = this.state.arr;
		this.setState({arr});
	}
	handleBtn = () => {
		console.log(this.state.arr);
	}
	render() {
	  	return (
			<div>
				<select multiple={true} value={this.state.arr} onChange={this.handleChange}>
					{this.state.options.map((item,index) => {
					return <option value={item.value} key={index}>{item.label}</option>;
				})}
				</select>
				<button onClick={this.handleBtn}>click</button>
			</div>
		);
	}
}
export default MulFlavorForm;
```

### 4、處理多個輸入

當需要處理多個 `input` 元素時，我們可以給每個元素添加 `name` 屬性，並讓處理函數根據 `event.target.name` 的值選擇要執行的操作。

```text
class Reservation extends React.Component {
	constructor(props) {
	  super();
	  this.state = {
			isGoing: true,
			numberOfGuests: 2
	  };
  
	  this.handleInputChange = this.handleInputChange.bind(this);
	}
	handleInputChange(event) {
	  const target = event.target;
	  const value = target.type === 'checkbox' ? target.checked : target.value;
	  const name = target.name;
	
	  this.setState({
			[name]: value
	  });
	}
	handleClick = () =>{
		console.log(this.state);
	}
	render() {
	  return (
		<form>
		  <label>
			参与:
			<input
				name="isGoing"
			  type="checkbox"
			  checked={this.state.isGoing}
			  onChange={this.handleInputChange} />
		  </label>
		  <br />
		  <label>
			來賓人數:
			<input
			  name="numberOfGuests"
			  type="number"
			  value={this.state.numberOfGuests}
			  onChange={this.handleInputChange} />
		  </label>
		  <input type="button" onClick={this.handleClick} value="click"/>
		</form>
	  );
	}
}
export default Reservation;
```

注意我們使用了 ES6 的 [computed property name](https://snh90100.medium.com/es6-%E8%AA%9E%E6%B3%95-computed-property-names-%E5%8B%95%E6%85%8B%E8%A8%88%E7%AE%97%E5%B1%AC%E6%80%A7%E5%90%8D-%E4%BB%8B%E7%B4%B9-883ca789cda6) 語法來更新與輸入中的 name 相對應的 state key：

```text
this.setState({
  [name]: value
});
```

這和下面的 ES5 程式碼是一樣的：

```text
var partialState = {};
partialState[name] = value;this.setState(partialState);
```

