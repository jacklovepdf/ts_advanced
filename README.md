# TypeScript

summary of ts

## Table of Contents

- [TypeScript](#typescript)
  - [Table of Contents](#table-of-contents)
  - [data types of ts](#data-types-of-ts)
  - [interface of ts](#interface-of-ts)

## data types of ts

1.TypeScript支持与JavaScript几乎相同的数据类型.

```typescript
    let isDone: boolean = false;
    let decLiteral: number = 6;
    let name: string = "bob";
    let list: number[] = [1, 2, 3];
    let list: Array<number> = [1, 2, 3]; //使用数组泛型

    // Declare a tuple type
    let x: [string, number]; //元组类型允许表示一个已知元素数量和类型的数组;
    // Initialize it
    x = ['hello', 10]; // OK
    // Initialize it incorrectly
    x = [10, 'hello']; // Error

    // 枚举类型, 默认情况下，从0开始为元素编号;
    enum Color {Red, Green, Blue}
    let c: Color = Color.Green;
    enum Color {Red = 1, Green, Blue}
    let colorName: string = Color[2];

    //任意类型, 不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查;
    let notSure: any = 4;
    notSure = "maybe a string instead";
    let list: any[] = [1, true, "free"];
    list[1] = 100;

    //空值,void类型像是与any类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是void
    function warnUser(): void {
        alert("This is my warning message");
    }

    // Null 和 Undefined
    // 默认情况下null和undefined是所有类型的子类型。 就是说你可以把null和undefined赋值给number类型的变量。
    // 然而，当你指定了--strictNullChecks标记，null和undefined只能赋值给void和它们各自。

    // Never

    // 类型断言
    // 通过类型断言这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不/// 进行特殊的数据检查和解构。
    // 类型断言有两种形式。 其一是“尖括号”语法：
    let someValue: any = "this is a string";
    let strLength: number = (<string>someValue).length;
    let someValue: any = "this is a string";
    let strLength: number = (someValue as string).length;
```

## interface of ts

1. TypeScript的核心原则之一是对值所具有的结构进行类型检查, 而接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。

(1) TypeScript具有ReadonlyArray<T>类型，它与Array<T>相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改。

(2) 最简单判断该用readonly还是const的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用const，若做为属性则使用readonly。

(3) 接口描述对象

```typescript
    interface Point {
        readonly x: number; //只读属性
        y: string //普通属性
        z?: string //可选属性

    }

    let a: number[] = [1, 2, 3, 4];
    let ro: ReadonlyArray<number> = a;
    ro[0] = 12; // error!
    ro.push(5); // error!
    ro.length = 100; // error!
    a = ro; // error!
```

(4) 接口描述函数

```typescript
    interface SearchFunc {
        (source: string, subString: string): boolean;
    }
    // 对于函数类型的类型检查来说，函数的参数名不需要与接口里定义的名字相匹配。
    let mySearch: SearchFunc;
    mySearch = function(src: string, sub: string): boolean {
        let result = src.search(sub);
        return result > -1;
    }
```

(5) 接口描述那些能够“通过索引得到”的类型
    字符串索引签名能够很好的描述dictionary模式，并且它们也会确保所有属性与其返回值类型相匹配。

```typescript
    interface StringArray {
        [index: number]: string;
    }

    let myArray: StringArray;
    myArray = ["Bob", "Fred"];

    let myStr: string = myArray[0];

    interface ReadonlyStringArray {
        readonly [index: number]: string;
    }
    let myArray: ReadonlyStringArray = ["Alice", "Bob"];
    myArray[2] = "Mallory"; // error!
```

(6) 类类型实现接口
//接口描述了类的公共部分，而不是公共和私有两部分;

```typescript
    interface ClockInterface {
        currentTime: Date;
        setTime(d: Date);
    }

    class Clock implements ClockInterface {
        currentTime: Date;
        setTime(d: Date) {
            this.currentTime = d;
        }
        constructor(h: number, m: number) { }
    }
```

(7) 类静态部分与实例部分的区别
//当一个类实现了一个接口时，只对其实例部分进行类型检查。

```typescript
    // 静态部分
    interface ClockConstructor {
        new (hour: number, minute: number): ClockInterface;
    }
    // 实例部分
    interface ClockInterface {
        tick();
    }

    function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
        return new ctor(hour, minute);
    }

    class DigitalClock implements ClockInterface {
        constructor(h: number, m: number) { }
        tick() {
            console.log("beep beep");
        }
    }
    class AnalogClock implements ClockInterface {
        constructor(h: number, m: number) { }
        tick() {
            console.log("tick tock");
        }
    }

    let digital = createClock(DigitalClock, 12, 17);
    let analog = createClock(AnalogClock, 7, 32);
```

(8) 继承接口
// 和类一样，接口也可以相互继承。 这让我们能够从一个接口里复制成员到另一个接口里，
// 可以更灵活地将接口分割到可重用的模块里。

```typescript
    interface Shape {
        color: string;
    }

    interface PenStroke {
        penWidth: number;
    }

    interface Square extends Shape, PenStroke {
        sideLength: number;
    }

    let square = <Square>{};
    square.color = "blue";
    square.sideLength = 10;
    square.penWidth = 5.0;
```

(9) 接口继承类
// 当接口继承了一个类类型时，它会继承类的成员但不包括其实现。
// 就好像接口声明了所有类中存在的成员，但并没有提供具体实现一样。
// 接口同样会继承到类的private和protected成员。
// 这意味着当你创建了一个接口继承了一个拥有私有或受保护的成员的类时，
// 这个接口类型只能被这个类或其子类所实现（implement）。

```typescript
    class Control {
        private state: any;
    }

    interface SelectableControl extends Control {
        select(): void;
    }

    class Button extends Control implements SelectableControl {
        select() { }
    }

    class TextBox extends Control {

    }

    // Error: Property 'state' is missing in type 'Image'.
    class Image implements SelectableControl {
        select() { }
    }

    class Location {

    }
```
