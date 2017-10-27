### JS数据结构-栈

1. 栈的JS实现

   定义一个Stack类，并定义一些方法来实现栈的操作：

   ```
   function Stack() {

   	//创建一个数组来存储栈里的数据
   	var items = [];
   	
   	// 推入一个元素入栈
   	this.push = function (ele) {
   		items.push(ele);
   	}
   	
   	// 删除栈顶的一个元素
   	this.pop = function () {
   		return items.pop()
   	}

   	// peek方法返回栈顶元素，不对栈本身做任何变动
   	this.peek = function () {
   		return items[items.length - 1];
   	}

   	// isEmpty方法检测栈是否为空
   	this.isEmpty = function () {
   		return items.length === 0;
   	}

   	// size方法返回栈的长度
   	this.size = function () {
   		return items.length;
   	}

   	// clear方法清除栈的所有元素
   	this.clear = function () {
   		items = [];
   	}

   }
   ```

2. 栈的使用：进制转换

   实现十进制到二进制的转换：

   ![](http://oxx2l7d61.bkt.clouddn.com/js%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84-%E6%A0%88_1.png)

   代码实现：

   ```
   function binaryTo2 (num) {
   	var remStack = new Stack () ,
   		rem,
   		binaryString = '';

   	// 将二进制数存入栈中
   	while (num > 0) {
   		// 获取余数
   		rem = Math.floor(num % 2);
   		// 余数推入栈中
   		remStack.push(rem);
   		// 获取结果以便下次循环
   		num = Math.floor(num / 2);
   	}

   	// 从栈中取二进制数
   	while (! remStack.isEmpty())
   		binaryString += remStack.pop().toString();

   	return binaryString;
   }

   binaryTo2(10) // "1010"
   ```

   修改算法，使之适用于2~16进制中任意进制的转换：

   ```
   function binaryPro (num, base) {
   	var remStack = new Stack () ,
   		rem,
   		binaryString = '',
   		model = '0123456789ABCDEF' ;

   	// 将x进制数存入栈中
   	while (num > 0) {
   		// 获取余数
   		rem = Math.floor(num % base);
   		// 余数推入栈中
   		remStack.push(rem);
   		// 获取结果以便下次循环
   		num = Math.floor(num / base);
   	}

   	// 从栈中取x进制数
   	while (! remStack.isEmpty())
   		binaryString += model[remStack.pop()];

   	return binaryString;
   }

   binaryPro(10, 2) // "1010" 
   binaryPro(31, 16) // "1F" 
   ```

   ​