# es6-Promise
Promise学习笔记
# Promise
翻译：承诺

作用：解决异步回调问题，传统方式大部分用回调函数，事件
## 语法
```js
let a = 10
let promise = new Promise(function(resolve,reject){
  if(a == 10){
    resolve()
  }else{
    reject()
  }
})

promise.then(res=>{
  console.log(res)
},err=>{
  console.log(err)
})

/*
  promise.then(res=>{
    console.log(res)
  })
  then的返回值还是 promise,所以可以直接跟catch

*/
promise.then(res=>{
  console.log(res)
}).catch(err=>{
  console.log(err)
})

```

## Promise.resolve() 将现有的东西，转成一个promise对象，resolve状态，成功的状态
```js
let p1 = Promise.resolve('aaa')
//等价于
let p2 = new Promose(resolve=>{
  resolve('aaa')
})

```

## Promise.reject() 将现有的东西，转成一个promise对象，resolve状态，成功的状态
```js
let p1 = Promise.reject('aaa')
p1.then(res=>{
  console.log(res)
}).catch(err=>{
  console.log(err)
})
//等价于
let p2 = new Promose((resolve,reject)=>{
  reject('aaa')
})

```
## Promise.all([p1,p2,p3]) 将Promise打包,但是必须确保所有的Promise对象，都是resolve的状态，否则就会报错。
```js
let p1 = Promise.resolve('aaa');
let p2 = Promise.resolve('ccc');
let p3 = Promise.resolve('bbb');

Promise.all([p1,p2,p3]).than(res=>{
  let [res1,res2,res3] = res;
  console.log(res1,res2,res3);  //['aaa','ccc','bbb']
})
```
## Promise.race([p1,p2,p3]) 将Promise打包,只要有一个成功就返回了。

##  模拟用户登陆
```js
  let status = 1;
	let userLogin = (resolve,reject)=>{
	  setTimeout(()=>{
	    if(status == 1){
	      resolve({
	        data:'sssss',
	        msg:'ssds',
	        token:'sasdasd'
	      })
	    }else{
	      reject('失败了')
	    }
	  },1000)
	}

	let getUserInfo = (resolve,reject)=>{
	  setTimeout(()=>{
	    if(status == 1){
	      resolve({
	        data:'获取用户用户信息成功',
	        msg:'ssdadds',
	        token:'sasddwdasd'
	      })
	    }else{
	      reject('失败了')
	    }
	  },2000)
	}

	new Promise(userLogin).then(res=>{
	  console.log('登陆成功');
	  return new Promise(getUserInfo);
	}).then(res=>{
	  console.log('获取信息成功');
	  console.log(res)
	})

	let p1 = Promise.all([new Promise(userLogin),new Promise(getUserInfo)]).then(doneCallbacks=>{
		console.log(doneCallbacks)
	}, failCallbacks=>{
		console.log(failCallbacks)
	})
```
