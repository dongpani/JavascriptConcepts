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

<br>

## 함수에서의 this 바인딩1

```
var value = 100;

var myObject = {
	value: 0,
	func1: function() {	
		this.value += 1;
		console.log('func1() called. this.value : ' + this.value);   // 1
		
		func2 = function() {
			this.value += 1;
			console.log('func2() called. this.value : ' + this.value);   // 101
			
			func3 = function() {
				this.value += 1;
				console.log('func3() called. this.value : ' + this.value);  // 102
			}
			func3();
		}
		
		func2();
	}
};

myObject.func1();
```

<br>

## 함수에서의 this 바인딩2


```
var value = 100;

var myObject = {
	value: 0,
	func1: function() {	
		var that = this;
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

- 함수내부에서의 this 는 전역객체로 바인딩 된다. 브라우저에서는 window 객체
- inner 함수도 예외는 없음.
- 함수호출 시 비밀리에 인자로 arguments 와 this 를 전달한다.
- 함수 내부에서 this 가 전역객체로 바인딩 되는 것을 막기 위해, this 를 임의적으로 that으로 선언한 후 해당 객체의 프로퍼티만 참조하도록 한다.



## 일반함수에서의 리턴값

```
var noReturnFunc = function() {
	console.log('This function has no return statement.');
};

var result = noReturnFunc();
console.log(result);
```

[결과]
This function has no return statement.
undefined

- 일반함수에서 return 값을 지정하지 않으면 undefined 가 출력된다.

<br>


## 생성자 함수에서의 리턴값 - 객체의 프로퍼티를 리턴값으로 변경.

```
function Person(name, age, gender) {
	this.name = name;
	this.age = age;
	this.gender = gender;

	return {name:'bar', age:20, gender:'woman'};
};

var foo = new Person('foo', 30, 'man');
console.dir(foo);
```

[결과]
Object {name: "bar", age: 20, gender: "woman"}

- 생성자함수는 보통 return 값을 지정하지 않는다.
- 생성자의 객체를 리턴한다.


<br>

## 생성자 함수에서의 리턴값 - 기본타입리턴

```
function Person(name, age, gender) {
	this.name = name;
	this.age = age;
	this.gender = gender;

	return 100;
}

var foo = new Person('foo', 30, 'man');
console.log(foo);
```

[결과]
Person {name: "foo", age: 30, gender: "man"}

- 생성자함수에 기턴을 기본(숫자)형으로 지정하였지만, 객체를 리턴하므로 반환하지 않는다.
