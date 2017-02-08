# jQuery is out of date

在我看来，jQuery确实已经过时了。本项目总结了绝大部分 jQuery API 替代的方法，类似项目[You-Dont-Need-jQuery](https://github.com/oneuijs/You-Dont-Need-jQuery)，并会再此基础上进行很多的补充。写这个项目主要想让自己和大家增进对javascript原生api的理解，也可以作为一个"原生jquery"的api文档随时查看。兼容ie9及以上浏览器，如不支持ie9会特别说明。

## 目录

1. [Regulation](#Regulation)
2. [DOM Manipulation](#DOM Manipulation)
3. [Query Selector](#Query Selector)
4. [Ajax](#Ajax)
5. [Events](#Events)
6. [Utilities](#Utilities)
7. [Animation](#Animation)

## Regulation

```javascript
function $(selector) {
  return document.querySelector(selector)
}

function $$(selector) {
  return Array.prototype.slice.call(document.querySelectorAll(selector))
}
```

如果在jQuery示例下的$是jquery对象，在Native示例下的$是以上的实现。相当于实现了chrome控制台上$，$$方法。以$开头的变量名为jQuery对象。

##  DOM Manipulation

很多人一直认为jQuery还没有过时的其中一个原因是在DOM操作上非常方便。接下来比较一下。

### addClass

为每个匹配的元素添加指定的样式类名

```javascript
// jQuery
$el.addClass(className)

// Native (IE 10+ support)
el.classList.add(className)
```
### after

在匹配元素集合中的每个元素后面插入参数所指定的内容，作为其兄弟节点。

```javascript
// jQuery
$el.after('<p></p>')

// Native (Html string)
el.insertAdjacentHTML('afterend', '<p></p>')

// Native (Element)
el.insertAdjacentElement('afterend', newEl)

// Native (Element)
el.parentNode.insertBefore(newEl, el.nextSibling)
```
### append

在每个匹配元素里面的末尾处插入参数内容。

```javascript
// jQuery
$el.append('<p></p>')

// Native (Html string)
el.insertAdjacentHTML('beforeend', '<p></p>')

// Native (Element)
el.insertAdjacentElement('beforeend', newEl)

// Native (Element)
el.appendChild(newEl)
```
### appendTo

​	与[append](#append)相反

### attr

获取匹配的元素集合中的第一个元素的属性的值。设置每一个匹配元素的一个或多个属性。

```javascript
// jQuery
$el.attr('foo')

// Native
el.getAttribute('foo')

// jQuery
$el.attr('foo', 'bar')

// Native
el.setAttribute('foo', 'bar')
```
### before

根据参数设定，在匹配元素的前面插入内容（外部插入）

```javascript
// jQuery
$el.before('<p></p>')

// Native (Html string)
el.insertAdjacentHTML('beforebegin', '<p></p>')

// Native (Element)
el.insertAdjacentElement('beforebegin', newEl)

// Native (Element)
el.parentNode.insertBefore(newEl, el)
```
### clone

创建一个匹配的元素集合的深度拷贝副本。

```javascript
// jQuery
$el.clone()

// Native
el.cloneNode()

// For Deep clone , set param as `true`
```
### css

获取匹配元素集合中的第一个元素的样式属性的值设置每个匹配元素的一个或多个CSS属性。

```javascript
// jQuery
$el.css('color')

// Native
// 注意：此处为了解决当 style 值为 auto 时，返回 auto 的问题
const win = el.ownerDocument.defaultView

// null 的意思是不返回伪类元素
win.getComputedStyle(el, null).color

// jQuery
$el.css({ color: '#f01' })

// Native
el.style.color = '#f01'

// 一次性设置多个样式
// jQuery
$el.css({ color: '#f01', 'background-color': '#000' })

// Native
const cssObj = {color: '#f01', backgroundColor: '#000'}
for (let key in cssObj) {
  el.style[key] = cssObj[key]
}

// Native
const cssText = 'color: #f01; background-color: #000'
el.style.cssText += cssText
```
### detach

从DOM中去掉所有匹配的元素。

### empty

从DOM中移除集合中匹配元素的所有子节点。

```javascript
// jQuery
$el.empty()

// Native
el.innerHTML = ''
```
### hasClass

确定任何一个匹配元素是否有被分配给定的（样式）类。

```javascript
// jQuery
$el.hasClass(className)

// Native (IE 10+ support)
el.classList.contains(className)
```
### height

获取匹配元素集合中的第一个元素的当前计算高度值。设置每一个匹配元素的高度值。

```javascript

```
### html

获取集合中第一个匹配元素的HTML内容 设置每一个匹配元素的html内容。

```javascript
// jQuery
$el.html()

// Native
el.innerHTML

// jQuery
$el.html(htmlString)

// Native
el.innerHTML = htmlString
```
### innerHeight

为匹配的元素集合中获取第一个元素的当前计算高度值,包括padding，但是不包括border。

```javascript
// jQuery
$el.innerHeight()

// Native
el.clientHeight()
```

### innerWidth

为匹配的元素集合中获取第一个元素的当前计算宽度值,包括padding，但是不包括border。

```javascript
// jQuery
$el.innerWidth()

// Native
el.clientWidth()
```

### insertAfter

与[after](#after)相反

### insertBefore

与[before](#before)相反

### offset

在匹配的元素集合中，获取的第一个元素的当前坐标，坐标相对于文档。 设置匹配的元素集合中每一个元素的坐标， 坐标相对于文档。

```javascript
// jQuery
$el.offset()

// Native
const elClientRect = el.getBoundingClientRect()
{
  top: elClientRect.top + window.pageYOffset - document.documentElement.clientTop,
  left: elClientRect.left + window.pageXOffset - document.documentElement.clientLeft
}

// jQuery
$el.offset(10, 10)

// Native
const elClientRect = el.getBoundingClientRect()
const elTop = 10 - elClientRect.top - document.documentElement.clientTop
const elLeft = 10 - elClientRect.left - document.documentElement.clientLeft
el.style.cssText += `position: relative;top: ${elTop}px;left: ${elLeft}px;`
```

### outerHeight

获取元素集合中第一个元素的当前计算高度值,包括padding，border和选择性的margin。返回一个整数（不包含“px”）表示的值  ，或如果在一个空集合上调用该方法，则会返回 null。

```javascript
// jQuery
$el.outerHeight()

// Native
el.offsetHeight

// jQuery
$el.outerHeight(true)

// Native
const win = el.ownerDocument.defaultView
const {marginTop, margintBottom} = win.getComputedStyle(el, null)
el.offsetHeight + parseFloat(marginTop) + parseFloat(margintBottom) === $el.outerHeight(true) // true
```

### outerWidth

与[outerHeight](#outerHeight)类似。

### position

获取匹配元素中第一个元素的当前坐标，相对于offset parent的坐标。( offset parent指离该元素最近的而且被定位过的祖先元素 )

```javascript
// jQuery
$el.position()

// Native
{ left: el.offsetLeft, top: el.offsetTop }
```

### prepend

将参数内容插入到每个匹配元素的前面（元素内部）。

```javascript
// jQuery
$el.prepend('<p></p>')

// Native (HTML string)
el.insertAdjacentHTML('afterbegin', '<p></p>')

// Native (Element)
el.insertBefore(newEl, el.firstChild)
```

### prependTo

与[prepend](#prepend)相反

### remove

将匹配元素集合从DOM中删除。（注：同时移除元素上的事件及 jQuery 数据。）

```javascript
// jQuery
$el.remove()

// Native
el.parentNode.removeChild(el)

// Native
el.outerHTML = ''
```

### removeAttr

为匹配的元素集合中的每个元素中移除一个属性（attribute）。

```javascript
// jQuery
$el.removeAttr(attr)

// Native
el.removeAttribute(attr)
```

### removeClass

移除集合中每个匹配元素上一个，多个或全部样式。

```javascript
// jQuery
$el.removeClass(className)

// Native
// IE 10+ support
el.classList.remove(className)
```
### replaceAll

与[replaceWith](#replaceWith)相反

### replaceWith

用提供的内容替换集合中所有匹配的元素并且返回被删除元素的集合。

```javascript
// jQuery
$el.replaceWith('<p></p>')

// Native (HTML string)
el.outerHTML = '<p></p>'

// Native (Element)
el.parentNode.replaceChild(newEl, el)
```

### scrollLeft

与[scrollTop](#scrollTop)一样

### scrollTop

获取匹配的元素集合中第一个元素的当前垂直滚动条的位置或设置每个匹配元素的垂直滚动条位置。设置每个匹配元素的垂直滚动条位置

```javascript
// jQuery
$(el).scrollTop()

// Native (el is window)
Math.max(document.documentElement.scrollTop, document.body.scrollTop)
or
window.pageYOffset

// Native (el is not window)
el.scrollTop

// jQuery
$(el).scrollTop(10)

// Native (el is window)
document.documentElement.scrollTop = 10
document.body.scrollTop = 10

// Native (el is not window)
el.scrollTop = 10
```

### text

得到匹配元素集合中每个元素的合并文本，包括他们的后代设置匹配元素集合中每个元素的文本内容为指定的文本内容。

```javascript
// jQuery
$el.text()

// Native
el.textContent

// jQuery
$el.text(string)

// Native
el.textContent = string
```

### toggleClass

在匹配的元素集合中的每个元素上添加或删除一个或多个样式类,取决于这个样式类是否存在或值切换属性。即：如果存在（不存在）就删除（添加）一个类。

```javascript
// jQuery
$el.toggleClass(className)

// Native
el.classList.toggle(className)
```

### unwrap

将匹配元素集合的父级元素删除，保留自身（和兄弟元素，如果存在）在原来的位置。

```javascript
// jQuery
$el.unwrap()

// Native
const parent = el.parentNode
parent.outerHTML = parent.innerHTML
```

### val

获取匹配的元素集合中第一个元素的当前值。设置匹配的元素集合中每个元素的值。

```javascript
// jQuery
$el.val()

// Native
el.value

// jQuery
$el.val(value)

// Native
el.value = value
```

### width

### wrap

### wrapAll

### wrapInner

## Query Selector

常用的 class、id、属性 选择器都可以使用 `document.querySelector` 或 `document.querySelectorAll` 替代。区别是

- `document.querySelector` 返回第一个匹配的 Element
- `document.querySelectorAll` 返回所有匹配的 Element 组成的 NodeList。它可以通过 `[].slice.call()` 把它转成 Array
- 如果匹配不到任何 Element，jQuery 返回空数组 `[]`，但 `document.querySelector` 返回 `null`，注意空指针异常。

> 注意：`document.querySelector` 和 `document.querySelectorAll` 性能很**差**。如果想提高性能，尽量使用 `document.getElementById`、`document.getElementsByClassName` 或 `document.getElementsByTagName`。

以下只实现和jquery有所区别的api

### :contains

选择所有包含指定文本的元素。

```javascript
// jQuery
$('selector:contains("metchString")')

// Native
$$('selector').filter(el => el.textContent.indexOf('metchString') !== -1)
```

### :emtpy

选择所有没有子元素的元素（包括文本节点）。

```javascript
// jQuery
$('selector:empty')

// Native
$$('selector').filter(el => el.innerHTML === '')
```

### :even

选择索引值为偶数的元素，从 0 开始计数。

```javascript
// jQuery
$('selector:even')

// Native
$$('selector').filter((el, index) => (index & 1) === 0)
```

### :focus

选择当前获取焦点的元素。

```javascript
// jQuery
$(':focus')

// Native
document.activeElement
```

### :gt

选择匹配集合中所有大于给定index（索引值）的元素。

```javascript
// jQuery
$('selector:gt(2)')

// Native
$$('selector').filter((el, index) => index > 2)
```

### :has

选择元素其中至少包含指定选择器匹配的一个种元素。

```javascript
// jQuery
$('selector:has(p)')

// Native
$$('selector').filter(el => el.querySelector('p') !== null)
```

### :header

选择所有标题元素，像h1, h2, h3 等.

```javascript
// jQuery
$('selector:header')

// Native
$$('selector').filter(el => /^h\d$/i.test(el.nodeName))
```

### :hidden

选择所有隐藏的元素。

```javascript
// jQuery
$('selector:hidden')

// Native
```

### :lt

选择匹配集合中所有索引值小于给定index参数的元素。

```javascript
// jQuery
$('selector:lt(2)')

// Native
$$('selector').filter((el, index) => index < 2)
```

### :not

选择所有元素去除不匹配给定的选择器的元素。

```javascript
// jQuery
$('input:not(:checked)')

// Native
$$('input').filter(el => !el.checked)

// jQuery
$('selector:not(.class)')

// Native (IE 10+ support)
$$('selector').filter(el => !el.classList.contains('class'))
```

### :odd 

选择索引值为奇数元素，从 0 开始计数。

```javascript
// jQuery
$('selector:odd')

// Native
$$('selector').filter((el, index) => (index & 1) ===1)
```

### only-child

如果某个元素是其父元素的唯一子元素，那么它就会被选中。

```javascript
// jQuery
$('selector:only-child')

// Native
$$('selector').filter(el => el.previousElementSibling === null && el.nextElementSibling === null)

// Native
$$('selector').filter(el => el.parentNode.children.length === 1)
```

### :only-of-type

选择器匹配属于其父元素的特定类型的唯一子元素的每个元素。

```javascript
// jQuery
$('selector:only-of-type')

// Native
$$('selector').filter(el => {
  for(let child of el.parentNode.children) {
    if (child !== el && child.nodeName === el.nodeName) {
      return true
    }
  }
  return false
})
```

### :parent

选择所有含有子元素或者文本的父级元素。

```javascript
// jQuery
$('selector:parent')

// Native
$$('selector').filter(el => el.innerHTML !== '')
```

### :selected

获取 select 元素中所有被选中的元素。

```javascript
// jQuery
$('select option:selected')

// Native (single)
$('select option')[$('select').selectedIndex]

// Native (multiple)
$$('select option').filter(el => el.selected)
```

### :visible

 选择所有可见的元素。

```javascript

```

## Ajax

## Events

## Utilities

### contains

检查一个DOM元素是另一个DOM元素的后代。

```javascript
// jQuery
$.contains(el, child)

// Native
el !== child && el.contains(child)
```

### each

一个通用的迭代函数，它可以用来无缝迭代对象和数组。数组和类似数组的对象通过一个长度属性（如一个函数的参数对象）来迭代数字索引，从0到length - 1。其他对象通过其属性名进行迭代。

```javascript
// jQuery
$.each(array, (index, value) => {
});

// Native
array.forEach((value, index) => {
});
```

### extend

将两个或更多对象的内容合并到第一个对象。

```javascript
// jQuery
$.extend({}, {a: 1}, {b: 2}) // {a: 1, b: 2}

// ES6-way
Object.assign({}, {a: 1}, {b: 2}) // {a: 1, b: 2}
```

### globalEval

在全局上下文下执行一些JavaScript代码。

```javascript
// jQuery
$.globaleval(code)

// Native
function Globaleval(code) {
  const script = document.createElement('script')
  script.text = code

  document.head.appendChild(script).parentNode.removeChild(script)
}

// Use eval, but context of eval is current, context of $.Globaleval is global.
eval(code)
```

### grep

查找满足过滤函数的数组元素。原始数组不受影响。

```javascript
// jQuery
$.grep([10,11,3,4], n => n > 9)

// Native
[10,11,3,4].filter(n => n > 9)
```

### inArray

在数组中查找指定值并返回它的索引（如果没有找到，则返回-1）。

```javascript
// jQuery
$.inArray(item, array)

// Native
array.indexOf(item) > -1

// ES6-way
array.includes(item)
```

### isArray

确定的参数是一个数组。

```javascript
// jQuery
$.isArray(array)

// Native
Array.isArray(array)
```

### isEmptyObject

检查对象是否为空（不包含任何属性）。

```javascript
// jQuery
$.isEmptyObject(obj)

// Native
function isEmptyObject(obj) {
  return Object.keys(obj).length === 0;
}
```

### isFunction

确定参数是否为一个Javascript 函数。

```javascript
// jQuery
$.isFunction(item);

// Native
function isFunction(item) {
  if (typeof item === 'function') {
    return true
  }
  var type = Object.prototype.toString(item)
  return type === '[object Function]' || type === '[object GeneratorFunction]'
}
```

### isNumeric

确定它的参数是否是一个数字。

```javascript
// jQuery
$.isNumeric(item)

// Native
function isNumeric(value) {
  var type = typeof value

  return (type === 'number' || type === 'string') && !isNaN(value - parseFloat(value))
}
```

### isPlainObject

测试对象是否是纯粹的对象（通过 "{}" 或者 "new Object" 创建的）

```javascript
// jQuery
$.isPlainObject(obj);

// Native
function isPlainObject(obj) {
  if (typeof (obj) !== 'object' || obj.nodeType || obj !== null && obj !== undefined && obj === obj.window) {
    return false;
  }

  if (obj.constructor &&
      !Object.prototype.hasOwnProperty.call(obj.constructor.prototype, 'isPrototypeOf')) {
    return false;
  }

  return true;
}
```

### isWindow

确定参数是否为一个window对象。

```javascript
// jQuery
$.isWindow(obj);

// Native
function isWindow(obj) {
  return obj !== null && obj !== undefined && obj === obj.window;
}
// jquery源码中是这么判断对象是否为window的，我的理解是代码可能会跑到服务器上，因为服务器上是没有window对象的。所以这么判断
```

### isXMLDoc

检查一个DOM节点是否在XML文档中（或者是一个XML文档）。

###  makeArray

转换一个类似数组的对象成为真正的JavaScript数组。

```javascript
// jQuery
$.makeArray(arrayLike)

// Native
Array.prototype.slice.call(arrayLike)

// ES6-way
Array.from(arrayLike)
```

### map

将一个数组中的所有元素转换到另一个数组中。

```javascript
// jQuery
$.map(array, (value, index) => {
})

// Native
array.map((value, index) => {
})
```

### merge

合并两个数组内容到第一个数组。

````javascript
// jQuery
$.merge(array1, array2)

// Native
// But concat function doesn't remove duplicate items.
function merge(...args) {
  return [].concat(...args)
}
````

### now

返回一个数字，表示当前时间。

```javascript
// jQuery
$.now()

// Native
Date.now()

// Native
+new Date()

// Native
new Date().getTime()
```

### parseHTML

将字符串解析到一个DOM节点的数组中。

```javascript
// jQuery
$.parseHTML(htmlString)

// Native
function parseHTML(string) {
  const context = document.implementation.createHTMLDocument()

  // Set the base href for the created document so any parsed elements with URLs
  // are based on the document's URL
  const base = context.createElement('base')
  base.href = document.location.href
  context.head.appendChild(base)

  context.body.innerHTML = string
  return context.body.children
}
```

### parseJSON

接受一个标准格式的 JSON 字符串，并返回解析后的 JavaScript 对象。

```javascript
// jQuery
$.parseJSON(str)

// Native
JSON.parse(str)
```

### parseXML

解析一个字符串到一个XML文档。

### proxy

接受一个函数，然后返回一个新函数，并且这个新函数始终保持了特定的上下文语境。

```javascript
// jQuery
$.proxy(fn, context)

// Native
fn.bind(context)
```

### trim

去掉字符串起始和结尾的空格。

```javascript
// jQuery
$.trim(string)

// Native
string.trim()
```

### type

确定JavaScript 对象的类型[[Class]] 。

```javascript
// jQuery
$.type(obj);

// Native
function type(item) {
  const reTypeOf = /(?:^\[object\s(.*?)\]$)/;
  return Object.prototype.toString.call(item)
    .replace(reTypeOf, '$1')
    .toLowerCase();
}
```

## Animation

