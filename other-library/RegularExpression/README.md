正则表达式
===

> Create by **jsliang** on **2018-11-14 10:41:20**  
> Recently revised in **2018-11-14 10:41:25**

<br>

# <a name="chapter-one" id="chapter-one">一 目录</a>

<br>

&emsp;**不折腾的前端，和咸鱼有什么区别~**

| 目录 |
| --- |
| <a name="catalog-chapter-one" id="catalog-chapter-one"></a>[一 目录](#chapter-one) |
| <a name="catalog-chapter-two" id="catalog-chapter-two"></a>[二 整合](#chapter-two) |
| <a name="catalog-chapter-three" id="catalog-chapter-three"></a>[三 前言](#chapter-three) |
| <a name="catalog-chapter-four" id="catalog-chapter-four"></a>[四 test 方法](#chapter-four) |
| <a name="catalog-chapter-five" id="catalog-chapter-five"></a>[五 search 方法](#chapter-five) |
| <a name="catalog-chapter-six" id="catalog-chapter-six"></a>[六 match 方法](#chapter-six) |
| <a name="catalog-chapter-seven" id="catalog-chapter-seven"></a>[七 replace 方法](#chapter-seven) |
| <a name="catalog-chapter-eight" id="catalog-chapter-eight"></a>[八 敏感词过滤](#chapter-eight) |
| <a name="catalog-chapter-nine" id="catalog-chapter-nine"></a>[九 匹配子项](#chapter-nine) |
| <a name="catalog-chapter-ten" id="catalog-chapter-ten"></a>[十 字符类](#chapter-ten) |
| <a name="catalog-chapter-eleven" id="catalog-chapter-eleven"></a>[十一 过滤标签](#chapter-eleven) |
| <a name="catalog-chapter-twelve" id="catalog-chapter-twelve"></a>[十二 获取class的方法](#chapter-twelve) |
| <a name="catalog-chapter-thirteen" id="catalog-chapter-thirteen"></a>[十三 获取class的方法](#chapter-thirteen) |
| <a name="catalog-chapter-fourteen" id="catalog-chapter-fourteen"></a>[十四 量词](#chapter-fourteen) |

<br>

# <a name="chapter-two" id="chapter-two">二 学习整合</a>

> [目录](#catalog-chapter-two)

<br>

| 字符  | 描述                                                                         |
| ----- | ---------------------------------------------------------------------------- |
| ^     | 匹配输入字符串的开始位置                                                     |
| $     | 匹配输入字符串的结尾位置                                                     |
| ()    | 标记一个子表达式的开始和结束位置                                             |
| []    | 标记一个中括号表达式的开始，里面的元素整体代表一个字符                       |
| {n,m} | 匹配从n开始到m结束的位置                                                     |
| *     | 匹配前面子表达式的零次或多次（即 {0,} ）                                     |
| ?     | 匹配前面的子表达式零次或一次（即 {0,1} ）                                    |
| +     | 匹配前面的子表达式一次或多次（即 {1,} ）                                     |
| .     | 匹配除换行符\n之外的任何单字符                                               |
| \     | 转义字符。将下一个字符标记为特殊字符、或原义字符、或向后引用、或八进制转义符 |
|       |                                                                              | 指明两项之间选择一项，相当于“或” |
| \s    | 空格（\S：非空格）                                                           |
| \d    | 数字（\D：非数字）                                                           |
| \w    | 字符（字母，数字，下划线。\W：非字符）                                       |
| ?<=   | 匹配以其后面规则为开头的字符串                                               |
| ?=    | 匹配以()结尾的字符串，并存储到分组中                                         |

<br>

&emsp;常用匹配：

1. 密码强度
* 必须包含数字+小写字母+大写字母的密码，位数在8-10位之间：` ^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}$ `
* 只能是字母、数字和下划线：` ^\w+$ `
2. 校验中文：` ^[\u4e00-\u9fa5]{0,}$ `
3. Email验证：
```
[\w!#$%&'*+/=?^_`{|}~-]+(?:\.[\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\w](?:[\w-]*[\w])?\.)+[\w](?:[\w-]*[\w])? 
```
4. 身份证验证：` ^(\d{6})(\d{4})(\d{2})(\d{2})(\d{3})([0-9]|X)$ `
5. 手机号验证，以1开头，第二位数是3/4/5/7/8的11位手机号码：` ^1[3,4,5,7,8,9]\d{9}$ `

<br>

# <a name="chapter-three" id="chapter-three">三 前言</a>

> [目录](#catalog-chapter-three)

<br>

&emsp;正则表达式：正则，也叫做规则，让计算机能够读懂人类的规则。  
&emsp;正则都是用来操作字符串的。  
&emsp;我们要从一串字符串中，把所有相连的数字找出来，然后把它们拼接成一个新数组，用常规写法和正则表达式分别是怎么写的呢？  

&emsp;常规查找：

```
window.onload = function() {
  var str = "abc123def456hash789"
  function findNum(str) {
    var arr = [];
    var tmp = '';
    for(var i=0; i<str.length; i++) {
      if(str.charAt(i)<="9" && str.charAt(i)>="0") {
        tmp += str.charAt(i);
      } else {
        if(tmp) {
          arr.push(tmp);
        tmp = "";
      }
    }
  }
  if(tmp) {
    arr.push(tmp);
    tmp = "";
  }
  return arr;
  }
  console.log(findNum(str));
}
```

<br>

&emsp;那么，我们再试试正则查找：

```
window.onload = function() {
  var str = "abc123def456hash789";
  function findNum(str) {
    return str.match(/\d+/g);
  }
  console.log(findNum(str));
}
```

<br>

&emsp;很牛逼有木有？！只需要一行代码，就能解决字符串查找的时候用的一大串代码！

<br>

# <a name="chapter-four" id="chapter-four">四 test 方法</a>

> [目录](#catalog-chapter-four)

<br>

&emsp;test : 正则去匹配字符串，如果匹配成功就返回真，如果匹配失败就返回假  
&emsp;test的写法：正则.test(字符串)

```
// \s : 空格
// \S : 非空格
// \d : 数字
// \D : 非数字
// \w : 字符 （字母，数字，下划线_）
// \W : 非字符

var str = "12313213ab2131";
var re = /\D/;
if(re.test(str)) {
  console.log("不全是数字！");
} else {
  console.log("全是数字！");
}
```

<br>

# <a name="chapter-five" id="chapter-five">五 search 方法</a>

> [目录](#catalog-chapter-five)

<br>

&emsp;search：正则去匹配字符串，如果匹配成功，就返回匹配成功的位置，如果匹配失败就返回-1  
&emsp;search 的写法：字符串.search(正则)

```
// 正则中的默认，是区分大小写的
// 如果不区分大小写的话，在正则的最后加标识i
var str = "abcdef";
var re = /g/i;
// var re = new RegExp("B", "i");
console.log(str.search(re));
```

<br>

# <a name="chapter-six" id="chapter-six">六 match 方法</a>

> [目录](#catalog-chapter-six)

<br>

&emsp;match : 正则去匹配字符串，如果匹配成功，就返回匹配成功的数组，如果匹配不成，就返回null  
&emsp;match的写法: 字符串.match(正则)

```
// 正则默认： 正则匹配成功就会结束，不会继续匹配
// 如果想查找全部，就要加标识 g（全局匹配）
// 量词：匹配不确定的位置
// + ： 至少出现一次

var str = "123fadf321dfadf4fadf1";
var re = /\d+/g;
console.log(str.match(re));
```

<br>

# <a name="chapter-seven" id="chapter-seven">七 replace 方法</a>

> [目录](#catalog-chapter-seven)

<br>

&emsp;replace ： 正则去匹配字符串，匹配成功的字符串去替换成新的字符串  
&emsp;replace的写法： 字符串.replace(正则,新的字符串)

```
var str = 'aaa';
var re = /a+/g;
str = str.replace(re, "b");
console.log(str);
```

<br>

# <a name="chapter-eight" id="chapter-eight">八 敏感词过滤</a>

> [目录](#catalog-chapter-eight)

<br>

> *.html

```
<div class="filtering-of-sensitive-words">
  <!-- 7、敏感词过滤 -->
  <h3>敏感词过滤</h3>
  <p>替换前</p>
  <textarea name="before" id="" cols="30" rows="10"></textarea>
  <input type="button" value="确定" id="input1">
  <p>替换后</p>
  <textarea name="after" id="" cols="30" rows="10"></textarea>
</div>
```

<br>

> *.js

```
// | : 或的意思
// replace : 第二个参数 ： 可以是字符串，也可以是一个回调函数

window.onload = function() {
  var aT = document.getElementsByTagName("textarea");
  var oInput = document.getElementById("input1");

  // 非诚勿扰在中国船的监视之下寸步难行
  var re = /非诚|中国船|监视之下/g;

  oInput.onclick = function() {
    // 字符串写法： aT[1].value = at[0].value.replace(re, "*");
    aT[1].value = aT[0].value.replace(re, function(str) {
      // 函数的第一个参数，就是匹配成功的字符
      var result = "";
      for(var i=0; i<str.length; i++) {
        result += "*";
      }
      return result;
    });
  }
}
```

<br>

# <a name="chapter-nine" id="chapter-nine">九 匹配子项</a>

> [目录](#catalog-chapter-nine)

<br>

&emsp;匹配子项 ： 小括号() (还有另外一个意思，分组操作)
&emsp;把正则的整体叫做（母亲）
&emsp;然后把左边第一个括号里面的正则，叫做这个第一个子项（母亲的第一个孩子）
&emsp;第二个小括号就是第二个孩子

> *.js

```
var str = "2018-5-25";
var re = /(\d+)(-)/g;
str = str.replace(re, function($0, $1, $2){
  // 第一个参数：$0（母亲),第二个参数：$1（第一个孩子），第三个参数：$1(第二个孩子)
  return $1 + '.';
  // return $0.substring(0, $0.length-1) + ".";
});
console.log(str);
```

<br>

&emsp;匹配子项2

```
var str = "abc";
var re = /(a)(b)(c)/
console.log(str.match(re));
// 输出 ： [ 'abc', 'a', 'b', 'c', index: 0, input: 'abc' ]
// 当match不加g的时候才能获取子项的集合
```

<br>

# <a name="chapter-ten" id="chapter-ten">十 字符类</a>

> [目录](#catalog-chapter-ten)

<br>

&emsp;字符类：一组类似的元素 [] 中括号的整体代表一个字符

```
// var str = "abcd";
// var re = /a[bcd]c/;
// console.log(re.test(str));

// 排除 ： ^ 如果写在[]里面的话，就代表排除的意思
// var str = "abc";
// var re = /a[^bcd]c/;
// console.log(re.test(str));

var str = "a.c";
var re = /a[a-z0-9A-Z]c/;
console.log(re.test(str));
```

<br>

# <a name="chapter-eleven" id="chapter-eleven">十一 过滤标签</a>

> [目录](#catalog-chapter-eleven)

<br>

```
window.onload = function() {
  var filterLabel = document.getElementById("filter-label");
  var aT = filterLabel.getElementsByTagName("textarea");
  var oInput = document.getElementById("input2");
  // var re = /<\w+/g;
  var re = /<[^>]+>/g;
  oInput.onclick = function() {
    aT[1].value = aT[0].value.replace(re, "");
  };
};
```

<br>

# <a name="chapter-twelve" id="chapter-twelve">十二 获取class的方法</a>

> [目录](#catalog-chapter-twelve)

<br>

```
window.onload = function () {
  // var aLi = document.getElementsByClassName("li1");
  var aLi = getByClass(document, "li1");
  for (var i = 0; i < aLi.length; i++) {
    aLi[i].style.background = "red";
  }

  function getByClass(oParent, sClass) {
    var arr = [];
    var aEle = oParent.getElementsByTagName("*");
    // var re = /sClass/; // 当正则需要传参的时候，一定要用全称的写法
    var re = new RegExp("\\b" + sClass + "\\b");
    for (var i = 0; i < aEle.length; i++) {
    if(re.test(aEle[i].className)) {
      arr.push(aEle[i]);
      }
    }
    return arr;
  }
};
```

<br>

# <a name="chapter-thirteen" id="chapter-thirteen">十三 获取class的方法</a>

> [目录](#catalog-chapter-thirteen)

<br>

```
// \1：重复的第一个子项
// \2: 重复的第二个子项
// ……

var str1 = "abca";
var re1 = /(a)(b)(c)\1/;
console.log(re1.test(str1));

var str2 = "assbsscssadc";
var arr2 = str2.split("");
str2 = arr2.sort().join("");
var value = "";
var index = 0;
var re = /(\w)\1+/g;
str2.replace(re, function($0, $1){
if(index<$0.length) {
  index = $0.length;
  value = $1;
}
});
console.log("最多的字符："+value+"，重复的次数："+index);
```

<br>

# <a name="chapter-fourteen" id="chapter-fourteen">十四 量词</a>

> [目录](#catalog-chapter-fourteen)

<br>

```
// 量词 ： {}
/*

{4, 7} : 最少出现4次，最多出现7次
{4,} : 最少出现4次
{4} :正好出现4次

+ : {1,} // 类似于\d{1,}
? : {0,1} : 出现0次或者1次
* : {0,} : 至少出现0次

*/

var str = "ac";
var re = /ab*/;
console.log(re.test(str));

// ^ : 正则的最开始位置，代表起始的意思
// $ : 正则的最末尾位置，代表结束的意思

//判断是不是QQ号
window.onload = function () {
  var aInput = document.getElementById("isQQ").getElementsByTagName("input");
  var re = /^[1-9]\d{4,11}$/;
  aInput[1].onclick = function () {
    if (re.test(aInput[0].value)) {
      console.log("是QQ号");
    } else {
      console.log("不是QQ号");
    }
  };
};

// 去掉前后空格
var str = ' hello ';
console.log('(' + trim(str) + ')');

function trim(str) {
  var re = /^\s+|\s+$/g;
  return str.replace(re, '');
}

console.log(trim(str));

// 正则集合
var re = {
qq: /[1-9][0-9]{4,9}/,
email: /^\w+@[a-z0-9]+(\.[a-z]+){1,3}$/,
number: /\d+/
};

re.email
```

<br>

> <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /><a xmlns:dct="http://purl.org/dc/terms/" property="dct:title">**jsliang** 的文档库</a> 由 <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/LiangJunrong/document-library" property="cc:attributionName" rel="cc:attributionURL">梁峻荣</a> 采用 <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">知识共享 署名-非商业性使用-相同方式共享 4.0 国际 许可协议</a>进行许可。<br />基于<a xmlns:dct="http://purl.org/dc/terms/" href="https://github.com/LiangJunrong/document-library" rel="dct:source">https://github.om/LiangJunrong/document-library</a>上的作品创作。<br />本许可协议授权之外的使用权限可以从 <a xmlns:cc="http://creativecommons.org/ns#" href="https://creativecommons.org/licenses/by-nc-sa/2.5/cn/" rel="cc:morePermissions">https://creativecommons.org/licenses/by-nc-sa/2.5/cn/</a> 处获得。