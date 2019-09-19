---
title: JavaScript 设计模式基础（一）
date: 2019-07-08 13:13:34
categories:
- design patterns
tags:
- Pattern intro
---

# 什么是设计模式？

> `模式` 起源于建筑学。20世纪70年代，哈佛大学建筑学博士`Christopher Alexander`和他的团队花大约20年，来研究为解决同一个问题而设计出的不同建筑结构，从中发现那些高质量设计中的相似性，并且用`模式`来指代这种相似性；

受`Christopher Alexander`观点的启发，`Erich Gamma, Richard Helm, Ralph Johnson和 John Vlissides `(人称Gang Of Four, GoF),将这种 `模式` 观点应用于面向对象的软件设计中，并总结了23种常见的软件开发设计模式， 于 1995 年发布一本名叫《Design Patterns: Elements Of Reusable Object-Oriented Software》的书

> 设计模式的定义：在面向对象软件设计过程种针对特定的问题的简洁而优雅的解决方案。就是给面向对象软件开发中一些 好的设计取个名。

## 为什么要给好设计取名？

在开发中，一些稍有经验的程序员也许在不知不觉中使用过设计模式，于是他向别人描述它时会遇到困难，沟通成本高、效率低。而 GoF 将这些好的设计从面向对象中挑选出来，这些都是久经考验的反应了开发者的经验和见解的使用模式来定义的技术，给它们一个好听又好记的名字，这样就方便我们更好的传播和学习，当遇到一个问题时，知道这是哪种设计模式，就能很快想出几种解决方案，提高工作效率。

**好处：**
1. 提供固定的解决方法来解决在软件开发中出现的问题。
2. 很容易地重用，防止在应用程序开发过程中出现的一些可能导致重大问题的小问题，提高工作效率。
3. 避免重复代码来减小我们文件大小。
3. 模式善于表达，交流更快速，降低沟通成本。


 **所有设计模式的实现都遵循一条原则：找出程序中变化的地方，将变化封装起来**。一个程序的设计总是可以分为 可变部分和不变的部分；找出可变部分，将其封装，剩下的不变和稳定部分就非常容易复用。
**作用：** 让人写出可复用和可维护的程序。
JavaScript是一门`面向对象语言`[[1]](https://www.ibm.com/developerworks/cn/web/1304_zengyz_jsoo/)[[2]](https://www.infoq.cn/article/3*8POPcRSClQh1Cp9Sqg)[[3]](https://zhuanlan.zhihu.com/p/33658346)，设计模式通过对面向对象的特征<label>**封装、继承、组合、多态**</label>等技术的反复使用，提炼出可复用的面向对象设计技巧。

---

# 设计模式基础
> JavaScript 在语言层面没有抽象类和接口的支持，没有类式继承，而是通过原型委托的方式来实现对象与对象之间的继承。

## 编程语言类型

**编程语言按数据类型大体可分两大类：静态类型语言和动态类型语言** 。静态类型语言在编译时就已经确定变量的类型，动态类型语言只有在程序运行的时候才能确定变量的类型。而JavaScript就是动态类型语言。

**静态语言优点：**
1. 编译时就能发现类型不匹配的错误，可以避免程序运行时有可能发生的错误。
2. 编译器可针对对应变量的类型进行优化，提高程序运行速度。

**静态语言缺点：**
1. 迫使程序员按照对应的规则来写程序，为每个变量规定数据类型分散程序员注意力，增加代码量。

**动态语言优点：**
1. 代码简洁，程序员可以把更多精力放在业务逻辑处理上，更加专注
2. 无需类型检测，灵活性高，只关注须对象的行为，无需关注对象本身
3. 不必借助超类型来实现"面向接口编程"

**动态语言缺点：**
1. 无法保证变量的类型，程序可能发生意想不到的bug.

## 面向对象的特征(参考JAVA)

**多态、继承、封装**


### 多态

>定义：同一操作作用于不同对象，产生不同的解释和不同的执行结果

给不同对象发送同一个消息时，这些对象会根据这个消息分别给出不同的回应。

```javascript
var sendInfo = target => {
    if ( target instanceof ObjOne) {
        console.log(new ObjOne)
    }else if (target instanceof ObjTwo) {
        console.log(new ObjTwo)
    }
}
 function ObjOne () {}
 function ObjTwo () {}

 sendInfo(new ObjOne())
 sendInfo(new ObjTwo())

```
上述代码段就体现了`多态性`,当发送 `sendInfo` 消息时每个对象做出不同的回应，但是这样的`多态性`很糟糕，如果再这个家一个对象 ObjThere 就得改动代码，对象越来越多时，**修改的代码越多，出错的可能性越大。**

多态背后的思想是将 `做什么` 和 `谁去做以及怎样做`分开，也就是将 `不变的事物和可能变化的事` 分开。上述代码段中，每个对象都会打印日志,这是不变的，而各个对象输出哪些信息是变化的，将不变的隔离出来，把变化的封装起来，这样程序就又了扩展能力，是可生长的，这样就符合 `开发-封闭` 原则。
**下面是修改后的代码：**

```javascript
let sendInfo = target => {
  target.log()
}
 function ObjOne () {}
ObjOne.prototype.log = function(){console.log(this)};

function ObjTwo () {}
ObjTwo.prototype.log = function(){console.log(this)};

 sendInfo(new ObjOne())
 sendInfo(new ObjTwo())

```
现在我们增加一个对象就不需要修改 `sendInfo`里的代码了

```javascript
function ObjThere () {}
ObjThere.prototype.log = function(){console.log(this)};
sendInfo(new ObjThere())
```


#### 类型检查和多态

类型检查是再表现出对象多态性之前一个绕不开的话题，`JavaScript` 是一门不必进行类型检查的动态类型语言，为了真正了解多态的目的，须从静态语言说起。

以JAVA为例，由于在编译时会进行类型检查，所有不能给变量赋予不同类型的值。
```Java
String str;

str = 'ab'
str =2 // error
```
将之前的例子换成 Java

```Java
public class ObjOne {
    public void log () {
        System.out.println('a')
    }
}
public class ObjTwo {
    public void log () {
        System.out.println('b')
    }
}
public class SendInfo {
    public void log (ObjOne objOne) {
      objOne.log()
    }
}
public class Test {
    public static void main (String args[] ) {
        SendInfo sendInfo = new SnedInfo();
        ObjOne objOne = new ObjOne()
        sendInfo(objOne) // 'a'
        ObjTwo objTwo = new objTwo()
        sendInfo(objTwo) // error
    }
}
```

上述代码段中 `sendInfo(objOne)` 可以顺利输出，而`sendInfo(objTwo)`因为传入的类不同而报错，为了解决这一问题，静态类性的面向对象通常被设计为可向上转型：**当给一个类变量赋值时，这个变量的类型既可以用这个类本身，也可以用这个类的超类** 就像描述一只咖啡猫在跑、一只波斯猫在跑，如果忽略它们具体类型，可以说 一只猫在跑。

同理，当 ObjOne 和 ObjTwo 对象的类型都被隐藏在超类 Objects 身后时， ObjOne 和 ObjTwo就能被交换使用，这就让对象表现出多态性，这种表现正是实现众多设计模式的目标。
**要实现多态归根结底得先要消除类型之间的耦合关系。一个JavaScript对象即可表示`ObjOne`又可表示`ObjThere`,这意味着JavaScript对象的多态性是与生俱来的**

#### 多态在面向对象程序设计中的作用
`Martin Fowler` 在重构一书中写到：
>   多态的最根本好处在于，你不必再向对象询问“你是什么类” 而后根据得到的答案调用对象的某个行为————你只管调用该行为就是了，其他的一切多态机制都会为你安排妥当。

**多态最根本的作用就是把过程化的条件分支语句转化为对象的多态性，从而消除条件分支语句**

就好比如在电影拍摄现场，导演喊出“action”时，每个人都做自己应该做的事，而不用导演走到每个人面前确认他们的职责，然后告诉他们该做什么。**对象应该做什么并不是临时决定的，而是事先约定排练好的，每个对象该做什么，已经成为该对象的一个方法，被安装在对像内部，每个对像负责自己的行为，然后这些对象通过同一个消息，有条不紊的工作**
将行为分布在各个对象中，让他们各自负责自己的行为，这便是面向对象设计的优点。


### 继承

**使用现有类的所有功能，并在无需重新编写原来的类的情况下对这些功能进行扩展。**通过继承创建的新类称为“子类”或“派生类”,被继承的类称为“基类”、“父类”或“超类”。继承的过程，就是从一般到特殊的过程。
> 使用继承来得到多态效果,是让对象表现出多态性最常用的手段。继承通常包括**实现继承(使用基类的属性和方法而无需额外编码的能力)和接口继承( 接口继承是指仅使用属性和方法的名称、但是子类必须提供实现的能力)和可视继承(子窗体（类）使用基窗体（类）的外观和实现代码的能力)**

**注意：** 使用继承时,两个类之间的关系应该是“属于”关系。

将上述JAVA例子进一步封装

```Java
public abstract class Objects { //抽象出一个超类
    abstract void log();
}
public class ObjOne extends Objects {
    public void log () {
        System.out.println('a')
    }
}
public class ObjTwo extends Objects {
    public void log () {
        System.out.println('b')
    }
}


public class SendInfo { //让 log 方法接收 Objects 类
    public void log (Objects objects) {
      objects.log()
    }
}
public class Test {
    public static void main (String args[] ) {
        SendInfo sendInfo = new SnedInfo();
        Objects  objOne = new ObjOne();
        Objects  objTwo = new objTwo();
        sendInfo(objOne) // 'a'
        sendInfo(objTwo) // 'b'
    }
}
```
上述代码段中 ObjOne 和 ObjTwo 继承自 Objects 类即可通过这个桥梁来使用对应的方法。


## 封装
封装是实现面向对象程序设计的第一步，将数据或函数等集合在一个个单元中（在java中称之为类，JavaScript中就是一个函数对象）
> 封装是隐藏数据、实现细节、设计细节以及对象的类型等，是代码模块化；是把过程和数据包围起来，只能通过已定义的方法访问数据。**把客观事物封装成抽象的类，并且只对可信的类或对象来操作这个类的数据和方法，而对不可信的对象进行信息隐藏**

**封装的意义：**
1. 保护数据成员，不然其他对象或类直接访问或修改，只能通过提供的接口访问，防止用户无意破坏（数据封装）
2. 方法的细节对外是隐藏的，只要接口不变，内容的修改不会影响到外部的调用这（封装实现）


### 封装数据

在许多语言的对象系统中，封装数据是由语法解析来实现的比如 JAVA 提供了 public 、private、protected等关键字来设置不同的访问权限。

**JavaScript 中并没有提供这些关键字，只能依赖作用域来实现封装特性，而且只能模拟出 public、和private**
```javascript
let myObject=(()=>{
    let __name = "owen"; // private
    return {  // publice 对象
        getName(){
            return __name
        },
        setName(value){
            return __name = value
        }
    }
})()
myObject.getName() // "owen"
myObject.setName('gao')
myObject.getName() // "gao"

```
ES6 中除了 let 、const 外还可使用 Symbol 类型建立私有属性

### 封装实现

封装使对象内部的变化对其他对象或类而言是透明不可见的，对象对他自己的行为负责，其他对象或类不用关心他内部的实现，对象之间只通过暴露 API接口来通信。

比如 Array中的forEach 方法遍历一个聚合对象，我们不用关心 forEach 内部是争议实现的，只要它提供的功能正确就行，即使修改它内部的代码，只要调用方式没有变化就不用关系它内部实现的改变

```javascript
let arr = [1,2,3]
arr.forEach(val => console.log(val))
```

封装在更重要的层面体现为封装变化《设计模式》一书曾提到：
> 考虑你的设计中哪些地方可能变法，这种方式与关注会导致重新设计的原因相反。它不是考虑什么时候会迫使你的设计改变，而是考虑你怎么样才能够在不重新设计的情况下进行改变。这里的关键在于封装发送变化的概念，这是许多设计模式的主题

《设计模式》一书中归纳总结了23种设计模式，从意图上可将这些模式划分为 `创建型模式`、`结构型模式`和`行为型模式`。

通过封装变化的方式，把系统中稳定不变的部分和容易变化的部分隔离开来，在系统演变过程中，只需替换哪些容易变化的部分，如果这些部分是已经封装好的，替换起来也相对容易；这样可以很大程度的保证程序的稳定性或可扩展性。

## 面向对象基本原则

### 单一职责原则（Single Responsibility Prunciple）

> 一个类只允许有一个职责，即只有一个导致该类变更的原因。

简单来说一个类只专注做一件事。并不是说一个类只有一个函数，而是说这个类中的函数所做的工作必须高度相关（高内聚）

**不过这个原则很容易违背，因为可能由于某种原因，原来功能单一的类需要被细化成颗粒更小的职责1跟职责2，不过这个拆的粒度可能因人而已，有时候并不需要拆的过细，不要成了为设计而设计。所以在每次迭代过程中可能需要重新梳理重构之前编写的代码，将不同的职责封装到不同的类或者模块中。**

**优点：**
1. 类的复杂性降低，实现什么职责都有清晰明确的定义,可读性提高，可维护性提高；
2. 变更引起的风险降低，变更是必不可少的，如果接口的单一职责做得好，一个接口修改只对相应的实现类有影响，对其他的接口无影响，这对系统的扩展性、维护性都有非常大的帮助。


### 开发关闭原则（Open Closed Principle）

> 一个软件实体应该是可以扩展的，但是不可修改。

**在软件开发过程中，永远不变的就是变化。因此当软件需要变化时，我们应该尽量通过扩展的方式来实现变化，而不是通过修改已有的代码来实现。封装变化，是实现开放封闭原则的重要手段，对于经常发生变化的状态，一般将其封装为一个抽象，拒绝滥用抽象，只将经常变化的部分进行抽象。**
**优点：**
1. 增加稳定性。
2. 可扩展性高。

### 里氏替换原则 （Liskov Substitution Principle）

> 子类必须能够替换掉它们的基类，而程序执行效果不变。

**所有使用基类代码的地方，如果换成子类对象的时候还能够正常运行，则满足这个原则，否则就是继承关系有问题，应该废除两者的继承关系，这个原则可以用来判断我们的对象继承关系是否合理。**尽量把父类设计为抽象类或者接口，让子类继承父类或实现父接口，并实现在父类中声明的方法.

**优点：**
1. 提高代码的重用性；
2. 代码共享，减少创建类的工作量，每个子类都拥有父类的方法和属性；
3. 提高代码的可扩展性，实现父类的方法就可以“为所欲为”了，很多开源框架的扩展接口都是通过继承父类来完成的；
4. 可以用来判断我们的对象继承关系是否合理,约束继承在使用上的泛滥。

**缺点：**
1. 降低代码的灵活性。子类必须拥有父类的属性和方法，让子类自由的世界中多了些约束；
2. 增强了耦合性。当父类的常量、变量和方法被修改时，必需要考虑子类的修改，而且在缺乏规范的环境下，这种修改可能带来非常糟糕的结果——大片的代码需要重构。

### 依赖倒置原则 （Dependence Inversion Principle）
>高层模块不应该依赖低层模块，两者都应该依赖其抽象；抽象不应该依赖细节；细节应该依赖抽象。

我们经常说“针对接口编程”，这里的接口就是抽象，我们应该依赖接口，而不是依赖具体的实现来编程。
假设B是较A低的模块，但B需要使用到A的功能，这个时候，B不应当直接使用A中的具体类；而应当由B定义一抽象接口，并由A来实现这个抽象接口，B只使用这个抽象接口；这样就达到了依赖倒置的目的，B也解除了对A的依赖，反过来是A依赖于B定义的抽象接口。通过上层模块难以避免依赖下层模块，假如B也直接依赖A的实现，那么就可能造成循环依赖

**高低应该是从开发者当前的角度来看的，不过DIP原则从不同角度来看它都适合且需要被遵守。假如我们高层模块直接依赖于底层模块，带来的后果是每次底层模块改动，高层模块就会受到影响，整个系统就变得不稳定，这也违反了开放关闭原则。**

 **通常我们会通过引入中间层的方式来解决这个问题，这个中间层相当于一个抽象接口层，高层模块和底层模块都依赖于这个中间层来交互，这样只要中间抽象层保持不变，底层模块改变不会影响到高层模块，这就满足了开放关闭原则；而且假如高层模块跟底层模块同时处于开发阶段，这样有了中间抽象层之后，每个模块都可以针对这个抽象层的接口同时开发，高层模块就不需要等到底层模块开发完毕才能继续了。**

**优点：** 通过抽象来搭建框架，建立类和类的关联，以减少类间的耦合性。而且以抽象搭建的系统要比以具体实现搭建的系统更加稳定，扩展性更高，同时也便于维护。


### 接口隔离原则 （Interface Segregation Principle）
> 多个特定的客户端接口要好于一个通用性的总接口。

**应当为客户端提供尽可能小的单独的接口，而不是提供大的总的接口。**

**需要注意的是：**接口的粒度也不能太小。如果过小，则会造成接口数量过多，使设计复杂化。
**优点：** 避免同一个接口里面包含不同类职责的方法，接口责任划分更加明确，符合高内聚低耦合的思想。
接口是设计时对外部设定的约定，通过分散定义多个接口，可以预防外来变更的扩散，提高系统的灵活性和可维护性。

### 迪米特法则 （Law Of Demeter） 或 最少知识原则（Least Knowledge Principle）

> 一个对象应该对其他对象有最少的了解;一个类应该对自己需要耦合或调用的类知道得最少。
**类的内部如何实现、如何复杂都与调用者或者依赖者没关系，调用者或者依赖者只需要知道他需要的方法即可，其他的我一概不关心。类与类之间的关系越密切，耦合度越大，当一个类发生改变时，对另一个类的影响也越大。**
**优点：**
1. 降低复杂度；降低耦合度；增加稳定性。
2. 减少类与类之间的关联程度，让类与类之间的协作更加直接。