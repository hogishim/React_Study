# PWA setting으로 App 런칭

### 📲PWA(Progressive Web App): 웹사이트를 모바일 앱처럼 사용할 수 있게 해주는 기술

[Service worker overview  |  Workbox  |  Chrome for Developers](https://developers.google.com/web/fundamentals/primers/service-workers)

### PWA의 장점

- 스마트폰, 테블릿 배경화면에 웹사이트를 설치할 수 있다
- 오프라인에서도 동작할 수 있다
- 설치 유도 비용이 적다

### PWA 사용

```
 npx create-react-app [project name] --template cra-template-pwa
```

- 프로젝트를 만들때, PWA 구동에 필요한 manifest.json과 service-worker.js를 생성해준다
- 기존 프로젝트를 이용하고 싶다면 새로운 프로젝트를 만든 이후에, 소스 파일을 옮기는 과정을 거친다. 단, index.js파일은 차이점을 찾아서 잘 수정해야한다
- router, redux를 그대로 사용할 수 있다
- PWA 이용이 쉬운 것은 구글의 workbox라는 라이브러리 덕분인데, PWA를 커스터마이징 하고 싶으면 이 workbox의 이용 방법을 익혀야 하는데, 공식 문서가 불친절하게 써있다….

Index.js

```jsx
serviceWorkerRegistration.unregister();
```

- index.js의 하단에 unregister라고 되어있는 부분을 register로 바꿔주면, service-worker.js를 자동으로 만들어준다
- service-worker.js파일은 기존 web 환경에서는 HTML CSS 파일을 서버를 통해 받아온 이후에 브라우저에 보여주지만, 앱은 하드에 이미 아이콘, 데이터 같은 것들을 설치해놓고 오프라인 상태에서도 확인할 수 있도록 해야한다
- service-worker.js파일이 자동으로 생성 되면서, caching처럼 서버에 요청 없이도 일반적인 앱과 마찬가지로 로고, 데이터 같은 것들을 확인할 수 있다

manifest.json

```jsx
{
  "version" : "여러분앱의 버전.. 예를 들면 1.12 이런거",
  "short_name" : "설치후 앱런처나 바탕화면에 표시할 짧은 12자 이름",
  "name" : "기본이름",
  "icons" : { 여러가지 사이즈별 아이콘 이미지 경로 },
  "start_url" : "앱아이콘 눌렀을 시 보여줄 메인페이지 경로",
  "display" : "standalone 아니면 fullscreen",
  "background_color" : "앱 처음 실행시 잠깐 뜨는 splashscreen의 배경색",
  "theme_color" : "상단 탭색상 등 원하는 테마색상",
}
```

- 앱의 아이콘, 이름, 테마색 등을 결정할 수 있는 부분이다
- 각 내용에 대한 설정을 진행할 수 있다

### 특정 파일을 caching대상에서 제외하기: 정규식 형식으로 지정

```jsx
new WorkboxWebpackPlugin.GenerateSW({
    clientsClaim: true,
    exclude: [/\.map$/, /asset-manifest\.json$/],
}) 
```

- 이전 버전 코드

```jsx
new WorkboxWebpackPlugin.InjectManifest({
    swSrc,
    dontCacheBustURLsMatching: /\.[0-9a-f]{8}\./,
     exclude: [/\.map$/, /asset-manifest\.json$/, /index\.html/], 
```

- 신버전 코드. 항목중에서 exclude항목이 어떤 파일을 caching하지 않을 것인지 결정하는 부분이다
- 단순하게 파일을 하나 지정하는 것이 아니라, 정규식으로 .css로 끝나는 파일, a로 시작하는 파일 같이 지정할 수 잇다
- 사실 이 부분은 그렇게까지 필요한 내용은 아니다

### PWA 디버깅 하기

- VS Code 익스텐션중에 live server를 설치하고, build폴더를 에디터로 오픈한 이후에 index.html을 우클릭하고, liver server로 띄우기를 누르면 된다
- 크롬에서 개발자 도구를 켜고, Application 탭에서 PWA에 관련된 내용을 확인할 수 있다
    - manifest: manifest.json관련 내용을 확인할 수 있다
    - service worker 메뉴: service-worker파일이 있는지, 오프라인에서 잘 동작하는지 테스트할 수 있고, 푸시 알림기능도 확인할 수 있다
    - cache storage: service-worker덕분에 하드에 설치된 CSS, HTML, JS 파일을 확인할 수 있다