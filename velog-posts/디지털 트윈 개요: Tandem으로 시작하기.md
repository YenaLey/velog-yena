<blockquote>
<p>요즘 뭘 하고 있는지</p>
</blockquote>
<p>이번 학기에는 프론트엔드 개발을 잠시 접고 국민대에서 주최하는 다학제 캡스톤 디자인 삼성 트랙에 집중해보려 한다.
앞으로 관심있게 볼 주제는 <code>디지털트윈</code>이다.</p>
<blockquote>
<p>디지털트윈(DT)이란?</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/bd7a2976-ad89-4ea3-8d7c-004b05a739ae/image.png" /> DT의 핵심은 연결성(connectivity)으로, IoT 센서 등을 통해 실제 자산의 상태 변화가 실시간으로 디지털 모델에 동기화되며 모니터링·시뮬레이션·예측이 가능하다.
6G 시대의 초저지연·초고속·초연결 통신은 디지털 트윈의 실시간성과 공간 확장을 가능하게 해, 스마트 시티, 자율주행, 메타버스형 산업관리에서 핵심 인프라로 자리잡게 될 것이다.</p>
<p>쉽게 정리하면, DT는 공간이나 기기의 구조와 데이터를 그대로 복제한 가상 모델이다. 현실과 가상이 실시간으로 연결되어, 가상 모델에서 모니터링과 시뮬레이션이 가능하다. 즉 실제 기기를 작동하지 않아도 결과를 예측할 수 있다는 것이다.</p>
<blockquote>
<p>Autodesk Tandem은 뭘까?</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/8acde267-4754-40ea-9f15-3ba9dce25108/image.png" /> Autodesk Tandem은 디지털트윈을 구현하기 위한 툴이다.</p>
<p>디지털 트윈 구축의 핵심 워크플로우에 대해 정리하면,</p>
<ol>
<li>모델링 툴(Revit, Blender 등)에서 건물, 공간, 장비의 3D 구조를 설계하고 IFC 파일로 내보냄</li>
<li>내보낸 IFC를 Autodesk Tandem에 불러오고 센서, 시스템 데이터 등 외부 실시간 데이터 연결하여 가상모델에 반영함</li>
</ol>
<p>모델링 도구는 외형만 만든다.
Tandem에서 외형에 데이터를 연결한다. 모델 요소마다 속성을 추가하고 실시간 데이터 값을 맵핑하는 것이다.</p>
<p>Tandem UI</p>
<ul>
<li>각 모델 요소에 <code>temperature</code>, <code>occupancy</code> 등 속성과 속성 타입을 정의</li>
<li>모델에 색상이나 라벨 등을 표현 (예: 온도 값에 따라 벽 색상이 바뀌도록 설정)</li>
</ul>
<p>Tandem API</p>
<ul>
<li>실시간 속성값을 업데이트</li>
<li>값만 보내는 파이프라인 역할</li>
</ul>
<p>다시 정리하면,
Tandem으로는 현실(센서 데이터)을 받아와서 가상 모델에 시각화하는 것을 구현한다.</p>
<p>가상 모델의 조작을 통해 현실을 제어하려면, 이를 중계할 미들웨어 시스템이 필요하다.</p>
<p>미들웨어는 사용자 컨트롤 대시보드(UI)와 Tandem 및 IoT 장치 간의 연결을 담당하는 서버로 구성된다.</p>
<p>예를 들어, 미들웨어의 UI를 React로 구현한다고 가정하면,
Tandem의 공식 Web Viewer SDK 또는 iframe을 이용해 가상 모델을 웹 페이지 내에 임베딩할 수 있다.</p>
<p>사용자는 모델 요소를 클릭하거나, 속성 패널(GUI)을 통해 특정 속성 값을 수정하고 변경사항을 미들웨어를 통해 두 방향으로 전파된다.</p>
<ul>
<li>Tandem API를 통해 가상 모델의 속성 값 업데이트</li>
<li>IoT API 또는 MQTT를 통해 현실 장치에 제어 명령 전송</li>
</ul>
<p>이렇게 미들웨어는 Tandem과 현실 사이의 중계 허브로, 가상 모델과 실제 기기의 상태를 양방향으로 동기화한다.</p>
<p>디지털 트윈 컨트롤 시스템 구축 4단계를 정리하면 다음과 같다.</p>
<p>1️⃣ 현실 모델의 3D 가상화 (모델링 단계)</p>
<ul>
<li>Revit, Blender + BlenderBIM, Archicad 등</li>
<li>IFC 파일이 결과물
<img alt="" src="https://velog.velcdn.com/images/yena121/post/140d3233-8c14-4c24-b1b6-102e4a6ccdc3/image.png" /></li>
</ul>
<p>2️⃣ 디지털 트윈 구성 및 속성 매핑 (Tandem 단계)</p>
<ul>
<li>Autodesk Tandem에 IFC 파일 업로드</li>
<li>각 모델 요소에 속성을 정의한 뒤 외부 실시간 데이터와 맵핑 (센서값, 장비 상태 등)
<img alt="" src="https://velog.velcdn.com/images/yena121/post/9abf0bb5-47d6-4f66-9e16-b55a163dce1c/image.png" /></li>
</ul>
<p>3️⃣ 현실 장치 제어 및 센싱 로직 (펌웨어 구축)</p>
<ul>
<li>ESP32, Raspberry Pi, Arduino 등 + C/Python 기반 펌웨어</li>
<li>미들웨어의 제어 명령을 수신하여 기기를 작동</li>
<li>센서를 통해 실시간 상태 값을 측정하고 API를 통해 Tandem 또는 미들웨어로 상태 보고</li>
</ul>
<p>4️⃣ 양방향 동기화 및 제어 로직 (미들웨어 구축)</p>
<ul>
<li>React (UI), Express/Flask (서버), HTTP or MQTT</li>
<li>사용자 UI에서 Tandem API를 통해 가상 모델의 속성값을 변경하고, IoT 기기의 상태를 현실에 반영하도록 명령 전송</li>
</ul>
<blockquote>
<p>그래서 앞으로 뭘할까?</p>
</blockquote>
<p>Autodesk Tandem의 기능 및 API를 정리할거다.
참고할 자료는 다음과 같다.</p>
<p>Tandem 튜토리얼 동영상
<a href="https://www.youtube.com/playlist?list=PLcuPmjjwqWK_bzm63nyR811wN_7UMqznS">https://www.youtube.com/playlist?list=PLcuPmjjwqWK_bzm63nyR811wN_7UMqznS</a></p>
<p>Tandem API 가이드
<a href="https://aps.autodesk.com/en/docs/tandem/v1/developers_guide/basics/">https://aps.autodesk.com/en/docs/tandem/v1/developers_guide/basics/</a></p>
<p>Tandem API Postman
<a href="https://documenter.getpostman.com/view/15787353/2s9YXk2faD">https://documenter.getpostman.com/view/15787353/2s9YXk2faD</a></p>
<p>Tandem API 예제 (Python)
<a href="https://github.com/autodesk-tandem/tutorial-rest-python">https://github.com/autodesk-tandem/tutorial-rest-python</a></p>
<p>디지털 트윈은 앞으로 더욱 주목받을 핵심 기술이지만, 이를 구현할 수 있는 대표 툴인 Autodesk Tandem에 대한 자료는 아직 매우 부족하다.
앞으로 Tandem을 공부하는 사람들에게 실질적인 참고서가 될 수 있는 기록을 남기는 것이 목표이다.</p>