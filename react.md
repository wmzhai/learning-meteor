# 在Meteor中使用React

这里不讨论整个React的技术体系和基本知识，仅考虑那些把React作为Meteor项目前端时需要考虑的那些问题。

## React Components, Elements, and Instances
[React Components, Elements, and Instances](https://medium.com/@dan_abramov/react-components-elements-and-instances-90800811f8ca#.dq71idjw6)

An element is a plain object describing what you want to appear on the screen in terms of the DOM nodes or other components. Elements can contain other elements in their props.

A component can be two things. It can be a class with a render() method that inherits from React.Component . Or it can be a function. In both cases, it takes props as an input, and returns an element tree as the output.

Functional components don’t have instances at all. Class components have instances, but you never need to create a component instance directly—React takes care of this.


## UI Component和Data Fetching

在[Smart and Dumb Components
](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#.tzdjqf8nx)一文里面，Dan Abramov提出了Smart和Dumb Components的概念。有效的区分二者对于提高代码的可重用性非常关键。二者区分如下

### Dumb Components

* 对app其他部分无依赖 （这样才方便复用）
* 可以通过this.props.children包含内容
* 只通过props来获得数据和callback (不通过state)
* 有一个对应的css文件
* 没有自己的状态(不使用state)
* 可以使用其他的Dumb Component

### Smart Component

* 包含一个或者多个dumb或smart component
* 获取并保存数据并将数据以对象形式传递给dumb component
* 从来没有自己的CSS（不负责显示）
* 使用dumb component来做布局

### Container Component
另外一篇文章[Container Components](https://medium.com/@learnreact/container-components-c0e67432e005#.or87rchk2) 从另外一个角度说了这个问题。这篇文章的理念是：
用container components来做数据获取的工作，并将渲染的工作给对应的子component来做。这里的对应的含义是

```js
StockWidgetContainer => StockWidget
TagCloudContainer => TagCloud
PartyPooperListContainer => PartyPooperList
```
这样有助于数据的分离和组件的重用。

## Functional Stateless Components

[Functional Stateless Components in React 0.14](https://medium.com/@joshblack/stateless-components-in-react-0-14-f9798f8b992d#.sy7qnvdu3)

回顾一下Components的定义形式， 最早的形式如下

```js
var Text = React.createClass({
  render: function () {
    return <p>{this.props.children}</p>;
  }
});
React.render(<Text>Hello World</Text>, document.body);
```

当react 0.13发布以后，则可以用ES2015的class来定义，如下

```js
class Text extends React.Component {
  render() {
    return <p>{this.props.children}</p>;
  }
}
React.render(<Text>Hello World</Text>, document.body);
```

但是，如果Component并没有任何内部状态，这样extend class的方式定义就有些麻烦了。在React 0.14中，引入了无状态的函数模块(Stateless Functional Components)的定义方式来表示无状态的Dumb Components。 这些Components采用函数的方式来定义，并没有内部状态，只要给定同样的propos，则会返回同样的结果。如下定义：


```js
const Text = (props) =>
  <p>{props.children}</p>;

ReactDOM.render(
  <Text>Hello World</Text>,
  document.querySelector('#root')
);
```

## React-Komposor

在[Let’s Compose Some React Containers](https://voice.kadira.io/let-s-compose-some-react-containers-3b91b6d9b7c8#.vw3dz5b0u)里，我们看一下Arunoda的方法。
