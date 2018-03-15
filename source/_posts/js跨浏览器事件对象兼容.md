---
title: js跨浏览器事件对象兼容
date: 2018-2-28 10:36:15
tags:
---

##代码演示##

<!-- more -->

```
var EventUtil = {

   addHandler : function(element, type, handler) {
        if (element.addEventListener) {
            // FF、chrom、ie9以上
            element.addEventListener(type, handler, false);
        } else if (element.attachEvent) {
            // ie8 以下浏览器
            element.attachEvent("on" + type, handler);
        } else {
            element["on" + type] = handler;
        }
    },

   removeHandler : function(element, type, handler) {
        if (element.removeEventListener) {
             element.removeEventListener(type, handler, false);
        } else if (element.detachEvent) {
             element.detachEvent("on" + type, handler);
        } else {
            element["on" + type] = null;
        }
    },

   getEvent : function(event) {
        return event ? event : window.event;
    },

   getTarget : function(event) {
        return event.target || event.srcElement;
    },

   preventDefault : function(event) {
        if (event.preventDefault) {
            event.preventDefault();
        } else {
            event.returnValue = false;
        }
    },

   stopPropagation : function(event) {
        if (event.stopPropagation) {
            event.stopPropagation();
        } else {
            event.cancelBubble = true;
        }
    }
    
};
```


## 其他兼容问题 ##

 - 封装一个获取行外样式的函数:(兼容所有浏览器,包括低版本IE6,7)
```
funtion getStyle(obj,name){
    if(obj.currentStyle){
        //IE
        return obj.currentStyle[name];
    }else{
        //Chrom,FF
        return getComputedStyle(obj,false)[name];
    }
}
```
*调用：getStyle(oDiv,'width');*

 - 关于用“索引”获取字符串每一项出现的兼容性问题:
```
    var str="abcde";
    aletr(str[1]);      //IE6,7不兼容
    兼容方法:str.charAt(i)    //全部浏览器都兼容
    var str="abcde";
    for(var i=0;i<str.length;i++){
      alert(str.charAt(i));   //放回字符串中的每一项
    }
```

 - 关于DOM中 childNodes 获取子节点出现的兼容性问题:

   childNodes:获取子节点,
    &nbsp; &nbsp; --IE6-8:获取的是元素节点,正常
    &nbsp; &nbsp; --高版本浏览器:但是会包含文本节点和元素节点(不正常)
  解决方法: 使用nodeType:节点的类型，并作出判断
    &nbsp; &nbsp; --nodeType=3-->文本节点
    &nbsp; &nbsp; --nodeTyPE=1-->元素节点

 *例: 获取ul里所有的子节点，让所有的子节点背景色变成红色*
 ```
  获取元素子节点兼容的方法：
  var oUl=document.getElementById('ul');
  for(var i=0;i<oUl.childNodes.length;i++){
    if(oUl.childNodes[i].nodeType==1){
      oUl.childNodes[i].style.background='red';
    }
  }
 ```
 ```
  注：上面childNodes为我们带来的困扰完全可以有children属性来代替。children属性:只获取元素节点,不获取文本节点,兼容所有的浏览器
      var oUl=document.getElementById('ul');
      oUl.children.style.background="red";
 ```

 - 关于使用 firstChild,lastChild 等，获取第一个/最后一个元素节点时产生的问题:
 ```
 兼容写法: 找到ul的第一个元素节点,并将其背景色变成红色
    var oUl=document.getElementById('ul');
    if(oUl.firstElementChild){
      //高版本浏览器
      oUl.firstElementChild.style.background='red';
    }else{
      //IE6-8
      oUl.firstChild.style.background='red';
    }
 ```

 - 关于获取滚动条距离而出现的问题：
 >   IE,Chrome: document.body.scrollTop
 >   FF: document.documentElement.scrollTop

 ```
    兼容处理:
    var scrollTop=document.documentElement.scrollTop||document.body.scrollTop
 ```
 
 - ajax兼容问题：
 >  IE：new ActiveXObject() 　
 >  FF、Chrome：new XMLHttpRequest()

 ```
解决办法：统一封装创建createXMLHttpRequest对象的方法
function createXMLHttpRequest(){  
    var XML;  
    try{  
        XML = new XMLHttpRequest();  
    }catch(e){  
        try{  
            xmlHttp = new ActiveXObject("Msxml2.XMLHTTP");
        }catch(e){  
            try{  
                xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");  
            }catch(e){  
                alert("提示: 您的浏览器暂时不支持Ajax请求!");  
                return false;  
            }  
        }  
    }  
    return xmlHttp;  
}
 ```
