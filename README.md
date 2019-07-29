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

