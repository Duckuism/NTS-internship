일단 모든 리뷰내용을 정리.
그리고 레이아웃, 접근성, 구형 브라우저 대응, 태그별 이슈로 리뷰 내용을 정리
내가 마크업 스타일링 순서로 진행하면서 각 단계마다 순서대로 생각해봐야 하는 것들을 정리.

1. 레이아웃 관련 이슈

   1. 모든 태그들이 영역을 잘 잡고 있어야한다.
   2. 시각 장애인이 스크린리더로 읽는다고 생각하고 마크업 순서를 구성해야 한다.
   3. 구조가 깨지면 안 된다. 왼쪽 오른쪽 나눴으면 화면을 줄여도 왼쪽 오른쪽 구분은 그대로여야 한다.
   4. 부모 요소가 확실히 자식요소를 감싸고 있어야 한다.
   5. 개발에 반영되는 가변 percent는 인라인 스타일로 입력하고 개발 주석을 달아주어야 한다.
   6. flow에 영향을 주는 텍스트가 들어있는 요소들은 width, height를 고정으로 넣지 말 것. 기본 폰트가 브라우저 별로 다 다르기 때문에 flow에 영향을 주는 경우가 생길 수도 있음.
   7. 반응형은 작은 것부터 개발한다. PC 고정 값이 기획안에 주어지면 그 값으로 가운데 정렬해서 먼저 만들고, 이 width보다 줄어들 때 %로 지정해서 개발하면 된다.

2. Box-model 관련 이슈

   1. block 요소는 width:100%를 선언할 필요가 없다. ( block elements list : http://itman.in/en/html5-block-level-elements/)
   2. margin은 병합이 일어나므로 한 방향으로 줘야한다. 보통 margin-top으로 주는 것이 나중에 수정하기 좋다.
   3. padding, margin은 부모 요소가 한 번에 가지고 있는 것이 좋다.

3. 접근성 이슈

   1. 스크린 리더가 읽어주지 않아도 되는 의미없는 디자인 요소라면 dom을 만들지 말고 가상 요소나 background로 넣어야 한다. 이 때는 대체 텍스트도 필요 없다. 만약 span등을 이용한 빈 태그를 사용하면 나중에 변경 사항이 생길 시 개발자에게 삭제 요청을 해야하므로 커뮤니케이션 비용이 발생한다.
   2. 출석 표시하는 도트와 같은 기호도 정보를 전달하는 것이면 블라인드 텍스트가 필요하다. 이럴 때는 가상요소가 아니라 span으로 해줘야 한다. 블라인드 텍스트에서 ```content:''``` 속성은 스크린리더 종류마다 읽기 여부가 다르므로 지양해야한다.
   3. 중복 되는 텍스트는 ```aria-hidden="true"``` 속성을 추가해서 스크린 리더에서 중복으로 읽지 않게 한다.

4. 구형 브라우저 대응 이슈 (IE8 이상 www.caniuse.com )

   1. rgba() : IE8이하 사용 불가

      1. 대응 방법 
         1. rgba에 적용된 색상을 color로 먼저 선언해놓고, rgba도 또 선언해놓으면 된다.
         2. filter 옵션을 추가한다. ( filter 옵션 생성 사이트 : https://web.archive.org/web/20130829031726/kimili.com/journal/rgba-hsla-css-generator-for-internet-explorer/)

   2. div같은 블록 요소를 display:inline-block으로 변경하면 IE8에서는 변경되지 않는 이슈가 있다.

   3. 가상 요소에 opacity는 IE8에서 적용되지 않는다.

      1. 가상 요소 별 브라우저 지원 확인 : https://kimblim.dk/css-tests/selectors/

   4. opacity IE 대응을 위해 filter 값을 넣을 때 filter앞에 -ms- 프리픽스는 안 붙여도 된다.

   5. transform : vendor prefix 적용

      ~~~css
      .example{
          -webkit-transform: rotate(30deg);
          -ms-transform:  rotate(30deg);
          transform: rotate(30deg);
      }
      ~~~

   6. background-size: cover는  IE8에서 안 먹는다.

   7. 두 줄 이상 말줄임이 IE8에서는 안 된다. 따라서 line-height를 줄 수 만큼 줘서 overflow hidden으로 짤라야 한다. 크롬은 webkit으로 하면 가능하다.

   8. 생활코딩 참고 : https://opentutorials.org/module/2258/13706

   9. 네이버 컨벤션 (여기를 항상 참고해야 할 듯!) : http://wit.nts-corp.com/2015/01/28/3032

5. 특정 태그 관련 이슈

   1. 여기서 각 요소별로 제한 조건을 찾아볼 수 있어요~

      http://w3c.github.io/html-reference/elements.html#elements

   2. 태그 별 CSS 기본 값 : https://www.w3schools.com/cssref/css_default_values.asp

   3. img 

      1. width, height가 required는 아니지만 되도록 명시하는 것이 좋다.
         1. 이 때 px값은 빼고 숫자만 기입한다.
         2. 이미지 자체가 크다면 width, height가 꼭 필요하다. 만약 없다면 원래 크기대로 reflow 때 DOM 렌더링을 하고, repaint 때 css에 따라 다시 크기를 줄이므로 성능에 영향을 끼친다.
      2. 데이터를 load하는 경우에만 사용한다. 의미가 없는 디자인적인 요소이거나 변경되지 않는 고정 이미지라면 background로 넣는다.
      3. src, alt값은 반드시 사용해야하는 필수 속성이다.
      4. 스프라이트 이미지 영역은 딱 맞게 잡아줘야 한다. 만약 꼭 공간이 필요하다면 다른 태그로 감싸서 padding으로 넣어주어야한다.
      5. img를 idv로 감싼 상태에서 float left를 하면 img아래 여백이 생기는데 이 때는 vertical-align:top으로 해결할 것.

   4. a

      1. 다른 페이지로 이동할 때 쓴다.

      2. 영역을 못잡고 있는 경우가 많다. ```display:block```을 사용해 영역을 제대로 잡을 수 있게 해줄 것.

      3. 클릭 영역을 잘 고려해서 넣어야 한다. 썸네일 같은 경우는 썸네일 전체 어디를 클릭하던 갈 수 있도록. 안에 하나 더 넣어서 중복 링크를 걸지 않도록 하자.

      4. a 태그도 보면 

         http://w3c.github.io/html-reference/a.html#a-constraints

         	•	The interactive element a must not appear as a descendant of the a element.

         	•	The interactive element a must not appear as a descendant of the button element

         이런 제약조건이 있죠. 그럼 a 태그 안에 자식으로 a나 button이 올 수 없다고 볼 수 있죠~

   5. input

      1. checkbox : 여러 항목 중에 맞는 것을 체크

      2. radio : 여러 항목 중 한 가지를 체크

      3. text 

         1. outline:none 적용은 하면 안 된다. 저시력 사용자를 위해 필요하다.

      4. label이 대부분 필요한데

         1. input과 label은 항상 감싸주는 것이 좋다.
         2. label에는 꼭 텍스트를 써줘야한다.
         3. label + input과 label > input을 적절히 사용해야한다.
         4. label이 먼저 올 수 없는 경우 안에 input을 넣는다.

      5. value값이 필수는 아니므로 써주지 않아도 된다.

      6. checked 속성을 사용하고, IE8에서는 사용이 안되므로 checked속성을 사용함과 동시에 checked 클래스도 넣고 체크 되었을 때 스타일링을 checked에 해준다. 그리고 꼭 개발 주석을 달아줄 것!

      7. label의 제약 조건은 http://w3c.github.io/html-reference/label.html#label-constraints 여기에 나와있어요~

         근데 label을 보니, 제약조건에 나와있지 않죠~?! label에 ul에 아니라 <label><div>test</div></label>을 넣어도 validation에 “ Element div not allowed as child of element label in this context. “ 이런 에러가 떠요. 그 이유는 <label>은 인라인 요소이기 때문에 기본적으로 인라인 요소 내에는 블록 요소가 올 수 없기 때문이에요~ 인라인 요소와 블록 요소를 잘 구분해두는 것이 좋아요~!

   6. button

      1. 클릭한 항목에 해당하는 세부 내역을 선택하는 기능에는 button이 더 적합. 달력과 같은 경우도 마찬가지이다.

      2. 버튼을 사용했다면 체크된 상태는 클래스를 추가하여 나타낸다.

         ~~~html
         <!-- [D]셀렉박스 선택 시 클래스 .select_on 추가 -->
         <button type="button" class="select_on"></button>
         ~~~

      3. type속성을 꼭!! 기입해준다. 넣지 않으면 IE8에서는 무조건 submit으로 동작해서 이슈가 있다.

      4. 의미가 있는 버튼이라면 blind된 대체 텍스트가 꼭 필요하다.

      5. 시각 장애인이 봤을 때 버튼을 어디에서 보면 좋을 지 잘 생각하고 넣는다.

      6. 삼각형 같은 아이콘이 버튼과 같이 경우는 button 안에 넣는다. 만약 밖에 넣으면 클릭이 안 될 수도 있으므로.

      7. 이전, 다음과 같은 화살표 버튼에 특수문자 같은 것은 절대 안된다. 접근성을 지켜야 하기때문에 화살표는 background로 넣어주고 블라인드 텍스트 (ex)```<span class="blind">이전</span>```)을 넣어서 버튼이 어떤 기능을 하는지 꼭 알려주어야 한다.

      8. disabled 속성은 IE8에서는 사용이 안되므로 disabled 속성을 사용함과 동시에 disabled 클래스도 넣고 disabled 되었을 때 스타일링을 이 클래스 선택자를 통해 해준다. 그리고 꼭 개발 주석을 달아줄 것!

      9. button에 기능에 관련된 의미를 가진 클래스명을 넣을 것.

   7. table

      1. table 태그에 table-layout:fixed를 꼭 선언해줄 것!!
      2. table th tr td 등 테이블 요소 자체에 padding/position을 넣지 말 것.
         1. table / table-cell을 쓸 때는 table-cell 안에 inner div를 생성해주고 padding으로 값을 조절한다. 
      3. table-cell의 기본 정렬이 vertical-align:middle이다. vertical-align을 중복선언하지 말 것.
      4. caption은 제목이 아니라, UI안에서 table의 범주를 나타내는 것이 좋다. 제목은 헤딩으로 지정할 것.
      5. th는 반드시 scope가 있어야 한다. (2권에서 예제 잘 확인해둘 것) 그런데 해당되는 td가 없으면 쓸 수 없다.
      6. colgroup은 필수가 아니다. colgroup에 width를 지정하지 말 것.
      7. 변경되는 셀은 병합하면 안 된다. th가 아닌 한 변동되는 UI에서 병합할 일은 없을 듯.
         1. id와 headers 값은 셀이 병합됐을 경우에만 사용할 것
      8. thead - tfoot - tbody 순서로 와야한다.

   8. select

      1. Select box list는 마우스 오버일 때 display: none / block으로 처리하면 된다.

   9. form 

      1. form에 style을 선언하면 안 된다. form자체를 div로 한 번 감싸거나 내부를 div로 감싼 뒤에 style을 넣어주어야 한다.

   10. span

      1. 개발 컨트롤이 필요한 부분은 span같은 태그로 일부러 감싸주는 것이 좋다.

6. 특정 속성 관련 이슈

   아래 글은 오래된 포스트이지만 충분히 읽을만한 가치가 있다. 정리가 안 되고 헷갈릴 때 한 번 씩 쭉 읽으면 될 듯 하다. 

   * position, float, display 속성간의 관계 : http://blog.wystan.net/2009/01/12/relationships-between-position-float-display

   **float과 position:absolute를 절대 같이 쓸 필요가 없다. 절대적으로 배치하는 absolute에서는 float 속성 자체가 필요가 없게 된다. 즉 position:absolute를 쓰면 float를 아얘 쓰지 말고 좌표값으로 조절하면 되고, position:absolute를 쓰지 않는 경우는 float만 사용하면 되는 것.** (https://stackoverflow.com/questions/11333624/float-right-and-position-absolute-doesnt-work-together)

   이유는? 일반적으로 float는 상위 컨테이너에 상대적인 요소의 위치(오른쪽 또는 왼쪽으로 부동)를 지정하므로 상대적인 위치 지정 문이다. position:absolute는 절대적인 지정문이므로 호환될 수가 없다. 즉 요소를 float으로 띄우고 브라우저가 상위 컨테이너에 상대적인 위치를 지정하도록 하거나, 절대 위치를 지정하고 상위 컨테이너에 관계없이 요소가 특정 위치에 표시되도록 할 수 있는 것. 직관적인 비교로는 position:absolute로는 -50px과 같이 음수 값으로 특정 위치에 오도록 강제 할 수 있지만, float는 부모의 영역이 기준이 되어 이 영역을 벗어날 수 없으므로 그렇게 할 수가 없다.

   상대적으로 위치시키는 float은 요소의 width, height 값 자체를 변경시키지 못 한다. 그냥 상대적으로 왼쪽, 오른쪽으로 붙게만 하는 것이다. 따라서 float:left 다음 요소에 overflow:hidden을 하면 float 



   1. float 

      1. float은 태그를 block요소로 바꾸므로 display:inline-block, block등의 속성을 줄 필요가 없다.
      2. **clear를 꼭 해줘야한다**. clear가 여의치 않을 때는 overflow:hidden.
      3. 만약 overflow:hidden이 드롭박스 가려짐과 같은 이슈로 사용할 수 없다면 floating되어져 있는 요소만큼 margin, padding을 사용하여 간격을 만들어준다.
      4. overflow:hidden으로 해제를 하면 floating된 요소의 높이를 형제요소들이 min-height로 공유하게 된다.
      5. 해제를 하면 부모 요소에 height를 줄 필요가 없다.

   2. position:absolute 

      1. position:absolute를 쓸 때는 무조건 좌표값(x, y축 중에 하나씩)을 잡아줘야 한다. 
      2. position:absolute를 쓰게 되면 normal flow에서 벗어나고 자동적으로 display:block으로 변경되므로 display속성 자체에 영향을 받지 않는다. 따라서 display: inline, inline-block, block 속성을 쓸 필요가 없다.
      3. 만약 높이에 대한 이슈가 있다면, absolute를 갖고 있는 부모의 height를 조절하는 수 밖에 없다.
      4. 직전 부모에 position:relative를 꼭 사용해서 기준을 잡아줘야 레이아웃이 깨지지 않는다.
      5. position:absolute를 쓸 때는 margin, padding을 사용할 필요가 없다. 그냥 좌표 값으로 맞춰주는 것이 깔끔하다.
      6. absolute는 gpu를 사용하게 되므로 큰 div에 적용하게 되면 오히려 성능 저하를 일으키는 요인이 될 수 있다. 반복되는 애니메이션이나 그래픽 요소가 들어가는 일부 영역에만 사용하는 것이 좋다.
      7. left: 0; right: 0;을 하게되면 부모의 너비만큼을 영역으로 잡고(width값 필요없음) border값도 내부안에서 채워진다.

   3. min-height

      1. min-height를 쓴다는 얘기는 해당 요소가 가변이 된다는 의미이므로 height를 100%로 고정을 주지 않는다.

   4. height

      1. 고정 height를 줄 때는 padding이랑 같이 쓰지 말자. 둘 중에 하나로 해결할 수 있는 지 항상 생각할 것.

   5. 색상

      1. blue와 같은 컬러명은 브라우저마다 다른 색이 나올 수 있으므로 절대 쓰면 안 된다.

   6. background

      1. background는 축약형으로 쓴다.

         1. background :  https://developer.mozilla.org/ko/docs/Web/CSS/background

         2. backgorund 단축 속성

            ~~~css
            background-color: #000;
            background-image: url(images/bg.gif);
            background-repeat: no-repeat;
            background-position: top right;
            
            background: #000 url(images/bg.gif) no-repeat top right;
            ~~~

            ( 참고 https://developer.mozilla.org/ko/docs/Web/CSS/Shorthand_properties )

      2. 모바일 일때는 size를 1/2로 지정하여 확대하지만, PC는 확대할 필요가 없다.

      3. background는 처음에 축약형(```background: 경로 리핏 0 0;  ``` - 기본 포지션 위치 0 0)으로 지정해놓고 각 요소에 background-position만 따로 빼서 지정한다.

   7. z-index ( 새로운 쌓임 맥락을 생성하는 경우 - https://developer.mozilla.org/ko/docs/Web/CSS/Understanding_z-index/The_stacking_context)

      1. static 요소에는 z-index가 적용이 안 된다. position:static은 left, top, z-index 등으로부터의 위치 구조를 무시한다는 의미이기 때문이다. (https://stackoverflow.com/questions/8486475/why-is-z-index-ignored-with-positionstatic)

   8. vertical-align

      1. 텍스트 정렬방식은 여러가지 요소가 복합적으로 작용합니다. 질문주신 부분과 상황으로 보면 강의에서 설명했듯이 인라인요소는 vertical-align에 따르게 되는데요. vertical-align속성의 기본값은 baseline입니다. baseline은 소문자 x기준으로 정렬이 되는데요. 내용에 x를 넣어서 보면 이미지 처럼 x의 끝이 같은 선상에 있는것을 볼수 있습니다.

         [![image](http://gitlab3.uit.navercorp.com/NT60366/htmlcss_basic/uploads/551e2afa1844d9ece47657a36fb080b8/image.png)](http://gitlab3.uit.navercorp.com/NT60366/htmlcss_basic/uploads/551e2afa1844d9ece47657a36fb080b8/image.png)

         저 baseline에 맞춰서 inline-block의 위치가 결정이 되기 됩니다. 이때 .ele_summery_tag의 font-size가 좀 더 작기 때문에 오른쪽 텍스트가 위로 올라간것처럼 보이는 것입니다. 개발자 도구로 확인해보면 .ele_summery_tag와 부모사이의 여백이 보일거에요~ .ele_summery_tag에 vertical-align:top을 주면 부모가 가진 라인박스 상단에 딱 붙게 됩니다.

         [![image](http://gitlab3.uit.navercorp.com/NT60366/htmlcss_basic/uploads/5ae7ab429ae35d4d34fd55590137d121/image.png)](http://gitlab3.uit.navercorp.com/NT60366/htmlcss_basic/uploads/5ae7ab429ae35d4d34fd55590137d121/image.png)

         vertical-align의 내용이 이해하기 어렵기 때문에 관련 자료들을 많이 보셔야 할거에요.

   9. display

      1. 영역을 차지하는 visibility는 되도록 지양하고 display를 사용할 것.

   10. Font-family

       1. 가장 많이 쓰는 것을 전체에 선언을 한다. 영문만 있는 폰트를 먼저 선언하고, 한글 폰트를 선언하고, generic font 선언하고. 그리고 font-weight만 조절해서 두께를 조절하면 된다. 만약 각 요소마다 font-family를 주게 되면 해당 폰트 속성를 갖지 않은 곳에서는 볼 수 없다는 얘기가 된다. 따라서 처음에 UI를 고려하여 선언할 것. 

7. 특정 선택자, 가상 요소 관련 이슈

   1. :before
      1. 가상요소로 간격을 넣는 것 절대 하지 말 것. 가상 요소도 CSS로 만드는 DOM이므로 불필요하게 DOM 생성하는 것은 성능에 좋지 않다.
   2. ```div>a``` or ```div a```
      1. 자식, 자손 선택자는 사용하지 말고 클래스를 지정하여 준다.
   3. 속성 선택자 ex) ```span.classname```
      1. 위와 같은 식으로 하면 우선순위가 굉장히 높아지므로 지양한다.
      2. 속성 선택자 자체를 꼭 필요할 때만 써야한다.
      3. nth-child() 같은 것들은 IE8에서 지원도 안 하고, 위치가 바뀌거나 개수가 바뀌면 스타일링이 변경될 가능성이 있으므로 사용하지 말고 클래스를 사용할 것.
   4. 클래스 선택자
      1. 공통된 범주로 묶여 있다면, 최대한 바깥의 부모에 공통된 속성을 주고 자식에서 분기하면서 각각의 특징적인 속성만 추가로 선언해 주면 됨. 형제마다 모두 제어할 필요가 없음
      2. 특정 경우에만 요소가 생기는 것과 클래스를 분기하는 것을 잘 구별해야. 특정 경우에만 요소가 생기는 것은 독립적인 경우이고 클래스를 분기하는 경우는 공통된 속성들을 가지고 있는 경우일 것.
      3. 클래스 이름은 언더바 하나만 사용한다. 너무 길면 안 됨.
   5. ID 선택자 
      1. 특정한 경우를 제외하고는 사용하지 않는다.
         1. 전체적인 레이아웃 구조를 잡기 위해 id를 header, content, footer등으로 줄 때
         2. label과 폼 요소들을 연결할 때
         3. 앵커링이 필요한 경우

8. 정렬 이슈

   1. 한 줄 정렬은 line-hight와 margin으로 조절할 것. 한 줄 수직 정렬을 위해 table/table-cell or inline-block은 과하다.
   2. 중앙 정렬 방법 모음 (널리) : http://wit.nts-corp.com/2017/02/06/4123
   3. 정렬 방법 모음 : https://css-tricks.com/centering-css-complete-guide/

9. SASS 관련

   1. 간단 내용 정리 : http://wit.nts-corp.com/2015/01/09/2936

10. 기타 이슈

   1. 주석은 [D] 개발 주석 혹은 UI구분을 위한 주석만 사용한다.
   2. 만약 데이터와 연동되는 부분이라면 개발 주석이 필요하다. (```<!--[D] 출석 시 td에 .attend 클래스 추가-->```)
   3. 시작과 끝같은 부분도 개발 주석이 필요하다.
   4. 달력 제목과 같이 개발자가 숫자를 바꾸거나 하는 등의 개발 컨트롤이 되어야 하는 부분은 태그로 꼭 감싸주어야한다.
   5. dimmed처리를 위해 빈 요소를 만들지 말 것. 가상 요소가 적절. 혹은 해당 요소에 ```background-color:rgba(0,0,0,0.5);```를 주는 것이 좋음
   6. 초기화 관련 이슈는 나중에 현업자로 일하게 되면 생각해보고, reset.css는 되도록 사용하지 말도록 한다.
   7. em 단위 사용은 지양한다.

11. 지난 기출 이슈

    1. 주요 엔티티 코드 정리 : http://front8062.tistory.com/33

    2. -child 선택자 순서 : 

       1. ```nth-child(n+0)``` : 괄호 안에 0이 들어가면 아무 일도 일어나지 않는다. 1부터 적용된다.

    3. 웹 접근성 

       1. skip navigation
       2. 대체 텍스트
       3. img alt
       4. head title

    4. sass 

       1. mixin / extend / import

    5. gulp

       1. 경로 쓰기

    6. sprite 이미지 썼을 때 장, 단점

       * 장점 : 하나의 파일에 다수의 이미지를 병합하여 HTTP 요청 횟수를 줄이므로 속도 개선에 좋다.
       * 단점 : 
         * 레이아웃이 복잡해질수록 더 많은 파일이 필요하게 되고, 더 많은 이미지를 사용한다. 파일 크기가 굉장히 커질 수 있다. 모바일 환경(특히 IOS)에서는 제한이 있다.
         * 또한 이미지의 퀄리티가 낮아질 수 있다. 이미지의 퀄리티를 위해서는 PNG-24형식의 이미지를 별도로 생성해야하고, IE6에서는 투명모드를 지원하지 않으니 투명한 부분이 없도록 하거나 filter 속성을 사용해야 한다. 

    7. Viewport 

       1. http://nuli.navercorp.com/sharing/blog/post/1132729

    8. 미디어 쿼리 적용 관련 

       1. 이미지 3

          이미지 5

          Max-widht가 480px일 때 3개만 보여주고

          Max-width가 480px이상일 떄 5개 보여주고

          Nth-child(3n+1) display:none;

12. 내가 예상하기에 나올 수 있는 부분 

    1. 미디어 쿼리 https://developer.mozilla.org/ko/docs/Web/Guide/CSS/Media_queries
    2. 성능 최적화
       1. 쿠키가 없는 도메인 사용
       2. CSS sprite 사용
       3. PC 600 Bytes, 모바일 200 Bytes 이하의 개별 이미지는 Data URI로 사용
       4. @import를 사용하지 않는다. (CSS 파일은 최대한 통합. 여의치 않다면 자동화 툴 사용)
       5. 불필요한 link를 사용하지 않는다.
       6. Minified CSS로 제공
       7. 불필요한 이미지 metadata 제거
       8. CSS는 head내에 link
       9. table은 table-layout:fixed 사용
       10. iframe 사용 지양
       11. absolute 레이어의 좌표나 크기는 최소로 정의

13. 마지막에 확인해야 할 부분

    1. 불필요한 선언은 없는가?

    2. 사용하지 않는 DOM은 없는가?

    3. 적절한 태그를 사용하였는가?

    4. 내가 자주 지적받는 부분

       1. %를 남발하지 말 것! (https://stackoverflow.com/questions/3600204/mixing-percent-and-fixed-css)

          1. 그렇다면 언제 고정 width, 언제 %를 사용해야 할까?

             1. 섞어서 사용하라는 주장 
                1. https://www.smashingmagazine.com/2009/06/fixed-vs-fluid-vs-elastic-layout-whats-the-right-one-for-you/
             2. 둘 중에 하나만 사용하라는 주장
                1. https://alistapart.com/article/conflictingabsolutepositions

          2. %는 상속을 받으므로 %가 부모 요소에 오고 자식요소에 고정 값이 오면 문제가 된다. 반대로 부모가 고정 값이고 자식 요소가 %면 문제가 없다.

          3. 또한 %에 margin이나 padding이 더해진 상태에서 고정 값과 같이 사용하면 문제가 된다.

             ~~~css
             .container
             {
                 width:300px;
             }
             .cell
             {
                 width:100%;
                 padding: 10px;
             }
             ~~~

             위의 경우에는 ```.cell```이 320px의 width값을 가지므로 감싸는 부모 요소인 ```.container```보다 커져서 넘치게 된다. 사실 이 때는 padding이므로 box-sizing을 주면 해결 할 수 있다. 그러나 margin을 주게 되면 문제가 커짐.

          4. Q: 도움이 되긴 하지만, 저는 믹싱이 진행 중인 렌더링의 초기 로드와 브라우저의 성능에 미치는 영향을 어떻게 혼동할 수 있는지 정말 찾고 있습니다.
             A: 생각해 보면, 백분율 너비의 모든 요소는 "고정" 폭 html 요소 내에 포함되어 있습니다. 기존 브라우저가 백분율(%)을 계층화(html의 50% 중 25%의 25% 중 25%)하는 것이 고정 너비(300px의 25%) 내에 배치되는 것보다 더 어려울 수 있습니다.

       2. div 사용을 자제하고 목적에 맞는 태그를 사용할 것!



플롯을 부모에 :after속성을 사용하여 해제 했으므로 더이상 부모요소나 자식요소가 불필요하게 height를 가지고 있지 않아도 됩니다 -> 이건 요소 두 개를 만들고 하나는 float 하나는 안 하고 해제 했을 때랑, 둘 다 float했을 때 높이를 찍어보자.

width값은 floating된 요소의 width를 뺀 만큼 계산되지만, 영역 표시는 browser기준으로 끝까지 된다.



그렇다면 실제 평가에서 전체적인 진행 순서는 어떻게 하는 것이 좋을까? 그리고 각 단계마다 체크해야 할 것은?

1. 디렉토리 구성
   1. gulp / sass 를 사용하는 것까지 고려서 디렉토리 구성
2. 마크업 구성
   1. 웹 접근성 고려
      1. 마지막에 NAS에서 접근성 검사 ( 이건 아애 local url은 확인이 불가하네. 이건 물어봐야겠다. )
   2. 유효성 검사 고려 
      1. 태그마다 required 속성을 모두 넣었는지
      2. 마지막에 W3C에서 html 유효성 검사 : https://validator.w3.org/#validate_by_upload+with_options
3. 스타일링 구성
   1. 컨벤션으로 사용하는 css 초기화 적용 : https://html5boilerplate.com/
   2. UI 리뷰 리스트를 확인
   3. 안쓰는 CSS 선택자 확인 (https://www.jitbit.com/unusedcss/)
   4. 마지막에 W3C에서 css 유효성 검사 : https://jigsaw.w3.org/css-validator/
   5. 코딩 컨벤션 확인 ( markup , sass )