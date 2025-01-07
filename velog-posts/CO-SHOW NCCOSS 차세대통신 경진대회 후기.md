<p>대구 EXCO 에서 열린 2024 CO-SHOW 경진대회 참가를 위해 11월 19일부터 22일까지 3박 4일 다녀왔다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/a4fde2c7-a79c-4371-b025-9c6f9ac63c6f/image.png" /></p>
<blockquote>
<p>나가게 된 계기</p>
</blockquote>
<p>9월 초에 아는 동생이 사용자의 사진을 화가의 스타일로 바꾸는 서비스를 하나 만들어서 11월에 대구에서 열릴 전시회에서 체험 부스를 운영해보겠냐는 김영길 교수님의 권유를 받았다고 한다. 건너 건너 연락을 받아 가벼운 마음으로 같이 시작하게 되었다. 너무나 가벼운 마음이었기에 대회인 줄 모르고 안 해본 경험을 하고자 백엔드 개발을 맡았다.</p>
<blockquote>
<p>개발하며 가장 중요하게 생각한 부분</p>
</blockquote>
<p>서비스 이름은 &quot;Articket&quot;이고, 부제는 &quot;당신의 사진이 예술이 되는 곳&quot;이다.</p>
<p>사용자의 사진을 피카소, 르누아르, 고흐, 리히텐슈타인의 스타일로 바꿔주고, 간단한 성격 테스트를 통해 어울리는 화가를 찾아주어 변환된 이미지와 성격 설명이 담긴 티켓을 출력해준다.</p>
<p>이때 성격 테스트는 모니터에 성격 테스트 질문을 띄워놓고 사용자 휴대폰에서 답변을 하여 휴대폰으로 모니터를 제어할 수 있도록 하였다. (이 부분은 모니터를 띄워놔야 하는 부스의 특성으로 편리함을 위해 추가한 기능이다.)</p>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/442f5a2a-1e39-4979-9caf-175fcbffb2a8/image.gif" /></p>
<p>이것이 출력된 티켓이다. 이렇게 출력을 해서 드릴 예정이다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/4fbc5a3e-7624-4f36-b956-9fe6bd9e2f23/image.png" /></p>
<p>나는 주로 AI 모델 관련 API 설계와 이미지 데이터 처리 등 Flask로 백엔드 개발을 담당하였다. 초기 기획을 토대로 개발하며 가능한지 여부에 따라 수시로 기능이 수정될 예정이라 초기 빠른 개발과 이후 유연한 수정 작업을 위해 Flask를 선택하였다.</p>
<p>개발하며, 가장 중요하게 생각했던 점이 있다.</p>
<p><strong>1️⃣ 이미지 생성 속도</strong></p>
<p>처음에는 스테이블디퓨전 오픈소스를 clone 해와서 직접 로컬에서 돌리며 생성한 public URL에 엔드포인트로 접근하여 API를 사용했다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/c184a1fa-13f3-4069-a2bc-e98e42178aaf/image.png" />
이때 CLIP 부터 세 개의 이미지 생성까지 총 3분 가까이 걸렸고, 다음에 더 무거워질 로라 모델에 대비하여 생성 속도를 꼭 줄여야 했다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/e0866e7b-74ed-4951-bcb3-63b1bcb3bcd3/image.png" />
이번엔 스테이블디퓨전 오픈소스를 google colab에서 NVIDIA A100라는 고성능의 GPU을 사용하여 돌렸고 16초까지 줄일 수 있었다.</p>
<p>이후 google colab은 사용 시간에 제한이 있었기 때문에 시간 제한 없이 사용량만큼 지불하는 Runpod에서 NVIDIA RTX4090을 사용하였다.</p>
<p>이후 CLIP 대신 BLIP으로 텍스트를 생성하기로 했고 그 과정은 몇 초 걸리지 않았다. 하지만 많은 양의 화가 작품을 학습한 로라 모델을 돌렸을 때는 하나의 이미지 생성에만 30초 이상 걸렸고, 네 명의 화가에 대한 이미지를 생성해야 했기 때문에 총 120초가 소요되었다.</p>
<pre><code>with ThreadPoolExecutor(max_workers=2) as executor:
    futures = []
    futures.append(executor.submit(process_artist_group, group1_artists, WEBUI_URL1))
    futures.append(executor.submit(process_artist_group, group2_artists, WEBUI_URL2))</code></pre><p>소요 시간을 줄이기 위해 <code>ThreadPoolExecutor</code>으로 스레드 풀을 사용하여 호출을 비동기적으로 실행하도록 하였다.
화가를 두 명씩 짝을 지어 병렬적으로 동시에 생성되도록 하여 총 소요 시간을 1분으로 줄였다. 네 명의 화가 이미지를 각각 동시에 생성하면 30초 정도로 줄었겠지만 이때 사용하는 스테이블디퓨전 public URL을 4개를 생성하기 위해 돈도 4배로 들기 때문에 2개만 생성하고 1분 동안 사용한 기술에 대해 설명하기로 하였다. (이때 한 public URL에 동시에 요청을 하면 GPU 메모리 사용량이 매우 커져 time out 에러가 나서 동시에 실행하는 스레드 개수만큼 public URL을 생성해야 했다...)</p>
<p>또 현재 GPU 메모리 사용량에 따라 이미지 생성에 실패할 때가 한 번씩 있었다. 한 API에서 네 명의 화가 이미지를 생성하도록 해놨기 때문에 한 화가로 이미지 생성 실패를 반환하는 것을 막기 위해 한 화가당 기회를 세 번까지 줘서 이미지 생성 실패를 하면 3번까지 다시 시도할 수 있도록 하였다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/f693c88d-fa47-4270-aa6f-f70753d69a92/image.png" />
이건 지금 생각해보니 높아진 GPU 메모리 사용량 때문에 에러가 났을 거 같은데 여러 개의 public URL에 대해 현재 메모리 사용량에 따라 또는 ping을 보내 가장 응답이 빠르게 오는 즉 가장 여유있는 URL에 API 요청을 하는 방법과 한 화가 이미지 생성이 실패하더라도 나머지 성공한 이미지들을 반환하여 일부만 제공하는 방법이 있었을 거 같다.</p>
<p><strong>2️⃣ 자동화</strong></p>
<p>많은 사람들이 부스에 올 것을 대비하기 위해 사용자로부터 받은 이미지가 티켓이 되는 과정을 최대한 자동화 시키고 싶었다.</p>
<p>사용자와 매칭된 화가에 대한 티켓 템플릿에 생성된 이미지를 넣어 출력을 해야 했다. 이것을 자동화 시키기 위해,
우선 백엔드 서버에 이미지 부분은 비워둔 각 화가의 티켓 템플릿 PDF 4개를 넣어두었다. 매칭된 화가에 대한 티켓 템플릿 PDF를 가져오고 ReportLab을 사용하여 생성된 이미지 각각 크기와 위치를 지정하여 새 PDF를 만들고 PyPDF2을 사용하여 기존 템플릿 PDF와 병합하여 완성된 티켓 PDF를 연결된 구글 클라우드에 업로드 되도록 하였다.
이렇게 체험자분 옆에서 응대하고 끝나면 가서 출력만 해오면 되도록 자동화하였다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/44c2055f-7dea-496c-958c-6041780211b4/image.png" /></p>
<p>스테이블디퓨전의 public URL와 BLIP의 public URL을 생성하여 이 URL로 요청을 넣기까지도 자동화를 시키고 싶었지만 고성능 GPU를 내 노트북에 탑재하여 돌리기는 무리였고 클라우드 서비스를 이용해야 했기 때문에 어쩔 수 없이 이건 직접 돌려 생성한 URL을 환경변수에 넣어 사용하였다.</p>
<blockquote>
<p>개발하며 어려웠던 부분</p>
</blockquote>
<p>스테이블디퓨전의 public URL로 API에 접근하는데 자꾸 에러가 났다. 알아보니 runpod에서 스테이블디퓨전 인스턴스를 실행을 하면 UI 모드로만 public URL이 생성되고, HTTP 엔드포인트로는 접근이 안 되었다. 분명 API를 정의한 파일이 존재하였고 public URL을 생성할 때 명령어를 수정하면 된다고 생각했다.</p>
<pre><code>def start():
    if '--api' in sys.argv:
        webui.api_only()  # API 서버만 실행
    else:
        webui.webui()  # UI 서버 실행</code></pre><p><code>python launch.py</code>하여 public URL을 생성할 때 뒤에 <code>--api</code>옵션을 추가해야 했다. 넣지 않으면 기본적으로 UI 서버만 활성화되고 API 서버는 비활성화 상태이다.
기본적으로 로컬에서만 접근 가능하도록 <code>127.0.0.1</code>에서만 서버를 열어준다. 하지만 나는 클라우드 서비스를 사용하여 스테이블디퓨전을 돌리고 있고 로컬이 아닌 내 노트북 즉 다른 ip에서 접근을 해야 한다. 이때 <code>--api</code> 옵션을 추가하면 서버가 <code>0.0.0.0</code>으로 바인딩되어 모든 ip 즉 모든 네트워크에서의 요청을 수신할 수 있다.</p>
<pre><code># BLIP-2 모델 로드 함수 정의
def load_blip_model():
    # GPU 사용 여부 확인, 모델 및 프로세서 로드
    # 오류 발생 시 RuntimeError로 처리
    pass

# 모델 로드
try:
    processor, model, device = load_blip_model()
except Exception as e:
    processor, model, device = None, None, None
    print(e)

# 이미지 캡션 생성 API
@app.post(&quot;/generate_caption&quot;)
async def generate_caption_for_image(file: UploadFile = File(...)):
    # 모델 로드 실패 처리

        # 이미지 파일을 로드하고 전처리
        image = Image.open(io.BytesIO(await file.read())).convert(&quot;RGB&quot;)

        # 모델 입력 생성 및 캡션 생성
        inputs = processor(images=image, text=&quot;&quot;, return_tensors=&quot;pt&quot;).to(device)
        generated_ids = model.generate(**inputs, max_new_tokens=30, min_new_tokens=10)
        caption = processor.batch_decode(generated_ids, skip_special_tokens=True)[0].strip()

        # 결과 반환
        return {&quot;caption&quot;: caption}

    except Exception as e:
        # 오류 발생 시 HTTP 500 응답 반환
</code></pre><p>또, 이미지 캡션 생성을 위해 BLIP 모델을 사용하기로 했는데 관련 모델의 코드를 뒤져보니 API를 제공하지 않았다. 그래서 직접 BLIP 모델에 접근하여 캡션을 반환하는 API를 작성하여 기존에 없던 3002포트에 바인딩되도록 하여 public URL을 통해 접근할 수 있도록 하였다.</p>
<blockquote>
<p>3박 4일간 대회를 진행하며</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/yena121/post/d2519b69-0e6f-4969-8f1b-8175ca16967f/image.png" /></p>
<p>생각했던 것보다 우리 부스에 훨씬 더 많은 분들이 찾아와주셨다.
다시 한 번 더 참여해주신 분들도 있었고, 나중에 지인을 데려오신 분들도 있었다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/9ecbc114-80da-4ea0-b08e-fd988e5ed294/image.png" />
많은 분들이 본인 사진이 변환된 모습을 보며 신기해하셨고, 반려동물도 많이 올려주시며 좋아하셨다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/508ea20e-5c92-46aa-bcbe-6c3811ee703c/image.png" />
<img alt="" src="https://velog.velcdn.com/images/yena121/post/ca56466d-5df4-4e1d-946c-5acd340c9910/image.png" />
<img alt="" src="https://velog.velcdn.com/images/yena121/post/7f4ad3a6-9700-4de1-aeda-3e8370465ad7/image.png" /></p>
<p>이미지가 생성되는 동안 아래 다이어그램을 보여주며 전체적인 데이터의 흐름을 설명해주었는데, 이때 개발 경험이 있으신 몇몇 분들이 WebSocket을 쓰면 되는 부분에 HTTP를 사용한 이유 (이건 단순한 요청일 경우에는 비실시간성인 http로 리소스 소비를 줄이기 위함이라고 답변 했다) 또는 이미지를 어떻게 처리하여 보냈는지 (이건 바이너리 값을 전송했을 때보다 데이터 손상이 적은 base64 문자열로 인코딩하여 전송했다고 답변 했다) 등등의 개발과 관련된 질문을 해주시는 분들이 몇몇 분들이 계셨다. AI 모델 학습이 아닌 이러한 전체적인 데이터 흐름을 짜고 개발했던 나로써는 이런 개발 관련 질문들이 너무 반가웠다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/0ebf3124-ea70-4f97-8dcb-85b228758299/image.png" /></p>
<p>하지만 이 서비스는 배포하지 않고 로컬에서 돌아갔기 대문에 사용자가 이미지를 업로드하여 서버로 전송하려면 사용자의 휴대폰이 내 노트북의 ip를 알 수 있도록 같은 네트워크에 들어와야 했다. 그러기 위해서 내 노트북에 접속된 와이파이에 먼저 접속하도록 부탁드려야 하는 번거로움이 있었다.</p>
<p>부스 운영 첫 날, 생각보다 많은 분들이 오셨고 이를 불편해 하시는 분들이 많았다.
사용자 휴대폰에서 와이파이 접속 없이 로컬 서버로 접근 할 수 있도록 <code>ngrok</code>를 사용하여 로컬 서버를 외부에서 접근하도록 변경하려 하였다.
하지만 알 수 없는 에러가 계속 났다. 다른 api는 다 동작하는데 생성된 이미지를 클라이언트에서 저장하기 위해 백엔드에서 이미지의 base64를 보내는 api에서만 에러가 났다.
원인을 알지 못해 결국 해결하지 못하고 그대로 진행하였다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/27858c56-f49b-4741-b075-5b0eb2179454/image.png" /></p>
<p>지금 블로그를 정리하며 알게된 점은,</p>
<p>브라우저와 서버가 같은 네트워크 내에서 동작할 때는 기본 데이터의 크기 제한이 충분히 크기 때문에 문제가 없었다.
하지만 <code>ngrok</code>과 같은 프록시를 사용하면 데이터가 브라우저 → ngrok → 서버로 전달되어야 하는데,
<code>ngrok</code>는 전송 가능한 데이터 크기를 제한하여 요청 데이터를 거부했을 가능성이 있다. </p>
<p>즉 로컬보다 네트워크 제약이 더 큰데, 제약 조건보다 응답 데이터 크기가 아주 컸기 때문에 에러가 났던거 같다.
(이러한 값을 전송해줘야 했다..ㅎㅎ)
<img alt="" src="https://velog.velcdn.com/images/yena121/post/d04d5d2d-355e-46e0-9242-07648d9ba05c/image.png" /></p>
<p>이틀동안 부스 운영을 하고 마지막 날,
발표와 질의응답으로 대회를 마무리 지었다.</p>
<p>사용한 기술에 대해 질문을 하실 거라는 예상과 달리 이 서비스의 상업성에 대한 질문을 많이 하셨다.
미술 외에 다른 분야로 뻗어가기에는 너무 많은 노력이 필요하지 않을지, 지금은 오픈소스를 사용을 한 것이지 이 서비스만의 독창적인 특징이 보이지 않는데 어떻게 생각하냐, 수익 구조는 뭐냐, 이 서비스의 타켓층의 크기는 어떻게 잡았는지 등 생각하지 못한 점들을 많이 물어보셨다. </p>
<p>이 서비스에 사용한 기술들을 제대로 알고 썼는지 그리고 부스 체험자 분께 사용한 기술을 얼마나 효과적으로 소개할 수 있었는지 이런 점들만 염두했는데, 이 서비스가 상용화 될 것을 생각해보지는 못했었다.</p>
<p>서비스를 만들 때 기본적으로 생각해야 할 부분들을 배울 수 있었다.</p>
<p>그리고</p>
<p>정말 기대하지 않았는데, 너무 감사하게도 4등을 주셨다.
<img alt="" src="https://velog.velcdn.com/images/yena121/post/5d149d66-5f04-459c-825a-d642a880c8e9/image.png" /></p>
<blockquote>
<p>대회를 끝마치며</p>
</blockquote>
<p>대회를 통해 받은 피드백을 토대로 보완하여 내년에 있을 동아리 축제 부스에서 사용할 생각이다.
그때는 사용자가 와이파이 접속을 하지 않아도 되도록 하고, 비용을 줄이며 이미지 생성을 더 빠르게 할 방법과 더 효율적인 데이터 흐름을 짜는 것에 중점을 두어 이런 큰 이벤트에 종종 쓸 수 있는 안정적인 서비스가 되도록 하고 싶다.</p>