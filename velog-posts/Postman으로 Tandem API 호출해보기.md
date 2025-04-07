<blockquote>
<p>Postman에서 Tandem API 호출해보기</p>
</blockquote>
<p>Tandem API를 테스트해볼 수 있는 공식 Postman 문서가 있다.
<a href="https://documenter.getpostman.com/view/15787353/2s9YXk2faD">https://documenter.getpostman.com/view/15787353/2s9YXk2faD</a></p>
<p>이 사이트는 View 전용이기 때문에, 실제로 사용하려면 Postman에 로그인한 후 Import를 통해 불러와야 한다.</p>
<p>Import한 뒤에는 각 API 요청에 사용할 수 있도록 Access Token을 Authorization에 직접 넣어줘야 한다.
(예: Bearer YOUR_ACCESS_TOKEN)</p>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/01db4d2e-ea0f-4065-873c-d065c5574c92/image.png" /></p>
<h3 id="✅-토큰-발급하기">✅ 토큰 발급하기</h3>
<p><a href="https://aps.autodesk.com/">https://aps.autodesk.com/</a> 에서 애플리케이션을 생성할 때, 아래와 같은 창이 뜬다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/fbe48aed-0c64-4ac5-94bc-9e2d00fb0598/image.png" />
이때 반드시 첫 번째 옵션인 Traditional Web App(3-legged 인증)을 선택해야 한다.
Server-to-Server App(2-legged 인증) 방식으로는 사용자 인증이 없기 때문에 Autodesk Docs, BIM 360, Tandem과 같은 유저 기반의 리소스에 접근할 수 없다. 즉 대부분의 Tandem API (예: 문서 조회, 모델 연동, 요소 데이터 호출) 등이 제대로 작동하지 않는다.
반드시 <code>Traditional Web App</code>을 선택하자.
사용자 인증을 포함하는 방식(Authorization Code Grant)으로
Tandem, Autodesk Docs, Forge Viewer, ACC 연동 기능 등 전체 기능을 사용할 수 있다.</p>
<p>생성하면 이렇게 <code>Client ID</code>와 <code>Client Secret</code>이 발급된다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/6e60c064-530f-4908-b442-38ddd049b6d8/image.png" /> Callback URL은 <code>http://localhost:1234/token/forgeoauth</code>으로 설정해준다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/23a8fb50-9b68-43aa-bbe1-33ddd7d1c891/image.png" /></p>
<p>이제 직접 3-legged 인증을 구현해서 토큰을 발급받을 수도 있지만,
이미 Node.js로 잘 구성된 예제 프로젝트가 있다.
<a href="https://github.com/xiaodongliang/Forge-3legged-Node-Cli">https://github.com/xiaodongliang/Forge-3legged-Node-Cli</a></p>
<p>이 프로젝트를 로컬에서 실행하기 전에,
<code>config.js</code> 파일에 앞서 발급받은 <code>Client ID</code>와 <code>Client Secret</code>을 입력해주면 된다.<img alt="" src="https://velog.velcdn.com/images/yena121/post/f7374ecb-e819-479a-a3f4-518b76f70811/image.png" /></p>
<p>또 <code>token.js</code>에서</p>
<pre><code>const refreshInterval = 30; // 30 minutes
// const refreshInterval = 0.3; // 18 seconds, just for demo </code></pre><p>개발 편의를 위해 이렇게 주석처리를 바꿔주고,</p>
<pre><code>var url =
  &quot;https://developer.api.autodesk.com&quot; +
  &quot;/authentication/v1/authorize?response_type=code&quot; +
  &quot;&amp;client_id=&quot; +
  config.credentials.client_id +
  &quot;&amp;redirect_uri=&quot; +
  config.callbackURL +
  &quot;&amp;scope=&quot; +
  config.scope.join(&quot; &quot;);</code></pre><p>여기에서 v1을 v2로 바꿔준다.</p>
<p>이제 <code>node index.js</code>로 실행하면
브라우저가 열리고 Autodesk 로그인을 진행할 수 있다.</p>
<p>로그인을 완료하면 아래와 같은 화면이 뜬다.
여기서 Allow 버튼을 눌러 권한을 허용해주자.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/43471dc5-67f9-49ea-9387-c39126370da4/image.png" />
그러면 콘솔 창에 다음과 같이 Access Token이 발급된 것을 확인할 수 있다.
(토큰은 코드에서 설정한 주기(예: 30분)마다 자동으로 갱신되도록 되어 있다. Postman에서 API를 테스트할 때는 가장 최신의 토큰을 사용해야 하므로, 테스트 전 콘솔에 출력된 Access Token을 복사해 최신 상태로 갱신해주어야 한다.)
<img alt="" src="https://velog.velcdn.com/images/yena121/post/0a08d116-060d-4bb2-b197-8592d2c264fa/image.png" /></p>
<h3 id="✅-postman에서-tandem-api-호출하기">✅ Postman에서 Tandem API 호출하기</h3>
<p>이제 간단한 API 호출을 직접 해보자.
<code>TandemAuthToken</code>에는 발급받은 access token을 넣어주고,
<code>:twinID</code> 자리에는 Tandem URL의 뒷부분,
예: urn:adsk.dtt:XCa3fSR8R6SNHNbK91GaIg 를 그대로 넣어주면 된다.</p>
<p>예시로 다음 두 개의 API를 호출해보자:</p>
<ul>
<li><code>/twins/:twinID/thumbnail</code></li>
<li><code>/twins/:twinID</code></li>
</ul>
<p>요청을 보내면 각각 정상적으로 응답이 오는 것을 확인할 수 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/a00969c7-94ce-456a-9957-84beb7641b14/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/48cc3e1d-cdc1-40ee-9208-7b13136d76ca/image.png" /></p>