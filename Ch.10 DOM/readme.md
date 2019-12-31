# 10장. 문서 객체 모델(DOM)

## 10.1 용어 정리

- **태그** : HTML 페이지에 <>로 묶인 것들.
- **문서 객체** : 태그를 JS에서 조작할 수 있도록 만든 객체. 
- **노드** : DOM 트리 구조에서의 요소.
  - 요소 노드 : 보통 태그를 말한다.
  - 텍스트 노드 : 요소 노드 안에 들어있는 글자.



## 10.2 문서 객체 만들기

- `createElement(tagName)` : 요소 노드를 생성.
- `createTextNode(text)` : 텍스트 노드를 생성.
- `appendChild(node)` : 객체에 노드를 연결.

```html
<!-- [10-6] -->
<!-- 문서 객체 연결 -->

<script>
    window.onload = function() {
        var header = document.createElement('h1'); //요소 노드 생성
        var textNode = document.createTextNode('Hello DOM'); //텍스트 노드 생성

        header.appendChild(textNode); //객체에 노드 연결
        document.body.appendChild(header); //body태그에 연결
    };
</script>
```



```html
<!-- [10-8] -->
<!-- 텍스트 노드가 없는 노드, img -->

<script>
    window.onload = function(){
        var img = document.createElement('img');
        img.src = 'test.jpg'
        img.width = 300;
        img.height = 300;

        document.body.appendChild(img);
    };
</script>
```

> 이 방법은 웹 표준에서 정의하거나 웹 브라우저가 지원하는 태그의 속성에만 사용할 수 있다.
> 따라서 웹 브라우저가 지원하지 않는 속성은 `setAttribute()`,`getAttribute()` 메서드를 사용해야 속성을 넣을 수 있다.



- `setAttribute(name, value)` : 객체의 속성을 지정.
- `getAttribute(name)` : 객체의 속성을 가져옴.

```html
<!-- [10-9] -->
<!-- setAttribute를 통한 객체 속성 지정 -->

<script>
    window.onload = function(){
        var img = document.createElement('img');
        img.setAttribute('src', 'test.jpg');
        img.setAttribute('width', 300);
        img.setAttribute('height', 300);
        
        //data-property 속성은 [10-8]에서의 방식으로 넣을 수 없다.
        img.setAttribute('data-property', 350); 

        document.body.appendChild(img);
    };
</script>
```



여기까지는 문서 객체를 생성방법이 <u>노드를 만들고 연결하는 방법</u>이다. 
**innerHTML 속성**을 사용하면 더 쉽게 문서 객체를 생성할 수 있다.
문자열을 선언하고 body 문서 객체의 innerHTML 속성에 넣기만 하면 문서 객체가 생성된다.

```html
<!-- [10-11] -->
<!-- innerHTML 속성을 사용한 문서 객체 생성 -->

<script>
    window.onload = function(){
        var output = '';
        output += '<ul>';
        output += ' <li>JavaScript</li>';
        output += ' <li>jQuery</li>';
        output += ' <li>Ajax</li>';
        output += '</ul>';
        
        document.body.innerHTML = output;

        //innerHTML 속성에 다음과 같이 복합 대입 연산자로 문자열을 추가할 수 있다.
        document.body.innerHTML += '<h1>Document Object Model</h1>'
    };
</script>
```



> > > **textContent 속성** 
> > >
> > > 만약 HTML 형태의 문자열을 HTML 태그로 놓지 않고 단순 글자로 넣고 싶다면
> > > <u>textContent 속성</u>을 사용하면 된다.
> > > ~~(단, 인터넷 익스플로러 7 버전 이하는 지원되지 않는다.)~~
> > >
> > > ```html
> > > <script>
> > >     window.onload = function(){
> > >         var output = '';
> > >         output += '<ul>';
> > >         output += ' <li>JavaScript</li>';
> > >         output += ' <li>jQuery</li>';
> > >         output += ' <li>Ajax</li>';
> > >         output += '</ul>';
> > >         
> > >         document.body.textContent = output;
> > >         document.body.textContent += '<h1>Document Object Model</h1>'
> > >     };
> > > </script>
> > > ```



## 10.3 문서 객체 가져오기

웹 페이지에 이미 존재하는 HTML 태그를 JS로 가져올 수 있다.

- `getElementById(id)` : 태그의 id 속성이 id 매개변수와 일치하는 문서 객체를 가져온다.

```html
<!-- [10-15] -->
<!-- getElementById로 문서 객체 가져오기 -->

<script>
    window.onload = function(){
        var header1 = document.getElementById('header-1');
        var header2 = document.getElementById('header-2');

        header1.innerHTML = 'with getElementById();';
    };
</script>

<body>
    <h1 id="header-1">Header</h1>
    <h1 id="header-2">Header</h1>
</body>
```



한 번에 여러 개의 문서 객체를 가져올 수 있다.

- `getElementByName(name)` : 태그의 name 속성이 name 매개변수와 일치하는 문서 객체를 배열로 가져옴.
- `getElementByTagName(tagName)` : tagName 매개변수와 일치하는 문서 객체를 배열로 가져옴.

```html
<!-- [10-17] -->
<!-- getElementsByTagName(), getElementsByName() -->
<!DOCTYPE html>
<head>
    <title>Index</title>
    <script>
        window.onload = function(){
            var headers = document.getElementsByTagName('h1');            
            headers[0].innerHTML = 'with getElementsByTagName()';
            headers[1].innerHTML = 'with getElementsByTagName()';

            var headers2 = document.getElementsByName('name');
            headers2[0].innerHTML = 'with getElementsByName()';
            headers2[1].innerHTML = 'with getElementsByName()';
        };
    </script>
</head>

<body>
    <h1>Header</h1>
    <h1>Header</h1>
    <h1>Header</h1>
    <h1 name="name">Header</h1>
    <h1 name="name">Header</h1>
    <h1 name="name">Header</h1>
</body>
</html>
```



`getElementsByTagName()`, `getElementsByName()`은 배열로 불러오므로 반복문을 사용할 수 있다.
<u>**하지만, for in 반복문은 사용하면 안된다.**</u>

```html
<!-- [10-18] -->
<!-- 반복문 사용해보기 -->
<!DOCTYPE html>
<head>
    <title>Index</title>
    <script>
        window.onload = function(){
            var headers = document.getElementsByTagName('h1');            
            for(var i = 0; i < headers.length; i++){
                headers[i].innerHTML = 'with getElementsByTagName()';
            }
        };
    </script>
</head>

<body>
    <h1>Header</h1>
    <h1>Header</h1>
</body>
</html>
```

> > >  **for in 반복문을 사용하면 안 되는 이유**
> > >
> > > for in 반복문을 사용하면 <u>문서 객체 이외의 속성</u>에도 접근하기 때문이다.
> > > 따라서, 꼭 단순 for 반복문을 사용할 것.



HTML5 부터는 document 객체에 새로운 메서드가 추가되었다.
**CSS 선택자**로 문서 객체를 선택하는 메서드이다.

- `querySelector(선택자)` : 선택자로 가장 처음 선택되는 문서 객체를 가져옴.
- `quertSelectorAll(선택자)` : 선택자를 통해 선택되는 문서 객체를 배열로 가져옴.

```html
<!-- [10-20] -->
<!-- quertSelector() -->

<!DOCTYPE html>
<html>
<head>
    <title>DOM Basic</title>
    <script>
        window.onload = function(){
            var header1 = document.querySelector('#header-1');
            var header2 = document.querySelector('#header-2');

            header1.innerHTML = 'with querySelector()';
            header2.innerHTML = 'with querySelector()';
        };
    </script>
</head>
<body>
    <h1 id="header-1">Header</h1>
    <h1 id="header-2">Header</h1>
</body>
</html>
```



## 10.4 문서 객체의 스타일 조작

문서 객체의 style 속성을 사용하면 객체의 스타일을  변경할 수 있다.

```html
<!-- [10-22] -->
<!-- style 속성으로 style 조작 -->
<!DOCTYPE html>
<html>
<head>
    <title>Style</title>
    <script>
        window.onload = function(){
            var header = document.getElementById('header');
            header.style.border = '2px solid black';
            header.style.color = 'orange';
            header.stlye.fontFamaily = 'helvetica';
        };
    </script>
</head>
<body>
    <h1 id="header">Header</h1>
</body>
</html>
```

> > > **스타일의 속성 이름**
> > >
> > > <u>JS는 (-)을 식별자에 사용할 수 없다.</u> 
> > > 따라서 style 속성 중에 (-)로 연결된 것은 대문자로 바꾸면 된다.
> > >
> > > ex) background-color -> backgroundColor
> > >
> > > 문자열로 style 속성에 접근하면, 두 가지 방법 모두 이용 가능하다.
> > >
> > > ```html
> > > document.body.style['backgroundColor'] = 'red';
> > > document.body.style['background-color'] = 'red';
> > > ```



## 10.5 문서 객체 제거

- `removeChild(child)` : 문서 객체의 자식 노드를 제거한다.

```html
<!-- [10-24] -->
<!-- 문서 객체 제거 -->
<!DOCTYPE html>
<html>
<head>
    <title>Remove</title>
    <script>
        window.onload = function(){
            var willRemove = document.getElementById('will-remove');
            document.body.removeChild(willRemove);
            // willRemove.parentNode.removeChild(willRemove); //일반적으로 이걸 사용한다.
        };   
    </script>
</head>
<body>
    <h1 id="will-remove">Header</h1>
    <h1 id="not-remove">Header not remove</h1>
</body>
</html>
```

[10-24]는 body 문서 객체 바로 아래에 제거하고자 하는 문서 객체가 있으므로 가능하다.

**일반적으로** `willRemove.parentNode.removeChild('willRemove');`**를 사용한다.**
<u>**부모 노드로 이동하고 그 자식 노드를 제거하는 것이다.**</u>
