# Promise 状态：pending、fulfilled、rejected  

1. pending: 待定，Promise的初始状态。可以settled 为fulfilled或rejected状态  

2. fulfilled: 解决，表示执行成功。Promise被resolve后的状态，状态不可改变，私有value  

3. rejected: 拒绝，表示执行失败。Promise被reject后的状态，状态不可改变，私有reason.  

## value和reason不可变，他们包含原始值或对象的不可修改的引用，默认值为undefined  

### then 方法  

要求必须提供一个then方法来访问当前最终的value或reason.  

promise.then(onFulfilled,onRejected)  

1. then方法接受两个函数作为参数，切参数可选  
2. 如果可选参数不为函数时忽略。  
3. 两个函数都是异步执行，会放入时间队列等待下一轮tick.  
4. 调用onFulfulled函数时，会将当前Promise的value作为参数传入  
5. 调用onRejected时，会将当前Promise的reason作为参数传入  
6. then函数的返回值为Promise对象  
7. then可以被同一个Promise多次调用  

### Promise 解决过程  

Promise 的解决过程是一个抽象操作，接收一个Promise和一个值X。  
针对X的值处理以下几种情况：  

1. X等于Promise  
抛出TypeError错误，拒绝Promise.  
2. X是Promise 的实例  
如果X处于待定状态，那么Promise继续等待直到X兑现或拒绝，否则根据X的状态兑现/拒绝Promise.  
3. X是对象或函数  
取出X.then并调用，调用时将this指向X.将then回调函数中取得的结果y传入新的Promise解决过程中，递归调用。  
如果执行报错，则将以对应的失败原因拒绝Promise.  
这种情况就是处理拥有then()函数的对象或函数，thenable.  
4. 如果X不是对象或函数  
以X作为值执行Promise.  

### 手写Promise  

1. 定义状态  
var PENDING = 'pending';  
var FULFILLED = 'fulfilled';  
var REJECTED = 'rejected';  

2. Promise 构造函数  
创建 Promise 时传入 execute 回调函数，接收两个参数，分别用来兑现和拒绝当前Promise.  
定义 resolve() 和 reject() 函数.  
初始状态 PENDING ,在执行时可能会有返回值 value, 在拒绝时会有拒绝原因 reason.  
同时需要注意， Promise 内部的异常不能直接抛出, 需要进行异常捕获.  

function Promise(execute) {  
    var _this = this;  
    _this.state = PENDING;  
    function resolve(value) {  
        if(_this.state === PENDING){  
           _this.state = FULFILLED;  
           _this.value = value;  
        }  
    }  
    function reject(reason) {  
        if(_this.state === PENDING){  
            _this.state = REJECTED;  
            _this.reason = reason;  
        }  
    }  
    try {  
        execute(resolve, reject);  
    } catch(e) {  
        reject(e);  
    }  
}  
