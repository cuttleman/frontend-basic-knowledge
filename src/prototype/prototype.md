# **Prototype**

1. 자바스크립트는 프로토타입 기반 언어.
2. [[Prototype]]은 객체가 상속할 대상임.<br>
   (a.[[Prototype]] = b 일때, a는 b를 상속한다고 함)
3. \_\_proto\_\_는 object(객체)에서 사용되며 [[Prototype]]용 getter, setter임.
4. 함수는 기본 속성으로 prototype를 가지고,<br>
   그 prototype은 자기자신을 가리키는 contructor 함수를 가짐.<br>
   (constructor는 함수가 가지고 있는 속성임)
5. 함수의 프로토타입 속성은 말그대로 '속성'임.
6. 함수의 프로토타입은 인스턴스를 만들때 [[Prototype]]을 가지게 해줌.
7. 일반 객체에서 인스턴스를 상속받을땐 constructor가 사라지지 않음.
8. 자식과 부모 객체안에 같은 이름의 속성이 있으면 부모객체의 속성은 가려짐.<br>
   (객체의 메소드는 'method overriding'이라는 표현을 씀 - 하위객체가 재정의함)

`생성자 함수를 호출해서 만든 객체를 인스턴스라 부름(붕어빵기계에서 나온 붕어빵)`

ex>

```js
const universe = {
  item: "Earth",
};

function God() {
  this.ability = 999;
}

God.prototype = universe; // constructor 함수가 없어짐.(내가 누군지 몰라)
// solution: God.prototype = {...universe, constructor: God}; 수동 작성
God.prototype.of = universe; // 내가 누군지도 알고 상속도 받음

// 생성자 함수 >> 일반 함수와 기술적으로 같음
// 관례상 첫글자는 대문자로, 반드시 'new' 연산자를 붙여서 실행해야함.
const god = new God();
// {
//  ability:999
//  __proto__:{
//    of: {
//      item: "Earth"
//      },
//    constructor: f God()
//  }
// }
console.log(god.of.item); // Earth
```

<br>

### _Reference_

> 함수의 프로토타입 및 상속[🔗](https://ko.javascript.info/function-prototype)
