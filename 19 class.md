/**
 * class 私有方法
 */
//命名上加以区别。不保险的，在类的外部，还是可以调用到这个方法。
class Widget {

  // 公有方法
  foo (baz) {
    this._bar(baz);
  }

  // 私有方法
  _bar(baz) {
    return this.snaf = baz;
  }

  // ...
}

//将私有方法移出模块，因为模块内部的所有方法都是对外可见的。

class Widget {
  foo (baz) {
    bar.call(this, baz);
  }

  // ...
}

function bar(baz) {
  return this.snaf = baz;
}

//利用Symbol值的唯一性，将私有方法的名字命名为一个Symbol值。
//bar和snaf都是Symbol值，导致第三方无法获取到它们，因此达到了私有方法和私有属性的效果。
const bar = Symbol('bar');
const snaf = Symbol('snaf');

export default class myClass{

  // 公有方法
  foo(baz) {
    this[bar](baz);
  }

  // 私有方法
  [bar](baz) {
    return this[snaf] = baz;
  }

  // ...
};

/**
 * Class 可以通过extends关键字实现继承
 * super关键字，它在这里表示父类的构造函数，用来新建父类的this对象。
 * 子类必须在constructor方法中调用super方法，否则新建实例时会报错。
 * 因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。
 * 如果不调用super方法，子类就得不到this对象。


 */

class Point {
}

class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}


/**
 * Object.getPrototypeOf方法可以用来从子类上获取父类
 */

Object.getPrototypeOf(ColorPoint) === Point// true

/**
 * super作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次super函数。
 */


/**
 * extends关键字后面可以跟多种类型的值。
 */


/**
 * Mixin 模式指的是，将多个类的接口“混入”（mix in）另一个类。		
 */






















