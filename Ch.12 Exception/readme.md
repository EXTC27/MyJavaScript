# 12장. 예외 처리

## 12.1 개요

프로그램이 실행되는 동안 오류가 발생하면 프로그램이 자동으로 중단된다.
이때 프로그램이 대처할 수 있게 처리하는 것을 예외 처리라고 한다.

---



## 12.2 기본 예외 처리

예외가 발생하지 않게 사전에 해결하는 것을 **기본 예외 처리**라고 부른다.
대부분의 예외처리는 **기본 예외 처리**로 처리할 수 있다.

보통 조건문을 사용한다.

```html
<!-- [12-01] -->
<!-- 이벤트 연결의 예외 처리 -->
<script>
    function registerEventListener(node, event, listener){
        if(node.addEventListener){
            node.addEventListener(event, listener, false);
        }
        else if(node.attachEvent){
            node.attachEvent(evnet, listener);
        }
    }
</script>
```

```html
<!-- [12-02] -->
<!-- 이벤트 객체의 예외 처리 -->
<script>
    function whenClick(e){
        var event = e || window.event;
        var willAlert = '';
        willAlert += `clientX:  + ${event.clientX} + \n`;
        willAlert += `clientY:  + ${event.clientY} + \n`;
        alert(willAlert);
    };
</script>
```

---



## 12.3 고급 예외 처리

**고급 예외 처리**는 `try` `catch` `finally` 키워드로 예외를 처리하는 방법이다.

- `try` : 예외가 발생할 거 같은 부분은 `try` 구문 안에 작성한다.
- `catch` : `try` 구문안의 코드에서 에러가 발생하면 `catch` 구문 안의 코드가 실행된다.
- `finally` : 예외가 발생하든 안 하든 무조건 실행되는 구문이다.

```html
<!-- [12-05] -->
<!-- try catch finally -->
<script>
    try{
        willExcept.byebye();
    }
    catch(exception){
        alert('예외가 발생했습니다.');
    }
    finally{
        alert('예외 발생 가능 부분을 통과했습니다.');
    }
</script>
```



이벤트 리스너의 추가도 **고급 예외 처리** 기법으로 처리할 수 있다.

```html
<!-- [12-06] -->
<!-- try catch를 사용한 이벤트 연결 -->

<script>
    function registerEventListener(node, event, listener){
        try{
            //최신 버전의 웹 브라우저
            node.addEventListener(event, listener, false);
        }
        catch(exception){
            //구버전의 익스플로러
            node.attachEvent('on' + event, listener);
        }
    }
    window.onload = function(){
        var header = document.getElementById('header');
        
        registerEventListener(header, 'click', function(){
            alert('Click');
        });
    };
</script>

<body>
    <h1 id="header">Click</h1>
</body>
```

보통 기본 예외 처리가 더 빠른데 요즘은 컴퓨터 성능이 좋아서 별 차이 없다.

---



## 12.4 예외 객체

`catch`의 괄호 안에 입력하는 식별자가 **예외 객체**이다.
일반적으로 `e`나 `exception` 식별자를 사용한다.

예외 객체의 속성은 브라우저마다 다르다. 그러나 모든 브라우저가 갖고있는 공통 속성이 있다.

- `message` : 예외 메시지
- `description` : 예외 설명
- `name` : 예외 이름

```html
<!-- [12-08] -->
<!-- 예외 객체의 속성 -->
<script>
    try{
        var array = new Array(9999999999);
    }
    catch(exception){
        var output = '';
        for(var i in exception){
            output += i + ': ' +  exception[i] + '\n'; 
        }
        alert(output);
    }
</script>
```

웹 브라우저에 따라서 추가 속성이 나올 수 있다.

JS는 다른 언어에 비해 예외 객체를 많이 활용하지는 않는다.

---



## 12.5 에러와 예외

`try` `catch` 구문으로 해결할 수 있으면 예외, 없으면 에러

예외 처리를 한다고 모든 문제를 해결할 수 있는 것은 아니다.

---



## 12.6 예외 강제 발생

필요한 경우 예외를 강제로 발생시켜야 하는 상황이있다.

예외를 강제로 발생시킬 때는 `throw` 키워드를 사용한다.

```html
<!-- [12-10] -->
<!-- throw -->
<script>
    throw 'CUSTOM EXCEPTION OCCUR';
</script>
```

예외를 강제로 발생시키는 이유는 객체를 사용하는 사용자에게 주의를 줄 수도 있고, 예외와 관련된 처리를 해달라고 부탁할 수도 있기 때문이다.

```html
<!-- [12-12] -->
<!-- 적절한 강제적 예외 발생과 처리 -->
<script>
    //나눗셈 함수
    function divide(alpha, beta){
        if(beta == 0){
            //0으로 나눌경우 강제적으로 예외를 발생시킨다.
            throw 'DivideByZeroException';
        }
        else{
            return alpha / beta;
        }
    }

    //출력
    try{
        divide(10, 0);
    }
    catch(exception){
        alert('CATCH');
    }
</script>

```

실제 JS로 웹 앱을 만들 때 예외 처리를 굉장히 많이 사용하므로 꼭 기억하자.

---



## 12.7 응용

#### 12.7.1 finally 구문

`finally` 구문은 반드시 실행된다는 특성이 있다.
따라서 `finally`를 사용하고 안 하고의 결과가 달라진다.

`try` `catch` 구문 안에서 `return `, `break`, `continue` 키워드를 만났을 때,
`finally` 구문안에 있는 코드는 반드시 실행된다.
없으면 바로 함수를 나가거나 반복문이 끝난다.



#### 12.7.2 외부 JS 파일

프로그램이 규모가 커지면 HTML파일과 별개로 JS파일을 관리하게 된다.

```js
// [12-15]
// 외부 JS 파일
function externalFileFunction(){ }
var externalFileVariable = 'Hello World!';
alert('Import External Script Complate');
```

```html
<!-- [12-15] -->
<!-- 외부 JS 파일 사용 -->
<!DOCTYPE html>
<html>
<head>
    <script src="12-15.js"></script>
</head>
<body>
    
</body>
</html>
```



외부 JS를 사용할 때, 주의할 점이 있다.
웹 브라우저는 위에서 아래로 태그를 차례대로 실행한다는 점이다.
즉, 외부 JS 파일을 추가하기 전에는 외부 JS 파일에 있는 변수나 함수를 사용할 수 없다.

```html
<!-- [12-17] -->
<!-- 외부 JS 파일 사용 시 주의점 -->
<head>
    <script>
        alert(typeof(externalFileVariable));
        alert(typeof(externalFileFunction));
    </script>
    
    <script src="12-15.js"></script> <!-- 외부 JS 파일 추가 -->
    <script>
        alert(typeof(externalFileVariable));
        alert(typeof(externalFileFunction));
    </script>
</head>
```



따라서 `head` 태그는 다음 처럼 구성하는 것이 일반적이다.

```html
<!-- [12-18] -->
<!-- head 태그 구성 형식 -->
<head>
    <title>Index</title>
    <meta name="" content=""/>
    <link rel="stylesheet" href="외부 스타일시트">
    <style>
        /* 생략 */
    </style>

    <script src="외부 JS"></script>
    <script>
        // 생략
    </script>
</head>
```

jQuery 플러그인을 사용할 때 이 순서는 매우 중요하니 꼭 기억하자.

---

