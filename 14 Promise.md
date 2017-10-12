/**
 * Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。
 * 从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。
 * 对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。
 * 一旦状态改变，就不会再变，任何时候都可以得到这个结果。
 * Promise对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。
 * Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise对象提供统一的接口，使得控制异步操作更加容易。
 */

/**
 * Promise也有一些缺点。
 * 首先，无法取消Promise，一旦新建它就会立即执行，无法中途取消。
 * 其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。
 * 第三，当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。
 * 如果某些事件不断地反复发生，一般来说，使用 Stream 模式是比部署Promise更好的选择。
 */

/**
 * Promise对象是一个构造函数，用来生成Promise实例。
 */

// Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。
// resolve函数,将Promise对象的状态从“未完成”变为“成功”，在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；
// reject函数,将Promise对象的状态从“未完成”变为“失败”，在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

var promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
// Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。
// then方法可以接受两个回调函数作为参数。
// 第一个回调函数是Promise对象的状态变为resolved时调用，
// 第二个回调函数是Promise对象的状态变为rejected时调用。
// 其中，第二个函数是可选的，不一定要提供。

promise.then(function(value) {
  // success
}, function(error) {
  // failure
});

//timeout方法返回一个Promise实例，表示一段时间以后才会发生的结果
function timeout(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, 'done');
  });
}

timeout(100).then((value) => {
  console.log(value);
});

//异步加载图片
function loadImageAsync(url) {
  return new Promise(function(resolve, reject) {
    var image = new Image();

    image.onload = function() {
      resolve(image);
    };

    image.onerror = function() {
      reject(new Error('Could not load image at ' + url));
    };

    image.src = url;
  });
}

//实现的 Ajax 操作
//调用resolve函数和reject函数时带有参数，那么它们的参数会被传递给回调函数。
//立即 resolved 的 Promise 是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。
//
var getJSON = function(url) {
  var promise = new Promise(function(resolve, reject){
    var client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

    function handler() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
  });

  return promise;
};

getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});

/**
 * Promise.prototype.then()
 * 为 Promise 实例添加状态改变时的回调函数。
 * then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。
 * 因此可以采用链式写法，即then方法后面再调用另一个then方法。
 */
getJSON("/post/1.json").then(function(post) {
  return getJSON(post.commentURL);
}).then(function funcA(comments) {
  console.log("resolved: ", comments);
}, function funcB(err){
  console.log("rejected: ", err);
});
/**
 * Promise.prototype.catch()
 * 是.then(null, rejection)的别名，用于指定发生错误时的回调函数。
 */
p.then((val) => console.log('fulfilled:', val))
  .catch((err) => console.log('rejected', err));

// 等同于
p.then((val) => console.log('fulfilled:', val))
  .then(null, (err) => console.log("rejected:", err));
/**
 * Promise.all方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。
 * Promise.all方法接受一个数组作为参数，p1、p2、p3都是 Promise 实例，
 * 如果不是，就会先调用下面讲到的Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。
 * （Promise.all方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。）
 * 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，
 * 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected
 */
var p = Promise.all([p1, p2, p3]);


// 生成一个Promise对象的数组
var promises = [2, 3, 5, 7, 11, 13].map(function (id) {
  return getJSON('/post/' + id + ".json");
});

Promise.all(promises).then(function (posts) {
  // ...
}).catch(function(reason){
  // ...
});
/**
 * 只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。
 * 
 */

var p = Promise.race([p1, p2, p3]);

//如果指定时间内没有获得结果，就将Promise的状态变为reject，否则变为resolve。
const p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
]);
p.then(response => console.log(response));
p.catch(error => console.log(error));
/**
 * 将现有对象转为Promise对象
 */
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
//参数是一个Promise实例,Promise.resolve将不做任何修改、原封不动地返回这个实例。




//参数是一个thenable对象,Promise.resolve方法会将这个对象转为Promise对象，然后就立即执行thenable对象的then方法。
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};

let p1 = Promise.resolve(thenable);
p1.then(function(value) {
  console.log(value);  // 42
});

//thenable对象指的是具有then方法的对象
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};

//参数不是具有then方法的对象，或根本就不是对象,Promise.resolve方法返回一个新的Promise对象，状态为resolved。
var p = Promise.resolve('Hello');

p.then(function (s){
  console.log(s)// Hello
});

// Promise.resolve方法允许调用时不带参数，直接返回一个resolved状态的Promise对象。
// setTimeout(fn, 0)在下一轮“事件循环”开始时执行，
setTimeout(function () {
  console.log('three');
}, 0);

Promise.resolve().then(function () {
  console.log('two');
});

console.log('one');

// one
// two
// three



/**
 * Promise.reject(reason)方法也会返回一个新的 Promise 实例，该实例的状态为rejected。
 * Promise.reject()方法的参数，会原封不动地作为reject的理由，变成后续方法的参数。
 */
var p = Promise.reject('出错了');
// 等同于
var p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s) {
  console.log(s)// 出错了
});

/**
 * 部署两个不在ES6之中、但很有用的方法。
 */
/**
 * done方法，总是处于回调链的尾端，保证抛出任何可能出现的错误。
 * Promise对象的回调链，不管以then方法或catch方法结尾，
 * 要是最后一个方法抛出错误，都有可能无法捕捉到（因为Promise内部的错误不会冒泡到全局）
 */
asyncFunc().then(f1).catch(r1).then(f2)
  .done();
//实现代码
Promise.prototype.done = function (onFulfilled, onRejected) {
  this.then(onFulfilled, onRejected)
    .catch(function (reason) {
      // 抛出一个全局错误
      setTimeout(() => { throw reason }, 0);
    });
};
//
//
/**
 * finally方法用于指定不管Promise对象最后状态如何，都会执行的操作。
 * 它与done方法的最大区别，它接受一个普通的回调函数作为参数，该函数不管怎样都必须执行。
 */
server.listen(0)
  .then(function () {
    // run test
  })
  .finally(server.stop);
//实现
Promise.prototype.finally = function (callback) {
  let P = this.constructor;
  return this.then(
    value  => P.resolve(callback()).then(() => value),
    reason => P.resolve(callback()).then(() => { throw reason })
  );
};
//
//
/**
 * 图片的加载
 */
const preloadImage = function (path) {
  return new Promise(function (resolve, reject) {
    var image = new Image();
    image.onload  = resolve;
    image.onerror = reject;
    image.src = path;
  });
};



//使用Generator函数管理流程，遇到异步操作的时候，通常返回一个Promise对象。
function getFoo () {
  return new Promise(function (resolve, reject){
    resolve('foo');
  });
}

var g = function* () {
  try {
    var foo = yield getFoo();
    console.log(foo);
  } catch (e) {
    console.log(e);
  }
};

function run (generator) {
  var it = generator();

  function go(result) {
    if (result.done) return result.value;

    return result.value.then(function (value) {
      return go(it.next(value));
    }, function (error) {
      return go(it.throw(error));
    });
  }

  go(it.next());
}

run(g);
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//