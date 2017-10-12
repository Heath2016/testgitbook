/**
 * export命令用于规定模块的对外接口，
 * import命令用于输入其他模块提供的功能。
 * export命令可以出现在模块的任何位置，只要处于模块顶层就可以。
 */

/**
 * 一个模块就是一个独立的文件。该文件内部的所有变量，外部无法获取。
 * 如果你希望外部能够读取模块内部的某个变量，就必须使用export关键字输出该变量。
 */
// profile.js
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;

// profile.js
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};

/**
 * 输出函数或类（class）。
 */

export function multiply(x, y) {
  return x * y;
};

/**
 * export输出的变量就是本来的名字，但是可以使用as关键字重命名
 * export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。
 */

function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};



// 报错
export 1;

// 报错
var m = 1;
export m;


// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};


// 报错
function f() {}
export f;

// 正确
export function f() {};

// 正确
function f() {}
export {f};





/**
 * 文件就可以通过import命令加载模块。
 */


/**
 * import命令接受一对大括号，里面指定要从其他模块导入的变量名。
 * 大括号里面的变量名，必须与被导入模块（profile.js）对外接口的名称相同。
 * 使用as关键字，将输入的变量重命名。
 * from指定模块文件的位置，可以是相对路径，也可以是绝对路径
 * 如果只是模块名，不带有路径，那么必须有配置文件，告诉 JavaScript 引擎该模块的位置。
 * import命令具有提升效果，会提升到整个模块的头部，首先执行。
 * 由于import是静态执行，所以不能使用表达式和变量，
 */
// main.js
import {firstName, lastName, year} from './profile';

function setName(element) {
  element.textContent = firstName + ' ' + lastName;
}



/**
 * 整体加载，即用星号（*）指定一个对象，所有输出值都加载在这个对象上面。
 */


// 
// circle.js

export function area(radius) {
  return Math.PI * radius * radius;
}

export function circumference(radius) {
  return 2 * Math.PI * radius;
}

//逐一指定要加载的方法
import { area, circumference } from './circle';

console.log('圆面积：' + area(4));
console.log('圆周长：' + circumference(14));

//整体加载
import * as circle from './circle';

console.log('圆面积：' + circle.area(4));
console.log('圆周长：' + circle.circumference(14));



/**
 * export default命令，为模块指定默认输出。
 */

//export default命令，为模块指定默认输出。
// export-default.js
export default function () {
  console.log('foo');
}


//其他模块加载该模块时，import命令可以为该匿名函数指定任意名字。
//import命令后面，不使用大括号。
//一个模块只能有一个默认输出，因此export default命令只能使用一次。
//export default本质是将该命令后面的值，赋给default变量以后再默认
// import-default.js
import customName from './export-default';
customName(); // 'foo'

// 第一组
export default function crc32() { // 输出
  // ...
}

import crc32 from 'crc32'; // 输入

// 第二组
export function crc32() { // 输出
  // ...
};

import {crc32} from 'crc32'; // 输入


/**
 * 先输入后输出同一个模块，import语句可以与export语句写在一起。
 */

export { foo, bar } from 'my_module';

// 等同于
import { foo, bar } from 'my_module';
export { foo, bar };

//模块的接口改名和整体输出

// 接口改名
export { foo as myFoo } from 'my_module';

// 整体输出
export * from 'my_module';


//默认接口的写法
export { default } from 'foo';

// 具名接口改为默认接口的写法
export { es6 as default } from './someModule';

// 等同于
import { es6 } from './someModule';
export default es6;

//默认接口也可以改名为具名接口。
export { default as es6 } from './someModule';


//模块继承。
//假设有一个circleplus模块，继承了circle模块。

export * from 'circle';
export var e = 2.71828182846;
export default function(x) {
  return Math.exp(x);
}



/**
 * const声明的常量只在当前代码块有效。
 * 如果想设置跨模块的常量（即跨多个文件），
 * 或者说一个值要被多个模块共享
 */
// constants.js 模块
export const A = 1;
export const B = 3;
export const C = 4;

// test1.js 模块
import * as constants from './constants';
console.log(constants.A); // 1
console.log(constants.B); // 3

// test2.js 模块
import {A, B} from './constants';
console.log(A); // 1
console.log(B); // 3


//使用的常量非常多，可以建一个专门的constants目录，
//将各种常量写在不同的文件里面，保存在该目录下。
//将这些文件输出的常量，合并在index.js里面。

// constants/db.js
export const db = {
  url: 'http://my.couchdbserver.local:5984',
  admin_username: 'admin',
  admin_password: 'admin password'
};

// constants/user.js
export const users = ['root', 'admin', 'staff', 'ceo', 'chief', 'moderator'];


// constants/index.js
export {db} from './db';
export {users} from './users';


/**
 * 脚本异步加载
 * defer等到整个页面正常渲染结束，才会执行，如果有多个defer脚本，会按照它们在页面出现的顺序加载
 * async一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染。多个async脚本是不能保证加载顺序的。
 */

// <script src="path/to/myModule.js" defer></script>
// <script src="path/to/myModule.js" async></script>

/**
 * 浏览器加载 ES6 模块
 * 带有type="module"的<script>，都是异步加载，不会造成堵塞浏览器，
 * 即等到整个页面渲染完，再执行模块脚本，等同于打开了<script>标签的defer属性。
 */

// <script type="module" src="foo.js"></script>


/**
 * 外部的模块脚本
 * 代码是在模块作用域之中运行，而不是在全局作用域运行。模块内部的顶层变量，外部不可见。
 * 模块脚本自动采用严格模式，不管有没有声明use strict。
 * 模块之中，可以使用import命令加载其他模块（.js后缀不可省略，需要提供绝对 URL 或相对 URL），也可以使用export命令输出对外接口。
 * 模块之中，顶层的this关键字返回undefined，而不是指向window。也就是说，在模块顶层使用this关键字，是无意义的。
 * 同一个模块如果加载多次，将只执行一次。
 */

import utils from 'https://example.com/js/utils.js';

const x = 1;

console.log(x === window.x); //false
console.log(this === undefined); // true

delete x; // 句法错误，严格模式禁止删除变量

































































































