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

