---
layout: post
title: JavaScript常用技巧集合
date: 2017-03-04
tag: JavaScript
---

### !! 将变量转换成布尔类型

!!（双重否定运算符），它能自动将任何类型的数据转换为布尔值，只有这些变量才会返回false：0，null，""，undefined或NaN，其他的都返回true。

```js
function Account(cash) {  
    this.cash = cash;
    this.hasMoney = !!cash;
}
var account = new Account(100.50);  
console.log(account.cash); // 100.50  
console.log(account.hasMoney); // true

var emptyAccount = new Account(0);  
console.log(emptyAccount.cash); // 0  
console.log(emptyAccount.hasMoney); // false  
```

### + 将变量转换成数字

+ 它只适用于数字字符串，不然就会返回NaN（不是数字）,同样适用于时间Date()。

```js
function toNumber(strNumber) {  
    return +strNumber;
}
console.log(toNumber("1234")); // 1234  
console.log(toNumber("ACB")); // NaN  

console.log(+new Date()) // 1461288164385  
```

### &&短路条件

可以在两个变量之间使用&&（AND运算符）来缩短代码。
也可以用这种方法来检查对象中是否存在某些属性或函数。

```js
if (conected) {  
    login();
}

conected && login();  

user && user.login(); 
```

### ||设置默认值

使用||（OR运算符），并把默认值作为它的第二个参数。如果第一个参数返回false，那么第二个参数将会被作为默认值返回。

```js
function User(name, age) {  
    this.name = name || "Oliver Queen";
    this.age = age || 27;
}
var user1 = new User();  
console.log(user1.name); // Oliver Queen  
console.log(user1.age); // 27

var user2 = new User("Barry Allen", 25);  
console.log(user2.name); // Barry Allen  
console.log(user2.age); // 25  
```



