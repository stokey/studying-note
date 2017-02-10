## Flow
---
新的开源JavaScript静态类型检查器。

### 核心思想
+ 假设大部分的JS代码是隐式静态类型

### 作用
能在代码运行之前检测到：

+ 隐式型转换失败
+ 空引用
+ undefined is not a function

### Start

+ 创建npm package到项目中

```
echo '{"name": "get_started", "scripts": {"flow": "flow; test $? -eq 0 -o $? -eq 2"}}' > package.json
```
+ 添加Flow到项目中

```
$> touch .flowconfig
$> npm install --save--dev flow-bin
```

+ 在需要Flow进行检测的文件头部添加`// @flow`字段
+ 运行Flow进行代码格式校验

```
$> npm run-script flow
``` 

### Flow的类型
---

+ Flow基本数据[JS基本数据]
	+ boolean
	+ number
	+ string
	+ null
	+ void(`undefined`)
+ any：是所有类型的父类，也是所有类型的子类。不需要进行类型检测
+ mixed：是所有类型的父类。mixed能在任何情况下被代替any
+ Arrays：`Array<T>`
+ Tuples：特殊的数组，用于表示有限的、由不同成分组成的集合（`tuple:[string, number, boolean]`）
+ Objects：`object: {foo: string, bar: number}`
+ Functions
+ Classes/Interfaces
+ Type Aliases
+ Generics 
