<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/5ddd5462-e517-437a-84e7-e227fb11a627/image.png" /></p>
<p>링크: <a href="https://yenaley.github.io/momentum/">https://yenaley.github.io/momentum/</a>
깃허브: <a href="https://github.com/YenaLey/momentum">https://github.com/YenaLey/momentum</a></p>
<p>바닐라JS를 공부하기 위해 Momentum을 클론코딩하며 내가 넣고 싶은 기능들을 넣어보았다.
버튼 요소를 최대한 없애고 키보드로 조작할 수 있는 깔끔한 디자인으로 하고 싶었다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/a3c2ef0e-786d-4fe0-8e61-a8cdf41aee3a/image.png" />
✅ todo 리스트를 클릭하여 가로선으로 완료된 일을 표시할 수 있고, 우클릭을 하여 삭제할 수 있다.
✅ 공부시간 스톱워치를 누르거나 c키를 눌러 측정 또는 정지를 할 수 있다.
✅ 유튜브 링크를 넣고 enter를 누르면 해당 동영상이 나온다. 새로고침을 하여 다시 설정할 수 있다.
✅ 이름, todo 리스트, 공부시간 스톱워치 기록은 재접속시에도 유지되도록 localstorage에 저장하였다. 오른쪽 아래 파란 화살표를 누르면 localstorage를 비우며 새로고침되어 로그아웃되도록 하였다.</p>
<blockquote>
<p>개발하며 필요했던 개념 정리</p>
</blockquote>
<h3 id="settimeout-setinterval-clearinterval">setTimeout, setInterval, clearInterval</h3>
<p>현재 시간을 구현하기 위해     <code>Date()</code>를 사용하여 현재 시간을 받아오고 1초마다 업데이트해줘야 했다. 이를 위해 <code>setInterval</code>을 사용하여 <code>setInterval(func, 1000);</code> 현재 시간을 받아오는 함수를 1초 간격으로 실행시켜 업데이트 되도록 하였다.</p>
<p>또 스톱워치를 구현할 때도 마찬가지로 1초를 더하는 로직을 1초 간격으로 실행시켰고, 정지를 할 때 <code>clearInterval</code>을 사용하여 반복실행을 멈추도록 하였다.</p>
<p>문득 <code>setTimeout</code>로 <code>setInterval</code>을 구현할 수 있을거 같은데 그 둘의 차이점은 무엇일지 궁금했다.</p>
<pre><code>let i = 1;
setInterval(function() {
  func(i++);
}, 100);

let i = 1;
setTimeout(function run() {
  func(i++);
  setTimeout(run, 100);
}, 100);</code></pre><p>첫 번째와 두 번째는 같은 동작을 할 거 같지만, 지연시간이 보장되는 유무에서 차이가 있다.
<code>setInterval</code>은 지연시간 100ms에 func(i++)의 실행 시간이 포함된다. 즉 func(i++)의 동작이 끝난 후의 지연시간은 100ms보다 짧다.
반면 <code>setTimeout</code>은 func(i++)의 동작이 끝난 후 100ms 후에 다시 실행이 되어 지연시간이 보장이 된다.</p>
<p>현재 시간과 스톱워치는 정확한 시간을 위해 로직이 처리되는 시간을 포함하여 1초 간격으로 실행이 되어야 해서 <code>setInterval</code>을 사용하였다.</p>
<h3 id="localstorage에-리스트-저장">localStorage에 리스트 저장</h3>
<p><code>localStorage</code>은 <code>localStorage.setItem(key, value)</code>에 의해 문자열인 key와 value 형태의 딕셔너리로 저장이 된다.</p>
<p>배열/객체도 문자열로 바꾸어 저장을 해야하는데,
<code>JSON.stringify()</code>로 배열/객체을 문자열로 바꾸어서 저장을 하고,
<code>JSON.parse()</code>로 문자열을 다시 배열/객체로 바꾸어 가져온다.</p>
<blockquote>
<p>첫 바닐라JS 프로젝트에 대한 후기</p>
</blockquote>
<p>이번 momentum 프로젝트는 바닐라JS에서 자주 사용되는 문법을 알아보기 위함이었다. DOM 조회, 수정, 삭제와 같은 메서드에는 익숙해진 거 같다.</p>
<p>바닐라JS를 공부하기로 마음 먹었던 이유는, 리액트는 빌드되며 바닐라JS로 변환이 되고 결국 SPA, MPA와 같은 동작 과정을 깊게 이해하려면 바닐라JS로 바뀌어 어떻게 처리되는지를 알아야겠다는 생각이 들었다.</p>
<p>그래서 다음에는 리액트와 같은 SPA을 바닐라JS로 구현하며 DOM 조작, 이벤트 처리, 라우팅, 상태 관리에 중점을 두어 공부해볼 계획이다.</p>