# jQuery is out of date

在我看来，jQuery确实已经过时了。本项目总结了大部分 jQuery API 替代的方法，类似项目[You-Dont-Need-jQuery](https://github.com/oneuijs/You-Dont-Need-jQuery)，并会再此基础上进行补充。写这个项目主要想让自己和大家增进对javascript原生api的理解。兼容ie9及以上浏览器，如不支持ie9会特别说明。

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
function $(query) {
  return document.querySelector(query)
}

function $$(query) {
  return Array.prototype.slice.call(document.querySelectorAll(query)) || []
}

```

如果在jQuery示例下的$是jquery对象，在Native示例下的$是以上的实现。相当于实现了chrome控制台上$，$$方法。以$开头的变量名为jQuery对象。

##  DOM Manipulation

很多人一直认为jQuery还没有过时的其中一个原因是在DOM操作上非常方便。接下来比较一下。

* addClass

  ```javascript
  // jQuery
  $el.addClass(className)

  // Native (IE 10+ support)
  el.classList.add(className)
  ```

* after

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

* append

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


* attr

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

* before

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

* clone

  ```javascript
  // jQuery
  $el.clone()

  // Native
  el.cloneNode()

  // For Deep clone , set param as `true`
  ```

* css

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

* detach

* empty

  ```javascript
  // jQuery
  $el.empty()

  // Native
  el.innerHTML = ''
  ```

* hasClass

  ```javascript
  // jQuery
  $el.hasClass(className)

  // Native
  el.classList.contains(className)
  ```

* height

  ```javascript

  ```

* html

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

* innerHeight

* innerWidth

* insertAfter

* insertBefore

* cssNumber

* offset

* outHeight

* position

* prepend

* prependTo

* prop

* remove

* removeAttr

* removeClass

  ```javascript
  // jQuery
  $el.removeClass(className)

  // Native
  // IE 10+ support
  el.classList.remove(className)
  ```

* removeProp

* replaceAll

* replaceWith

* scrollLeft

* scrollTop

* text

* toggleClass

* unwrap

* val

* width

* wrap

* wrapAll

* wrapInner

## Query Selector

常用的 class、id、属性 选择器都可以使用 `document.querySelector` 或 `document.querySelectorAll` 替代。区别是

- `document.querySelector` 返回第一个匹配的 Element
- `document.querySelectorAll` 返回所有匹配的 Element 组成的 NodeList。它可以通过 `[].slice.call()` 把它转成 Array
- 如果匹配不到任何 Element，jQuery 返回空数组 `[]`，但 `document.querySelector` 返回 `null`，注意空指针异常。当找不到时，也可以使用 `||` 设置默认的值，如 `document.querySelectorAll(selector) || []`

> 注意：`document.querySelector` 和 `document.querySelectorAll` 性能很**差**。如果想提高性能，尽量使用 `document.getElementById`、`document.getElementsByClassName` 或 `document.getElementsByTagName`。

以下只实现和jquery有所区别的api

* :contains

  ```javascript
  // jQuery
  $('.class:contains("metchString")')

  // Native
  $$('.class').filter(el => el.innerText.indexOf('metchString') !== -1)
  ```

* ​

## Ajax

## Events

## Utilities

## Animation

