# 关于JS的Promise对象的理解

## 1. Promise对象

```js
new Promise((resolve,reject)=>{
    resolve('成功')
    // reject('失败')
})
```

上面是Promise对象的声明，可见Promise可以通过resolve和reject进行返回

- 通过resolve，Promise对象的状态将从Pending改变为fulfilled，即成功

- 通过reject，Promise对象的状态将从Pending改变为rejected，即失败

## 2. resolve，reject函数的声明

### 2.1 resolve函数的声明

resolve函数可以从then()函数中进行声明：

```js
new Promise((resolve,reject)=>{
    resolve('成功')
    // reject('失败')
}).then((result)=>{
    console.log(result)
})
```

当then函数执行结束后，会返回一个Promise对象，其状态为fulfilled，结果为空

若要定义返回的Promise对象的结果值，可以直接return结果

```js
new Promise((resolve,reject)=>{
    resolve('成功1')
    // reject('失败')
}).then((result)=>{
    console.log(result)
    return '成功2'
})
```

这时返回值为状态为fulfilled，结果为'成功2'的Promise对象。

这个结果还会被下一个then函数捕捉

这就是Promise的链式调用

### 2.2 reject函数的声明

reject函数可以从catch函数中进行声明，若在链式调用中，catch声明的失败处理函数将在任意Promise状态为失败时调用。

```js
new Promise((resolve,reject)=>{
    resolve('成功1')
    // reject('失败')
}).then((result)=>{
    console.log(result)
    return '成功2'
}).catch((error)=>{
    console.log(error)
})
```

## 3. 使用Promise对象解决异步回调地狱

通过异步操作的返回值改变Promise对象的状态，即可使用链式调用解决异步操作的回调地狱

```js
new Promise((resolve,reject)=>{
    // 开始异步1操作
    setTimeout(()=>{ resolve('异步1完成') },1000)
}).then((res)=>{
    console.log(res)
    return new Promise((resolve,reject)=>{
        // 开始异步2操作
        setTimeout(()=>{ resolve('异步2完成') },1000)
    })
}).then((res)=>{
    console.log(res)
    return new Promise((resolve,reject)=>{
        // 开始异步3操作
        setTimeout(()=>{ resolve('异步3完成') },1000)
    })
}).then((res)=>{
    console.log(res)
}).catch((err)=>{
    console.log(err)
})
```

## 4. 总结

通过Promise的链式调用，可以让异步操作通过同步的形式表现出来，并且没有函数的相互嵌套，使得代码更整洁，逻辑更清晰