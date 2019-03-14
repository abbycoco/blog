+++
title = "Flow"
date = 2019-03-14T16:50:06+08:00
tags = ["React"]
categories = ["React"]
draft = false
description = 'flow使用文档'
+++

# 安装

ONE: yarn add —dev babel-preset-flow

Two: 配置.babel 在presets 字段中添加 “flow”字段

THREE: yarn add —dev flow-bin

FOUR:设置eslint and webstorm , yarn add —dev eslint-plugin-flowtype

在webstorm 中设置，webstorm-preferences->Languages & Frameworks->JavaScript,在这个面板中设置JavaScript language version 为flow,设置flow 路径，选择项目下的node_modules/flow-bin/vender/flow

Tip:如果路径下没有可以在flow-bin下选择其他文件

选择之后点击apply或者ok即可。

FIVE: yarn run flow init 类似eslint生成.flowconfig文件

SIX：使用前最后一步配置， yarn run flow，如果报错，设置配置文件。

常规配置ignore，option.忽略node_modules下的一些文件,一些包文件使用了flow，但有不规范的地方。option 文件设置文件后缀。

[ignore]

.*/node_modules/draft-js/.*

[include]

[libs]

[lints]

[options]

module.file_ext=.scss

module.file_ext=.js

# 开始

> 开发者经常会将Flow和React搭配使用，因此Flow可以高效的输入常用的和高级的React模式变得非常重要。这篇指南将指导你如何使用Flow创建更安全的React应用。
>
> 在本指南中，我们将假定你了解React的基础知识并且着重考虑添加你已经熟悉的类型。我们将使用基于react-dom的例子，但是所有的模式也适用于其他环境像react-native。
>
> ## 在React中配置Flow
>
> Flow可以和Babel更好的搭配，因此作为一个使用了Babel的React用户您无须花费更多时间去适应Flow。如果你需要通过Babel安装Flow，你可以参考这篇指南。
>
> Babel跟Create React App亦可以开箱即用，仅需安装Flow并创建一个.flowconfig即可。

# 组件

##### 了解如何使用Flow键入React类组件和无状态功能组件

> 将Flow输入React组件有着难以置信的作用。如果你使用了Flow，Flow将静态地确保你将使用组件按照它被设计的模式。
>
> 在早期React库中提供了PropTypes去执行基础运行检查。Flow是更强大的，当你滥用组件时，它可以实时告诉你在不运行代码的情况下。
>
> 这有许多从Flow中生成的Babel插件，例如[[`babel-plugin-react-flow-props-to-prop-types`](https://github.com/thejameskyle/babel-plugin-react-flow-props-to-prop-types)],如果你希望Flow同时进行静态检查和运行时检查。
>
> ## 类组件
>
> 在我们展示如何在React组件中使用FLow时，让我们先展示一下如何写React组件搭配React的prop types。你需要继承React.Component并且增加一个静态propTypes 属性
>
> ```
> import React from 'react';
> import PropTypes from 'prop-types';
>
> class MyComponent extends React.Component {
>   static propTypes = {
>     foo: PropTypes.number.isRequired,
>     bar: PropTypes.string,
>   };
>
>   render() {
>     return <div>{this.props.bar}</div>;
>   }
> }
> ```

> 现在，我们将使用FLow写我们刚刚的组件
>
> ```
> import * as React from 'react';
>
> type Props = {
>   foo: number,
>   bar?: string,
> };
>
> class MyComponent extends React.Component<Props> {
>   render() {
>     this.props.doesNotExist; // Error! You did not define a `doesNotExist` prop.
>
>     return <div>{this.props.bar}</div>;
>   }
> }
>
> <MyComponent foo={42} />;
> ```
>
> 我们移除了对prop-types包的依赖并且增加了Flow对象取名Props跟Prop types拥有一样的结构但是使用了Flow的静态类型语法。然后我们传递新的Props 类型作为类型参数到React.Component
>
> 现在如果你使用<MyComponent/>带有string类型的参数foo替代数字类型你将看到一个报错。
>
> 现在无论何时在React组件中你使用this.props，FLow都将把它视为我们定义的Props类型。
>
> **注意：当你直接使用**
>
> ```
> extends React.Component<{ foo: number, bar?: string }>
> ```
>
> **在同一行的定义方式时，可以不用再次使用Props 类型。**
>
> **在这里，我们引入React作为命名空间**
>
> ```
> import * as React from 'react'
> ```
>
> **替代之前默认引入方式**
>
> ```
> import React from 'react'
> ```
>
> **当引入React作为Es模块时你可以使用任一样式，但是作为命名空间导入可以访问React的[[utility types](https://flow.org/en/docs/react/types)]**
>
> ```
> React.Component<Props, State>
> ```
>
> **通常带有两个参数，props和state，第二个参数是可选的。默认情况下它是undefined，你可以看到在上方例子中我们并没有引入State。我们将在后续章节中写更多State**
>
> ## 添加State

> 给你的React组件添加state类型然后创建一个新的对象类型在下面的例子中我们将它命名为State并把它作为第二个参数传递给React.Component
>
> ```
> import * as React from 'react';
>
> type Props = { /* ... */ };
>
> type State = {
>   count: number,
> };
>
> class MyComponent extends React.Component<Props, State> {
>   state = {
>     count: 0,
>   };
>
>   componentDidMount() {
>     setInterval(() => {
>       this.setState(prevState => ({
>         count: prevState.count + 1,
>       }));
>     }, 1000);
>   }
>
>   render() {
>     return <div>Count: {this.state.count}</div>;
>   }
> }
>
> <MyComponent />;
> ```
>
> 在上方例子中我们使用React setState()更新函数但你也可以将部分状态对象传递给setState()
>
> ## 使用默认值
>
> React支持defaultProps的概念，你可以把它看作默认函数参数。当你创建一个没有包含默认prop的元素，React将会defaultProps替换相应的prop。Flow也支持这个概念，在你的类中声明默认类型并增加static defaultProps属性。
>
> ```
> import * as React from 'react';
>
> type Props = {
>   foo: number, // foo is required.
> };
>
> class MyComponent extends React.Component<Props> {
>   static defaultProps = {
>     foo: 42, // ...but we have a default prop for foo.
>   };
> }
>
> // So we don't need to include foo.
> <MyComponent />
> ```
>
> Flow将会从static defaultProps中推断默认类型，所以在使用时你不需要添加过多的注释。
>
> **注意：你不需要在Props type中设置可以为空的属性，如果你又默认值，Flow将会确保foo是可选的。**
>
> ## 无状态功能组件
>
> 除了类之外，React还支持无状态功能组件。像键入组件一样键入一个方法：
>
> ```
> import * as React from 'react';
>
> type Props = {
>   foo: number,
>   bar?: string,
> };
>
> function MyComponent(props: Props) {
>   props.doesNotExist; // Error! You did not define a `doesNotExist` prop.
>
>   return <div>{props.bar}</div>;
> }
>
> <MyComponent foo={42} />
> ```
>
> ## 为方法组件设置默认值
>
> 在无状态组件中，React依旧支持默认值。跟类组件相似，默认值在无状态组件中生效不需要多余的注释。
>
> ```
> import * as React from 'react';
>
> type Props = {
>   foo: number, // foo is required.
> };
>
> function MyComponent(props: Props) {}
>
> MyComponent.defaultProps = {
>   foo: 42, // ...but we have a default prop for foo.
> };
>
> // So we don't need to include foo.
> <MyComponent />;
> ```
>
> **注意：你不需要在Props type中设置可以为空的属性，如果你又默认值，Flow将会确保foo是可选的。**
>
>

# 事件处理

#### 使用Flow键入React事件处理

> 在React文档中“事件处理”章节中对于如何处理事件提供了几种不同的方法。如果你正在用Flow，我们建议你使用最简单的静态类型即属性初始定义方法。属性初始定义方法就像这样
>
> ```
> class MyComponent extends React.Component<{}> {
>   handleClick = event => { /* ... */ };
> }
> ```
>
> 进行事件处理，你或许会用到SyntheticEvent<T>类型，像这样：
>
> ```
> import * as React from 'react';
>
> class MyComponent extends React.Component<{}, { count: number }> {
>   handleClick = (event: SyntheticEvent<HTMLButtonElement>) => {
>     // To access your button instance use `event.currentTarget`.
>     (event.currentTarget: HTMLButtonElement);
>
>     this.setState(prevState => ({
>       count: prevState.count,
>     }));
>   };
>
>   render() {
>     return (
>       <div>
>         <p>Count: {this.state.count}</p>
>         <button onClick={this.handleClick}>
>           Increment
>         </button>
>       </div>
>     );
>   }
> }
> ```
>
> 这依旧还有很多具体的异步事件类型像`SyntheticKeyboardEvent<T>`, `SyntheticMouseEvent<T>`, 或者 `SyntheticTouchEvent<T>`。该`SyntheticEvent<T>`类型都采取单一的类型参数。事件处理程序所在的HTML元素的类型。
>
> 如果你不想添加元素实例的类型，你也可以使用 `SyntheticEvent`与*无*类型的参数，像这样：`SyntheticEvent<>`。
>
> 注意：要获取对象实例，像上面例子中的HTMLButtonElement，经常容易犯的错误是使用`event.target` 代替 `event.currentTarget`。我们之所以使用event.currentTarget是源于 [event propagation](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Examples#Example_5:_Event_Propagation).使用`event.target` 或许会得到错误的对象。
>
> 注意：React使用它自身的事件系统，所以使用SyntheticEvent类型代替像Event、KeyboardEvent和MouseEvent等DOM类型是至关重要的。
>
> React提供的SyntheticEvent<T>以及相关的DOM事件：
>
> - `SyntheticEvent<T>` for [Event](https://developer.mozilla.org/en-US/docs/Web/API/Event)
> - `SyntheticAnimationEvent<T>` for [AnimationEvent](https://developer.mozilla.org/en-US/docs/Web/API/AnimationEvent)
> - `SyntheticCompositionEvent<T>` for [CompositionEvent](https://developer.mozilla.org/en-US/docs/Web/API/CompositionEvent)
> - `SyntheticInputEvent<T>` for [InputEvent](https://developer.mozilla.org/en-US/docs/Web/API/InputEvent)
> - `SyntheticUIEvent<T>` for [UIEvent](https://developer.mozilla.org/en-US/docs/Web/API/UIEvent)
> - `SyntheticFocusEvent<T>` for [FocusEvent](https://developer.mozilla.org/en-US/docs/Web/API/FocusEvent)
> - `SyntheticKeyboardEvent<T>` for [KeyboardEvent](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
> - `SyntheticMouseEvent<T>` for [MouseEvent](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent)
> - `SyntheticDragEvent<T>` for [DragEvent](https://developer.mozilla.org/en-US/docs/Web/API/DragEvent)
> - `SyntheticWheelEvent<T>` for [WheelEvent](https://developer.mozilla.org/en-US/docs/Web/API/WheelEvent)
> - `SyntheticTouchEvent<T>` for [TouchEvent](https://developer.mozilla.org/en-US/docs/Web/API/TouchEvent)
> - `SyntheticTransitionEvent<T>` for [TransitionEvent](https://developer.mozilla.org/en-US/docs/Web/API/TransitionEvent)
> - ​

# ref方法

## 如何使用Flow写React的ref方法

> React允许你通过ref方法去抓取一个对象或者一个组件的实例。要使用ref方法需要在你的类中增加一个可能的实例类型并将实例分配到ref方法的属性中。
>
> ```
> import * as React from 'react';
>
> class MyComponent extends React.Component<{}> {
>   // The `?` here is important because you may not always have the instance.
>   button: ?HTMLButtonElement;
>
>   render() {
>     return <button ref={button => (this.button = button)}>Toggle</button>;
>   }
> }
> ```
>
> ?HTMLButtonElement 中?非常重要，上方例子中第一个参数ref将会是HTMLButtonElement | null，如果组件未挂载，React将会回调ref为空。同时，MyComponent中button属性不会被设置直到React的render方法执行完。在此之前button的ref属性将是undefined。为了避免这种情况的出现，使用？可以避免bug.

# Children

## 学习如何使用Flow严格规范子组件类型

> React对象可能有0、1、或者多个Children。使用Flow书写Children可以使你构建强大的APIs。
>
> 通常来说，当为你的React组件增加children类型时第一个去尝试的应该是[`React.Node`](https://flow.org/en/docs/react/types/#toc-react-node).
>
> 像这样：
>
> ```
> import * as React from 'react';
>
> type Props = {
>   children?: React.Node,
> };
>
> function MyComponent(props: Props) {
>   return <div>{props.children}</div>;
> }
> ```
>
> 注意：为了获得 [`React.Node`](https://flow.org/en/docs/react/types/#toc-react-node)类型，你需要使用`import * as React from 'react'` 代替`import React from 'react'` 。我们在 [React Type Reference](https://flow.org/en/docs/react/types/).中解释了为什么需要这样写的原因。
>
> 但是，如果您想使用React children API做任何更强大的功能，那么您将需要强烈的直觉关于React如何处理Children。在继续帮助建立直觉之前，我们来看几个例子。
>
> 如果您已经对React Children的工作有了很强的直觉，那么请随时[跳到我们的示例，演示如何输入通常显示在React组件中的各种Children模式](https://flow.org/en/docs/react/children/#examples)。
>
> 我们的第一个实例是一个没有Children的对象:
>
> ```
> <MyComponent />;
>
> // which is the same as...
> React.createElement(MyComponent, {});
> ```
>
> 如果你创造一个没有Children元素时，访问`props.children`将不会设置。如果您尝试访问`props.children`，将返回undefined。
>
> 那么如果只有一个Children会发生什么呢？
>
> ```
> <MyComponent>{1}{2}</MyComponent>;
>
> // which is the same as...
> React.createElement(MyComponent, {}, 1, 2);
> ```
>
> 现在如果你传递两个值，props.children将会是一个数组。明确的说在这种情况下，`props.children` 将会是 `[1, 2]`.
>
> 多个children可能看起来是这样：
>
> ```
> <MyComponent>{'hello'} world</MyComponent>;
>
> // which is the same as...
> React.createElement(MyComponent, {}, 'hello', ' world');
> ```
>
> 这种情况下， `props.children` 将会是数组 `['hello', ' world']`.
>
> 或者
>
> ```
> <MyComponent>{'hello'} <strong>world</strong></MyComponent>;
>
> // which is the same as...
> React.createElement(
>   MyComponent,
>   {},
>   'hello',
>   ' ',
>   React.createElement('strong', {}, 'world'),
>
> ```

> 这种情况下， `props.children` 将会是数组 `['hello', ' ', <strong>world</strong>]`.
>
> 移到下一个问题。如果我们只有一个children并且它是数组将会发生什么？
>
> ```
> <MyComponent>{[1, 2]}</MyComponent>;
>
> // which is the same as...
> React.createElement(MyComponent, {}, [1, 2]);
> ```

>
>
> 这遵从了同样的规则，当你仅有一个children时，props.children将会是确切的值。尽管 `[1, 2]`是一个数组但它是单一的值，props.children将是确切的值。也就是说 `props.children`将是数组[1, 2]，并不是被包含的数组。
>
> 这种情况经常出现，当你在如下情况中使用array.map()时。
>
> ```
> <MyComponent>
>   {messages.map(message => <strong>{message}</strong>)}
> </MyComponent>
>
> // which is the same as...
> React.createElement(
>   MyComponent,
>   {},
>   messages.map(message => React.createElement('strong', {}, message)),
> );
> ```
>
> 一个单独的数组被单独使用，那么如果我们有多个children并且都是数组呢？
>
> ```
> <MyComponent>{[1, 2]}{[3, 4]}</MyComponent>;
>
> // which is the same as...
> React.createElement(MyComponent, {}, [1, 2], [3, 4]);
> ```

> 在这里props.children将会是数组中的数组，明确的说`props.children` 将会是 `[[1, 2], [3, 4]]`.
>
> React Children规则应该被铭记，如果你没有children，props.children将不会被设置，如果你仅有一个，props.children将会被设定为确切的值，如果你有两个或是多个，props.children将会是这些值的一个数组。
>
> 注意：注意空格！ 就像下方所示：
>
> ```
> <MyComponent>{42}  </MyComponent>
> ```

> 在有空格的情况下，等同于React.createElement(MyComponent, {}, 42, ' ')。为什么空格会被传递？是因为在这种情况下`props.children` 将会是[42, ' ']`而不是数字42.但是，如下是好的：
>
> ```
> <MyComponent>
>   {42}
> </MyComponent>
> ```

> 它将编译到你所期望的： `React.createElement(MyComponent, {}, 42)`。
>
> 换行符之后的换行符和缩进被删除,但使用严格模式时，请注意children周围的空格或许会改变children的值。
>
> 注意： 当心注释！如下
>
> ```
> <MyComponent>
>   // some comment...
>   {42}
> </MyComponent>
> ```

> 这等同于React.createElement(MyComponent, {}, '// some comment...', 42)。如何查看注释是否是对象的children？在这种情况下`props.children` 将会是 `['// some comment...', 42]`，包含了注释，使用JSX语法书写注释。
>
> ```
> <MyComponent>
>   {/* some comment... */}
>   {42}
> </MyComponent>
> ```

> 现在让我们看你如何使用直觉输入React组件多种多样的children
>
> ## 仅允许特定对象作为children
>
> 有时你仅希望一个特定组件作为children在你的React组件中。通常会在构建需要特定列子元素的表组件或者每个选项卡需要特定配置的选项卡栏时发生这种情况。使用此模式的一个这样的选项卡栏组件是React Native的`<TabBarIOS>`组件。
>
> [React Native的``组件](http://facebook.github.io/react-native/docs/tabbarios.html)仅允许React元素子元素，而这些元素*必须*具有组件类型`<TabBarIOS.Item>`。你应该使用`<TabBarIOS>`像：
>
> ```
> <TabBarIOS>
>   <TabBarIOS.Item>{/* ... */}</TabBarIOS.Item>
>   <TabBarIOS.Item>{/* ... */}</TabBarIOS.Item>
>   <TabBarIOS.Item>{/* ... */}</TabBarIOS.Item>
> </TabBarIOS>
> ```

> 使用时不允许执行以下操作`<TabBarIOS>`：
>
> ```
> <TabBarIOS>
>   <TabBarIOS.Item>{/* ... */}</TabBarIOS.Item>
>   <TabBarIOS.Item>{/* ... */}</TabBarIOS.Item>
>   <View>{/* ... */}</View>
>   <SomeOtherComponent>{/* ... */}</SomeOtherComponent>
>   <TabBarIOS.Item>{/* ... */}</TabBarIOS.Item>
> </TabBarIOS>
> ```

>
>
> 看看我们如何添加`<View>`和`<SomeOtherComponent>`作为孩子 `<TabBarIOS>`？这是不允许的，`<TabBarIOS>`会抛出一个错误。我们如何确保Flow不允许这种模式？
>
> ```
> import * as React from 'react';
>
> class TabBarIOSItem extends React.Component<{}> {
>   // implementation...
> }
>
> type Props = {
>   children: React.ChildrenArray<React.Element<typeof TabBarIOSItem>>,
> };
>
> class TabBarIOS extends React.Component<Props> {
>   static Item = TabBarIOSItem;
>   // implementation...
> }
>
> <TabBarIOS>
>   <TabBarIOS.Item>{/* ... */}</TabBarIOS.Item>
>   <TabBarIOS.Item>{/* ... */}</TabBarIOS.Item>
>   <TabBarIOS.Item>{/* ... */}</TabBarIOS.Item>
> </TabBarIOS>;
> ```

> 我们将设定props的好多React.ChildrenArray<React.Element<typeof TabBarIOSItem>>类型将会保证<TabBarIOS>必须有一个children那就是TabBarIOS.Item对象
>
> 我们的[各类参考](https://flow.org/en/docs/react/types/)有关于它们两者的信息 [`React.ChildrenArray`](https://flow.org/en/docs/react/types/#toc-react-childrenarray)和 [`React.Element`](https://flow.org/en/docs/react/types/#toc-react-element)。
>
> 注意：如果你想使用 `map()` 和 `forEach()` 这类的方法去处理 [`React.ChildrenArray`](https://flow.org/en/docs/react/types/#toc-react-childrenarray) 作为一个正常的JavaScript数组然后React可以提供
>
> [`React.Children` API](https://facebook.github.io/react/docs/react-api.html#react.children) 去做这些事情。它具有这样的功能`React.Children.toArray(props.children)`，您可以将其视为[`React.ChildrenArray`](https://flow.org/en/docs/react/types/#toc-react-childrenarray) 平面数组。
>
> ## 强制一个组件只能获得一个孩子。
>
> 有时你想强制你的组件*只*接收一个孩子。您可以使用[`React.Children.only()`函数](https://facebook.github.io/react/docs/react-api.html#react.children.only)来强制执行此约束，但您也可以在Flow中强制实施。为了做到这一点，你不会把孩子的类型包裹起来 [`React.ChildrenArray`](https://flow.org/en/docs/react/types/#toc-react-childrenarray)。像这样：
>
> ```
> import * as React from 'react';
>
> type Props = {
>   children: React.Element<any>,
> };
>
> function MyComponent(props: Props) {
>   // implementation...
> }
>
> // Not allowed! You must have children.
> <MyComponent />;
>
> // Not ok! We have multiple element children.
> <MyComponent>
>   <div />
>   <div />
>   <div />
> </MyComponent>;
>
> // This is ok. We have a single element child.
> <MyComponent>
>   <div />
> </MyComponent>;
> ```

> ## 关于输入方法和其他类型的children组件
>
> React 允许你传递任何值作为React组件的children。关于这个性能，这有许多创造性的用法比如使用一个如下所示的方法：
>
> ```
> <MyComponent>
>   {data => (
>     <div>{data.foo}</div>
>   )}
> </MyComponent>
> ```

> React-router 4要求一个方法作为 [<Router组件的Children](https://reacttraining.com/react-router/core/api/Route/children-func).你将提供一个方法作为children注入react-router像这样：
>
> ```
> <Route path={to}>
>   {({ match }) => (
>     <li className={match ? 'active' : ''}>
>       <Link to={to} {...rest}/>
>     </li>
>   )}
> </Route>
> ```

> 以下展示了如何使用Flow书写<Router>组件
>
> ```
> import * as React from 'react';
>
> type Props = {
>   children: (data: { match: boolean }) => React.Node,
>   path: string,
>   // other props...
> };
>
> class Route extends React.Component<Props> {
>   // implementation...
> }
>
> <Route path={to}>
>   {({ match }) => (
>     <li className={match ? 'active' : ''}>
>       <Link to={to} {...rest}/>
>     </li>
>   )}
> </Route>;
> ```

> Children 的类型是一个方法，传递许多对象类型并且返回 [`React.Node`](https://flow.org/en/docs/react/types/#toc-react-node) 是一个被React render任意类型值。一个children方法没有必要返回[`React.Node`](https://flow.org/en/docs/react/types/#toc-react-node)。它可以返回任意类型，但在这种情况下，`react-router`要呈现该`children` 函数返回的结果。
>
> 这种模式也不限于功能children。您也可以传递任意对象或类类型。
>
> ## 使用`React.Node`但没有一些原始类型，如字符串。
>
> [`React.Node`](https://flow.org/en/docs/react/types/#toc-react-node)是Children的一般类型，但有时您可能希望使用[`React.Node`](https://flow.org/en/docs/react/types/#toc-react-node)当排除一些原始类型，如字符串和数字。[例如，React Native <View> 组件](http://facebook.github.io/react-native/docs/view.html)执行此操作。
>
> [React Native <View>组件](http://facebook.github.io/react-native/docs/view.html)将允许任何原始值或任何React元素作为其子元素。但是，`<View>`不允许字符串或数字作为Children！您可以使用[`React.Node`](https://flow.org/en/docs/react/types/#toc-react-node)children类型`<View>`，但是[`React.Node`](https://flow.org/en/docs/react/types/#toc-react-node) 包括我们不想要的字符串`<View>`。所以我们需要创建自己的类型。
>
> ```
> import * as React from 'react';
>
> type ReactNodeWithoutStrings = React.ChildrenArray<
>
>   | void
>
>   | null
>
>   | boolean
>
>   | React.Element<any>
>
> ;
>
> type Props = {
>
>   children?: ReactNodeWithoutStrings,
>
>   // other props...
>
> };
>
> class View extends React.Component<Props> {
>
>   // implementation...
>
> }
> ```
>
> [`React.ChildrenArray`](https://flow.org/en/docs/react/types/#toc-react-childrenarray)是一种类型，为儿童建立了React嵌套数组数据结构。`ReactNodeWithoutStrings`用作[`React.ChildrenArray`](https://flow.org/en/docs/react/types/#toc-react-childrenarray)一个任意嵌套的null，boolean或React元素的数组。
>
> [`React.Element`](https://flow.org/en/docs/react/types/#toc-react-element)是类似于`<div/>`或的React元素的类型`<MyComponent/>`。值得注意的元素与组件不一样！
>
> > **注意：**如果想要像...一样`map()`和/ `forEach()`或处理 [`React.ChildrenArray`](https://flow.org/en/docs/react/types/#toc-react-childrenarray)一个正常的JavaScript数组的方法，那么React就提供了[`React.Children`API](https://facebook.github.io/react/docs/react-api.html#react.children)来做到这一点。它具有这样的功能`React.Children.toArray(props.children)`，您可以将其视为[`React.ChildrenArray`](https://flow.org/en/docs/react/types/#toc-react-childrenarray) 平面数组。

# 高阶组件

#### 学习如何使用Flow去规范React的高阶组件

高阶模式在React中很受欢迎，所以我们使用Flow提供有效的类型检查组件是很重要的。接下来之前，请确保你已经知道高阶组件或者去阅读 [React documentation on higher-order components](https://facebook.github.io/react/docs/higher-order-components.html) 。

学习如何进行高阶组件类型检查，我们将以 [Recompose](https://github.com/acdlite/recompose) 为例。 [Recompose](https://github.com/acdlite/recompose) 是一个提供很多高阶组件的React库。让我们看一下，你如何使用 [`mapProps()` higher-order component from Recompose](https://github.com/acdlite/recompose/blob/0ff7cf36f35e97dbd422a6924c7e7eddd47d0d34/docs/API.md#mapprops) 进行类型检查。

mapProps() 是一个将传入的props处理后输出的函数。你可以使用mapProps()想这样：

