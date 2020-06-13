# Array迭代方法 #

----------
 - 一共5个迭代方法
 - 每个方法接收两个参数：要在每一项上运行的函数和（可选的）运行改行的作用域对象--影响this的值。
 - 传入方法中的函数会接收三个参数：数组项的值、该项在数组中的位置和数组对象本身。（item，index，array）  
 - 以上方法都不会修改数组中的包含的值。

----------
 - every() ：对数组中的每一项运行给定函数，如果该函数对每一项都返回 true ，则返回 true 。  

 - filter() ：对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。 

 - forEach() ：对数组中的每一项运行给定函数。这个方法没有返回值。 

 - map() ：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。 

 - some() ：对数组中的每一项运行给定函数，如果该函数对任一项返回 true ，则返回 true 。 

----------
第一类，数组项检查是否满足条件  

 every() 和 some()  

```js
var numbers = [1,2,3,4,5,4,3,2,1];
var everyResult = numbers.every(function(item, index, array){
	return (item > 2);
});
alert(everyResult); //false
var someResult = numbers.some(function(item, index, array){
	return (item > 2);
});
alert(someResult); //true
```

----------
第二类，返回<font color="red">**新数组**</font>

 	filter() 和 map() 

```js
var numbers = [1,2,3,4,5,4,3,2,1];
var filterResult = numbers.filter(function(item, index, array){
	return (item > 2);
});
alert(numbers);	//[1,2,3,4,5,4,3,2,1]
alert(filterResult); //[3,4,5,4,3]
```


​     
```js
var numbers = [1,2,3,4,5,4,3,2,1];
var mapResult = numbers.map(function(item, index, array){
	return item * 2;
});
alert(numbers);	//[1,2,3,4,5,4,3,2,1]
alert(mapResult); //[2,4,6,8,10,8,6,4,2]
```

----------
第三类，无返回值

```js
forEach()  

var numbers = [1,2,3,4,5,4,3,2,1];
numbers.forEach(function(item, index, array){
//执行某些操作
array[index]+=1;
});
alert(numbers); //[2,3,4,5,6,5,4,3,2]
// 这样操作改变了原数组是因为直接修改了array内容，要不要改原数组看代码怎么写
```