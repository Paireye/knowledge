# JS常用



### 虚拟表单

> 发送POST请求跳转到指定页面
> 通过虚拟表单的形式提交post请求

```
function httpPost(url, params) {
    var temp = document.createElement("form");
    temp.action = url;
    temp.method = "post";
    temp.style.display = "none";
    for (var x in params) {
        var opt = document.createElement("input");
        opt.setAttribute("type", "hidden");
        opt.setAttribute("name", x);
        opt.setAttribute("value", params[x]); 
        temp.appendChild(opt);
    }
    document.body.appendChild(temp);
    temp.submit();
    return temp;
}

function send(){
    var pageUrl =  "/mproject/toInfoPage";
    var params = {
        "ppc_ctime": data.PPC_CTIME,
        "proc_name": data.PROC_NAME,
        "type": data.TYPE
    };
    //通过POST发送表单
    httpPost(pageUrl, params);
}
```



### 表单提交

- HTML 内容

```html
 <form id="editfrom" name="editfrom" method="post">
    <input name="name1" type="hidden" value="">
 </from>
<a href="javascript:void(0);" onclick="saveFrom('#editfrom')" >提交表单</a>
```

- JS 中的数据内容

```javascript
function saveFrom(formId){
	var v=$(formId).form('validate');//表单验证
	if(!v){
		$.messager.alert('信息提示', '您的信息填写不完整!', 'info'); 
		return false;
	}
	$.ajax({
		url:"/fundGrid/addSave",
		cache:false,
		data:$(formId).serializeArray(),
        dataType:"json",
        type: 'post',
		success: function(data){
	        if(data.code =='1'){
	        	$.messager.alert('提示','保存成功','info');
	        }else{
	        	$.messager.alert('提示',data.message,'info');
	        } 
		}
	}); 
}
```



### getJSON

>  通过 HTTP GET 请求载入 JSON 数据。

```javascript
jQuery.getJSON(url,data,success(data,status,xhr))
```

| 参数                       | 描述                                                         |
| -------------------------- | ------------------------------------------------------------ |
| `url`                      | 必需。规定将请求发送的哪个 URL。                             |
| `data`                     | 可选。规定连同请求发送到服务器的数据。                       |
| `success(data,status,xhr)` | 可选。规定当请求成功时运行的函数。额外的参数：*`response`* - 包含来自请求的结果数据；  *`status`* - 包含请求的状态；  *`xhr`* - 包含 XMLHttpRequest 对象； |



### 详细说明

该函数是简写的 Ajax 函数，等价于：

```javascript
$.ajax({
  url: url,
  data: data,
  success: callback,
  dataType: json
});
```

发送到服务器的数据可作为查询字符串附加到 URL 之后。如果 *data* 参数的值是对象（映射），那么在附加到 URL 之前将转换为字符串，并进行 URL 编码。

传递给 *callback* 的返回数据，可以是 JavaScript 对象，或以 JSON 结构定义的数组，并使用 $.parseJSON() 方法进行解析

- 示例 ： 从 test.js 载入 JSON 数据并显示 JSON 数据中一个 name 字段数据：

```javascript
$.getJSON("test.js",{'name':'123'}, function(json){
  alert("JSON Data: " + json.users[3].name);
});
```



### typeof用法

typeof 运算符把类型信息当作字符串返回。typeof 返回值有六种可能： `number`， `string` ，`boolean`， `object`， `function` 和  `undefined` 我们可以使用typeof来获取一个变量是否存在，如 `if(typeof a != "undefined"){}`，而不要去使用  `if( a )`  因为如果a不存在（未声明）则会出错，对于 Array，Null 等特殊对象使用 typeof 一律返回 object，这正是 typeof 的局限性。

`typeof`运算符后跟操作数：

```
typeof operand
or
typeof (operand) 
```

| 参数        | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| `operand`   | 是一个表达式，表示对象或[原始值](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)，其类型将被返回。 |
| `(operand)` | 括号是可选的。                                               |

**`typeof`可能的返回值**

| 类型                                        | 结果                       |
| :------------------------------------------ | :------------------------- |
| Undefined                                   | `"undefined"`              |
| Null                                        | `"object"`（见下文）       |
| Boolean                                     | `"boolean"`                |
| Number                                      | `"number"`                 |
| String                                      | `"string"`                 |
| Symbol （ECMAScript 6 新增）                | `"symbol"`                 |
| 宿主对象（由JS环境提供）                    | *Implementation-dependent* |
| 函数对象（[[Call]] 在ECMA-262条款中实现了） | `"function"`               |
| 任何其他对象                                | `"object"`                 |

**示例**

```js
// Numbers
typeof 37 === 'number';
typeof 3.14 === 'number';
typeof Math.LN2 === 'number';
typeof Infinity === 'number';
typeof NaN === 'number'; // 尽管NaN是"Not-A-Number"的缩写
typeof Number(1) === 'number'; // 但不要使用这种形式!

// Strings
typeof "" === 'string';
typeof "bla" === 'string';
typeof (typeof 1) === 'string'; // typeof总是返回一个字符串
typeof String("abc") === 'string'; // 但不要使用这种形式!

// Booleans
typeof true === 'boolean';
typeof false === 'boolean';
typeof Boolean(true) === 'boolean'; // 但不要使用这种形式!

// Symbols
typeof Symbol() === 'symbol';
typeof Symbol('foo') === 'symbol';
typeof Symbol.iterator === 'symbol';

// Undefined
typeof undefined === 'undefined';
typeof declaredButUndefinedVariable === 'undefined';
typeof undeclaredVariable === 'undefined'; 

// Objects
typeof {a:1} === 'object';

// 使用Array.isArray 或者 Object.prototype.toString.call
// 区分数组,普通对象
typeof [1, 2, 4] === 'object';

typeof new Date() === 'object';

// 下面的容易令人迷惑，不要使用！
typeof new Boolean(true) === 'object';
typeof new Number(1) === 'object';
typeof new String("abc") === 'object';

// 函数
typeof function(){} === 'function';
typeof class C{} === 'function'
typeof Math.sin === 'function';
typeof new Function() === 'function';
```



### jquery click点击一次执行两次解决方法

```javascript
$('#clickId').unbind('click').click(function () {
    ...
});
```



### 毫秒转换成时间

```javascript
  //获得年月日 得到日期oTime
  function getMyDate (str) {
    var oDate = new Date(str);
    var oYear = oDate.getFullYear();
    var oMonth = oDate.getMonth() + 1;
    var oDay = oDate.getDate();
    var oHour = oDate.getHours();
    var oMin = oDate.getMinutes();
    var oSen = oDate.getSeconds();
    //最后拼接时间
    var oTime = oYear + '-' + getzf(oMonth) + '-' + getzf(oDay) + ' ' + getzf(oHour) + ':' + getzf(oMin) + ':' + getzf(oSen);
    return oTime;
  };
  //补0操作
  function getzf (num) {
    if (parseInt(num) < 10) {
      num = '0' + num;
    }
    return num;
  }
```



### 让js中的函数只有一次有效调用的三种常用方法

1. 通过闭包来实现。

```
  window.onload = function () {
    function once (fn) {
      var result;

      return function () {
        if (fn) {
          result = fn.apply(this, arguments);
          fn = null;
        }
        return result;
      };
    }

    var callOnce = once(function () {
      console.log('javascript');
    });

    callOnce(); // javascript
    callOnce(); // null
  }
```
2. 第一次调用后，把func函数值空。func= function(){};

```
  var func = function () {
    alert("正常调用");
    func = function () {};
  }
  func();
  func();
```

3. 设置一个值，通过boolean来控制后面的调用。flag

```
  window.onload = function () {
    var flag = true;

    function once () {
      if (flag) {
        alert("我被调用");
        flag = false;
      } else {
        return;
      }
    }

    once();
    once();
  }
```