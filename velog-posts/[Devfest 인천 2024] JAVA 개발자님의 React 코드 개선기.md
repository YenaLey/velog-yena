<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/3a539131-0460-45f0-ab22-1885c67dea8b/image.png" />
GDG가 주최하는 Devfest 인천 2024 컨퍼런스에 다녀왔다.
총 5개의 강연을 들었는데
async/await의 구동 원리, React에서 쉽게 버그나는 유형, Compose UI 조합 관련 강연들을 들었는데 경험이 많이 부족한 나에게는 한번에 이해하기 힘든 내용이었고,
마침 JS를 깊이 파고들어야겠다고 굳게 다짐을 했던 시점에서
현업 개발자분들이 직접 파고드는 내용을 보며 가이드를 얻을 수 있을 거 같았다. 그래서 강연에서 제공해준 자료를 보며 하나씩 이해하고 정리해보려 한다.</p>
<blockquote>
</blockquote>
<p>&quot;JAVA 개발자의 React 코드 개선기&quot; 강연을 듣고</p>
<p>이 강연에서는 React에서 쉽게 버그나는 유형 3가지를 설명해주었다.</p>
<p><strong>(1) UI 트리 동일 위치 컴포넌트 갈아끼우기</strong></p>
<p>☑️ 문제
리액트는 부모 컴포넌트로부터 위치를 기준으로 컴포넌트를 구분한다. 이 경우 Props가 달라지더라도 같은 위치에 렌더링이 되기 때문에 기존 상태가 유지되어 새로운 상태를 반영하지 않는다. <img alt="" src="https://velog.velcdn.com/images/yena121/post/d0edd3cd-fb0f-4869-ac9c-0f3a2cdb204b/image.png" />
✅ 해결방법1
조건부 렌더링을 사용해 <code>&lt;ListGrid /&gt;</code>와 <code>&lt;CardGrid /&gt;</code>를 별도의 위치에서 순서대로 렌더링한다.
조건에 따라 두 컴포넌트는 항상 동일한 순서로 렌더링하지만, 한쪽은 실제 DOM에 표시되고, 다른 쪽은 빈 컴포넌트로 처리된다.
React는 이를 &quot;다른 컴포넌트&quot;로 인식하여 각각의 상태를 독립적으로 관리한다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/35ac274c-de2d-4a50-a893-6e1437de42f1/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/8cfd1b3b-386c-4f45-835e-3074ae3b27d7/image.png" />
✅ 해결방법2
같은 자리에 컴포넌트를 그리더라도 <code>key</code>를 사용하면 다른 컴포넌트로 인식이 된다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/511a8dc2-348f-4c0b-950f-db97fc9f473d/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/302a8bdb-21c7-46c3-8656-bd07cb89cd80/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/d5aaabf0-944c-42ab-bd20-29ad5a5552b7/image.png" /></p>
<p><strong>(2) 호출 의존성 무시하는 코드 선언 위치</strong></p>
<p>☑️ 문제
채팅 기능에 들어갔는데 빈 화면이 나오는 문제가 발생했다.
컴포넌트가 <code>null</code>에 의해 조기 종료된 이후 함수 <code>scrollToBottom</code>가 초기화 되지 않은 상태에서 상위 Props에 의존하는 <code>useEffect</code>가 이를 호출하여 <code>Ref. Error</code>가 발생하였다. 
<img alt="" src="https://velog.velcdn.com/images/yena121/post/175fc085-4c61-4828-afc7-759b3a38eb22/image.png" /></p>
<p>✅ 해결방법
<code>TypeError: scrollToBottom is not a function</code> 이러한 에러는 문법적인 오류를 검사하는 컴파일 환경에서는 알기 어렵고 코드가 실행되어야 알 수 있는 런타임 환경에서 발견할 수 있는 오류이다.</p>
<p>이 에러를 해결하기 위해 '호이스팅' 개념을 알아야 한다.</p>
<p>JS에서 함수 선언과 변수 선언은 호이스팅 된다. 즉 코드 실행 전에 해당 선언이 스코프 최상단으로 끌어올려지며 선언과 초기화가 이루어진다.
함수 표현식은 호이스팅되지 않는다. 표현식은 변수에 값을 할당하는 방식으로, 변수는 선언만 호이스팅되고, 초기화는 런타임에 이루어진다. </p>
<pre><code>// 함수 선언 (정의)
sayHello(); // 정상 작동
function sayHello() {
  console.log(&quot;Hello!&quot;);
}

// 함수 표현식
sayHelloExpression(); // ReferenceError: Cannot access 'sayHelloExpression' before initialization
const sayHelloExpression = function () {
  console.log(&quot;Hello!&quot;);
};</code></pre><p><code>TypeError: scrollToBottom is not a function</code> 또는 <code>ReferenceError</code>이 에러는 <strong>런타임 에러</strong>이다. 함수 표현식은 초기화가 런타임에 이루어지므로, 컴파일러는 문법적으로 문제가 없다고 판단하고 통과시킨다. </p>
<p>첫 번째 해결방법은,
함수 표현식을 함수 선언으로 작성하는 것이다.</p>
<pre><code>function scrollToBottom(): void {
  if (conversationScrollRef.current) {
    conversationScrollRef.current.scrollIntoView();
  }
}</code></pre><p>두 번째 해결방법은,
조기 종료 코드를 Main 컴포넌트 return 문 바로 직전에 작성하는 것이다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/67bdf37d-81ea-4ff5-861c-070136df4e21/image.png" /> </p>
<p><strong>(3) 너무 복잡한 조건부 렌더링</strong></p>
<p>☑️ 문제
너무 많은 렌더링 조건문 코드를 수정한다면, 모든 경우에 정확하게 렌더링될지 알기 어렵다. <img alt="" src="https://velog.velcdn.com/images/yena121/post/526441e9-49a5-44f5-adc8-5a115afd79c3/image.png" /></p>
<p>✅ 해결방법
조건을 판단하는 커스텀 훅을 만들어 사용하여,
client code에서 조건을 판단하는 로직을 넣지 말고 최종 결과만 확인할 수 있도록 하는게 좋다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/6696dad1-8975-4136-b533-462305564714/image.png" /></p>