# **Event Listener**

1. 이벤트 등록 / 해제
2. 이벤트 전달방법 선택

나의 자바스크립트에서의 이벤트 관리은 등록후 해당 엘레멘트가 트리거 되었을때 동작하게 함.(엘레멘트마다 리스너를 달아줌)
이벤트 버블링, 캡쳐, 위임등을 모르고 있었어서 사용할 기회가 없었기 때문.

앞으로 유연하게 사용하기 위해 1. 2. 내용을 정리하겠음.

<br>

## **Event Registration/Deregistration**

이벤트 등록은 addEventListener() 메소드로 할 수 있음.

1. 첫번째 인자 - '이벤트 타입'
2. 두번째 인자 - '리스너 함수'
3. 세번째 인자 - '옵션들'
   등을 인자로 받는데, 세번째 인자로는 boolean값으로  
   useCapture를 활성화 할 수 있다.(기본값은 false)

이벤트 해제는 removeEventListener() 메소드로 할 수 있음.  
이벤트 등록에 사용한 인자들을 그대로 사용해야 해당 이벤트를 해제할 수 있음.

ex>

```js
const El = document.querySelector("button");

const listener = (e) => {
  console.log(e);
};

El.addEventListener("click", listener);

setTimeout(() => {
  El.removeEventListener("click", listener);
}, 3000); // 3초 뒤에 El 클릭 이벤트는 동작하지 않음
```

## **Event Bubbling**

특정 엘레멘트에서 이벤트가 발생했을 때 해당 이벤트가 더 상위의 엘레멘트들로 전달되어 가는 특성을 의미함.

```html
<div class="outter">
  <div class="middle">
    <div class="inner"></div>
  </div>
</div>
```

```js
const Els = document.querySelectorAll("div");

const clickHandle = (e) => {
  console.log(e.currentTarget.className);
};

Els.forEach((el) => el.addEventListener("click", clickHandle));
```

`inner 엘레멘트 클릭시 inner - middle - outter 차례대로 콘솔에 찍힘`

<br>

## **Event Capture**

이벤트 버블링과 반대 방향으로 진행되는 이벤트 전파 방식.

```js
const Els = document.querySelectorAll("div");

const clickHandle = (e) => {
  console.log(e.currentTarget.className);
};

Els.forEach((el) => el.addEventListener("click", clickHandle, true));
```

`inner 엘레멘트 클릭시 outter - middle - inner 차례대로 콘솔에 찍힘`

<br>

## **Event Delegation**

이벤트 위임은 상위 엘레멘트에 이벤트를 등록하여 하위 엘레멘트의 이벤트를 제어함.  
자바스크립트로 DOM에 엘레멘트를 추가하면, 시점상 자바스크립트에서 이벤트 제어를 하지 못하기 때문에 그 상위 요소에 이벤트를 위임해서 하위 요소 이벤트를 감지 / 제어함.(하위요소 이벤트가 버블링 되어 상위요소 이벤트가 동작함.)

```html
<div class="outter">
  <div class="middle">
    <div class="inner"></div>
  </div>
</div>
```

```js
const outter = document.querySelector(".outter");

const clickHandle = (e) => {
  console.log(e.target.className);
};

outter.addEventListener("click", clickHandle);
```

`엘레멘트 클릭시 해당 엘레멘트의 클래스 이름이 콘솔에 찍힘`
