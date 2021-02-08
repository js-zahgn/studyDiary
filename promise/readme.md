## Promise 状态：pending、fulfilled、rejected  

pending: 待定，Promise的初始状态。可以settled 为fulfilled或rejected状态  

fulfilled: 解决，表示执行成功。Promise被resolve后的状态，状态不可改变，私有value  

rejected: 拒绝，表示执行失败。Promise被reject后的状态，状态不可改变，私有reason.  

value和reason不可变，他们包含原始值或对象的不可修改的引用，默认值为undefined.  


## then 方法

要求必须提供一个then方法来访问当前最终的value或reason.  

promise.then(onFulfilled,onRejected)  
  1. then方法接受两个函数作为参数，切参数可选  
  2. 如果可选参数不为函数时忽略。  
  3. 两个函数都是异步执行，会放入时间队列等待下一轮tick.  
  4. 调用onFulfulled函数时，会将当前Promise的value作为参数传入  
  5. 调用onRejected时，会将当前Promise的reason作为参数传入  
  6. then函数的返回值为Promise对象  
  7. then可以被同一个Promise多次调用  


