/**
 * 各种类型的值（包括对象）都可以当作键。
 * Map结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。
 */

const m = new Map();
const o = {p: 'Hello World'};

m.set(o, 'content')
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false

const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"


// 任何具有 Iterator 接口、且每个成员都是一个双元素的数组的数据结构（详见《Iterator》一章）都可以当作Map构造函数的参数。
const set = new Set([
  ['foo', 1],
  ['bar', 2]
]);
const m1 = new Map(set);
m1.get('foo') // 1

const m2 = new Map([['baz', 3]]);
const m3 = new Map(m2);
m3.get('baz') // 3


//Map 的键实际上是跟内存地址绑定的，只要内存地址不一样，就视为两个键。
//这就解决了同名属性碰撞（clash）的问题，我们扩展别人的库的时候，
//如果使用对象作为键名，就不用担心自己的属性与原作者的属性同名。
const map = new Map();

const k1 = ['a'];
const k2 = ['a'];

map
.set(k1, 111)
.set(k2, 222);

map.get(k1) // 111
map.get(k2) // 222

/**
 * 属性
 */
// size: 返回 Map 结构的成员总数。
const map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2

//set(key, value) 设置键名key对应的键值为value，然后返回整个 Map 结构。如果key已经有值，则键值会被更新，否则就新生成该键。

const m = new Map();

m.set('edition', 6)        // 键是字符串
m.set(262, 'standard')     // 键是数值
m.set(undefined, 'nah')    // 键是 undefined

//set方法返回的是当前的Map对象，因此可以采用链式写法。
let map = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');

// get方法读取key对应的键值，如果找不到key，返回undefined。
const m = new Map();

const hello = function() {console.log('hello');};
m.set(hello, 'Hello ES6!') // 键是函数

m.get(hello)  // Hello ES6!

// has(key) has方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。
const m = new Map();

m.set('edition', 6);
m.set(262, 'standard');
m.set(undefined, 'nah');

m.has('edition')     // true
m.has('years')       // false
m.has(262)           // true
m.has(undefined)     // true

// delete(key)删除某个键，返回true。如果删除失败，返回false。
const m = new Map();
m.set(undefined, 'nah');
m.has(undefined)     // true

m.delete(undefined)
m.has(undefined)       // false

// clear() 清除所有成员，没有返回值。
let map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
map.clear()
map.size // 


/**
 * 遍历方法
 * keys()：返回键名的遍历器。
 * values()：返回键值的遍历器。
 * entries()：返回所有成员的遍历器。
 * forEach()：遍历 Map 的所有成员。
 * Map 的遍历顺序就是插入顺序。	
 */

const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"


// Map 结构转为数组结构，比较快速的方法是使用扩展运算符（...）。
const map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

[...map.keys()]
// [1, 2, 3]

[...map.values()]
// ['one', 'two', 'three']

[...map.entries()]
// [[1,'one'], [2, 'two'], [3, 'three']]

[...map]
// [[1,'one'], [2, 'two'], [3, 'three']]



/**
 * WeakMap:用于生成键值对的集合。
 * 
 */




























