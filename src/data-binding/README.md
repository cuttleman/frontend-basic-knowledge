# **Data Binding**

1. 데이터 바인딩은 화면에 보이는 데이터와 브라우저 메모리에 있는 데이터를 일치시키는 기법.  
   (브라우저 메모리 -> 여러 자바스크립트 객체 등)
2. MVC 모델에서 model과 view를 묶어서 모델과 뷰를 자동 동기화 시키기라고 볼 수 있음.

## **2 ways Data Binding**

- 컨트롤러에서 model이 변경됨 -> view변경됨
- view에서 scope model이 변경됨 -> 컨트롤러에서 model이 변경됨
- HTML -> JS, JS -> HTML 양쪽 모두 가능
- 데이터의 변경을 프레임워크에서 감지하고 있다가, 데이터가 변경되는 시점에 DOM 객체에 렌더링을 해주거나 페이지 내에서 모델의 변경을 감지해 JS 실행부에서 변경.
- 사용자의 입력값이 곧바로 코드 상 변수에 바인딩 됨.(가능함)

ex>

```html
<body>
  <form id="form">
    <input type="text" placeholder="try it" v-model="input" />
    <p>{{input}}</p>
  </form>
  <script>
    new Vue({
      el: "#form",
      data: {
        input: "",
      },
    });
  </script>
</body>
```

<br>

## **1 ways Data Binding**

- 데이터와 템플릿을 결합해 화면을 생성하는 것.(JS -> HTML만 가능)
- 입력값을 곧바로 데이터상에 적용하지 않고, 이벤트를 걸어 데이터를 받아온 후 화면에 뿌려줌.
- 사용자의 입력값(데이터)이 event를 통해서 코드 상 변수에 담김.

ex>

```html
<body>
  <form id="form">
    <input type="text" placeholder="try it" />
    <p></p>
  </form>
  <script>
    const p = document.querySelector("p");
    const form = document.getElementById("form");

    form.addEventListener("keyup", function (e) {
      p.innerText = e.target.value;
    });
  </script>
</body>
```

<br>

### _Reference_

> 데이터 바인딩(단방향, 양방향)[🔗](https://authorkim0921.tistory.com/13#:~:text=%EB%8D%B0%EC%9D%B4%ED%84%B0%20%EB%B0%94%EC%9D%B8%EB%94%A9%20%EC%9D%B4%EB%9E%80%20%EB%91%90%20%EB%8D%B0%EC%9D%B4%ED%84%B0,%EB%AA%A8%EB%91%90%20%EC%9D%BC%EC%B9%98%EC%8B%9C%ED%82%A4%EB%8A%94%20%EA%B8%B0%EB%B2%95%EC%9D%B4%EB%8B%A4.&text=%EC%96%91%EB%B0%A9%ED%96%A5%20%EB%B0%94%EC%9D%B8%EB%94%A9%EC%9D%98%20%EA%B2%BD%EC%9A%B0%2C%20%EC%82%AC%EC%9A%A9%EC%9E%90,%EC%97%90%20%EB%8D%B0%EC%9D%B4%ED%84%B0%20%EA%B0%92%EC%9D%B4%20%EB%8B%B4%EA%B8%B4%EB%8B%A4.)
