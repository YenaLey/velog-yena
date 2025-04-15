<blockquote>
<p>본격적인 기능 소개에 앞서, Tandem이 어떤 구조와 흐름으로 동작하는지 간단히 살펴보자.</p>
</blockquote>
<p>Autodesk Tandem은 BIM 모델을 기반으로 건물의 자산 정보를 정리하고, 센서 데이터를 실시간으로 연결해 시각화하며 관리할 수 있는 디지털 트윈 플랫폼이다. 단순히 3D 모델을 보는 걸 넘어, 공간 안에서 일어나는 다양한 정보를 한눈에 파악하고 활용할 수 있게 해준다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/768d453a-89f7-41c0-86b7-ded86a393ca5/image.png" /></p>
<p>처음 시작은 Facility를 만드는 것부터다. 여기에는 Revit이나 IFC 모델을 업로드하게 되는데, 이때 단순히 모델만 올린다고 끝이 아니다. 모델 안에 들어 있는 벽, 창, 기기 같은 요소들이 Tandem에서는 어떤 자산으로 분류되고, 어떤 정보를 가져야 할지 기준이 필요하다. 그걸 정해주는 게 바로 <code>Facility Template</code>이다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/087216dc-4cac-430c-a029-53e50fe852f0/image.png" /></p>
<p>이 템플릿에는 자산을 어떻게 분류할지, 어떤 속성들이 필요한지가 정리되어 있다. 모델을 업로드한 다음엔, 이 템플릿과 모델 사이를 연결해주는 매핑 작업을 하게 된다. 예를 들어 Revit 안의 'Assembly Code' 같은 속성을 Facility Template 안의 분류 기준과 연결하는 식이다. 이 과정을 통해 모델이 디지털 트윈에 맞는 구조로 정리된다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/2df32d6c-ac42-42ef-afec-af60337c485e/image.png" /></p>
<p>정리된 자산 정보는 <code>Inventory</code> 탭에서 표처럼 쭉 확인할 수 있다. 각 자산이 어떤 분류에 속하는지, 속성 값은 어떻게 되는지, 연결된 시스템은 있는지 등을 확인할 수 있고, 엑셀로 내보내거나 다시 불러와 수정하는 것도 가능하다. 필터 기능을 통해 원하는 정보만 골라볼 수도 있다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/db4a005b-f888-4b01-a4e4-eb4e26ca67a1/image.png" /></p>
<p>이제 데이터를 연결해볼 차례다. 실제 센서 데이터를 자산에 연결하려면 <code>Connections</code> 기능을 쓴다. 하나의 센서는 Tandem에서 하나의 <code>Stream</code>으로 연결되는데, 각 센서는 Tandem에서 발급한 <code>HTTP</code> 주소로 데이터를 전송하게 된다. 이렇게 들어온 값은 자동으로 해당 자산의 속성 패널에 반영된다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/efae1786-a4f4-4d0b-9d09-dec00b2d85b4/image.png" />
이 URL은 특정 자산의 스트림을 나타내는 엔드포인트다.
이 주소로 POST 요청을 보내면 센서 데이터가 Tandem에 반영되고, 이후 <code>Streams</code> 탭에서 해당 데이터의 시계열 차트를 확인할 수 있으며, <code>Inventory</code>나 <code>Properties</code> 패널에서도 실시간으로 값이 업데이트되는 것을 볼 수 있다.</p>
<pre><code>https://:iWekUloESOyr_kuXjiCSOQ@tandem.autodesk.com/api/v1/timeseries/models/urn:adsk.dtm:vtZCG1yuTo-Vic2dRvv1Jg/streams/AQAAAMQhOtfHY0s3pHCXTvgx8g4AAAAA</code></pre><p>들어온 데이터는 <code>Streams</code> 탭에서 시간 흐름에 따라 차트로 확인할 수 있다. 임계값(<code>Threshold</code>)을 설정하면 기준을 넘긴 값은 색상으로 강조돼서 쉽게 식별할 수 있다. 데이터는 표 형식으로도 확인할 수 있어 엑셀처럼 다루는 것도 가능하다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/160923b0-4912-41ea-b6a3-b03b2c667d64/image.png" /></p>
<p>이 데이터를 공간이나 시스템에 색상으로 시각화하고 싶다면 <code>Heatmap</code> 기능을 사용하면 된다. 예를 들어 온도가 높은 곳은 빨간색, 낮은 곳은 파란색으로 표시되도록 설정할 수 있다. 색상 범위, 투명도, 테마 등은 원하는 대로 조정 가능하며 실시간으로 반영된다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/2c566437-39bc-4814-89f5-ca8f58b5da44/image.png" /></p>
<p>조금 더 전체적인 상태를 확인하고 싶을 땐 <code>Dashboard</code>를 활용하면 된다. 속성이 얼마나 채워졌는지, 분류는 얼마나 완료됐는지 등을 카드 형태로 보여주고, 클릭하면 관련 자산을 바로 <code>Inventory</code>에서 확인할 수 있어 흐름도 자연스럽다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/97162409-69bc-499c-a0d9-a6ecba0e0aaf/image.png" /></p>
<p>이 외에도 자주 쓰는 필터나 뷰 상태를 저장하는 <code>View</code> 기능, 자산 간 시스템 구조를 구성하는 <code>System</code> 패널, 센서 간 관계를 추적할 수 있는 <code>Connection Tracing</code>, 작업 히스토리 확인, 사용자 권한 설정 등 실무에 필요한 기능들도 함께 갖춰져 있다.</p>
<p>Tandem은 단순한 뷰어가 아니라, BIM과 IoT 데이터를 함께 다루며 건물을 디지털 기반으로 통합 관리할 수 있게 해주는 툴이다.</p>
<p><a href="https://velog.io/@yena121/Autodesk-Tandem-%EA%B8%B0%EB%8A%A5%EB%B3%84-%EC%A0%95%EB%A6%AC-Full-Guide">다음 블로그</a>는 <a href="https://help.autodesk.com/view/TANDEM/ENU/?guid=tandem-facilities-overview">Autodesk Tandem 공식 가이드</a>를 참고해 기능별로 정리한 글이다. 공식 문서를 보다 쉽게 풀어쓴 내용으로, Tandem을 사용하면서 기능이 궁금할 때 언제든지 꺼내 볼 수 있는 참고서가 되었으면 한다.</p>