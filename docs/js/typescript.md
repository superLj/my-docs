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
