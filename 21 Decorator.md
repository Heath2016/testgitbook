/**
 * Decorator函数，用来修改类的行为。
 */
@decorator
class A {}

// 等同于

class A {}
A = decorator(A) || A;




@testable
class MyTestableClass {
  // ...
}

function testable(target) {
  target.isTestable = true;
}

MyTestableClass.isTestable // true



/**
 * 修饰器testable可以接受参数，这就等于可以修改修饰器的行为。
 */
function testable(isTestable) {
  return function(target) {
    target.isTestable = isTestable;
  }
}

@testable(true)
class MyTestableClass {}
MyTestableClass.isTestable // true

@testable(false)
class MyClass {}
MyClass.isTestable // false


/**
 * 添加实例属性，可以通过目标类的prototype对象操作。
 */
function testable(target) {
  target.prototype.isTestable = true;
}

@testable
class MyTestableClass {}

let obj = new MyTestableClass();
obj.isTestable // true

/**
 * 修饰器函数一共可以接受三个参数，
 * 第一个参数是所要修饰的目标对象，即类的实例（这不同于类的修饰，那种情况时target参数指的是类本身）；
 * 第二个参数是所要修饰的属性名，第三个参数是该属性的描述对象。
 */


/**
 * core-decorators.js是一个第三方模块，
 * autobind修饰器使得方法中的this对象，绑定原始对象。
 * readonly修饰器使得属性或方法不可写。
 * override修饰器检查子类的方法，是否正确覆盖了父类的同名方法，如果不正确会报错。
 * deprecate或deprecated修饰器在控制台显示一条警告，表示该方法将废除。
 */


























































