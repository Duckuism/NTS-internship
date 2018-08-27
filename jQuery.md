# jQuery

## 사전 강의

~~~javascript
var $var1 = $(window);
var $var2 = $(window);
$var1 === $var2 // false; 윈도우의 인스턴스를 그 때 그 때 만들어서 반환
var $var3 = $var1;
$var3 === $var1 // true; 같은 인스턴스를 참조한다.
~~~

var1의 [window]와 var2의 [window]가 있을 때, 서로 다른 인스턴스이지만, 안의 window는 같은 것을 나타낸다.

~~~javascript
$(".btn").text("a");
$(".btn").atrr("title","b");
//불필요하게 객체를 한 번 더 만든다.
//체이닝을 통해 한 줄에 할 수 있다.
$(".btn").text("a").atrr("title","b");
~~~

그런데 위의 코드에서 text() 메서드는 대상 자체를 바꾸므로, 체이닝을 쓰면 초기 상태에서 메서드를 적용할 수 없다.

~~~javascript
var $btn = $("btn");

$btn.text("a");

$btn.attr("title","b");

//위의 코드처럼 하면 객체를 한 번 만들고, 초기 상태의 변수를 가지고 활용할 수 있으므로 조금 더 효율적
~~~

```$();``` 호출해도 빈 jQuery객체 [ ]를 반환한다. 메서드는 다 가지고 있을 것.

jQuery에 jQuery객체가 들어오면 jQuery객체 안에서 처리하려고 하는 대상을 뽑아서 다시 새로운 jQuery객체를 반환한다. window객체를 넣을 때 처럼 객체로 한 번 더 감싸서 나오는게 아니다.

~~~javascript
var $btn = $(".btn");
var $btn2 = $($btn); //[[btn, btn, btn, ...]]이 될 것 같지만 그냥 새로운 인스턴스를 만들어서 [btn, btn, btn, ...]로 반환한다. 즉 $(".btn");과 다를바가 없는것
var $btn === $btn2; // 같은 객체겠지만 새로만들어진 인스턴스 이므로 false.
~~~



~~~javascript
$('body') === document.body // false jQuery객체와 DOM 객체를 비교하는 것이므로.

var $body = $("body");

$body[0] === document.body // true jQuery객체의 첫 번째 객체인 DOM객체와 DOM객체를 비교하는 것이므로 
$body.get(0) //jQuey객체의 0번째 인자들을 반환. $body[0]과 같다.
$body.eq(0) //jQuery객체의 0번째 인자들을 모아서 jQuery객체로 반환
~~~

그렇다면, 왜 eq를 만들었을까? 
".btn"의 두 번째 인자만 가지고 하고 싶다.
```$($btn[0])```의 형식으로 사용해야 한다.
이렇게 쓰게 되면 모양이 좀 이상?하고 번거롭다. 그래서 이것을 줄인 것이 ```$.eq()```



* $(".btn").text()는 getter이므로 무조건 해당 값이 나온다. 
  * ```$btn.text().attr("...");```을 하면 $btn.text()의 결과값이 string이므로 attr 메서드를 적용할 수 없다는 에러가 나온다. 물론 String 객체에 있는 메서드는 사용할 수 있을 것.
* text("링크")는 setter이므로 적용된 모든 값이 "링크"로 바뀐다. 그리고 jQuery객체로 반환된다.



* attr("a") 파라미터 하나만 넣었을 때는 getter가 된다.
* attr("a,"b") 두 개 이상이면 setter로 동작해서 "a" 속성들이 모두 "b"로 바뀐다.



---

이벤트가 어떻게 처리되는지에 대한 원리를 이해하는 것이 중요.



---



## 기초

~~~javascript
//getter,setter 패턴 1 - 파라미터 유무
//setter
$("li").width(500); 
//getter
$("li").width(); 

//getter,setter 패턴 2 - 파라미터 개수
//setter
$("li").css("border","1px solid black"); 
//multiple setter 
$("li").css({
   "border":"1px solid red" 
}); 
//getter 
$("li").css("border") 
~~~

setter/getter의 구분

* 파라미터 개수 (css, attr, prop, offset, data...)
* 파라미터 여부 (val, height, width...)



setter의 경우에는 jQuery객체가 반환되므로 **메서드 체이닝**이 가능!

~~~javascript
$("li").width(500).height(500);
~~~



많이 사용하는 메서드들 ( jQuery cheat sheet - https://oscarotero.com/jquery/ )

- 돔을 제어 (append, prepend, html)
- 애니메이션 (animate, fadeOut, slideUp)
- 이벤트 (on, event object)
- Class 제어 (addClass, removeClass, toggleClass)
- Ajax($.ajax)



DOM이 들어가는 인자는 기본적으로 native element, html 문자, jQuery객체 모두 가능하다.

~~~javascript
var todo = document.getElementById('todo');

$("li") // selector - selector에 맞는 method실행
$(todo) // object - 해당 element의 method 실행
$("<div>foo</div>") // html string - element 생성

$("li").append(todo);
$("li").append("<div>foo</div>");


//위의 3가지 방법이 다 가능하므로 아래와 같은 코드를 짤 필요가 없다.
$($(todo))
$($("<div>foo</div>"))

~~~



## EVENT

~~~javascript
//브라우저 이벤트 등록
$("#add").on("click", function(e){
   console.log("click");
});

//브라우저 이벤트 해제 - #detach에 click event가 발생되면 #add에 click이벤트 바인딩을 해제.
$("#detach").on("click", function(e){
   	$("#add").off("click", function(e){
   		console.log("click");
	});
});

//하지만 위의 이벤트 해제는 정상적으로 동작하지 않는다. 함수의 내용은 같지만, 새로 만들어진 다른 인스턴스의 함수이기 때문이다. 만약 우리가 원하는 대로 해제하려면 함수를 미리 선언하고 이 함수의 참조값을 on에 매개변수로 넘겨서 이벤트를 지정했다가 off를 통해 다시 해제해야 한다.
function click(e){
    console.log("click");
}

$("#add").on("click", click);

$("#detach").on("click", function(e){
   	$("#add").off("click", click);
});
~~~

#### jQuery에서의 load/DOMContentLoaded 활용

native에서는 DOMContentLoaded이지만 jQuery에서는 ready라는 이름으로 쓰인다.

~~~javascript
$(window).on("load", function(){
    console.log("load");
});
$(document).on("ready", function(){//ready event를 쓰는 방법 1
    console.log("ready");
});

//일부메서드에 대해서는 이벤트명으로 함수가 등록이 되어있다.
$(window).load(function(){
    console.log("load");
});
$(window).click(function(){
    console.log("click");
});
$(window).ready(function(){//ready event를 쓰는 방법 2
    console.log("ready");
});

$(function(){//ready event를 쓰는 방법 3
    console.log("ready");
});
~~~

#### Event delegation

~~~javascript
$("#todolist").on("click", function(){
    console.log(e.target);
    //아래와 같이 input에만 event를 건다.
    if(e.target.tagName === "INPUT"){
        console.log("checkbox");
    }
});

//위 코드를 jQuery에서 좀 더 세련되게 두 번째 인자로 셀렉터를 넣을 수 있다.
$("#todolist").on("click","input", function(3){
    console.log("checkbox");
});
~~~



## Attribute

#### jQuery에서 속성을 다루는 방법

* attr/removeAttr
* prop/removeProp
* hasClass/addClass/removeClass/toggleClass
* data

~~~javascript
//id 값을 갖고 오고 싶다.
$("#check").attr("id");
//id 값을 변경하고 싶다.
$("#check").attr("id","select");

//id 값을 갖고 오고 싶다.
$("#check").prop("id");
//id 값을 변경하고 싶다.
$("#check").prop("id","select");
~~~

**prop과 attr의 차이는?** attr같은 경우는 해당 html을 가지고 온다고 보면 된다. 따라서 html에 있는 속성은 가지고 올 수 있지만 동적으로 추가되고 사라지는 checked같은 상태 속성은 가지고 올 수 없고, prop은 해당 DOM element를 가져온다. ```console.dir($("#check")[0]);```을 하면 값이 나오는데, prop는 이 값들의 속성들을 모두 control한다고 보면 된다. 

~~~javascript
//check됐는지 안 됐는지 알고 싶을 때.
$("#check").attr("checked"); //무조건 undefined
$("#check").prop("checked"); //true/false. 
~~~

대부분은 prop을 쓰고, custom attributes를 쓸 때만 attr를 쓴다고 보면 된다. 

~~~html
<!-- foo라는 커스텀 속성 추가 -->
<input type="checkbox" id="check" foo="bar">
~~~

위의 html에서 prop은  DOM element에 foo라는 속성이 없으므로 읽을 수 없다. 이 때는 attr로만 읽을 수 있다. 

속성 삭제는 removeAttr, removePropd로만 가능하다.

#### 클래스 조작

~~~java
//class 삭제
$("#foo").removeClass("checked");
//class 추가
$("#foo").addClass("checked");
//class 존재여부확인
$("#foo").hasClass("checked");
//checked class가 있으면 class 삭제, 없으면 checked class 추가
$("#foo").toggleClass("checked");
~~~

#### 데이터 조작

웹 어플리케이션이 점점 복잡해짐에 따라서 html에 데이터 성격을 갖는 값들을 지정해야되는 상황이 발생한다. 

예를 들어, 메일함을 클릭하면 메일함의 특정 키 값을 가지고 관련된 메일을 보여준다고 했을 때, 각각의 메일함에는 고유 키 값을 가지고 있는 것이 유리하다. 

~~~html
<!--예전에 사용하던 방법-->
<ul>
    <li mailbox-id="1"><a href="#">나한테 보낸 메일</a></li>
    <li mailbox-id="2"><a href="#">명세서</a></li>
    <li mailbox-id="3"><a href="#">기타 메일</a></li>    
</ul>
~~~

위의 코드에서 특정 html을 가져오려면

~~~javascript
$("ul li:first").attr("mailbox-id");
~~~

위의 jQuery코드를 사용한다. 그러나 이렇게 하면 mailbox라는 중복되는 텍스트를 계속 선언해야된다. 이를 해결하기 위해 html5에서는 data라는 prefix를 붙이도록 하였다. 

~~~html
<!--예전에 사용하던 방법-->
<ul>
    <li data-id="1"><a href="#">나한테 보낸 메일</a></li>
    <li data-id="2"><a href="#">명세서</a></li>
    <li data-id="3"><a href="#">기타 메일</a></li>    
</ul>
~~~

~~~javascript
$("ul li:first").data("id"); //1
$("ul li:first").data("id","3"); //[<li data-id="1">...</li>]
$("ul li:first").data("id"); //3
//attribute에 객체를 넣을 수도 있다.
$("ul li:first").data("id", [1,2,3])
$("ul li:first").data("id"); //[1,2,3]
$("ul li:first").data({
    "id":1,
    "test":2
}); //[<li data-id="1">...</li>]
$("ul li:first").data("id"); //1
$("ul li:first").data("test"); //2
//모든 값을 가져오고 싶다면 데이터에 빈 값을 넣는다.
$("ul li:first").data(); //Ojbect{id: 1, test: 2}
~~~



## DOM insertion

DOM을 추가하는 방법을 알아본다.

* append/prepend
* appendTo/prependTo
* html/text

~~~javascript
//DOM 추가
var html = "<li><input type=\"checkbox\"/><span>할 일 하기3</span></li>";
$("#todolist").append(html); //마지막 자식노드로 붙인다.
$("#todolist").prepend(html); //맨 앞 자식노드로 붙인다.
//html도 들어가지만 element 자체도 들어간다.
var ele = document.createElement("li");
$("#todolist").prepend(ele);
//jQuery객체도 들어간다.
var $ele = $("#todo");
$("#todolist").prepend($ele);

//to가 붙으면 반대로 붙인다고 생각하면 된다.
//html코드의 위치를 바꿔서 넣는다.
//html코드는 안들어가고 셀렉터와 해당 element, jQuery 인스턴스가 들어간다. 
$(html).prependTo(ele);


~~~

위치를 바꿔서 동작하는 것은 알겠는데, 굳이 prepend 대신 prependTo를 쓰는 시점이 언제일까?
html을 붙인다음, html DOM에 행위를 추가할 때 To 메서드를 활용한다.
현재 우리는 todolist에 li를 붙인 다음 이 붙인 li를 애니메이션해서 스르륵 내려가는 기능을 만들고 싶다.

기존에 prepend를 사용하면 코드는 다음과 같다.

~~~javascript
//먼저 ID값을 넣고 slideDown 애니메이션은 기본상태가 보여지지 않는 상태여야 하므로 display:none을 인라인으로 추가.
var html = "<li><input id='foo' style=\"display:none;\" type=\"checkbox\"/><span>할 일 하기3</span></li>";
//todolist에 id값이 지정된 html을 넣고 
$("#todolist").prepend(html); 
//다시 id값으로 찾은 후 애니메이션 지정.
$("#foo").slideDown();
~~~

prependTo를 사용하면 코드는 다음과 같다. 행위는 똑같지만 코드가 단순해졌다. 

~~~javascript
//애니메이션을 적용할 element를 붙이고 다시 찾기 위한 id를 넣을 필요가 없다.
var html = "<li><input style=\"display:none;\" type=\"checkbox\"/><span>할 일 하기3</span></li>";
//다음과 같이 위치가 바뀌어서 메서드 체이닝이 가능해지기 때문이다. 코드도 간결해지고 효율적이 된다.
$(html).prependTo("#todolist").slideDown();
~~~

prependTo는 결국 부모와 자식의 위치만 바꿔서 기술하는 코드가 아니라, 붙인 자식에 대해 행위를 연결해서 쓸 때 사용하는 것.



#### html&text

html : prepend나 append 같은 경우는 밑의 자식노드가 이미 있는 상태에서 element를 추가한다. 그러나 특정 element자체에 아무 것도 없는 상태에서 html태그 자체를 바꾸고 싶을 때 쓴다.

~~~html
<ul>
    <li><input type="checkbox"/><span>할 일 하기</span></li>
    <li><input type="checkbox"/><span>할 일 하기2</span></li>
</ul>
~~~

html은 html코드가 전체적으로 ul밑으로 들어간다. (기존에 있던 것들은 모두 대체 되는 것.)

~~~javascript
var html = "<li><input type=\"checkbox\"/><span>할 일 하기3</span></li>";
$("#todolist").html(html);
~~~

text : escape된 html코드 문자열을 넣는다고 보면 된다. 즉, html 문자열을 넣는 것.



## Event Object

jQuery 이벤트를 활용하면서 첫 번째 인자로 들어오는 것이 이벤트 객체이다. 이 객체에는 어떤 속성들이 있는지 알아본다. event객체가 native객체라고 착각할 수도 있는데, jQuery 객체다.

* which/keyCode
* target/currentTarget(this)
* delegate Target/relatedTarget
* preventDefault/stopPropagation

~~~javascript
$("li input").on("click", function(event){
    console.log(event);
});

//which/keyCode : key event가 발생했을 때 해당 눌린 key의 값을 알아내는 속성. (keydown, keypressup등)
$("#todo").on("keydown", function(event){
    console.log(event.which); //keyboard에 mapping된 값이 출력 됨.
    console.log(event.keyCode); //keyCode는 표준이 아니므로 브라우저마다 다른 값이 출력될 수 있다. which를 사용할 것.
}); 

//target/currentTarget의 차이 : 일반 target은 실제 event를 발생시킨 element이고, currentTarget는 이벤트가 실제 바인딩 된 element이다.
$("#todolist").on("click", function(event){
    console.log("target %o", event.target); //%o를 하면 객체를 템플릿형태로 보여주어서 좀 더 편하게 로깅할 수 있다.
    console.log("target %o", event.currentTarget);
    console.log(this); //jQuery에서 event객체 안에서의 this는 항상 event.currentTarget을 가리킨다. 따라서 currentTarget 대신 this를 많이 사용한다.
});
~~~



## Traversing

DOM을 탐색하는 traversing에 대해 알아본다. 우리가 일반적으로 element를 탐색할 때 가장 많이 쓰는 방법이 $(셀렉터)인데,  가끔 사용하다 보면 추가적으로 DOM을 탐색하고 싶은 사항이 있다. 그 때 쓰는 메서드들을 알아본다.

* closest/parent/parents
* find
* get/eq

#### closest/parent/parents

~~~javascript
//부모 element를 싶을 때
$("#bar12").parent();
//부모 element에 맞는 선택자를 가진 element를 찾을 때
$("#bar12").parent("ul");
//parents에 셀렉터를 넣지 않으면 root까지 찾아 올라간다.
$("#bar12").parents();
//특정 셀렉터를 넣으면 끝까지 올라가서 셀렉터에 맞는 요소를 반환한다.
$("#bar12").parents("ul");
~~~

parent와 parents는 둘 다 부모 element를 찾는데, parent는 해당 요소에 가장 근접해있는 상위 부모를 찾고, parents는 root까지 올라갔다가 셀렉터에 맞는 조건인 부모를 찾는 것. 일반적으로는 위와 같이 사용하면 되지만, 문제가 되는 상황은 다음과 같다.

~~~javascript
//해당 요소 위에 위에 있는 element를 찾고 싶을 때
$("#bar11").parent("ul"); //[] parent는 1단계 위 요소까지만 찾기 때문에 조건에 만족하는 2번째 부모 요소를 찾을 수가 없다.
//이러한 문제를 해결하려고 하는 일반적인 방법
$("#bar11").parent().parent(); //2단계를 올라감. 하지만 단계가 높아지면 코드도 길어지고 번거로워 진다.
//가까운 특정 부모 element를 찾을 때 쓰는 것이 closest
$("#bar11").closest("ul");
~~~

closest와 parent의 차이는 parent같은 경우는 바로 한 단계 위에 있는 부모를 찾을 때 쓰고, closest는 해당 요소를 기준으로 가장 가까운 element를 찾는 다고 보면 된다. parents는 내 요소의 부모를 계속 올라가서 root까지 올라간다. 일반적으로 closest를 가장 많이 쓴다.

parent는 부모 element부터 찾지만, closest는 본인 element부터 찾는다. 

#### find

css()를 적용하고, 적용한 요소 안의 또 다른 요소에 css()를 적용하고 싶을 때가 있다.

~~~javascript
//일반적인 방법.코드가 길고 번거롭다.
$("#depth2").css("background-color","red");
$("#depth3").css("background-color","blue");

//depth2의 색상을 바꾸고, 바꾼 depth2를 기준으로 자식 요소 중에 depth3를 찾아서 색상을 바꾼다. 위의 코드는 문서 전체에서 찾지만, 아래 코드는 자식 코드에서 바로 찾으므로 좀 더 효율적이다.
$("#depth2").css("background-color","red").find("#depth3").css("background-color","blue");

//클래스 자손 선택자 등을 쓸 때 유용하다.
$("#depth2").css("background-color","red");
$("#depth2 li").css("background-color","blue");

$("#depth2").css("background-color","red").find("li").css("background-color","blue");
~~~

#### get/eq

같은 태그 요소가 여러 개 있을 때, 가끔 index에 맞는 태그만 찾고 싶을 때가 있다. jQuery 객체는 key와 value를 갖는 object인데, key 값이 index로 되어 있다. 그래서 배열처럼 사용할 수 있는 유사배열이다.

~~~javascript
$("li")[0] //그런데 이 element는 jQuery instance가 아니라 native element이다. 따라서 체이닝을 통해 jQuery메서드를 연속으로 쓸수 없다. 이 코드는 document.getElementsByTagName("li")[0]과 같다.
$("li").get(0) //마찬가지로 native element. $("li")[0]과 같다.
$("li")[0].find() // 에러. $(...)[].find is not a function(...)
~~~

그런데, native element가 아니라 jQuery instance를 반환하고 싶을 때가 있다. 만약 get()메서드를 체이닝을 통해 쓰려면 아래 코드와 같이 써야한다.

~~~javascript
$($("li").get(0)).find("ul");

//이 때 간편하게 jQuery instance를 반환해줘서 메서드 체이닝을 가능하게 하는 메서드가 바로 eq()이다.
$("li").eq(0).css("color","red");
~~~

## Effect

jQuery를 통해 element에 효과를 줄 수 있는 방법을 알아본다.

* show/hide/toggle
* slideDown/slideUp
* animate

#### show/hide/toggle

일반적으로 어떤 element를 숨기고 싶을 때는 다음과 같이 많이 쓴다.

~~~javascript
$("#foo").hide();
$("#foo").show();

//위의 두 코드를 합친 기능.
$("#foo").toggle();
~~~

모든 effect에는 공통적인 규칙이 존재한다.

* 첫 번째 인자로 숫자가 들어가면 duration을 지정할 수 있다. 문자가 들어가면 정해진 ms로 animate가 실행된다.

  ~~~javascript
  //기본은 400
  $("#foo").hide(600); //좀 더 느리게 사라진다.
  $("#foo").show(600); //좀 더 느리게 나타난다.
  
  $("#foo").hide("slow"); //좀 더 느리게 사라진다.
  $("#foo").hide("fast"); //좀 더 빠르게 사라진다.
  ~~~

* 두 번째 인자로 함수가 들어간다. animation이 끝나고 실행되는 콜백 함수이다.

  ~~~javascript
  $("#foo").show(600, function(){
      console.log("done");
  });//element가 사라지고 done이 출력됨.
  ~~~

Show, hide는 auto layouting이 일어나므로 성능면에서 좋지 않다. 따라서 hide class를 따로 만들어서 display:none; 속성을 이용하는 것을 추천한다.



#### slideDown/slideUp

~~~javascript
//위로 올라간다.
$("#foo").slideUp("slow");
//아래로 내려간다.
$("#foo").slideDown("slow");
~~~


