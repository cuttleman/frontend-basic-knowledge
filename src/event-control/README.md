# **Event Control**

1. 이벤트 제어는 이벤트 리스너에 등록되는 콜백함수안에서 처리하는 기능들을 제어하는 기술.
2. 스크롤 이벤트를 감지해서 무한스크롤을 구현하거나, input 요소에 타이핑을 감지해서 api요청을 하는 경우<br>
   이벤트 제어를 하지 않을 경우 무리한 서버요청, 성능저하 이슈를 발생시킬 수 있음.

## Throttle vs Debounce

1. throttle 은 정해둔 시간이 되면 기능을 활성화 시킴.
2. debounce 는 정해둔 시간이 되기전에 이벤트가 다시 호출 될 시에 시간은 초기화시키고, 시간이 다 됐을 경우에만 기능이 활성화 됨.

```js
let throttle: any = null;

const onThrottle = (e: any) => {
  if (!throttle) {
    setTimeout(() => {
      console.log("throttle: ", e.target.value);
      throttle = null;
    }, 1000);
  }
  throttle = true;
};

let debounce: any = null;

const onDebounce = (e: any) => {
  clearTimeout(debounce);
  debounce = setTimeout(() => {
    console.log("debounce: ", e.target.value);
  }, 1000);
};
```

### Pros

1. 서버에 요청하는 회수를 제어할 수 있음.<br>
   (ex> "안녕하세요" 입력시 제어하지 않았을 경우 12번의 서버요청을 하게 되고, debounce로 이벤트 제어를 했을 경우엔 1번의 서버요청을 하게 됨.)
2. 성능 이슈를 예방할 수 있음.<br>
   (ex> 스크롤 이벤트를 감지해서 특정 기능을 구현 하고 있을 때 매 이벤트마다 기능을 활성화 시키기 때문에 성능이 떨어질 수 있음.<br>
   이벤트 제어를 하게되면 스크롤 이벤트 끝나는 시점에 기능을 활성화 해서 성능에 이슈가 생기는 문제를 예방 할 수 있음.)
