---
layout: post
title: Javascript
description: >
  1/20 Javascript
tags: [study]
comments: true
---

# 1/20 Javascript

- javascript 사용 : 
  - 스크립트 파일 include
  - 스크립트 직접 코딩 -> script 태그 안의 공간은 '전역'

>  'use strict' : 프로그램을 더 엄격한 운용 콘텍스트로 실행하게 한다.



**DOM** : document object model.

​			자바스크립트로 HTML문서를 다룰 수 있게 브라우저 환경 제공 API



## DOM 사용 하기

### element 찾기

document.getElement~ : DOM 객체를 return 해준다.

> getElementById : returns HTMLElement
>
> getElementsByTagName : returns HTMLCollection
>
> getElementsByClassName : returns HTMLCollection

HTMLCollection : 유사배열. Array와 비슷하지만 Array는 아니다.

​							   index로 접근하고 length를 갖는다.

getElements~는 다시 element에서 사용하여 검색의 범위를 좁힐 수 있다. (DOM이 트리 구조로 모델링 되기 때문)



### 이벤트 핸들러 적용/해제

```javascript
element.addEventListener("click",function(){

}, false);
```

parameters:

1. 이벤트명
2. 이벤트 핸들러
3. 캡처링 사용 여부

> 이벤트 핸들러 내부 this는 'element'를 의미한다.

```javascript
element.removeEventListener("click", handler, false);
```

해제 시에 이벤트 핸들러가 필요하기 때문에 <u>함수의 참조를 저장</u>해둬야한다. 



### 동적으로 element 만들기/삭제

#### createElement 

> **document.createElement(tagName)**
>
> **document.createTextNode(text)**

> **element.appendChild(node)**

```javascript
var newDiv = document.createElement("div");
var newP = document.createElement("p");
var newText = document.createTextNode("HELLO ROOKIES");

newP.appendChild(newText);
newDiv.appendChild(newP);

document.body.appendChild(newDiv);
```

> **element.removeChilde(node)**

이벤트 핸들러 때와 같이 참조를 하고 있어야 삭제할 수 있다.

#### innerHTML

> **element.innerHTML = '';**

html 텍스트를 이용해 자식 노드를 생성한다.

```javascript
document.body.innerHTML = "<div><p>HELLO ROOKIES</p></div>";
```

- 장점 : 새로운 element를 만드는 과정에서 빠르다.
- 단점 : element에 변화를 주는 경우에 비효율적이다. (이벤트 처리 등)

주의: 해당 함수로 값을 세팅하는 경우 모든 element를 지우고 새로 생성한다. 

​		  => string을 만든 후에 세팅 해야한다.

#### textContent

> **element.textContent = '';**

innerHTML과 비슷하지만 텍스트 노드를 생성하거나 참조할 수 있다.

자식element들의 모든 텍스트 노드를 확인할 수 있다.



## 체크박스 다루기

html

```javascript
<input type="checkbox" value="someValue" checked />
```

js

```javascript
input.value
input.checked //체크여부(radio, checkbox)
```



## 이벤트의 전파

이벤트는 특정 방향으로 전파된다.

Capturing : 위에서 아래 (부모에서 자식 순서로)

Bubbling : 아래에서 위로 (자식에서 부모 순서로)

> capturing -> event -> bubbling

### 이벤트 전파 제어

addEventListener의 세번째 인자로 캡처링/버블링을 선택할 수 있다.

true면 캡처링

```javascript
var useCapturing = true;

element.addEventListener('click', function(eventObject) {
  eventObject.stopPropagation(); // 캡처링이나 버블링을 취소한다.(이벤트 전파를 차단한다)

  //etc
  eventObject.preventDefault(); // 디폴트 동작을 취소한다.(ex. 링크 이동 차단)
  //체크 박스의 클릭을 취소하는경우 브라우저마다 다른 동작을 한다.
  //그밖에 많에 정보를 포함
}, useCapturing);
```



## CSS 제어

element객체의 style 속성을 이용해서 CSS를 적용한다.

```javascript
// background-color같이 하이픈으로 이어진 속성은 카멜케이스로 변경
element.style.backgroundColor = '#f00';

//HTML의 class속성과 동일
element.className = 'myClass';

//다중 적용시 띄어쓰기
element.className = 'myClass1 myClass2';

//아이디도 동일하게 적용가능(다중 적용 X)
element.id = 'myId';
```

html 파일에서 상단 <head>안에 style 태그 적용도 가능하다. 

```html
<style>
  .myClass {border:1px solid #f00}
  #myId {padding: 5px}
</style>
```

디자인 적용은 주로 class 사용이 좋다.



## QuerySelector

```javascript
document.querySelector('.myClass');	//첫 한 개만 return
document.querySelectorAll('.myClass');	//해당 모든 element return
```



## Element Node 속성

```javascript
element.firstChild // 첫번째 자식
element.lastChild // 마지막 자식
element.parentNode // 부모
element.nextSibling // 다음 형제
element.previousSibling  // 이전 형제
element.childNodes // 자식들을 모두 담고 있는 HTMLCollection
```



## 함수

함수 표현식

```javascript
const add = functin() {};
```

함수 선언식 : 순서에 영향을 받지 않는다.

```javascript
function add() {

};
```



## 참조

DOM API 정보 검색은 Mozilla developer network 활용

mdn 사이트 : https://developer.mozilla.org/ko/

- 웹플랫폼 -> API와 DOM매뉴에서 element와 node파트 확인
- 웹 기술에 대한 거이 모든 정보를 얻을 수 있다.



#### 교육 내용 출처: NHN 기술교육 - 1/20 JavaScript 기초 (김성호 책임)

