---
{"dg-publish":true,"dg-permalink":"SDE/2024/npm-theory","permalink":"/SDE/2024/npm-theory/","title":"NPM Theory","dgHomeLink":true,"dgShowBacklinks":true,"dgShowFileTree":true,"dgEnableSearch":true,"dgShowToc":true,"dgShowTags":true}
---




以 https://github.com/bvaughn/js-search 为例子 



![NPM 原理-20240729143226433.webp](/img/user/output/SDE%20up/attachments/NPM%20%E5%8E%9F%E7%90%86-20240729143226433.webp)

可以找到
![NPM 原理-20240729143540781.webp](/img/user/output/SDE%20up/attachments/NPM%20%E5%8E%9F%E7%90%86-20240729143540781.webp)



但是另一个react项目中
会有一些 @开头的东西
![NPM 原理-20240729143746740.webp](/img/user/output/SDE%20up/attachments/NPM%20%E5%8E%9F%E7%90%86-20240729143746740.webp)

<span style="color:rgb(255, 0, 0)">这是静态类型检查 </span>

# When

什么时候会有 

在使用 npm 安装依赖包时，`@types` 包会被添加到 `node_modules` 目录中的几种情况如下：

### 1. 明确安装 `@types` 包

如果你在安装某个 JavaScript 库的同时，也明确地安装了该库的类型定义包，这个类型定义包（通常是以 `@types/` 前缀命名的）会被添加到 `node_modules` 目录中。例如，如果你安装了 `lodash` 和它的类型定义：

```bash

npm install lodash
npm install --save-dev @types/lodash

```

这种情况下，`@types/lodash` 会出现在 `node_modules/@types` 目录下。

### 2. 库内置类型定义

有些库直接在其自身的 npm 包中包含了 TypeScript 类型定义文件。这些库不需要额外的 `@types` 包，因为它们的类型定义已经被内置在库的发布版本中。这些类型定义通常在库的 `package.json` 文件中通过 `types` 或 `typings` 字段指定。例如，如果你安装了这样一个库：

```bash

npm install some-library

```

并且该库内置了类型定义，你不需要安装任何 `@types/some-library`，类型定义已经随库一起被安装。

### 3. 间接依赖

如果你安装的某个库依赖于其他需要类型定义的库，这些依赖的库的类型定义（如果存在）也可能被自动安装。这取决于你安装的主库如何管理其类型依赖。如果主库在其 `package.json` 中列出了 `@types/` 依赖作为它的依赖之一，那么这些类型定义包也会被安装。

### 4. 开发依赖自动安装

在某些项目配置中，你可能在项目的 `package.json` 文件中已经包含了一系列开发依赖（包括各种 `@types` 包），当你运行 `npm install` 时，所有列出的依赖项都会被安装。

### 总结

`@types` 类型定义包的安装通常是为了在 TypeScript 项目中提供 JavaScript 库的类型信息，以支持类型检查和其他 TypeScript 功能。安装这些包可以是明确的（直接安装）或间接的（作为依赖或内置在库中）。在日常开发中，正确管理这些类型定义是维护 TypeScript 项目类型安全的重要方面。




## 没有的话想有怎么办 
自己写

## 有的话想没有怎么办
### 1. 使用 `any` 类型

最简单的方法是通过声明整个模块为 `any` 类型来绕过 TypeScript 的类型系统。这种方法非常快速，几乎不需要额外的代码或文件。你可以在项目的一个全局类型声明文件中添加如下内容：

```tsx
typescript复制代码
// 在一个全局的 d.ts 文件，如 types.d.ts
declare module 'example-library';

```

这行代码会使得 TypeScript 将 `example-library` 视为任意类型，从而不会对其内容进行类型检查。这样做的好处是简便，坏处是你失去了类型检查带来的很多好处，比如编码时的自动完成和错误检测。

### 2. 配置 TypeScript 编译器选项

如果你不想在你的项目中特别为某些库编写或维护类型声明，你可以通过修改 `tsconfig.json` 文件中的编译器选项来减少类型错误：

```json
json复制代码
{
  "compilerOptions": {
    "skipLibCheck": true,
    "noImplicitAny": false}
}

```

- `skipLibCheck`: 这个选项会让 TypeScript 编译器忽略所有第三方库文件（`.d.ts` 文件）的类型检查错误。这对于快速绕过错误的类型定义文件非常有用。
- `noImplicitAny`: 设置为 `false` 可以允许 TypeScript 将没有明确指定类型的表达式和声明隐式地视为 `any` 类型，这减少了显式声明 `any` 的需要。

### 3. 评估是否更换库

如果一个库因为缺乏类型支持而导致在 TypeScript 项目中使用时遇到持续的问题，也可以考虑是否有其他更好的替代库，这些库可能已经有良好的 TypeScript 支持。选择一个社区支持更强、更新更频繁、内置类型声明的库，可能会减少很多开发中的麻烦。



# Why
为什么会有 = 有什么好处 

引入类型检查，特别是在使用如 TypeScript 这样的静态类型语言时，可以带来多个明显的好处。下面我将解释为什么要加类型检查，并通过代码示例来展示这些好处。

### 为什么需要类型检查

1. **错误减少**：类型检查可以在编译阶段捕捉错误，如类型不匹配、未定义的函数调用、参数错误等。这意味着很多潜在的运行时错误可以在代码运行前就被发现和解决。
2. **代码质量提升**：类型系统强制开发者在写代码时就思考数据的类型和结构，这促使开发者更深入地思考问题，从而写出更清晰和更可维护的代码。
3. **自动化文档**：类型定义本身就起到了文档的作用，明确地展示了函数期望的输入和输出。这使得代码更易于理解和使用，尤其是在团队协作中。
4. **智能的代码补全**：大多数支持 TypeScript 的开发环境（如 Visual Studio Code）提供了基于类型信息的智能代码补全、导航和重构工具，大大提高了开发效率。


代码示例
假设我们有一个简单的 JavaScript 函数，用于处理用户信息，如果没有类型检查，可能写成这样：

```
function handleUserInfo(user) {
    console.log("User Name: " + user.name);
    console.log("User Age: " + user.age);
}

// 调用函数  不会报错
handleUserInfo({ name: "Alice", age: "25" });
```

在上面的 JavaScript 代码中，age 字段被错误地传入了一个字符串，而我们期望的是一个数字。这种错误在 JavaScript 中很常见，因为它是动态类型语言。

在这段代码中，由于 `age` 传入了一个字符串 `"25"` 而不是数字，以下是可能的情况和影响：

### 1. 输出

在当前的函数实现中，`age` 被作为字符串处理并直接连接到另一个字符串，所以这个错误不会直接导致程序崩溃或者明显的运行时错误。输出将会是：

```sql
sql复制代码
User Name: Alice
User Age: 25

```

这看起来可能并没有什么问题，但这并不意味着数据是正确处理的。

### 2. 后续数值操作的影响

如果程序的其他部分期望 `age` 是一个数字，并在此基础上进行数值计算，比如计算年龄区间或者验证年龄是否在某个范围内，使用字符串就可能导致错误或不预期的行为。例如：

```jsx
javascript复制代码
if (user.age < 30) {
    console.log("Under 30 years old.");
} else {
    console.log("30 years or older.");
}

```

在 JavaScript 中，尝试对字符串和数字进行比较会导致 JavaScript 尝试将字符串转换为数字。在这种情况下，由于 `"25"` 是一个有效的数字字符串，它会被转换为数字 `25`，并正确地进行比较。但如果字符串不是有效的数字（如 `"twenty-five"`），则比较的结果会是 `NaN` (Not a Number)，这会使得条件判断失败，导致逻辑上的错误。
现在，我们用 TypeScript 来改写上述代码，并加入类型检查：

```
interface User {
    name: string;
    age: number;
}

function handleUserInfo(user: User) {
    console.log("User Name: " + user.name);
    console.log("User Age: " + user.age);
}
// 尝试调用函数
handleUserInfo({ name: "Alice", age: "25" });  // TypeScript 会在这里报错
```


在 TypeScript 版本中，我们定义了一个 User 接口来指定 user 参数的结构，包括类型信息。如果尝试像之前那样错误地传递一个字符串作为年龄，TypeScript 编译器将会在编译时抛出错误，提示我们 age 的类型不匹配。这样的类型检查可以防止我们在代码进一步执行之前犯下可能导致运行时错误的低级错误。




# webpack

如果想要查看源码的问题的话

- `"require": "./dist/umd/js-search.js"`：如果包是通过 `require()`（即 CommonJS 方式）被导入，那么将使用 `./dist/umd/js-search.js`。
- `"import": "./dist/esm/js-search.js"`：如果包是通过 ES6 `import` 语句被导入，那么将使用 `./dist/esm/js-search.js`。


![NPM 原理-20240729170932499.webp](/img/user/output/SDE%20up/attachments/NPM%20%E5%8E%9F%E7%90%86-20240729170932499.webp)

# require和 import的区别是什么


### 语法

- **require()**：
    
    - CommonJS 模块使用 `require()` 函数来加载模块。
    - 语法较为简单，主要用在 Node.js 中。
    - 示例：`const lodash = require('lodash');`
- **import**：
    
    - ES6 模块使用 `import` 语句来导入模块。
    - 支持更复杂的导入方式，如导入单个函数、多个函数、或重命名。
    - 示例：`import { map, reduce } from 'lodash';` 或 `import * as lodash from 'lodash';`