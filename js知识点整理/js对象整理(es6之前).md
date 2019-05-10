#### javascript对象
    定义：对象是JavaScript的一个基本数据类型，是一种复合值，它将很多值（原始值或者其他对象）聚合在一起，可通过名字访问这些值。即属性的无序集合。
    每一个属性都是一个名值对，属性名是字符串，因此我们可以把对象看作是字符串到值的映射。

* 内置对象：由ECMASCRIPT规范定义的对象或类，例如：数组，函数，日期和正则表达式都是内置对象
* 宿主对象：js解释器嵌入的宿主环境（如web浏览器）定义的，宿主环境定义的方法可以当成普通的js函数对象，所以，宿主对象也可以当成内置对象
* 自定义对象：在运行的js代码中定义的对象。
* 自有属性：直接在对象中定义的属性。
* 继承属性：是在对象的原型对象中定义的属性。

**创建对象**
1. 通过对象直接量
    ```  javascript
        // 空对象
        let obj = {};

        //含有属性的对象
        let obj1 = {
            name: "js",
            age: "18"
        };
    ```
    >注意： 在函数循环体内使用表达式创建对象，将会创建很多新对象，并且值有可能不用。
2. 关键字new
    ``` javascript
        let o = new Object(); // 创建一个空对象，和{}一样
        let arr = new Array(); // 创建一个空数组， 和[]一样
        let d = new Date(); // 创建一个表示当前时间的Date对象
        let r = new RegExp("js"); // 创建一个可以进行模式匹配的regExp对象
    ```
3. Object.create()函数
    * 参数一：对象的原型
    * 参数二（可选）：用以对对象的属性进行进一步描述

    ``` javascript
        let o1 = Object.create({x:1, y:2});
        // o1继承了属性x,y

        let o2 = Object.create(null);
        // 创建一个原型为null的对象，这样创建的对象不会继承任何方法，
        // 甚至是基础方法，比如toString()

        let o3 = Object.create(Object.prototype);
        // o3 和{} ，new Object 一样
    ```

* __属性查询和设置__
    可以通过"."或[]运算符来获取属性的值，也可以创建属性或给属性赋值。
    ``` javascript
        let obj = {
            name: "john",
            age: "12"
        }
        // 获取属性
        let name = obj.name;
        let age = obj["age"];

        // 设置属性或创建属性
        obj.address = "";
        obj["name"] = "mars";
    ```
    > "."和[]使用上是有差别的，点运算符后面的标识符是静态的，是写死在程序里的，而方括号内的表达式必须返回字符串，字符串值是动态的，可以在运行时更改。
* __删除属性__
    delete运算符，delete运算符只能删除自有属性，不能删除继承属性（要删除继承属性必须从定义这个属性的原型对象上删除它，而且这会影响到所有继承自这个原型的对象）。
    ``` javascript
        // delete非严格模式下删除全局对象的可配置属性
        this.x = 1; 
        delete x; // 将它删除

        // 严格模式下
        delete x; // 在严格模式下语法报错
        delete this.x // 正常

    ```
    > delete 表达式删除成功时，或没有任何副作用时（比如删除不存在的属性），它返回true，如果delete后不是一个属性访问表达式，delete同样返回true。
    ``` javascript
        o = {x:1};
        delete o.x; //删除x,返回true
        delete o.x; // 什么也没做，x已经删除，返回true
        delete o.toString; // 什么也没做，toString是继承来的，返回true
        delete 1; // 无意义，返回true
    ```
    > delete不能删除那些可配置性为false的属性

* __检测属性__
    1. in运算符
    2. hasOwnProperty()
    3. propertyIsEnumerable()
    ``` javascript
        // in运算符
        let o = { x:1 };
        "x" in o; // true
        "y" in o  // false
        "toString" in o // true ,toString是继承来的

        // hasOwnProperty()检测自有属性，继承属性返回false
        o.hasOwnProperty("x"); // true
        o.hasOwnProperty("y"); // false
        o.hasOwnProperty("toString"); // false

        // propertyIsEnumerable() 检测是否是自有属性，且可枚举
        // 继承来的属性返回false
        o.propertyIsEnumerable("x") // true , 自有，且可枚举
        Object.prototype.propertyIsEnumerable("toString") // false,不可枚举

        // "!==" 判断属性是否是undefined
        o.x !== undefined; // true 
        o.y !== undefined; // false
        o.toString !== undefined // true
    ```
    > in运算符可以区分不存在的属性和存在但是值为undefined的属性，而"!=="检测值为undefined的属性时，返回false。

* __枚举属性__
    1. for/in
    2. Object.keys():返回对象中可枚举的自有属性的名称的数组
    3. Object.getOwnPropertyNames():返回对象的所有自有属性的名称

* __getter/setter__
    存取器属性。
    属性同时具有setter/getter,是一个可读/写属性
    属性只有getter，是一个只读属性
    属性只有setter,是一个只写属性，读取只写属性，总是返回null
    1. getter:查询存取器属性的值
    2. setter:设置存取器属性的值
    ``` javascript
        let o = {
            x:1,
            y:2,
            get r() {
                return x+y;
            },
            set r(newVal) {
                this.x += newVal;
                this.y += newVal;
            }
        }
    ```

* __属性的特性__
    1. 数据属性：值（value），可写性（writable），可枚举性（enumerable)，可配置性（configurable）
    2. 存取器属性：读取（get），写入（set），可枚举性（enumerable)，可配置性（configurable）

    调用 **Object.getOwnPropertyDescriptor()** 可以获得某个对象特定属性的属性描述符。
    ``` javascript
        Object.getOwnPropertyDescriptor({x:1}, "x");
        // {value:1, writable:true, enumerable:true, configurable:true}
        // 对于继承属性和不存在的属性，返回undefined
    ```
    **Object.defineProperty()** 设置属性的特性，或者让新建的属性具有某种特性
    ``` javascript
        let o = {};
        // 添加了一个不可枚举的，值为1的属性
        Object.defineProperty(o, "x", {
            value:1,
            writable:true, 
            enumerable:false, 
            configurable:true
        })
        // 数据属性改为存取器属性
        Object.defineProperty(o, "x", {
            get: function() { return 0; };
        })
        o.x // 0
    ```
    > 这个方法修改已有属性和自有属性，不能修改继承属性

    **Object.defineProperties()**
    同时修改多个属性或创建属性
    第一个参数是要修改的对象，第二个参数是包含要修改和新建属性的名称，属性描述符的映射表。
    ``` javascript
        //给一个空对象添加两个数据属性，和一个存取起属性
        let p = Object.defineProperties({}, {
            x:{
                value:1,
                writable:true, 
                enumerable:true, 
                configurable:true
            },
            y:{
                value:1,
                writable:true, 
                enumerable:true, 
                configurable:true
            },
            z:{
                get:function() {
                    return this.x + this.y;
                },
                enumerable:true, 
                configurable:true
            },
        })
    ```
    > Object.defineProperty()和 Object.defineProperties()使用规则：
       1. 如果对象不可拓展，只能修改已有的自有属性，不能新创建属性。
       2. 如果对象不可配置，则不能修改他的可配置型和可枚举性。
       3. 如果存取器属性不可配置，则不能修改setter和getter，也不能将它转为数据属性。
       4. 如果数据属性不可配置，则不能转为存取器属性。
       5. 如果数据属性不可配置，则不能将它的可写性从false修改为true，但是可以从true修改为false.
       6. 如果数据属性是不可写且不可配置的，则不能修改它的值，可配置不可写的属性的值是可以修改的。
    
##### 对象的三个属性：原型(prototype),类(class)和可拓展性(extensible attribute)

* __原型属性__
    

