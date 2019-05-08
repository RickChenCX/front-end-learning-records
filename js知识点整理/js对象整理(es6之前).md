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

* __继承__
    