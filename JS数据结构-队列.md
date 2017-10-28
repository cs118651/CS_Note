### JS数据结构-队列

1.队列的JS实现

```
function Queue() {
	
	var items = [];
 
	this.enqueue = function (ele) {
		items.push(ele);
	}

	this.dequeue = function () {
		return items.shift()
	}

	this.front = function () {
		return items[0];
	}

	this.isEmpty = function () {
		return items.length === 0;
	}

	this.clear = function () {
		items = [];
	}

	this.size = function () {
		return items.length;
	}
}
```

2. JS实现优先级队列(priority数值越小优先级越高)

```
function PriorityQueue () {

	var items = [];
	// 新元素的抽象 每个新元素都是一个QueueElement 带优先级属性priority
	function QueueElement (ele, priority) {
		this.ele = ele;
		this.priority = priority;
	}
	
	// 进入队列的方法(依照优先级进入队列)
	this.enqueue = function (ele, priority) {

		var queueElement = new QueueElement (ele, priority);

		if (items.length === 0) {
			items.push(queueElement);
		} else {
			for (var i = 0; i < items.length; i++) {

				var flag = false;

				// 如果新入列的元素优先级比items[i]高
				if (queueElement.priority < items[i]) {
					items.splice(i, 0, queueElement);
					flag = true;
					break;
				}
			}

			// 如果队列中所有元素优先级都比新插入元素高
			if (!flag)
				items.push(queueElement);
		}
	}

	// 其他方法与Queue相同
}
```

