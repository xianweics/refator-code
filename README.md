# 重构 - 改善既有代码的设计
### 12.7 移除子类

- 对立：以子类取代类型码


- 目的：
	- 减少系统复杂度


- 场景：
	- 当子类的用处太少时
	- 当子类简单的情况


- 例子：
	```
	class Person {
		get genderCode() {
			return 'X';
		}
	}
	class Male extends Person {
		get genderCode() {
			return 'M';
		}
	}
	class Female extends Person  {
		get genderCode() {
			return 'F';
		}
	}
	```
	```
	class Person {
		constructor(genderCode) {
			this._genderCode = genderCode;
		}
		get genderCode() {
			return this._genderCode;
		}
	}
	```

### 12.8 提取超类

- 目的：
	- 把重复的行为收拢起来


- 场景：
	- 当多个子类的方法基本一致时


- 例子：
	```
	class Department {
		get totalAnnualCost() {}
		get name() {}
	}
	class Employee {
		get annualCost() {}
		get name() {}
		get id() {}
	}
	```
	```
	class Party {
		get totalAnnualCost() {}
		get name() {}
	}
	class Department extends Party {
		get primaryCost() {}
	}
	class Employee extends Party {
		get id() {}
	}
	```


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
	```transfer
	class Employee {}
	```

### 12.10 以委托取代子类

- 目的：
	- 增强类的扩展性


- 场景：
	- 当子类可能存在多种类型上的变化


- 例子
	```
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
	``` transfer
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
	``` 
	class List {}
	class Stack extends List {}
	```
	``` transfer
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


