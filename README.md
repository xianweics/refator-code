# 重构 - 改善既有代码的设计

## 1 重构，第一个示例

- 重构前，先检查自己是否有一套可靠的测试集。这些测试必须有自我验证能力。TDD
- 重构技术就是以微小的步伐修改程序。如果犯下错误，很容易便可发现它。
- 傻瓜都能写出计算机可以理解的代码。唯有能写出人类容易理解的代码的，才是优秀的程序员。
- 编程时，需要遵循营地法则：保证你离开时，代码库一定比来的时候更健康。
- 好代码验证的标准是人们是否能轻而易举的修改它。

## 2 重构的原则

### 2.1 何谓重构

- 重构：对软件内部结构的一种调整，目的是在*不改变可观察行为的前提下*，提高其可理解性，降低其修改成本。
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
	- 如果重写比重构容易。

### 2.5 重构的挑战

- 缓解新功能开发
	- 重构的唯一目的就是让我们开发更快，用更少的工作量创造更大的价值
	- 重构应该总是由经济利益驱动，而不是在于把代码库打磨得闪闪发光
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
- 自测试代码---持续集成---重构

### 2.8 重构与性能
- 除了对性能有严格要求的实时系统，其他情况下，“编写快速软件”的秘诀是：先写出可调优的代码，然后调优它以求获得足够的速度
- 短期看，重构可能会让软件变慢，但它的优化阶段的软件性能调优更容易，最终还是会得到好的效果

### 2.9 重构起源何处
- 优秀的程序员肯定会花一些时间来清理自己的代码，因为他们指定自己几乎无法一开始就写出整洁的代码

### 2.10 自动化重构
- 强大的IDE会让重构变得很轻松

## 3 代码的坏味道
- 神秘命名：如果想不出一个好的名字，说明背后很可能隐藏着更深的设计问题
- 重复代码
- 过长函数
- 过长参数列表
- 全局数据
- 可变数据：优化=》函数式编程=》数据永不改变
- 发散式变化：做出的某个模块的小修改，必须修改多个函数。优化=》每次只关心一个上下文
- 霰弹式修改：每次遇到变化，都必须在很多不同的类内做出许多小修改。
- 依恋情结：如果一个函数跟另一个模块中的函数或者数据交流格外频繁，远胜于在自己所处的模块内部交流
	- 模块化：力求将代码分出区域，最大化区域内部的交互、最小化跨区域的交互。所谓高内聚，低耦合
- 数据泥团
- 基本类型偏执：一些基本类型无法表示一个数据的真实意义，例如电话号码、温度等。优化=》使用类字符串类型变量取代基本类型
- 重复的switch
- 循环语句
- 冗赘的元素
- 夸夸其谈通用性
- 临时字段
- 过长的消息链
- 中间人
- 内幕交易
- 过大的类
- 异曲同工的类
- 纯数据类：它们拥有一些字段，以及访问、读写这些字段的函数
- 被拒绝的遗赠：如果子类继承超类的数据和方法，但不使用
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

    ```
    describe('province', ()=>{
        const shanghai = new Province('shanghai');
        it('shortfall', ()=>{
            expect(shanghai.shortfall).equal(5)
        })
    })
    ```
    change to
    ```
    describe('province', ()=>{
        let shanghai = null;
        beforeEach(()=>{
            shanghai = new Province('shanghai');
        })
        it('shortfall', ()=>{
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

- 对立：内联函数

- 目的：将意图与实现分开。意图=》主干；实现=〉分支的实现

- 场景：如果需要花时间浏览一段代码才能弄清它到底干什么，那么就应该将其提炼到一个函数中，并根据它所做的事为其命名。以后再读到这段代码时，可以一眼就能知道函数的用途，大多数根本不需要关心函数如何实现。

- 例子： 
    ```
    function printOwing(invoice){
        printBanner();
        const outstanding = calculateOutstanding();
        
        //print details
        console.info('name:', invoice.name);
        console.info('amount:', outstanding);
    }
    ```
    ```
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

- 对立：提炼函数

- 目的：去除不必要间接层/委托层，降低系统复杂度

- 场景：
    - 一堆不合理的函数，可以将其内联到一个大型函数中，再重新提炼到小函数中
    - 代码太多间接层，使系统中的所有函数都似乎只是对另一个函数简单的委托

- 例子： 
    ```
    function getRating(driver){
        return moreThanFiveDeliveries(driver) ? 2 : 1;
    }
    
    function moreThanFiveDeliveries(driver){
        return driver.deliveries > 5;
    }
    ```
    ```
    function getRating(driver){
        return driver.deliveries > 5 ? 2 : 1;
    }
    ```

### 6.3 提炼变量

- 对立：内联变量

- 目的：去除不必要间接层/委托层，降低系统复杂度

- 场景：
    - 一堆不合理的函数，可以将其内联到一个大型函数中，再重新提炼到小函数中
    - 代码太多间接层，使系统中的所有函数都似乎只是对另一个函数简单的委托

- 例子： 
    ```
    return order.quantity * order.itemPrice - Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 + Math.min(order.quantity * order.itemPrice * 0.1, 100);
    ```
    ```
    const basePrice = order.quantity * order.itemPrice;
    const quantityDiscount =  Math.max(0, order.quantity - 500) * order.itemPrice * 0.05;
    const shipping = Math.min(basePrice * 0.1, 100);
    return basePrice - quantityDiscount + shipping;
    ```
    
### 6.4 内联变量

- 对立：提炼变量

- 目的：去除不必要变量

- 场景：
    - 表达式比变量更有表现力

- 例子： 
    ```
    const basePrice = order.basePrice;
    return basePrice > 25;
    ```
    ```
    return order.basePrice > 25;
    ```
    
### 6.5 改变函数声明

- 目的：好名字能让人一眼看出函数的用途，而不必看代码实现

- 使用：
    - 先写一句注释描述这个函数的用途，再把这句注释变成函数的名字

- 例子： 
    ```
    function calc(){}
    ```
    ```
    function calcOrder(){}
    ```
    
### 6.6 封装变量

- 目的：
    - 重构数据转移为重构函数，更易于处理
    - 监控数据的变化

- 场景：如果数据的可访问范围大

- 例子：
    ```
    let defaultOwner = {};
    ```
    ```
    let defaultOwner = {};
    export function defaultOwner(){
        return defaultOwner;
    }
    export function getDefaultOwner(arg){
        defaultOwner = arg;
    }
    ```

### 6.7 变量改名

- 目的：好名字可让上下问更清晰

- 例子： 
    ```
    const a = height * width;
    ```
    ```
    const area = height * width;
    ```
    
## 10 简化条件逻辑

# 10.1 分解条件表达式

- 目的：简化条件，易于理解代码逻辑

- 场景：当检查处理逻辑复杂时

- 例子：
	```
	if(!date.isBefore(plan.summerStart) && !date.isAfter(plan.summerEnd)){
		charge = quanlity * plan.summerRate;
	}else{
		charge = quanlity * plan.regularRate + plan.regularServiceCharge;
	}
	```
	```
	const isSummer = !date.isBefore(plan.summerStart) && !date.isAfter(plan.summerEnd);
	function summerCharge(){
		return quanlity * plan.summerRate;
	}
	function regularCharge(){
		return quanlity * plan.regularRate + plan.regularServiceCharge;
	}
	
	charge = isSummer() ? summerCharge() : regularCharge();
	```
# 10.2 合并条件表达式

- 目的：统一处理条件语句

- 场景：当检查条件各不相同，最终行为一致时

- 例子：
	```
	if(age < 18) return 'younger';
	if(exprience < 5) return 'younger';
	if(!isPassTest) return 'younger';
	```
	```
	if(isYounger()) return 'younger';
	function isYounger(){
		return age<18 || exprience < 5 || !isPassTest
	}
	```

# 10.3 卫语句取代嵌套条件表达式

- 目的：减少逻辑的复杂度

- 场景：当出现需要单独检查某个特定条件时

- 例子：
	```
	function getPayment(){
		let result;
		if(isRead){
			result = deadAmount();
		}else{
			if(isSeparated){
				result = seperatedAmount();
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
	```
	function getPayment(){
		if(isRead) return deadAmount();
		if(isSeparated) return seperatedAmount();
		if(isRetired) return retiredAmount();
		
		return normalAmount();
	}
	```

# 10.4 以多态取代条件表达式

- 目的：增强扩展性，减少逻辑的复杂度

- 场景：当多个逻辑处理情况

- 例子：
	```
	switch(bird.type){
		case 'EuropeanSwallow':
			return 'EuropeanSwallow';
		case 'AfricanSwallow':
			return 'AfricanSwallow';
		default:
			return 'unknown';
	}
	```
	```
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

# 10.5 引入特例

- 目的：提供复用性以及统一性

- 场景：如果某部分逻辑都在检查某个特殊值，并且处理的逻辑也都相同

- 例子：
	```
	if(customer === 'unknown'){
		customerName = 'occupant';
	}
	```
	```
	class UnknowCustomer{
		get name(){
			return 'occupant';
		}
	}
	```
# 10.6 引入断言

- 目的：保障传入值是可预测的，预习发现测试的BUG

- 例子：
	```
	if(this.discountRate){
		base = base - this.discountRate * base;
	}
	```
	```
	asset(this.discountRate>0);
	if(this.discountRate){
		base = base - this.discountRate * base;
	}
	```

## 11 重构API

# 11.1 查询函数和修改函数分离

- 目的：减少函数副作用 =》任何有返回值的函数，都要减少它的副作用

- 场景：当函数中又有查询，又有命令

- 例子：
	```
	function getTotalOutstadingAndSendBill(person){
		const result = customer.invoice.reduce((total, each)=>each.amount + total, 0);
		sendBill();
		return result;
	}
	```
	```
	function getTotalOutstading(person){
		return customer.invoice.reduce((total, each)=>each.amount + total, 0);
	}
	function sendBill(){}
	function handler(){
		const totalOutstading = getTotalstading();
		sendBilld();
	}
	```

# 11.2 函数参数化

- 目的：增强函数的功能

- 场景：如果发现多个函数逻辑相似，只有某1-2个字面量不同

- 例子：
	```
	function tenPercentRaise(person){
		person.salary = person.salary.multiply(1.1);
	}
	function fivePercentRaise(person){
		person.salary = person.salary.multiply(0.05);
	}
	```
	```
	function raise(person, factor){
		person.salary = person.salary.multiply(factor);
	}
	```

# 11.3 移除标记参数

- 目的：代码更清晰，减少函数的复杂度

- 场景：如果参数值影响函数内部的控制流

- 例子：
	```
	function setDimension(name, value){
		if(name === 'height'){}
		if(name === 'width'){}
	}
	```
	```
	function setHeight(value){
		this._height = value
	}
	function setWidth(value){
		this._width = value
	}
	```

# 11.4 保持对象完整性 

- 目的：缩短参数列表，参数配置灵活

- 场景：如果传入多个参数传值

- 例子：
	```
	const{low, high} = temperature;
	if(isValidTemperature(low,high)){};
	```
	```
	if(isValidTemperature(temperature)){};
	```

# 11.5 以查询取代参数 

- 对立：以参数取代查询

- 目的：减少传参，从而减少调用者的成本，保持

- 场景：如果传入多个参数，并且从一个参数推导出另一个参数

- 例子：
	```
	availableVacation(employee, employee.grade);
	function availableVacation(employee,grade){}
	```
	```
	availableVacation(employee);
	function availableVacation(employee){
		const{grade} = employee;
	}
	```

# 11.6 以参数取代查询

- 对立：以查询取代参数

- 目的：减少函数的副作用，以及引用关系，保持函数的纯净度

- 场景：如果函数引用了一个全局变量，或者引用想移除的元素

- 例子：
	```
	const weather ={};
	targetTemperature(plan)
	
	function targetTemperature(plan){
		const{curTemperature} = weather;
	}
	```
	```
	const weather ={};
	targetTemperature(plan, weather)
	
	function targetTemperature(plan, weather){
		const{curTemperature} = weather;
	}
	```

# 11.7 移除设置函数

- 目的：防止某字段被修改

- 场景：当不希望某个字段被修改时

- 例子：
	```
	class Person{
		get id(){}
		set id(name){}
	}
	```
	```
	class Person{
		get id(){}
	}
	```
	
# 11.8 以工厂函数取代构造函数

- 目的：增加灵活性

- 场景：不存在继承关系时

- 例子：
	```
	const leadEngineer = new Employee('name','E');
	```
	```
	const leadEngineer = createEngineer('name');
	```

# 11.9 以命令取代函数

- 对立：以函数取代命令

- 目的：命令对象提供更大的灵活性，并且还可以支持撤销、生命周期的管理等附加操作

- 场景：当普通函数无法提供强有力灵活性

- 例子：
	```
	function score(candidate, media){
		let result = 0;
		let healthLevel = 0;
	}
	function hasPassMedicalExam(){}
	```
	```
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

# 11.10 以函数取代命令

- 对立：以命令取代函数

- 目的：函数简单化

- 场景：大多数情况下，只想调用一个函数，完成自己的工作，不需要函数那么复杂

- 例子：
	```
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
	```
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

- 对立：函数下移

- 目的：提高复用性，减少重复

- 场景：当函数被大部分部分子类用到时

- 例子：
	```
	class Employee{}
	class Salesman extends Employee{
		get name(){}
	}
	class Engineer extends Employee{
		get name(){}
	}
	```
	```
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
	```
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
	```
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

- 对立：函数上移

- 目的：内聚子类的方法

- 场景：当函数被小部分子类用到时

- 例子：
	```
	class Employee{
		get quota(){}
	}
	class Salesman extends Employee{}
	class Engineer extends Employee{}
	```
	```
	class Employee{}
	class Salesman extends Employee{
		get quota(){}
	}
	class Engineer extends Employee{}
	```

### 12.5 字段下移

- 对立：字段上移

- 目的：内聚子类的字段

- 场景：当字段被小部分子类用到时

- 例子：
	```
	class Employee{
		constuctor(quote){
			this._quote = quote;
		}
	}
	class Salesman extends Employee{
		constuctor(quote){
			super(quote);
		}
	}
	class Engineer extends Employee{}
	```
	```
	class Employee{}
	class Salesman extends Employee{
		constuctor(quote){
			this._quote = quote;
		}
	}
	class Engineer extends Employee{}
	```

### 12.6 以子类取代类型码

- 对立：移除子类

- 目的：更明确地表达数据与类型之间的关系，增强子类的扩展性

- 场景：当不同的状态码表现不同的行为时

- 例子：
	```
	function createEmployee(name, type){
		return new Employee(name, type);
	}
	```
	```
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

- 对立：以子类取代类型码

- 目的：减少系统复杂度

- 场景：当子类的用处太少时，当子类简单

- 例子：
	```
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
	```
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
	```
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
	```
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
	```
	class Employee{}
	class Sales extends Employee{}
	```
	```transfer
	class Employee{}
	```

###  12.10 以委托取代子类

- 目的：增强类的扩展性


- 场景：当子类可能存在多种类型上的变化


- 例子
	```
	class Booking{
		constuctor(show, date){
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
	``` transfer
	class Booking{
		constuctor(show, date){
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
	``` 
	class List{}
	class Stack extends List{}
	```
	``` transfer
	class List{}
	class Stack{
		constructor(){
			this._list = new List();
		}
	}
	```
> 超类的所有方法都适用于子类，子类的所有实例都是超类的实例 => 使用继承尽量使用继承

> 如果发现继承有问题（扩展困难），再使用[以委托取代超类](#1211-以委托取代超类)
