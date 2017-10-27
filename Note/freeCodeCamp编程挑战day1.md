## freeCodeCamp编程挑战

#### 1.检查回文字符串，如果一个字符串忽略标点符号、大小写和空格，正着读和反着读一模一样，那么这个字符串就是palindrome(回文)。

思路：构造一个函数，传入待检查字符串，若是回文则返回true，反之返回false

实现：

```
function palindrome(str) {
	//去除字符串中所有空格和标点符号并变成小写，将处理后的字符串存入result
  	var result = str.replace(/[\s\,\_\.\-\:\\\/\(\)]*/g,'').toLowerCase();

  	//反转字符串并变成小写之后并存入reverse
  	var reverse = result.split('').reverse().join('').toLowerCase();

  	if (result === reverse)
  		return true;
  	return false;
}
```

#### 2.找出一个句子中最长的单词，并返回它的长度

```
function findLongestWord(str) {
    //先将句子中每个单词分开并存入数组arr
    var arr = str.split(' ');
    //定义空数组length用于存放每个单词的长度
    var length = [];
    //遍历arr 将每个单词的长度push到length数组中
    arr.forEach(function (item, index) {
    	length.push(item.length); 
    });
    //将length数组从小到大排序并且执行pop()操作，会返回最后一个元素，也就是最大长度值
    var result = length.sort(function (a,b) {
    	 return a-b;
    }).pop()
    return result;
}

findLongestWord("What if we try a super-long word such as otorhinolaryngology"); 
```

如果要求不仅返回最大长度值，还返回对应的单词怎么办呢？

。。。。。。。

#### 3.一个句子中每个单词首字母大写其他字母小写

```
function titleCase(str) {
	//将传入的句子分词并存入arr中 
	var arr = str.split(' ');
	//将每个单词首字母大写 其他字母小写
	str = arr.map(function (item) {
		//将每个单词分成字母并存入数组arr1
		var arr1 = item.split('');
	  	arr1 = arr1.map(function (item, index) {
			if (index === 0) {
				 item = item.toUpperCase();
			}else {
				 item = item.toLowerCase();
			}
			return item;
	});
	//重新拼接处理后的单词
	item = arr1.join('');
	//返回每个单词
	return item;
	});
	//拼接句子
	str = str.join(' ');
	return str;
}
```

#### 4.猴子吃香蕉 分割数组

把一个数组 `arr` 按照指定数组大小 `size` 分割成若干个数组块，例如：

chunk([1,2,3,4], 2) = [[1,2],[3,4]];
chunk([1,2,3,4,5], 2) = [[1,2],[3,4],[5]];

```
function chunk(arr, size) {
	var result = [];
	for (var i = 0; i < Math.ceil(arr.length/size); i++) {
		var item = arr.slice(i*size, i*size+size);
		result.push(item);
	}
	
	return result;
}

chunk(["a", "b", "c", "d"], 2);
```

思路：遍历`arr`，每次分割出`size`个元素，并且存入`result`数组中

```
Math.ceil() // 向上取整
Math.floor() // 向下取整
Math.round() // 四舍五入
```

#### 5.比较字符串

如果数组第一个字符串元素包含了第二个字符串元素的所有字符，函数返回true，例如：
`mutation(["Mary", "Aarmy"])` 应当返回 `true` 。

```
function mutation(arr) {
  	var arr1 = arr[1].toLowerCase().split('');
  	for (var i = 0; i < arr1.length; i++) {
  		if (arr[0].toLowerCase().indexOf(arr1[i]) === -1)
  			return false;
  	}
  	return true;
}

mutation(["hello", "Hello"]);   //true
```

思路：1.将数组中第2个参数分割成数组arr1
​	    2.遍历数组arr1
​	    3.如果arr1中存在一个元素不存在于第一个参数时，返回false
​	    4.否则返回true

#### 6.过滤数组假值

删除数组中所有假值，在js中假值有`false`、`null`、`0`、`''`、`undefined`、`NaN` 

```
function bouncer(arr) {
	arr = arr.filter(function (item) {
		return !!item === true;
	});
	return arr;
}

bouncer([7, "ate", "", false, 9]); // [[7, "ate", 9]]
```

思路：1.用filter函数过滤不符合规则的元素
​            2. `!! item` 将任意类型变量转换成布尔值。

#### 7.摧毁数组

实现一个摧毁(destroyer)函数，第一个参数是待摧毁的数组，其余的参数是待摧毁的值。

```
function destroyer(arr) {
    var args = [];
    for(var i = 1;i < arguments.length;i ++) {
        args.push(arguments[i]);
    }
    arr = arr.filter(function (item) {
        return args.indexOf(item) < 0 ;
    });
    return arr;
}

destroyer([1, 2, 3, 1, 2, 3], 2, 3);  // [1, 1]
```

思路：1.首先用一个数组 `args` 存储除第一个参数外的所有参数，即要删除的值

​	    2.用`filter`函数过滤掉`arr`里同样存在与`args`数组中的值。

#### 8.凯撒密码

1.移位密码也就是密码中的字母会按照指定的数量来做移位。

2.一个常见的案例就是ROT13密码，字母会移位13个位置。由'A' ↔ 'N', 'B' ↔ 'O'，以此类推。

3.写一个ROT13函数，实现输入加密字符串，输出解密字符串。

所有的字母都是大写，不要转化任何非字母形式的字符(例如：空格，标点符号)，遇到这些特殊字符，跳过它们。

```
function rot13(str) {

	// 用于存储字符串中所有字符的Unicode值
	var arr = [];
	
	// 存储结果
	var re = [];
	
	for (let i = 0; i < str.length; i++) {
		arr.push(str[i].charCodeAt(0));
	}

	for (let i = 0; i < arr.length; i++) {

		// 如果字符是A~Z 则编码加13
		if (arr[i] >= 65 && arr[i] <= 90) {
			if (arr[i] <= 77)
				arr[i] += 13;
			else
				arr[i] = arr[i] + 13 - 26 ;
		}
		re.push(String.fromCharCode(arr[i]));
	}

	// 重新连接字符串
	re = re.join('');
	return re;
}


 rot13("SERR PBQR PNZC");  // "FREE CODE CAMP"
```

1.首先存储str中所有字符的Unicode值，存到arr。

```
str.charCodeAt(i)  //返回字符串中索引值为i的字符的Unicode编码
```

2.遍历arr，将A~Z的Unicode编码按 '移13位' 的规则处理，其余字符编码不变

3.将处理后的每一个字符编码转换成对应字符并推入re中

```
String.fromCharCode(num) //返回num对应的字符 num为单个字符的Unicode编码
```

4.重新连接字符串