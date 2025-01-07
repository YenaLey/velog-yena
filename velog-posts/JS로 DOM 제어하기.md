<blockquote>
</blockquote>
<ol>
<li>Window, DOM, BOM 이란?</li>
<li>DOM 가져오기</li>
<li>DOM 추가 / 수정 / 삭제</li>
<li>DOM 이벤트 처리</li>
</ol>
<h2 id="✅-window-dom-bom-이란">✅ Window, DOM, BOM 이란?</h2>
<h3 id="✔️-window">✔️ Window</h3>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/1658f6eb-3c2b-4f49-be94-d5e928142ccc/image.png" />
브라우저의 가장 최상위 전역 객체로 <strong>DOM</strong>와 <strong>BOM</strong>로 구성되어 있다.
대부분의 브라우저에서 사용되고 선언된 객체는 <code>window</code> 객체의 속성으로 저장된다.
전역 객체를 참조하는 <code>this</code>는 <code>window</code>와 같다.</p>
<p>var로 선언된 변수는 window 객체 속성으로 추가되고, let이나 const로 선언된 변수는 추가되지 않는다.</p>
<pre><code>// var로 선언한 변수는 window 객체에 속성으로 추가됨
var myVar = 10;
console.log(window.myVar); // 10
console.log(this.myVar);   // 10
console.log(this === window); // true

// let이나 const로 선언한 변수는 window 객체에 추가되지 않음
let myLetVar = 20;
const myConstVar = 30;

console.log(window.myLetVar); // undefined
console.log(window.myConstVar); // undefined</code></pre><p>브라우저가 웹 페이지를 로드할 때, 
브라우저 엔진이 메모리(RAM)에 window 객체를 생성하고, 
window 객체에는 DOM과 BOM 등의 정보가 담긴다. 
이후 JavaScript 엔진이 window 객체를 통해 웹 페이지의 요소들과 상호작용한다.</p>
<h3 id="✔️-dom-document-object-model">✔️ DOM (Document Object Model)</h3>
<p>브라우저가 HTML 문서을 이해할 수 있도록 객체화한 구조
태그마다 일대일로 객체를 만든다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/f461a482-d29c-4174-b025-986c97d2de4d/image.png" /></p>
<p>문서노드(document node) - 트리의 최상위 노드, 시작점
요소노드(element node) - 태그
어트리뷰트노드(attribute node) - 태그 안 속성들
텍스트노드(text node) - 태그 내 텍스트, 요소노드의 자식이며 DOM 트리의 최종단</p>
<h3 id="✔️-bom-browser-object-model">✔️ BOM (Browser Object Model)</h3>
<p>브라우저와 소통하는 데 필요한 객체들의 집합
document 문서가 아닌 window를 제어
navigator, location, document, screen, history로 구성</p>
<h3 id="✔️-javascript">✔️ JavaScript</h3>
<p>DOM, BOM 을 제어
DOM을 제어할 때 <code>window.document.querySelector</code> 또는 <code>document.querySelector</code>
BOM을 제어할 때 <code>window.location</code>
querySelector은 DOM 안에 메서드이고, location은 window 안에 객체이다.</p>
<h2 id="✅-dom-가져오기">✅ DOM 가져오기</h2>
<p><code>document.querySelectorAll('ul &gt; li:last-child')</code> css 선택자에 대응하는 모든 요소들 반환
<code>document.querySelectorAll(':hover')</code>가상 클래스(pseudo-class)도 사용</p>
<p><code>querySelector</code> css 선택자에 대응하는 첫 번째 요소 반환
<code>elem.querySelectorAll(css)[0]</code>와 동일</p>
<p><code>getElementById</code> 해당 ID를 가진 하나의 요소 반환
<code>getElementByName</code> <code>getElementsByTagName</code> <code>getElementsByClassName</code>
해당하는 요소를 담은 컬렉션 반환
<em>살아있는</em> 컬렉션을 반환하고, 문서에 변경이 생기면 자동 갱신된다.</p>
<pre><code>&lt;div&gt;첫 번째 div&lt;/div&gt;

&lt;script&gt;
  let divs = document.getElementsByTagName('div');
  alert(divs.length); // 1
&lt;/script&gt;

&lt;div&gt;두 번째 div&lt;/div&gt;

&lt;script&gt;
  alert(divs.length); // 2
&lt;/script&gt;</code></pre><p>querySelectorAll은 <em>정적인</em> 컬렉션 반환하고, 문서에 변경되어도 반영하지 못한다</p>
<pre><code>&lt;div&gt;첫 번째 div&lt;/div&gt;

&lt;script&gt;
  let divs = document.querySelectorAll('div');
  alert(divs.length); // 1
&lt;/script&gt;

&lt;div&gt;두 번째 div&lt;/div&gt;

&lt;script&gt;
  alert(divs.length); // 1
&lt;/script&gt;</code></pre><p><code>elem.closest(css selector)</code> DOM 요소에서 css 선택자에 해당하는 가장 가까운 상위 요소를 반환</p>
<pre><code>&lt;div class=&quot;parent&quot;&gt;
  &lt;div class=&quot;child&quot;&gt;
    &lt;div class=&quot;grandchild&quot;&gt;&lt;/div&gt;
  &lt;/div&gt;
&lt;/div&gt;</code></pre><pre><code>const grandchild = document.querySelector('.grandchild');

// closest() 메소드 사용
const parent = grandchild.closest('.parent');

console.log(parent); // &lt;div class=&quot;parent&quot;&gt;...&lt;/div&gt;</code></pre><p><code>str.matches(정규식)</code> 정규식과 일치하는 문자열을 배열로 반환</p>
<pre><code>var re = /see (chapter \d+(\.\d)*)/i;
var str = &quot;For more information, see Chapter 3.4.5.1&quot;;
var result = str.match(re);

console.log(result);

// 결과값
[
  &quot;see Chapter 3.4.5.1&quot;, // 전체 일치한 문자열
  &quot;Chapter 3.4.5.1&quot;,     // 첫 번째 그룹 &quot;(chapter \d+(\.\d)*)&quot;
  &quot;.1&quot;                   // 두 번째 그룹 &quot;(\.\d)&quot;
]
</code></pre><pre><code>var str = &quot;ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz&quot;;
var regexp = /[A-E]/gi; //g 플래그(모든 일치 항목을 찾음), i 플래그(대소문자를 구분하지 않음)
var matches_array = str.match(regexp);

console.log(matches_array);
// ['A', 'B', 'C', 'D', 'E', 'a', 'b', 'c', 'd', 'e']</code></pre><p><code>eleA.contains(elemB)</code> elemB가 eleA의 자손인지 여부에 따라 boolean 값 반환</p>
<h2 id="✅-dom-추가--수정--삭제">✅ DOM 추가 / 수정 / 삭제</h2>
<h3 id="✔️-생성-및-추가">✔️ 생성 및 추가</h3>
<p><code>createElement</code> 해당 태그를 가진 HTML 요소를 만들어 반환
<code>appendChild</code> 하나의 노드를 부모의 마지막 자식으로 추가, 추가된 노드를 반환
<code>append</code> 다수의 노드나 텍스트를 부모의 마지막 자식으로 추가, 반환값 없음</p>
<pre><code>var newDiv = document.createElement('div');  // &lt;div&gt;&lt;/div&gt; 생성
var newText1 = document.createTextNode(&quot;Hello&quot;);  // 텍스트 노드 1 생성
var newText2 = document.createTextNode(&quot; World!&quot;);  // 텍스트 노드 2 생성

newDiv.appendChild(newText1);  // &lt;div&gt;Hello&lt;/div&gt;
newDiv.appendChild(newText2);  // &lt;div&gt;Hello World!&lt;/div&gt;

document.body.appendChild(newDiv);  // &lt;body&gt; 안에 &lt;div&gt;Hello World!&lt;/div&gt; 추가</code></pre><pre><code>var newDiv = document.createElement('div');  // &lt;div&gt;&lt;/div&gt; 생성

// append()는 여러 텍스트와 노드를 동시에 추가 가능
newDiv.append(&quot;Hello&quot;, &quot; World!&quot;, document.createElement(&quot;span&quot;));

// &lt;div&gt;Hello World!&lt;span&gt;&lt;/span&gt;&lt;/div&gt;
document.body.append(newDiv);  // &lt;body&gt; 안에 &lt;div&gt;Hello World!&lt;span&gt;&lt;/span&gt;&lt;/div&gt; 추가</code></pre><h3 id="✔️-수정">✔️ 수정</h3>
<p><code>innerHTML</code> HTML 태그를 포함한 전체 내용을 문자열로 반환
<code>innerText</code> 화면에 보이는 텍스트만 가져오며, HTML 태그, 스타일 무시
<code>textContent</code> 모든 텍스트를 가져오며, HTML 태그, 스타일 무시</p>
<pre><code>&lt;div id='content'&gt;
  text1
  &lt;span style='color: red'&gt;text2&lt;/span&gt;
  &lt;span style='display:none'&gt;text3&lt;/span&gt;
&lt;/div&gt;

&lt;script&gt;
  const content = document.getElementById('content');

  console.log(content.innerHTML);
  // 결과:
  // text1
  // &lt;span style='color: red'&gt;text2&lt;/span&gt;
  // &lt;span style='display:none'&gt;text3&lt;/span&gt;

  console.log(content.innerText);
  // 결과:
  // text1
  // text2

  console.log(content.textContent);
  // 결과:
  // text1
  // text2
  // text3
&lt;/script&gt;</code></pre><p>innerHTML는 HTML 태그를 해석해서 DOM에 반영하고, HTML을 텍스트로 출력하려면 태그 기호가 HTML 엔티티로 변환하여 출력한다.
innerText와 textContent는 HTML 태그를 해석하지 않고 텍스트로 처리하며, innerText는 화면에 보이는 텍스트만, textContent는 숨겨진 텍스트까지 모두 가져온다.</p>
<pre><code>&lt;div id='example'&gt;&lt;/div&gt;

&lt;script&gt;
  const example = document.getElementById('example');

  // 1. innerHTML을 사용하여 값을 넣기
  example.innerHTML = &quot;&lt;strong style='color: green'&gt;text&lt;/strong&gt;&quot;;

  console.log(example.innerHTML); // &lt;strong style='color: green'&gt;text&lt;/strong&gt;
  console.log(example.innerText); // text
  console.log(example.textContent); // text
  //-----------------------------------------------------

  // 2. innerText를 사용하여 값을 넣기
  example.innerText = &quot;&lt;em style='color: blue'&gt;text&lt;/em&gt;&quot;;

  console.log(example.innerHTML); // &amp;lt;em style='color: blue'&amp;gt;text&amp;lt;/em&amp;gt;
  console.log(example.innerText); // &lt;em style='color: blue'&gt;text&lt;/em&gt;
  console.log(example.textContent); // &lt;em style='color: blue'&gt;text&lt;/em&gt;
  //-----------------------------------------------------

  // 3. textContent를 사용하여 값을 넣기
  example.textContent = &quot;&lt;span style='color: red'&gt;text&lt;/span&gt;&quot;;

  console.log(example.innerHTML); // &amp;lt;span style='color: red'&amp;gt;text&amp;lt;/span&amp;gt;
  console.log(example.innerText); // &lt;span style='color: red'&gt;text&lt;/span&gt;
  console.log(example.textContent); // &lt;span style='color: red'&gt;text&lt;/span&gt;
&lt;/script&gt;</code></pre><p><code>getAttribute</code> 요소가 가지고 있는 속성의 값을 반환, 없으면 null 반환
<code>setAttribute</code> 요소에 새로운 속성과 값을 설정, 반환값 없음(undefined)
<code>removeAttribute</code> 요소에서 특정 속성 제거, 반환값 없음(undefined)</p>
<pre><code>&lt;div&gt;&lt;/div&gt;
&lt;script&gt;
  const elem = document.querySelector('div');

  elem.setAttribute('id', 'wrap');
  console.log(elem.outerHTML); // &lt;div id=&quot;wrap&quot;&gt;&lt;/div&gt;

  console.log(elem.getAttribute('id')); // 반환값: 'wrap'

  elem.removeAttribute('id');
  console.log(elem.outerHTML); // &lt;div&gt;&lt;/div&gt;
&lt;/script&gt;</code></pre><h3 id="✔️-삭제">✔️ 삭제</h3>
<ol>
<li><p><code>elem.innerHTML=''</code>
요소의 HTML 내용을 빈 문자열로 교체하여 모든 자식 요소를 삭제한다.
elem의 자식요소가 모두 삭제되고 빈 문자열을 HTML로 파싱하여 DOM 트리가 다시 만들어지고, elem의 자식 요소로 넣는다.
재파싱이 필요하여 성능이 저하될 수 있다.</p>
</li>
<li><p><code>elem.textContent=''</code>
elem 내 모든 텍스트 노드를 삭제한다.
DOM을 재파싱하지 않는다.</p>
</li>
<li><p><code>elem.replaceChildren()</code>
elem의 모든 자식 요소들이 삭제된다.
DOM 요소를 직접 다루기 때문에 재파싱이 필요 없다.
최적화된 방식으로 DOM을 처리하므로 성능이 매우 좋다.</p>
</li>
<li><p><code>removeChild()</code>
elem에서 첫 번째 자식 노드를 하나씩 가져와 삭제한다.</p>
<pre><code>while(elem.firstChild) { elem.removeChild(elem.firstChild); }</code></pre></li>
<li><p><code>remove()</code>
elem의 첫 번째 자식 노드 자신을 하나씩 삭제한다.</p>
<pre><code>while(elem.firstChild) { elem.firstChild.remove(); }</code></pre></li>
</ol>
<h2 id="✅-event-처리하기">✅ Event 처리하기</h2>
<p>이벤트란 웹 페이지에서 발생하는 사용자 상호 작용이다.</p>
<h3 id="✔️-이벤트-종류">✔️ 이벤트 종류</h3>
<p><strong>마우스 이벤트</strong>
<code>click</code> 마우스 클릭
<code>dblclick</code> 마우스 더블클릭
<code>mousedown</code> 마우스 누를 때
<code>mouseup</code> 마우스 뗐을 때
<code>mousemove</code> 마우스 움직일 때
<code>mouseover</code> 마우스가 요소 위로 올라갈 때
<code>mouseout</code> 마우스가 요소 바깥으로 나갈 때</p>
<p><strong>키보드 이벤트</strong>
<code>keydown</code> 키보드 누를 때
<code>keyup</code> 키보드 뗐을 때
<code>keypress</code> 키보드를 누르고 뗐을 때 모두</p>
<p><strong>폼 이벤트</strong>
<code>submit</code> 폼을 제출할 때
<code>change</code> 입력 요소 값이 변경될 때
<code>input</code> 입력 요소에 사용자가 입력할 때</p>
<p><strong>포커스 이벤트</strong>
<code>focus</code> 요소에 포커스될 때
<code>blur</code> 요소에 포커스가 해제될 때</p>
<p><strong>윈도우 이벤트</strong>
<code>load</code> 자원이 로드될 때
<code>resize</code> 창 크기가 변경될 때
<code>scroll</code> 스크롤바가 움직일 때</p>
<h3 id="✔️-이벤트-객체">✔️ 이벤트 객체</h3>
<p><code>e.target.value</code> 이벤트가 실제로 발생한 요소의 현재 값 반환
<code>e.currentTarget</code> 이벤트 핸들러가 연결된 요소</p>
<pre><code>&lt;div id=&quot;parent&quot;&gt;
  &lt;button id=&quot;child&quot;&gt;Click me&lt;/button&gt;
&lt;/div&gt;

&lt;script&gt;
  const parent = document.getElementById(&quot;parent&quot;);

  parent.addEventListener(&quot;click&quot;, (e) =&gt; {
    console.log(&quot;target:&quot;, e.target); // 클릭된 실제 요소 (button)
    console.log(&quot;currentTarget:&quot;, e.currentTarget); // 이벤트가 걸린 요소 (div)
  });
&lt;/script&gt;</code></pre><p><code>e.preventDefault()</code> 이벤트의 기본 동작(새로고침, 링크 이동 등)을 막음, ex) 폼 제출 시 페이지 새로고침 막음
<code>e.stopPropagation()</code> 이벤트 전파(버블링, 캡처링) 차단</p>
<h3 id="✔️-이벤트-리스너-등록-및-삭제">✔️ 이벤트 리스너 등록 및 삭제</h3>
<p><code>addEventListener</code> 메서드를 통해 요소에 <strong>이벤트 핸들러</strong>(실행될 함수)를 등록하는 방식</p>
<pre><code>&lt;button id=&quot;button&quot;&gt;클릭&lt;/button&gt;

var Button = document.getElementById('button');
Button.addEventListener('click', myEvent);

function myEvent() {
  alert('클릭!');
}</code></pre><p>click 이벤트 발생 시 alert('클릭!');가 실행된다.
만약 <code>Button.addEventListener('click', myEvent());</code> 로 함수에 괄호를 붙이면 어떻게 될까?
함수를 즉시 실행하라는 의미로, 클릭 이벤트가 발생하지 않아도 함수가 바로 호출된다.
즉, myEvent() 함수의 반환값을 이벤트 핸들러로 전달하게 되어 반환하지 않는다면 undefined가 전달되어 이벤트 발생 시 아무 일도 일어나지 않게 된다.</p>
<p>또 어떻게 함수를 정의하기 전에 이벤트 핸들러로 전달할 수 있었을까?
함수가 <code>호이스팅</code> 되었기 때문인데, JS에서 선언된 변수와 함수는 코드 실행 전에 상단으로 끌어올려진다. 단, let와 const로 선언된 변수는 초기화 전에 접근하면 에러가 난다.</p>
<pre><code>var Button = document.getElementById('button');
Button.addEventListener('click', myEvent); //error

const myEvent = () =&gt; {
  alert('클릭!');
}</code></pre><h4 id="인라인-이벤트-핸들러">인라인 이벤트 핸들러</h4>
<p>HTML 태그 내에서 인라인 이벤트 핸들러 <code>onclick</code>의 속성에 함수를 문자열로 작성하여 이벤트를 처리할 수 있다.
<code>&lt;button onclick=&quot;myEvent()&quot;&gt;클릭&lt;/button&gt;</code>
문자열로 작성된 코드를 실행하기 때문에 ()을 붙여야 한다. 생략하면 참조만 하고 호출하지 않는다.
이 문자열을 실행 가능한 JS 코드로 변환하여 처리한다.</p>
<p>근데 인라인 이벤트 핸들러는 가독성, 유지보수 등의 이유로 추천되지 않는다.
HTML과 JS를 다른 파일로 분리하는 것이 바람직하다.</p>
<p>여기서 궁금한게 있다. 리액트에서 인라인 이벤트 핸들러처럼 이벤트를 처리하지 않나?
결론은 인라인 이벤트 핸들러와는 다르다.</p>
<pre><code>const handleClick = () =&gt; {
  alert('클릭!');
};

&lt;button onClick={handleClick}&gt;클릭&lt;/button&gt;</code></pre><p><u>JSX는 JS로 컴파일될 때, 내부적으로 <code>addEventListener</code>로 처리된다.</u></p>
<p>window 객체에 이벤트 리스너를 등록한 경우</p>
<pre><code>function handleResize() {
    console.alert(&quot;Resizing the browser.&quot;);
}

window.addEventListener(&quot;resize&quot;, handleResize);</code></pre><p>DOM 요소에 이벤트 리스너를 등록한 경우</p>
<pre><code>const title = document.querySelector(&quot;#title&quot;);
title.innerHTML = &quot;This is JAVASCRIPT!!!! &quot;;
title.style.color = &quot;white&quot;;

function handleClick() {
  title.style.color = &quot;blue&quot;;
}

title.addEventListener(&quot;click&quot;, handleClick);</code></pre><h4 id="이벤트-리스너-제거">이벤트 리스너 제거</h4>
<p>등록된 이벤트 리스너를 <code>removeEventListener</code> 메서드로 제거할 수 있다.</p>
<pre><code>function handleClick() {
  console.log(&quot;Button clicked!&quot;);
}

const button = document.querySelector('button');
button.addEventListener('click', handleClick);

button.removeEventListener('click', handleClick);</code></pre><p>다음 상황에서 제거해야 한다.</p>
<ol>
<li><strong>메모리 누수 방지</strong> - DOM 요소가 삭제되었는데 등록된 이벤트 리스너가 남아 있는 경우 메모리 누수가 발생할 수 있다.</li>
<li><strong>중복된 이벤트 리스너 등록 방지</strong> - 같은 이벤트에 대해 여러 번 리스너를 등록하면, 중복 이벤트 호출이 발생할 수 있다.</li>
</ol>
<p>리액트에서 useEffect에서 마운트될 때 이벤트 리스너를 등록하고 언마운트 또는 의존성 배열이 변경되며 컴포넌트가 사라질 때 return안에서 이벤트 리스너가 남아 있지 않도록 하여 메모리 누수를 방지할 수 있다.</p>
<pre><code>  useEffect(() =&gt; {
    function handleResize() {
      console.log('Resizing the window');
    }

    // 이벤트 리스너 등록
    window.addEventListener('resize', handleResize);

    // 컴포넌트가 언마운트될 때 이벤트 리스너 제거
    return () =&gt; {
      window.removeEventListener('resize', handleResize);
    };
  }, []);</code></pre><blockquote>
</blockquote>
<p>레퍼런스
<a href="https://velog.io/@kim_unknown_/JavaScript-Difftext">https://velog.io/@kim_unknown_/JavaScript-Difftext</a>
<a href="https://hianna.tistory.com/722">https://hianna.tistory.com/722</a>
<a href="https://ramincoding.tistory.com/entry/JavaScript-%EC%9D%B4%EB%B2%A4%ED%8A%B8%EC%99%80-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%95%B8%EB%93%A4%EB%9F%AC-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EA%B0%9D%EC%B2%B4-evente">https://ramincoding.tistory.com/entry/JavaScript-%EC%9D%B4%EB%B2%A4%ED%8A%B8%EC%99%80-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%95%B8%EB%93%A4%EB%9F%AC-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EA%B0%9D%EC%B2%B4-evente</a></p>