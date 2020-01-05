# 13장. jQuery 기본

## 13.1 개요

jQuery는 모든 브라우저에서 동작하는 클라이언트 JS 라이브러리이다.
2006년 1월, 존 레식이 BarCamp NYC에서 발표했으며, 무료로 사용 가능한 오픈 소스 라이브러리이다.

jQuery는 다음 기능을 위해 제작됐다.

- DOM과 관련된 처리를 쉽게 구현
- 일관된 이벤트 연결을 쉽게 구현
- 시각적 효과를 쉽게 구현
- Ajax 앱을 쉽게 개발

새로운 라이브러리와 프레임워크가 등장했음에도 불구하고 점유율이 매우 높다.

---



## 13.2 사용

jQuery는 두 가지 방법으로 사용할 수 있다.

1. CDN 호스트를 사용하는 방법
2. 직접 내려받아 사용하는 방법

CDN은 사용자에게 간편하게 콘텐츠를 제공하는 방식이다.
사용하려면 밑에와 같이 HTML 페이지를 구성해야 한다.

```html
<!-- [13-01] -->
<!-- CDN 호스트 사용 -->
<!DOCTYPE html>
<html>
<head>
    <script src="https://code.jquery.com/jquery-3.4.1.js"
            integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU="
            crossorigin="anonymous"></script>
</head>
<body>
    
</body>
</html
```

> > > **CDN (Content Delivery Network)**
> > >
> > > 콘텐츠를 효율적으로 전달하기 위해 전 세계 여러 지점의 서버에 파일을 저장해두고, 사용자와 가까운 지역에서 해당 파일(콘텐츠)을 제공해주는 네트워크 시스템을 의미한다.
> > > 최근에는 'Content Distribution Network' 라고 부르기도 한다.



jQuery 파일명은 .js 파일과 .min.js 파일로 나뉜다.

- `.js` : Uncompressed 버전이라고 부른다.
- `.min.js` : Minified 버전이라고 부른다.

두 파일의 용량은 5배 이상 차이난다. Minified 버전은 파일의 용량을 최소화하려고 zipping한 파일이다.

```html
<!-- [13-02] -->
<!-- 내려받아 사용 -->
<!DOCTYPE html>
<html>
<head>
    <script src="jquery-3.4.1.js"></script>
    <script src="jquery-3.4.1.min.js"></script>
    <script>
        
    </script>
</head>
<body>
    
</body>
</html
```

사용할려면 같은 폴더에 jQuery 파일이 있어야 한다.

---



## 13.3 $(document).ready()

jQuery를 사용한 모든 웹 페이지는 다음 코드로 시작한다.

```html
<!-- [13-04] -->
<!-- document 객체의 raedy 이벤트 연결 -->
<script>
    $(document).ready(function(){

    });
</script>
```

`$(document).ready()`는 문서가 준비되면 매개변수로 넣은 콜백 함수를 실행하라는 의미이다.
`window.onload = function(){};` 와 비슷한 기능을 수행한다.

jQuery의 이벤트 메서드는 DOM Level 2 이벤트 모델과 같이 <u>여러 개의 함수를 연결</u>할 수 있다.

```html
<!-- [13-06] -->
<!-- 여러 개의 이벤트 연결 -->
<script src="jquery-3.4.1.js"></script>
<script>
    $(document).ready(function(){
        alert('First READY');
    });
    $(document).ready(function(){
        alert('Second READY');
    });
    $(document).ready(function(){
        alert('Third READY');
    });
</script>
```



다음과 같이 **간단하게 사용**할 수도 있다.

```html
<script>
	$(function(){
        
    });
</script>
```

> > > **$ 식별자**
> > >
> > > JS에서 식별자로 사용할 수 있는 특수 기호는 _와 $이다.
> > >
> > > jQuery 식별자를 $로 대체했을 뿐.

---



## 13.4 기본 선택자

jQeury 메서드의 가장 기본적이고 가장 많이 사용하는 형태, 문서 객체를 다룰 때 사용하는 형태

`$(선택자).메서드`

`$('h1').css('color', 'red');`



#### 13.4.1 전체 선택자

CSS의 가장 기본적인 선택자는 **전체 선택자(*)**이다.

```html
<!-- [13-08] -->
<!-- 전체 선택자 -->
<!DOCTYPE html>
<html>
<head>
    <script src="http://code.jquery.com/jquery-3.4.1.js"></script> <!-- url로 import할 수도 있다. -->
    <script>
        $(document).ready(function(){
            $('*').css('color', 'red');
        });
    </script>
</head>

<body>
    <h1>Lorem ipsum</h1>
</body>
</html>
```

`css()` 메서드는 jQuery의 가장 기본적인 메서드.
첫 번째 매개변수에 바꾸고자 하는 스타일 속성 이름을 입력하고,
두 번째 메서드에 스타일 속성 값을 입력한다.



#### 13.4.2 태그 선택자

특정 태그를 선택하는 선택자.

```html
<!-- [13-10] -->
<!-- 태그 선택자 -->
<!DOCTYPE html>
<html>
<head>
    <script src="jquery-3.4.1.js"></script>
    <script>
        $(document).ready(function(){
            $('h1').css('color', 'orange');
        });
    </script>
</head>

<body>
    <h1>Lorem ipsum</h1>    
    <p>Lorem ipsum dolor sit amet.</p>
    <h1>Lorem ipsum</h1>    
    <p>consetetur adipiscing elit.</p>
</body>
</htm>
```

`$('h1', 'p').css('color', 'orange');` 처럼 여러 개의 태그를 선택할 수 있다.



#### 13.4.3 아이디 선택자

특정 id 속성이 있는 문서 객체를 선택하는 선택자이다.

```html
<!-- [13-13] -->
<!-- id 선택자 -->
<!DOCTYPE html>
<html>
<head>
    <script src="jquery-3.4.1.js"></script>
    <script>
        $(document).ready(function(){
            $('#target').css('color', 'orange');
        });
    </script>
</head>

<body>
    <h1>Header-0</h1>    
    <h1 id="target">Header-1</h1>
    <h1>Header-2</h1>
</body>
</html
```

`$('h1#target')...` 으로 더 정확하게 문서 객체를 선택할 수 있다.



#### 13.4.4 클래스 선택자

특정 클래스 속성이 있는 문서 객체를 선택하는 선택자이다.

```html
<!-- [13-16] -->
<!-- 클래스 선택자 -->
<!DOCTYPE html>
<html>
<head>
    <script src="jquery-3.4.1.js"></script>
    <script>
        $(document).ready(function(){
            $('.item').css('color', 'orange');
            $('h1.item').css('background', 'red');
            $('.item.select').css('background', 'black');
        });
    </script>
</head>

<body>
    <h1 class="item">Header-0</h1>    
    <h1 class="item select">Header-1</h1>
    <h1 class="item">Header-2</h1>
</body>
</html
```

`$('.item.select')...` 처럼 두 클래스 속성을 모두 갖는 문서 객체를 선택할 수 있다.

---



## 13.5 자손 선택자, 후손 선택자

**자손 선택자**와 **후손 선택자**는 기본 선택자 앞에 붙여 사용하며, <u>기본 선택자의 범위를 제한</u>한다.

- **자손** : 바로 밑에 있는 문서 객체
- **후손** : 밑에 있는 모든 문서 객체

#### 13.5.1 자손 선택자

자손을 선택하는 선택자. `요소A > 요소B`의 형태로 사용한다.

```html
<!-- [13-19] -->
<!-- 자손 선택자 -->
<!DOCTYPE html>
<html>
<head>
    <script src="jquery-3.4.1.js"></script>
    <script>
        $(document).ready(function(){
            $('body > *').css('color', 'orange');
        });
    </script>
</head>

<body>
    <div>
        <ul>
            <li>Apple</li>
            <li>Bag</li>
            <li>Cat</li>
            <li>Dog</li>
            <li>Elephant</li>
        </ul>
    </div>
</body>
</html
```

![image-20200104153809108](C:\Users\SinJ\AppData\Roaming\Typora\typora-user-images\image-20200104153809108.png)

#### 13.5.2 후손 선택자

후손을 선택하는 선택자. `요소A 요소B`의 형태로 사용한다. 

```html
<!-- [13-20] -->
<!-- 후손 선택자 -->
<!DOCTYPE html>
<html>
<head>
    <script src="jquery-3.4.1.js"></script>
    <script>
        $(document).ready(function(){
            $('body *').css('color', 'orange');
        });
    </script>
</head>

<body>
    <div>
        <ul>
            <li>Apple</li>
            <li>Bag</li>
            <li>Cat</li>
            <li>Dog</li>
            <li>Elephant</li>
        </ul>
    </div>
</body>
</html
```

![image-20200104153717068](C:\Users\SinJ\AppData\Roaming\Typora\typora-user-images\image-20200104153717068.png)

---



## 13.6 속성 선택자

**속성 선택자**는 **기본 선택자** 뒤에 붙여 사용한다.

| 선택자 형태     | 설명                                                       |
| --------------- | ---------------------------------------------------------- |
| 요소[속성=값]   | 속성과 값이 같은 문서 객체를 선택.                         |
| 요소[속성\|=값] | 속성 안의 값이 특정 값과 같은 문서 객체 선택.              |
| 요소[속성~=값]  | 속성 안의 값이 특정 값을 단어로 시작하는 문서 객체를 선택. |
| 요소[속성^=값]  | 속성 안의 값이 특정 값으로 시작하는 문서 객체를 선택.      |
| 요소[속성$=값]  | 속성 안의 값이 특정 값으로 끝나는 문서 객체를 선택.        |
| 요소[속성*=값]  | 속성 안의 값이 특정 값을 포함하는 문서 객체를 선택.        |

```html
<!-- [13-22] -->
<!-- 속성 선택자 -->
<!DOCTYPE html>
<html>
<head>
    <script src="jquery-3.4.1.js"></script>
    <script>
        $(document).ready(function(){
            $('input[type="text"]').val('Hello jQuery!');
        });    
    </script>
</head>
<body>
    <input type="text"/>
    <input type="password"/>
    <input type="radio"/>
    <input type="checkbox"/>
    <input type="file"/>
</body>
</html>
```

`.val()` 메서드는 매개변수를 입력하면 `input` 태그의 `value` 속성을 지정하고,
입력하지 않으면 `value` 속성을 검사한다.

---



## 13.7 필터 선택자

선택자 중에  : 기호를 포함하는 선택자를 **필터 선택자**라고 한다.



#### 13.7.1 입력 양식 필터 선택자

**속성 선택자**를 사용하면 `input`태그의 `type` 속성에 따라 문서 객체를 선택할 수 있다.
조금 더 <u>간단한 방법</u>이 **필터 선택자**이다.

| 선택자 형태    | 설명                                                         |
| -------------- | ------------------------------------------------------------ |
| input:button   | input 태그 중 type 속성이 button인 문서 객체와 button 태그를 선택한다. |
| input:checkbox | input 태그 중 type 속성이 check인 문서 객체를 선택한다.      |
| input:file     | input 태그 중 type 속성이 file인 문서 객체를 선택한다.       |
| input:image    | input 태그 중 type 속성이 image인 문서 객체를 선택한다.      |
| input:password | input 태그 중 type 속성이 password인 문서 객체를 선택한다.   |
| input:radio    | input 태그 중 type 속성이 radio인 문서 객체를 선택한다.      |
| input:reset    | input 태그 중 type 속성이 reset인 문서 객체를 선택한다.      |
| input:submit   | input 태그 중 type 속성이 submit인 문서 객체를 선택한다.     |
| input:text     | input 태그 중 type 속성이 text인 문서 객체를 선택한다.       |



`input` 태그 말고도 입력 양식과 관련된 **필터 선택자**들은 다음과 같다.

| 선택자 형태   | 설명                                                         |
| ------------- | ------------------------------------------------------------ |
| 요소:checked  | 체크되어 있는 입력 양식을 선택한다.                          |
| 요소:disabled | 비활성화된 입력 양식을 선택한다.                             |
| 요소:enabled  | 활성화된 입력 양식을 선택한다.                               |
| 요소:focus    | 초점이 맞추어져 있는 입력 양식을 선택한다.                   |
| 요소:input    | 모든 입력 양식을 선택한다(input, textarea, select, button 태그). |
| 요소:selected | option 객체 중 선택된 태그를 선택한다.                       |

`:selected` 필터는 굉장히 많이 사용되므로 따로 외워두는 것도 좋다.

```html
<!-- [13-24] -->
<!-- 입력 양식 필터 선택자 -->
<!DOCTYPE html>
<html>
<head>
    <script src="jquery-3.4.1.js"></script>
    <script>
        $(document).ready(function(){
            //5초 후 코드 실행
            setTimeout(() => {
                var value = $('select > option:selected').val();

                alert(value);
            }, 5000);
        });
    </script>
</head>
<body>
    <select>
        <option>Apple</option>
        <option>Bag</option>
        <option>Cat</option>
        <option>Dog</option>
        <option>Elephant</option>
    </select>
</body>
</html
```



#### 13.7.2 위치 필터 선택자

| 선택자 형태 | 설명                                     |
| ----------- | ---------------------------------------- |
| 요소:odd    | 홀수 번째에 위치한 문서 객체를 선택한다. |
| 요소:even   | 짝수 번째에 위치한 문서 객체를 선택한다. |
| 요소:first  | 첫 번째에 위치한 문서 객체를 선택한다.   |
| 요소:last   | 마지막에 위치한 문서 객체를 선택한다.    |

```html
<!-- [13-26] -->
<!-- 위치 필터 선택자 -->
<!DOCTYPE html>
<html>
<head>
    <script src="jquery-3.4.1.js"></script>
    <script>
        $(document).ready(function(){
            $('tr:odd').css('background', '#f9f9f9');
            $('tr:even').css('background', '#9f9f9f');
            $('tr:first').css('background', '#000000').css('color', '#ffffff');
        });
    </script>
</head>

<body>
    <table>
        <tr><th>이름</th><th>혈액형</th><th>지역</th></tr>
        <tr><td>강민수</td><td>AB형</td><td>서울특별시 송파구</td></tr>
        <tr><td>구지연</td><td>B형</td><td>미국 캘리포니아</td></tr>
        <tr><td>김미화</td><td>AB형</td><td>미국 메사추세츠</td></tr>
        <tr><td>김선화</td><td>O형</td><td>서울특별시 강서구</td></tr>
        <tr><td>남기주</td><td>A형</td><td>서울특별시 노량진구</td></tr>
        <tr><td>윤하린</td><td>B형</td><td>서울특별시 용산구</td></tr>
    </table>    
</body>
</html
```

대부분의 jQuery 메서드가 **체이닝**을 사용할 수 있다.



#### 13.7.3 함수 필터 선택자

jQuery는 함수 형태의 필터 선택자를 제공한다.

| 선택자 형태           | 설명                                         |
| --------------------- | -------------------------------------------- |
| 요소:contains(문자열) | 특정 문자열을 포함하는 문서 객체를 선택한다. |
| 요소:eq(n)            | n번째에 위치하는 문서 객체를 선택한다.       |
| 요소:gt(n)            | n번째 초과에 위치하는 문서 객체를 선택한다.  |
| 요소:has(h1)          | h1 태그가 있는 문서 객체를 선택한다.         |
| 요소:lt(n)            | n번째 미만에 위치하는 문서 객체를 선택한다.  |
| 요소:not(선택자)      | 선택자와 일치하지 않는 문서 객체를 선택한다. |
| 요소:nth-child(3n+1)  | 3n+1번째에 위치하는 문서 객체를 선택한다.    |

```html
<!-- [13-27] -->
<!-- 함수 필터 선택자 -->
<!DOCTYPE html>
<html>
<head>
    <script src="jquery-3.4.1.js"></script>
    <script>
        $(document).ready(function(){
            $('tr:eq(0)').css('background', '#000000').css('color', 'white');
            $('td:nth-child(3n+1)').css('background', '#565656');
            $('td:nth-child(3n+2)').css('background', '#a9a9a9');
            $('td:nth-child(3n)').css('background', '#f9f9f9');
        });
    </script>
</head>
<body>
    <table>
        <tr><th>이름</th><th>혈액형</th><th>지역</th></tr>
        <tr><td>강민수</td><td>AB형</td><td>서울특별시 송파구</td></tr>
        <tr><td>구지연</td><td>B형</td><td>미국 캘리포니아</td></tr>
        <tr><td>김미화</td><td>AB형</td><td>미국 메사추세츠</td></tr>
        <tr><td>김선화</td><td>O형</td><td>서울특별시 강서구</td></tr>
        <tr><td>남기주</td><td>A형</td><td>서울특별시 노량진구</td></tr>
        <tr><td>윤하린</td><td>B형</td><td>서울특별시 용산구</td></tr>
    </table>
</body>
</html>
```

**필터 선택자**는 **기본 선택자**에 비해 활용도가 굉장히 떨어진다. 
따라서 **필터 선택자**는 필요할 때 찾아 쓰는 쪽을 추천한다.

---



## 13.8 배열 관리

jQuery로 배열을 관리할 때는 `each()` 메서드를 사용한다.
`each()` 메서드는 매개변수로 입력한 함수로 `for in` 반복문처럼 객체나 배열의 요소를 검사하는 메서드이다.

`each()` 메서드는 두 가지 형태로 사용한다.

1. `$.each(object, function(index, item){})`
2. `$(selector).each(function(index, item){})`



#### 13.8.1 JS 배열 관리

JS 배열에 들어 있는 내용을 HTML 페이지에 표시해보자.

```html
<!-- [13-30] -->
<!-- $.each() -->
<script src="jquery-3.4.1.js"></script>
<script>
    $(document).ready(function(){
        var array = [
            {name:'Google', link:'http://google.co.kr'},
            {name:'Naver', link:'http://naver.com'},
            {name:'Daum', link:'http://daum.net'},
        ];
        $.each(array, function(index, item){
            var output = '';

            output += `<a href="${item.link}">`;
            output += `<h1>${item.name}</h1>`;
            output += `</a>`;

            document.body.innerHTML += output;
        });
    });
    
</script>
```

Ajax를 진행할 때 `$.each()` 형태의 메서드를 많이 사용하므로 꼭 외워두자.

> > > **`forEach()` 메서드와 차이점**
> > >
> > > 매개변수의 순서가 다르다.
> > >
> > > | each() 메서드                            | forEach() 메서드                             |
> > > | ---------------------------------------- | -------------------------------------------- |
> > > | $.each(function(**index**, **item**){}); | [].forEach(function(**item**, **index**){}); |



#### 13.8.2 jQuery 배열 관리

jQuery 배열 객체를 다뤄보자. 
<u>jQuery의 배열 객체</u>는 따로 만드는 것이 아니라, <u>선택자로 여러 개의 문서 객체를 선택할 때 생성</u>된다.

```html
<!-- [13-35] -->
<!-- $(selector).each() -->
<!DOCTYPE html>
<html>
<head>
    <style>
        .high-light-0{ background: red; }
        .high-light-1{ background: orange; }
        .high-light-2{ background: yellow; }
        .high-light-3{ background: green; }
        .high-light-4{ background: blue; }
    </style>
    <script src="jquery-3.4.1.js"></script>
    <script>
        $(document).ready(function(){
            $('h1').each(function(index, item){
                $(item).addClass(`high-light-${index}`); //문서 객체에 class 속성을 추가하는 메서드.
            });
        });
    </script>
</head>
<body>
    <h1>item - 0</h1>
    <h1>item - 1</h1>
    <h1>item - 2</h1>
    <h1>item - 3</h1>
    <h1>item - 4</h1>
</body>
</html>
```

`each()` 메서드로 각각의 객체에 서로 다른 클래스가 적용된다.



매개변수 `item`을 사용할 수도 있지만, 일반적으로 `this` 키워드를 더 많이 사용한다.
`each()` 메서드의 매개변수로 입력한 함수 안에서 `this` 키워드와 `item`은 의미가 같다.

```html
<!-- [13-36] -->
<!-- each() 메서드에서의 this 키워드 -->
<!DOCTYPE html>
<html>
<head>
    <style>
        .high-light-0{ background: red; }
        .high-light-1{ background: orange; }
        .high-light-2{ background: yellow; }
        .high-light-3{ background: green; }
        .high-light-4{ background: blue; }
    </style>
    <script src="jquery-3.4.1.js"></script>
    <script>
        $(document).ready(function(){
            $('h1').each(function(index, item){
                $(this).addClass(`high-light-${index}`); //item 대신 this 사용
            });
        });
    </script>
</head>
<body>
    <h1>item - 0</h1>
    <h1>item - 1</h1>
    <h1>item - 2</h1>
    <h1>item - 3</h1>
    <h1>item - 4</h1>
</body>
</html>
```



참고로 `addClass()` 메서드의 매개변수에 함수를 입력할 수 있다.
대부분의 jQuery 메서드들은 이와 같은 특성이 있다.

```html
<script>
	$(document).ready(function(){
        $('h1').addClass(function(index){
            return `high-light-${index}`;
        });
    });
</script>
```

---



## 13.9 객체 확장

`$.extend()` 메서드는 정말 중요한 메서드지만, 내용 자체는 간단하다.
JS로 빈 객체를 생성하고 새 속성을 추가할 때, 추가할 속성의 수가 많아지면 매우 귀찮고 코드가 지저분해진다.

```html
<script>
	$(document).readey(function(){
        var object = {};
        object.name = 'RinIanTta';
        object.gender = 'Male';
        object.part = 'Second Guitar';
    });
</script>
```



이러한 문제를 해결하기위해 만들어진게 `$.extend()` 메서드이다.

```html
<!-- [13-40] -->
<!-- $.extend() 메서드 -->
<script src="jquery-3.4.1.js"></script>
<script>
    $(document).ready(function(){
        var object = {
            name: '윤인성'
        };

        //$.extend() 메서드 사용
        $.extend(object, {
           region: '서울특별시 강서구',
           part: '세컨드 기타' 
        });

        var output = '';
        $.each(object, function(key, item){
            output += `${key} : ${item} \n`;
        });
        alert(output);
    });
</script>
```

---



## 13.10 jQuery 충돌 방지

jQuery 말고도 여러 가지 JS 프레임워크가 있다. 
여러 플러그인을 같이 사용하면 플러그인 간의 충돌이 발생한다.

충돌을 방지할 때 `$.noConflict()` 메서드를 사용한다.
하지만, `$.noConflict()` 메서드를 사용하면 더 이상 식별자 $ 를 사용할 수 없다.

```html
<script>
	$.noConflict();
    jQuery(document).ready(function(){
        
    });
</script>
```



간단하게 쓰고 싶다면 jQuery 객체를 다른 변수에 저장해서 사용하자.

```html
<script>
    $.noConflict();
    var J = jQuery;

    J(document).ready(function(){
        
    });
</script>
```

---



## 13.11 응용

#### 13.11.1 $.extend() 메서드를 사용한 옵션 객체 보완

jQuery를 사용해 무언가 만들 때 옵션 객체 초기화를 굉장히 많이 사용한다.

객체를 결합하는 `$.extend()` 메서드를 활용하면 간단하게 구현할 수 있다.
객체를 결합할 때, <u>속성 이름이 같으면 값이 덮어 씌워지는 특성</u>을 이용한다.
이를 활용하면 <u>'앞에 기본값 객체를 넣고, 뒤에 사용자 정의 객체'</u>를 넣어서 옵션 객체를 보완할 수 있다.

```html
<!-- [13-45] -->
<!-- $.extend()를 사용한 옵션 객체 보완 -->
<script src="jquery-3.4.1.js"></script>
<script>
    function test(options){
        //옵션 객체 초기화
        options = $.extend({
            //기본 값
            valueA: 10,
            valueB: 20,
            valueC: 30,
        }, options); //참고로 객체 하나만 넣으면 기본 값으로 깊은복사가 되어 버린다.
        
        alert(`${options.valueA} : ${options.valueB} : ${options.valueC}`);
    }

    test({
        valueA: 52,
        valueB: 273
    });
</script>
```

---

