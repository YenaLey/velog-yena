<p>이제 실제 ESP32 보드의 LED 상태를 실시간으로 Autodesk Tandem에 반영하려 한다. 또한 Tandem에 업로드한 가상 모델을 웹 대시보드에 띄우고, 화면 속 스위치 모델을 클릭하면 실제 스위치가 눌리고 LED 상태가 변경되며, 이 변화가 다시 Tandem과 웹 화면에 동기화되도록 구현할 예정이다.</p>
<blockquote>
<p>ESP32 보드와 Tandem 연결하기</p>
</blockquote>
<p>ESP32에서 수집한 LED 상태 데이터를 Wi-Fi 2.4GHz를 통해 Tandem API로 전송한다.</p>
<p>우선 <code>3-Legged Token</code>이 필요하다.
<a href="https://velog.io/@yena121/Postman%EC%9C%BC%EB%A1%9C-Tandem-API-%ED%98%B8%EC%B6%9C%ED%95%B4%EB%B3%B4%EA%B8%B0">이 블로그</a>를 참고하여 <code>3-Legged Token</code>을 생성하여 API 요청할 때 같이 보내준다.</p>
<p><code>Tandem &gt; Users</code> 메뉴에서 <code>Add API key</code> 옵션을 선택한 후, <code>Client ID</code>를 입력하고 <em>Permission</em>을 <code>Manage</code>로 설정하면, 해당 App(Client)이 외부에서 Tandem Facility에 API를 통해 접근하고 조작할 수 있는 권한을 갖게 된다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/8e6bf014-5740-42d1-a333-66417ea805f3/image.png" /></p>
<p>각 Stream ID별로 개별 URL에 데이터를 전송할 수도 있지만, <code>Generic Webhook Endpoint</code>를 사용하면 여러 Stream의 데이터를 하나의 HTTP POST 요청으로 통합하여 전송할 수 있다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/f5a1c5ea-f560-45c5-8778-2ef56e9e33fb/image.png" /></p>
<ul>
<li>요청 URL<pre><code class="language-javascript">https://tandem.autodesk.com/api/v1/timeseries/models/urn:adsk.dtm:Fk2zdny7QDKxF07arLserg/webhooks/generic</code></pre>
</li>
<li>Payload
<code>id</code>: 각각의 Stream ID (Tandem에서 스트림 생성 시 자동 생성된 고유 ID)
<code>value</code>: 해당 Stream에 업데이트할 값 (숫자 또는 Boolean 등)<pre><code class="language-javascript">[
{
  &quot;id&quot;: &quot;AQAAAMnQ2pAQd0IOhK647hF6A30AAAAA&quot;,
  &quot;value&quot;: 1.0
},
{
  &quot;id&quot;: &quot;AQAAAKBmehwDIEhirCRRkXjT_GIAAAAA&quot;,
  &quot;value&quot;: 0.0
},
{
  &quot;id&quot;: &quot;AQAAAFTDpe_Zg0hFhKJalFYSkioAAAAA&quot;,
  &quot;value&quot;: 0.0
},
{
  &quot;id&quot;: &quot;AQAAAOD64XPdz0X-pr2YAFp5BkEAAAAA&quot;,
  &quot;value&quot;: 1.0
}
]
</code></pre>
</li>
</ul>
<pre><code>![](https://velog.velcdn.com/images/yena121/post/7cebbb51-2d59-4e7a-ae4b-de171647effb/image.png)
Postman에서 Tandem Generic Webhook Endpoint를 테스트해 200 OK 응답이 정상적으로 반환되는 것을 확인한 후, ESP32 펌웨어를 수정해준다.

ESP32는 `WiFi.h`를 이용해 Wi-Fi에 연결하고, `HTTPClient.h`를 사용해 Tandem Webhook으로 HTTP POST 요청을 전송한다.
![](https://velog.velcdn.com/images/yena121/post/6bc99850-4c08-4c16-8fc6-4b74c36d69a5/image.png) 시리얼 모니터를 통해 스위치를 누를 때마다 `HTTP Response: 200` 응답이 출력되고, Tandem에 값이 반영되는 것을 확인한다.
![](https://velog.velcdn.com/images/yena121/post/1a46bec3-9a5d-434e-9a0e-fff1191d206d/image.png) ![](https://velog.velcdn.com/images/yena121/post/6f010735-0a67-4864-9ef6-86f61c1863c5/image.gif)

&gt; 가상 모델에서 실제 모델 제어하기

`Autodesk Forge Viewer`를 사용해 웹에 가상 LED 모델을 시각화하고, 사용자가 스위치 가상 모델을 클릭하면, 이 이벤트가 실제 ESP32 장치로 전달되어 물리적인 LED가 제어된다.
이후 해당 상태는 Autodesk Tandem에 동기화되어, 가상 모델과 현실 상태가 실시간으로 일치하도록 구현했다.

우선 `Autodesk Forge Viewer`를 사용하기 위해 이번엔 `2-Legged Token`이 필요하다.

[Autodesk Platform Services](https://aps.autodesk.com)에서 `Server-to-Server App`을 선택하여 `Client ID`와 `Client Secret`을 생성한다. ![](https://velog.velcdn.com/images/yena121/post/839c492c-a267-45ac-b3b7-cfb591930969/image.png)![](https://velog.velcdn.com/images/yena121/post/a4d345be-76b4-4313-96a5-e9b6f9217270/image.png)![](https://velog.velcdn.com/images/yena121/post/a8448e94-f770-46f1-982b-82bcfbe41785/image.png)  

다음 프로젝트(https://github.com/Autodesk-Forge/models.autodesk.io)를 `npm install`과 `npm start.js`로 실행하고 http://localhost:3000 에 접속하여 방금 생성한 `Client ID`와 `Client Secret`을 넣어주어 토큰을 생성한다. ![](https://velog.velcdn.com/images/yena121/post/942ec6d4-58cf-47ff-832d-a9e8f427da51/image.png) 그리고 이 프로젝트에서 .rvt 파일을 업로드하면, 해당 파일은 Autodesk Forge의 Object Storage에 저장되고 `objectId`가 부여된다.
이 objectId는 Base64로 인코딩되어 `URN`으로 변환되며, 이를 이용해 Derivatives API에 `SVF 포맷`(2D 및 3D 뷰 포함)으로의 번역이 요청된다.
번역이 완료되면, 생성된 URN을 통해 `Forge Viewer`에서 해당 모델을 바로 시각화할 수 있다.
![](https://velog.velcdn.com/images/yena121/post/56d855a0-5172-495c-b2f0-b96a22878c3f/image.png) 보기 버튼(👁️)을 누르면 업로드한 모델을 `Forge Viewer`로 바로 확인할 수 있다.
이제 여기서 발급한 `2-Legged Token`과 변환된 `URN`을 사용해, 직접 만든 *Next.js* 웹 프로젝트에 *Forge Viewer*을 띄우려 한다.
![](https://velog.velcdn.com/images/yena121/post/7ae40e8f-8d84-4855-b54e-f7928840f1ef/image.png)

프론트엔드와 백엔드를 모두 통합할 수 있고, 서버리스로 간편하게 구현 가능한 `Next.js`를 기반으로 *Forge Viewer*를 웹에 연동하였다.

우선 모델을 띄우기 위해 위에서 생성한 `2-Legged Token`과 `URN`을 사용하여 Viewer 스크립트를 로드한 후 다음과 같이 초기화한다:
```javascript
Autodesk.Viewing.Initializer({ accessToken }, () =&gt; {
  viewer.start();
  Autodesk.Viewing.Document.load(`urn:${URN}`, (doc) =&gt; {
    viewer.loadDocumentNode(doc, doc.getRoot().getDefaultGeometry());
  });
});</code></pre><p>모델이 로드되면, 각 요소에는 <code>dbId</code>가 부여되며, 사용자가 특정 요소(예: 스위치)를 클릭할 경우 이벤트로 다음과 같이 제어할 수 있다:</p>
<pre><code class="language-javascript">viewer.addEventListener(Autodesk.Viewing.SELECTION_CHANGED_EVENT, (event) =&gt; {
  const dbId = event.dbIdArray[0];
  if (dbId === switchDbId) {
    fetch(&quot;/api/esp32/toggle-led&quot;, { method: &quot;POST&quot; });
  }
});</code></pre>
<p>이 요청은 내부적으로 ESP32 장치의 <code>/toggle-led</code> 엔드포인트를 호출하며, 해당 핸들러에서는 현재 점등 중인 LED를 순차적으로 전환하는 함수 <code>toggleLed()</code>가 실행된다. 즉, Forge Viewer에서의 클릭 이벤트가 실제 ESP32의 물리적 회로를 직접 제어하는 구조이다.</p>
<p>ESP32는 LED의 상태를 2초마다 Tandem Webhook을 통해 전송한다.
웹에서는 <code>/api/tandem/latest</code> 엔드포인트를 2초마다 호출하여 주기적으로 데이터를 받아 가상 LED 색상을 시각적으로 반영한다:</p>
<pre><code class="language-javascript">setInterval(async () =&gt; {
  const data = await fetch(&quot;/api/tandem/latest&quot;).then(res =&gt; res.json());
  viewer.setThemingColor(dbId1, data.led1 ? red : redDim);
}, 2000);</code></pre>
<p>이렇게 <code>가상 스위치 클릭 → 실제 장치 제어 → 상태 Tandem에 전송 → 가상 모델 갱신</code>이라는 양방향 디지털 트윈 제어 구조를 구현하였다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/f72e2855-fdd6-4baf-ade6-3a578f7a07c4/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/7007a1f3-e3af-48dd-82f3-5640d6ca80b5/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/01ab8bce-d967-4f6e-964f-1b37ad5a41fa/image.gif" /></p>
<p>다음은 위에서 설명한 구조를 기반으로 구현한 프로젝트의 GitHub이다.
<a href="https://github.com/YenaLey/StepLED">https://github.com/YenaLey/StepLED</a></p>
<p>이 프로젝트 <code>public &gt; assets</code> 안에 .rvt 모델 파일과 ESP32용 펌웨어 코드도 함께 포함하였다.
프로젝트를 실행하기 위해서는 루트 디렉터리에 <code>.env</code> 파일을 생성한 후, 아래와 같이 환경변수 값을 설정해주면 된다:</p>
<pre><code class="language-javascript">NEXT_PUBLIC_FORGE_ACCESS_TOKEN=&lt;2-Legged Token&gt;
NEXT_PUBLIC_FORGE_URN=&lt;Forge URN&gt;  

TANDEM_TOKEN=&lt;3-Legged Token&gt;   
TANDEM_MODEL_URN=&lt;Tandem Model URN&gt; 

ESP32_HOST=&lt;http://[ESP32 IP]&gt;</code></pre>
<blockquote>
<p>이번 프로젝트에 대한 회고 및 앞으로의 목표</p>
</blockquote>
<p>현재 구조는 ESP32가 2초마다 Tandem에 상태 데이터를 HTTP POST 방식으로 전송하고, 웹 대시보드는 2초마다 Tandem의 마지막 상태를 <code>Polling</code>한다. 구조적으로 간단하고 구현이 쉬운 장점이 있지만, 상태 변화가 없는 경우에도 반복적인 네트워크 요청이 발생하며, 실제 변화가 발생한 시점과 화면 반영 사이에 delay가 생길 수밖에 없다.</p>
<p>이러한 <code>풀(Polling)</code> 기반 구조는 불필요한 트래픽을 발생시킨다는 점에서 실시간 동기화가 중요한 디지털 트윈 구현에 적합하지 않다.</p>
<p>이를 개선하기 위해, 상태 변화가 발생했을 때만 데이터를 전송하는 이벤트 기반 구조로 전환하려 한다.<img alt="" src="https://velog.velcdn.com/images/yena121/post/91170aa1-a49c-43b6-b3e0-14841b634a04/image.png" />
<code>WebSocket</code>을 활용하여 양방향 연결을 유지하면, 상태 변화가 있을 때마다 즉시 push 방식으로 통신할 수 있어 폴링 없이도 빠른 반응이 가능하다.</p>
<p>또는, <code>MQTT</code>를 사용하여 ESP32와 서버, 웹 클라이언트 간에 pub/sub(발행-구독) 구조로 실시간 상태를 브로드캐스팅할 수도 있다. 이는 여러 장치가 동시에 연결된 상황에서 유리하다.</p>
<p><code>WebSocket</code>과 <code>MQTT</code>의 장단점을 비교 분석한 후, 실시간성이 핵심인 디지털 트윈 서비스에 더 적합한 방식을 선택해 구조를 개선할 계획이다.
이를 통해, 보다 빠르고 정확한 상태 반영이 가능한 실시간 디지털 트윈 플랫폼으로 발전시키도록 방안을 찾는 것이 다음 목표이다.</p>