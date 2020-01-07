# 15장. jQuery 문서 객체 조작

## 15.1 개요

기존의 JS만으로 DOM을 다루려면 정말 복잡하다. jQuery를 사용하면 쉽게 다룰 수 있다.

---



## 15.2 문서 객체의 클래스 속성 추가

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

---



