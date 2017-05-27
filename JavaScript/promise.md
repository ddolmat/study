# promise
thenable syntax코드 구조를 사용해 비동기 코드를 마치 동기로 작동되는 코드처럼 만들 수 있다.
then메소드에 의해 실행되는 콜백함수는 resolve나 reject메소드의 실행이후에야 실행이 된다. 즉 비동기콜백이 끝날때  resolve나 reject메소드를 실행해주면, then의 콜백함수를 실행 시킬 수 있다. promise를 사용하면 비동기 로직의 코드 가독성이 좋아진다.

아래는 MDN에 있는 슈퍼심플 예제코드이다.
```JavaScript
let myFirstPromise = new Promise((resolve, reject) => {
  setTimeout(function(){
    resolve("Success!"); // Yay! Everything went well!
  }, 250);
});

myFirstPromise.then((successMessage) => {
  console.log("Yay! " + successMessage);
});
```

resolve와 then만 간략하게 구현해 보면 대략 이런형태 이지 싶다.
```JavaScript
const MyPromise = function(promiseCallback){
	this.then = function(thenCallback) {
		this.thenCallback = thenCallback;
	};

	this.resolve = function(arg) {
		setTimeout(this.thenCallback(arg), 0);
	}.bind(this);

	promiseCallback(this.resolve);
};
```

