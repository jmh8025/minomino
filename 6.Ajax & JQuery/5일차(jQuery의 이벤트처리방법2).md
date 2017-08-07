5일차(jQuery의 이벤트처리방법2)
===============================

---

04remove.html
-------------

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jquery예제(특정태그의 내용을 삭제(remove,empty))</title>
<script type="text/javascript"
            src="http://code.jquery.com/jquery-3.2.1.min.js">
</script>
<script>
$(function(){
	//$('#button1').bind('click',function(){
	$('#button1').click(function(){
		$('#div1').remove()//$(선택자).remove()->전체 내용 삭제(ex 폴더의 파일까지)
	})
	//
	$('#button2').click(function(){
		$('#div1').empty()//$(선택자).empty()->특정태그의 자식태그을 모두 제거
	})
	//
	$('#button3').click(function(){
		$('.test').empty()//특정 자식태그의 내용 삭제
	})
})
</script>
</head>
<body>
  <div id="div1" style="height:100px;width:300px;
                                   border:1px solid black;background-color:yellow" >
     <p class="test" style="height:50px;width:50px;
                                   border:1px solid red;background-color:yellow">
          div first</p>
     <p>div second</p>                              
  </div><br>
  <button id="button1">Remove div</button>
  <button id="button2">Empty div</button>
  <button id="button3">Empty p</button>
</body>
</html>
```

#### 전체내용을 삭제

```JavaScript
$(선택자).remove()
```

#### 자식태그를 모두삭제 및 특정 자식태그를 제거

```JavaScript
$(선택자).empty()
```

---

05event.html
------------

#### $(event.target)와 같다(이벤트를 발생시킨 객체를 의미함)

```JavaScript
var $target=$(this)
```

#### 타겟의 크기를 두배로 해줘!!

```JavaScript
$target.width($target.width()*2)
```

#### 더이상 타겟의 이벤트를 실행시키지맛!

```JavaScript
$target.unbind()
```

#### 그런데 .. 타겟을 크기를 한번만 두배로 한다면 bind를 시키고 unbind를 해서 해제 시키지말고 다른방법은 없을까 ?

#### 방법 !

```JavaScript
$('img').bind('dblclick',function(event){ 을

$('img').one('dblclick',function(event){ 로 변경합니다!
```

---

06hover.html
------------

css삽입

```javascript
 $(this).addClass(css구문)
```

css제거

```javascript
 $(this).removeClass(css구문)
```

---

07keyup.html
------------

#### 키입력시 이벤트호출

```javascript
$('input').keyup(function(){
이벤트 호출구문
})
```

#### 태그에 값을 넣기

```javascript
$(대상).text(넣고싶은문장)
```

---

08change.html
-------------

#### 셀렉문이 바뀔때마다

```javascript
$('#셀렉문').change(function(){
이벤트 구문
})
```

---

09formevent.html
----------------

#### 커서가 들어갔을때 (focus)

```javascript
$('input').focus(function(){

	})
```

#### 커서를 벗어났을때

```javascript
$('input').blur(function(){

    })
```

#### ul안에다가 삽입

```javascript
var new_line=$('<li>'+$('#data').val()+'</li>')
		$('#disp').append(new_line)
event.preventDefault();//전송을 하지 못하게 설정
```

cf) 이벤트 동작정지 함수들

| 함수명                           | 설명                                                                                                                |
|----------------------------------|---------------------------------------------------------------------------------------------------------------------|
| event.preventDefault()           | 현재 이벤트의 기본 동작을 중단한다.                                                                                 |
| event.stopPropagation()          | 현재 이벤트가 상위로 전파되지 않도록 중단한다.                                                                      |
| event.stopImmediatePropagation() | 현재 이벤트가 상위뿐 아니라 현재 레벨에 걸린 다른 이벤트도 동작하지 않도록 중단한다.                                |
| return false                     | jQuery를 사용할 때는 위의 두개 모두를 수행한 것과 같고, jQuery를 사용하지 않을 때는 event.preventDefault() 와 같다. |
