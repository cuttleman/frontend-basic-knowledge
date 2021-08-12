# **Async**

## **Callback**

매개변수를 통해 다른 함수의 내부로 전달되는 함수를 콜백 함수라고 하며, 매개변수를 통해 콜백 함수를 전달받는 함수를 고차함수 라고 함

고차함수는 함수를 매개변수로 받거나, 함수를 반환하는 함수.

```js
function callback(a, b) {
  return a + b;
}

function higherOrderFunc(x, y, cb) {
  return cb(x, y);
}

console.log(higherOrderFunc(1, 2, callback)); // 3
```

콜백함수로 비동기동작을 구현할 수 있음.
순차적인 비동기동작을 콜백함수 기반으로 구현하면 '콜백 지옥'에 빠지게 됨.

### 콜백 지옥이란?

```js
function loadScript(script, cb) {
  const scriptEl = document.createElement("script");
  scriptEl.src = script;
  scriptEl.onload = cb(null, script);
  scriptEl.onerror = cb(new Error("Error!"));
  document.body.append(scriptEl);
}

loadScript("one.js", function (err, ok) {
  if (err) {
    // error handling
  }
  if (ok) {
    loadScript("two.js", function (err, ok) {
      if (err) {
        // error handling
      }
      if (ok) {
        loadScript("three.js", function (err, ok) {
          if (err) {
            // error handling
          }
          if (ok) {
            console.log("finish");
          }
        });
      }
    });
  }
});
```

`콜백 안에 콜백을 부르는 것으로 오른쪽으로 계속 길어지는 것을 말함.`

## **Promise**

1. Promise는 pending state를 거쳐 fulfilled, rejected state의 프로미스 객체를 반환함.
2. Promise객체에는 then, catch, finally 메소드가 있어, 후속처리를 해줄 수 있음.
3. then() 메소드에 첫번째 인자는 resolve value가 인자로 들어간 함수가 들어가고, 두번째 인자는 error value가 인자로 들어간 함수가 들어감.
4. then() 메소드는 Promise 객체를 반환하게 때문에 then, catch, finally 함수를 이어줄 수 있음.
5. .then((result, error)=>{...}) 와 .then(result=>{...}).catch(error=>{...}) 는 다른 에러 핸들링을 나타냄. (전자는 result에서의 에러를 잡아내지 못함)

- **Promise.all()**

  ```js
  Promise.all([...promises...]);

  return [...]

  ```

  - Promise.all 메소드는 Promise로 구성된 iterable 객체를 인수로 받고 새로운 Promise를 반환함.
  - 인수의 요소가 모두 resolve 되야함, 하나라도 거절되면 전체를 거절함.
  - Promise가 아닌 요소가 이터러블 객체안에 들어가면 그 값이 그대로 resolve 배열값으로 들어감.

<br />

- **Promise.allSettled()**

  ```js
  Promise.allSettled([...promises]);

  return [
    {status: 'fulfilled', value: ...응답...},
    {status: 'rejected', reason: ...에러 객체...},
    {status: 'fulfilled', value: ...응답...}
  ]
  ```

  - Promise.all과 기본적으로 같음.
  - allSettled 메소드는 거절된 프로미스가 있더라도 결과를 반환함.

<br />

- **Promise.race()**

  ```js
  Promise.race([...promises]);

  return one;
  ```

  - Promise.all과 기본적으로 같음.
  - 가장 먼저 처리되는 Promise의 값이 반환됨. (이행 또는 거절 값)

<br />

- **Promise(.resolve|.reject)()**

  ```js
  Promise.resolve(value);
  Promise.reject(value);
  ```

  - 결과값이 Promise인 값을 반환.
  - 호환성을 위해 쓰임. (async/await 문법으로 요샌 안쓰임)

<br />

## **async / await**

1. function 앞에 `async`키워드를 붙이면 함수는 항상 Promise를 반환함.
2. `await`는 `async`로 감싼 함수안에서만 쓸 수 있음.
3. 자바스크립트는 `await` 키워드를 만나면 Promise가 settled될 때까지 함수 실행을 기다림.
4. `await`는 최상위 레벨에서는 작동안함.
5. `await`는 thenable 객체를 받음.
6. class 메소드에도 `async` 키워드 사용 가능.

<br />

## **Error Handling**

1. Promise 객체는 catch 메소드로 에러를 잡아냄.
2. then 메소드의 두번째 인자로 에러를 처리해줄 수 있음.
3. 2번방법으로 처리시 해당 then 메소드에서 발생한 에러를 잡아내지못함.
4. catch메소드는 이전 Promise 객체의 에러들을 잡아낼 수 있지만, 그 이후 것은 잡아내지 못함.
5. `async/await` 문법은 try...catch문과 catch메소드로 에러를 핸들링함.

<br />

## **Call Stack / MicroTask / MacroTask**

1. 콜스택에는 실행컨텍스트가 쌓임.
2. Promise의 then, catch, finally 메소드는 마이크로태스크 큐에 들어감.
3. queueMicrotask 함수로 감싼 콜백함수는 마이크로태스크 큐에 들어감.
4. web api를 통해 들어오는 함수는 매크로태스크 큐에 들어감.
5. 일반 함수 > 마이크로태스크 함수 > 매크로태스크 함수 우선순위를 가짐.
6. 매크로태스크 함수가 처리되기 전에 마이크로태스크 큐에 있는 함수들이 먼저 처리되고 콜 스택에 들어감.
7. event loop는 콜스택이 비어있는 것을 확인하고 태스크 큐(마이크로/매크로)의 함수들을 콜스택에 넣어줌.

<img src="https://uploads.disquscdn.com/images/9466d8aa53fc5b3e63a92858a94bb429df02bbd20012b738f0461343beaa6f90.gif?w=800&h=416" />

`이미지에서 볼 수 있듯, 콜스택에 있는 일반 함수 처리후 마이크로태스크 함수들 처리 -> 매크로태스크 함수 처리를 하고 있음.`

```js
function foo() {
  console.log("foo");
}
function bar() {
  setTimeout(foo);
  console.log("bar");
}
function baz() {
  setTimeout(() => console.log("baz"));
}
function liz() {
  requestAnimationFrame(() => console.log("liz"));
}
function rep() {
  Promise.resolve().then(() => console.log("rep"));
}
console.log("start");
foo();
bar();
baz();
liz();
rep();
console.log("end");
```

결과값은??

`start - foo - bar - end - rep - liz - foo - baz`

- requestAnimationFrame은 setTimeout과 같이 매크로태스크에 배정되지만, rAF는 렌더링 직전에 실행되고, setTimeout은 렌더링 후에 실행되서 실행 우선순위가 다름. (브라우저마다 매크로태스크 함수 실행 순서가 달라 결과가 다르게 나올 수 있음.)
