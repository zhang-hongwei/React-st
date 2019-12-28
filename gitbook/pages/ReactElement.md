# ReactElement.js

## 保留属性

```js
const RESERVED_PROPS = {
  key: true,
  ref: true,
  __self: true,
  __source: true,
};
```

## hasValidRef，hasValidKey

> 判断key和ref是否存在

## defineKeyPropWarningGetter，defineRefPropWarningGetter

## ReactElement  返回一个$$typeof = REACT_ELEMENT_TYPE的对象

```js
 ReactElement(type, key, ref, self, source, owner, props)

 const element = {
    $$typeof: REACT_ELEMENT_TYPE,
    type: type,
    key: key,
    ref: ref,
    props: props,
    _owner: owner,
  };


```

1. 定义一个$$typeof=REACT_ELEMENT_TYPE的element对象
2. 返回element

## createElement

> 创建并返回指定类型的新 React 元素。其中的类型参数既可以是标签名字符串（如 'div' 或 'span'），也可以是 React 组件 类型 （class 组件或函数组件），或是 React fragment 类型。

```js

React.createElement(
  type,
  config,
  children
)

```

1. 判断config 是否存在ref和key，如果存在单独存储，ref和key不属于props
2. 提取RESERVED_PROPS中不存在的属性到props中
3. createElement从第三个参数开始都属于子元素，可以为1到多个，从第三个参数开始都被赋值给props.children
4. 处理type为class components的时候，如果存在defaultProps，此时需要判断props中是否包含defaultProps中的属性，如果没有需要从defaultProps中取
5. 执行ReactElement，返回一个React 元素

## createFactory

- 返回用于生成指定类型 React 元素的函数

```js
function createFactory(type) {
  const factory = createElement.bind(null, type);
  factory.type = type;
  return factory;
}
```

## cloneAndReplaceKey

## cloneElement

## isValidElement  

### 作用 : 验证对象是否为 React 元素，返回值为 true 或 false

```js

export function isValidElement(object) {
  return (
    typeof object === 'object' &&
    object !== null &&
    object.$$typeof === REACT_ELEMENT_TYPE
  );
}


```
