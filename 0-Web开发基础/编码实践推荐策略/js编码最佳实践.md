## js编码最佳实践

以下建议有助于写出更易维护，更高质量的代码，建议采用。



### 可维护性

---

可维护性是指易理解，符合常识，易适配，易扩展，易调试。



1. 代码缩进规范

   无论是4个空格，还是其他数量，整个项目保持一致，更容易让人看懂，可读性更强。一般编辑器比如vscode里面都有格式化文档，顺手格式化一下还是很有必要的。

2. 写注释

   在函数和方法，大型代码块，复杂的算法，黑科技的地方添加注释是很有必要的。注释的目的是增强项目代码的可读性，代码更容易理解，后面维护起来也方便。

   注释要求清楚明白简洁，如果注释里面长篇大论，我宁愿去读代码。

   > 很多代码可能到未来自己也看不懂了，写注释对自己，对他人，对项目都是好事。

3. 变量和函数命名

   命名总体要求是见名知意，下面是几个建议，可以帮助做到这一点。

   a.变量名用名词；b.函数名动车开头(如isEmpty)；c.变量和函数使用符合逻辑的名称，长度可以作为次要问题(后面有压缩)；d.使用驼峰大小写形式；e.尽量使用描述性和直观的词。

4. 变量类型透明化

   js是松散类型的语言，数据类型不清晰。

   a.初始化赋值标明数据类型；b.注释标明数据类型；c.ts也可以做到

5. HTML/JavaScript解耦

   html是数据，js是行为，css是样式，各司其职，如果相互之间过于耦合，必然降低代码可维护性。

   ```js
   // 举例
   <!-- 使用<script>造成 HTML/JavaScript 紧密耦合 -->
   <script>
   document.write("Hello world!");
   </script>
   <!-- 使用事件处理程序属性造成 HTML/JavaScript 紧密耦合 -->
   <input type="button" value="Click Me" onclick="doSomething()"/>
       // HTML 紧密耦合到了 JavaScript
   function insertMessage(msg) {
   	let container = document.getElementById("container");
   	container.innerHTML = `<div class="msg">
   		<p> class="post">${msg}</p>
   		<p><em>Latest message above.</em></p>
   		</div>`;
   }
   ```

   这样是有问题的：

   a.如果有问题，html和js问题定位不清晰，修改的话也会同时影响；

   b.js中不应该大量创建html，动态生成html很难查问题；

   c.最好是js直接调用已经创建好的隐藏在页面中的标记。

6. css/js解耦

   原理同上，各司其职。

   ```js
   // CSS 紧耦合到了 JavaScript
   element.style.color = "red";
   element.style.backgroundColor = "blue";
   // CSS 与 JavaScript 松散耦合
   element.className = "edit";
   ```

   a.css和js同时修改是一个噩梦；

   b.主流方式是采用动态修改类名实现样式修改；

7. 应用程序逻辑/事件处理程序解耦

   监听事件是很常见的，但是程序逻辑解耦我们没怎么注意过。

   ```js
   function handleKeyPress(event) {
   	if (event.keyCode == 13) {
           let target = event.target;
           let value = 5 * parseInt(target.value);
           if (value > 10) {
               document.getElementById("error-msg").style.display = "block";
           }
   	}
   }
   ```

   a.程序逻辑没法单独测试；

   b.程序逻辑没法复用；

   ```js
   function validateValue(value) {
       value = 5 * parseInt(value);
       if (value > 10) {
      	 document.getElementById("error-msg").style.display = "block";
       }
   }
   function handleKeyPress(event) {
       if (event.keyCode == 13) {
           let target = event.target;
           validateValue(target.value);
       }
   }
   ```

   修改之后的写法，相当于把事件监听和处理程序逻辑分离开了。

   解耦有三个注意事项：

   a.不要把event对象传给其他方法，而是只传递event对象中必要的数据。

   b.应用程序中每个可能的操作都应该无须事件处理程序就可以执行。

   c.事件处理程序应该处理事件，而把后续处理交给应用程序逻辑。

   > 第一，我认为第一个要求，event数据大只是次要因素，因为只是引用。
   >
   > 第二，主要是为了处理程序能独立，并且复用，直接传类似event的数据做不到独立复用。
   >
   > 第三，拆分开各司其职，事件触发问题还是程序处理问题一目了然。

8. 尊重对象所有权

   不要修改不属于你的对象。如果你不负责创建和维护某个对象及其构造函数或方法，就不应该对其进行任何修改。比如给实例或原型添加属性和方法，重定义已有的方法。

   > 好像我们经常这么做，window上加属性，重写console，error方法，修改array原型链方法。

   会导致下面这些问题：

   a.修改之后别人不知道；大家调用同一个对象，别人不知道你做了什么。

   b.浏览器厂商行为未知；不知道浏览器厂商会对原生对象做什么，会引发兼容问题。

   推荐的做法是：

   a.创建新对象，通过它与别的对象交互；

   b.创建新的自定义类型继承本来想要修改的类型，可以给自定义类型添加新功能。

   > 有些属性暴露在window上，别人就可以直接在浏览器控制台操作，很有风险。

9. 不声明全局变量

   尽量不声明全局变量和函数。和上面是一样的理由，如果一定需要，那就把相关的内容集合在一起。

   ```js
   // 创建全局对象
   var Wrox = {};
   // 为本书（Professional JavaScript）创建命名空间
   Wrox.ProJS = {};
   // 添加本书用到的其他对象
   Wrox.ProJS.EventUtil = { ... };
   Wrox.ProJS.CookieUtil = { ... };
   ```

10. 不要比较null

    代码中类型检测最常见的是看值是不是null，判断null只是去除错误答案，不是正确的判断。

    ```js
    function sortArray(values) {
        if (values != null) { // 不要这样比较！
     	   values.sort(comparator);
        }
    }
    function sortArray(values) {
        if (values instanceof Array) { //  推荐
       	 values.sort(comparator);
        }
    }
    ```

    推荐做法：

    a.如果是引用类型，推荐 instanceof 操作符检测其构造函数。

    b.如果是原始类型，推荐 typeof 检查类型。

    c.如果希望值是有特定方法名的对象，则使用 typeof 操作费确保对象上存在给定名字的方法。

11. 使用常量

    好处是数据和逻辑分离，减少魔术字符串。

    常量使用适用下面的场景：

    a.重复出现的值；b.用户界面字符串；c.URL；d.任何可能变化的值。



### 性能

---

性能问题最直观的感受是卡顿。代码执行时间长，页面渲染频繁，文件大都可能有影响。



1. 避免全局查找

   相比于局部值，全局变量和函数都最费时间的，因为需要经历作用域链查找。

   ```js
   function updateUI() {
       let imgs = document.getElementsByTagName("img");
       for (let i = 0, len = imgs.length; i < len; i++) {
       	imgs[i].title = '${document.title} image ${i}';
       }
       let msg = document.getElementById("msg");
       msg.innerHTML = "Update complete.";
   }
   ```

   看起来好像只有三个，经历了for循环，次数可能上百次。

   解决办法是：**通过在局部作用域保存document对象的引用，能够明显提升这个函数的性能，因为只需要作用域链查找。**

   ```js
   function updateUI() {
       let doc = document;
       let imgs = doc.getElementsByTagName("img");
       for (let i = 0, len = imgs.length; i < len; i++) {
     	  imgs[i].title = '${doc.title} image ${i}';
       }
       let msg = doc.getElementById("msg");
       msg.innerHTML = "Update complete.";
   }
   ```

   建议是如果函数中超过两个的全局对象引用，就把这个对象保存为一个局部变量。

2. 不适用with语句

   a.with会自己创建作用域；b.代码不清晰

3. 避免不必要的属性查找

   涉及到算法复杂度，就是O(1),O(n)等，道理不用讲，复杂度越低越好。

   ```js
   let query = window.location.href.substring(window.location.href.indexOf("?"));
   ```

   改进之后

   ```js
   let url = window.location.href;
   let query = url.substring(url.indexOf("?"));
   ```

   改进之前是6次属性查找，改进之后是4次，节省了33%；代码量越大，效果越明显。

4. 优化循环

   循环和时间紧密相关，优化是很有必要的。

   a.简化终止条件；每次都会计算终止条件，越快越好。

   b.简化循环体；这个最耗时间，尽量把能移到外面的代码移出。

   c.使用后测试循环；for和while是先测试循环，do-while是后测试循环，可以避免对终止条件的初始评估，从而节省时间。

5. 展开循环

   如果循环次数已知并且有限，不创建循环，直接执行代码会效率更高。

   ```js
   // 抛弃循环
   process(values[0]);
   process(values[1]);
   process(values[2]);
   ```

   > 这种优化方式，只能说视情况而定。

6. 避免重复解释

   这个是JavaScript代码尝试解释JavaScript代码的情形。eval函数，Function构造，setTimeout传入字符串时会出现。

   ```js
   // 对代码求值：不要
   eval("console.log('Hello world!')");
   // 创建新函数：不要
   let sayHi = new Function("console.log('Hello world!')");
   // 设置超时函数：不要
   setTimeout("console.log('Hello world!')", 500);
   ```

   这些包含JavaScript代码的字符串，在初始解析阶段不会被解释，因为代码包含在字符串里。这就意味着在JavaScript运行时，必须启动新解析器实例来解析这些字符串中的代码。实例化新解析器比较费时间，因此比较慢。推荐写法如下：

   ```js
   // 直接写出来
   console.log('Hello world!');
   // 创建新函数：直接写出来
   let sayHi = function() {
   	console.log('Hello world!');
   };
   // 设置超时函数：直接写出来
   setTimeout(function() {
   	console.log('Hello world!');
   }, 500);
   ```

   > 重复解析是指会启动新的解析器去解析代码

7. 尽量使用原生方法

   原生方法是使用C或C++等编译型语言写的，比JavaScript写的方法快得多。

   比较容易忽视的是Math对象中的复杂数学运算方法。这些方法总是比之下相同任务的JavaScript函数快得多。

   > 这个很有用，编译型语言有天然的优势

8. switch更快

   如果有复杂的 if-else 语句，换成 switch 语句可以更快，然后，把最可能的放前面，可以进一步提升性能。

9. 位操作更快

   在执行数学运算操作时，位操作一定比任何布尔值或数值计算更快。比如求模、逻辑AND与逻辑OR或都更适合位操作。

10. 多个变量声明

    声明多个变量很容易出现多条语句。

    ```js
    // 有四条语句：浪费
    let count = 5;
    let color = "blue";
    let values = [1,2,3];
    let now = new Date();
    // 一条语句更好
    let count = 5,
    color = "blue",
    values = [1,2,3],
    now = new Date();
    ```

11. 插入迭代性值

    任何时候只要使用迭代性值（即会递增或递减的值），都要尽可能使用组合语句。

    ```js
    let name = values[i];
    i++;
    // 改进
    let name = values[i++];
    ```

    一条语句完成了两句的事情。

12. 使用数组和对象字面量

    构造函数和字面量的性能对比。

    ```js
    // 创建和初始化数组用了四条语句：浪费
    let values = new Array();
    values[0] = 123;
    values[1] = 456;
    values[2] = 789;
    // 创建和初始化对象用了四条语句：浪费
    let person = new Object();
    person.name = "Nicholas";
    person.age = 29;
    person.sayName = function() {
    	console.log(this.name);
    };
    ```

    换成字面量

    ```js
    // 一条语句创建并初始化数组
    let values = [123, 456, 789];
    // 一条语句创建并初始化对象
    let person = {
        name: "Nicholas",
        age: 29,
        sayName() {
      	  console.log(this.name);
        }
    };
    ```

    前者有8条语句，后者2条语句，应尽可能使用字面量，减少不必要的语句。

    > 没有绝对，视情况而定

13. 实时更新最小化

    访问DOM，只要访问的部分是显示页面的一部分，就是在执行实时更新操作。

    每次这样的更新，无论是插入一个字符还是删除一点内容，都会导致性能损失。这是因为浏览器需要为此重新计算数千项指标，之后才能执行更新。实时更新次数越多，执行代码所需的视觉也越长。

    ```js
    let list = document.getElementById("myList"),
    	item;
    for (let i = 0; i < 10; i++) {
        item = document.createElement("li");
        list.appendChild(item);
        item.appendChild(document.createTextNode('Item ${i}');
    }
    ```

    上面的代码执行了 20 次实时更新。要解决问题，使用文档片段构建DOM结构，一次性添加进来。

    ```js
    let list = document.getElementById("myList"),
        fragment = document.createDocumentFragment(),
        item;
    for (let i = 0; i < 10; i++) {
        item = document.createElement("li");
        fragment.appendChild(item);
        item.appendChild(document.createTextNode("Item " + i));
    }
    list.appendChild(fragment);
    ```

    这样写就只有一次实时更新。

    **只要是必须更新 DOM，就尽量考虑使用文档片段来预先构建 DOM 结构，然后再把构建好的 DOM结构实时更新到文档中。**

14. 使用innerHTML

    创建节点有两种方式：使用DOM方法createElement和appendChild，以及使用innerHTML。

    少量DOM更新情况下，差别不大，但是对于大量DOM更新，innerHTML更快。

    原理是：**在给 innerHTML 赋值时，后台会创建 HTML 解析器，然后会使用原生 DOM 调用而不是 JavaScript的 DOM 方法来创建 DOM 结构。原生 DOM方法速度更快，因为该方法是执行编译代码而非解释代码。**

    ```js
    let list = document.getElementById("myList"),
    	html = "";
    for (let i = 0; i < 10; i++) {
    	html += '<li>Item ${i}</li>';
    }
    list.innerHTML = html;
    ```

15. 使用事件委托

    大多数web应用汇大量使用事件处理程序实现用户交互。事件处理程序数量过多也会影响速度。

    采用事件委托的话，根据事件冒泡，可以在高层元素上去添加事件处理程序，如果可能，就可以在文档级添加事件处理程序，可以处理整个页面的事件。

16. 注意HTMLCollection

    任何时候，只要访问HTMLCollection，无论是属性还是方法，都会触发查询文档，而这个查询相当耗时。减少访问次数可以极大地提升脚本的性能。

    ```js
    let images = document.getElementsByTagName("img"),
    	image;
    for (let i = 0, len=images.length; i < len; i++) {
    	image = images[i];
    	// 处理
    }
    ```

    这个例子的优化：

    a.计算HTMLCollection长度的代码转移到for循环初始化的部分；

    b.新增了image变量，有了局部变量，就可以不访问HTMLCollection了

    下面这些情况会返回 HTMLCollection：

    a. 调用 getElementsByTagName()；

    b. 读取元素的 childNodes 属性；

    c.读取元素的 attributes 属性；

    d.访问特殊集合，如 document.form 、 document.images 等。











### 参考资料

---

1. JavaScript高级程序设计.第四版，第28章