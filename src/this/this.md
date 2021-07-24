# **This**

1. This는 함수 호출 방법에 따라 결정됨.
2. `strict mode`와 `non-strict mode`에서 다르게 바인딩 됨.  
   (엄격 - undefined, 비엄격 - 글로벌객체)
3. 일반함수 내부의 this는 글로벌 객체가 바인딩됨.
4. 객체의 메소드 내부의 this는 해당 객체가 바인딩됨.(메소드가 호출될 때 . 앞에 있는 객체를 바인딩함)
5. 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스가 바인딩됨.
6. this는 하위 함수에 상속되지 않음.
7. (ES6) arrow 함수에서는 글로벌 객체대신 상위 함수의 this를 가져옴.
8. Call, Apply, Bind 메소드 사용시, 첫번째 인수로 들어가는 객체가 바인딩 됨.
9. Call, Apply 메소드를 사용하면 유사배열 객체인 arguments에 **배열 메소드**를 사용 할 수 있음.<br> (ex> Array.prototype.join.call(arguments) )

```js
// new 키워드 없이 생성자 함수를 호출할 경우
function Sample(arg1, arg2) {
  this.arg1 = arg1;
  this.arg2 = arg2;
  console.log(this);
}

Sample("te", "st"); // {...windowObject, arg1:"te", arg2:"st"}
```

## **Call**

`Func.call(thisArg, [arg,[arg,[arg...]]])`

```js
const obj = {
  name: "user",
  age: 30,
  func: function () {
    console.log(this.name, this.age);
  },
  arr: function (a, b, c) {
    console.log(arguments.join()); // TypeError
    console.log(Array.prototype.join.call(arguments, "")); // 123
  },
};

obj.func(); // "user" 30

obj.func.call({ name: "hong", age: 29 }); // "hong" 29

obj.arr(1, 2, 3);
```

## **Apply**

`Func.apply(thisArg, [argsArray])`

```js
const obj = {
  name: "user",
  age: 30,
  func: function (num) {
    console.log(this.name, this.age + num);
  },
  arr: function (a, b, c) {
    // arguments는 유사배열 객체임.
    console.log(arguments.join()); // TypeError
    console.log(Array.prototype.join.apply(arguments, [""])); // 123
  },
};

obj.func(2); // "user" 32

// 인수를 배열형태로 넣어야함.
obj.func.apply({ name: "hong", age: 29 }, [3]); // "hong" 32

obj.arr(1, 2, 3);
```

## **Bind**

`Func.bind(thisArg, [arg,[arg,[arg...]]])`

```js
const obj = {
  name: "user",
  age: 30,
  func: function (num) {
    console.log(this.name, this.age + num);
  },
};

obj.func(2); // "user" 32

const bindFunc = obj.func.bind({ name: "hong", age: 29 }, 3);
/* bind 메소드는 함수를 호줄하지 않고 새로 바인딩한 함수를
 * 반환하기 때문에 변수에 할당하여 함수를 호출해야함.
 */
bindFunc(); // "hong" 32
```

call과 bind은 기본적으로 같은 역할을 하는 메소드인데,<br>
call은 함수를 호출하고, bind는 함수를 호출하지 않음.
