/**
 * 异步"，简单说就是一个任务不是连续完成的，
 * 可以理解成该任务被人为分成两段，先执行第一段，然后转而执行其他任务，等做好了准备，再回过头执行第二段。
 */

/**
 * 把任务的第二段单独写在一个函数里面，等到重新执行这个任务的时候，就直接调用这个函数。
 * 第一段执行完以后，任务所在的上下文环境就已经结束了。
 * 在这以后抛出的错误，原来的上下文环境已经无法捕捉，只能当作参数，传入第二段
 */
fs.readFile('/etc/passwd', 'utf-8', function (err, data) {
  if (err) throw err;
  console.log(data);
});

/**
 * Promise 对象允许将回调函数的嵌套，改成链式调用。不是新的语法功能，而是一种新的写法，
 * Promise 的最大问题是代码冗余，原来的任务被 Promise 包装了一下，不管什么操作，一眼看去都是一堆then，原来的语义变得很不清楚。
 */
var readFile = require('fs-readfile-promise');

readFile(fileA)
.then(function (data) {
  console.log(data.toString());
})
.then(function () {
  return readFile(fileB);
})
.then(function (data) {
  console.log(data.toString());
})
.catch(function (err) {
  console.log(err);
});


/**
 * "协程"（coroutine），多个线程互相协作，完成异步任务。
 * 第一步，协程A开始执行。
 * 第二步，协程A执行到一半，进入暂停，执行权转移到协程B。
 * 第三步，（一段时间后）协程B交还执行权。
 * 第四步，协程A恢复执行。
 */

function* asyncJob() {
  // ...其他代码
  var f = yield readFile(fileA);
  // ...其他代码
}

/**
 * Generator可以交出函数的执行权（即暂停执行）。
 * 异步操作需要暂停的地方，都用yield语句注明。
 * Generator 函数可以暂停执行和恢复执行，这是它能封装异步任务的根本原因。
 */

/**
 * next返回值的value属性，是 Generator 函数向外输出数据；next方法还可以接受参数，向 Generator 函数体内输入数据。
 */

function* gen(x){
  var y = yield x + 2;
  return y;
}

var g = gen(1);
g.next() // { value: 3, done: false }
g.next(2) // { value: 2, done: true }

/**
 * Generator 函数内部还可以部署错误处理代码，捕获函数体外抛出的错误。
 */
function* gen(x){
  try {
    var y = yield x + 2;
  } catch (e){
    console.log(e);
  }
  return y;
}

var g = gen(1);
g.next();
g.throw('出错了');
// 出错了

/**
 * 如何使用 Generator 函数，执行一个真实的异步任务。
 *  Generator 函数将异步操作表示得很简洁，但是流程管理却不方便（即何时执行第一阶段、何时执行第二阶段）。
 */

var fetch = require('node-fetch');

function* gen(){
  var url = 'https://api.github.com/users/github';
  var result = yield fetch(url);
  console.log(result.bio);
}

var g = gen();
var result = g.next();

result.value.then(function(data){
  return data.json();
}).then(function(data){
  g.next(data);
});


/**
 * "传值调用"（call by value），即在进入函数体之前，就计算x + 5的值（等于6），再将这个值传入函数
 * “传名调用”（call by name），即直接将表达式x + 5传入函数体，只在用到它的时候求值
 * Thunk 函数:编译器的“传名调用”实现，往往是将参数放到一个临时函数之中，再将这个临时函数传入函数体。
 * 
 */











































