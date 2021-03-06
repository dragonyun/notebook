## history对象

​	history 对象保存着用户上网的历史记录，从窗口被打开的那一刻算起。因为 history 是 window 对象的属性，因此**每个浏览器窗口、每个标签页乃至每个框架，都有自己的 history 对象与特定的 window 对象关联**。出于安全方面的考虑，开发人员无法得知用户浏览过的 URL。不过，借由用户访问过的页面列表，同样可以在不知道实际 URL 的情况下实现后退和前进。

> 浏览历史跳转，很实用的对象
>
> history与特定的 window 对象关联
>
> 开发者无法知道浏览历史中具体的 URL

> history直接使用并不是很频繁，更多的是一些基于它封装
>
> 我们只要知道history是关于 **URL历史记录** 的对象就好了



### 属性

- length

  保存历史记录的数量。

  这个数量
  包括所有历史记录，即所有向后和向前的记录。对于加载到窗口、标签页或框架中的第一个页面而言，history.length 等于 0。

  ```js
  // 通过像下面这样测试该属性的值，可以确定用户是否一开始就打开了你的页面。
  if (history.length == 0){ 
   //这应该是用户打开窗口后的第一个页面
  }
  ```

  > 当页面的 URL 改变时，就会生成一条历史记录

  



### 方法

- go()

  在历史记录中任意跳转，可以向前也可以向后。

  接受一个参数，表示向前或者向后跳转的页面数的一个整数值，可正可负。

  ```js
  // 后退一页
  history.go(-1);
  // 前进两页
  history.go(2);
  ```

  该参数也可以是字符串参数，此时浏览器会跳转到历史记录中包含该字符串的第一个
  位置——可能后退，也可能前进，具体要看哪个位置最近。如果历史记录中不包含该字符串，那么这个方法什么也不做，如下

  ```js
  //跳转到最近的 wrox.com 页面
  history.go("wrox.com"); 
  //跳转到最近的 nczonline.net 页面
  history.go("nczonline.net");
  ```

- back() 和 forward()

  go()方法的简写，分别代表 后退一页 和 前进一页。

