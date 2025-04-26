<p>ESP32에 LED와 스위치를 연결해 간단한 IoT 기기를 만들고, 이를 Autodesk Tandem 기반 웹 대시보드와 실시간으로 연동하는 토이 프로젝트(프로젝트 이름은 StepLED)를 진행해보려 한다.</p>
<blockquote>
<p>StepLED 프로젝트 소개</p>
</blockquote>
<p>스위치를 누를 때마다 Red, Green, Yellow LED가 순서대로 켜지면서, 이 상태가 Tandem 모델 안에서도 실시간으로 반영된다. 가능하다면, Tandem 모델을 웹 대시보드에 올려 사용자가 직접 색상을 선택하거나 모델 요소를 클릭했을 때 해당 LED 모델이 켜지도록, 그리고 이 상태가 실제 LED에도 반영되도록 할 계획이다.</p>
<blockquote>
<p>이번 블로그에서 다룰 내용은?</p>
</blockquote>
<p>이번 글에서는 Revit에서 만든 모델을 Tandem에 올리기 전에, Tandem에서 제대로 작업하려면 Revit 쪽에서 어떤 설정이 필요한지 정리해보려 한다.
그냥 Revit에서 모양만 만들어 올리는 걸로는 부족하다. Tandem에서는 Revit 모델 안의 각 요소를 식별하고, Tandem에서 미리 정의한 템플릿과 연결해야 한다. 따라서 Revit 단계에서 필요한 정보를 미리 잘 정의해줘야 한다.</p>
<p>정리하면, 이번 글에서는 Tandem에 올리기 위한 Revit 설정들과, Revit 모델을 Tandem에 올리고 실제 센서와 연결하기 전에 필요한 맵핑 과정을 다룰 예정이다. 이후에 센서와 실제로 연동해서 현재 상태가 Tandem 안에서 실시간으로 반영되도록 하는 과정을 다뤄보겠다.</p>
<blockquote>
<p>첫 번째, 회로 설계</p>
</blockquote>
<p>회로는 Autodesk Tinkercad를 이용해 먼저 설계하고 시뮬레이션으로 동작을 확인한 뒤, 실제 하드웨어로 구현해 테스트했다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/2f53ff00-e7a6-4a8b-8b38-62d5574539ee/image.png" /></p>
<p>동작 코드는 다음과 같다.
현재는 단순히 동작 확인만을 위한 코드이며, 이후 서버와 통신할 수 있도록 수정할 예정이다.</p>
<pre><code class="language-c">const int buttonPin = 33;             // 버튼 입력 핀
const int ledPins[] = {27, 26, 25};  // 빨강, 초록, 노랑 LED 핀 번호
int currentLed = 0;                  // 현재 켜진 LED 인덱스
int lastButtonState = HIGH;
int buttonState = HIGH;
int debounceDelay = 50;             // 디바운싱 시간 (ms)
unsigned long lastDebounceTime = 0;

void setup() {
  for (int i = 0; i &lt; 3; i++) {
    pinMode(ledPins[i], OUTPUT);
    digitalWrite(ledPins[i], LOW);   // 모든 LED 끄고 시작
  }

  pinMode(buttonPin, INPUT_PULLUP);  // 내부 풀업 저항 사용
  digitalWrite(ledPins[currentLed], HIGH);  // 첫 번째 LED 켜기
}

void loop() {
  int reading = digitalRead(buttonPin);

  if (reading != lastButtonState) {
    lastDebounceTime = millis();  // 상태가 바뀌면 시간 기록
  }

  if ((millis() - lastDebounceTime) &gt; debounceDelay) {
    // 실제 상태가 안정됐을 때만 상태 반영
    if (reading != buttonState) {
      buttonState = reading;

      if (buttonState == LOW) {
        // 현재 LED 끄기
        digitalWrite(ledPins[currentLed], LOW);

        // 다음 LED로 이동
        currentLed = (currentLed + 1) % 3;

        // 다음 LED 켜기
        digitalWrite(ledPins[currentLed], HIGH);
      }
    }
  }

  lastButtonState = reading;
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/31e54e10-ca78-45cf-ae7a-d85cf2532d28/image.png" />ESP32 보드에 직접 업로드해 테스트했으며, 원하는 대로 잘 동작하는 것을 확인했다.<img alt="" src="https://velog.velcdn.com/images/yena121/post/4fa746e5-89d0-46f1-8d3a-7543360c7547/image.gif" /></p>
<blockquote>
<p>두 번째, Revit 모델 만들기 (Revit 버전 26.0.4.409 기준)</p>
</blockquote>
<p>Revit에서 프로젝트를 생성하면 다음 화면이 뜬다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/f3fbe986-b60c-4610-b8e8-c0a50fbf1201/image.png" /> Revit을 처음 다뤄보기 때문에, 이번에는 ESP32 보드나 전선, 저항 같은 세부적인 것들은 다음으로 미루고, 일단 필요한 Bread Board, LED 세 개, 스위치 하나만 모델링해볼 거다.</p>
<p>지금 화면에 보이는 평면도는 위에서 내려다본 모습을 담은 2D 구조도이고, 동그라미 네 개는 각각 입면도 생성을 위한 카메라 같은 역할을 한다.
왼쪽 패널을 보면 입면도 쪽에 네 방향에 대한 입면도가 생성되어 있고, 이제 저 사각형 범위 안에 모델을 만들어주면 된다.</p>
<h3 id="r-1-모형-생성하기">R-1. 모형 생성하기</h3>
<p>우선 <code>구조&gt;구성요소&gt;내부편집 모델링</code>을 누르면 나오는 창에서 필터 리스트는 구조와 바닥으로 선택하여 이 요소가 페밀리 구성 요소 중 어디에 속하도록 할지 정한다.
그리고 <code>돌출</code> 선택 후 직사각형을 화면에 그리고 원하는 깊이를 작성 후 ✅를 누르면 직육면체가 만들어진다. 아래 화면은 3D 뷰로 확인할 수 있다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/e89d3991-5f44-4d8b-ad7e-647dc1cd0605/image.png" /></p>
<h3 id="r-2-level-생성하기">R-2. Level 생성하기</h3>
<p>Tandem의 Filter 기능을 보면, Level(층)별로 요소들을 확인할 수 있다. 이를 위한 작업이다.
우선 입면도를 연다. 그리고 건축&gt;레벨을 눌러 원하는 위치에 레벨을 생성하고, 요소들을 레벨 기준으로 원하는 위치에 배치한다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/ddf2801f-a5ed-44dd-bf93-f903e4867da0/image.png" /></p>
<h3 id="r-3-모형에-질감재료-넣기">R-3. 모형에 질감(재료) 넣기</h3>
<p>재료를 만들기 전, 우리가 만든 재료를 볼 수 있는 환경을 마련해야 한다.
<code>뷰&gt;사용자 인터페이스&gt;특성</code>을 체크하여 왼쪽에 특성창을 연다.
특성창에서 <code>그래픽&gt;그래픽 화면표시 옵션 편집</code>에서 <code>사실적</code> 스타일을 적용해야 우리가 만든 재료가 입혀진 모습을 볼 수 있다.</p>
<p>이제 원하는 요소에 재료를 만들어준다.
원하는 모델을 더블클릭 후 다시 클릭하면 옆에 구속조건, 그래픽, 재료 등등이 뜬다. 여기서 재료 오른쪽에 ...을 눌러 재료 탐색기를 연다.
왼쪽 아래에 재료 작성 버튼&gt;새 재료 작성을 누른다.
<code>아이덴티티</code>에서 이름과 설명, 클래스를 선택한다. ex) 이름: base_surface, 설명: 일반 플라스틱, 클래스: 플라스틱
<code>모양</code>에서 오른쪽 위에 자산 대체 버튼을 눌러 원하는 자산을 선택한다. ex) Paek(베이지 색) 불투명 플라스틱
그리고 색상, 반사율, 거칠기 등등 원하는 재료 특성을 설정한다.
<code>그래픽</code>&gt;음영처리에서 렌더 모양 사용을 꼭 체크한다. (이걸 해야 Tandem에 모델을 올렸을 때 재료가 반영된다.)
<img alt="" src="https://velog.velcdn.com/images/yena121/post/9dd99871-1c15-43d4-a8f5-7ed711d5b394/image.png" /> 재료가 적용된 모습이다. <img alt="" src="https://velog.velcdn.com/images/yena121/post/0aeb4592-3388-45f9-b022-ae533abf9269/image.png" /></p>
<h3 id="r-4-parameter-생성하기">R-4. Parameter 생성하기</h3>
<p>LED의 상태(ON/OFF)에 따라 LED 색상의 밝기 차이를 시각적으로 다르게 표현하기를 원한다.
이를 위해 매개변수(Parameter)를 생성하여 이 매개변수 값에 따라 재료를 다르게 적용할 수 있도록 해야한다. 즉 실제 센서값과 연동될 변수와 같은 개념이다.
<code>관리&gt;프로젝트 매개변수</code>을 누르고 왼쪽 아래에 새 매개변수 버튼을 눌러 매개변수 특성창을 연다.
매개변수 데이터에서 이름(예: led1_status), 분야(예: 공통), 데이터 유형(예: 예/아니요), 그룹 매개변수(예: 전기)를 설정하고, 꼭 인스턴스를 체크해준다. (이걸 해야 Tandem Parameter와 맵핑하기 위한 목록에 뜬다.)
<img alt="" src="https://velog.velcdn.com/images/yena121/post/0871c39d-c258-4d1a-81d8-700cd67e5cd6/image.png" /> 필요한 변수들을 모두 생성해준다.</p>
<p><code>뷰&gt;필터</code>를 통해 매개변수 값에 따라 적용될 그래픽을 설정할 수 있다는데 이 설명은 추후에 업데이트 해보겠다.</p>
<h3 id="r-5-요소-분류-코드-생성하기">R-5. 요소 분류 코드 생성하기</h3>
<p>(이 단계는 왜 하는지 모르겠다. 일단 하자)
(이 단계는 아래에 Tandem Classification 생성을 위한 Excel 파일을 정의 후 그에 맞추어 Revit으로 돌아와 설정을 추가하면 된다. T-1단계 이후에 진행하자.)
해당 요소를 눌러 나오는 특성창에서 유형 편집 버튼을 누르면 유형 특성 창이 뜬다. 여기에서 유형 마크(Type Mark)에 Excel 파일에서 Code 필드의 값을 입력해준다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/f70df95b-4bd0-421c-a86a-db066d020ba9/image.png" /></p>
<p>이렇게 Revit 모델을 생성했다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/0ccaa222-d2fc-434b-8b99-8714476e8dde/image.png" /></p>
<p>이제부터 이 모델(.rvt)을 갖고 다룰 Tandem에 대한 설명이다.</p>
<blockquote>
<p>세 번째, Tandem에 Revit 모델 적용하기 (25년도 Tandem 기준)</p>
</blockquote>
<h3 id="t-1-facility-templates-생성하기">T-1. Facility Templates 생성하기</h3>
<p>우선, Facility Templates을 만들기 전에 Classification과 Parameter을 정의해야 한다.</p>
<p><code>Manage&gt;Classes</code>에서 Download 버튼을 눌러 Classification을 생성하기 위한 <code>Excel</code> 템플릿을 다운로드 받는다. 이 템플릿에 고유 식별 코드와 계층 구조 및 계층 깊이를 작성한다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/ba638e0e-7f81-4731-984f-d3299d709170/image.png" /></p>
<p>Add 버튼을 눌러 이 템플릿 파일을 업로드하면 Tandem Classification에 적용된다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/d1d8c82e-a46d-469c-a225-6bf60e8f2593/image.png" /></p>
<p>이제 <code>Manage&gt;Parameters</code>에서 사용할 변수에 대해 정의한다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/0876bcb4-0e17-4ba2-a710-ed8cabdbd134/image.png" /></p>
<p>이제 <code>Manage&gt;Facility Templates</code>에서 Add 버튼을 눌러 위에서 생성한 Parameter와 Classification으로 Facility Templates을 만든다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/f2fe9981-6344-43df-b85a-77d2a17d4aa2/image.png" /></p>
<h3 id="t-2-facility-template와-revit-모델-정보-맵핑하기">T-2. Facility Template와 Revit 모델 정보 맵핑하기</h3>
<p>Facility를 생성해주고 Revit 모델(.rvt)를 업로드한다. 그럼 이런 화면이 뜬다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/d703f5e3-4058-41a0-b293-0ff11fd2efbe/image.png" /></p>
<p>Assets에서 Facility Template의 Parameter와 Revit의 Parameter을 맵핑해준다.
Revit에서 설정한 이름들이 Tandem에서는 다르게 사용될 수 있기 때문에 이 둘을 맵핑해주는 것이다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/2007197d-b270-4252-9f1d-dd4561c39758/image.png" /></p>
<h3 id="t-3-속성-설정하기">T-3. 속성 설정하기</h3>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/b5a229a3-65ba-422a-ab27-510e4181e6a5/image.png" /> 모델을 누르면 오른쪽에 나타나는 속성(Properties)창에서 속성값들을 설정한다.
설정한 속성값들을 기반으로 Filters 기능을 사용할 수 있다.
다음은 level 0, level 1로 필터링된 모습이다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/47f5afdc-160d-4f80-8882-9fabafc1ccaa/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/40297b7e-abba-4a1d-8070-3b39757c5679/image.png" /></p>
<h3 id="t-4-connections-생성하기">T-4. Connections 생성하기</h3>
<p>Connections에서 Create 창에서 Connection 이름(예: led1_status_connection)과 해당 Classification을 입력한다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/ff58da72-6877-4b7e-9384-25c4543afef7/image.png" /></p>
<p>이제 생성된 Connection에 요소(Host라고 부르더라)를 할당해준다.
Connections을 누르면 나오는 오른쪽 창에서 relationships&gt;assign to host을 누르고 해당 요소를 클릭 후 가운데 아래에 assign host을 누르면 
<img alt="" src="https://velog.velcdn.com/images/yena121/post/23c08cf3-3e4a-45f9-b2aa-b16f8425cb66/image.png" />
이렇게 요소에 작은 원이 추가된다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/7dbd6a54-210d-45b1-b0b3-09fb4014440d/image.png" /></p>
<p>※ 아래에 no connected parameters가 뜬다. 
connection(예: led1_status_connection)과 parameter(예: Status1)을 연결해야 하는데 잘 안된다... 원인을 찾아 수정해보겠다.</p>
<p>이렇게 Connection들을 생성하여 실제 센서값을 받아올 연결망을 만들어준다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/8c2eb3fb-27ff-4def-bb9e-a9ab97277822/image.png" /></p>
<blockquote>
<p>마치며</p>
</blockquote>
<p>여기까지가 실제 센서와 연결하기 전에 해야 하는 모든 과정이다.
아예 모르는 상태에서 아래 사진처럼 GPT와 씨름하면서 하나하나 알아낸 내용이라, 미처 몰라서 설정하지 못한 부분들도 분명 있을 거다.
앞으로 이 게시물을 업데이트하면서 빈 부분들을 조금씩 채워나갈 예정이다.
아래에 버전별로 수정 로그를 남길 테니, 뭐가 달라졌는지 참고하면 좋을 것 같다. 
<img alt="" src="https://velog.velcdn.com/images/yena121/post/995c35c7-04b4-419a-9b31-e505001e5887/image.png" /></p>
<blockquote>
<p>Version History</p>
</blockquote>
<p><strong>2025.04.26</strong></p>
<ul>
<li>최초 작성</li>
<li>개선 예정<ul>
<li>Revit에서 Parameter에 따라 재료 다르게 적용하는 기능 설명 추가</li>
<li>Type Mark 역할 설명 추가</li>
<li>Connections 항목에서 Parameter 연결이 미흡함</li>
</ul>
</li>
</ul>