# Airbnb React/JSX Style Guide

*用更合理的方式书写React和JSX*

## 目录
  1. [基本规则](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [命名](#naming)
  1. [声明](#declaration)
  1. [对齐](#alignment)
  1. [引号](#quotes)
  1. [空格](#spacing)
  1. [属性](#props)
  1. [括号](#parentheses)
  1. [标签](#tags)
  1. [方法](#methods)
  1. [排序](#ordering)
  1. [`isMounted`](#ismounted)

## 基本规则

  - 每个文件只包含一个React组件；
    - 但是[无状态, 或者 Pure 组件](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) 允许一个文件包含多个组件。eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - 始终使用 JSX 语法;
  - 不要使用 `React.createElement`方法，除非初始化 app 的文件不是 JSX 格式。

## Class vs `React.createClass` vs stateless

  - 如果组件拥有内部的 state 或者 refs 的时，更推荐使用 `class extends React.Component`，除非你有一个非常好的理由要使用 mixin。 eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md)

    ```javascript
    // bad
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // good
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    如果组件没有拥有内部的 state 或者 refs，那么普通函数 (不要使用箭头函数) 比类的写法更好：
    ```javascript

    // bad
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // bad (因为箭头函数没有“name”属性)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // good
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## 命名

  - **扩展名**：React 组件使用`.jsx`扩展名；
  - **文件名**：文件名使用帕斯卡命名。 例如： `ReservationCard.jsx`。
  - **引用命名**：React 组件使用帕斯卡命名，引用实例采用驼峰命名。 eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)
    ```javascript
    // bad
    import reservationCard from './ReservationCard';

    // good
    import ReservationCard from './ReservationCard';

    // bad
    const ReservationItem = <ReservationCard />;

    // good
    const reservationItem = <ReservationCard />;
    ```

  - **组件命名**：组件名称应该和文件名一致， 例如： `ReservationCard.jsx` 应该有一个`ReservationCard`的引用名称。 但是， 如果是在目录中的组件， 应该使用 `index.jsx` 作为文件名 并且使用文件夹名称作为组件名：
    ```javascript
    // bad
    import Footer from './Footer/Footer';

    // bad
    import Footer from './Footer/index';

    // good
    import Footer from './Footer';
    ```

## 声明

  - 不要使用｀displayName｀属性来命名组件，应该使用类的引用名称。
    ```javascript
    // bad
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // good
    export default class ReservationCard extends React.Component {
    }
    ```

## 对齐

  - 为 JSX 语法使用下列的对其方式。eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```javascript
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // 如果组件的属性可以放在一行就保持在当前一行中
    <Foo bar="bar" />

    // 多行属性采用缩进
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>
    ```

## 引号

  - JSX 的属性都采用双引号，其他的 JS 都使用单引号。eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

  > 为什么这样做？JSX 属性 [不能包含转义的引号](http://eslint.org/docs/rules/jsx-quotes), 所以当输入`"don't"`这类的缩写的时候用双引号会更方便。

  > 标准的 HTML 属性通常也会使用双引号，所以 JSX 属性也会遵守这样的约定。

    ```javascript
    // bad
    <Foo bar='bar' />

    // good
    <Foo bar="bar" />

    // bad
    <Foo style={{ left: "20px" }} />

    // good
    <Foo style={{ left: '20px' }} />
    ```

## 空格
  - 终始在自闭合标签前面添加一个空格。
    ```javascript
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

## 属性
  - 属性名称始终使用驼峰命名法。
    ```javascript
    // bad
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // good
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```
  - 当属性值等于`true`的时候，省略该属性的赋值。 eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```javascript
    // bad
    <Foo
      hidden={true}
    />

    // good
    <Foo
      hidden
    />
    ```

## 括号

  - 用括号包裹多行 JSX 标签。 eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)

    ```javascript
    // bad
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // good, when single line
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## 标签

  - 当标签没有子元素时，始终时候自闭合标签。 eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)
    ```javascript
    // bad
    <Foo className="stuff"></Foo>

    // good
    <Foo className="stuff" />
    ```

  - 如果控件有多行属性，关闭标签要另起一行。 eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```javascript
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## 方法

  - 在 render 方法中事件的回调函数，应该在构造函数中进行bind绑定。 eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

  > 为什么这样做? 在 render 方法中的 bind 调用每次调用 render 的时候都会创建一个全新的函数。

    ```javascript
    // bad
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // good
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

  - React 组件的内部方法命名不要使用下划线前缀。
    ```javascript
    // bad
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // good
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

## 排序

  - `class extends React.Component`的顺序：

  1. `static`静态方法
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *点击回调或者事件回调* 比如 `onClickSubmit()` 或者 `onChangeDescription()`
  1. *`render`函数中的 getter 方法* 比如 `getSelectReason()` 或者 `getFooterContent()`
  1. *可选的 render 方法* 比如 `renderNavigation()` 或者 `renderProfilePicture()`
  1. `render`

  - 怎样定义 `propTypes`, `defaultProps`, `contextTypes`等

    ```javascript
    import React, { PropTypes } from 'react';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - `React.createClass`的排序：eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *点击回调或者事件回调* 比如 `onClickSubmit()` 或者 `onChangeDescription()`
  1. *`render`函数中的 getter 方法* 比如 `getSelectReason()` 或者 `getFooterContent()`
  1. *可选的 render 方法* 比如 `renderNavigation()` 或者 `renderProfilePicture()`
  1. `render`

## `isMounted`

  - 不要使用 `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > 为什么这样做? [`isMounted`是一种反模式][反模式]，当使用 ES6 类风格声明 React 组件时该属性不可用，并且即将被官方弃用。

  [反模式]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

**[⬆ back to top](#table-of-contents)**
