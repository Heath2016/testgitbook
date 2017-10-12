/**
 * Generator函数是一种异步编程解决方案
 * Generator 函数是一个状态机，封装了多个内部状态。
 * 一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。
 */

/**
 * 特征
 * function关键字与函数名之间有一个星号
 * 函数体内部使用yield表达式，定义不同的内部状态
 */

/**
 * 函数有三个状态：hello，world 和 return 语句（结束执行）。
 * 调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象
 * 调用遍历器对象的next方法，使得指针移向下一个状态。
 * 每次调用next方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个yield表达式（或return语句）为止。
 * Generator 函数是分段执行的，yield表达式是暂停执行的标记，而next方法可以恢复执行。
 */

/**
 * 调用 Generator 函数，返回一个遍历器对象，代表 Generator 函数的内部指针。
 * 每次调用遍历器对象的next方法，就会返回一个有着value和done两个属性的对象。
 * value属性表示当前的内部状态的值，是yield表达式后面那个表达式的值；
 * done属性是一个布尔值，表示是否遍历结束。
 */
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();

hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }


/**
 * yield表达式就是暂停标志。
 * 遇到yield表达式，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。
 * 下一次调用next方法时，再继续往下执行，直到遇到下一个yield表达式。
 * 如果没有再遇到新的yield表达式，就一直运行到函数结束，直到return语句为止，
 * 并将return语句后面的表达式的值，作为返回的对象的value属性值。
 * 如果该函数没有return语句，则返回的对象的value属性值为undefined。
 */

/**
 * yield表达式只能用在 Generator 函数里面，用在其他地方都会报错。
 */
var arr = [1, [[2, 3], 4], [5, 6]];

var flat = function* (a) {
  a.forEach(function (item) {
    if (typeof item !== 'number') {
      yield* flat(item);
    } else {
      yield item;
    }
  });
};

for (var f of flat(arr)){
  console.log(f); //Uncaught SyntaxError: Unexpected identifier
}


var arr = [1, [[2, 3], 4], [5, 6]];

var flat = function* (a) {
  var length = a.length;
  for (var i = 0; i < length; i++) {
    var item = a[i];
    if (typeof item !== 'number') {
      yield* flat(item);
    } else {
      yield item;
    }
  }
};

for (var f of flat(arr)) {
  console.log(f);
}
// 1, 2, 3, 4, 5, 6

/**
 * yield表达式如果用在另一个表达式之中，必须放在圆括号里面。
 * yield表达式用作函数参数或放在赋值表达式的右边，可以不加括号。
 */
function* demo() {
  console.log('Hello' + yield); // SyntaxError
  console.log('Hello' + yield 123); // SyntaxError

  console.log('Hello' + (yield)); // OK
  console.log('Hello' + (yield 123)); // OK

  foo(yield 'a', yield 'b'); // OK
  let input = yield; // OK
}


/**
 * yield表达式本身没有返回值，或者说总是返回undefined。
 * next方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值。
 */

function* f() {
  for(var i = 0; true; i++) {
    var reset = yield i;
    if(reset) { i = -1; console.log(i)}
    console.log(reset,i)
  }
}

var g = f();

g.next() // { value: 0, done: false }
g.next() // { value: 1, done: false }
g.next() // { value: 2, done: false }
g.next(true) // { value: 0, done: false }
g.next(true) // { value: 0, done: false }









/**
 * yield*表达式,在 Generator 函数内部，调用另一个 Generator 函数
 */

function* foo() {
  yield 'a';
  yield 'b';
}
function* bar() {
  yield 'x';
  yield* foo();
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  yield 'a';
  yield 'b';
  yield 'y';
}

/**
 * yield表达式后面跟的是一个遍历器对象，需要在yield表达式后面加上星号，表明它返回的是一个遍历器对象。这被称为yield*表达式。
 */
let delegatedIterator = (function* () {
  yield 'Hello!';
  yield 'Bye!';
}());

let delegatingIterator = (function* () {
  yield 'Greetings!';
  yield* delegatedIterator;
  yield 'Ok, bye.';
}());

for(let value of delegatingIterator) {
  console.log(value);
}
// "Greetings!
// "Hello!"
// "Bye!"
// "Ok, bye."





/**
 * 在 Generator 函数运行的不同阶段，从外部向内部注入不同的值，从而调整函数行为。
 * next方法的参数表示上一个yield表达式的返回值，所以在第一次使用next方法时，传递参数是无效的。
 * next方法不含参数。yield表达式返回的是undefined
 */
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}

var a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:true}

var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }








/**
 * 把 Generator 赋值给对象的Symbol.iterator属性，从而使得该对象具有 Iterator 接口。
 */
var myIterable = {};
myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};

[...myIterable] // [1, 2, 3]



/**
 * Generator 是实现状态机的最佳结构。
 * 每运行一次，就改变一次状态
 */
var clock = function* () {
  while (true) {
    console.log('Tick!');
    yield;
    console.log('Tock!');
    yield;
  }
};


/**
 * 协程（coroutine）是一种程序运行的方式，可以理解成“协作的线程”或“协作的函数”。
 * 协程既可以用单线程实现，也可以用多线程实现。
 */

/**
 * 传统的“子例程”（subroutine）采用堆栈式“后进先出”的执行方式，只有当调用的子函数完全执行完毕，才会结束执行父函数。
 * 协程,多个线程（单线程情况下，即多个函数）可以并行执行，但是只有一个线程（或函数）处于正在运行的状态，
 * 其他线程（或函数）都处于暂停态（suspended），线程（或函数）之间可以交换执行权。
 * 也就是说，一个线程（或函数）执行到一半，可以暂停执行，将执行权交给另一个线程（或函数），
 * 等到稍后收回执行权的时候，再恢复执行。
 * 这种可以并行执行、交换执行权的线程（或函数），就称为协程。
 * 在内存中，子例程只使用一个栈（stack），而协程是同时存在多个栈，但只有一个栈是在运行状态，
 * 也就是说，协程是以多占用内存为代价，实现多任务的并行。
 */

/**
 * 协程适合用于多任务运行的环境
 * 同一时间可以有多个线程处于运行状态，但是运行的协程只能有一个，其他协程都处于暂停状态。
 * 普通的线程是抢先式的，到底哪个线程优先得到资源，必须由运行环境决定，
 * 但是协程是合作式的，执行权由协程自己分配。
 */

/**
 * Generator 函数的暂停执行的效果，意味着可以把异步操作写在yield表达式里面，等到调用next方法时再往后执行。
 * 处理异步操作，改写回调函数。
 */
function* loadUI() {
  showLoadingScreen();
  yield loadUIDataAsynchronously();
  hideLoadingScreen();
}
var loader = loadUI();
// 加载UI
loader.next()

// 卸载UI
loader.next()

/**
 * 控制流管理
 */
let steps = [step1Func, step2Func, step3Func];

function *iterateSteps(steps){
  for (var i=0; i< steps.length; i++){
    var step = steps[i];
    yield step();
  }
}

let jobs = [job1, job2, job3];

function* iterateJobs(jobs){
  for (var i=0; i< jobs.length; i++){
    var job = jobs[i];
    yield* iterateSteps(job.steps);
  }
}






















