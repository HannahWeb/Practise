# practise

filter()基本语法：

arr.filter(callback[, thisArg])
-----------------
实例
function isBigEnough(element) {
  return element >= 10;
}
var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
// filtered is [12, 130, 44]
document.write(filtered);
----------------

filter 被添加到 ECMA-262 标准第 5 版中，兼容旧环境 如IE8

if (!Array.prototype.filter)
{
  Array.prototype.filter = function(fun /*, thisArg */)
  {
    "use strict";

    if (this === void 0 || this === null)
      throw new TypeError();

    var t = Object(this);
    var len = t.length >>> 0;
    if (typeof fun !== "function")
      throw new TypeError();

    var res = [];
    var thisArg = arguments.length >= 2 ? arguments[1] : void 0;
    for (var i = 0; i < len; i++)
    {
      if (i in t)
      {
        var val = t[i];

        // NOTE: Technically this should Object.defineProperty at
        //       the next index, as push can be affected by
        //       properties on Object.prototype and Array.prototype.
        //       But that method's new, and collisions should be
        //       rare, so use the more-compatible alternative.
        if (fun.call(thisArg, val, i, t))
          res.push(val);
      }
    }

    return res;
  };
}


=======================================================================================


forEach()基本语法：
array.forEach(callback[, thisObject]);

forEach()被添加到 ECMA-262 标准第 5 版中，兼容旧环境

if (!Array.prototype.forEach)
{
 Array.prototype.forEach = function(fun /*, thisp*/)
 {
  var len = this.length;
  if (typeof fun != "function")
   throw new TypeError();
 
  var thisp = arguments[1];
  for (var i = 0; i < len; i++)
  {
   if (i in this)
    fun.call(thisp, this[i], i, this);
  }
 };
}
 
function printBr(element, index, array) {
 document.write("<br />[" + index + "] is " + element +"this:"+this.arr); 
}
 
obj.arr.forEach(printBr);






Object.seal(O) / Object.isSealed

方法用于把对象密封，也就是让对象既不可以拓展也不可以删除属性（把每个属性的configurable设为false）,
属性值仍然可以修改，Object.isSealed由于判断对象是否被密封

Object.seal(o);
  o.age = 25; //仍然可以修改
  delete o.age; //Cannot delete property 'age' of #<Object>


Object.freeze(O) / Object.isFrozen

终极神器，完全冻结对象，在seal的基础上，属性值也不可以修改（每个属性的wirtable也被设为false）

Object.freeze(o);
        o.age = 25; //Cannot assign to read only property 'age' of #<Object>
================================================================================================


js的数组迭代器函数map和filter，可以遍历数组时产生新的数组，和python的map函数很类似
1）filter是满足条件的留下，是对原数组的过滤；
2）map则是对原数组的加工，映射成一一映射的新数组
var xx = [1, 2, 5, 7];
function pp(x){return x % 2;}
function px(x){return x % 2;}
var m = xx.map(pp);
console.log("m = " + m);
var f = xx.filter(px);
console.log("f = " + f);
～～～～～～～～～～～～～～～～～～～～～～～～～～～
m = 1,0,1,1
f = 1,5,7










自己的库

var tobeUnique=function(val,index,arr){
  return arr.indexOf(val)===index;//重复的词只有第一个返回真
}
var arrUnique= function(arr){
  return arr.filter(tobeUnique);

}
var Yan=Object.freeze({
  arrUnique:arrUnique,
  containedWords: function(dict, source) {
            var regWord = [];
            var regR = new RegExp("([+&.()])", 'g');
            $.each(dict, function (i, val) {
                regWord[i] = val.replace(regR, "\\\$1");//加转义字符标识
            });

            if (regWord.length) {
                var reg = new RegExp('(' + regWord.join('|') + ')', 'g');
                //得到的违禁词
                console.log(reg);
                var contains = source.match(reg) || [];
                return Yan.arrUnique(contains);
            } else {
                return [];
            }
        }
})

//Yan.containedWords(['s','&','fdf'],"ss,s,s&,fdf2983r");
// /(s|\&|fdf)/g
//["s", "&", "fdf"]
