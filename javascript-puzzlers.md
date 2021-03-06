
# 44道JavaScript难题[（原文）](http://javascript-puzzlers.herokuapp.com/ "JavaScript难题")

#### 1. ["1", "2", "3"].map(parseInt)
    答案：[1, NaN, NaN]
    解析：parseInt (val, radix) ：两个参数，val值，radix基数（就是多少进制转换）
         map 能传进回调函数 3参数 (element, index, array)
         parseInt('1', 0);  //0代表10进制
         parseInt('2', 1);  //没有1进制，不合法
         parseInt('3', 2);  //2进制根本不会有3
    巩固：["1", "1", "11","5"].map(parseInt) //[1, NaN, 3, NaN]  
#### 2. [typeof null, null instanceof Object]
    答案：["object", false]
    解析：null代表空对象指针，所以typeof判断成一个对象。可以说JS设计上的一个BUG
         instanceof 实际上判断的是对象上构造函数，null是空当然不可能有构造函数
    巩固：null == undefined //true    null === undefined //flase
#### 3. [ [3,2,1].reduce(Math.pow), [].reduce(Math.pow) ]
    答案：an error
    解析：Math.pow (x , y)  x 的 y 次幂的值
         reduce（fn,total）
         fn (total, currentValue, currentIndex, arr) 
             如果一个函数不传初始值，数组第一个组默认为初始值.
             [3,2,1].reduce(Math.pow)
             Math.pow(3,2) //9
             Math.pow(9,1) //9
         [].reduce(Math.pow)       //空数组会报TypeError
    巩固：[1].reduce(Math.pow)      //只有初始值就不会执行回调函数，直接返回1
         [].reduce(Math.pow,3)     //只有初始值就不会执行回调函数，直接返回1
         [2].reduce(Math.pow,3)    //传入初始值，执行回调函数，返回9
#### 4. 
     var val = 'smtg';
     console.log('Value is ' + (val === 'smtg') ? 'Something' : 'Nothing');

这段代码的执行结果？ 
    
    答案：Something
    解析：字符串连接比三元运算有更高的优先级 
         所以原题等价于 'Value is true' ? 'Somthing' : 'Nonthing' 
         而不是 'Value   is' + (true ? 'Something' : 'Nonthing')
    巩固：0 && null === 0  //0    === 优先级高于 &&
#### 5. 
    var name = 'World!';
    (function () {
    if (typeof name === 'undefined') {
        var name = 'Jack';
        console.log('Goodbye ' + name);
    } else {
        console.log('Hello ' + name);
    }
    })();

这段代码的执行结果？ 
    
    答案：Goodbye Jack
    解析：（1）typeof时var name 在函数内部之声明未定义
         （2）typeof优先级高于===
    巩固：
        var str = 'World!';   
        (function (name) {
        if (typeof name === 'undefined') {
            var name = 'Jack';
            console.log('Goodbye ' + name);
        } else {
            console.log('Hello ' + name);
        }
        })(str);
        答案：Hello World 因为name已经变成函数内局部变量
#### 6.               
    var END = Math.pow(2, 53);
    var START = END - 100;
    var count = 0;
    for (var i = START; i <= END; i++) {
        count++;
    }
    console.log(count);

这段代码的执行结果？ 
    
    答案：other ,不是101
    解析：js中可以表示的最大整数不是2的53次方，而是1.7976931348623157e+308。2的53次方不是js能表示的最大整数而应该是能正确计算且不失精度的最大整数，
    巩固：
    var END = 1234567635;
    var START = END - 1024;
    var c = count = 0;
    for (var i = START; i <= END; i++) {
        c = count++;
    }
    console.log(count);   //1025
    console.log(c);       //1024
#### 7.         
    var ary = [0,1,2];
    ary[10] = 10;
    ary.filter(function(x) { return x === undefined;}); 
这段代码的执行结果？ 

    答案：[]
    解析：filter() 不会对空数组进行检测。会跳过那些空元素
    巩固：
          var ary = [0,1,2,undefined,undefined,undefined,null];
          ary.filter(function(x) { return x === undefined;});
          // [undefined, undefined, undefined] 
#### 8.                 
    var two   = 0.2
    var one   = 0.1
    var eight = 0.8
    var six   = 0.6
    [two - one == one, eight - six == two]
这段代码的执行结果？ 

    答案：[true, false]
    解析：IEEE 754标准中的浮点数并不能精确地表达小数
    巩固：var two   = 0.2;
         var one   = 0.1;
         var eight = 0.8;
         var six   = 0.6;
         ( eight - six ).toFixed(4) == two 
         //true
#### 9.  
    function showCase(value) {
        switch(value) {
        case 'A':
            console.log('Case A');
            break;
        case 'B':
            console.log('Case B');
            break;
        case undefined:
            console.log('undefined');
            break;
        default:
            console.log('Do not know!');
        }
    }
    showCase(new String('A'));
这段代码的执行结果？ 

    答案：Do not know!
    解析：new String(x)是个对象
    巩固：下一题
#### 10.
    function showCase2(value) {
        switch(value) {
        case 'A':
            console.log('Case A');
            break;
        case 'B':
            console.log('Case B');
            break;
        case undefined:
            console.log('undefined');
            break;
        default:
            console.log('Do not know!');
        }
    }
    showCase2(String('A'));
这段代码的执行结果？ 

    答案：Case A
    解析：String('A')就是返回一个字符串
#### 11.
    function isOdd(num) {
        return num % 2 == 1;
    }
    function isEven(num) {
        return num % 2 == 0;
    }
    function isSane(num) {
        return isEven(num) || isOdd(num);
    }
    var values = [7, 4, '13', -9, Infinity];
    values.map(isSane);
这段代码的执行结果？ 

    答案：[true, true, true, false, false]
    解析：%如果不是数值会调用Number（）去转化
         '13' % 2       // 1
          Infinity % 2  //NaN  Infinity 是无穷大
          -9 % 2        // -1
    巩固： 9 % -2        // 1   余数的正负号随第一个操作数
#### 12.
    parseInt(3, 8)
    parseInt(3, 2)
    parseInt(3, 0)
这段代码的执行结果？ 

    答案：3  NaN  3
    解析：2进制不可能有3
#### 13.
    Array.isArray( Array.prototype )
这段代码的执行结果？ 

    答案：true
    解析：Array.prototype是一个数组
         数组的原型是数组，对象的原型是对象，函数的原型是函数
#### 14.
    var a = [0];
    if ([0]) {
      console.log(a == true);
    } else {
      console.log("wut");
    }
这段代码的执行结果？ 

    答案：false
    解析：[0]的boolean值是true
    巩固：a[0] 的boolean是 false
#### 15.[]==[]
    答案：false
    解析：两个引用类型， ==比较的是引用地址
    巩固：[]== ![] 
         (1)! 的优先级高于== ，右边运算结果等于 false
         (2)一个引用类型和一个值去比较 把引用类型转化成值类型，左边0
         (3)所以 0 == false  答案是true
#### 16.        
    '5' + 3
    '5' - 3
  这段代码的执行结果？  

    答案：53  2
    解析：加号有拼接功能，减号就是逻辑运算
#### 17.  1 + - + + + - + 1      
    答案：2
    解析：+-又代表正负号， 负负得正。

#### 18. 
    var ary = Array(3);
    ary[0]=2
    ary.map(function(elem) { return '1'; });
这段代码的执行结果？ 

    答案：["1", empty × 2]
    解析：如过没有值，map会跳过不会执行回调函数
#### 19.
    function sidEffecting(ary) {
      ary[0] = ary[2];
    }
    function bar(a,b,c) {
      c = 10
      sidEffecting(arguments);
      return a + b + c;
    }
    bar(1,1,1) 
这段代码的执行结果？ 

    答案：21, 
    解析：arguments会和函数参数绑定。
    但如果es6付给初始值则无法修改
     function sidEffecting(ary) {
      ary[0] = ary[2];
    }
    function bar(a=1,b,c) {
      c = 10
      sidEffecting(arguments);
      return a + b + c;
    }
    bar(1,1,1)
    //12

#### 20.
    var a = 111111111111111110000,
    b = 1111;
    a + b;
这段代码的执行结果？  

    答案：11111111111111111000
    解析：在JavaScript中number类型在JavaScript中以64位（8byte）来存储。这64位中有符号位1位、指数位11位、实数位52位。2的53次方时，是最大值。其值为：9007199254740992（0x20000000000000）。超过这个值的话，运算的结果就会不对.
#### 21.
    var x = [].reverse;
    x();
 这段代码的执行结果？ 
    答案：window
    解析：但在chrome上执行报错，没太懂
#### 22.Number.MIN_VALUE > 0
    答案：true
    解析：MIN_VALUE 属性是 JavaScript 中可表示的最小的数（接近 0 ，但不是负数）。它的近似值为 5 x 10-324。
#### 23.[1 < 2 < 3, 3 < 2 < 1]
    答案：[true,true]
    解析： 1 < 2    =>  true;
          true < 3 =>  1 < 3 => true;
          
          3 < 2     => false;
          false < 1 => 0 < 1 => true;
#### 24.2 == [[[2]]]
    答案：true
    解析：值和引用类型去比较,把引用类型转话成值类型
         [[[2]]]）//2
#### 25.        
    3.toString()
    3..toString()
    3...toString()
 这段代码的执行结果？ 
    答案：error, "3", error
    解析：因为在 js 中 1.1, 1., .1 都是合法的数字. 那么在解析 3.toString 的时候这个 . 到底是属于这个数字还是函数调用呢? 只能是数字, 因为3.合法啊!
#### 26.
    (function(){
      var x = y = 1;
    })();
    console.log(y);
    console.log(x);
 这段代码的执行结果？ 
    答案：1, error
    解析：y 被赋值成全局变量，等价于
         y = 1 ;
         var x = y;
#### 27.
    var a = /123/,
    b = /123/;
    a == b
    a === b
 这段代码的执行结果？ 
    答案：error, error
    解析：正则是对象，引用类型，相等（==）和全等（===）都是比较引用地址
#### 28.          
    var a = [1, 2, 3],
    b = [1, 2, 3],
    c = [1, 2, 4]
    a ==  b
    a === b
    a >   c
    a <   c
 这段代码的执行结果？ 
    答案：false, false, false, true
    解析：相等（==）和全等（===）还是比较引用地址
         引用类型间比较大小是按照字典序比较，就是先比第一项谁大，相同再去比第二项。
#### 29.          
    var a = {}, b = Object.prototype;
    [a.prototype === b, Object.getPrototypeOf(a) === b]    
 这段代码的执行结果？ 
    答案：false, true
    解析：Object 的实例是 a，a上并没有prototype属性
         a的__poroto__ 指向的是Object.prototype，也就是Object.getPrototypeOf(a)。a的原型对象是b
#### 30.          
    function f() {}
    var a = f.prototype, b = Object.getPrototypeOf(f);
    a === b         
 这段代码的执行结果？ 
    答案：false
    解析：a是构造函数f的原型 ： {constructor: ƒ}
         b是实例f的原型对象 ： ƒ () { [native code] }
#### 31.          
    function foo() { }
    var oldName = foo.name;
    foo.name = "bar";
    [oldName, foo.name]     
 这段代码的执行结果？ 
    答案：["foo", "foo"]
    解析：函数的名字不可变.
#### 32."1 2 3".replace(/\d/g, parseInt)     
    答案："1 NaN 3"
    解析：replace() 回调函数的四个参数:
          1、匹配项  
          2、与模式中的子表达式匹配的字符串  
          3、出现的位置  
          4、stringObject 本身 。
    如果没有与子表达式匹配的项，第二参数为出现的位置.所以第一个参数是匹配项，第二个参数是位置
     parseInt('1', 0)
     parseInt('2', 2)  //2进制中不可能有2
     parseInt('3', 4)
    巩固：
       "And the %1".replace(/%([1-8])/g,function(match,a , b ,d){
          console.log(match +"  "+ a + " "+ b +" "+d )
        });
       //%1  1 8 And the %1 
#### 33.          
    function f() {}
    var parent = Object.getPrototypeOf(f);
    f.name // ?
    parent.name // ?
    typeof eval(f.name) // ?
    typeof eval(parent.name) //  ?  
 这段代码的执行结果？ 
    答案："f", "Empty", "function", error
    解析：f的函数名就是f
         parent是f原型对象的名字为"" ,
         先计算eval(f.name) 为 f,f的数据类型是function
         eval(parent.name) 为undefined, "undefined"
#### 34.          
    var lowerCaseOnly =  /^[a-z]+$/;
    lowerCaseOnly.test(null), lowerCaseOnly.test()]
 这段代码的执行结果？ 
    答案：[true, true]
    解析：这里 test 函数会将参数转为字符串. 'nul', 'undefined' 自然都是全小写了
#### 35.[,,,].join(",")
    答案：",,"
    解析：因为javascript 在定义数组的时候允许最后一个元素后跟一个,
         所以这个数组长度是3，
    巩固： [,,1,].join(".").length //  3 
#### 36.[,,,].join(",")
    答案：other
    解析：这取决于浏览器。类是一个保留字，但是它被Chrome、Firefox和Opera接受为属性名。在另一方面，每个人都会接受大多数其他保留词（int，私有，抛出等）作为变量名，而类是VordBoint。
#### 37.var a = new Date("epoch")
    答案：other
    解析：您得到“无效日期”，这是一个实际的日期对象（一个日期的日期为true）。但无效。这是因为时间内部保持为一个数字，在这种情况下，它是NA。
          在chrome上是undefined 
          正确的是格式是var d = new Date(year, month, day, hours, minutes, seconds, milliseconds);
#### 38.
    var a = Function.length,
    b = new Function().length
    a === b
这段代码的执行结果？ 
    
    答案：false
    解析：首先new在函数带（）时运算优先级和.一样所以从左向右执行
         new Function() 的函数长度为0
    巩固：function fn () {
             var a = 1;
          }
          console.log(fn.length) 
          //0 fn和new Function()一样
#### 39.
    var a = Date(0);
    var b = new Date(0);
    var c = new Date();
    [a === b, b === c, a === c]
这段代码的执行结果？ 
    
    答案：[false, false, false]
    解析：当日期被作为构造函数调用时，它返回一个相对于划时代的对象（JAN 01 1970）。当参数丢失时，它返回当前日期。当它作为函数调用时，它返回当前时间的字符串表示形式。
    a是字符串   a === b // 数据类型都不同，肯定是false
    b是对象     b === c // 引用类型，比的是引用地址
    c也是对象   a === c // 数据类型都不同，肯定是false
    巩固：  var a = Date(2018);
           var b = Date(2001);
           [a ===b ]
           //[true] Date() 方法获得当日的日期,作为函数调用不需要，返回的同一个字符串
           "Tue Jun 12 2018 14:36:24 GMT+0800 (CST)" 当然如果a,b执行时间相差1秒则为false
#### 20.
    var min = Math.min(), max = Math.max()
    min < max
这段代码的执行结果？ 
    
    答案：false
    解析： Math.min 不传参数返回 Infinity, Math.max 不传参数返回 -Infinity 
          无限集合之间不能比较大小。
    巩固：Number.MAX_VALUE  > Number.MIN_VALUE  //true
#### 41.
    function captureOne(re, str) {
      var match = re.exec(str);
      return match && match[1];
    }
    var numRe  = /num=(\d+)/ig,
        wordRe = /word=(\w+)/i,
        a1 = captureOne(numRe,  "num=1"),
        a2 = captureOne(wordRe, "word=1"),
        a3 = captureOne(numRe,  "NUM=2"),
        a4 = captureOne(wordRe,  "WORD=2");
    [a1 === a2, a3 === a4]
这段代码的执行结果？ 
    
    答案：[true, false]
    解析： ／g有一个属性叫lastIndex，每次匹配如果没有匹配到，它将重置为0，如果匹配到了，他将记录匹配的位置。我们看一个简单的例子吧。
            var numRe  = /num=(\d)/g;
             numRe.test("num=1abcwewe") //true
             numRe.lastIndex            //5     匹配到num=1后在5的索引位置
             numRe.exec("num=1")        //fales 这次要从5的索引位置，开始匹配
             numRe.lastIndex            //0     上一次匹配失败了numRe.lastIndex重制为0
#### 42.
    var a = new Date("2014-03-19"),
    b = new Date(2014, 03, 19);
    [a.getDay() === b.getDay(), a.getMonth() === b.getMonth()]
这段代码的执行结果？ 
    
    答案：[false, false]
    解析： var a = new Date("2014-03-19")    //能够识别这样的字符串，返回想要的日期
          Wed Mar 19 2014 08:00:00 GMT+0800 (CST)
          b = new Date(2014, 03, 19);       //参数要按照索引来
          Sat Apr 19 2014 00:00:00 GMT+0800 (CST)
          月是从0索引，日期是从1 
          getDay()是获取星期几
          getMonth()是获取月份所以都不同
    巩固： [a.getDate() === b.getDate()] //true
#### 43.
     if ('http://giftwrapped.com/picture.jpg'.match('.gif')) {
        'a gif file'
      } else {
        'not a gif file'
      }
这段代码的执行结果？ 
    
    答案：'a gif file'
    解析： String.prototype.match 接受一个正则, 如果不是, 按照 new RegExp(obj) 转化. 所以 . 并不会转义 。 那么 /gif 就匹配了 /.gif/
    巩固： if ('http://giftwrapped.com/picture.jpg'.indexOf('.gif')) {
            'a gif file'
          } else {
            'not a gif file'
          }
          //'a gif file' 同样的道理

#### 44.
    function foo(a) {
      var a;
      return a;
    }
    function bar(a) {
        var a = 'bye';
        return a;
    }
    [foo('hello'), bar('hello')]
        
这段代码的执行结果？ 
    
    答案：["hello", "bye"]
    解析：最后一题很简单吧，变量声明