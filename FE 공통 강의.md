## Module pattern

자바스크립트 코드를 글로벌로 함수나 변수를 만들게 되면 다른 파일들과 합치거나 같이 로딩했을 때 이름이 서로 겹쳐 문제가 날 확률이 높고, 이를 찾기란 어렵다. 그래서 자바스크립트 코드를 모듈화 하여 외부에 private하게 함수나 변수를 사용한다. 

~~~javascript
function add(){
    baz();
}

function destroy(){
    
}

function baz(){
    
}

function init(){
    document.getElementById('foo').addEventListener("click", add);
    document.getElementById('bar').addEventListener("click", destroy);
}

document.addEventListener("DOMContentLoaded", function(){
    init();
});
~~~
위의 코드는 짧아서 한 눈에 파악이 가능하지만, 코드량이 많아서 여러 개의 파일로 나누어져 있다면 실수로 똑같은 이름의 함수를 정의해서 덮어씌울 수도 있다. 이렇게 되면 문제가 생기므로, 아얘 방지해야한다.

* 즉시 실행함수(Immediately-invoked function expression)

  자바스크립트는 함수 단위로 scope가 생성되므로 익명함수 안의 변수들을 외부에서 쓸 수 없다. 즉 실행할 때만 쓰이고 외부로는 노출되지 않는다.

  ~~~javascript
  (function(){
      console.log("run");
  })();
  
  !function(){
      console.log("run");
  }();
  ~~~

  

* 모듈 패턴(module pattern)

  즉시 실행 함수 안에 메서드들을 선언한다.

  ~~~javascript
  (function(){
      function add(){
      	baz();	
      }
  
      function destroy(){
  
      }
  
      function baz(){
  
      }
  
      function init(){
          document.getElementById('foo').addEventListener("click", add);
          document.getElementById('bar').addEventListener("click", destroy);
      }
  })();
  
  console.log(add());//접근불가
  console.log(destroy());//접근불가
  
  //함수 바깥의 scope이므로 익명 함수 안의 메서드 add와 다른 scope에서 새로운 add 메서드를 선언하게 된다.
  function add(){
      
  }
  
  document.addEventListener("DOMContentLoaded", function(){
      init();
  });
  ~~~

  위의 코드에서는 DOMContentLoaded가 되고나서 실행되는 init()이 외부로 노출되지 않았다. 하지만 노출해야하는 필요가 있을 때는 다음과 같이 함수를 변수에 할당한 후, 함수의 반환값으로 노출하고자 하는 값을 반환해서 접근하면 된다.

    ~~~javascript
  var Foo = (function(){
        function add(){
        	baz();	
        }
    
        function destroy(){
    
        }
    
        function baz(){
    
        }
    
        function init(){
            document.getElementById('foo').addEventListener("click", add);
            document.getElementById('bar').addEventListener("click", destroy);
        }
      
      var foo = 1;
      var bar = 1;
      	
      return {
          init: init,
          bar: var
      }
      
    })();  
    
    document.addEventListener("DOMContentLoaded", function(){
        Foo.init();
        console.log(Foo.bar);
    });
    ~~~
  이렇게되면 Foo에 할당되어있는 즉시실행함수의 변수나 함수들은 외부에서 접근이 안 된다. 그렇기 때문에 이후의 코드에 따라서 변경되거나 중복 선언 되는 상황이 발생하지 않고, 모듈화에 최적화 되어있다.



## Template pattern

JS개발에서 많이 사용하는 방법이 HTML문자(템플릿)를 데이터와 함쳐서 엘리먼트로 삽입하는 것이다. 이 때 대부분 HTML 문자를 따로 관리하기도 하고 같이 JS파일을 관리하기도 하는데 여기서는 HTML문자를 따로 관리하여 로직과 뷰 부분을 분리하는 방법을 알아본다.

* 라이브러리를 쓰지 않을 때 바닐라 자바스크립트로 구현하는 일반적인 방법

  index.html

  ~~~html
  <!doctype html>
  <html lang="ko">
      <head>
          <meta charset="utf-9">
          <title>HTML Template</title>
      </head>
      <body>
          <input id="todo" type="text" placeholder="add" />
          <input id="add" type="button" value="todo" />
          <ul id="todolist">
              <li><input type="checkbox" /><span>할 일 하기</span></li>
          </ul>
          <script type="text/javascript" src="template.js"></script>
      </body>
  </html>
  ~~~

  template.js

  ~~~javascript
  (function(){
      //1. 요소할당
      var todo = document.getElementById('todo');
      var button = document.getElementById('add');
      var todolist = document.getElementById('todolist');
      
      function createTodoForStr(todoVal, html){
          todolist.insertAdjacentHTML("beforeend", html);
      }
      
      //3. 새로운 li요소 생성 후 붙이기
      function createTodo(todoVal){
          var li = document.createElement("li");
          var span = document.createElement("span");
          var input = document.createElement("input");
          input.setAttribute("type", "checkbox");
          span.innerText = todoVal;
          
          li.appendChild(input);
          li.appendChild(span);
          
          todolist.appendChild(li);
      }
      
      //2. 이벤트바인딩
      button.addEventListener("click", function(e){
          createTodo(todo.value);
          //4. todo가 등록되고 value값을 없앤다.
          todo.value = "";
      });
      
      
  })();
  ~~~

  위의 코드는 동작하는데 문제가 없다. 하지만 createTodo() 메서드만 봤을 때, ```<li><input type="checkbox"/><span>내용</span></li>``` 의 구조를 가진 html DOM을 생성하는 메서드라는 것을 직관적으로 알아차리기 힘들다. 코드가 길면 길수록 더 알 기가 힘들 것. 따라서 직접 html 템플릿을 문자열로 만들어서 활용한다.

* 간단한 html 템플릿 적용

  ~~~javascript
  (function(){
      //1. 요소할당
      var todo = document.getElementById('todo');
      var button = document.getElementById('add');
      var todolist = document.getElementById('todolist');
      
      function createTodoForStr(todoVal){
          //3. 새로운 li요소 생성 후 붙이기
          var todoTemplate = 
              "<li>"
          		+"<input type=\"checkbox\"/><span>"+todoVal+"</span>"+
              "</li>";
          todolist.insertAdjacentHTML("beforeend", todoTemplate);
      }
      
      //2. 이벤트바인딩
      button.addEventListener("click", function(e){	    
  		createTodoForStr(todo.value);
          //4. todo가 등록되고 value값을 없앤다.
          todo.value = "";
      });
      
      
  })();
  ~~~

  이렇게 해놓고 보면, string으로 DOM구조를 명시해 놓는 것이 훨씬 보기 편하다는 것을 알 수 있다. 각각 생성하는 것이 아니라, string 사이의 값만 바꿔주면 되기 때문이다. 그러나 이 string은 dom요소가 아니므로 appendChild()에 매개변수로 넣어서 사용할 수가 없다. 따라서 insertAdjacentHTML()을 사용해서 string을 appendChild()에 dom요소를 넣은 것과 똑같이 자식 요소 맨 마지막에 붙여준다.

  코드가 단순하고 읽기 좋아졌다. 그런데 이 문자열은 계속 반복해서 쓰이는 부분이다. 따라서 이 부분을 따로 빼서 관리할 수도 있다. 변수로 따로 빼서 값이 변하는 부분만 치환할 수 있도록 하고, 매번 템블릿의 값이 변경되는 부분을 replace()를 통해 변경한다.

  ~~~javascript
  (function(){
      //1. 요소할당
      var todo = document.getElementById('todo');
      var button = document.getElementById('add');
      var todolist = document.getElementById('todolist');
      var todoTemplate = 
          "<li>"
  		    +"<input type=\"checkbox\"/><span>{{todo}}</span>"+
          "</li>";    
      
      function createTodoForStr(todoVal){
          //3. 새로운 li요소 생성 후 붙이기
          //template에서 원하는 부분만 내용을 바꾼다.
  		var todoStr = todoTemplate.replace("{{todo}}",todoVal);
          todolist.insertAdjacentHTML("beforeend", todoTemplate);
      }
   
      //2. 이벤트바인딩
      button.addEventListener("click", function(e){	    
  		createTodoForStr(todo.value);
          //4. todo가 등록되고 value값을 없앤다.
          todo.value = "";
      });
      
      
  })();
  ~~~

  그렇다면 이렇게 분리하는 것이 왜 중요할까?

  나중에 템플릿 코드가 점점 많아지면, 이 템플릿 코드들을 따로 관리해야 될 필요가 생긴다. 보통은 템플릿 코드만 별도의 file로 분리하여 가져다 쓰는 방식을 많이 쓴다. 또한 용량도 크기 때문에 나눠서 관리한다면 성능 측면에서도 효율을 높일 수 있다.

  * micro template : javascript 코드를 템플릿에서 쓸 수 있어서 자유도가 높다.
  * handlebars : 좀 더 시멘틱한 템플릿을 만들기 위해서 쓰는 용도이다.  

* handlebars.js 사용

  ~~~html
  <!doctype html>
  <html lang="ko">
      <head>
          <meta charset="utf-9">
          <title>HTML Template</title>
      </head>
      <body>
          <input id="todo" type="text" placeholder="add" />
          <input id="add" type="button" value="todo" />
          <ul id="todolist">
              <li><input type="checkbox" /><span>할 일 하기</span></li>
          </ul>
          <script type="text/javascript" src="handlebars-v4.0.5.js"></script>
          <script type="text/javascript" src="template.js"></script>
      </body>
  </html>
  ~~~

  ~~~javascript
  (function(){
      //1. 요소할당
      var todo = document.getElementById('todo');
      var button = document.getElementById('add');
      var todolist = document.getElementById('todolist');
      var todoTemplate = 
          "<li>"
  		    +"<input type=\"checkbox\"/><span>{{todo}}</span>"+
          "</li>";    
      //handlebars는 해당 source(todoTemplate)를 compile을 하고 메서드를 반환한다.
      var template = Handlebars.compile(todoTemplate);
      
      function createTodoForStr(todoVal){
          //3. 새로운 li요소 생성 후 붙이기
          //template()을 실행하면 html 코드를 반환한다. 이 때 파라미터로 key와 value를 갖는 object형태를 넣으면 된다. template메서드가 key값에 맞는 부분을 알아서 찾은 후 value값으로 replace() 해주고 완성된 html dom코드를 반환한다.
          todolist.insertAdjacentHTML("beforeend", template({
              "todo":todoVal
          }));
      }
      
      //2. 이벤트바인딩
      button.addEventListener("click", function(e){	    
  		createTodoForStr(todo.value);
          //4. todo가 등록되고 value값을 없앤다.
          todo.value = "";
      });
      
      
  })();
  ~~~

이렇게 템플릿을 만들어서 관리하는 것의 핵심은 해당 뷰 영역(html코드)과 로직을 분리한 후, 뷰 영역을 따로 관리하기 위한 방법들이다.



## Event delegation

element에 이벤트를 바인딩하여 이벤트 콜백에서 작업을 주로 한다. 일반적인 방법으로 엘리먼트가 추가되거나 삭제될 때, 이벤트를 추가하거나 삭제하도록 이벤트를 관리한다. 이 작업은 생각보다 귀찮은 작업으로 이를 해소하고자 event delegation 기법을 활용한다. Evenet delegation은 상위에 있는 엘리먼트에 이벤트를 바인딩하여 이벤트 처리를 위임하고, 하위 엘리먼트에서 이벤트가 버블링되면 이벤트를 분석하여 이벤트들을 관리하는 기법니다. 기존의 방식을 event delegation으로 변경하고 event delegation의 유의점을 알아 본다.

아래 코드는 특정 할 일 목록의 checkbox를 클릭하면 해당 요소의 클래스 네임으로 complete가 추가 되고, complete가 생긴  목록에 취소선이 가는 효과를 구현한 코드이다.

* 기존의 방식 

  index.html

  ~~~html
  <!doctype html>
  <html lang="ko">
      <head>
          <meta charset="utf-9">
          <title>Event Delegation</title>
          <style type="text/css">
              .complete{
                  text-decoration: line-through;
                  font-style:italic;
                  color:grey;
              }
          </style>	
      </head>
      <body>
          <input id="todo" type="text" placeholder="add" />
          <input id="add" type="button" value="todo" />
          <ul id="todolist">
              <li><input type="checkbox" /><span>할 일 하기</span></li>
          </ul>
          <script type="text/javascript" src="template.js"></script>
      </body>
  </html>
  ~~~

  delegate.js

  ~~~javascript
  (function(){
      //1. 요소할당
      var todo = document.getElementById('todo');
      var button = document.getElementById('add');
      var inputList = document.querySelectorAll("li input");
      var todolist = document.getElementById('todolist');    
      
      //3. 새로운 li요소 생성 후 붙이기
      function createTodo(todoVal){
          var li = document.createElement("li");
          var span = document.createElement("span");
          var input = document.createElement("input");
          input.setAttribute("type", "checkbox");
          span.innerText = todoVal;
          
          li.appendChild(input);
          li.appendChild(span);
          
          todolist.appendChild(li);
      }
      
      //2. 이벤트바인딩
      button.addEventListener("click", function(e){
          createTodo(todo.value);
          //4. todo가 등록되고 value값을 없앤다.
          todo.value = "";
      });
      
      //5.event delegation
      for (var i = 0; i < inputList.length; i++){
          inputList[i].addEventListener("click", function(e){
              //checkbox의 parent인 li의 className 검사
              if(e.target.parentNode.className === "complete"){
                  e.target.parentNode.className = "";
              } else {
                  e.target.parentNode.className = "complete";
              }
          });
      }
      
      
  })();
  ~~~

  그런데 위와 같은 상태에서는 만약 새로운 할 일 목록이 생기면, 이벤트가 바인딩 되지 않아서 클릭해도 아무 일도 일어나지 않는다. 따라서 다음과 같은 코드로 바꿔야한다.

  ~~~javascript
  (function(){
      //1. 요소할당
      var todo = document.getElementById('todo');
      var button = document.getElementById('add');
      var inputList = document.querySelectorAll("li input");
      var todolist = document.getElementById('todolist');    
      
      function toggle(e){
          //checkbox의 parent인 li의 className 검사
          if(e.target.parentNode.className === "complete"){
              e.target.parentNode.className = "";
          } else {
              e.target.parentNode.className = "complete";
          }
      }
  
      //3. 새로운 li요소 생성 후 붙이기
      function createTodo(todoVal){
          var li = document.createElement("li");
          var span = document.createElement("span");
          var input = document.createElement("input");
          input.setAttribute("type", "checkbox");
          span.innerText = todoVal;
          //새로운 input요소를 생성할 때 마다 생성된 요소의 click event에 toggle 함수를 바인딩한다.
          input.addEventListener("click", toggle);
          
          li.appendChild(input);
          li.appendChild(span);
          
          todolist.appendChild(li);
      }
      
      //2. 이벤트바인딩
      button.addEventListener("click", function(e){
          createTodo(todo.value);
          //4. todo가 등록되고 value값을 없앤다.
          todo.value = "";
      });
      
      //5.event delegation
      //기존에 존재하던 input요소들의 click event에 toggle 함수를 바인딩한다.
      for (var i = 0; i < inputList.length; i++){
          inputList[i].addEventListener("click", toggle);
      }
      
      
  })();
  ~~~

  위의 코드가 일반적인 방식이고, 이것을 개선한 방법이 event delegation이다.

* Event delegation

  매번 생성할 때 마다 event를 추가하는 것이아니라, input을 가지고 있는 li 상단의  ul에 이벤트를 걸고 자손 요소들의 이벤트를 버블링해서 받는다.

  ~~~javascript
  (function(){
      //1. 요소할당
      var todo = document.getElementById('todo');
      var button = document.getElementById('add');
      var inputList = document.querySelectorAll("li input");
      var todolist = document.getElementById('todolist');    
      
      function toggle(e){
          //checkbox의 parent인 li의 className 검사
          if(e.target.parentNode.className === "complete"){
              e.target.parentNode.className = "";
          } else {
              e.target.parentNode.className = "complete";
          }
      }
  
      //3. 새로운 li요소 생성 후 붙이기
      function createTodo(todoVal){
          var li = document.createElement("li");
          var span = document.createElement("span");
          var input = document.createElement("input");
          input.setAttribute("type", "checkbox");
          span.innerText = todoVal;
          
          li.appendChild(input);
          li.appendChild(span);
          
          todolist.appendChild(li);
      }
      
      //2. 이벤트바인딩
      button.addEventListener("click", function(e){
          createTodo(todo.value);
          //4. todo가 등록되고 value값을 없앤다.
          todo.value = "";
      });
      
      //5.event delegation
      todolist.addEventListener("click", function(e){
          if(e.target.tagName === "INPUT"){
          	toggle(e);    
          }
          //확장하기도 쉽다.
          else if(e.target.tagName === "SPAN"){
              //...
          }
      });
      
      
  })();
  ~~~

* 주의점

  Event delegation을 하다보면 상위 요소를 클릭했을 때, 자손에서 일어나는 모든 이벤트를 버블링 받게 된다. 따라서 body에 모든 event를 바인딩하고, 이벤트가 일어나는 해당 요소만 필터링해서 사용할 수 있지 않을까? 라는 생각을 할 수 있지만, 여기에는 큰 문제점이있다. 위와 같이 간단한 마크업이 아니라 굉장히 규모가 큰 마크업이라면,  body이벤트를 걸면 모든 요소에서 이벤트가 발생하고, 필터링을 하는 작업이 동시에 이루어지므로 성능 저하를 심각하게 유발할 수 있다. 따라서 event delegation은 event delegation을 받는 부모의 영역이 가능한 적어야 효율적으로 활용할 수 있다.



## 자바스크립트 초기화

자바스크립트를 초기화하는 다양한 방법을 알아보고, 각 방법들에 대한 이슈를 확인한다.

* script를 제일 하단으로

  index.html

  ~~~html
  <!doctype html>
  <html lang="ko">
      <head>
          <meta charset="utf-8">
          <title>자바스크립트 초기화</title>
      </head>
      <body>
          <div id="foo">            
          </div>
          <scirpt type="text/javascript" src="init.js"></scirpt>        
      </body>
  </html>
  ~~~

  init.js

  ~~~javascript
  //getSTYLE(ele, "width"); 를 실행하면 width 값을 반환한다.
  function getStyle(ele, style){
      return window.getComputedStyle(ele, null).getPropertyValue(style);
  }
  
  function init(){
      //엘리먼트를 찾고,
      var foo = document.getElementById("foo");
      //이벤트를 할당
      foo.addEventListener("click", function(){
          console.log("click");
      });
  }
  ~~~

  만약 script가 header에 있으면 내가 사용하는 element 전에 해당 함수를 호출하므로 에러가 난다. 따라서 내가 사용하는 element 뒤에 script를 넣어야 한다.

* load이벤트 활용

  ~~~javascript
  //getSTYLE(ele, "width"); 를 실행하면 width 값을 반환한다.
  function getStyle(ele, style){
      return window.getComputedStyle(ele, null).getPropertyValue(style);
  }
  
  function init(){
      //엘리먼트를 찾고,
      var foo = document.getElementById("foo");
      //이벤트를 할당
      foo.addEventListener("click", function(){
          console.log("click");
      });
  }
  
  //html의 모든 resource들이 load가 된 이후에 발생하는 것.
  window.addEventListener("load",function(){
      init();
      //load된 resource들의 정보가 필요할 때는 load 이벤트를 사용해야 한다.
      var bar = document.getElementById('bar');
      console.log(getStyle(bar,"width"));
  });
  ~~~

  그런데 이것은 문제점이 있다. 서비스가 resource가 굉장히 많다면 load 시점이 늦어지므로 자바스크립트 초기화 코드가 늦게 실행되게 된다. 그렇게 되면 사용자들이 자바스크립트와 관련된 인터렉션을 전혀 사용할 수 없게 된다. 이럴 때는 domcontentloaded를 쓴다.

* domcontentloaded이벤트 활용

  ~~~javascript
  //getSTYLE(ele, "width"); 를 실행하면 width 값을 반환한다.
  function getStyle(ele, style){
      return window.getComputedStyle(ele, null).getPropertyValue(style);
  }
  
  function init(){
      //엘리먼트를 찾고,
      var foo = document.getElementById("foo");
      //이벤트를 할당
      foo.addEventListener("click", function(){
          console.log("click");
      });
  }
  
  //html의 모든 resource들이 load가 된 이후에 발생하는 것.
  window.addEventListener("DOMContentLoaded",function(){
      init();
  });
  ~~~

  DOM이 load되고 resource들이 load를 시작하기 전에 초기화하는 것이다. 그러나 이것을 항상 사용할 수는 없고, 꼭 load를 사용해야 하는 경우들이 있다. loaded된 resource들에 접근을 해야 될 때는 load를 써야한다. 이미지가 로딩 된 이후에 이미지를 화면에 맞게 조절하거나 하는 경우는 먼저 이미지의 크기를 읽어야 변경을 할 수 있다. 따라서 이럴 때는 load를 사용해야 한다.