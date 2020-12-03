## replace使用



### 我的问题

---

我以为replace是全部替换，但是实际上是可选单个或者全部。。。



### 定义和用法

---

replace() 方法用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。



- 语法

  `stringObject.replace(regexp/substr,replacement)`

- 参数

  regexp/substr：必需。规定子字符串或要替换的模式的 RegExp对象。

  replacement：必需。一个字符串。规定了替换文本或生成替换文本的函数。

- 返回值

  一个新的字符串，是用replacement 替换了 regexp的第一次匹配或所有匹配之后的。

- 说明

  1、可以替换第一个，也可以替换全部

  2、regexp/substr可以是字符串，可以是 正则

  3、replacement可以是字符串，也可以是函数

  4、replacement中的$有特殊含义

  - $1、$2、...、$99：与 regexp 中的第 1 到第 99 个子表达式相匹配的文本。
  - $&：与 regexp 相匹配的子串。
  - $`：位于匹配子串左侧的文本。
  - $'：位于匹配子串右侧的文本。
  - $$：直接量符号。



### 使用示例

---

```js
// 1、单个，替换第一个
var str="&&&&"
str.replace(/&/, "%")
// %&&&
// 可以改成字符串，或者加上i，都是等价的

// 2、全部，全部替换
var str="&&&&"
str.replace(/&/g, '%')
// %%%%

// 2.2
name = '"a", "b"';
name.replace(/"([^"]*)"/g, "'$1'");
// ''a','b'' 双引号换单引号，也是全部替换

// 3、位置转换
name = "Doe, John";
name.replace(/(\w+)\s*, \s*(\w+)/, "$2 $1");
// "John Doe"

// 4、函数
name = 'aaa bbb ccc';
uw=name.replace(/\b\w+\b/g, function(word){
  return word.substring(0,1).toUpperCase()+word.substring(1);}
  );
```



### 温馨提醒

---

功能本身很强大，自己用的很浅显。

如果习惯使用正则表达式，应该会比较好一点 /str/g