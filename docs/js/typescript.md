> 清晰的函数参数 / 接口属性, 静态检查, 生成 api 文档, 配合编辑器各种提示

> 原始类型: boolean, number, string, void(空值, 没返回值), null, undefined, any(任意类型), 元组

> 类型推论

> 联合类型: |

> 对象的类型——接口

```
  interface Person {
    readonly id: number; // 只读属性
    name: string;
    age?: number; // 可选属性
    [propName: string]: any; // 任意属性
  }
```

> 数组类型: 「类型 + 方括号」

> 函数类型:

```
  用接口定义函数的现状
  interface SearchFunc {
    (source: string, subString: string): boolean;
  }
  可选参数
  参数默认值
  剩余参数
  function sum(x?: number, y: number = 1, ...item): number {
    return x + y
  }
```

> 类型断言(手动指定一个值的类型):

```
  function getLength(something: string | number): number {
    if ((<string>something).length) {
      return (<string>something).length;
    } else {
      return something.toString().length;
    }
  }
```

> 类型别名

```
  type Name = string;
  type NameResolver = () => string;
  type NameOrResolver = Name | NameResolver;
  function getName(n: NameOrResolver): Name {
  if (typeof n === 'string') {
  return n;
  } else {
  return n();
  }
  }
```

> 字符串字面量类型, type EventNames = 'click' | 'scroll' | 'mousemove'

> 元组(数组合并了相同类型的对象，而元组（Tuple）合并了不同类型的对象)

> 枚举(用于取值被限定在一定范围内的场景)

> 类

```
  类的用法: 属性和方法, 类的继承, 存取器, 静态方法
  TypeScript 中类的用法:
  public, private, protected
  参数属性
  readonly
  class Animal {
    readonly name;
    public constructor(name) {
      this.name = name;
    }
  }
```

> 抽象类

> 类实现接口(不同类之间可以有一些共有的特性，这时候就可以把特性提取成接口（interfaces），用 implements 关键字来实现)

```
  interface Alarm { // 报警
    alert();
  }

  class Door { // 门
  }

  class SecurityDoor extends Door implements Alarm { // 报警门
    alert() {
      console.log('SecurityDoor alert');
    }
  }

  class Car implements Alarm {
    alert() {
      console.log('Car alert');
    }
  }

  接口继承接口
  interface Alarm {
    alert();
  }

  interface LightableAlarm extends Alarm {
    lightOn();
    lightOff();
  }

  接口继承类
  class Point {
    x: number;
    y: number;
  }

  interface Point3d extends Point {
    z: number;
  }
```

> 泛型(在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性)

```
  function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
      result[i] = value;
    }
    return result;
  }

  createArray<string>(3, 'x')

  多个类型参数

  泛型约束(在函数内部使用泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它的属性或方法)
  interface Lengthwise {
    length: number;
  }

  function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
  }

  泛型接口

  泛型类
  class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
  }

  let myGenericNumber = new GenericNumber<number>();
  myGenericNumber.zeroValue = 0;
  myGenericNumber.add = function(x, y) { return x + y; }

  泛型参数的默认类型
```

> 声明合并: 函数的合并, 接口的合并, 类的合并

> 代码检查, eslint

> 编译选项

> 实用技巧
```
interface Person {
  name: string;
  age: number;
  gender: "male" | "female";
}
```

1. typeof 关键词除了做类型保护 还可以从实现推出类型
2. keyof 取得一个对象接口的所有 key 值
3. 映射类型 in
```
  //批量把一个接口中的属性都变成可选的
  type PartPerson = {
    [Key in keyof Person]?: Person[Key];
  }
```
4. infer关键字
```
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : any;
取到函数返回值的类型
```
5. 内置工具类型
```
type Exclude<T, U> = T extends U ? never: T
type Extract<T, U> = T extends U ? T: never
type NonNullable<T> = T extends null | undefined? never | T

// Parameters 该工具类型主要是获取函数类型的参数类型
type Parameters<T> = T extends (...args: infer R) => any?R: any
type Partial<T> = { [P in keyof T]?: T[P] }

Omit<K,T> 基于已经声明的类型进行属性剔除获得新类型
type MyOmit<T, K extends keyof T> = {
  [P in Exclude<keyof T, K>]: T[P]
}
```

> TypeScript 装饰器
1. 装饰器是一种特殊类型的声明,它能够被附加到类声明、方法、属性或参数上，可以修改类的行为

2. 常见的装饰器有类装饰器、属性装饰器、方法装饰器和参数装饰器

3. 装饰器的写法分为普通装饰器和装饰器工厂

4. 装饰器执行顺序


(1) 类装饰器
```
namespace a {
  //当装饰器作为修饰类的时候，会把构造器传递进去
  function addNameEat(constructor: Function) {
    constructor.prototype.name = "hello";
    constructor.prototype.eat = function () {
      console.log("eat");
    };
  }
  @addNameEat
  class Person {
    name!: string;
    eat!: Function;
    constructor() {}
  }
  let p: Person = new Person();
  console.log(p.name);
  p.eat();
}
```

(2) 属性装饰器

(3) 方法装饰器
 
(4) 参数装饰器

> 编译
1. tsconfig.json 的作用
```
用于标识 TypeScript 项目的根路径
用于配置 TypeScript 编译器
用于指定编译的文件
```

2. tsconfig.json 重要字段
```
files - 设置要编译的文件的名称
include - 设置需要进行编译的文件，支持路径模式匹配
exclude - 设置无需进行编译的文件，支持路径模式匹配
compilerOptions - 设置与编译流程相关的选项
```

3. compilerOptions 选项
```
{
  "compilerOptions": {

    /* 基本选项 */
    "target": "es5",                       // 指定 ECMAScript 目标版本: 'ES3' (default), 'ES5', 'ES6'/'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'
    "module": "commonjs",                  // 指定使用模块: 'commonjs', 'amd', 'system', 'umd' or 'es2015'
    "lib": [],                             // 指定要包含在编译中的库文件
    "allowJs": true,                       // 允许编译 javascript 文件
    "checkJs": true,                       // 报告 javascript 文件中的错误
    "jsx": "preserve",                     // 指定 jsx 代码的生成: 'preserve', 'react-native', or 'react'
    "declaration": true,                   // 生成相应的 '.d.ts' 文件
    "sourceMap": true,                     // 生成相应的 '.map' 文件
    "outFile": "./",                       // 将输出文件合并为一个文件
    "outDir": "./",                        // 指定输出目录
    "rootDir": "./",                       // 用来控制输出目录结构 --outDir.
    "removeComments": true,                // 删除编译后的所有的注释
    "noEmit": true,                        // 不生成输出文件
    "importHelpers": true,                 // 从 tslib 导入辅助工具函数
    "isolatedModules": true,               // 将每个文件做为单独的模块 （与 'ts.transpileModule' 类似）.

    /* 严格的类型检查选项 */
    "strict": true,                        // 启用所有严格类型检查选项
    "noImplicitAny": true,                 // 在表达式和声明上有隐含的 any类型时报错
    "strictNullChecks": true,              // 启用严格的 null 检查
    "noImplicitThis": true,                // 当 this 表达式值为 any 类型的时候，生成一个错误
    "alwaysStrict": true,                  // 以严格模式检查每个模块，并在每个文件里加入 'use strict'

    /* 额外的检查 */
    "noUnusedLocals": true,                // 有未使用的变量时，抛出错误
    "noUnusedParameters": true,            // 有未使用的参数时，抛出错误
    "noImplicitReturns": true,             // 并不是所有函数里的代码都有返回值时，抛出错误
    "noFallthroughCasesInSwitch": true,    // 报告 switch 语句的 fallthrough 错误。（即，不允许 switch 的 case 语句贯穿）

    /* 模块解析选项 */
    "moduleResolution": "node",            // 选择模块解析策略： 'node' (Node.js) or 'classic' (TypeScript pre-1.6)
    "baseUrl": "./",                       // 用于解析非相对模块名称的基目录
    "paths": {},                           // 模块名到基于 baseUrl 的路径映射的列表
    "rootDirs": [],                        // 根文件夹列表，其组合内容表示项目运行时的结构内容
    "typeRoots": [],                       // 包含类型声明的文件列表
    "types": [],                           // 需要包含的类型声明文件名列表
    "allowSyntheticDefaultImports": true,  // 允许从没有设置默认导出的模块中默认导入。

    /* Source Map Options */
    "sourceRoot": "./",                    // 指定调试器应该找到 TypeScript 文件而不是源文件的位置
    "mapRoot": "./",                       // 指定调试器应该找到映射文件而不是生成文件的位置
    "inlineSourceMap": true,               // 生成单个 soucemaps 文件，而不是将 sourcemaps 生成不同的文件
    "inlineSources": true,                 // 将代码与 sourcemaps 生成到一个文件中，要求同时设置了 --inlineSourceMap 或 --sourceMap 属性

    /* 其他选项 */
    "experimentalDecorators": true,        // 启用装饰器
    "emitDecoratorMetadata": true          // 为装饰器提供元数据的支持
  }
}
```

> 模块和声明文件
1. 全局模块

2. 文件模块

3. 声明文件
```
我们可以把类型声明放在一个单独的类型声明文件中
文件命名规范为*.d.ts
查看类型声明文件有助于了解库的使用方式
```

4. 第三方声明文件

