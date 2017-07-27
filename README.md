# 1.基本原则

每个文件只写一个组件，但是多个无状态组件可以放在单个文件中。


# 2.组件

2.1【强制】有内部状态组件使用ES6 Class写法。
```js
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
2.2 【推荐】没有内部状态，方法或者是无需对外暴露ref的组件，使用函数写法。

```js
// bad
class Listing extends React.Component {
  render() {
    return <div>{this.props.hello}</div>;
  }
}

// good
const Listing = ({ hello }) => (
  <div>{hello}</div>
);
```
# 3.PropTypes/DefaultProps

3.1 【强制】有状态组件，使用ES7类静态属性提案写法。

```js
static defaultProps = {
    welcome: {
      user: {}
    }
  };
  // propTypes 验证
  static propTypes = {
    welcome: PropTypes.object.isRequired,
    actions: PropTypes.shape({
      dispatch: PropTypes.func.isRequired,
      fetchUser: PropTypes.func.isRequired,
      startEntry: PropTypes.func.isRequired
    }).isRequired
  };
```
3.2 【强制】无状态组件，使用类静态属性写法。
```js
Icon.defaultProps = {
  type: ""
};
Icon.propTypes = {
  type: PropTypes.string.isRequired,
  style: PropTypes.object
};
```
3.3 【强制】PropTypes必须。

# 4.DisplayName

【推荐】为了调试方便，建议在组件最上面写displayName。
```js
// good
class A extends Component {
  static displayName = 'A';

  // ....
}
```

# 5.命名

> There are only two hard things in Computer Science: cache invalidation and naming things.-- Phil Karlton

5.1 【强制】react组件文件命名后缀为jsx，普通js文件为 js

5.2 【强制】组件的命名采用UpperCamel

```js

class Upload extends Component {}
or 
const Image = ({ success, src }) => {}
```
5.3 【推荐】命名要有意义，符合行业规范。

```
// good
priceCountReader      // No abbreviation.
numErrors             // "num" is a widespread convention.
numDnsConnections     // Most people know what "DNS" stands for.

// bad
nErr                  // Ambiguous abbreviation.
nCompConns            // Ambiguous abbreviation.
wgcConnections        // Only your group knows what this stands for.
pcReader              // Lots of things can be abbreviated "pc".
cstmrId               // Deletes internal letters.
kSecondsPerDay        // Do not use Hungarian notation.
```

5.4 【推荐】不要使用中国化的命名方式，注意相近词汇的使用场景。

```js
// bad
const educationCredential
// good
const postgraduateCertificate
```

# 6.属性

6.1【强制】JSX属性名总是使用驼峰式风格。

```js
// bad
<Foo UserName="hello" phone_number={12345678} />

// good
<Foo userName="hello" phoneNumber={12345678} />
```
6.2【建议】如果属性值为true, 可以直接省略。
```js
// bad
<Foo hidden={true} />

// good
<Foo hidden />
```
6.3【强制】数组中或者遍历中输出相同的React组件，属性key必需。

```js
// bad
[<Hello />, <Hello />, <Hello />];

data.map(x => <Hello>x</Hello>);

// good
[<Hello key="first" />, <Hello key="second" />, <Hello key="third" />];

data.map((x, i) => <Hello key={i}>x</Hello>);
```

# 7.方法

7.1【推荐】按照以下顺序排序内部方法。
```
1. static methods and properties
2. lifecycle methods: displayName, propTypes, contextTypes, childContextTypes, mixins, statics,defaultProps, constructor, getDefaultProps, getInitialState, state, getChildContext, componentWillMount, componentDidMount, componentWillReceiveProps, shouldComponentUpdate, componentWillUpdate, componentDidUpdate, componentWillUnmount (in this order).
3. custom methods
4. render method`
```


7.2【推荐】不要在componentDidMount以及componentDidUpdate中调用setState，除非是在绑定的回调函数中设置State。
```js
// bad
class Hello extends Component {
  componentDidMount() {
    this.setState({
      name: this.props.name.toUpperCase()
    });
  }
  render() {
    return <div>Hello {this.state.name}</div>;
  }
}

// good
class Hello extends Component {
  componentDidMount() {
    this.onMount(newName => {
      this.setState({
        name: newName
      });
    });
  }
  render() {
    return <div>Hello {this.state.name}</div>;
  }
}
```

7.3【推荐】使用箭头函数来获取本地变量。
```js
function ItemList(props) {
  return (
    <ul>
      {props.items.map((item, index) => (
        <Item
          key={item.key}
          onClick={() => doSomethingWith(item.name, index)}
        />
      ))}
    </ul>
  );
}
```



7.4【推荐】在React模块中，不要给所谓的私有函数添加_前缀，本质上它并不是私有的。

解释：_下划线前缀在某些语言中通常被用来表示私有变量或者函数。但是不像其他的一些语言，在JS中没有原生支持所谓的私有变量，所有的变量函数都是共有的。尽管你的意图是使它私有化，在之前加上下划线并不会使这些变量私有化，并且所有的属性（包括有下划线前缀及没有前缀的）都应该被视为是共有的。

```js
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

