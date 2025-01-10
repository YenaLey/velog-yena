<p><code>npx create-react-app [프로젝트 이름]</code> 으로 리액트 프로젝트 생성 시 에러가 나며 일부 패키지가 깔리지 않았다.
<code>@testing-library/react@13.4.0</code>와 같은 라이브러리가가 React 19와 호환되지 않기 때문이다.</p>
<p>현재 <code>create-react-app</code>은 최신 버전(19)으로 설치하려 하는데, React 19는 아직 일부 주요 라이브러라와의 완전히 호환되지 않는다.
React 19는 아직 실험적인 부분이 많아 안정적인 개발을 위해 React 18로 버전을 낮춰야 한다.</p>
<blockquote>
<p>해결방법</p>
</blockquote>
<ol>
<li><code>npx create-react-app</code>으로 프로젝트를 생성한다.</li>
<li>다음 명령어로 React와 일부 라이브러리를 React 18 버전으로 다운그레이드한다.</li>
</ol>
<pre><code>npm install react@18 react-dom@18 @testing-library/jest-dom@5.17.0 @testing-library/react@13.4.0 @testing-library/user-event@13.5.0 web-vitals</code></pre>