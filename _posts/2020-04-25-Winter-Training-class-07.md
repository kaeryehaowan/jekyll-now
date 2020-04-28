## JavaScript | 语句，对象

### Grammar (语句)

- 简单语句
  - ExpressionStatement  
    `表达式语句，最主体的语句，所涉及到计算的语句，比如：a = 1 + 2;产生type为normal的CompletionRecord;如果写错，产生了运行时错误，会产生throw`
  - EmptyStatement  
    `空语句，可能就一个分号，比如：;产生type为normal的CompletionRecord;`
  - DebuggerStatement  
    `debugger语句，预留给调试的语句，比如：debugger;产生type为normal的CompletionRecord;`
  - ThrowStatement  
    `throw语句，比如：throw a;产生type为throw的CompletionRecord;`
  - ContinueStatement  
    `continue语句，比如：continue label;产生type为continue的CompletionRecord;`
  - BreakStatement  
    `break语句，比如：break label;产生type为break的CompletionRecord;`
  - ReturnStatement  
    `return语句，比如：return 1 + 2;产生type为return的CompletionRecord;`

- 组合语句
  - BlockStatement
  ```text
  {
      xxxx
  }
  ```  
  `只要是以大括号开头的语句，一定是BlockStatement，[[type]]:normal，[[value]]:--，[[target]]:--。BlockStatement有一个特殊的地方，如果BlockStatement里有某一个语句返回的是非normal结果的CompletionRecord的记录，那么这个BlockStatement就停掉了。否则就是一条条的顺序执行。`

  - IterationStatement  
    `switchStatement能消费target:label的completionRecord`
    - while  
      `如果while里面有return或throw,它的completionRecord就会变成return或throw。如果它里面的是continue或break，while语句就会把它消费掉,while里的语句会执行多次。`
    - do/while  
      `与while相差无几，只是语句多执行一次。`
    - for  
      `for的（1;2;3）里的1处可以放声明，var/const/let。`
    - for/in  
      `循环对象的属性。`
    - for/of  
      `可以循环数组，访问其中的元素。其实，它循环的是有迭代器的对象`
    - for/await/of  
      `可以与asyncGeneratorFunction配合使用`

  - IfStatement
  - SwitchStatement  
    `switchStatement能消费target:label的completionRecord`
  - WithStatement
  - LabelledStatement
  - TryStatement  
    `type: return的completionRecord。同时，try与if不一样，if后面的大括号可以省略，但是try后面的大括号不能省略。`

- 声明(Declaration)
  - FunctionDeclaration
  ```js
  // 函数声明一定要有名字
  function f() {}
  // 函数表达式，可以有名字也可以没名字，但是它不能出现在语句开头。
  var f = function () {};
  ```
  - GeneratorDeclaration
  ```js
  function* f() {
    yield 1;
    yield 2;
  }
  var f = function* () {
    yield 1;
    yield 2;
  };
  ```
  - AsyncFunctionDeclaration
  ```js
  // 异步函数
  async function f() {
    await new Promise();
  }
  ```
  - AsyncGeneratorDeclaration
  ```js
  // 异步generator函数
  async function* f() {
    yield 1;
    await new Promise();
  }
  ```
  - VariableStatement  
    `var声明在同一个作用域内，随便在哪个位置声明效果都一样，var行为一定要放到最前面，不然会加大维护者的成本`
  - ClassDeclaration  
    `class不能在声明之前使用，不能重复声明`
  - LexicalDeclaration  
    `let/const不能在声明之前使用，不能重复声明`

### Runtime (运行时)

`作用域与上下文的区别：作用域指的是你源代码的范围，上下文是指用户运行时内存里存变量的地方。`

- Completion Record (完成记录)
  - [[type]]: normal, break, continue, return, throw (返回类型)
  - [[value]]: Types (返回值)
  - [[target]]: label (返回 label)
  ```js
  // 魔法写法（挨打技巧）：
  function Cla() {
    pulish: 
      this.a = 1;
      this.b = 2;
    private: 
      var x = 3;
      var y = 4;
  }
  ```
- Lexical Enviroment (词法环境)
- Types/Object
  - 对象是独一无二，就算属性方法都完全一模一样，它也是独一无二的。任何一个对象都是唯一的，这与它本身的状态无关，我们用状态来描述对象，状态的改变就是行为。面向对象的语言里，只会是同一个对象，不可能出现两个一样的对象。同时，一定要注意一个原则，‘行为改变状态’。
  - 对象的本质，是状态(state)、行为(behavior)、唯一性(identifier)。
  - 封装、复用、解耦、内聚是架构层面的概念，解决代码的合理性。
  - 继承
  - 多态，描述动态性的程度，大体上是说一段同样的代码，执行的结果不一样。
  - Object——class  
    `类是一种常见的描述对象的方式。而'归类'和'分类'则是两个主要的流派。对于'归类'方法而言，多继承是非常常见的事情，比如C++。而采用'分类'思想的计算机语言，则是单继承结构。并且会有一个基类Object`
  - Object-Prototype  
    `原型是一种理接近人类原始认知的描述对象的方法。我们并不试图做严谨的分类，而是采用‘相似’这样的方式去描述对象。任何对象仅仅需要描述它自己与原型的区别即可。`
  - 在 JavaScript 运行时，原生对象的描述非常简单，我们只需关注对象的属性和原型两个部分。
    - 属性  
      `属性可以是多个，属性就是kv对，输入k就出v,k可以是symbol或string,v是data或accessor。data：[[value]]、writable是否可写、enumerable是否能够被forin、configurable是否可改特性设置；accessor：get取值器、set赋值器、enumerable是否能够被forin、configurable是否可改特性设置；`
    - 原型  
      `当我们访问一个属性时，如果当前对象没有，则会沿着原型找原型对象是否有，而原型对象也可能有自己的原型，因此会有‘原型链’的说法。这就保证了，对象只需要描述自己与原型的区别即可。`
  - Object API/Grammar
    - {}/[]/./Object.defineProperty 基本的对象能力
    - Object.create/Object.setPrototypeOf/Object.getPrototypeOf
    - new/class/extends
    - new/function/prototye
  - 特殊对象
    - 函数对象 (Function Object)  
      `函数对象除了属性和板型，它还有一个行为[[call]]，我们用function关键字、箭头运算符、Function构造器创建的对象，都会有[[call]]这个行为。我们可以用o()这样的语法，把对象当成函数调用时，会访问[[call]]这个行为。如果对象没有这个行为，则会报错。`
    - 数组对象 (Array Object)  
      `它有一个特殊的[[length]]`
    - 万物之始 (Object.prototype)  
      `这个特殊对象，是不能再给它设置原型的`
