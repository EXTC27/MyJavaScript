# 14장. jQuery 문서 객체 선택과 탐색

## 14.1 기본 필터 메서드

jQuery의 선택자를 사용하면 원하는 문서 객체를 대부분 선택할 수 있다.

기본적으로 지원하지 않는 필터로 문서 객체를 선택해야 하면 `filter()` 메서드를 사용해야 한다.
`filter()` 메서드는 문서 객체를 필터링해주며, 두 가지 형태로 사용된다.

	1. `$(선택자).filter(선택자);`
 	2. `$(선택자).filter(function(){});`

첫 번째 형태는 13장에서 공부한 **필터 선택자**와 동일하다고 생각하면 된다. 
예를 들어 `filter()` 메서드의 매개변수로 `:even` 과 같은 선택자를 넘겨주면 된다.

```html
<!-- [14-03] -->
<!-- filter() -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        //$('h3:even').css(...); 와 같다.
        $('h3').filter(':even').css({
            backgroundColor: 'black',
            color: 'white'
        });
    });
</script>   
<body>
    <h3>Header - 0</h3>
    <h3>Header - 1</h3>
    <h3>Header - 2</h3>
    <h3>Header - 3</h3>
    <h3>Header - 4</h3>
    <h3>Header - 5</h3>
</body>
```



두 번째 형태는 <u>매개변수의 함수가 리턴하는 값에 따라 문서 객체를 선택</u>한다.

```html
<!-- [14-04] -->
<!-- filter 메서드의 매개변수로 함수를 넣는 경우 -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        $('h3').filter(function(index){
            return index % 3 == 0;
        }).css({
            backgroundColor: 'black',
            color: 'white'
        });
    });
</script>
<body>
    <h3>Header - 0</h3>
    <h3>Header - 1</h3>
    <h3>Header - 2</h3>
    <h3>Header - 3</h3>
    <h3>Header - 4</h3>
    <h3>Header - 5</h3>
</body>
```

---



## 14.2 문서 객체 탐색 종료

굳이 첫 번째 형태의 `filter()` 메서드를 왜 사용하냐?

**체이닝**을 사용해서 <u>한 줄로 서로 다른 문서 객체를 선택</u>할 수 있으므로 <u>코드가 간결</u>해지기 때문이다.

```html
<!-- [14-05] -->
<!-- filter() 메소드의 체이닝 -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        $('h1').css('background', 'orange').filter(':even').css('color', 'red');
    });
</script>
<body>
    <h1>Header-0</h1>
    <h1>Header-1</h1>
    <h1>Header-2</h1>
</body>
```

하지만, 이 방법은 <u>문서 객체의 범위를 점점 좁게만 선택</u>할 수 있다.

`end()` 메서드는 문서 객체 선택을 한 단계 뒤로 돌린다.
`end()` 를 사용해서 더 많은 문서 객체를 체이닝으로 선택할 수 있다.

```html
<!-- [14-06] -->
<!-- end() 메서드와 체이닝을 활용한 선택 -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        $('h1').css('background', 'orange').filter(':even').css('color', 'red').end()
        .filter(':odd').css('color', 'blue');
    });
</script>
<body>
    <h1>Header-0</h1>
    <h1>Header-1</h1>
    <h1>Header-2</h1>
</body>
```

`end()` 메서드는 `filter()` 뿐만 아니라 선택과 탐색에 관련된 모든 메서드에 적용할 수 있다.

---



## 14.3 특정 위치의 문서 객체 선택

**필터 선택자**를 이용하면 <u>특정 위치에 존재하는 문서 객체를 선택</u>할 수 있다.
특정 위치에 존재하는 문서 객체를 선택하는 필터 선택자는 자주 사용되서 아예 메서드로 제공한다.

| 메서드 이름 | 설명                                       |
| ----------- | ------------------------------------------ |
| `eq()`      | 특정 위치에 존재하는 문서 객체를 선택한다. |
| `first()`   | 첫 번째에 위치하는 문서 객체를 선택한다.   |
| `last()`    | 마지막에 위치하는 문서 객체를 선택한다.    |



`eq()` 메서드는 매개변수에 숫자를 입력한다. 
음수를 입력할 수도 있다. 음수를 입력하면 뒤쪽을 기준으로 선택한다.

```html
<!-- [14-09] -->
<!-- eq() -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        $('h1').eq(0).css('background', 'orange');
        $('h1').eq(-1).css('background', 'red ');
    });
</script>
<body>
    <div>
        <h1>Header-0</h1>
        <h1>Header-1</h1>
        <h1>Header-2</h1>
    </div>
</body>
```

---



## 14.4 문서 객체 추가 선택

`add()` 메서드를 사용하면 <u>현재 선택한 문서 객체의 범위를 확장</u>할 수 있다.

```html
<!-- [14-10] -->
<!-- add() -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        $('h1').css('background', 'Gray').add('h2').css('float', 'left');
    });
</script>
<body>
    <h1>Header-0</h1>
    <h2>Header-1</h2>
    <h1>Header-2</h1>
    <h2>Header-3</h2>
    <h1>Header-4</h1>
</body>
```

---



## 14.5 문서 객체의 특징 판별

`is()` 메서드는 <u>문서 객체가 특징이 있는지 판단</u>할 때 사용한다. 매개변수로 선택자를 입력한다.
boolean 자료형을 리턴한다.

```html
<!-- [14-13] -->
<!-- is() -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        $('h1').each(function(){
            if($(this).is('.select')){
                $(this).css('background', 'orange');
            }
        });
    });
</script>
<body>
    <h1 class="select">Header-0</h1>
    <h1>Header-1</h1>
    <h1 class="select">Header-2</h1>
</body>
```

---



## 14.6 특정 태그 선택

`find()` 메서드를 사용하면 특정 태그를 선택한다. XML 문서에서 데이터를 추출할 때 많이 사용된다.

```html
<!-- [14-17] -->
<!-- XML 파싱 -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    var xml = '';
    xml += '<friends>';
    xml += '    <friend>';
    xml += '        <name>연하진</name>';
    xml += '        <language>Ruby</language>';
    xml += '    </friend>';
    xml += '    <friend>';
    xml += '        <name>윤명월</name>';
    xml += '        <language>Basic</language>';
    xml += '    </friend>';
    xml += '    <friend>';
    xml += '        <name>윤하린</name>';
    xml += '        <language>C#</language>';
    xml += '    </friend>';
    xml += '</friends>';

    $(document).ready(function(){
        var xmlDoc = $.parseXML(xml);
        $(xmlDoc).find('friend').each(function(index){
            var output = '';
            output += '<div>';
            output += `<h1>${$(this).find('name').text()}</h1>`;
            output += `<p>${$(this).find('language').text()}</p>`;
            output += '</div>';

            document.body.innerHTML += output;
        });
    });
</script>
<body>
    
</body>
```

---



## 14.7 응용

#### 14.7.1 parent() 메서드

특정 태그의 부모 태그를 선택하는 메서드이다.

```html
<!-- [14-19] -->
<!-- parent() 메서드와 이벤트ㄴ -->
<script src="jquery-3.4.1.min.js"></script>
<body>
    <script>
        $(document).ready(function(){
            $('span').parent().css('background', 'red');
            $('button').click(function(){
                $(this).text('비활성화하기');
                $(this).parent().css('background', 'red');
                $(this).parent().find('span').text('활성화');
            });
        });
    </script>
    <div>
        <span>비활성화</span>
        <button>활성화하기</button>
    </div>
    <div>
        <span>비활성화</span>
        <button>비활성화하기</button>
    </div>
</body>
```

`parent()` 속성은 이렇게 다양한 문서 객체 탐색 메서드와 조합하는 경우가 많다.

---

