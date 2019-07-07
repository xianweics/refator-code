# 重构 - 改善既有代码的设计




### 12.9 折叠继承体系

- 目的：
	- 合并子类，减少不必要的子类，降低系统复杂度


- 场景：
	- 当超类与子类没有多大差别


- 例子：
	```
	class Employee {}
	class Sales extends Employee {}
	```
	```
	class Employee {}
	```

### 12.10 以委托取代子类

- 目的：
	- 增强类的扩展性


- 场景：
	- 当子类可能存在多种类型上的变化


- 例子
	``` bad
	class Booking {
		constuctor(show, date) {
			this._show = show;
			this._date = date;
		}
	}
	
	class PremiumBooking extends Booking {
		constructor(show, date, extras) {
			super(show, date);
			this._extras = extras;
		}
	}
	```
	``` good
	class Booking {
		constuctor(show, date) {
			this._show = show;
			this._date = date;
			this._premium = null;
		}
		bePremium(extras) {
			this._premium = new PremiumBookingDelegate(this, extras);
		}
	}
	
	class PremiumBookingDelegate {
		constructor(root, extras) {
			this._root = root;
			this._extras = extras;
		}
	}
	```

### 12.11 以委托取代超类

- 目的：
	- 增强类的扩展性


- 场景：
	- 当超类的部分方法不适用于子类
	- 不清晰的继承关系


- 例子：
	``` bad
	class List {}
	class Stack extend List {}
	```
	``` good
	class List {}
	class Stack {
		constructor() {
			this._list = new List();
		}
	}
	```
- 拓展：
	- 超类的所有方法都适用于子类，子类的所有实例都是超类的实例 => 使用继承
	- 尽量使用继承，如果发现继承有问题（扩展困难），再使用以委托取代超类


