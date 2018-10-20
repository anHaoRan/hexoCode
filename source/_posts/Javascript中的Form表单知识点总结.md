---
title: Javascript中的Form表单知识点总结
date: 2017-04-24 18:52:49
tags: HTML
toc: true
---
转载(http://www.cnblogs.com/tugenhua0707/p/4508986.html)
 在HTML中，表单是由form元素来表示的，但是在javascript中，表单则由HTMLFormElement类型，此元素继承了HTMLElement，因此与其他HTML元素具有相同的默认属性；HTMLFormElement有自己以下属性和方法；

acceptCharset: 服务器能够处理的字符集；等价于HTML中的accept-charset特性；

action:  接收请求的URL，等价于HTML中的action

elements: 表单中所有控件的集合.

enctype: 请求的编码类型；等价于HTML中的enctype特性；
<!--more-->
length: 表单中控件的数量；

method： 要发送的http请求类型，一般是get或者是post，等价于HTML中的method；

name: 表单的名称；

reset(): 将所有表单域重置为默认值；

submit(): 提交表单；

target:用于发送请求和接收响应的窗口名称；

如何获取form表单的引用？

假如现在页面上有一个form表单元素，html代码如下：

<form id="form" name="form1"></form>
我现在想取到上面的form表单的引用，一共有以下方式可以获取到上面 的form表单引用；

1. 通过获取form表单的id，来获取form表单的引用；如下代码：

var formId = document.getElementById("form");
console.log(formId);
2. 通过document.forms 取得页面中的所有表单元素，然后通过索引来取到对应的form元素，如下代码所示：取得页面第一个form元素；

console.log(document.forms[0]);
3. 通过from表单中的name属性来获取，代码如下：

console.log(document.forms['form1']);
如何提交表单

下面的所有事件都是来自上一篇博客javascript事件总结的事件，都依赖于此封装的事件，代码如下：

复制代码
var EventUtil = {
    addHandler: function(element,type,handler) {
        if(element.addEventListener) {
            element.addEventListener(type,handler,false);
        }else if(element.attachEvent) {
            element.attachEvent("on"+type,handler);
        }else {
            element["on" +type] = handler;
        }
    },
    removeHandler: function(element,type,handler){
        if(element.removeEventListener) {
            element.removeEventListener(type,handler,false);
        }else if(element.detachEvent) {
            element.detachEvent("on"+type,handler);
        }else {
            element["on" +type] = null;
        }
    },
    getEvent: function(event) {
        return event ? event : window.event;
    },
    getTarget: function(event) {
        return event.target || event.srcElement;
    },
    preventDefault: function(event){
        if(event.preventDefault) {
            event.preventDefault();
        }else {
            event.returnValue = false;
        }
    },
    stopPropagation: function(event) {
        if(event.stopPropagation) {
            event.stopPropagation();
        }else {
            event.cancelBubble = true;
        }
    },
    getRelatedTarget: function(event){
        if (event.relatedTarget){
            return event.relatedTarget;
        } else if (event.toElement){
            return event.toElement;
        } else if (event.fromElement){
            return event.fromElement;
        } else {
            return null;
        }
    },
    getWheelDelta: function(event) {
        if(event.wheelDelta) {
            return event.wheelDelta;
        }else {
            return -event.detail * 40
        }
    },
    getCharCode: function(event) {
        if(typeof event.charCode == 'number') {
            return event.charCode;
        }else {
            return event.keyCode;
        }
    }
};
复制代码
用户单击提交按钮或图像按钮时，就会提交表单，使用input或者button都可以提交表单，只需将type设置为submit或者image即可，如下三种方式都可以；

第一种：

<form id="form" name="form1" action="http://www.baidu.com">
    <!-- 存放一个input放在这，为了获取焦点，然后我们可以按enter键提交 -->
    <input type="text">
    <input type="submit" value="submit">
</form>
第二种：

<form id="form" name="form1" action="http://www.baidu.com">
    <!-- 存放一个input放在这，为了获取焦点，然后我们可以按enter键提交 -->
    <input type="text">
    <button type="submit">submit</button>
</form>
第三种：

<form id="form" name="form1" action="http://www.baidu.com">
    <!-- 存放一个input放在这，为了获取焦点，然后我们可以按enter键提交 -->
    <input type="text">
    <input type="image" src="aa.jpg">
</form>
我们也可以通过如下方式提交表单，但是也可以阻止form表单提交：如下代码：

复制代码
EventUtil.addHandler(formId,"submit",function(event){
    // 取得事件对象
    event = EventUtil.getEvent(event);
    // 阻止默认事件
    EventUtil.preventDefault(event);
});
复制代码
如何重置表单

如果我们使用按钮重置表单的话，有下面2种方式：

第一种代码如下：

<form id="form" name="form1" action="http://www.baidu.com">
    <input type="text">
    <input type="reset" value="reset">
</form>
第二种代码如下：

<form id="form" name="form1" action="http://www.baidu.com">
    <input type="text">
    <button type="reset">reset</button>
</form>
我们也可以通过像提交form表单一样来进行重置表单，代码如下：

var formId = document.getElementById("form");
formId.reset();
如何访问表单字段？

第一种方式我们可以使用dom节点来访问；

第二种方式：每个表单都有elements属性，该属性是表单中所有表单元素的集合；这个elements是个有序列表；包含着所有字段，比如有input,textarea,button,fieldset等；

比如如下HTML代码：

复制代码
<form id="form" name="form1" action="http://www.baidu.com">
    <input type="text" name="input1">
    <select name="select1">
        <option>111</option>
    </select>
</form>
复制代码
JS获取表单字段如下：

复制代码
var formId = document.getElementById("form");
// 取得表单中的第一个字段
var firstCol = formId.elements[0];
console.log(firstCol);
// 取得名字name为select1的字段
console.log(formId.elements['select1']);
// 取得表单中包含字段的数量
console.log(formId.elements.length);
复制代码
如果一个表单中，有多个name相同的属性，那么取得数据是一个集合，如下HTML代码：

<form id="form" name="form1" action="http://www.baidu.com">
    <input type="radio" name="radio2"/>
    <input type="radio" name="radio2"/>
    <input type="radio" name="radio2"/>
</form>
JS代码如下：

var formId = document.getElementById("form");
var radios = formId.elements["radio2"];
console.log(radios.length);  // 打印3
共有的表单字段属性

所有的表单字段都有一组相同的属性；表单共有的属性如下：

disabled: 布尔值，表示当前字段是否被禁用；

form: 指向当前字段所属表单的指针，只读；

name: 当前字段的名称;

readOnly:布尔值，表示当前字段是否可读。

tabIndex: 表示当前字段的切换(tab)序号。

type: 当前字段的类型，如checkbox,radio等；

value: 当前字段被提交到服务器的值；

共有的表单字段方法

每个表单字段都有两个方法focus()和blur()，其中focus是获取焦点；比如在页面加载完成后，我希望form表单中的第一个字段获取焦点（除隐藏域之外）；如下代码：

<form id="form" name="form1" action="http://www.baidu.com">
    <input type="text" name="radio2"/>
    <input type="text" name="radio2"/>
    <input type="text" name="radio2"/>
</form>
JS代码如下：

var formId = document.getElementById("form");
EventUtil.addHandler(window,'load',function(event){
    formId.elements[0].focus();
});
但是HTML5中为表单字段新增了一个autofocus属性，在支持这个属性浏览器中，如果设置了这个属性，不用javascript就能将焦点移动到某个输入框下，比如如下HTML代码，在页面加载完成后，我把焦点放在第二个输入框内，如下HTML代码：

<form id="form" name="form1" action="http://www.baidu.com">
    <input type="text" name="radio2" />
    <input type="text" name="radio2" autofocus/>
    <input type="text" name="radio2"/>
</form>
支持autofocus属性的浏览器有：firefox4+，safari5+，chrome和Opera9.6+

但是我想要兼容其他不支持autofocus的浏览器，我们可以写一段JS，为了全兼容；

复制代码
var formId = document.getElementById("form");
EventUtil.addHandler(window,'load',function(event){
    var element = formId.elements[1];
    if(element.autofocus !== true) {
        element.focus();
    }
});
复制代码
因为autofocus是一个布尔值，支持他的浏览器默认为true；不支持他的浏览器，默认值为空字符串；

共有的表单字段事件

所有的表单字段都支持以下三个事件；

blur:当前字段失去焦点时触发；

change:对于input和textarea元素，值发生改变的时候触发；

focus: 当前字段获得焦点时触发；

理解文本框脚本

在HTML中，有2种方式来实现文本框，一种是input元素的单行文本框，另一种是textarea元素的多行文本框；

input元素有属性type=”text”, 还可以通过设置size属性，用来指定文本框显示的字符数，还可以设置value，用来显示文本框的初始值，还可以设置maxlength属性，用于指定文本框可以接受的最大字符数；如下代码：

<input type="text" size="2" maxlength='12' value=""/>

多行文本框textarea也有一些属性，这里就不做多介绍了；

如何选择文本：

input和select两种元素都支持select()方法，这个方法用于选择文本框中的所有文本，在调用select()方法中(除Opera外),都会将焦点设置到文本框中，这个方法不接受任何参数，如下代码：

<form id="formId">
    <input type="text" name="input" value="我是龙恩"/>
</form>
JS代码如下：

var formId = document.getElementById("formId");
formId.elements['input'].select();
如下图所示：



如上是页面一进来的时候，默认选择input元素框所有的内容；我们也可以当获取焦点的时候，就选中所有的内容，JS代码可以改为如下：

复制代码
var formId = document.getElementById("formId"),
input = formId.elements['input'];
EventUtil.addHandler(input,"focus",function(event){
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
    target.select();
});
复制代码
在火狐和谷歌浏览器下能实现当获取焦点的时候，就选中input元素框内的所有内容，但是在IE7或者8下，还是页面加载完后就已经选中了文本框内的所有元素；

  1.   选择事件(select)

与select方法对应的，还有一个select事件，在IE9+，firefox，chrome，opera和safari中，只有用户选择了文本且释放鼠标时，会触发select事件；但是在IE8及以下，只要用户选择了一个字母且不必释放鼠标，就会触发select事件；如下代码：

var formId = document.getElementById("formId"),
input = formId.elements['input'];
EventUtil.addHandler(input,"select",function(event){
    alert(input.value);
});
2. 取的选择的文本

虽然通过select事件我们知道用户什么时候选择了文本，但是我们并不知道用户选择了什么文本，在HTML5中，我们通过两个属性selectionStart和selectionEnd，这两个属性表示选择的范围(即文本区开头和结尾的偏移量)；因此要取得用户选择的文本，可以使用如下代码；

复制代码
var formId = document.getElementById("formId"),
    input = formId.elements['input'];
EventUtil.addHandler(input,"select",function(event){
    alert(getSelectedText(input));
});
function getSelectedText(elem) {
    return elem.value.substring(elem.selectionStart,elem.selectionEnd);
}
复制代码
但是目前浏览器支持程度有：IE9+，firefox，chrome，Opera及safari；

IE8及之前的版本不支持这两个属性，但是他们提供了另外一种document.selection对象，其中保存着用户在整个文档范围内选择的文本信息，但是呢与前面的select事件使用在一起的话，只能选择一个字符就会触发事件，也就是说，不能选择大于1和字符的文字，不过可以知道选择的值时多少；如下代码：

复制代码
var formId = document.getElementById("formId"),
input = formId.elements['input'];
EventUtil.addHandler(input,"select",function(event){
    alert(getSelectedText(input));
});
function getSelectedText(elem) {
    if(typeof elem.selectionStart == "number") {
         return elem.value.substring(elem.selectionStart,elem.selectionEnd);
    }else if(document.selection) {
        return document.selection.createRange().text;
    }
}
复制代码
选择部分文本

HTML5也为选择文本框中的部分文本提供了解决方案，使用setSelectionRange()方法，这个方法接收2个参数，要选择的第一个字符的索引，和要选择的最后一个字符之后的字符的索引；

浏览器支持有：IE9+，chrome，safari和opera，firefox貌似不支持；

代码如下：

var formId = document.getElementById("formId"),
    input = formId.elements['input'];
input.value = "我是龙恩，我是中国人";
// 选择所有文本
input.setSelectionRange(0,input.value.length);
截图如下：



// 选择前3个字符

input.setSelectionRange(0,3);
截图如下：



// 选择第四到第六个字符

input.setSelectionRange(4,7);

截图如下：



IE8及以下版本可以使用范围来选择部分文本，要选择部分文本，必须首先使用IE在所有文本框中提供的createTextRange()方法创建一个范围，且我们需要使用collapse()将范围折叠到文本框的开始位置，再使用moveStart()和moveEnd()这两个范围方法将范围移动到位；

如下代码选择所有的文本：

复制代码
input.value = "我是龙恩，我是中国人";
var range = input.createTextRange();
// 选择所有文本
range.collapse(true);
range.moveStart("character",0);
range.moveEnd("character",input.value.length);
range.select();
复制代码
演示如下：



切记：使用F5刷新没有用的，要在地址栏中，然后按enter键刷新即可看到效果；

// 选择前3个字符
range.collapse(true);
range.moveStart("character",0);
range.moveEnd("character",3);
range.select();
演示如下：



// 选择第4到第6个字符
range.collapse(true);
range.moveStart("character",4);
range.moveEnd("character",3);
range.select();
演示如下：



为了让跨浏览器效果，我们可以封装一个方法，如下：

复制代码
function selectText(elem,startIndex,stopIndex) {
    if(elem.setSelectionRange) {
          elem.setSelectionRange(startIndex,stopIndex);
    }else if(elem.createTextRange) {
        var range = elem.createTextRange();
        range.collapse(true);
        range.moveStart("character",startIndex);
        range.moveEnd("character",stopIndex - startIndex);
        range.select();
    }
}
复制代码
测试数据如下：貌似firefox不支持

复制代码
// 选择所有文本
selectText(input,0,input.value.length);
        
// 选择前3个字符
selectText(input,0,3);

// 选择第四个字符到第六个字符
selectText(input,4,7);
复制代码
过滤输入

    有时候我们会要求用户在输入框里面输入特定格式的数据，我们就可以使用过滤输入这种手段来进行了，首先我们来看看如何屏蔽字符；

 1.   屏蔽字符

比如我在一个input输入框中，只允许只能输入数字，那么我们可以先获取通过keypress事件来监听，然后每次获取到键码，然后通过String.fromCharCode()这个方法，把键码转换成字符串，然后通过正则判断下，如果不是数字，直接阻止默认事件即可不让用户输入，如下代码：

<form id="formId">
    <input type="text" name="input"/>
</form>
JS代码如下：

复制代码
var formId = document.getElementById("formId"),
input = formId.elements['input'];    
EventUtil.addHandler(input,'keypress',function(event) {
    event = EventUtil.getEvent(event);
    var charCode = EventUtil.getCharCode(event);
    if(!/\d/.test(String.fromCharCode(charCode))) {
        EventUtil.preventDefault(event);
    }
});    
复制代码
如上代码能满足日常需求，但是有些游览器，比如firefox，safari(3.1版本之前)会对向上键，向下键，退格键和删除键也会触发keypress事件了；所以为了避免这些事件的发生，我们需要做一些处理来满足所有版本的浏览器的需求，我们发现在firefox中，所有由非字符键触发keypress键码都为0；而在safari3以前的版本中，对应的字符编码全部为8；所以我们要对字符编码进行判断下；

如下代码：

复制代码
EventUtil.addHandler(input,'keypress',function(event) {
    event = EventUtil.getEvent(event);
    var charCode = EventUtil.getCharCode(event);
    if(!/\d/.test(String.fromCharCode(charCode)) && charCode > 9) {
        EventUtil.preventDefault(event);
    }
});
复制代码
操作剪贴板

到目前为止，IE，chrome，safari，opera都支持剪贴板事件，貌似firefox就不支持了（书上说支持）；但是我操作就不支持了；下面是6个操作剪贴板事件；如下：

beforecopy: 在发生复制操作前触发；

copy: 在发生复制操作时触发；

beforecut: 在发生剪贴操作前触发；

cut: 在发生剪贴操作时触发；

beforepaste: 在发生黏贴操作前触发；

paste: 在发生粘帖操作时触发；

针对上面的事件，我们可以使用如下代码测试下就可以证明了；代码如下所示：

EventUtil.addHandler(input,'beforecopy',function(event) {
    alert(1);
});
如果要访问剪贴板中的数据，可以使用clipboardData对象，在IE中，这个对象是window对象的属性，在safari或者chrome上，这个对象是event的属性，这个clipboardData对象有三个方法，getData(),setData(),和clearData();

getData()是从剪贴板中取得数据，他接受一个参数，即要取得数据的格式，在IE中，有二种数据格式”text” 和 “url”，在safari和chrome中这个参数是一种MIME类型，不过，可以使用text代表text/plain.

setData()方法是给剪贴板设置文本，接受2个参数，第一个数据是数据类型；第二个参数是放在剪贴板中的文本；但是此方法接受的数据类型只能是text/plain,不能是text；因此为了全兼容浏览器(出firefox外)，我们可以写一个通用的方法出来，如下：

复制代码
getClipboardText: function(event) {
    var clipboardData = (event.clipboardData || window.clipboardData);
        return clipboardData.getData("text");
    },
setClipboardText:function(event,value) {
    if(event.clipboardData) {
        return event.clipboardData.setData("text/plain",value);
    }else if(window.clipboardData) {
        return window.clipboardData.setData("text",value);
    }
}
复制代码
因此EventUtil封装的所有方法如下：

复制代码
var EventUtil = {
    addHandler: function(element,type,handler) {
        if(element.addEventListener) {
                    element.addEventListener(type,handler,false);
        }else if(element.attachEvent) {
            element.attachEvent("on"+type,handler);
        }else {
            element["on" +type] = handler;
        }
    },
    removeHandler: function(element,type,handler){
        if(element.removeEventListener) {
                    element.removeEventListener(type,handler,false);
        }else if(element.detachEvent) {
            element.detachEvent("on"+type,handler);
        }else {
            element["on" +type] = null;
        }
    },
    getEvent: function(event) {
        return event ? event : window.event;
    },
    getTarget: function(event) {
        return event.target || event.srcElement;
    },
    preventDefault: function(event){
        if(event.preventDefault) {
            event.preventDefault();
        }else {
            event.returnValue = false;
        }
    },
    stopPropagation: function(event) {
        if(event.stopPropagation) {
            event.stopPropagation();
        }else {
            event.cancelBubble = true;
        }
    },
    getRelatedTarget: function(event){
        if (event.relatedTarget){
            return event.relatedTarget;
        } else if (event.toElement){
            return event.toElement;
        } else if (event.fromElement){
            return event.fromElement;
        } else {
            return null;
        }
    },
    getWheelDelta: function(event) {
        if(event.wheelDelta) {
            return event.wheelDelta;
        }else {
            return -event.detail * 40
        }
    },
    getCharCode: function(event) {
        if(typeof event.charCode == 'number') {
            return event.charCode;
        }else {
            return event.keyCode;
        }
    },
    getClipboardText: function(event) {
        var clipboardData = (event.clipboardData || window.clipboardData);
        return clipboardData.getData("text");
    },
    setClipboardText:function(event,value) {
        if(event.clipboardData) {
        return event.clipboardData.setData("text/plain",value);
            }else if(window.clipboardData) {
            return window.clipboardData.setData("text",value);
        }
    }
};
复制代码
测试代码如下：还是上面测试输入框的值是否为数字；每次粘帖上次，都能获取到黏贴的是文字数据，代码如下：

HTML代码如下：

<form id="formId">
     <input type="text" name="input"/>
</form>
JS代码如下：

复制代码
var formId = document.getElementById("formId"),
input = formId.elements['input'];
EventUtil.addHandler(input,'paste',function(event) {
    event = EventUtil.getEvent(event);
    var text = EventUtil.getClipboardText(event);
    alert(text);
    if (!/^\d*$/.test(text)){
        EventUtil.preventDefault(event);
    }
});
复制代码
理解自动切换输入框或者textarea的焦点

比如我们在填写表单的页面上，当用户输入完自己的数据的时候，不需要用户手动切换到下一个输入框里面去，我们可以自动切换去，这样的话，对于用户体验来说，比较方便，比如我们现在页面上有一个form表单，这里为了做测试，我们先用一个输入框用于手机号码的，另外一个是textarea，当手机号码输入11位数字后，会自动切换到textarea中；当然页面中的隐藏域除外；代码如下：

HTML代码如下：

<form id="formId">
    <input type="text" name="input" maxlength=11/>
    <textarea></textarea>
</form>
JS代码如下：

复制代码
var formId = document.getElementById("formId"),
input = formId.elements['input'];
EventUtil.addHandler(input,'keyup',tabForward);
function tabForward(event) {
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
    if(target.value.length == target.maxLength) {
        // 获取当前的form表单的引用
        var form = target.form;
        for(var i = 0, ilen = form.elements.length; i < ilen; i++) {
            if(form.elements[i] == target) {
                if(form.elements[i+1]) {
                    form.elements[i+1].focus();
                }
                return;
            }
        }        
    }
}
复制代码
理解HTML5新增属性

required属性；

比如在HTML5中对表单input，textarea，或者select标签的话，提交表单时，需要判断是否为空，特别对于在做移动端的朋友来说，可以使用HTML5中的新增属性required；如下HTML代码：

<form id="formId">
    <input type="text" name="input" maxlength=11 required/>
    <textarea></textarea>
    <input type="submit"/>
</form>
提交时候，在chrome下看到效果如下：



在firefox下，提示如下：



如上是根据不同的浏览器本身的性质来提示的，因此样式不同，所以适合在移动端根据本身浏览器内核来提示；

但是在Javascript是如何判断的呢？比如如下HTML代码：

<form id="formId">
    <input type="text" name="input" maxlength=11 required/>
    <textarea></textarea>
    <input type="submit" name="submit"/>
</form>
JS代码如下：

复制代码
var formId = document.getElementById("formId"),
submit = formId.elements['submit'];
EventUtil.addHandler(submit,'click',function(event) {
    var isRequired = formId.elements["submit"].required;
    console.log(isRequired);
});
复制代码
如上打印出false；可以获取到submit的属性required，如果输入框值为空的话，会打印出false出来；

如果想知道浏览器是否支持required这个属性的话，我们可以使用如下代码判断下，如果返回true，说明支持，否则不支持；如下：

var isRequiredSupported = "required" in document.createElement("input");
console.log(isRequiredSupported);
input输入框类型type的值是email或者url

<input type=”email” name=”email”/>

<input type=”url” name=”url”/>

email类型要求输入的文本必须符合电子邮件的格式，url类型要求输入的文本必须符合URL的格式；如下chrome浏览器截图如下；





选择框脚本

选择框是通过<select>和<option>元素创建的，除了表单所有的共有属性和方法外，HTMLSelectElement类还提供了下列属性和方法；

add(newOption,relOption);向控件中插入新<option>元素，其位置在relOption之前；

multiple:布尔值; 表示是否允许多项选择，等价于HTML中的multiple特性；

options: 控件中所有<option>元素的HTMLCollection；

remove(index):   移除给定位置的选项；

selectedIndex:   基于0的选中索引，如果没有该选项，则值为 -1；

size:  选择框中的可见的行数。

如下select框代码：

复制代码
<form id="formId">
    <select name="location" id="selLocation">
        <option value="A">A</option>
        <option value="B">B</option>
        <option value="C">C</option>
        <option value="">D</option>
        <option>E</option>
    </select>
</form>
复制代码
JS代码如下：

复制代码
var formId = document.getElementById("formId"),
select = formId.elements['location'];
console.log(select.value);
EventUtil.addHandler(select,'change',function(){
    console.log(select.value)
});
复制代码
第一次页面加载完成后，打印出值为A；

当每次切换的时候，如果有value就打印出值，如果value=””;则打印空字符串，但是如果option选项没有指定value，在firfox和chrome下打印出当前的文本值，比如上面的文本为E，则值为E；但是IE8及以下，打印的还是空字符串；

在DOM中，每个<option>元素都有一个HTMLOptionElement对象表示；为方便访问数据，对象添加了如下属性；

index: 当前选项在options集合中的索引；

label： 当前选项的标签，等价于HTML中的label

selected：布尔值，表示当前选项是否被选中，将这个属性设置为true可以选中当前选项。

text：选项的文本；

value：选项的值；

我们还是以上面的form表单作为HTML代码，我们使用JS来测试下：

如下代码：

复制代码
var formId = document.getElementById("formId"),
select = formId.elements['location'];
// 获取options集合中的第一项选项的文本
console.log(select.options[0].text);   // 打印出A
// 获取options集合中的第一项选项的value
console.log(select.options[0].value);  // 打印出A
复制代码
对于下拉框只能选择一项的选择框，访问最简单的方式，就是使用selectedIndex属性，如下HTML代码：

复制代码
<form id="formId">
    <select name="location" id="selLocation">
        <option value="A">A</option>
        <option value="B" selected>B</option>
        <option value="C">C</option>
        <option value="">D</option>
        <option>E</option>
    </select>
</form>
复制代码
假如页面初始化的时候默认选择第二项，那么我们可以先使用selectedIndex的属性获取选中的索引，然后根据索引获取当前的文本和值；如下JS代码：

复制代码
var formId = document.getElementById("formId"),
select = formId.elements['location'];
// 获取当前选中的选项的索引selectedIndex
var selectedIndex = select.selectedIndex;
// 获取索引为selectedIndex的option
var selectedOption = select.options[selectedIndex];    
console.log("selected index:"+selectedIndex+"\nselect text:"+selectedOption.text+"\nselect value:"+selectedOption.value);
复制代码
添加选项

可以使用javascript动态创建选项，并将它们添加到选择框中，添加选择框有以下常见3种方式；

第一种方式使用DOM的方式如下：

HTML代码如下：

<form id="formId">
    <select name="location" id="selLocation">
            
    </select>
</form>
JS代码如下：

复制代码
var formId = document.getElementById("formId"),
select = formId.elements['location'];
var newOption = document.createElement("option");
newOption.appendChild(document.createTextNode("A"));
newOption.setAttribute("value","AAAA");
select.appendChild(newOption);
复制代码
第二种方式使用Option构造函数来创建新选项，Option构造函数接收2个参数，文本(text)和值(value),第二个参数可选，比如如下代码：

var formId = document.getElementById("formId"),
select = formId.elements['location'];
var newOption = new Option("Option text","Option value");
select.appendChild(newOption);
但是这种方式在IE下是不生效的；

下面我们来看看第三种方式吧！是使用选择框add()方法，DOM规定这个方法接收2个参数，要添加的新选项和将位于新选项之后的选项，如果想在列表的最后添加一个选项，应将第二个参数设置为null；在IE对add()方法的实现中，第二个参数是可选的，但是标准DOM浏览器中，必须要指定第二个参数，因此为了全兼容浏览器，我们必须添加第二个参数，但是我们可以将第二个参数设置为undefined，含义是：在所有的浏览器将新选项插入到列表的最后了~

如下代码：

var formId = document.getElementById("formId"),
select = formId.elements['location'];
var newOption = new Option("Option text","Option value");
select.add(newOption,undefined);
移除选项的方式如下：

使用dom的removeChild()方法，为其传入要移除的选项；如下代码：
HTML代码如下：

复制代码
<form id="formId">
    <select name="location" id="selLocation">
        <option value="A">A</option>
        <option value="B">B</option>
        <option value="C">C</option>
    </select>
</form>
复制代码
Javascript代码如下：

var formId = document.getElementById("formId"),
select = formId.elements['location'];
// 第一种：移除第一项如下方式
select.removeChild(select.options[0]);
2. 第二种方式是使用选择框的remove()方法，这个方法接收一个参数，既要移除选项的索引；如下代码：

var formId = document.getElementById("formId"),
select = formId.elements['location'];
// 移除第一项
select.remove(0);
3. 最后一种方式，就是将相应的选项设置为null，如下代码：

var formId = document.getElementById("formId"),
select = formId.elements['location'];
// 移除第一项
select.options[0] = null;
理解表单序列化

在javascript中，可以利用表单字段的type属性，连同name和value属性一起实现对表单的序列化，序列化后将把这些数据发送给服务器。

下面是将那些字段需要进行序列化的；

对表单字段的名称和值进行URL编码，使用&分割；
不发送禁用的表单字段；
只发送勾选的单选框和复选框按钮数据；
不发送type为reset和button的按钮
多选选择框中的每个选中的值单独一个条目；
Select元素的值，就是选中option元素的value的值，如果option没有属性value，则是选中的文本值；
如下JS代码是封装form表单的序列化的JS如下：

// 序列化JS代码封装

复制代码
function serialize(form) {
    var arrs = [],
    field = null,
    i,
    len,
    j,
    optLen,
    option,
    optValue;
    for(i = 0,len = form.elements.length; i < len; i++) {
        field = form.elements[i];
        switch(field.type) {
            case "select-one":
               case  "select-multiple":
            if(field.name.length) {
                 for(j = 0,optLen = field.options.length; j < optLen; j++) {
                       option = field.options[j];
                       if(option.selected) {
                           optValue = '';
                           if(option.hasAttribute) {
                               optValue = option.hasAttribute("value") ? option.value : option.text;
                           }else {
                               optValue = option.attributes["value"].specified ? option.value : option.text;
                           }
                           arrs.push(encodeURIComponent(field.name) + "=" +encodeURIComponent(optValue));
                       }
                   }
             }
            break;
            case undefined:      //字段集
            case "file":         // 文件输入
            case "submit":       // 提交按钮
            case "reset":        // 重置按钮
            case "button":       // 自定义按钮
            break;
                    
            case "radio":        // 单选框
            case "checkbox":     // 复选框
            if(!field.checked) {
                break;
            }
            /* 执行默认动作 */
           default:
           // 不包含没有名字的表单字段
           if(field.name.length) {
               arrs.push(encodeURIComponent(field.name) + "=" +encodeURIComponent(field.value));
           }
       }
    }
    return arrs.join("&");
}
复制代码
     如上对form表单序列化的函数serialize，定义了一个arrs数组，用来保存需要序列化后的名值对，然后通过for循环迭代每个表单中的字段，先使用临时变量field保存表单中任意一个字段的引用，然后使用switch语句判断字段的类型type(如果type未定义的话，此元素就不需要表单序列化)，第一种情况是select的单选和多选框，对于select单选框，只可能有一个选中项，对于多选框可能有零或多个选中项，如果有选中项的话(通过属性selected来判断)，需要确定使用什么值，如果不存在value特性，或者虽然存在该特性，但是值为空字符串，都是使用选项的文本来代替，为检查这个特性，在兼容DOM的浏览器下我们需要使用hasAttribute()方法，而在IE中需要使用特性的specified属性；对于type=“file”或者submit，reset，button等就不支持，如果比如上传图片的时候，需要图片的二进制数据使用form表单提交的话，可以在序列化后在加上这个参数即可；对于单选框和复选框如果没有选中的话，同样不进行序列化；下面我们现在来看看一个demo吧！如下HTML代码：

复制代码
<form id="formId">
    <select name="location" id="selLocation" one="one">
        <option value="A">A</option>
        <option value="B">B</option>
        <option value="C">C</option>
    </select>
    <select multiple="multiple" style="width: 50px;" id="mymultiple" name="select2">
        <option>1</option>
        <option>2</option>
        <option>3</option>
        <option>4</option>
        <option>5</option>
    </select>
    <input name="a" type="radio"/>
    <input name="b" type="checkbox"/>
    <input type="file" name="aaaa">
    <input type="hidden" name="hidden" value="hidden"/>
    <input type="submit" value="提交" name="submit"/>
</form>
复制代码
JS代码如下：

复制代码
var formId = document.getElementById("formId");
console.log(serialize(formId));
var submit = formId.elements['submit'];
EventUtil.addHandler(submit,'click',function(e){
    EventUtil.preventDefault(e);
    console.log(serialize(formId));
});
复制代码
我们看到上面的form表单代码，上面有select单选框，也有select多选框，也有隐藏域和input框，但是请注意：上面name=”a”和name=”b”，当他们选中的时候，没有值属性，所以在各个浏览器上都会自带一个值为on的值传给服务器端，但是这个并不是我们想要的，所以的如果需要值的话，一定要设置值，如下如下截图：

在火狐和谷歌下截图如下：



在IE下：



上面的select多选框，如果需要多选的话，记得先要按住键盘上的ctrl键就可以多选了，比如上面的select2=2&select2=3&select2=4 就是select框多选。

理解富文本编辑

在网页中编辑内容，IE最早引入这个功能，随后opera，safari，firefox和chrome也实现了这个功能，基本原理就是在页面中嵌入一个空HTML页面的iframe，通过设置designMode属性，这个空白的HTML页面可以添加文字，而添加文字则是该页面的body元素的html代码，如下所示：



designMode有2个属性，off（默认值）和on, 当设置为on的时候，整个文档变得可编辑，但是我们也可以给他们添加css样式，为了更加美观；首先我们先来看看demo，如下HTML页面嵌套一个iframe；

<iframe name="richedit" src="bank.html"></iframe>

而bank.html页面是一个空页面，代码如下：

复制代码
<!doctype html>
<html lang="en">
 <head>
  <meta charset="UTF-8">
  <meta name="Generator" content="EditPlus®">
  <meta name="Author" content="">
  <meta name="Keywords" content="">
  <meta name="Description" content="">
  <title>Document</title>
 </head>
 <body>
    
 </body>
</html>
复制代码
然后在主页面上使用JS，当页面加载完成后，将designMode属性设置为on即可，如下JS代码：

EventUtil.addHandler(window,'load',function(event){
    frames['richedit'].document.designMode = "on";
});
然后在页面上显示如下：



第二种实现方式是使用contenteditable属性来实现

contenteditable属性是有IE最早实现的，可以把contenteditable属性给页面中的任何元素，然后用户可以立即编辑该元素，不需要iframe，空页面及JS，只需要使用contenteditable属性即可；如下代码给div设置contenteditable属性；如下代码：

<div class="richedit" contenteditable style="width:100px;height:100px;border:1px solid #ccc"></div>
在浏览器下效果如下：



支持的浏览器有；IE，firefox，chrome，safari和opera；

在移动设备上，有ios5+和Android3+；

Contenteditable属性有三个可能值，true表示打开，false表示关闭，inherit表示从父元素那边继承。