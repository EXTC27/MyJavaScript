# ES6+

> ES6는 ECMA에서 2015년에 채택한 JS 표준이다. ES6 이후 JS에 많은 변화가 왔다.
>
> 1. `const`, `let`
> 2. 객체, 배열의 사용성 개선 (단축 속성명, 계산된 속성명, 전개 연산자, 비구조화 할당)
> 3. 강화된 함수 기능 (매개변수 기본값, 나머지 매개변수, 명명된 매개변수, 화살표 함수)
> 4. 프로미스
> 5. `async`, `await`
> 6. 템플릿 문자열



## 1. `const`, `let`

ES5까지 `var`를 이용해서 변수를 정의했고 유일한 방법이었다. `const`와 `let`은 `var`가 안고 있는 문제들을 해결하기위해 나왔다.

---



### ◆ `var`의 문제

#### 1. 함수 스코프

**스코프**란 변수가 사용될 수 있는 영역을 말한다. `var`는 함수 스코프이기에, 함수를 벗어난 영역에서 사용하면 에러가 발생한다.

```js
// 함수를 벗어나면 사용할 수 없다.
function exam() {
	var i = 1;
}

console.log(i) // 참조 에러
```

`var`를 함수가 아닌 **프로그램의 가장 바깥에 정의하면 전역 변수**가 되는데, 이는 **프로그램 전체를 감싸는 하나의 함수**가 있다고 생각하면 이해하기 쉽다.

특이한 점은 **함수 안에서 `var` 키워드를 사용하지 않고 변수를 선언**하면, 그 변수는 **전역 변수**가 된다.

```js
// 특이한 점
function exam1() {
    i = 1; // var 키워드 없이 선언하여 전역변수가 됨.
}
function exam2() {
    console.log(i);
}
exam1();
exam2(); // 1 출력
```

또한, 함수 스코프이기 때문에 **for 반복문에서 정의된 변수가 반복문이 끝난 이후에도 계속 남는 문제점**이 있다. while, switch, if 문 에서도 마찬가지이다.

```js
// 반복문에서 벗어나도 변수가 사라지지 않는다.
for(var i = 0; i < 10; i++){
    console.log(i);
}
console.log(i); // 10 출력
```

>  `var` 변수의 스코프를 제한하기위해 **'즉시 실행 함수'**를 사용하기도 한다. 즉시 실행 함수는 정의한 시점에서 바로 실행되고 사라지기 때문에 `var`를 즉시 실행 함수로 묶으면 스코프를 제한할 수 있다.
>
> 하지만, 작성하기 **번거롭고 가독성도 떨어져서** 근본적인 해결책이 될 수는 없다.



#### 2. `var`의 호이스팅

`var`로 정의된 변수는 그 **변수가 속한 스코프의 최상단으로 끌어올려진다**. 이를 **호이스팅(hoisting)**이라고 부른다.

```js
// 정의된 시점보다 먼저 변수를 사용해도 에러가 나지 않는다.
console.log(myVar); // undefined 출력 
var myVar = 1;
```

위의 코드에서, 변수를 정의하기 전에 사용했음에도 에러가 발생하지 않는다. 게다가 제대로 작동하지도 않는다. (1이 아니라 undefined를 출력한다.)

이것은 해당 **변수의 정의가 위쪽으로 끌어올려졌기 때문**인데, 밑의 코드처럼 변경됐다고 생각하면 쉽다.

```js
var myVar = undefined;
console.log(myVar);
myVar = 1;
```

변수의 정의만 끌어올려지고, 값은 원래 정의한 위치에서 할당된다. 밑의 코드처럼 변수가 정의된 곳에서 값을 할당 할 수도 있다.

```js
console.log(myVar); // undefined 출력
myVar = 2;
console.log(myVar); // 2 출력
var myVar = 1;
```

**버그 같은데 에러 없이 사용될 수 있다는 점이 단점**이다. 호이스팅은 직관적이지 않으며, 보통의 프로그래밍 언어에서는 찾아보기 힘든 성질이다.



#### 3. 기타 문제들

- **한 번 정의된 변수를 재정의할 수 있다.**

  - ```js
    var myVar = 1;
    var myVar = 2; // 변수 재정의가 가능하다.
    ```

  - 변수를 정의 한다는 것은, 이전에 없던 변수를 생성한다는 의미로 통용된다. 따라서 위의 코드가 에러 없이 사용될 수 있다는 것은 **직관적이지도 않고, 자칫하면 버그**로 이어질 수 있다.  (코드가 길어져서 이전에 선언한 변수를 까먹고 같은 이름으로 새로 생성한다고 했을 때, 서비스 장애로 이어진다.)

- **`var`는 재할당 가능한 변수로밖에 만들 수 없다.**
  
  - 상수처럼 써야하는 경우에도 재할당 가능한 변수로 만들어야한다. 안정적인 면에서 매우 위험하다.

---

### 

### ◆ `const`, `let`으로 해결

#### 1. 블록 스코프

블록 스코프는 대부분의 언어에서 사용하므로 개발자에게 익숙한 개념이다.

```js
if(true){
	const i = 0;
}
console.log(i); // 참조 에러
```

위와 같은 상황에서 에러가 발생하는 것이 **직관적이며, 이해하기도 쉽다**. 

`var`를 사용한다면, if 문 안에서 생성된 변수가 밖에서도 계속 살아있기 때문에, 함수 스코프를 벗어나기 전까지 신경 써서 관리해야 했다.

```js
// 같은 이름을 갖는 변수의 사용 예
let foo = 'bar1';
console.log(foo); // bar1 출력
if(true){
    let foo = 'bar2';
    console.log(foo); // bar2 출력
}
console.log(foo); // bar1 출력
```



#### 2. `const`, `let`의 호이스팅

`const`, `let`도 호이스팅된다. 하지만, `var`와는 다르게 **정의하기 전에 사용하면 참조 에러**가 발생한다.

```js
console.log(foo); // 참조 에러
const foo = 1;
```

> 위와 같은 상황때문에 `const`, `let`은 호이스팅이 되지 않는다고 생각하기 쉽지만, 실제로는 호이스팅이 된다.
>
> 다만, 변수가 **정의된 위치와 호이스팅된 위치 사이에서 변수를 사용하려고 하면 에러가 발생**한다. 이 구간을 **임시적 사각지대 (temporal dead zone)**라고 한다.
>
> ```js
> const foo = 1; // 1번 변수
> {
>     // 호이스팅된 위치는 변수가 속한 스코프의 최상단이다. 
>     // 여기부터, 변수가 정의되는 위치 이전까지, 임시적 사각지대라고 보면 된다.
> 	console.log(foo); // 참조 에러
> 	const foo = 2; // 2번 변수
> }
> ```
>
> 만약 2번 변수가 호이스팅되지 않았다면, 참조 에러는 발생하지 않고, 1번 변수가 출력될 것이다.
>
> 2번 변수의 호이스팅 때문에, console.log 안의 변수는 2번 변수를 참조하게 된다.
> 그리고 console.log()가 임시적 사각지대 안에 있기 때문에 에러가 발생한다.
>
> `var`는 임시적 사각지대가 없다.



#### 3. `const`는 변수를 재할당 불가능하게 만든다

재할당이 불가능한 변수는 프로그램의 복잡도를 상당히 낮춰주기 때문에 되도록이면 재할당 불가능한 변수를 사용하는 게 좋다.

`const` 변수는 **재할당이 불가능**하다. 상수처럼 사용할 수 있다. **재할당 시 에러**가 발생한다. 재할당이 가능하게 할려면 `let`을 사용하면된다. 또한, `const`는 무조건 초기화를 해주어야 한다.

다만, `const`로 정의된 **객체는 내부 속성값은 수정, 추가가 가능**하다. java의 `final`, 다른 언어의 `const`와 같은 방식으로 동작한다.

> 객체 내부의 속성값도 수정 불가능하게 만들고 싶다면 immer, immutable.js 등 외부 패키지를 활용하는 것이 좋다.
>
> 외부 패키지는 객체를 수정하려고 할 때 기존 객체는 변경하지 않고, 새로운 객체를 생성한다.
>
> 새로운 객체를 생성하는 편의 기능이 필요없고, 단지 수정 불가만 하고 싶다면, js 내장 함수를 사용하면 된다.
>
> - Object.preventExtensions
> - Object.seal
> - Object.freeze

---



## 2. 객체와 배열의 사용성 개선

아래와 같은 이유로 코드 작성과 가독성이 좋아졌다.

1. **단축 속성명, 계산된 속성명**을 이용하면 **객체를 생성하고 수정하는 코드를 쉽게 작성**할 수 있다.

2. **전개 연산자, 비구조화 할당**을 통해 객체와 배열의 **속성값을 밖으로 꺼내고 할당하는 것도 쉬워졌다.**

---



### ◆ 단축 속성명

**객체의 리터럴 코드를 간편하게 작성할 목적으로 만들어진 문법**이다.

새로 만들려는 객체의 속성값 일부가 이미 변수로 존재하면, 변수 이름만 적어 주면 된다.

속성값이 함수라면 `function` 키워드 없이 함수명만 적어 주면 된다.

```js
const name = 'SinJ';
const printObj = function(){
	console.log(this);
};

// 단축 속성명을 사용하지 않는 코드
const obj1 = {
    age: 29,
    name: name,
	getName: function(){ return this.name; },
    printObj: printObj,
};

// 단축 속성명을 사용한 코드
const obj2 = {
    age: 30,
    name,
	getName(){ return this.name; },
    printObj,
};
```

---



### ◆ 계산된 속성명

**동적으로 객체의 속성명을 결정**하기 위해 나온 문법이다.

```js
// 계산된 속성명을 사용하지 않은 코드
function makeObj1(key, value){
    const obj = {};
    obj[key] = value;
    return obj;
}

// 계산된 속성명을 사용한 코드
function makeObj2(key, value){
	return { [key]: valeu };
}
```

또한, 계산된 속성명은 **컴포넌트의 상태값을 변경할 때 유용**하다.

```js
class MyComponent extends React.Component {
    state = {
        count1: 0,
        count2: 0,
        count3: 0,
    };

	/* 생략 */

	onClick = idx => {
        const key = `const${idx}`;
        const value = this.state[key];
        this.setState({ [key]: value + 1 }); // 상태값 변경에 유용
    }
}
```

---



### ◆ 전개 연산자

**배열이나 객체의 모든 속성을 풀어 놓을 때 사용**한다. **매개변수가 많은 함수를 호출할 때 유용**하다.

```js
Math.max(1, 3, 7, 9);

const numbers = [1, 3, 7, 9];
Math.max(...numbers); // 동적으로 함수의 매개변수를 전달할 수 있다.
```

>**동적으로 함수의 매개변수를 전달하는 다른 방법**
>
>전개 연산자를 사용하지 않고, 밑과 같이 동적으로 함수의 매개변수를 전달할 수 있다.
>
>```js
>const numbers = [-1, 5, 11, 3];
>Math.max.apply(null, numbers); // apply 함수 사용.
>```
>
>위의 코드는 this 바인딩이 필요하지 않기 때문에 첫 번째 매개변수로 null을 입력하고 있다. 
>
>전개 연산자에 비해 작성이 번거롭고, 가독성도 떨어진다.

전개 연산자는 **배열과 객체를 복사할 때도 유용**하다.

```js
const arr1 = [1, 2, 3];
const obj1 = { age: 29, name: 'SinJ' };

const arr2 = [...arr1];
const obj2 = { ...obj1 };

arr2.push(4);
obj2.age = 80; // 새로운 객체와 배열이 생성되었기 때문에 속성을 추가하거나 변경해도 원래 객체에 영향을 주지 않는다.
```

배열의 경우 전개 연산자를 사용하면 **순서가 유지**된다.

```js
[1, ...[2, 3], 4]; // [1, 2, 3, 4]
new Date(...[2020, 12, 05]); // 2020년 12월 5일
```

서로 **다른 두 배열이나 객체를 쉽게 합칠 수 있다.**

```js
const obj1 = { age: 29, name: 'SinJ' };
const obj2 = { hobby: 'computer' };
const obj3 = { ...obj1, ...obj2 };
```

ES6부터 **중복된 속성명이 허용**된다. **최종 결과는 마지막 속성명의 값**이 된다.

```js
const obj1 = { x: 1, x: 2, y: 'a' }; // { x: 2, y: 'a' }
const obj2 = { ...obj1, y: 'b' };    // { x: 2, y: 'b' }
```

---



### ◆ 비구조화 할당

**배열과 객체의 여러 속성값을 변수로 쉽게 할당**할 수 있다.

#### 1. 배열 비구조화

비구조화 사용시 배열의 **속성값이 왼쪽의 변수에 순서**대로 들어간다.

```js
// 비구조화를 사용한 코드
const arr = [1, 2];
const [a, b] = arr;
console.log(a); // 1 출력
console.log(b); // 2 출력

// 존재하는 변수에 할당
let a, b;
[a, b] = [1, 2];
```

배열 비구조화 시, **기본 값을 정의할 수 있다.** 배열의 속성값이 undefined라면 정의된 기본값이 할당되고, 그렇지 않다면 원래의 속성값이 할당된다.

```js
const arr = [1];
const [a = 10, b = 20] = arr;
console.log(a); // 1 출력
console.log(b); // 20 출력
```

두 변수의 값을 쉽게 교환할 수 있다. 제 3의 변수를 이용하지 않고도 교환이 가능하다.

```js
let a = 1;
let b = 2;
[a, b] = [b, a];
```

배열에서 일부 속성값을 무시하고 싶다면  건너뛰는 개수만큼 쉼표를 입력하면 된다.

```js
const arr = [1, 2, 3];
const [a, , c] = arr;
console.log(a); // 1
console.log(c); // 3
```

쉼표 개수만큼을 제외한 나머지를 새로운 배열로 만들 수 있다.

```js
const arr = [1, 2, 3];
const [first, ...rest1] = arr;
console.log(rest1); // [2, 3]

const [a, b, c, rest2] = arr;
console.log(rest2); // [], 나머지 속성값이 존재하지 않으면 빈 배열이 만들어진다.
```



#### 2. 객체 비구조화

객체 비구조화는 **중괄호를 사용**한다. 배열 비구조화는 순서가 중요했지만, **객체 비구조화는 순서가 무의미**하다. 
대신 **기존 속성명을 그대로 사용**해야한다. 존재하지 않는 속성명을 사용하면 undefined가 할당된다.

```js
const obj = { age: 29, name: 'SinJ' };
const { age, name, hobby } = obj;
console.log(age); // 29
console.log(name); // 'SinJ'
console.log(hobby) // undefined
```

객체 비구조화에서는 **속성명과 다른 이름으로 변수를 생성**할 수 있다.  대신 기존의 속성명은 사라진다.
**중복된 변수명을 피하거나, 구체적인 변수명을 만들 때 좋다.**

```js
const obj = { age: 29, name: 'SinJ' };
const { age: theAge, name } = obj;
console.log(theAge); // 29
console.log(age);    // 참조 에러
```

객체 비구조화에서도 **기본값을 정의**할 수 있다. **속성값이 undifined인 경우에는 기본값이 할당**된다.
하지만, **속성값이 `null`이면 기본값이 할당되지 않는다.**

```js
const obj = { age: undefined, name: 'SinJ', hobby: null };
const { age = 20, name = 'noName', hobby = 'noHobby' } = obj;
console.log(age); // 20
console.log(name); // 'SinJ'
console.log(hobby) // null

// 기본값을 정의하면서 별칭을 함께 사용할 수 있다.
const obj = { age: undefined };
const { age:theAge = 20 } = obj;
```

기본 값으로 함수 반환값을 넣을 수 있다. 한 가지 재미있는 건 **기본값이 사용될 때만 함수가 호출된다는 점**이다.

```js
function getDefaultAge(){
    console.log('hello');
    return 0;
}
function getDefaultName(){
    console.log('ES6');
    return 'noName';
}

const obj = { age: 29, name: undefined };
const { age = getDefaultAge(), name = getDefaultName() } = obj;
// age는 undefined가 아니므로 기본값이 사용되지 않는다. 따라서 'hello'는 출력되지않는다.
// name은 기본값이 사용되므로 'ES6'가 출력된다.
```

**사용되지 않은 나머지 속성들을 별도의 객체로 생성**할 수 있다.

```js
const obj = { age: 29, name: 'SinJ', hobby: 'computer' };
const { age, ...rest } = obj;
console.log(rest); // {name: 'SinJ', hobby: 'computer' }
```

**for 문에서 객체를 원소로 갖는 배열을 순회할 때 편리**하다.

```js
const people = [
    { age: 29, name: 'SinJ', hobby: 'computer' },
    { age: 20, name: 'John', hobby: 'soccer' }
];

for(const { age, name } of people){
    /* 생략 */
}
```



#### 3. 기타 특성

**기본값의 정의**는 변수로 한정되지 않는다. **패턴 단위로도 적용**된다.

```js
const [{ prop: x } = { prop: 123 }] = [];
console.log(x); // 123
const [{ prop: x } = { prop: 123 }] = [{}];
console.log(x); // undefined
```

**객체 비구조화도 계산된 속성명을 활용**할 수 있다.

```js
const idx = 1;
const { [`key${idx}`]: valueOfIdx } = {key1: 123};
console.log(valueOfIdx); // 123
```

별칭에 단순한 변수명만 입력할 수 있는 것은 아니다. 밑에 코드를 보면 obj 객체의 prop이라는 속성과 배열의 첫 번째 원소에 값을 할당하고 있다.

```js
const obj = {};
const arr = {};
({ foo: obj.prop, bar: arr[0] } = { foo: 123, bar: true });
console.log(obj); // {prop: 123}
console.log(arr); // [true]
```

---



## 3. 강화된 함수의 기능

1. **매개변수에 기본값**을 줄 수 있게 되었다.
2. **나머지 매개변수**를 통해, **가변 길이 매개변수를 좀 더 명시적으로 표현**할 수 있게 되었다.
3. **명명된 매개변수**를 통해, **함수를 호출하는 코드의 가독성**이 좋아졌다.
4. **화살표 함수**를 통해, **코드가 간결**해지고, **this 바인딩에 대한 고민을 덜 수 있게 되었다.**

---



### ◆ 매개변수 기본값

**매개변수가 undefined이면, 기본값이 적용**된다.

```js
function printLog(a = 1){
	console.log({ a });
}

printLog(); // { a: 1 }
```

객체 비구조화 처럼, **기본값으로 함수 호출을 넣을 수 있고, 기본값이 필요한 경우에만 함수가 호출**된다.

```js
function getDefault(){
    return 1;
}

function printLog(a = getDefault()){
    console.log({ a });
}

printLog(); // { a: 1 }
```

>**매개변수 기본값을 활용하여 필숫값을 표현하는 고오급 스킬!**
>
>```js
>function required(){
>    throw new Error('no parameter');
>}
>
>function printLog(a = required()){
>    console.log({ a });
>}
>
>printLog(10); // 10
>printLog(); // 매개변수가 없어서, 기본값인 required 함수가 호출되고, 예외를 발생시킨다.
>```

---



### ◆ 나머지 매개변수

입력된 인수 중에서 **정의된 매개변수 개수만큼을 제외한 나머지를 배열**로 만들어 준다.
나머지 매개변수는 **매개변수 개수가 가변적일 때 유용**하다.

```js
function printLog(a, ...rest){
    console.log({ a, rest });
}

printLog(1, 2, 3); // { a: 1, rest: [2, 3] }
```

>ES5에서는 arguments 키워드가 나머지 매개변수와 비슷한 역할을 한다.
>
>```js
>function printLog(a){
>    const rest = Array.from(arguments).split(1);
>    console.log({ a, rest });
>}
>
>printLog(1, 2, 3); // { a: 1, rest: [2, 3] }
>```
>
>매개변수에서 arguments의 존재가 명시적으로 드러나지 않기 때문에 가독성이 안좋다.
>
>또한, arguments는 배열이 아니기 때문에, 배열처럼 사용하기 위해서는 변환 과정이 필요하다.
>
>따라서 나머지 매개변수를 사용한 방식이 더 낫다.

---



### ◆ 명명된 매개변수

**객체 비구조화를 이용해서 구현**할 수 있다. 명명된 매개변수를 사용하면 **함수 호출 시 매개변수의 이름과 값을 동시에 적을 수 있으므로 가독성이 높다.**

```js
const number = [10, 20, 30, 40];
const result1 = getValues(numbers, 5, 25);
const result2 = getValues({ numbers, greaterThan: 5, lessThan: 25 }); // 명명된 매개변수
```

명명된 매개변수를 이용하면 **선택적 매개변수의 활용도가 올라간다.** 필숫값과 반대로, 있어도 되고 없어도 되는 매개변수를 선택적 매개변수라고 한다.

```js
const result1 = getValues(numbers, undefined, 25); // 1번
const result2 = getValues({ numbers, greaterThan: 5 }); // 2번
const result3 = getValues({ numbers, lessThan: 5 }); // 3번
```

1번 코드는 명명된 매개변수 없이 선택적 매개변수를 사용한 예이다. 필요없는 매개변수 자리에 undefined를 넣으면 된나, 이 방식은 매개변수가 많아지면 관리가 힘들어진다.

2번, 3번은 명명된 매개변수를 사용했다. 필요한 인수만 넣어 주면 되기 때문에 매개변수가 늘어나도 별 문제없이 사용할 수 있다.

>명명된 매개변수를 사용하면 함수를 호출할 때마다 객체가 생성되기 때문에 비효율적일 것이라고 생각할 수 있다.
>
>하지만, JS 엔진이 최적화를 통해 새로운 객체를 생성하지 않으므로 안심하고 사용해도 된다.

---



### ◆ 화살표 함수

일단 화살표 함수는 함수를 간결하게 작성할 수 있도록 해준다.

```js
const addAndReturnObj = (a, b) => ({ result: a + b }); // 객체를 리턴한다면 ()로 묶자
```

**화살표 함수가 일반 함수와 다른 점은 `this`와 `arguments`가 바인딩 되지 않는다는 점이다.** 따라서 화살표 함수에서 `arguments`가 필요하다면 나머지 매개변수를 이용한다.

```js
const printLog = (...rest) => console.log(rest);
printLog(1, 2); // [1, 2]
```



#### **1. 일반 함수에서 `this` 바인딩 때문에 버그가 발생하는 경우**!

일반 함수에서 `this`는 **<u>호출 시점에 사용된 객체로 바인딩</u>** 된다. 따라서 **객체에 정의된 일반 함수를 다른 변수에 할당해서 호출하면 버그가 발생**할 수 있다.

```js
const obj = {
    value: 1,
    increase: function(){
        this.value++;
    }
};

obj.increase(); // obj 객체가 this에 바인딩되므로 obj.value가 증가한다.
console.log(obj.value); // 2

const increase = obj.increase; // 객체 없이 호출되는 경우에는 전역 객체가 바인딩된다.
// 브라우저 환경에서는 window, Node 환경에서는 global 객체가 바인딩된다.
increase(); // 따라서 obj.value가 증가하지 않는다.
console.log(obj.value); // 2
```

화살표 함수 안에서 사용된 this와 arguments는 자신을 감싸고 있는 가장 가까운 일반 함수의 것을 참조한다.



#### 2. 생성자 함수 내부에서 정의된 화살표 함수의 this!

**생성자 함수 내부에서 정의된 화살표 함수의 `this`는 생성된 객체를 참조**한다.

```js
function Something(){
	this.value = 1;
    this.increase = () => this.value++; // 화살표 함수의 this는 가장 가까운 일반 함수인 Something의 this를 참조한다.
}

const obj = new Something(); // Something 함수는 생성자이고 obj 객체가 생성될 때 호출된다.
// new 키워드를 이용해서 생성자 함수를 호출하면 this는 생성되는 객체인 obj를 참조한다.
obj.increase(); // 따라서 호출 시점의 객체와는 무관하게 increase 함수의 this는 항상 생성된 객체를 참조한다.
console.log(obj.value); // 2

const increase = obj.increase;
increase(); // 따라서 호출 시점의 객체와는 무관하게 increase 함수의 this는 항상 생성된 객체를 참조한다.
console.log(obj.value); // 3
```



#### 3. setInterval 함수 사용 시 this 바인딩 문제!

다음 코드에서 `this`가 어떤 객체를 참조할지 생각해보자.

```js
function Something(){
  this.value = 1;
  setInterval(function increase(){
    this.value++;
  }, 1000);
}

const obj = new Something();
```

`setInterval` 함수의 인수로 들어간 increase 함수는 **전역 환경에서 실행**되기 때문에 `this`는 `window` 객체를 참조한다. 따라서 value는 증가하지 않는다.

ES5에서는 위와 같은 문제를 해결하기위해 다음과 같은 **편법**을 썼다. **화살표 함수를 쓰면 이제 이렇게 안해도 된다.**

```js
function Something(){
  this.value = 1;
  var that = this;
  setInterval(function increase(){
    that.value++; // increase 함수에서 클로저를 이용해서 미리 저장해둔 that 변수를 통해 this 객체에 접근한다.
  }, 1000);
}

const obj = new Something();

// 화살표 함수로 바꾸면 다음과 같다.
function Something(){
    this.value = 1;
    setInterval(() => {
        this.value++;
	}, 1000);
}
```

> **클로저란 무엇인가?**
>
> 클로저는 **함수가 생성되는 시점에 접근 가능했던 변수들을 생성 이후에도 계속해서 접근할 수 있게 해 주는 기능**이다. **접근할 수 있는 변수는 그 함수를 감싸고 있는 상위 함수들의 <u>매개변수</u>와 <u>내부 변수</u>**들이다.
>
> ```js
> function makeAddFunc(x){
>   return function add(y) { // add 함수는 상위 함수인 makeAddFunc의 매개변수 x에 접근할 수 있다.
>     return x + y;
>   };
> }
> 
> const add5 = makeAddFunc(5);
> console.log(add5(1)); // 6, add5 함수가 생성된 이후에도 상위 함수를 호출할 때 사용했던 인수 5에 접근할 수 있다.
> 
> const add7 = makeAddFunc(7); // 중간에 makeAddFunc(7)이 호출되지만, add5에 영향을 주지는 않는다. 생성된 add 함수별로 클로저 환경이 생성된다.
> console.log(add7(1)); // 8
> console.log(add5(1)); // 6
> ```

---



## 4. 프로미스

프로미스는 **비동기 상태를 값으로 다룰 수 있는 객체**이다. 비동기 프로그래밍을 할 때, 동기 프로그래밍 방식으로 코드를 작성할 수 있다.

> 프로미스 이전에는 콜백 패턴이 많이 쓰였으나, 현재는 몇 가지 프로미스 라이브러리가 등장하면서 프로미스를 사용하는 개발자가 많아졌고, 이제는 여러 라이브러리의 비동기 함수가 프로미스를 반환할 만큼 널리 쓰인다.

---



### ◆ 콜백 패턴의 문제

콜백 패턴은 콜백이 조금만 중첩돼도 **코드가 상당히 복잡**해진다.

**코드의 흐름이 순차적이지 않기 때문에, 코드를 읽기가 상당히 힘들다.**

프로미스를 사용하면 코드가 순차적으로 실행되게 작성할 수 있다.

---



### ◆ 프로미스의 세 가지 상태

프로미스는 다음 세 가지 상태 중 하나의 상태로 존재한다.

1. **대기중(pending)**: 결과를 기다리는 중.
2. **이행됨(fulfilled)**: 수행이 정상적으로 끝났고, 결과값을 갖고 있음.
3. **거부됨(rejected)**: 수행이 비정상적으로 끝남.

이행과 거부 상태를 **처리됨(settled)** 상태라고 부른다. 프로미스는 **처리됨 상태가 되면 더 이상 다른 상태로 변경되지 않는다.**

---



### ◆ 프로미스를 생성하는 법

프로미스는 세 가지 방식으로 생성할 수 있다.

```js
// 1번
const p1 = new Promise((resolve, reject) => {
	//resolve(data);
    //reject(data);
});

// 2번
const p2 = Promise.reject('error msg');

// 3번
const p3 = Promise.resolve(param);
```

- 1번

  - 일반적으로 `new` 키워드를 사용해서 프로미스를 생성한다. 이 방법으로 생성된 프로미스는 **대기중 상태**가 된다.

  - 생성자에 입력되는 함수는 `resolve`와 `reject` 콜백 함수를 매개변수로 갖는다. 비동기로 어떤 작업을 수행 후 성공했을 때 `resolve`, 실패했을 때 `reject`를 호출하면 된다.

    `resovle`를 호출하면 이행상태가 되고, `reject`를 호출하면 거부상태가 된다.

  - `new` 키워드를 사용해서 프로미스를 **생성하는 순간 생성자의 입력함수가 실행**된다. 만약 API 요청을 보내는 비동기 코드가 있다면 프로미스가 **생성되는 순간 요청**을 보내면 된다.

- 2번

  - `Promise.reject`를 호출하면 거부됨 상태인 프로미스가 생성된다.

- 3번

  - Promise.resolve를 호출할 때, 만약 입력값이 프로미스 였다면 그 객체가 그대로 반환되고,  프로미스가 아니라면 이행됨 상태인 프로미스가 반환된다.

    ```js
    //Promise.resolve의 반환값
    const p1 = Promise.resolve(123);
    console.log(p1 !== 123); // true
    // 123은 프로미스가 아닌 인수이기 때문에, 그 값 그대로 이행됨 상태인 프로미스가 반환된다.
    // p1은 123을 데이터로 가진 프로미스다.
    
    const p2 = new Promise(resolve => {
      setTimeout(() => resolve(10), 1);
    });
    console.log(Promise.resolve(p2) === p2); // true
    // 프로미스가 입력되면, 입력된 자신이 반환된다.
    ```

---



### ◆ .then()

`then`은 처리됨 상태가 된 프로미스를 처리할 때 사용되는 메서드다. 프로미스가 처리됨 상태가 되면 `then()` 메서드의 인수로 전달된 함수가 호출된다.

```js
requestData().then(onResolve, onReject); // 처리됨 상태가 되면 onResolve가 호출되고, 거부됨 상태가 되면 onReject가 호출된다.
Promise.resolve(123).then(data => console.log(data)); // 123
Promise.reject('err').then(null, error => console.log(error)) // 에러 발생
```

`then()` 메서드는 항상 프로미스를 반환한다. 따라서 하나의 프로미스로부터 연속적으로 `then()` 메서드를 출력할 수 있다.

만약 프로미스가 아닌 값을 반환하면 then 

