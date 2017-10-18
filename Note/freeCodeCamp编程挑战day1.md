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

#### 3.一个句子中每个单词首字母大写