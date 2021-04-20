### Promise then的方法 resolve 与 reject

JS中的Promise一共有三种状态，分别为pending(等待)、fulfilled(成功)、rejected(失败),
Promise的状态同一时间只能为一种状态。

resolve和reject是Promise的方法**而then和catch是Promise实例的方法（Promise.prototype.then 和 Promise.prototype.catch）**

Promise只能由Pending转化为fulfilled或者rejected，fulfilled与rejected不能相互转化

Promise的链式结构原理是pro.then()或pro.catch()这个函数执行的返回值会返回一个promise对象，所以可以继续调用then或者catch，来完成链式结构。

```js
var pro=new Promise((resolve,reject)=>{
    resolve();
    reject();
})
pro.then(()=>{
    console.log('resolve1');
},()=>{console.log('reject1')}).catch(()=>{
    console.log('catch1')
}).then(()=>{
    console.log('resolve2');
},()=>{console.log('reject2')}).catch(()=>{
    console.log('catch2');
}).then(()=>{
    console.log('resolve3');
})
//输出resolve1 resolve2 resolve3
```

但是有个重点是"promise执行完毕后返回的promise默认进入fulfilled状态"，所以执行完毕后默认执行链式结构中的resolve回调。

执行进入失败状态后，链式结构也会执行之后的resolve回调。

```js
var pro=new Promise((resolve,reject)=>{
    // resolve();
    reject();
})
pro.then(()=>{
    console.log('resolve1');
},()=>{console.log('reject1')}).catch(()=>{
    console.log('catch1')
}).then(()=>{
    console.log('resolve2');
},()=>{console.log('reject2')}).catch(()=>{
    console.log('catch2');
}).then(()=>{
    console.log('resolve3');
})
//reject1 resolve2 resolve3
```

### .then里面的resolve报错，catch是可以捕获报错信息，then的第二个参数不能捕获

```js
var pro=new Promise((resolve,reject)=>{
    resolve();
    reject();
})
pro.then(()=>{
    console.log('resolve1');
    var a = undefined;
    a.b();
    console.log('报错了的代码');
},(err)=>{console.log('reject1',err)}).catch((err)=>{
    console.log('catch1',err);
}).then(()=>{
    console.log('resolve2');
}).catch(()=>{
    console.log('catch2');
})
```

![](D:\renjBlog\img\promise-catch.png)

then的第二个参数和catch捕获错误信息的时候会就近原则，如果是promise内部报错，reject抛出错误后，then的第二个参数和catch方法都存在的情况下，只有then的第二个参数能捕获到，如果then的第二个参数不存在，则catch方法会捕获到。

```js
const promise = new Promise((resolve, rejected) => {
    throw new Error('test');
});
 
//此时只有then的第二个参数可以捕获到错误信息
promise.then(res => {
    //
}, err => {
    console.log(err);
}).catch(err1 => {
    console.log(err1);
});
 
 
//此时catch方法可以捕获到错误信息
promise.then(res => {
    //
}).catch(err1 => {
    console.log(err1);
});
 
 
//此时只有then的第二个参数可以捕获到Promise内部抛出的错误信息
promise.then(res => {
    throw new Error('hello');
}, err => {
    console.log(err);
}).catch(err1 => {
    console.log(err1);
});
 
//此时只有then的第二个参数可以捕获到Promise内部抛出的错误信息
promise.then(res => {
    throw new Error('hello');
}, err => {
    console.log(err);
});
 
 
//此时catch可以捕获到Promise内部抛出的错误信息
promise.then(res => {
    throw new Error('hello');
}).catch(err1 => {
    console.log(err1);
});
```

**Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个`catch`语句捕获。**

```js
getJSON('/post/1.json').then(function(post) {
  return getJSON(post.commentURL);
}).then(function(comments) {
  // some code
}).catch(function(error) {
  // 处理前面三个Promise产生的错误
});
```

上面代码中，一共有三个 Promise 对象：一个由`getJSON`产生，两个由`then`产生。它们之中任何一个抛出的错误，都会被最后一个`catch`捕获。

这也是then的第二个参数处理不了的。