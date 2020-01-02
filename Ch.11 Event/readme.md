# 11장. 이벤트

## 11.1 이벤트의 종류

- 마우스 이벤트
- 키보드 이벤트
- HTML 프레임 이벤트
- HTML 입력 양식 이벤트
- 유저 인터페이스 이벤트
- 구조 변화 이벤트
- 터치 이벤트

---



## 11.2 용어 정리

- **이벤트 이름(이벤트 타입)** : 보통 on 뒤에 나오는 거. *ex) onload의 이벤트 이름은 load*
- **이벤트 속성** : 객체의 속성. *ex) window.onload의 이벤트 속성은 onload*
- **이벤트 리스너(이벤트 핸들러)** : 이벤트 속성에 할당한 함수.
- **이벤트 모델** : 문서 객체에 이벤트를 연결하는 방법. DOM Level 단계에 따라 두 가지로 분류할 수 있다.
  - DOM Level 0
    - 인라인 이벤트 모델
    - 기본 이벤트 모델
  - DOM Level 1
    - Microsoft 인터넷 익스플로러 이벤트 모델
    - 표준 이벤트 모델

---



## 11.3 고전 이벤트 모델

```html
<!-- [11-5] -->
<!-- 고전 이벤트 모델, onclick -->
<!DOCTYPE html>
<html>
<head>
    <script>
        window.onload = function(){
            var header = document.getElementById('header');

            //이벤트 연결
            header.onclick = function(){
                alert('클릭');

                //이벤트 제거
                header.onclick = null;
            };
        };
    </script>
</head>
<body>
    <h1 id="header">Click</h1>
</body>
</html>
```

실행하고 `h1`태그를 클릭하면 이벤트가 발생하고 경고창이 출력된다.
이벤트 리스너를 제거할 때는 문서 객체의 이벤트 속성에 `null`을 넣어준다.
한 번 실행된 이후에 이벤트를 제거하므로, 두 번째 클릭부터는 이벤트가 발생하지 않는다.

고전 이벤트 모델은 <u>이벤트 하나에 리스너 하나</u>만 연결할 수 있다.

---



## 11.4 이벤트 발생 객체와 이벤트 객체

이벤트 리스너 안에서 `this` 키워드를 사용하면 이벤트가 발생한 객체를 찾을 수 있다.

```html
<!-- [11-6] -->
<!-- this -->
<!DOCTYPE html>
<html>
<head>
    <script>
        window.onload = function(){
            document.getElementById('header').onclick = function(){
                alert(this);
                
                //이벤트 발생 객체의 스타일 변경
                this.style.color = 'orange';
                this.style.backgroundColor = 'red';
            };
        };
    </script>
</head>
<body>
    <h1 id="header">Click</h1>
</body>
</html>
```

리스너 안에서 `this` 키워드의 스타일을 바꾸는 것은 이벤트가 발생한 객체의 스타일을 변경하는 것이다.

이벤트가 발생한 객체 정보 이외의 정보는 이벤트 객체에 있다.

```html
<!-- [11-8] -->
<!-- 이벤트 객체 -->
<!DOCTYPE html>
<html>
<head>
    <script>
        window.onload = function(){
            document.getElementById('header').onclick = function(e){
               var event = e || window.event; //e가 undefined이면 window.event 속성을 변수 event에 넣는 조건문.

               document.body.innerHTML = '';
               for(var key in event){
                   document.body.innerHTML += `<p> ${key} : ${event[key]} </p>`;
               }
            };
        };
    </script>
</head>
<body>
    <h1 id="header">Click</h1>
</body>
</html>
```

> > > **조건문이 붙은 이유**
> > >
> > > 익스플로러 8 이하의 버전은 이벤트가 발생할 때 이벤트 객체를 `window.event` 속성으로 전달하지만, 
> > > 다른 브라우저는 이벤트 리스너의 매개변수로 전달하기 때문.

---



## 11.5 이벤트 강제 실행

적절한 부분에 활용하면 코드의 길이를 많이 줄일 수 있다.

이벤트도 속성이고, 함수 자료형을 넣으므로 메서드이다.
따라서 <u>메서드를 호출하는 것</u>처럼 이벤트 속성을 호출하면 이벤트가 강제로 실행된다.

```html
<!-- [11-10] -->
<!-- 이벤트 강제 실행 -->
<!DOCTYPE html>
<html>
<head>
    <script>
        window.onload = function(){
            var buttonA = document.getElementById('button-a');
            var buttonB = document.getElementById('button-b');
            var counterA = document.getElementById('counter-a');
            var counterB = document.getElementById('counter-b');

            //이벤트 연결
            buttonA.onclick = function(){
                counterA.innerHTML = Number(counterA.innerHTML) + 1;                
            }
            buttonB.onclick = function(){
                counterB.innerHTML = Number(counterB.innerHTML) + 1;
                buttonA.onclick(); //이벤트 강제 실행
            }

        };    
    </script>
</head>
<body>
    <button id="button-a">ButtonA</button>
    <button id="button-b">ButtonB</button>
    <h1>Button A - <span id="counter-a">0</span></h1>
    <h1>Button B - <span id="counter-b">0</span></h1>
</body>
</html>
```

---



## 11.6 인라인 이벤트 모델

HTML 페이지의 가장 기본적인 이벤트 연결 방법.

```html
<!-- [11-18] -->
<!-- 인라인 이벤트 모델 -->
<!DOCTYPE html>
<html>
<head>
    <script>
        function whenClick(e){
            alert('클릭');
        }    
    </script>
</head>

<body>
    <h1 onclick="whenClick(event)">Click</h1> <!-- 이벤트 속성을 주고 핸들러 연결 -->
</body>
</html>
```

---



## 11.7 디폴트 이벤트 제거

일부 HTML 태그는 이미 이벤트 리스너가 있다. 이를 **디폴트 이벤트**라고 한다. *ex) <a> 태그*

이 절에서는 디폴트 이벤트를 제거하는 방법을 알아보자.
디폴트 이벤트 제거는 <u>입력 양식에 많이 사용</u>된다.

```html
<!-- [11-21] -->
<!-- 디폴트 이벤트 제거, 고전 이벤트 모델 -->
<!DOCTYPE html>
<html>
<head>
    <script>
        window.onload = function(){
            document.getElementById('my-form').onsubmit = function(){
                var pass = document.getElementById('pass').value;
                var passCheck = document.getElementById('pass-check').value;

                //유효성 검사
                if(pass == passCheck){
                    alert('성공');
                }
                else{
                    alert('다시 입력해주세요.');
                    return false; //디폴트 이벤트 제거
                }
            };
        };    
    </script>
</head>
<body>
    <form id="my-form">
        <label for="name">이름</label><br>
        <input type="text" name="name" id="name"><br>
        <label for="pass">비밀번호</label><br>
        <input type="password" name="pass" id="pass"><br>
        <label for="pass-check">비밀번호 확인</label><br>
        <input type="password" id="pass-check"><br>
        <input type="submit" value="제출">
    </form>
</body>
</html>
```

**고전 이벤트 모델**의 경우 이벤트 리스너에 `false`를 리턴해주면 된다.

**인라인 이벤트 모델**의 경우 `form` 태그의 `onsubmit` 이벤트 속성에 `return 함수()` 형태를 입력해야한다.

```html
<!-- [11-22] -->
<!-- 디폴트 이벤트 제거, 인라인 이벤트 모델 -->
<!DOCTYPE html>
<html>
<head>
    <script>        
        function whenSubmmit(){
            var pass = document.getElementById('pass').value;
            var passCheck = document.getElementById('pass-check').value;

            //유효성 검사
            if(pass == passCheck){
                alert('성공');
            }
            else{
                alert('다시 입력해주세요.');
                return false; //디폴트 이벤트 제거
            }
        };           
    </script>
</head>
<body>
    <form onsubmit="return whenSubmmit()">
        <label for="name">이름</label><br>
        <input type="text" name="name" id="name"><br>
        <label for="pass">비밀번호</label><br>
        <input type="password" name="pass" id="pass"><br>
        <label for="pass-check">비밀번호 확인</label><br>
        <input type="password" id="pass-check"><br>
        <input type="submit" value="제출">
    </form>
</body>
</html>
```

---



## 11.8 이벤트 전달
