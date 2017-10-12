/**
 * 
 */
//将Object对象的一些明显属于语言内部的方法（比如Object.defineProperty），放到Reflect对象上。
//从Reflect对象上可以拿到语言内部的方法。
//

//修改某些Object方法的返回结果，让其变得更合理。
//Object.defineProperty(obj, name, desc)在无法定义属性时，会抛出一个错误，
//而Reflect.defineProperty(obj, name, desc)则会返回false。  
// 老写法
try {
  Object.defineProperty(target, property, attributes);
  // success
} catch (e) {
  // failure
}

// 新写法
if (Reflect.defineProperty(target, property, attributes)) {
  // success
} else {
  // failure
}

//让Object操作都变成函数行为。
//某些Object操作是命令式，比如name in obj和delete obj[name]，
//而Reflect.has(obj, name)和Reflect.deleteProperty(obj, name)让它们变成了函数行为。
// 老写法
'assign' in Object // true

// 新写法
Reflect.has(Object, 'assign') // true

//Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法。

//采用Reflect.set方法将值赋值给对象的属性，确保完成原有的行为，然后再部署额外的功能。

Proxy(target, {
  set: function(target, name, value, receiver) {
    var success = Reflect.set(target,name, value, receiver);
    if (success) {
      log('property ' + name + ' on ' + target + ' set to ' + value);
    }
    return success;
  }
});


/**
 * 静态方法
	Reflect.apply(target,thisArg,args)
	Reflect.construct(target,args)
	Reflect.get(target,name,receiver)
	Reflect.set(target,name,value,receiver)
	Reflect.defineProperty(target,name,desc)
	Reflect.deleteProperty(target,name)
	Reflect.has(target,name)
	Reflect.ownKeys(target)
	Reflect.isExtensible(target)
	Reflect.preventExtensions(target)
	Reflect.getOwnPropertyDescriptor(target, name)
	Reflect.getPrototypeOf(target)
	Reflect.setPrototypeOf(target, prototype)
 */

/**
 * Reflect.get(target, name, receiver)
 * Reflect.get方法查找并返回target对象的name属性，如果没有该属性，则返回undefined。
 * 如果第一个参数不是对象，Reflect.get方法会报错。
 */

var myObject = {
  foo: 1,
  bar: 2,
  get baz() {
    return this.foo + this.bar;
  },
}

Reflect.get(myObject, 'foo') // 1
Reflect.get(myObject, 'bar') // 2
Reflect.get(myObject, 'baz') // 3


/**
 * Reflect.set(target, name, value, receiver)
 * Reflect.set方法设置target对象的name属性等于value。
 * 如果第一个参数不是对象，Reflect.set会报错。
 */
var myObject = {
  foo: 1,
  set bar(value) {
    return this.foo = value;
  },
}

myObject.foo // 1

Reflect.set(myObject, 'foo', 2);
myObject.foo // 2

Reflect.set(myObject, 'bar', 3)
myObject.foo // 3


//如果name属性设置了赋值函数，则赋值函数的this绑定receiver。
var myObject = {
  foo: 4,
  set bar(value) {
    return this.foo = value;
  },
};

var myReceiverObject = {
  foo: 0,
};

Reflect.set(myObject, 'bar', 1, myReceiverObject);
myObject.foo // 4
myReceiverObject.foo // 1
//
//
//
/**
 * 实例：使用 Proxy 实现观察者模式
 * 观察者模式（Observer mode）指的是函数自动观察数据对象，一旦对象有变化，函数就会自动执行。
 */
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















