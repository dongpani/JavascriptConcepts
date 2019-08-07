# JavascriptConcepts
자바스크립트 핵심 개념

## 클로저

```
function parent()  {
	var a = 100;
	
	var child = function() {
		console.log(a);
	}
	
	return child;
};

var inner = parent();
inner();
```

- 기본적으로 유효범위(scope)따라 함수 안에 선언된 변수에는 접근할 수 없는게 원칙이다.
- 하지만 함수내부에서 선언된 변수의 값을 리턴하게 되면 그 값을 참조할 수 있다.
- inner() 같이 이미 실행이 종료 된 함수 스코프에 변수를 참조하는 것을 <b>클로저<b/> 라고 부른다.
  
<br>
  
## arguments객체

```
function add(a, b) {
	console.dir(arguments);
	return a+b;
}

console.log(add(1));  // NaN
console.log(add(1,2)); // 3
console.log(add(1,2, 3)); // 3
```

- 함수의 인자값을 객체형태로 읽을 수 있다.
- js 에서 매개변수에 맞게 호출을 하지 않아도 오류가 발생하지 않는다.

<br>

## 객체, 메서드 호출의 this 바인딩

```
var myObject = {
	name: 'foo',
	sayName: function() {
		console.log(this.name);
	}
};

var otherObject = {
	name: 'bar'
};

otherObject.sayName = myObject.sayName;

myObject.sayName(); 	// foo
otherObject.sayName();  // bar
```

- 객체 안에 선언된 함수(메서드)의 this 는 자신이 속한 객체를 의미한다.


## 함수에서의 this 바인딩


```
var myObject = {
	value: 0,
	func1: function() {	
		that = this;
		this.value += 1;
		console.log('func1() called. this.value : ' + this.value);   // 1
		
		func2 = function() {
			that.value += 1;
			console.log('func2() called. this.value : ' + that.value);   // 2
			
			func3 = function() {
				that.value += 1;
				console.log('func3() called. this.value : ' + that.value);  // 3
			}
			func3();
		}
		
		func2();
	}
};

myObject.func1();
```

