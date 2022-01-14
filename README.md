# 重构 - 改善既有代码的设计

## 1 重构，第一个示例

- 重构前，先检查自己是否有一套可靠的测试集。这些测试必须有自我验证能力。TDD
- 重构技术就是以微小的步伐修改程序。如果犯下错误，很容易便可发现它。
- 傻瓜都能写出计算机可以理解的代码。唯有能写出人类容易理解的代码的，才是优秀的程序员。
- 编程时，需要遵循营地法则：保证你离开时，代码库一定比来的时候更健康。
- 好代码验证的标准是人们是否能轻而易举的修改它。

## 2 重构的原则

### 2.1 何谓重构

- 重构：对软件内部结构的一种调整，目的是在***不改变可观察行为的前提下***，提高其可理解性，降低其修改成本。
- 重构的关键在于运用大量小且保证软件行为的步骤，一步步达到大规模的修改。
- 如果有人说他们的代码在重构过程中有1-2天时间不可用，基本上可以确定，他们在做的事不是重构。

## 2.2 两顶帽子

- 添加新功能，可能需要优化之前的程序结构；当功能开发好，也需要优化下程序的结构。不同的角色切换，是在添加功能过程中必不可少的步骤。

### 2.3 为何重构

- 改进软件的设计
- 使软件更容易理解
- 帮助开发者找到bug
- 提高编程速度

### 2.4 何时重构

- 预备性：让添加新功能更容易
- 帮助理解：使代码更易懂
- 捡垃圾式重构
- 有计划和见机行事的重构：肮脏的代码必须重构，但漂亮的代码也需要很多重构
- 长期重构
- 复审代码时重构
- 何时不应该重构：
	- 只有当需要理解其工作原理时
	- 如果重写比重构容易

### 2.5 重构的挑战

- 缓解新功能开发
	- 重构的唯一目的就是让我们开发更快，用更少的工作量创造更大的价值
	- 重构应该总是由**经济利益**驱动，而不是在于把代码库打磨得闪闪发光
- 分支
	- 持续集成，也叫基于主干开发，避免任何分支彼此差距太大，从而降低合并的难度
- 测试
	- 自测试代码：快速发现错误
- 遗留代码
	- 重构可以很好地帮助我们理解遗留系统，但遗留的系统大多数是没有测试。解决办法是：没测试就加测试。书籍推荐《修改代码的艺术》

### 2.6 重构、架构和YAGNI

- 一旦代码写出来，架构就固定了，只会因为程序员的草率对待而逐渐腐败，重构可以改变这个状态
- 重构可以应对未来的需求变化

### 2.7 重构与软件开发过程

- 极限编程是最早的敏捷软件开发方法之一。要真正以敏捷的方式运作项目，团队成员必须在重构上有能力、有热情，他们采用的开发过程必须与常规的、持续的重构相匹配
- 自测试代码 -> 持续集成 -> 重构

### 2.8 重构与性能

- 除了对性能有严格要求的实时系统，其他情况下，“编写快速软件”的秘诀是：先写出可调优的代码，然后调优它以求获得足够的速度
- 短期看，重构可能会让软件变慢，但它的优化阶段的软件性能调优更容易，最终还是会得到好的效果

### 2.9 重构起源何处

- 优秀的程序员肯定会花一些时间来清理自己的代码，因为他们确定自己几乎无法一开始就写出整洁的代码

### 2.10 自动化重构

- 强大的IDE会让重构变得很轻松

## 3 代码的坏味道

- 神秘命名：如果想不出一个好的名字，说明背后很可能隐藏着更深的设计问题
- 重复代码：优化：对比差异，提取相同。
- 过长函数：优化：条件、循环、公共集中的过程提取处理
- 过长参数列表：优化：使用对象合并参数
- 全局数据：优化：合并数据到方法、类成员中
- 可变数据：优化：函数式编程、数据永不改变
- 发散式变化：做出的某个模块的小修改，必须修改某个类的多个函数。优化：每次只关心一个上下文，将联动的改变提取处理
- 霰弹式修改：每次遇到变化，都必须在很多*不同的类*内做出许多小修改。优化：提取公共方法
- 依恋情结：如果一个函数跟另一个模块中的函数或者数据交流格外频繁，远胜于在自己所处的模块内部交流
	- 模块化：力求将代码分出区域，最大化区域内部的交互、最小化跨区域的交互。所谓高内聚，低耦合
- 数据泥团：两个类中相同的字段、许多函数签名中相同的参数。优化：提取公共字段为一个类、对象
- 基本类型偏执：一些基本类型无法表示一个数据的真实意义，例如电话号码、温度等。优化：使用类、对象字符串类型变量取代基本类型
- 重复的 `switch`：优化：使用策略模式、提取子类
- 循环语句：同*过长函数*
- 冗赘的元素：优化：内联、删除
- 夸夸其谈通用性：优化：内联、删除
- 临时字段：优化：内联、删除
- 过长的消息链：优化：减少委托关系
- 中间人：优化：用继承替代代理委托
- 内幕交易：优化：合并相同的联系，提取不同的成分
- 过大的类：优化：提取类、子类、接口
- 异曲同工的类：优化：提取公共类、使用子类继承
- 纯数据类：它们拥有一些字段，以及访问、读写这些字段的函数。优化：将相关操作封装进去，降低 `public` 成员变量
- 被拒绝的遗赠：如果子类继承超类的数据和方法，但不使用。优化：用内联数据和方法、代理委托替代继承关系
- 注释：当你感觉需要编写注释时，请先尝试重构，试着让所有注释变得多余

## 4 构筑测试体系

### 4.1 自测试代码的价值

- 程序员编写代码的时间仅占所有时间中很少的一部分，但是花费在调试上的时间是最多的。修复bug通常是比较快的，但找出bug所在却是一场噩梦
- 确保所有测试都是完全自动化，让他们检查自己的测试结果
- 一套测试就是一个强大的bug侦探器，能够大大缩减查找bug所需的时间

### 4.2 测试代码示例

### 4.3 第一个测试

- 总是确保测试不该通过时，会产生失败
- 频繁地运行测试，对于你正在处理的代码与其对应的测试至少每隔几分钟就要运行一次，每天至少运行一次所有的测试

### 4.4 再添加一个测试

- 编写为臻完善的测试并经常运行，好过对完美测试的无尽等待
- 保持每个测试用例独立性，避免产生共享对象。因为测试之间会通过共享产生交互，而测试的结果就会受测试运行次序的影响，导致测试结果的不确定性
- 例子
  ```javascript
  describe('province', () => {
    const shanghai = new Province('shanghai');
    it('shortfall', () => {
	  expect(shanghai.shortfall).equal(5)
    })
  })
  ```
  change to
  ```javascript
  describe('province', () => {
    let shanghai = null;
    beforeEach(() => {
      shanghai = new Province('shanghai');
    })
    it('shortfall', () => {
	  expect(shanghai.shortfall).equal(5)
    })
  })
  ```

### 4.5 修改测试夹具

- 配置 - 检查 - 验证
- 准备 - 行为 - 断言

### 4.6 探测边界条件

- 考虑可能出错的边界条件，把测试火力集中在那儿
- 不要因为测试无法捕捉所有的bug就不写测试，因为测试的确可以捕捉到大多数bug
- 任何测试都不能证明一个程序没有bug
- 当测试数量达到一定程度后，继续增加测试代理的边际效用会递减
- 应该把测试集中在可能出错的地方，观察代码，看哪儿变得复杂、哪些地方可能出错

### 4.7 测试远不止如此

- 一个架构的好坏，很大程度上要取决于它的可测试性，这是一个好的行业趋势
- 每当收到bug报告，请先写一个单元测试来暴露这个bug
- 一个测试集是否够好，最好的衡量标准其实是主观的，试问自己：如果有人在代码里引入了一个缺陷，自己有多大的自信它能被测试集发现

## 5 介绍重构名录

## 6 第一组重构

### 6.1 提炼函数

- 对立：[内联函数](#62内联函数)

- 目的：将意图与实现分开。意图 == 主干；实现 == 分支的实现

- 场景：如果需要花时间浏览一段代码才能弄清它到底干什么，那么就应该将其提炼到一个函数中，并根据它所做的事为其命名。以后再读到这段代码时，可以一眼就能知道函数的用途，大多数根本不需要关心函数如何实现。

- 例子： 
  ```javascript
  function printOwing(invoice){
    printBanner();
    const outstanding = calculateOutstanding();      
    //print details
    console.info('name:', invoice.name);
    console.info('amount:', outstanding);
  }
  ```
  ```javascript
  function printOwing(invoice){
    printBanner();
	const outstanding = calculateOutstanding();
	printDetails(outstanding, invoice);

	function printDetails(){
	  console.info('name:', invoice.name);
	  console.info('amount:', outstanding);
	}
  }
  ```

### 6.2 内联函数

- 对立：[提炼函数](#61-提炼函数)

- 目的：去除不必要间接层/委托层，降低系统复杂度

- 场景：
    - 一堆不合理的函数，可以将其内联到一个大型函数中，再重新提炼到小函数中
    - 代码太多间接层，使系统中的所有函数都似乎只是对另一个函数简单的委托

- 例子： 
    ```javascript
    function getRating(driver){
      return moreThanFiveDeliveries(driver) ? 2 : 1;
    }
    
    function moreThanFiveDeliveries(driver){
      return driver.deliveries > 5;
    }
    ```
    ```javascript
    function getRating(driver){
      return driver.deliveries > 5 ? 2 : 1;
    }
    ```

### 6.3 提炼变量

- 对立：[内联变量](#64-内联变量)

- 目的：将复杂的表达式使用变量说明

- 例子： 
  ```javascript
	return order.quantity * order.itemPrice - Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 + Math.min(order.quantity * order.itemPrice * 0.1, 100);
	```
	```javascript
	const basePrice = order.quantity * order.itemPrice;
	const quantityDiscount =  Math.max(0, order.quantity - 500) * order.itemPrice * 0.05;
	const shipping = Math.min(basePrice * 0.1, 100);
	return basePrice - quantityDiscount + shipping;
	```
    
### 6.4 内联变量

- 对立：[提炼变量](#63-提炼变量)

- 目的：去除不必要变量

- 场景：
  
  - 表达式比变量更有表现力
  
- 例子： 
	```javascript
	const basePrice = order.basePrice;
	return basePrice > 25;
	```
	```javascript
	return order.basePrice > 25;
	```
    
### 6.5 改变函数声明

- 目的：好名字能让人一眼看出函数的用途，而不必看代码实现

- 使用：
  
  - 先写一句注释描述这个函数的用途，再把这句注释变成函数的名字
  
- 例子： 
	```javascript
	function calc(){}
	```
	```javascript
	function calcOrder(){}
	```
    
### 6.6 封装变量

- 目的：
    - 重构数据转移为重构函数，更易于处理
    - 监控数据的变化

- 场景：如果数据的可访问范围大

- 例子：
	```javascript
	let defaultOwner = {};
	```
	```javascript
	let defaultOwner = {};
	export function defaultOwner(){
		return defaultOwner;
	}
	export function getDefaultOwner(arg){
		defaultOwner = arg;
	}
	```

### 6.7 变量改名

- 目的：好名字可让上下文更清晰

- 例子： 
	```javascript
	const a = height * width;
	```
	```javascript
	const area = height * width;
	```
    
### 6.8 引入参数对象

- 目的：组织数据结构，让数据项之间的关系更清晰，参数列表也能缩短

- 场景：一个函数接受多个参数

- 例子： 
	```javascript
	function invoice(startDate, endDate){}
	function received(starDate, endDate){}
	```
	```javascript
	function invoice(dateRange){}
	function received(dateRanges){}
	```
    
### 6.9 函数组合成类

- 目的：
    - 对象内部调用这些函数可以少传参数，从而简化函数调用，而且一个对象可更方便传递给系统的其他部分
    - 客户端修改对象的核心数据，通过计算得出的派生数据会自动与核心数据保存一致
    
- 场景：如果一组函数形影不离地操作同一块数据（通常是将这块数据作为参数传递给函数）

- 例子： 
	```javascript
	function base(reading){}
	function taxableCharge(reading){}
	function calcBaseCharge(reading){}
	```
	```javascript
	class Reading{
		base(){}
		taxableCharge(){}
		calcBaseCharge(){}
	}
	```
    
### 6.10 函数组合成变换

- 目的：
    - 增强数据，将数据逻辑统一在一个地方处理
    - 高内聚
    
- 场景：需要把数据放到另一个程序中运行，计算出各种派生信息

- 例子： 
	```javascript
	function base(reading){}
	function taxableCharge(reading){}
	```
	```javascript
	function enrichReading(arg){
		const reading = _.cloneDeep(arg);
		reading.baseCharge = base(reading);
		reading.taxableCharge = taxableCharge(reading);
		return reading;
	}
	```
    
### 6.11 拆分阶段

- 目的：保证单一原则，一段代码只做一件事

- 场景：如果一段代码同时处理两件或者更多不同的事情

- 例子：
	```javascript
	const orderArr = orderStr.split(/\s+/);
	const productPrice = priceList(order[0].split('-')[1]);
	const orderPrice = parseInt(orderArr[1]) * productPrice;
	```
	```javascript
	const orderRecord = parseOrder(order);
	const orderPrice = price(orderRecord, priceList);

	function parseOrder(str){
		const values = str.split(/\s+/);
		return {
			priceId: values[0].split('-')[1],
			quantity: parseInt(values[1]) 
		};
	}

	function price(order, priceList){
		return order.quantity * priceList[order.productId];
	}
	```

## 7 封装

### 7.1 封装记录

- 目的：
	- 对象可以隐藏结构的细节
	- 有助于字段改名

- 场景：对于可变数据

- 例子：
  ```javascript
  const organization = {name: 'John', country: 'GB'};
  ```

  ```javascript
  class Organization{
    constructor(data){
      this._name = data.name;
      this._country = data.country;
    }
    get name(){
      return this._name;
    }
    set name(arg){
      this._name = arg;
    }
    get country(){
      return this._country;
    }
    set country(arg){
      this._country = arg;
    }
  }
  ```	

### 7.2 封装集合

- 目的：控制外界对类中集合的访问权，避免集合被直接修改

- 场景：类中集合为可变数据

- 例子：
	```javascript
	class Person{
	  get courses(){
	    return this._courses;
	  }
	  set courses(list){
	    this._course = list;
	  }
	}
	```
	```javascript
	class Person{
	  get course(){
	    return this._courses.slice();
	  }
	  addCourse(course){}
	  removeCourse(course){}
	}
	```

### 7.3 以对象取代基本类型

- 目的：扩展或者增强数据的行为

- 场景：如果数据项需要更多的含义或者行为时

- 例子：
	```javascript
	orders.filter(o => 'high' === o.priority || 'rush' === o.priority);
	```
	```javascript
	orders.filter(o => o.priority.higherThan(new Priority('normal')));
	```
	
### 7.4 以查询取代临时变量

- 目的：
  - 新函数与原函数之间的边界更清晰
  - 便于代码抽离
  - 利于函数复用

- 场景：那些值被计算一次且之后不再被修改的变量

- 例子：
  ```javascript
  const basePrice = this._quantity * this._itemPrice;
  if(basePrice > 1000){
    return basePrice * 0.95;
  }else{
    return basePrice * 0.98;
  }
  ```
  ```javascript
  get basePrice(){ 
    this._quantity * this._itemPrice;
  }
  if(this.basePrice > 1000){
    return this.basePrice * 0.95;
  }else{
    return this.basePrice * 0.98;
  }
  ```


### 7.5 提取类

- 对立：[内联类](#76-内联类)

- 目的：将大类分成小类

- 场景：如果维护一个大量函数和数据的类

- 例子：
	```javascript
	class Person{
		get officeAreaCode(){
			return this._officeAreaCode;
		}
		get officeNumber(){
			return this._officeNumber;
		}
	}
	```
	```javascript
	class Person{
		get officeAreaCode(){
			return this._telephoneNumber.areaCode;
		}
		get officeNumber(){
			return this._telephoneNumber.number;
		}
	}
	class TelephoneNumber{
	  get areaCode(){
	    return this._areaCode;
	  }
	  get number(){
	    return this._number;
	  }
	}
	```
	
### 7.6 内联类

- 对立：[提炼类](#75-提炼类)

- 目的：减少不必要的类

- 场景：
	- 如果一个类不再承担足够的责任
	- 重新分类两个类的不同职责

- 例子：
	```javascript
	class Person{
		get officeAreaCode(){
			return this._telephoneNumber.areaCode;
		}
		get officeNumber(){
			return this._telephoneNumber.number;
		}
	}
	class TelephoneNumber{
	  get areaCode(){
	    return this._areaCode;
	  }
	  get number(){
	    return this._number;
	  }
	}
	```
	```javascript
	class Person{
		get officeAreaCode(){
			return this._officeAreaCode;
		}
		get officeNumber(){
			return this._officeNumber;
		}
	}
	```
	
### 7.7 隐藏委托关系

- 对立：[移除中间人](#78-移除中间人)

- 目的：每个模块尽可能减少了解系统的其他部分，把部分依赖关系隐藏起来，减少调用者双方了解更多细节

- 场景：如果被调方接口频繁修改时

- 例子：
	```javascript
	const manager = person.department.manager;
	```
	```javascript
	const manager = person.manager;
	class Person{
	  get manager(){
	    return this.department.manager;
	  }
	}
	```
	
### 7.8 移除中间人

- 对立：[隐藏委托关系](#77-隐藏委托关系)

- 目的：减少不不必要的委托

- 场景：如果过多的转发函数没有让程序本身提升的扩展性，就应删除部分委托

- 例子：
	```javascript
	const manager = person.manager;
	class Person{
	  get manager(){
	    return this.department.manager;
	  }
	}
	```
	```javascript
	const manager = person.department.manager;
	```
	
### 7.9 替换算法

- 目的：用简单算法处理

- 场景：随着对业务不断深入，发觉有更简单的算法实现

- 例子：
	```javascript
	function foundPerson(people){
	  for(let i = 0; i < people.length; i++){
	    if(people[i] === 'John'){
	      return 'John';
	    }
	    if(people[i] === 'Maria'){
	      return 'Maria';
	    }
	     if(people[i] === 'Mike'){
	      return 'Mike';
	    }
	  }
	  return '';
	}
	```
	```javascript
	function foundPerson(people){
		const candidate = ['John', 'Maria', 'Mike'];
	  return people.find(p => candidate.includes(p)) || '';
	}
	```

## 8 搬移特性

### 8.1 搬移函数

- 目的：减少对不常用函数的外部依赖，增加常用函数的内部依赖，高内聚

- 场景：如果一个方法频繁调用别处的一个函数，并且被频繁调用的函数在该上下文关系不大时

- 例子：
    ```javascript
    class Account{
        get overdraftCharge(){}
    }
    ```
    ```javascript
    class AccountType{
        get overdraftCharge(){}
    }
    ```
    
    > 将内聚放在一个类中，改名。
    
    > 变量为名词
    
    > 方法为动词

### 8.2 搬移字段

- 目的：高内聚，将不属于当前类的属性搬到另一个类中

- 场景：如果修改一条记录时，总是需要同时改动另一个记录

- 例子：
  ```javascript
  class Customer{
  	get plan(){
  		return this._plan;
  	}
  	get disountRate(){
  		return this._discountRate;
  	}
  }
  ```
  ```javascript
  class Customer{
  	get plan(){
  		return this._plan;
  	}
  	get disountRate(){
  		return this.plan.discountRate;
  	}
  }
  ```
  
### 8.3 搬移语句到函数

- 对立：[搬移语句到调用者](#83-搬移语句到调用者)

- 目的：消除重复，提取公共内容，即表现一致的行为

- 场景：发现调用某个函数时，总有一些相同的代码也需要每次执行

- 例子：
	```javascript
	result.push(`<p>title: ${person.photo.title}</p>`);
	result.concat(photoData(person.photo));
	function photoData(photo){
	  return {
	    `<p>location: ${photo.location}</p>`;
	  	`<p>date: ${photo.date}</p>`;
	  }
	}
	```
	```javascript
	result.concat(photoData(person.photo));
	function photoData(photo){
	  return {
	    `<p>title: ${person.photo.title}</p>`;
	    `<p>location: ${photo.location}</p>`;
	  	`<p>date: ${photo.date}</p>`;
	  }
	}
	```
	
### 8.4 搬移语句到调用者

- 对立：[搬移语句到函数](#83-搬移语句到函数)

- 目的：将可变的行为搬移到调用者内部

- 场景：以往多个地方公共的行为，如今需要在某些调用点表现不同的行为，并且调用点与调用者之间的边界差别不大。如果差别较大的，只能重新设计

- 例子：
	```javascript
	emitPhotoData(outStream, person.photo);
	function emitPhotoData(outStream, photo){
	  outStream.write(`<p>title: ${photo.title}</p>`);
	  outStream.write(`<p>title: ${photo.location}</p>`);
	}
	```
	
	```javascript
	emitPhotoData(outStream, person.photo);
	outStream.write(`<p>title: ${photo.title}</p>`);
	function emitPhotoData(outStream, photo){
	  outStream.write(`<p>title: ${photo.location}</p>`);
	}
	```
	
### 8.5 以函数调用取代内联代码

- 目的：消除重复

- 场景：如果一些内联代码做的事情是已有函数可以做到的

- 例子：
	```javascript
	let hasMa = false;
	for(const i of states){
	  if(i === 'MA'){
	    hasMa = true;
	  }
	}
	```
	
	```javascript
	const hasMa = states.includes('MA');
	```
	
### 8.6 移动语句

- 目的：高内聚内部代码

- 场景：如果有几行代码取用了同一个数据结构，那么最好让它们在一起

- 例子：
  ```javascript
  const pricePlan = retrievePricingPlan();
  const order = retreiveOrder();
  let charge;
  const chargePerUnit = pricingPlan.unit();
  ```
  ```javascript
  const pricePlan = retrievePricingPlan();
  const chargePerUnit = pricingPlan.unit();
  const order = retreiveOrder();
  let charge;
  ```

### 8.7 拆分循环

- 目的：保持循环内部只做一件事

- 场景：如果循环内部身兼多职

- 例子：
	```javascript
	let averageAge = 0;
	let totalSalary = 0;
	for(const p of people){
	  averageAge += p.age;
	  totalSalary += p.salary;
	}
	averageAge = averageAge / people.length;
	```
	```javascript
	let averageAge = 0;
	for(const p of people){
	  averageAge += p.age;
	}
	let totalSalary = 0;
	for(const p of people){
	  averageAge += p.age;
	}
	averageAge = averageAge / people.length;
	```
	
	> 先进行重构，在进行性能优化。将代码变得清晰，对后期的扩展、优化，都极其方便 	

### 8.8 以管道取代循环

- 目的：提高代码可读性

- 场景：如果代码的逻辑可以通过内置方法处理

- 例子：
	```javascript
	const names = [];
	for(const i of input){
	  if(i.job === 'programmer'){
	    names.push(i.name);
	  }
	}
	```
	```javascript
	const names = input
	.filter(i => i.job === 'programmer')
	.map(i => i.name);
	```

### 8.9 移除死代码

- 目的：移除无用代码

## 9 重新组织数据

### 9.1 拆分代码

- 目的：每个变量只承担一个责任，同一个变量承担两件不同的事情，会令代码阅读者糊涂

- 场景：大多数情况下变量只赋值一次，除了：循环变量（例如：`for(let i =0; i < 5; i++)` 中的`i`），收集结果变量

- 例子：
    ```javascript
    let temp = 2 * (height + width);
    temp = height * width;
    ```
    ```javascript
    const perimeter = 2 * (height * width);
    const area = height * width;
    ```

    > 变量声明可以刚开始声明为 `const`，如果发觉需要重复赋值，再改为 `let`

### 9.2 字段改名

- 目的：好的名字可以帮助阅读者更易理解

- 例子：
    ```javascript
    class Organization{
      get name(){}
    }
    ```
    ```javascript
    class Organization{
      get title(){}
    }
    ```
    
### 9.3 以查询取代派生变量

- 目的：减少方法中的*副作用*，单一原则

- 场景：对数据的修改常常导致代码的各个部分以丑陋的形式互相耦合：在一处修改数据，却在另一处造成难以发现的破坏

- 例子：
    ```javascript
    get discountTotal(){
      return this._discountTotal;
    }
    set discount(number){
      const old = this._discount;
      this._discount = number;
      this._discountTotal += old - number;
    }
    ```
    ```javascript
    get discountTotal(){
      return this._baseTotal - this._discount;
    }
		get discount(){
		  return this._discount;
		}
    set discount(number){
      this._discount = number;
    }
    ```
    
### 9.4 将引用对象改为值对象

- 对立：[将值对象改为引用对象](#94-将值对象改为引用对象)

- 目的：值对象的不可变性处理起来更容易，可以任意的传递，防止被外部修改

- 场景：如果不需要改变值的引用关系，每个值是不可变的

- 例子：
    ```javascript
    class Product{
      applyDiscount(arg){
        this._price -= arg;
      }
    }
    ```
    ```javascript
    class Product{
      applyDiscount(arg){
        this._price = new Money(this._price.amount - arg, this._price.currency);
      }
    }
    ```
    
### 9.5 将值对象改为引用对象

- 目的：保持数据共享

- 场景：如果数据结构中包含多个记录，而这些记录都有关联到同一个逻辑的数据结构，例如一个数据的改动，需要共享到整个数据集

- 例子：
    ```javascript
    let customer = new Customer(customerData);
    ```
    ```javascript
    let customer = customerRepository.get(customerData, id);
    ```
    
## 10 简化条件逻辑

### 10.1 分解条件表达式

- 目的：简化条件，易于理解代码逻辑

- 场景：当检查处理逻辑复杂时

- 例子：
	```javascript
	if(!date.isBefore(plan.summerStart) && !date.isAfter(plan.summerEnd)){
		charge = quantity * plan.summerRate;
	}else{
		charge = quantity * plan.regularRate + plan.regularServiceCharge;
	}
	```
	```javascript
	const isSummer = !date.isBefore(plan.summerStart) && !date.isAfter(plan.summerEnd);
	function summerCharge(){
		return quantity * plan.summerRate;
	}
	function regularCharge(){
		return quantity * plan.regularRate + plan.regularServiceCharge;
	}
	
	charge = isSummer() ? summerCharge() : regularCharge();
	```
	
### 10.2 合并条件表达式

- 目的：统一处理条件语句

- 场景：当检查条件各不相同，最终行为一致时

- 例子：
	```javascript
	if(age < 18) return 'younger';
	if(experience < 5) return 'younger';
	if(!isPassTest) return 'younger';
	```
	```javascript
	if(isYounger()) return 'younger';
	function isYounger(){
		return age<18 || experience < 5 || !isPassTest;
	}
	```

### 10.3 卫语句取代嵌套条件表达式

- 目的：减少逻辑的复杂度

- 场景：当出现需要单独检查某个特定条件时

- 例子：
	```javascript
	function getPayment(){
		let result;
		if(isRead){
			result = deadAmount();
		}else{
			if(isSeparated){
				result = separatedAmount();
			}else{
				if(isRetired){
					result = retiredAmount();
				}else{
					result = normalAmount();
				}
			}
		}
	}
	```
	```javascript
	function getPayment(){
		if(isRead) return deadAmount();
		if(isSeparated) return separatedAmount();
		if(isRetired) return retiredAmount();
		
		return normalAmount();
	}
	```

### 10.4 以多态取代条件表达式

- 目的：增强扩展性，减少逻辑的复杂度

- 场景：当多个逻辑处理情况

- 例子：
	```javascript
	switch(bird.type){
		case 'EuropeanSwallow':
			return 'EuropeanSwallow';
		case 'AfricanSwallow':
			return 'AfricanSwallow';
		default:
			return 'unknown';
	}
	```
	```javascript
	class EuropeanSwallow{
		get name(){
			return 'EuropeanSwallow';
		}
	}
	class AfricanSwallow{
		get name(){
			return 'AfricanSwallow';
		}
	}
	```

### 10.5 引入特例

- 目的：提供复用性以及统一性

- 场景：如果某部分逻辑都在检查某个特殊值，并且处理的逻辑也都相同

- 例子：
	```javascript
	if(customer === 'unknown'){
		customerName = 'occupant';
	}
	```
	```javascript
	class UnknownCustomer{
		get name(){
			return 'occupant';
		}
	}
	```
	
### 10.6 引入断言

- 目的：保障传入值是可预测的，预习发现测试的BUG

- 例子：
	```javascript
	if(this.discountRate){
		base = base - this.discountRate * base;
	}
	```
	```javascript
	asset(this.discountRate>0);
	if(this.discountRate){
		base = base - this.discountRate * base;
	}
	```

## 11 重构API

### 11.1 查询函数和修改函数分离

- 目的：减少函数副作用 == 任何有返回值的函数，都要减少它的副作用

- 场景：当函数中又有查询，又有命令

- 例子：
	```javascript
	function getTotalOutStandingAndSendBill(person){
		const result = customer.invoice.reduce((total, each)=>each.amount + total, 0);
		sendBill();
		return result;
	}
	```
	```javascript
	function getTotalStanding(person){
		return customer.invoice.reduce((total, each)=>each.amount + total, 0);
	}
	function sendBill(){}
	function handler(){
		const totalOutstanding = getTotalStanding();
		sendBill();
	}
	```

### 11.2 函数参数化

- 目的：增强函数的功能

- 场景：如果发现多个函数逻辑相似，只有某1-2个字面量不同

- 例子：
	```javascript
	function tenPercentRaise(person){
		person.salary = person.salary.multiply(1.1);
	}
	function fivePercentRaise(person){
		person.salary = person.salary.multiply(0.05);
	}
	```
	```javascript
	function raise(person, factor){
		person.salary = person.salary.multiply(factor);
	}
	```

### 11.3 移除标记参数

- 目的：代码更清晰，减少函数的复杂度

- 场景：如果参数值影响函数内部的控制流

- 例子：
	```javascript
	function setDimension(name, value){
		if(name === 'height'){}
		if(name === 'width'){}
	}
	```
	```javascript
	function setHeight(value){
		this._height = value;
	}
	function setWidth(value){
		this._width = value;
	}
	```

### 11.4 保持对象完整性 

- 目的：缩短参数列表，参数配置灵活

- 场景：如果传入多个参数传值

- 例子：
	```javascript
	const{low, high} = temperature;
	if(isValidTemperature(low, high)){};
	```
	```javascript
	if(isValidTemperature(temperature)){};
	```

### 11.5 以查询取代参数 

- 对立：[以参数取代查询](#116-以参数取代查询)

- 目的：减少传参，从而减少调用者的成本

- 场景：如果传入多个参数，并且从一个参数推导出另一个参数

- 例子：
	```javascript
	availableVacation(employee, employee.grade);
	function availableVacation(employee, grade){}
	```
	```javascript
	availableVacation(employee);
	function availableVacation(employee){
		const {grade} = employee;
	}
	```

### 11.6 以参数取代查询

- 对立：[以查询取代参数](#115-以查询取代参数)

- 目的：减少函数的副作用，以及引用关系，保持函数的纯净度

- 场景：如果函数引用了一个全局变量，或者引用想移除的元素

- 例子：
	```javascript
	const weather ={};
	targetTemperature(plan);
	
	function targetTemperature(plan){
		const{curTemperature} = weather;
	}
	```
	```javascript
	const weather ={};
	targetTemperature(plan, weather);
	
	function targetTemperature(plan, weather){
		const{curTemperature} = weather;
	}
	```

### 11.7 移除设置函数

- 目的：防止某字段被修改

- 场景：当不希望某个字段被修改时

- 例子：
	```javascript
	class Person{
		get id(){}
		set id(name){}
	}
	```
	```javascript
	class Person{
		get id(){}
	}
	```
	
### 11.8 以工厂函数取代构造函数

- 目的：增加灵活性

- 场景：不存在继承关系时

- 例子：
	```javascript
	const leadEngineer = new Employee('name','E');
	```
	```javascript
	const leadEngineer = createEngineer('name');
	```

### 11.9 以命令取代函数

- 对立：[以函数取代命令](#1110-以函数取代命令)

- 目的：命令对象提供更大的灵活性，并且还可以支持撤销、生命周期的管理等附加操作

- 场景：当普通函数无法提供强有力灵活性

- 例子：
	```javascript
	function score(candidate, media){
		let result = 0;
		let healthLevel = 0;
	}
	function hasPassMedicalExam(){}
	```
	```javascript
	class Scorer{
		constructor(candidate, medicalExam){
			this._candidate = candidate;
			this._medicalExam = medicalExam;
		}
		execute(){
			let result = 0;
			let healthLevel = 0;
		}
		hasPassMedicalExam(){}
	}
	```

### 11.10 以函数取代命令

- 对立：[以命令取代函数](#119-以命令取代函数)

- 目的：函数简单化

- 场景：大多数情况下，只想调用一个函数，完成自己的工作，不需要函数那么复杂

- 例子：
	```javascript
	class ChargeCalculator{
		constructor(customer, usage){
			this._customer = customer;
			this._usage = usage;
		}
		execute(){ 
			return this._customer.rate * this._usage;
		}
	}
	```
	```javascript
	function charge(customer, usage){
		return customer.rate * usage;
	}
	```

## 12 处理继承关系 

继承体系里上下调整：[函数上移](#121-函数上移)、[字段上移](#122-字段上移)、[构造函数本体上移](#123-构造函数本体上移)、[函数下移](#124-函数下移)、[字段下移](#125-字段下移)

继承体系添加新类或者删除旧类：[移除子类](#127-移除子类)、[提取超类](#128-提取超类)、[折叠继承体系](#129-折叠继承体系)

一个字段仅用于类型码使用：[以子类取代类型码](#126-以子类取代类型码)

如果本来使用集成的场景变得不再适合：[以委托取代子类](#1210-以委托取代子类)、[以委托取代超类](#1211-以委托取代超类)

### 12.1 函数上移

- 对立：[函数下移](#124-函数下移)

- 目的：提高复用性，减少重复

- 场景：当函数被大部分部分子类用到时

- 例子：
	```javascript
	class Employee{}
	class Salesman extends Employee{
		get name(){}
	}
	class Engineer extends Employee{
		get name(){}
	}
	```
	```javascript
	class Employee{
		get name(){}
	}
	class Salesman extends Employee{}
	class Engineer extends Employee{}
	```

### 12.2 字段上移
[移构造函数本体上移](#123-构造函数本体上移)

### 12.3 构造函数本体上移

- 目的：提高复用性

- 场景：当多个子类有公共字段

- 例子：
	```javascript
	class Party{}
	class Employee extends Party{
		constructor(id){
			this._id = id;
		}
	}
	class Salesman extends Employee{
		constructor(id){
			this._id = id;
			this._name = name;
		}
	}
	```
	```javascript
	class Party{
		constructor(id){
			this._id = id;
		}
	}
	class Employee extends Party{
		constructor(id){
			super(id);
		}
	}
	class Salesman extends Employee{
		constructor(id){
			super(id);
			this._name = name;
		}
	}
	```

### 12.4 函数下移

- 对立：[函数上移](#122-字段上移)

- 目的：内聚子类的方法

- 场景：当函数被小部分子类用到时

- 例子：
	```javascript
	class Employee{
		get quota(){}
	}
	class Salesman extends Employee{}
	class Engineer extends Employee{}
	```
	```javascript
	class Employee{}
	class Salesman extends Employee{
		get quota(){}
	}
	class Engineer extends Employee{}
	```

### 12.5 字段下移

- 对立：[字段上移](#122-字段上移)

- 目的：内聚子类的字段

- 场景：当字段被小部分子类用到时

- 例子：
	```javascript
	class Employee{
		constructor(quote){
			this._quote = quote;
		}
	}
	class Salesman extends Employee{
		constructor(quote){
			super(quote);
		}
	}
	class Engineer extends Employee{}
	```
	```javascript
	class Employee{}
	class Salesman extends Employee{
		constructor(quote){
			this._quote = quote;
		}
	}
	class Engineer extends Employee{}
	```

### 12.6 以子类取代类型码

- 对立：[移除子类](#127-移除子类)

- 目的：更明确地表达数据与类型之间的关系，增强子类的扩展性

- 场景：当不同的状态码表现不同的行为时

- 例子：
	```javascript
	function createEmployee(name, type){
		return new Employee(name, type);
	}
	```
	```javascript
	function createEmployee(name, type){
		const employeeTypes = (name) =>{
			return{
				'engineer': new Engineer(name),
				'salesman': new Salesman(name)
			}
		}
		return employeeTypes(name)[type];
	}	
	```

### 12.7 移除子类

- 对立：[以子类取代类型码](#126-以子类取代类型码)

- 目的：减少系统复杂度

- 场景：当子类的用处太少时，当子类简单

- 例子：
	```javascript
	class Person{
		get genderCode(){
			return 'X';
		}
	}
	class Male extends Person{
		get genderCode(){
			return 'M';
		}
	}
	class Female extends Person {
		get genderCode(){
			return 'F';
		}
	}
	```
	```javascript
	class Person{
		constructor(genderCode){
			this._genderCode = genderCode;
		}
		get genderCode(){
			return this._genderCode;
		}
	}
	```

### 12.8 提取超类

- 目的：把重复的行为收拢起来

- 场景：当多个子类的方法基本一致时

- 例子：
	```javascript
	class Department{
		get totalAnnualCost(){}
		get name(){}
	}
	class Employee{
		get annualCost(){}
		get name(){}
		get id(){}
	}
	```
	```javascript
	class Party{
		get totalAnnualCost(){}
		get name(){}
	}
	class Department extends Party{
		get primaryCost(){}
	}
	class Employee extends Party{
		get id(){}
	}
	```

### 12.9 折叠继承体系

- 目的：合并子类，减少不必要的子类，降低系统复杂度

- 场景：当超类与子类没有多大差别

- 例子：
	```javascript
	class Employee{}
	class Sales extends Employee{}
	```
	```javascript
	class Employee{}
	```

###  12.10 以委托取代子类

- 目的：增强类的扩展性

- 场景：当子类可能存在多种类型上的变化

- 例子
	```javascript
	class Booking{
		constructor(show, date){
			this._show = show;
			this._date = date;
		}
	}
	
	class PremiumBooking extends Booking{
		constructor(show, date, extras){
			super(show, date);
			this._extras = extras;
		}
	}
	```
	```javascript
	class Booking{
		constructor(show, date){
			this._show = show;
			this._date = date;
			this._premium = null;
		}
		bePremium(extras){
			this._premium = new PremiumBookingDelegate(this, extras);
		}
	}
	
	class PremiumBookingDelegate{
		constructor(root, extras){
			this._root = root;
			this._extras = extras;
		}
	}
	```

### 12.11 以委托取代超类

- 目的：增强类的扩展性

- 场景：当超类的部分方法不适用于子类，不清晰的继承关系

- 例子：
	``` javascript
	class List{}
	class Stack extends List{}
	```
	```javascript
	class List{}
	class Stack{
		constructor(){
			this._list = new List();
		}
	}
	```
    > 超类的所有方法都适用于子类，子类的所有实例都是超类的实例 => 使用继承
  
    > 如果发现继承有问题（扩展困难），再使用[以委托取代超类](#1211-以委托取代超类)
