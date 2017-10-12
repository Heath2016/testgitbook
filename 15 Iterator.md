/**
 * 遍历器（Iterator）是一种接口，为各种不同的数据结构提供统一的访问机制。
 * 任何数据结构只要部署Iterator接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。
 * Iterator 的作用有三个：一是为各种数据结构，提供一个统一的、简便的访问接口；
 * 二是使得数据结构的成员能够按某种次序排列；
 * 三是ES6创造了一种新的遍历命令for...of循环，Iterator接口主要供for...of消费。
 */



/**
 * Iterator 的遍历过程
 * （1）创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。
 * （2）第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员。
 * （3）第二次调用指针对象的next方法，指针就指向数据结构的第二个成员。
 * （4）不断调用指针对象的next方法，直到它指向数据结构的结束位置。
 */

/**
 * 每一次调用next方法，都会返回数据结构的当前成员的信息。
 * 就是返回一个包含value和done两个属性的对象。
 * 其中，value属性是当前成员的值，done属性是一个布尔值，表示遍历是否结束。
 */

/**
 * 一个数据结构只要具有Symbol.iterator属性，就可以认为是“可遍历的”（iterable）。
 * Symbol.iterator属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器。
 * 
 */
const obj = {
  [Symbol.iterator] : function () {
    return {
      next: function () {
        return {
          value: 1,
          done: true
        };
      }
    };
  }
};

/**
 * 原生具备 Iterator 接口的数据结构
 * Array,Map,Set,String,TypedArray,函数的 arguments 对象,NodeList 对象
 * 对象（Object）之所以没有默认部署 Iterator 接口
 * 遍历器是一种线性处理，对于任何非线性的数据结构，部署遍历器接口，就等于部署一种线性转换。
 * 一个对象如果要具备可被for...of循环调用的 Iterator 接口，就必须在Symbol.iterator的属性上部署遍历器生成方法
 */
let obj = {
  data: [ 'hello', 'world' ],
  [Symbol.iterator]() {
    const self = this;
    let index = 0;
    return {
      next() {
        if (index < self.data.length) {
          return {
            value: self.data[index++],
            done: false
          };
        } else {
          return { value: undefined, done: true };
        }
      }
    };
  }
};

/**
 * Symbol.iterator方法的最简单实现
 */
var myIterable = {};

myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};
[...myIterable] // [1, 2, 3]

// 或者采用下面的简洁写法

let obj = {
  * [Symbol.iterator]() {
    yield 'hello';
    yield 'world';
  }
};

for (let x of obj) {
  console.log(x);
}
// hello
// world


/**
 * return:for...of循环提前退出（通常是因为出错，或者有break语句或continue语句）
 * 一个对象在完成遍历前，需要清理或释放资源，就可以部署return方法
 */




































































