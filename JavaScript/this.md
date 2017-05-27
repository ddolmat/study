# this

javascript에서의 this의 값은 함수를 호출하는 방법에 의해 결정된다. 실행하는 동안의 할당(assignment)에 의해 설정 될 수 없고, 함수가 호출될 때 마다 다를 수 있다. 

## Global context(전역 컨텍스트)
전역 실행 컨텍스트(어떤 함수의 외부)에서는, this는 strict모드인지 아닌지에 관계없이 전역 객체를 나타낸다.
크롬개발자 도구의 콘솔에서 아래와 같이 입력해보면 다음과 같이 나온다. 웹 브라우저의 전역 객체는 window임을 알 수 있다.
```JavaScript
console.log(this);
//Window {stop: function, open: function, alert: function, confirm: function, prompt: function…}

this.name = "ddolmat";
console.log(window.name); //ddolmat
```
## Function context

### 간단한 호출
함수 내부에서의 this는 함수를 호출한 방법에 의해 결정된다.
```JavaScript
function func1(){
	return this;
}

console.log(func1());
//Window {stop: function, open: function, alert: function, confirm: function, prompt: function…}
```
위의 코드의 경우는 strict모드가 아니기 때문에 this의 값은 전역객체인 window가 된다. 이 경우 this의 값은 호출에 의해 설정되지 않는다.

### call 과 apply
call 과 apply의 첫번째 argument는 호출하는 함수의 this가 된다.
call은 두번째 argument부터 호출되는 함수에 parameter로 전달되고 apply의 두번째 argument는 호출되는 함수의 parameter들의 배열이지만 여기서는 this를 다루는게 목적이므로 생략함.
```JavaScript
function printName(){
	console.log(this.name);
}

var ddolmat = { name:"ddolmat"};

printName.call(ddolmat); //ddolmat
printName.apply(ddolmat); //ddolmat
```

### Object method
함수가 객체의 메소드로 호출되었을 경우 this는 메소드가 호출된 객체를 가리킴. 생성자 함수나 프로토타입을 이용해서 객체를 선언한 경우에도 메소드가 각 객체에 있는것처럼 this는 메소드가 호출된 객체를 나타냄.
```JavaScript
var ddolmat = {
	name: "ddolmat",
	printName: function(){
		console.log(this.name);
	} 
};

this.name = "window";
ddolmat.printName(); //ddolmat;
```

### DOM event Handler
함수가 이벤트 핸들러로 사용될 때 this는 이벤트가 발생한 엘리먼트로 설정된다.(브라우저에 따라 아닌경우도 있다.)

```JavaScript
var body = document.querySelector("body");

body.addEventListener("click", function(){
	console.log(this.tagName); // BODY
});
```

## Async
비동기로 호출되는 함수에서는 객체의 메소드 안에서 불렸다고 하더라도 this가 해당객체를 나타내지 않음.
```JavaScript
var ddolmat = {
	name: "ddolmat",
	printName: function(){
		console.log(this.name);
	},
	printNameAsync: function(){
		//비동기함수 setTimeout
		setTimeout(function(){
			console.log(this.name);
		}, 100);
	}
};

ddolmat.printName(); // ddolmat
ddolmat.printNameAsync(); // 아무것도 찍히지 않음
```

위의 예제에서 printNameAsync메소드에서 printName과 동일한 결과값이 나오기 위해서는 비동기 함수인 setTimeout의 콜백함수의 컨택스트를 강제해줄 필요가 있다. 방법은 다음과 같다.

### bind메소드를 사용
```JavaScript
var ddolmat = {
	name: "ddolmat",
	printName: function() {
		console.log(this.name);
	},
	printNameAsync: function(){
		setTimeout(function(){
			console.log(this.name);
		}.bind(this), 100);
	}
};

ddolmat.printNameAsync(); // ddolmat
``` 

### ES6의 arrow function사용
```JavaScript
var ddolmat = {
var ddolmat = {
	name: "ddolmat",
	printName: function() {
		console.log(this.name);
	},
	printNameAsync: function(){
		setTimeout(() => {
			console.log(this.name);
		}, 100);
	}
};

ddolmat.printNameAsync(); // ddolmat
``` 

### that사용
```JavaScript
var ddolmat = {
	name: "ddolmat",
	printName: function() {
		console.log(this.name);
	},
	printNameAsync: function(){
		var that = this;
		setTimeout(function(){
			console.log(that.name);
		}, 100);
	}
};

ddolmat.printNameAsync(); // ddolmat
```