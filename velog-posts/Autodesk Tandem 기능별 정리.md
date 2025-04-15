<blockquote>
<p>Tandem 기능을 소개하기 전에 간단히 배경을 설명하면</p>
</blockquote>
<p>Autodesk Tandem은 BIM(Building Information Modeling) 모델을 <code>층(Level)</code>, <code>공간(Space)</code>, <code>요소(Element)</code>라는 계층적 구조로 구성하며, 각 요소에는 다양한 <code>속성(Properties)</code>이 연결된다.</p>
<p>사용자는 모델 내 <code>자산(Asset)</code>들을 선택하고 <code>필터(Filter)</code>, 3D 뷰어(Viewer), 속성 패널(Properties Panel)을 통해 원하는 데이터를 직관적으로 탐색할 수 있다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/f6ad61ef-ecc5-4001-af56-2883ae1255f2/image.png" /></p>
<p>요소는 벽, 창, 장비, 공간 등 모델을 구성하는 각각의 부품이며, 기본 속성 외에도 사용자 정의 속성(Custom Properties)을 추가할 수 있어 에너지 해석 결과나 센서 데이터를 해당 요소에 직접 연결할 수 있다.</p>
<p>사용자는 여러 자산을 묶어 <code>시스템(System)</code>을 구성할 수 있고, 각 자산이 속한 공간이나 연결된 시스템 등 자산 간의 <code>관계(Relationships)</code>를 정의할 수 있다. 또한 각 자산에는 도면, 사양서, 유지보수 매뉴얼 등의 <code>문서(Docs)</code>를 연결하여 실무적인 정보 관리를 지원한다.</p>
<p><code>Connections</code> 기능을 통해 IoT 센서나 장비를 자산에 연결할 수 있으며, 이로부터 수집된 데이터는 <code>스트림(Streams)</code>으로 저장되어 시간별 차트, 히트맵 등의 형태로 시각화된다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/f002a6f6-ceaa-4c28-a9f6-33f28f47faa8/image.png" /></p>
<p>모든 자산의 상태와 속성, 연결 여부 등을 <code>인벤토리(Inventory)</code>에서 표 형식으로 한눈에 확인하고 관리할 수 있다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/d06b57dc-54d2-4223-926a-71f5a23e1588/image.png" /></p>
<blockquote>
<p>Autodesk Tandem 공식 가이드를 참고해서, 한글로 쉽게 정리해봤다.
<a href="https://help.autodesk.com/view/TANDEM/ENU/?guid=tandem-facilities-overview">Autodesk Tandem 공식 가이드</a></p>
</blockquote>
<h3 id="✅-specify-classifications-parameters-facility-templates">✅ Specify: Classifications, Parameters, Facility Templates</h3>
<p>자산 데이터를 구조화하고 관리하기 위해 3단계 구성으로 정의할 수 있다: 
<code>Classifications</code>, <code>Parameters</code>, 그리고 <code>Facility Templates</code></p>
<p><strong>☑️ Classifications (자산 분류 체계)</strong>
Classifications는 자산의 유형과 그룹을 정의한다.</p>
<ul>
<li><code>Manage &gt; Classes</code> 탭으로 이동</li>
<li>사전에 정의된 classification system를 선택하거나, 커스텀 classification system를 업로드할 수 있음</li>
<li>(built-in) 이 표시된 항목은 기본 내장 시스템임
<img alt="" src="https://velog.velcdn.com/images/yena121/post/373ae4ae-9565-48a2-9a8f-eff7a8ed293f/image.png" /></li>
</ul>
<p>custom classification system 추가 방법</p>
<ol>
<li>드롭다운에서 분류 체계 선택 후 <code>Download</code> 클릭 → Excel 템플릿 생성</li>
<li>Excel에서 Code, Description, Level 항목 작성 → 저장</li>
<li>Tandem으로 돌아와 <code>Add Classification</code> 클릭</li>
<li>이름 입력 후 Excel 파일 업로드
<img alt="" src="https://velog.velcdn.com/images/yena121/post/70315424-0aab-446f-b0b0-b4bf37ea606a/image.png" /></li>
</ol>
<p>classification system를 드롭다운에서 선택해 검토, 수정, 삭제 가능
업데이트 시 최신 Excel 파일을 드래그 앤 드롭하여 반영</p>
<p><strong>☑️ Parameters (자산 속성 정의)</strong>
Parameters는 속성 정보를 정의한다.</p>
<ul>
<li><code>Manage &gt; Parameters</code> 탭으로 이동</li>
<li>오른쪽 상단 토글로 기본 <code>Library</code> on/off 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/660b86ed-100b-40a2-b4de-d19636ca7eb5/image.png" /></li>
</ul>
<p>custom parameter 생성 방법</p>
<ol>
<li><code>Add Parameter</code> 클릭 → 이름, 데이터 타입, 단위, 적용 레벨(Context: Element or Type)을 입력 (현재는 Element, Type 레벨만 지정 가능 (Space, Facility는 추후 예정))
<img alt="" src="https://velog.velcdn.com/images/yena121/post/8be209bf-a256-48af-a7c1-b002f063f701/image.png" /></li>
</ol>
<p><strong>☑️ Facility Templates (시설 템플릿)</strong>
Facility Template은 Classification와 Parameter를 조합하여 특정 시설 유형에 적합한 자산 정보 구조를 설정하는 기능이다.</p>
<ul>
<li><code>Manage &gt; Facility Templates</code> 탭으로 이동</li>
<li>기본 템플릿은 잠금(🔒) 아이콘이 표시되며 삭제 불가</li>
<li><code>Add Facility Template</code> 클릭 → 이름, 설명, 분류 체계 선택 후 생성
<img alt="" src="https://velog.velcdn.com/images/yena121/post/48578c2c-2175-4338-a7cd-a6ba94ae6916/image.png" /></li>
</ul>
<p>Parameter 할당 방법</p>
<ul>
<li>왼쪽 트리에서 Class 선택 → <code>Assign Parameters</code> 클릭</li>
<li>Standard 탭: 기본 매개변수 / Custom 탭: 사용자 정의 매개변수 검색 및 선택</li>
<li>상위 클래스에 적용 시 하위 코드에 자동 상속됨
<img alt="" src="https://velog.velcdn.com/images/yena121/post/a760d4ed-54fc-4be5-a39c-09e92cc48f66/image.png" /></li>
</ul>
<p>Facility Template 적용</p>
<ul>
<li>facility 열기 → <code>Assets</code> 탭 → <code>+Apply Template</code> 클릭하여 원하는 템플릿 선택
<img alt="" src="https://velog.velcdn.com/images/yena121/post/73b5ccf1-b74d-410c-a285-d42651f48874/image.png" /></li>
</ul>
<p>Classifications, Parameters, Templates 모두 버전 관리되어 수정/업데이트 가능하고, 프로젝트 변경 사항에 따라 유연하게 반영 가능</p>
<hr />
<h3 id="✅-filters">✅ Filters</h3>
<p>모델에서 필요한 요소만 골라 볼 수 있는 기능이다.
필터 항목에 마우스를 올리면 해당 요소가 뷰어에서 노란색으로 하이라이트된다.
처음 모델을 불러오면 기본 속성들로 필터 패널이 자동 설정된다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/6c72b4e4-7b52-44b7-bbab-f580905a6730/image.png" /></p>
<p><strong>☑️ 커스텀 Parameter</strong></p>
<ul>
<li>필터 패널 우측 상단 ⚙️ 클릭하여 Edit Filters 열기</li>
<li>3가지 유형 중 선택
Standard Parameters: 시스템에서 기본 제공
Asset Parameters: 사용자가 템플릿에 정의한 속성
Design Parameters: 모델 원본에서 가져온 속성</li>
<li>Search Properties로 검색 후 + 아이콘으로 추가</li>
<li>매개변수 순서는 드래그로 변경 가능</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/3cacea7c-55c5-4f51-b9cf-cbc737f1ddbf/image.png" /></p>
<p><strong>☑️ 필터 적용 방식</strong>
필터는 위에서 아래로 순차적으로 적용되기 때문에 조건이 쌓일수록 점점 더 세분화된 결과가 나타난다.
예: 1단계 Turnover Date 필터 → 2단계 특정 Space로 제한
공간 필터를 사용하려면 Edit Filters에서 Spaces 항목을 활성화해야 함
<img alt="" src="https://velog.velcdn.com/images/yena121/post/68ada319-3a6e-4ab2-8363-4d812835c908/image.png" /></p>
<p><strong>☑️ 색상 커스터마이징</strong>
매개변수마다 색상을 다르게 지정할 수 있다.
매개변수 오른쪽의 색상 원(circle)을 클릭하면 Edit Colors 창이 열리며, 원하는 색상을 설정할 수 있다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/a88d9166-bbd6-4b03-bb86-666f2c0ccc53/image.png" /></p>
<p><strong>☑️ 필터 초기화</strong>
전체 필터를 제거하고 싶을 때는 우측 상단 Revert 아이콘을 클릭하면 된다.</p>
<hr />
<h3 id="✅-assets">✅ Assets</h3>
<p>Assets은 Facility Template을 통해 정의된다.<br />Assets 패널을 통해 템플릿을 적용할 수 있으며, 템플릿이 업데이트되었을 경우 Update 버튼을 클릭해 최신 버전으로 반영할 수 있다.</p>
<p><strong>☑️ Classification Mapping</strong><br />Revit 모델 안의 자산들이 어떤 분류에 속하는지를 자동으로 인식하게 해주는 과정이다.<br />즉, Facility Template에 정의된 분류 체계(Classification System)에 맞춰,<br />Revit 모델 안에 담긴 자산 분류 정보를 연결(Mapping)하는 것이다.</p>
<ul>
<li>Assets 탭에서 Facility Template이 적용되면, 해당 템플릿 안에 정의된 Classification System(예: Real Estate Core)이 자동으로 표시된다.</li>
<li>아래로 스크롤하면 <code>Classification (Not mapped)</code> 항목이 나타나고, 여기서 Revit 모델에 정의된 자산 분류 정보를 담고 있는 속성을 직접 선택해야 한다.</li>
<li>보통 <code>Assembly Code</code>, <code>OmniClass Number</code> 등의 속성이 자산 분류 코드를 포함하고 있으며, 드롭다운에서 검색해 선택할 수 있다.</li>
<li>해당 속성을 선택하면, Revit 자산에 들어 있는 코드값이 템플릿의 분류 항목과 일치하는 경우 자동으로 분류가 적용된다.
예: Assembly Code에 &quot;D5020&quot;이 입력되어 있고, Template에도 &quot;D5020&quot;이 정의되어 있다면, 해당 자산은 자동으로 매칭되어 분류된다.</li>
<li>이 기능은 수작업 없이 자산 분류 작업을 자동화하는 데 매우 유용하다.</li>
</ul>
<p>※ 단, Revit 속성값과 템플릿의 분류 코드가 정확히 일치해야 자동 분류가 정상 작동한다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/d1259f70-10d5-4641-8560-d2f6c11ca8a9/image.png" /></p>
<p><strong>☑️ Parameter Mapping</strong><br />Revit 자산의 속성(Parameter)을 Tandem의 속성과 연결하는 작업이다.</p>
<ul>
<li>Classification 항목 아래에는 Tandem 템플릿에 정의된 여러 속성(Parameter) 목록이 매개변수 카테고리별로 정리되어 있다.</li>
<li>각 속성 오른쪽 드롭다운을 클릭하면, Revit 모델에 있는 속성 목록이 나타난다.</li>
<li>예를 들어, Tandem의 Asset Tag 항목에 Revit의 Mark 값을 연결해서 Revit에서 입력된 값이 자동으로 채워진다.</li>
<li>검색창을 활용하면 원하는 속성을 쉽게 찾을 수 있으며, 매핑 후 Tandem에서도 자산 정보가 잘 정리되어 표시된다.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/43437099-443e-4f57-b544-3d5218157394/image.png" /></p>
<hr />
<h3 id="✅-files">✅ Files</h3>
<p><strong>☑️ 모델 추가 방법</strong>  </p>
<ul>
<li>열려 있는 Facility에서 좌측 메뉴의 Files 탭으로 이동  </li>
<li><code>+ Import Model</code> 클릭 → Import 방법 선택  <ul>
<li>첫 Import인 경우: <code>Add Models</code> 클릭  </li>
<li>그 외: <code>Import Model</code> 선택  </li>
</ul>
</li>
<li>Revit 또는 IFC 모델을 File Upload 또는 Autodesk Docs에서 불러올 수 있다  <ul>
<li>File Upload 시에는 모델 제목을 입력  </li>
<li>파일을 드래그 앤 드롭하거나 브라우저로 선택 가능</li>
</ul>
</li>
<li>Visibility 옵션을 설정할 수 있다<ul>
<li>시설을 열 때 모델이 자동으로 로드되지 않도록 하려면 <code>Visible in Default View</code> 옵션을 해제 </li>
<li>모델에 여러 Phase가 포함되어 있다면, 가져오기 전에 원하는 Phase를 선택(Phase: Revit에서 공정 단계별로 모델 요소를 나눈 구분 – 예: 기존 구조, 신축 부위 등)</li>
<li>Tandem은 한 번에 한 Phase만 로드 가능</li>
</ul>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/3b039ef5-4448-4e33-a932-91da34a26e2e/image.png" /></p>
<p><strong>☑️ Autodesk Docs에서 모델 가져오기</strong>  </p>
<ul>
<li>Autodesk ID로 연결된 <strong>Docs 계정에 로그인</strong>  </li>
<li>계정과 프로젝트 선택 → 파일 탐색 후 Import  </li>
<li>Phase가 있는 경우 원하는 Phase 선택 → <code>Import</code> 클릭  </li>
<li>모델 크기 및 디테일에 따라 처리 시간은 달라질 수 있으며, 처리 중에는 브라우저를 닫거나 다른 화면으로 이동해도 무방</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/7dab393c-549a-434f-ab75-7a7980ccca5b/image.png" /></p>
<p><strong>☑️ 가져온 모델 관리하기</strong>  </p>
<ul>
<li>Toggle Load States(👁️): 모델의 기본 로드 상태를 켜거나 끌 수 있음  </li>
<li>More Actions(⋮) 메뉴: 모델을 최신 버전으로 <code>Update</code> 또는 <code>Delete</code>로 삭제 가능</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/c342eb40-9580-41f8-a8b0-c41c5f56c33a/image.png" /></p>
<p><strong>☑️ Relink Models</strong>  </p>
<p><code>Autodesk Docs</code>에서 가져온 모델의 연결을 다른 Docs 경로로 변경하고 싶을 때, 모델 경로를 다시 지정할 수 있다.<br />(※ Revit의 Relink 기능과 유사하며, <code>Autodesk Docs</code>를 사용하는 경우에만 해당)</p>
<p>이 기능은 주로 Tandem Facility를 다른 계정으로 이전한 경우 또는 Docs 내 모델 위치가 변경된 경우에 사용된다.</p>
<ul>
<li>Files 탭 열기 → 가져온 모델 우측의 <code>⋮</code> 메뉴 클릭 → <code>Relink</code> 선택  </li>
<li>Autodesk Docs 계정에서 연결할 모델 선택<ul>
<li>이때 정확한 Autodesk Docs 계정과 프로젝트인지 반드시 확인  </li>
</ul>
</li>
<li>필요한 경우 Phase 또는 View 선택  </li>
<li><code>Relink</code> 버튼 클릭 → 모델 경로 재연결 완료</li>
</ul>
<p>이후 Autodesk Docs에서 해당 모델이 업데이트되면, Tandem에서도 연결된 최신 모델로 자동 반영된다.</p>
<p>※ <code>File Upload</code> 방식으로 직접 업로드한 모델은 Relink 대상이 아님
<img alt="" src="https://velog.velcdn.com/images/yena121/post/14ab6b72-b974-444b-8194-d06cd2f3288c/image.png" /></p>
<hr />
<h3 id="✅-docs">✅ Docs</h3>
<p>Asset에 PDF 문서를 첨부할 수 있다.</p>
<p><strong>☑️ 문서 추가하기</strong>  </p>
<ul>
<li>Tandem Facility에서 상단의 Docs 아이콘 클릭  </li>
<li>메뉴에서 <code>+ Add Documents</code> 선택  </li>
<li><em>Add Link</em> 대화창이 열리며, 프로젝트 내 문서 목록이 표시됨  <ul>
<li>문서는 직접 업로드하거나 Autodesk Docs에서 가져오기 가능  </li>
</ul>
</li>
<li>하나 이상의 문서를 선택하고 <code>Add</code> 클릭  </li>
<li>추가한 문서는 Docs 메뉴 인터페이스에서 확인 가능</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/10c59a24-f1a8-40cb-9f63-7423de2bb121/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/41b061a0-2230-4996-8204-4ab5dce20d10/image.png" /></p>
<p><strong>☑️ 문서를 Asset에 연결하기</strong><br />문서를 자산에 연결하려면 해당 자산이 Link Parameter가 포함된 Classification으로 분류되어 있어야 함</p>
<ul>
<li>연결할 문서를 추가한 후, 해당 자산으로 이동  </li>
<li>자산의 Properties 탭 열기  </li>
<li><code>Link</code> 매개변수 항목에서 Document 아이콘 클릭  </li>
<li><em>Add Link</em> 대화창에서 문서 선택 방법 3가지 중 선택  <ul>
<li>Facility Documents  </li>
<li>Autodesk Docs  </li>
<li>Hyperlink (외부 파일 저장소 URL 붙여넣기)</li>
</ul>
</li>
<li>잘못된 파일을 선택했다면, <code>빨간 X</code> 버튼 클릭으로 제거 가능</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/ea553432-e6ed-484d-aca5-787381783262/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/5ba99416-8153-4eb6-aa34-2ad5780d5fb8/image.png" /></p>
<p><strong>☑️ 첨부 문서 보기 및 관리</strong>  </p>
<ul>
<li>첨부된 PDF는 Tandem 내에서 미리보기 가능  </li>
<li>또는 Open Link 아이콘 클릭 시 새 탭에서 전체 보기 가능</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/f8558155-fd4b-4a7e-a048-ebf333ff40be/image.png" /></p>
<p><strong>☑️ 문서 버전 관리 및 삭제</strong>  </p>
<ul>
<li>Autodesk Docs에서 문서가 버전 관리되는 경우, Tandem은 변경 사항을 인식하지만 수동으로 업데이트를 트리거해야 적용됨 </li>
<li>Autodesk Docs에서 문서를 삭제하더라도, Tandem에는 가장 마지막 버전이 유지됨</li>
<li>문서가 더 이상 필요 없다면 문서 우측의 ⋮ 메뉴 클릭 → <code>Delete</code> 선택으로 삭제 가능</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/91922a95-98a4-44de-b362-42633edba9ff/image.png" /></p>
<hr />
<h3 id="✅-systems">✅ Systems</h3>
<p>자산이 건물 내 어디에, 어떻게 연결되어 있는지를 파악하고 시설 전체의 흐름과 연계를 이해하는 데 도움이 되는 기능이다.</p>
<p><strong>☑️ Creating Systems</strong>  </p>
<ul>
<li>필요한 Revit 모델을 모두 불러온 후 Filter 패널에서 System Classification 항목 활성화 </li>
<li>Systems 패널에서 <code>+ Create</code>을 눌러 시스템을 생성  <ul>
<li>Adjacency Tolerance 값으로 연결 간 거리 기준을 설정 (얼마나 가까워야 연결로 간주할지)</li>
</ul>
</li>
<li>System Elements 드롭다운은 필터 패널에서 선택한 항목이 나타나며 단일 선택만 가능 </li>
<li>오른쪽 상단의 ⚙️으로 세부 설정 조정 가능</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/3e05320a-d8f2-43ec-8252-c1c4c116c24c/image.png" /></p>
<p><em>Quick Actions</em><br />시스템 내 자산을 선택하면 아래 작업을 수행할 수 있다:</p>
<ul>
<li>Show nearby elements: 선택한 자산 근처의 다른 요소들을 표시  </li>
<li>Connect element: 요소를 시스템에 추가  </li>
<li>Disconnect element: 요소를 시스템에서 제거  </li>
</ul>
<p>자산을 우클릭하면 위 옵션이 포함된 컨텍스트 메뉴가 나타난다
<img alt="" src="https://velog.velcdn.com/images/yena121/post/a5202bab-873b-420b-af9d-4a98092f1630/image.png" /></p>
<p><em>Connection Types</em><br />요소 간 연결이 확실하지 않을 경우 <code>Connection types</code> 메뉴에서 연결 유형 선택/해제 가능  </p>
<ul>
<li><p>선택된 유형은 색상으로 구분됨  </p>
</li>
<li><p>녹색 선으로 인접한 요소들과의 연결 표시됨</p>
</li>
<li><p>시스템이 정제되고 연결되면 해당 자산의 Properties 패널 → <em>Relationships</em> 항목에서 확인 가능  </p>
</li>
<li><p>시스템명을 클릭하면 Systems 패널이 열리며 뷰어에 시스템이 표시됨</p>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/a4c45494-a0d1-4dff-91f6-0667de16250f/image.png" /></p>
<p><strong>☑️ System Tracing</strong><br />연결된 주요 요소들을 따라가는 시각적 경로 표시 기능이다.<br />경로가 하이라이트되고, 이름이 라벨로 표시되며, 색상으로 가지(branch)를 구분할 수 있다.</p>
<ul>
<li>자산 선택 후 우클릭 → <code>System Tracing</code> 선택  </li>
<li>뷰 좌측 상단에 System Trace Toolbar가 나타남  <ul>
<li>시작 요소는 선택한 자산  </li>
<li>종료 지점은 드롭다운으로 선택  </li>
</ul>
</li>
<li>Trace 라인, 라벨, 색상 구분은 토글로 켜고 끌 수 있음  </li>
<li>라벨 클릭 시 해당 요소까지의 경로만 하이라이트됨</li>
<li>공간(rooms)도 함께 선택하면 시스템이 어느 공간과 관련 있는지 직관적으로 확인 가능  </li>
<li><code>Esc</code> 키를 누르면 전체 시스템 경로로 다시 돌아감</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/b99ec5e2-115f-4b1d-a606-ab45afc31ab5/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/5f2e98c9-5f9e-4dc4-9e75-b4bd7243c6c1/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/4d2d505e-6970-4832-96fe-2705ceee6ee3/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/974f4ca4-9898-4f8e-a90d-382baf2cb89c/image.png" /></p>
<hr />
<h3 id="✅-connections">✅ Connections</h3>
<p>IoT 센서 데이터를 디지털 트윈과 연결해 실시간으로 수신하고 시각화할 수 있는 기능이다.</p>
<p><strong>☑️ Create Connections</strong></p>
<p><em>Connection</em></p>
<ul>
<li>센서 또는 외부 시스템과 Tandem 사이의 연결로, 데이터를 받기 위한 통로 역할이다.</li>
</ul>
<p><em>Streams</em> </p>
<ul>
<li>해당 Connections을 통해 들어오는 시간 기반 데이터(Time-series data)를 저장하고 시각화하는 단위이다. 하나의 센서에 하나의 Stream이 연결된다.</li>
<li>각 Stream은 개별적으로 생성, 분류(Classification), 삭제할 수 있으며, 속성(Properties)도 갖고 있다.</li>
</ul>
<p>Streams 생성 방법</p>
<ul>
<li>Facility 생성 후, 분류(Classification) 및 매개변수(Parameter)가 포함된 Facility Template 지정</li>
<li>좌측 메뉴의 Connections 탭 → <code>+ Create</code> 클릭
<img alt="" src="https://velog.velcdn.com/images/yena121/post/d4df50cf-934d-4665-b3a9-fa477cdf3947/image.png" /></li>
<li>개별 생성(Individual Connection)<ul>
<li>이름 입력, 분류(Classification) 선택
<img alt="" src="https://velog.velcdn.com/images/yena121/post/b9d8123f-d217-4f58-ae04-7c85f624f8dd/image.png" /></li>
</ul>
</li>
<li>대량 생성(Batch Create Connections)<ul>
<li>센서 수가 많을 경우 CSV/XLSX 파일을 활용해 다수 스트림 생성 가능</li>
<li>Streams 생성 창에서 템플릿 파일 다운로드 후 Name, Class, User Classification 항목 입력</li>
<li>CSV 또는 XLSX로 저장 → 파일 업로드</li>
<li>주의: 첫 2줄은 수정하면 안 됨. 변경 시 업로드 실패
<img alt="" src="https://velog.velcdn.com/images/yena121/post/88c94ca2-2523-4dbf-9784-6247a9f60204/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/d605d927-bbf6-426f-b772-40e015d5cac7/image.png" /></li>
</ul>
</li>
<li>생성된 Stream 설정<ul>
<li>분류가 지정되지 않은 경우 → Properties 패널에서 수동 지정 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/b66b3716-54d6-4caa-8ad7-735ff7c53d35/image.png" /></li>
<li>툴바를 통해 차트 보기, Webhook 시크릿 초기화, 삭제 등의 기능 제공
<img alt="" src="https://velog.velcdn.com/images/yena121/post/5b551f3e-cdc9-4bdd-a024-8bdda557bd13/image.png" /></li>
</ul>
</li>
</ul>
<p>Connections을 확인하려면 Filters 패널에서 적절한 Classification을 선택한 후 Inventory 패널에서 확인 가능하다. 수정 불가능한 속성은 회색으로 표시된다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/79b16414-ea53-4bd1-9263-904f827da853/image.png" /></p>
<p><strong>☑️ Configure Connections</strong></p>
<ul>
<li><p>Connections 탭 → 우측 상단 <code>⋮</code>을 클릭</p>
</li>
<li><p>Configure connections: 새 연결을 직접 설정하거나, 기존 연결의 세부 항목을 수정</p>
</li>
<li><p>Import/Export connections: 여러 개의 연결을 한 번에 가져오거나 내보냄</p>
</li>
<li><p>Webhook integration: 외부 시스템에서 Tandem으로 데이터를 실시간 보내고 싶을 때 사용하는 연결 정보
<img alt="" src="https://velog.velcdn.com/images/yena121/post/ad599636-a328-4d98-984b-9c7ea693b618/image.png" /></p>
</li>
<li><p>IoT 장치와 Tandem을 연결하려면?</p>
<ul>
<li>기본 JSON 형식(key-value)으로 데이터 전송 가능해야 함<pre><code class="language-json">&quot;Date&quot;: &quot;2023-03-21T01:15:00Z&quot;,
&quot;Temperature (C)&quot;: 41</code></pre>
</li>
</ul>
</li>
<li><p>HTTP Endpoint로 POST 요청 가능해야 함</p>
<ul>
<li>각 스트림의 엔드포인트는 Streams 탭에서 링크 아이콘 클릭 시 확인 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/54af53ee-c787-47cd-b402-1c923d7df0d0/image.png" /></li>
</ul>
</li>
<li><p>Offline 상태 설정</p>
<ul>
<li>기본값: 15분 이상 데이터 미수신 시 Offline 처리</li>
<li>Classification별로 시간 간격 조정 가능</li>
<li>Summary 탭에서 전체 연결 상태 및 수량 확인 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/7e7e34c1-10f9-453c-b009-93d6214fa5c1/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/d4cdbc01-07b7-44ae-8c52-723b745a02db/image.png" /></li>
</ul>
</li>
</ul>
<p>다음 페이로드 수신 시 속성 패널에 데이터가 반영됨
<img alt="" src="https://velog.velcdn.com/images/yena121/post/ad6140b0-2cc0-4a41-8c5a-578b17793c7f/image.png" /></p>
<p><strong>☑️ IoT 데이터 보기</strong><br />Connections 탭에서 실시간 연결 상태 확인 (실시간/오프라인)</p>
<p>Streams 패널</p>
<ul>
<li>Streams 패널 내에서 개별 차트 확인 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/61f130e2-26fb-4953-999b-29085f3e6bec/image.png" /></li>
<li>필터로 차트 목록 빠르게 좁히기
<img alt="" src="https://velog.velcdn.com/images/yena121/post/d7509d93-27f9-4df1-86bd-f02c9622e3f7/image.png" /></li>
<li>날짜 선택 도구로 최근 30일, 1주, 사용자 지정 범위 지정 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/ce21ae12-245e-48a8-ad3f-5e6b7935d269/image.png" /></li>
<li>각 차트는 확장 아이콘으로 크게 볼 수 있음
<img alt="" src="https://velog.velcdn.com/images/yena121/post/61f691f7-00d9-412e-aef0-d8d26cf37294/image.png" /></li>
<li>⋮ 메뉴에서:<ul>
<li>마지막 수신값 보기</li>
<li>Threshold 선 표시 여부 선택 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/eddfb948-ee2e-4b28-86e7-ef87a5d0b62e/image.png" /></li>
</ul>
</li>
<li>데이터를 Grid View로도 전환 가능<ul>
<li>선택한 필터 기준으로 데이터 목록을 표 형식으로 확인 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/957186de-b685-455d-aee5-7e8ee43bd5b4/image.png" /></li>
</ul>
</li>
</ul>
<p><em>Thresholds</em> </p>
<ul>
<li>Streams 패널 내에서 각 스트림에 대해 Thresholds(임계값) 설정</li>
<li>차트에서 Threshold 아이콘 클릭
<img alt="" src="https://velog.velcdn.com/images/yena121/post/4d2e6d22-2886-4444-a0f6-1456c01e438d/image.png" /></li>
<li>이름 설정 + 다음 범위 지정:<ul>
<li>Warning (High / Low)</li>
<li>Alert (High / Low)
<img alt="" src="https://velog.velcdn.com/images/yena121/post/385e5088-3471-4f00-bb1e-85b56518c5f0/image.png" /></li>
</ul>
</li>
<li>색상:<ul>
<li>Warning: 노란색</li>
<li>Alert: 빨간색
<img alt="" src="https://velog.velcdn.com/images/yena121/post/b0865682-b045-4806-b5a2-7c2a1f80b598/image.png" /></li>
</ul>
</li>
<li>수정 방법<ul>
<li>Thresholds 아이콘 → 연필 아이콘 클릭 → 값 변경 후 저장
<img alt="" src="https://velog.velcdn.com/images/yena121/post/17f8535d-9e89-48c5-847f-f3a193254163/image.png" /></li>
<li>또는 Grid 뷰에서 셀 더블 클릭으로 수정 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/6f41c526-0f0b-478f-bfa0-a64b5ba92566/image.png" /></li>
</ul>
</li>
</ul>
<p><em>Heatmaps</em> </p>
<ul>
<li>Streams의 실시간 데이터를 공간 또는 시스템에 색상으로 시각화하는 기능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/226cb38c-9358-4375-b083-93cf3297774a/image.png" /></li>
<li>우측 하단 Data Visualization 메뉴 → Heatmap 선택
<img alt="" src="https://velog.velcdn.com/images/yena121/post/5765bbcf-a1b3-4aa4-a9cf-1234da7989f5/image.png" /></li>
<li>Heatmap 위젯에서 연결된 Streams 중 하나를 선택
<img alt="" src="https://velog.velcdn.com/images/yena121/post/494eaf99-9771-489f-8890-eee413a85d48/image.png" /></li>
<li>설정 아이콘 클릭:<ul>
<li>시스템별 Heatmap 적용 여부</li>
<li>색상 테마 선택</li>
<li>투명도, 최소/최대값 설정 가능</li>
<li>설정은 실시간 반영 및 뷰 저장 시 저장됨
<img alt="" src="https://velog.velcdn.com/images/yena121/post/dd74dbcb-a656-447d-bfde-91e2e1d7ee8c/image.png" /></li>
</ul>
</li>
<li>적용 방식<ul>
<li>공간 기반 시각화: 공간과 연결된 스트림 기준 색상 표시
<img alt="" src="https://velog.velcdn.com/images/yena121/post/d196917f-8821-4e90-9b8c-62722f61b3b7/image.png" /></li>
<li>시스템 기반 시각화: 시스템 선택 후 Heatmap 적용 시 표시됨
<img alt="" src="https://velog.velcdn.com/images/yena121/post/8c2bdd79-c711-4ece-8edc-c922db5f2e18/image.png" /></li>
</ul>
</li>
</ul>
<hr />
<h3 id="✅-spaces">✅ Spaces</h3>
<p>Autodesk Tandem에서는 Rooms(방)과 Spaces(공간)도 중요한 자산으로 간주되며, 사용자 정의 매개변수(예: Room Name, Number, Department, Area 등)를 통해 분류하고 관리할 수 있다.</p>
<ul>
<li><p>시설(Facility)에서 좌측 메뉴의 Spaces 패널을 연다
<img alt="" src="https://velog.velcdn.com/images/yena121/post/60ff5216-b5e0-45fc-a4a8-b3f8b10f5868/image.png" /></p>
</li>
<li><p>Spaces 탭에서는 공간을 Rooms 또는 Levels 기준으로 볼 수 있다  </p>
<ul>
<li>Rooms 탭에서는 이름(Name), 번호(Number), 층(Level) 기준으로 정렬 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/17d3a4a6-8178-48bb-a9b7-b07ca1f257dd/image.png" /></li>
</ul>
</li>
<li><p>하나 이상의 공간을 선택하면 Properties 패널이 열리며 다음 정보들을 확인할 수 있다:  </p>
<ul>
<li>공통 식별 정보(Common identity data)  </li>
<li>사용자 정의 매개변수(Custom parameters)  </li>
<li>관계 정보(Relationships)  </li>
<li>설계 속성(Design properties)
<img alt="" src="https://velog.velcdn.com/images/yena121/post/301235a8-eeb4-4a61-a9c0-886d29ca8d7b/image.png" /></li>
</ul>
</li>
<li><p>Relationship 섹션 </p>
<ul>
<li>연결된 시스템(Systems)이 있는지, IoT 연결(Connections)이 있는지 요약 표시됨
<img alt="" src="https://velog.velcdn.com/images/yena121/post/7624f9f6-2333-49d2-aa6c-6e15c58182b4/image.png" /></li>
</ul>
</li>
<li><p>Spaces 패널의 Levels 탭에서는 각 층(Level)에 대한 정보 확인 가능</p>
<ul>
<li>각 Level을 클릭하면 해당 Level의 속성과 관계(Relationships)가 나타남  </li>
<li>예: 어떤 층이 결합되어 있는지 확인 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/f280e203-db15-4a82-9354-86a6568e59ef/image.png" />  </li>
</ul>
</li>
<li><p>여러 모델을 불러온 경우 중복 Level(Duplicate Level)이 생성될 수 있음  </p>
<ul>
<li>이 경우 해당 Level의 속성 패널을 열고, Name 필드를 기존 리스트에 있는 이름으로 수정  </li>
<li>예: “Datum”을 “00 – Site”로 수정 → Enter 입력 시 Datum은 목록에서 사라지고 “00 – Site”의 관계 섹션에 병합됨
<img alt="" src="https://velog.velcdn.com/images/yena121/post/3c558216-0f9d-45fa-90e7-d6f9e9af64fc/image.png" /></li>
</ul>
</li>
</ul>
<hr />
<h3 id="✅-data-dashboard">✅ Data Dashboard</h3>
<p>디지털 트윈 내 자산 데이터의 완성도(Completeness)를 시각적으로 분석하고 추적할 수 있도록 도와준다.
대시보드는 자산 분류 진행 상황 및 데이터 매핑 상태를 차트 형태로 보여준다.</p>
<p><strong>☑️ 대시보드 생성</strong>  </p>
<ul>
<li>Facilities 메인 페이지에서 <code>Create Dashboard</code> 선택
<img alt="" src="https://velog.velcdn.com/images/yena121/post/d68b7250-9f59-4d5b-afd8-712a7f039384/image.png" /></li>
<li>대시보드 이름 입력 → 기존 View 선택 → <code>Create</code> 클릭  <ul>
<li>View를 선택하지 않으면 기본 Master View가 사용됨
<img alt="" src="https://velog.velcdn.com/images/yena121/post/65370e5f-23cf-407a-8bf9-0dbfeea05fad/image.png" /></li>
</ul>
</li>
</ul>
<p><strong>☑️ Navigating Cards</strong>  </p>
<ul>
<li>카드 내 파란색 텍스트 클릭 → 해당 매개변수 상세 보기
<img alt="" src="https://velog.velcdn.com/images/yena121/post/124e4e3b-e715-41e2-a38c-5e9665ed0c28/image.png" /></li>
<li><code>Go to Inventory</code> 버튼 클릭 시:<ul>
<li>현재 필터 기준으로 Inventory 탭 열림</li>
<li>해당 매개변수가 두 번째 열에 강조되어 표시됨
<img alt="" src="https://velog.velcdn.com/images/yena121/post/01db2fab-c060-4fbf-b6e4-5c3d48cf1316/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/9755b47a-09ab-40f3-8659-49a2894f16d6/image.png" /></li>
</ul>
</li>
<li>대시보드 내보내기<ul>
<li>우측 상단 드롭다운 메뉴에서 <code>Export as PDF</code> 선택 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/7bd87f03-9b80-4f27-aad7-c1d4f8fcfd48/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/ba1a3881-09e3-415f-a6e3-d60830653386/image.png" /></li>
</ul>
</li>
</ul>
<p><strong>☑️ Creating and Customizing Cards</strong>  </p>
<ul>
<li>카드 크기 조절: 가장자리 드래그</li>
<li>카드 이동: 회색 헤더 클릭 후 드래그
<img alt="" src="https://velog.velcdn.com/images/yena121/post/b3d26f11-b91d-4be2-ba76-1154e4f33c9d/image.png" /></li>
</ul>
<p>왼쪽 카드 종류  </p>
<ul>
<li>Summary:<ul>
<li>Classification 요약</li>
<li>tagged assets 수</li>
<li>전체 elements 수</li>
<li>classified elements 수</li>
<li>parameters 수</li>
</ul>
</li>
<li>Table:<ul>
<li>Classification 별 매개변수 목록</li>
<li>Completeness 기준 매개변수 목록
<img alt="" src="https://velog.velcdn.com/images/yena121/post/eb9bdbdc-3e75-48c2-82e7-76d60b0c481c/image.png" /></li>
</ul>
</li>
</ul>
<p>하단 카드 종류  </p>
<ul>
<li>Data:<ul>
<li>Classification 상태</li>
<li>Revit 카테고리별 분류 현황</li>
<li>사용자 매개변수 완료 상태 (분류 기준 / Revit 기준)</li>
<li>레벨별 데이터 완료도</li>
<li>레벨별 태그된 자산 수</li>
</ul>
</li>
<li>Visualization:<ul>
<li>파이 차트 (Pie Chart)</li>
<li>도넛 차트 (Donut Chart)</li>
<li>선버스트 차트 (Sunburst Chart)
일부 시각화는 특정 데이터 타입에서만 사용 가능함
<img alt="" src="https://velog.velcdn.com/images/yena121/post/8d1e07f6-9526-45ef-b6a8-080ea4b7e894/image.png" /></li>
</ul>
</li>
</ul>
<p>카드 수정 및 추가/삭제</p>
<ul>
<li>카드 우측 상단 Gear 아이콘 클릭 → 카드 설정 변경</li>
<li>휴지통 아이콘 → 카드 삭제</li>
<li><code>+</code> 아이콘 → 새 카드 추가
<img alt="" src="https://velog.velcdn.com/images/yena121/post/2decc497-722d-45d0-bab0-835dd0e4b102/image.png" /></li>
</ul>
<p><strong>☑️ Filtering the Dashboard</strong>  </p>
<ul>
<li>뷰 바(View Bar)에서 기존에 설정된 필터를 선택해 원하는 데이터만 골라볼 수 있음</li>
<li>비슷한 특성끼리 묶거나(클러스터링) 색상으로 구분할 수 있고, 필터의 적용 순서나 구조도 조정할 수 있음
<img alt="" src="https://velog.velcdn.com/images/yena121/post/9782c635-63c0-4d3e-bbbb-12dcc3dbd373/image.png" /></li>
<li>필터 아이콘 클릭 시 모든 속성 목록이 표시되고 필터 항목에 추가 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/bdc62108-c192-4ee0-86c5-92c7efc4dcfc/image.png" /></li>
</ul>
<hr />
<h3 id="✅-views">✅ Views</h3>
<p>특정 Assets(자산) 또는 Spaces(공간) 그룹을 정의할 때, Views 기능을 활용하면 필터링된 데이터셋을 저장하고 빠르게 불러올 수 있다.</p>
<p><strong>☑️ 주요 기능</strong>  </p>
<ul>
<li>자주 사용하는 자산/공간/시스템 그룹을 뷰로 저장하여 빠르게 전환 가능  </li>
<li>시설 내 다양한 구성요소를 빠르게 탐색하는 데 유용
<img alt="" src="https://velog.velcdn.com/images/yena121/post/256a6c4f-cb8c-4cce-ab86-0d265985dde2/image.png" /></li>
</ul>
<p><strong>☑️ View가 저장하는 요소들</strong>  </p>
<ul>
<li>필터 패널에 표시되는 Parameters</li>
<li>필터 적용 조건  </li>
<li>인벤토리 탭의 열 구성(Column 구성)</li>
<li>뷰어 설정:<ul>
<li>색상 코딩(Color coding)  </li>
<li>클러스터링(Clustering)</li>
<li>카메라 위치(Camera position)  </li>
<li>초점 거리(Focal length)  </li>
<li>단면 뷰 설정(Section planes)</li>
</ul>
</li>
</ul>
<hr />
<h3 id="✅-inventory">✅ Inventory</h3>
<p>필터 조건에 따라 자산(Assets)의 목록을 표(Spreadsheet) 형태로 표시하는 기능이다.<br />Classification, Assembly Code, Revit Category 등 다양한 속성을 기준으로 자산을 열람할 수 있다.</p>
<p><strong>☑️ Inventory 패널 구성</strong>  </p>
<table>
<thead>
<tr>
<th>번호</th>
<th>기능 설명</th>
</tr>
</thead>
<tbody><tr>
<td>1</td>
<td>상단 파란선: 패널 크기 조절 가능</td>
</tr>
<tr>
<td>2</td>
<td>Filter Rows: 필드/조건/값을 선택하여 필터 적용 (<code>+ Add Condition</code>으로 다중 조건 가능)</td>
</tr>
<tr>
<td>3</td>
<td>Follow Selection: 자산 더블클릭 시 뷰어에서 확대 및 하이라이트</td>
</tr>
<tr>
<td>4</td>
<td>Show Tagged Assets: 필터된 항목 중 태그된 자산만 보기</td>
</tr>
<tr>
<td>5</td>
<td>Columns: <code>Edit Column</code> 창을 열어 보고 싶은 자산 속성 선택/정렬</td>
</tr>
<tr>
<td>6</td>
<td>Export: 현재 목록을 <code>.csv</code> 또는 <code>.xlsx</code>로 내보내기 (이름 설정 가능)</td>
</tr>
<tr>
<td>7</td>
<td>Import: 내보낸 파일에 변경사항을 반영해 다시 가져오기</td>
</tr>
<tr>
<td>8</td>
<td>Open in New Window: Inventory 패널을 독립된 창으로 분리</td>
</tr>
<tr>
<td>9</td>
<td>패널 닫기 (Close)</td>
</tr>
</tbody></table>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/833cda8b-437a-4cae-a62e-39434e03f9d5/image.png" /></p>
<p><strong>☑️ Inventory 패널 구성 설정</strong>  </p>
<ul>
<li><code>Edit Column</code> 창에서 표시할 자산 속성을 검색 및 선택 가능  </li>
<li>열 순서도 드래그로 변경 가능  </li>
<li><code>Update</code> 클릭 시 반영됨
<img alt="" src="https://velog.velcdn.com/images/yena121/post/4ef6c7c2-fd90-4594-bcb0-ac8ab50f87cf/image.png" /></li>
</ul>
<p>셀 색상에 따른 편집 가능 여부:  </p>
<ul>
<li>연한 회색: 직접 편집 가능  </li>
<li>진한 회색: 편집 불가 (Revit 속성)  </li>
<li>전체 회색 열: 작성 모델(Revit)에서만 수정 가능</li>
</ul>
<p>열 머리글 색상 구분:  </p>
<ul>
<li>기본 Tandem 매개변수: 앞쪽 두 열  </li>
<li>Revit 요소 속성: 파란색  </li>
<li>Revit 타입 속성: 초록색
<img alt="" src="https://velog.velcdn.com/images/yena121/post/65b22063-19c5-4582-ac66-f315e25101df/image.png" /></li>
</ul>
<p>열 정렬 및 사이즈 조절:  </p>
<ul>
<li>열 헤더 드래그 → 순서 변경  </li>
<li>열 경계 클릭 후 드래그 → 너비 조절</li>
</ul>
<p><strong>☑️ Filtering in Inventory</strong>  </p>
<ul>
<li>Filter Panel 사용:<br /> 필터를 적용하거나 해제하면, 해당 조건에 맞는 자산만 Inventory에 표시됨</li>
<li>Inventory Panel 내 필터링 (Filter Rows):<br /> 선택된 자산 목록에 직접 조건 필터 적용  <ul>
<li>이 방식은 Filter Panel의 조건과는 독립적으로 작동
<img alt="" src="https://velog.velcdn.com/images/yena121/post/8d6efa55-8299-4700-b176-bf3ded63269f/image.png" /></li>
</ul>
</li>
</ul>
<p><strong>☑️ 자산 데이터 추가/수정 방법</strong>  </p>
<ol>
<li><p>직접 입력(Manual Entry) </p>
<ul>
<li>연한 회색 셀 클릭 → 직접 값 입력 가능  </li>
<li>다수 셀 선택 후 우클릭 → 아래 메뉴 사용 가능:  <ul>
<li>Show/Hide Classification Title  </li>
<li>Copy column value to all rows  </li>
<li>Clear cell value  </li>
<li>Auto-fit columns (너비 자동 조정)
<img alt="" src="https://velog.velcdn.com/images/yena121/post/86d10194-6909-407f-b215-aeb6a75d3611/image.png" /></li>
</ul>
</li>
</ul>
</li>
<li><p>Export / Import 방식</p>
<ul>
<li><code>Export</code>로 데이터 파일 저장 → 수정 후 → <code>Import</code>로 반영  </li>
<li>변경된 항목만 적용되며, 나머지 데이터는 유지됨
<img alt="" src="https://velog.velcdn.com/images/yena121/post/0a1a47d6-bf34-4bd5-b022-712ac8522b5a/image.png" /></li>
</ul>
</li>
</ol>
<hr />
<h3 id="✅-properties">✅ Properties</h3>
<p>선택한 자산(asset)의 정보를 손쉽게 확인하고 편집할 수 있도록 도와준다.<br />뷰어에서 자산을 클릭하면 자동으로 패널이 열리며, 모델과 함께 가져온 속성(Design Properties) 또는 수동으로 입력된 정보들을 확인할 수 있다.</p>
<p><strong>☑️ Properties 패널 구성</strong>  </p>
<table>
<thead>
<tr>
<th>번호</th>
<th>기능 설명</th>
</tr>
</thead>
<tbody><tr>
<td>1</td>
<td>Viewer Toolbar: 뷰어 툴바에서 Properties 패널 열기</td>
</tr>
<tr>
<td>2</td>
<td>View History: 해당 자산을 마지막으로 수정한 사용자 확인 가능</td>
</tr>
<tr>
<td>3</td>
<td>Close: 패널 최소화</td>
</tr>
<tr>
<td>4</td>
<td>Element Properties: 해당 개별 자산의 속성 및 사용자 정의 매개변수 보기</td>
</tr>
<tr>
<td>5</td>
<td>Type Properties: 동일한 유형의 자산 그룹에 적용되는 속성 보기</td>
</tr>
</tbody></table>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/dcdadf65-643b-4069-97d9-be11aec7eb76/image.png" /> <img alt="" src="https://velog.velcdn.com/images/yena121/post/e77173ed-6ffc-4d2e-ba51-9b70d0db8e30/image.png" /></p>
<p><strong>☑️ Properties 패널의 데이터 구성</strong>  </p>
<ol>
<li><p>Asset Properties</p>
<ul>
<li>사용자 정의 매개변수(Custom Parameters)를 카테고리별로 정리  </li>
<li>편집 가능</li>
</ul>
</li>
<li><p>Relationship </p>
<ul>
<li>해당 자산이 연결된 공간(Spaces), 연결(Connections), 시스템(Systems) 정보 표시  </li>
<li>병합된 층(Level)이 있는 경우 함께 표시될 수 있음</li>
</ul>
</li>
<li><p>Design Properties  </p>
<ul>
<li>모델 작성 툴에서 가져온 속성 (카테고리별로 정리됨)  </li>
<li>Tandem 내에서 편집 불가 </li>
<li>작성 모델에서 수정 후 재업로드 시 변경된 항목만 반영되며, 기존 전체를 덮어쓰지 않음</li>
</ul>
</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/49259e01-b224-4273-bfa2-4d43d1050049/image.png" /></p>
<p><strong>☑️ 자산 데이터 입력 및 업데이트 방법</strong>  </p>
<ul>
<li>속성을 수정하려면 먼저 자산이 분류(Classified)되어 있어야 함  </li>
<li>분류된 자산의 경우, Properties 패널에서 각 속성 필드 클릭 → 직접 입력 또는 선택 가능</li>
<li>매개변수에 값이 입력되면 해당 자산은 Tagged Asset으로 간주되며,  </li>
<li>사용자의 구독 범위 내에서 태그된 자산 수로 집계됨</li>
<li>전체 시설의 태그된 자산 수는 <code>Manage &gt; Usage</code> 탭에서 확인 가능
<img alt="" src="https://velog.velcdn.com/images/yena121/post/74ce84b7-3e69-4927-9f9e-0e95cabed1fc/image.png" /></li>
</ul>
<p><strong>☑️ Type 속성 편집</strong>  </p>
<ul>
<li>Type 속성은 동일한 유형(Type)의 모든 자산에 적용됨  </li>
<li>예: 동일한 창 4개 중 하나를 수정하면, 모든 동일한 창에 해당 속성이 반영됨
<img alt="" src="https://velog.velcdn.com/images/yena121/post/4c539342-2c2e-480e-921d-2d13d48bd517/image.png" /></li>
</ul>
<p>분류(Classification) 및 매개변수 매핑(Parameter Mapping)을 활용하면 자산 정보를 자동으로 채울 수 있음  </p>