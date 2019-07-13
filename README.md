# 重构 - 改善既有代码的设计
* [1.语法示例](#1)

## 12 处理继承关系 

通过一系列方式处理函数和字段，来为集成体系添加新类或者删除旧类。
如果本来使用集成的场景变得不再适合，就可以使用<span id="j10-12">托取代子类</span>

### 12.1 函数上移

- 对立：函数下移

- 目的：提高复用性，减少重复

- 场景：当函数被大部分部分子类用到时

- 例子：
	```
	class Employee {}
	class Salesman extends Employee {
		get name() {}
	}
	class Engineer extends Employee {
		get name() {}
	}
	```
	```
	class Employee {
		get name() {}
	}
	class Salesman extends Employee {}
	class Engineer extends Employee {}
	```

### 12.2 字段上移(构造函数本体上移)

## 1

### 12.3 构造函数本体上移 (字段上移)

- 目的：提高复用性

- 场景：当多个子类有公共字段

- 例子：
	```
	class Party {}
	class Employee extends Party {
		constructor(id) {
			this._id = id;
		}
	}
	class Salesman extends Employee {
		constructor(id) {
			this._id = id;
			this._name = name;
		}
	}
	```
	```
	class Party {
		constructor(id) {
			this._id = id;
		}
	}
	class Employee extends Party {
		constructor(id) {
			super(id);
		}
	}
	class Salesman extends Employee {
		constructor(id) {
			super(id);
			this._name = name;
		}
	}
	```

### 12.4 函数下移

- 对立：函数上移

- 目的：内聚子类的方法

- 场景：当函数被小部分子类用到时

- 例子：
	```
	class Employee {
		get quota() {}
	}
	class Salesman extends Employee {}
	class Engineer extends Employee {}
	```
	```
	class Employee {}
	class Salesman extends Employee {
		get quota() {}
	}
	class Engineer extends Employee {}
	```

### 12.5 字段下移

- 对立：字段上移

- 目的：内聚子类的字段

- 场景：当字段被小部分子类用到时

- 例子：
	```
	class Employee {
		constuctor(quote) {
			this._quote = quote;
		}
	}
	class Salesman extends Employee {
		constuctor(quote) {
			super(quote);
		}
	}
	class Engineer extends Employee {}
	```
	```
	class Employee {}
	class Salesman extends Employee {
		constuctor(quote) {
			this._quote = quote;
		}
	}
	class Engineer extends Employee {}
	```

### 12.6 以子类取代类型码

- 对立：移除子类

- 目的：更明确地表达数据与类型之间的关系，增强子类的扩展性

- 场景：当不同的状态码表现不同的行为时

- 例子：
	```
	function createEmployee(name, type) {
		return new Employee(name, type);
	}
	```
	```
	function createEmployee(name, type) {
		const employeeTypes = (name) => {
			return {
				'engineer': new Engineer(name),
				'salesman': new Salesman(name)
			}
		}
		return employeeTypes(name)[type];
	}	
	```

### 12.7 移除子类

- 对立：以子类取代类型码

- 目的：减少系统复杂度

- 场景：当子类的用处太少时，当子类简单

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

- 目的：把重复的行为收拢起来

- 场景：当多个子类的方法基本一致时

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

- 目的：合并子类，减少不必要的子类，降低系统复杂度

- 场景：当超类与子类没有多大差别

- 例子：
	```
	class Employee {}
	class Sales extends Employee {}
	```
	```transfer
	class Employee {}
	```

[12.10] ### 12.10 以委托取代子类

- 目的：增强类的扩展性


- 场景：当子类可能存在多种类型上的变化


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

- 目的：增强类的扩展性

- 场景：当超类的部分方法不适用于子类，不清晰的继承关系

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
