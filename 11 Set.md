/**
 * Set:成员的值都是唯一的，没有重复的值。
 */
// Set 本身是一个构造函数，用来生成 Set 数据结构.Set 结构不会添加重复的值。

const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4

// Set 函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。
// 例一：去重
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

// 例二
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5

// 例三
function divs () {
  return [...document.querySelectorAll('div')];
}

const set = new Set(divs());
set.size // 56

// 类似于
divs().forEach(div => set.add(div));
set.size // 56



// Set 操作方法（用于操作数据）
// add(value)：添加某个值，返回Set结构本身。
// delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
// has(value)：返回一个布尔值，表示该值是否为Set的成员。
// clear()：清除所有成员，没有返回值。

s.add(1).add(2).add(2);
// 注意2被加入了两次

s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2);
s.has(2) // false


//去除数组重复成员
function dedupe(array) {
  return Array.from(new Set(array));
}

dedupe([1, 1, 2, 3]) // [1, 2, 3]


// 遍历方法:Set的遍历顺序就是插入顺序。
// keys()：返回键名的遍历器
// values()：返回键值的遍历器
// entries()：返回键值对的遍历器
// forEach()：使用回调函数遍历每个成员


let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]


//Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的values方法。
//可以省略values方法，直接用for...of循环遍历 Set。

let set = new Set(['red', 'green', 'blue']);

for (let x of set) {
  console.log(x);
}
// red
// green
// blue

// Set结构的实例的forEach方法，用于对每个成员执行某种操作，没有返回值。
let set = new Set([1, 2, 3]);
set.forEach((value, key) => console.log(value * 2) )
// 2
// 4
// 6


//扩展运算符（...）内部使用for...of循环，所以也可以用于 Set 结构。

let set = new Set(['red', 'green', 'blue']);
let arr = [...set];
// ['red', 'green', 'blue']

//扩展运算符和 Set 结构相结合，就可以去除数组的重复成员。

let arr = [3, 5, 2, 2, 5, 5];
let unique = [...new Set(arr)];
// [3, 5, 2]



//数组的map和filter方法也可以用于 Set 了。

let set = new Set([1, 2, 3]);
set = new Set([...set].map(x => x * 2));
// 返回Set结构：{2, 4, 6}

let set = new Set([1, 2, 3, 4, 5]);
set = new Set([...set].filter(x => (x % 2) == 0));
// 返回Set结构：{2, 4}



//let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}


/**
 * WeakSet 的成员只能是对象，而不能是其他类型的值。
 * WeakSet 中的对象都是弱引用，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存
 * WeakSet 适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在 WeakSet 里面的引用就会自动消失。
 */

//WeakSet 是一个构造函数，可以使用new命令，创建 WeakSet 数据结构。
const ws = new WeakSet();






function test(){
const map = new Map();
let a = ['a']

map.set(a, 555);
console.log(map.get(a) )// undefined
}

test();












































