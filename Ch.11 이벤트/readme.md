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
  - DOM Level 2
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

<u>어떤 이벤트가 먼저 발생해서 어떤 순서로 발생</u>하는 지를 **이벤트 전달**이라고 한다.

```html
<!-- [11-23] -->
<!-- 이벤트 전달 -->
<!DOCTYPE html>
<html lang="en">
<head>
    <style>
        * {border: 3px solid black;}
    </style>
    <script>
    </script>
</head>

<body>
    <div onclick="alert('outer-div')">
        <div onclick="alert('inner-div')">
            <h1 onclick="alert('header')">
                <p onclick="alert('paragraph')">Paragraph</p>
            </h1>
        </div>
    </div>
</body>
</html>
```

일반적으로 JS의 이벤트 전달 순서는 **이벤트 버블링** 방식에 따른다.
**이벤트 버블링**은 <u>자식 노드에서 부모 노드 순으로 이벤트를 실행</u>하는 것을 의미한다.

<u>이벤트 버블링과 반대되는 개념</u>이 **이벤트 캡쳐링**이다.
**이벤트 캡쳐링**은 <u>부모 노드에서 자식 노드 순으로 실행</u>되는 것이다.

전 세계에서 가장 점유율 높은 브라우저인 익스플로러가 캡쳐링을 지원하지 않으므로 **<u>이벤트 캡쳐링은 ~~절대~~ 사용하지 않는다.</u>**



우리는 <u>이벤트 전달을 막는 방법</u>을 알아야 한다. 
이벤트 전달을 막아서 원하는 기능만 실행되도록 하는 스킬이 필요하기 때문이다.

익스플로러와 그외 브라우저가 이벤트 전달을 막는 방법은 다른다.

- **인터넷 익스플로러** : 이벤트 객체의 `cancelBubble` 속성을 `true`로 변경한다.
- **그외 브라우저** : 이벤트 객체의 `stopPropagation()` 메서드를 사용한다.

```html
<!-- [11-26] -->
<!-- 이벤트 전달 제거 -->
<!DOCTYPE html>
<html>
<head>
    <script>
        window.onload = function(){
            document.getElementById('header').onclick = function(){
                alert('header');
            };

            document.getElementById('paragraph').onclick = function(e){
                //이벤트 객체 처리
                var event = e || window.event;

                alert('paragraph');

                //이벤트 전달 제거
                event.cancelBubble = true;
                if(event.stopPropagation){
                    event.stopPropagation();
                }
            };
        };
    </script>
</head>
<body>
    <h1 id="header">
        <p id="paragraph">Paragraph</p>
    </h1>
</body>
</html
```

익스플로러는 이벤트 객체에 `stopPropagtion()` 메서드가 없으므로 조건문으로 에러를 방지한다.
그외 브라우저에서는 `cancelBubble` 속성을 사용하는 것이 문제가 될 수 있지만, 속성에 값을 입력하는 것은 오류가 발생하지 않으므로 문제없다.

---



## 11.9 인터넷 익스플로러 이벤트 모델

**고전 이벤트 모델**이나 **인라인 이벤트 모델**은 <u>한 번에 하나의 이벤트 리스너</u>만 가질 수 있다.
이러한 DOM Level 0 이벤트 모델들의 <u>단점을 보완하려고 만들어진 이벤트 모델이 DOM Level 2 이벤트 모델</u>이다.

**인터넷 익스플로러 이벤트 모델**은 다음 두 메서드로 이벤트를 연결하거나 제거할 수 있다.
<u>**첫 번째 매개변수에 이벤트 속성**</u>을 쓴다.

- attachEvent(eventProperty, eventListner);
- detachEvent(eventProperty, eventListner);

```html
<!-- [11-28] -->
<!-- 인터넷 익스플로러 이벤트 모델, 여러 개의 이벤트 연결 -->
<!DOCTYPE html>
<html>
<head>
    <script>
        //윈도우가 로드될 때
        window.attachEvent('onload', function(){
            var header = document.getElementById('my-header');
            
            //이벤트 연결
            header.attachEvent('onclick', function(){alert('클릭1');});
            header.attachEvent('onclick', function(){alert('클릭2');});
            header.attachEvent('onclick', function(){alert('클릭3');});
        });
    </script>
</head>
<body>
    <h1 id="my-header">Click</h1>    
</body>
</html
```

인터넷 익스플로러의 이벤트 모델이므로 <u>익스플로러에서만 실행</u>된다.

DOM Level 2 이벤트 모델은 <u>여러 번 이벤트를 추가</u>할 수 있다.
[11-28]을 보면 이벤트 리스너를 세개 연결하는데, <u>세 개 모두 연결한 순서대로 동작</u>한다.



이벤트를 제거할 때, <u>익명 함수</u>를 이벤트 리스너로 사용한 이벤트는 제거할 수 없다.
어떤 이벤트 리스너를 제거할지 명확하게 알려주어야 하기 때문이다.

```html
<!-- [11-29] -->
<!-- 인터넷 익스플로러 이벤트 모델, 이벤트 제거 -->
<!DOCTYPE html>
<html>
<head>
    <script>
        //윈도우가 로드될 때
        window.attachEvent('onload', function(){
            var header = document.getElementById('my-header');
            var handler = function(){alert('클릭');};
            
            //이벤트 연결
            header.attachEvent('onclick', handler);
            //이벤트 제거
            header.detachEvent('onclick', handler);
        });
    </script>
</head>
<body>
    <h1 id="my-header">Click</h1>    
</body>
</html
```

참고로 **인터넷 익스플로러 이벤트 모델**에서 <u>이벤트 리스너의 `this` 키워드는 이벤트 발생 객체를 의미하지 않는다. `window` 객체를 나타낸다.</u>
이벤트 발생 객체를 사용하려면 이벤트 객체의 `srcElement` 속성을 사용해야한다.

`attachEvent()` 메서드는 인터넷 익스플로러에만 있으므로 다른 브라우저에서는 에러가 난다.
따라서 다른 브라우저일 경우 조건문으로 해당 내용을 실행하지 않게 해줘야한다.

```html
<!-- [11-30] -->
<!-- 인터넷 익스플로러 이벤트 모델, 조건문 -->
<!DOCTYPE html>
<html>
<head>
    <script>
        //윈도우가 로드될 때
        window.onload = function(){
            var header = document.getElementById('my-header');
			//인터넷 익스플로러의 경우 실행한다.
            if(header.attachEvent){
                var handler = function(){
                    window.event.srcElement.style.color = 'red';
                    window.event.srcElement.detachEvent('onclick', handler);
                };
                header.attachEvent('onclick', handler);
            }
        };
    </script>
</head>
<body>
    <h1 id="my-header">Click</h1>    
</body>
</html
```

**인터넷 익스플로러 이벤트 모델**은 그냥 이런 것도 있구나 하고 넘어가도 된다.

---



## 11.10 표준 이벤트 모델

**표준 이벤트 모델**은 W3C에서 공식적으로 지정한 DOM Level 2 이벤트 모델이다.
한 번에 여러 가지의 이벤트 리스너를 추가할 수 있으며, 이벤트 캡쳐링도 지원한다.

> 익스플로러는 **표준 이벤트 모델**을 익스플로러9 부터 지원한다.



**표준 이벤트 모델**은 다음 메서드를 사용한다.

- `addEventListener(eventName, handler, useCapture)` 
- `removeEventListner(eventName, handler)`

매개 변수 `useCapture`는 입력하지 않으면 자동으로 `false`가 입력된다.

```html
<!-- [11-31] -->
<!-- 표준 이벤트 모델, 이벤트 연결 -->
<!DOCTYPE html>
<html>
<head>
    <script>
        window.onload = function(){
            var header = document.getElementById('my-header');

            header.addEventListener('click', function(){
                this.innerHTML += '+';
            });
        };
    </script>
</head>
<body>
    <h1 id="my-header">Click</h1>
</body>
</html
```

**표준 이벤트 모델**은 이벤트 리스너 안에서 <u>`this` 키워드가 이벤트 발생 객체</u>를 의미한다. 



```html
<!-- [11-32] -->
<!-- 이벤트 모델의 통합적 사용 -->
<!DOCTYPE html>
<html>
<head>
    <script>
        window.onload = function(){
            var header = document.getElementById('my-header');

            if(header.attachEvent){
                //익스플로러의 경우 실행
                var handler = function(){
                    window.event.srcElement.style.color = 'red';
                    window.event.srcElement.detachEvent('onclick', handler);
                };
                header.attachEvent('onclick', handler);
            }
            else{
                //그외 브라우저 실행
                var handler = function(){
                    this.style.color = 'red';
                    this.removeEventListner('click', handler);
                };
                header.addEventListener('click', handler);
            }
        };
    </script>
</head>

<body>
    <h1 id="my-header">Click</h1>        
</body>
</html
```

---



