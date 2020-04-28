## JavaScript | 表达式，类型转换

### Expression (表达式)

- Atom
  - Grammar
    - Tree vs Priority  
        1. 优先级是用表达式生成树来实现
        2. new.target 是给 fun 用的，只能在函数里用
        3. Object.setProtyotypeOf()
        4. new Foo() 与 new Foo 不一样，new Foo()优先级高于 new Foo;
    - Left hand side & Right hand side
  - Runtime
    - Type Convertion
- Expressions
  - [资料](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)
  - Member (成员访问)/Left hand side
    - a.b
    - a[b]
    - foo\`string`
    - super.b
    - super[b]
    - new.target
    - new Foo()
  - New /Left hand side
    - new Foo
  - Call /Left hand side
    - foo()
    - super()
    - foo()['b']
    - foo().b
    - foo()\`abc`
  - Update /Right hand side
    - a ++
    - a --
    - ++ a
    - -- a
  - Unary
    - delte a.b
    - void foo()
    - typeof a
    - \- a
    - \+ a
    - ~ a
    - ! a
    - await a
  - Exponental /Right hand side 乘方
    - \*\*
  - Multiplicative 乘法
    - \* / %
  - Additive 加法
    - \- +
  - Shift 左右移位
    - << 、>> 、>>>
  - Relationship 比较
    - < 、> 、<= 、>= 、instanceof 、in
  - Equality
    - ==
    - !=
    - ===
    - !==
  - Bitwise
    - & ^ |
  - Logical
    - &&
    - ||
  - Conditional
    - ? :
- Boxing & Unboxing
  - ToPremitive
  - toString vs valueOf
  - Object 带 new 与不带 new 是一样的  
    `Object() 与 new Object()是一样的结果`
  - Symbol 不能用 new,只能 Symbol()  
    `可以用Object(Symbol('a'))，这样装箱。然后Object.getPrototypeOf(Object(Symbol('1'))) === Symbol.prototype为true，Object(Symbol('1')) instanceof Symbol也是为true。(function(){return this}).apply(Symbol('2'))这样也是能装箱。`
  - Boxing  
    `装箱就是把string、number、boolean、symbol这4种类型转成对应的对象，就是装箱。`
  - Unboxing  
    `折箱就是取对象的基本值，比如 1 + {},优先使用[Symbol.toPrimitive]，如果没有再优先使用valueOf，如果没有再使用toString方法`
- Exercise
  - StringToNumber
  - NumberToString

```js
// 取符号方法
function sign(number) {
  if (number !== 0 && !number) return undefined;
  if (number === 0) {
    return 1 / number === Infinity
      ? 1
      : 1 / number === -Infinity
      ? -1
      : undefined;
  }
  return number / Math.abs(number);
}

{
  var name = "winter";
  function foo() {
    console.log(arguments);
  }
  foo`hello ${name}!`; // 类似  foo(['hello ', '!'], 'winter', xxx, xxx)
}

{
  function cls1(s) {
    console.log(s);
  }
  function cls2(s) {
    console.log(2, s);
    return cls1;
  }
  new new cls2("good")();
}
```
