# 15장. jQuery 문서 객체 조작

## 15.1 개요

기존의 JS만으로 DOM을 다루려면 정말 복잡하다. jQuery를 사용하면 쉽게 다룰 수 있다.

---



## 15.2 문서 객체의 클래스 속성 추가

#### `addClass()`

`addClass()` 메서드는 <u>문서 객체의 클래스 속성을 추가</u>한다.

```html
<!-- [15-02] -->
<!-- addClass() -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        $('h1').addClass('item');
    });
</script>
<body>
    <h1>Header-0</h1>
    <h1>Header-1</h1>
    <h1>Header-2</h1>
</body>
```

![image-20200107155106976](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\image-20200107155106976.png)



함수를 매개변수로 입력할 수 있다. 이 때 콜백 함수는 `index` 매개변수를 갖는다. 선택자로 선택한 문서 객체에 차례대로 속성을 지정할 수 있다.

```html
<!-- [15-03] -->
<!-- addClass() 메서드의 콜백 함수 -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        $('h1').addClass(function(index){
            return 'class' + index;
        });
    });
</script>
<body>
    <h1>Header-0</h1>
    <h1>Header-1</h1>
    <h1>Header-2</h1>
</body>
```

---



## 15.3 문서 객체의 클래스 속성 제거

#### `removeClass()`

`removeClass()` 메서드는 <u>문서 객체의 클래스 속성을 제거</u>한다.
매개변수에 아무것도 입력하지 않으면, 문서 객체의 모든 클래스를 제거한다.

```html
<!-- [15-05] -->
<!-- removeClass() -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        $('h1').removeClass('select');
    });
</script>
<body>
    <h1 class="item">Header-0</h1>
    <h1 class="item select">Header-1</h1>
    <h1 class="item">Header-2</h1>
</body>
```

![image-20200107165210653](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\image-20200107165210653.png)



> > > **toggleClass()**
> > >
> > > `toggleClass()` 메서드는 매개변수로 입력한 클래스 속성이 이미 지정되어 있으면 제거하고, 지정되어 있지 않으면 추가한다.

---



## 15.4 문서 객체의 속성 검사

#### `attr()`

`attr()` 메서드는 <u>속성과 관련된 모든 기능을 수행</u>한다.

```html
<!-- [15-07] -->
<!-- attr() - Getter -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        var src = $('img').attr('src');
    
        alert(src);
    });
</script>

<body>
    <img src="mountain1.jpg"/>
    <img src="mountain2.jpg"/>
    <img src="mountain3.jpg"/>
</body>
```

`img` 태그가 3개나 있지만, 실행하면 첫 번째 `img` 태그의 `src` 속성을 출력한다.

대부분의 값을 알아내는 메서드는 <u>여러 개의 문서 객체를 선택</u>했을 때 <u>첫 번째에 위치한 문서 객체의 값을 출력</u>한다.

---



## 15.5 문서 객체의 속성 추가

객체 속성을 추가할 때도 `attr()` 메서드를 사용한다.

다음 세 가지 형태로 사용할 수 있다.

1. `$(selector).attr(name, value);`
2. `$(selector).attr(name, function(index, attr){ });`
3. `$(selector).attr(object);`



1번 형태의 경우, 첫 번째 매개변수에는 속성 이름을 입력하고, 두 번째 매개변수에는 값을 넣어준다.

```html
<!-- [15-09] -->
<!-- attr() - Setter 1 -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        $('img').attr('width', 200);
    });
</script>

<body>
    <img src="mountain1.jpg"/>
    <img src="mountain2.jpg"/>
    <img src="mountain3.jpg"/>
</body>
```

![image-20200110160302861](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\image-20200110160302861.png)



2번 형태의 경우, 첫 번째 매개변수에는 속성의 이름을 입력하고, 두 번째 매개변수에는 index 매개변수를 갖는 함수를 입력한다. `addClass()` 와 비슷한 내용이다.

```html
<!-- [15-10] -->
<!-- attr() - Setter 2 -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        $('img').attr('width', function(index){
            return (index + 1) * 100;
        });
    });
</script>

<body>
    <img src="mountain1.jpg"/>
    <img src="mountain2.jpg"/>
    <img src="mountain3.jpg"/>
</body>
```

![image-20200110161344257](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\image-20200110161344257.png)



3번 형태의 경우, 매개변수로 입력한 객체에 키로 속성의 이름을 지정하고 값을 입력한다.
값을 입력할 때는 1번 형태와 2번 형태를 모두 사용할 수 있다.

```html
<!-- [15-11] -->
<!-- attr() - Setter 3 -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        $('img').attr({
            width: function(index){
                return (index + 1) * 100;
            },
            height: 100
        });
    });
</script>

<body>
    <img src="mountain1.jpg"/>
    <img src="mountain2.jpg"/>
    <img src="mountain3.jpg"/>
</body>
```

![image-20200110162119275](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\image-20200110162119275.png)

---



## 15.6 문서 객체의 속성 제거

#### `removeAttr(name)`

`removeAttr(name)` 메서드는 <u>문서 객체의 속성을 제거</u>한다.

첫 번째 매개변수에 삭제하려는 속성의 이름을 입력한다.

```html
<!-- [15-13] -->
<!-- removeAttr() -->
<script src="jquery-3.4.1.min.js"></script>
<script>
    $(document).ready(function(){
        $('h1').removeAttr('data-index');
    });
</script>

<body>
    <h1 data-index="0">Header-0</h1>
    <h1 data-index="1">Header-1</h1>
    <h1 data-index="2">Header-2</h1>
</body>
```

![image-20200110172527236](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\image-20200110172527236.png)

> > >  **`removeAttr()` 와 `removeClass()`**
> > >
> > > 두 메서드 모두 클래스 속성을 제거할 수 있다. 차이점은 다음과 같다.
> > >
> > > - `removeAttr()` : 모든 클래스 속성이 한 번에 제거.
> > > - `removeClass()` : 여러 개의 클래스 속성 중에서 선택적으로 제거.

---



## 15.7 문서 객체의 스타일 검사

#### `css()`

`css()` 메서드는 <u>스타일과 관련된 모든 기능을 수행</u>한다.

```html
<!-- [15-15] -->
<!-- css() - Getter-->
<!DOCTYPE html>
<html>
<head>
    <style>
        .first{ color: red; }
        .second{ color: pink; }
        .third{ color: orange; }
    </style>
    <script src="jquery-3.4.1.min.js"></script>
    <script>
        $(document).ready(function(){
            var color = $('h1').css('color');

            alert(color);
        });
    </script>
</head>

<body>
    <h1 class="first">Header-0</h1>
    <h1 class="second">Header-1</h1>
    <h1 class="third">Header-2</h1>
</body>
</html>
```

