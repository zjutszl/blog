# ES6-Module

## 概述

ES6 在语言规格的层面上，实现了模块功能，而且实现得相当简单，完全可以取代现有的 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案。

ES6 模块的设计思想，是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。**CommonJS 和 AMD 模块，都只能在运行时确定这些东西。**
比如，**CommonJS 模块就是对象**，输入时必须查找对象属性。

```javascript
var { stat, exists, readFile } = require("fs");
```

ES6 模块不是对象，而是通过 export 命令显式指定输出的代码，输入时也采用静态命令的形式

```javascript
import { stat, exists, readFile } from "fs";
```

所以，ES6 可以在编译时就完成模块编译，效率要比 CommonJS 模块高。

## export，import

模块功能主要由两个命令构成：export 和 import。export 命令用于用户自定义模块，规定对外接口；**import 命令用于输入其他模块提供的功能，同时创造命名空间（namespace），防止函数名冲突。**

> import 能创造命名空间，防止函数名冲突？ 厉害了！

ES6 允许将独立的 JS 文件作为模块，也就是说，允许一个 JavaScript 脚本文件调用另一个脚本文件。**该文件内部的所有变量，外部无法获取，必须使用 export 关键字输出变量。**

> export 指定输出的，外部才可见、可调用。

---

### export 用法

```javascript
// Version 1
var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;

// Version 2
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export { firstName,lastName,year }
```

上面代码在 export 命令后面，使用大括号指定所要输出的一组变量。**它与前一种写法(直接放置在 var 语句前)是等价的，但是应该优先考虑使用这种写法。**因为这样就可以在脚本尾部，一眼看清楚输出了哪些变量。

使用 export 命令定义了模块的对外接口以后，其他 JS 文件就可以通过 import 命令加载这个模块（文件）。

```javascript
import { firstName, lastName, year } from "./profile";

function sfirsetHeader(element) {
  element.textContent = firstName + " " + lastName;
}
```

import 命令接受一个对象（用大括号表示），里面指定要从其他模块导入的变量名。**大括号里面的变量名，必须与被导入模块（profile.js）对外接口的名称相同。**

如果想为输入的变量**重新取个名字，import 语句中要使用 as 关键字，将输入的变量重命名**。

```javascript
import { lastName as surname } from "./profile";
```

### 模块的整体输入，module 命令

export 命令除了输出变量，还可以输出方法或类(class)。

```javascript
// circle.js
export function area(radius) {
  return Math.PI * radius * radius;
}

export function circumference(radius) {
  return 2 * Math.PI * radius;
}

// main.js

import { area, circumference } from "circle";

console.log("圆面积：" + area(4));
console.log("圆周长：" + circumference(14));
```

module 命令可以**取代 import 语句**，达到整体输入模块的效果

```javascript
// main.js

module circle from 'circle';

console.log("圆面积：" + circle.area(4));
console.log("圆周长：" + circle.circumference(14));
```

### export default 命令

如果想要输出匿名函数，可以使用 export default 命令。

```javascript
// export-default.js

export default function() {
  console.log("foo");
}
```

其他模块输入该模块时，**import 命令可以为该匿名函数指定任意名字。**

```javascript
// import-default.js

import customName from "./export-default";
customName();
```

上面代码的 import 命令，可以用任意名称指向输出的匿名函数。**需要注意的是，这时 import 命令后面，不使用大括号。**

export default 命令用在非匿名函数前，也是可以的。

```javascript
// export-default.js

export default function foo() {
  console.log('foo');
}

// 或者写成

function foo() {
  console.log('foo');
}

export default foo;
```

上面代码中，foo 函数的函数名 foo，在模块外部是无效的。加载的时候，视同匿名函数加载。

export default 命令用于指定模块的默认输出。如果模块加载时，只能输出一个值或方法，那就是 export default 所指定的那个值或方法。所以，import 命令后面才不用大括号。显然，一个模块只能有一个默认输出，因此 export default 命令只能使用一次。

如果想在一条 import 语句中，同时输入默认方法和其他变量，可以写成下面这样。

```javascript
import customName, { otherMethod } from "./export-default";
```

如果要输出默认的值，只需将值跟在 export default 之后即可。

```javascript
export default 42;
```
