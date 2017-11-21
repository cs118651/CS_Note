#### 深拷贝与浅拷贝

浅拷贝

```js
var shallowCopy = function (obj) {
  if (typeof obj !== 'object') {
    return
  }
  
  var newObj = obj instanceof Array ? [] : {}
  for (var key in obj) {
    if (obj.hasOwnProperty(key)) {
      newObj[key] = obj[key]
    }
  }

  return obj
}
```

深拷贝

```js
var deepCopy = function (obj) {
  if (typeof obj !== 'object') {
    return
  }

  var newObj = obj instanceof Array ? [] : {}
  for (var key in obj) {
    if (obj.hasOwnProperty(key)) {
      newObj[key] = typeof obj[key] === 'object' ? deepCopy(obj[key]) : obj[key]
    }
  }

  return newObj
}
```

