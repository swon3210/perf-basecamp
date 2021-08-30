## 🔥 결과

배포한 CloudFront 접근 경로: https://d2gwrmvddjdl0x.cloudfront.net/

<br>

### 개선 전후 성능 측정 결과

- lighthouse 점수 : **53 -> 100**

![image](https://user-images.githubusercontent.com/32982670/131308850-000fe291-f648-4438-8583-72fe080dffce.png)

- 프랑스 파리에서 Fast 3G 환경으로 접속했을 때 
  - 첫번째 접속시 LCP : **81.691s -> 2.813s**
  - 두번째 접속시 LCP : **2.183s -> 0.285s**

![image](https://user-images.githubusercontent.com/32982670/131311526-11df4656-566e-4af7-bdc6-c8bc1920354b.png)

- Home 페이지에서 불러오는 스크립트 리소스 크기: **797KB -> 51.5KB**

![image](https://user-images.githubusercontent.com/32982670/131313611-b68155b4-e207-43e1-98ea-4d0566042783.png)

- 히어로 이미지 크기: **10.1MB -> 114KB**


## ✅ 개선 작업 목록
/* 각 요구사항을 위해 어떤 개선 작업을 진행했는지 적어주세요 
   코드 변경사항으로 확인하기 어려운 CloudFront 설정 사항 등은 리뷰어가 확인할 수 있게 스크린샷이나 적용한 항목들을 적어주면 좋겠지요? 🙂
*/

**1 요청 크기 줄이기**
- [x] 소스코드 크기 줄이기
  - HTML, CSS, Javascript 압축 및 난독화
  - CSS 코드를 자바스크립트 번들에 포함시키지 않고 별도 파일로 분리(번들과 따로 요청되도록 만듬)
  - CloudFront 에서 자원을 전달할 때 gzip(구형 브라우저), brotli(신형 브라우저) 형식으로 압축시킨채 전달
    ![image](https://user-images.githubusercontent.com/32982670/131369320-1973acc8-3e82-492d-b635-1bb7cd7fdccc.png)
    ![image](https://user-images.githubusercontent.com/32982670/131369384-1a279ff1-6a40-4838-9dec-4084423f83e6.png)

- [x] 필요없는 요청 없애기
  - 폰트 다운로드를 위해 불필요하게 다운로드 했던 font-face 정보들이 담긴 CSS 파일을 요청하지 않도록 수정
  
- [x] 이미지 크기 줄이기
  - hero.png 파일 최적화
    - png 이미지 파일을 webp 이미지 파일로 변환
    - picture 태그를 통해 webp 이미지 형식을 지원하지 않을 경우 리사이징 된 png 파일을 사용하도록 수정
    - 모바일, 타블렛 기기의 너비를 가진 기기나 브라우저 창 크기에서 접속시 기존 hero.webp 보다 작게 리사이징 된 이미지 파일을 로드하도록 설정
  - gif 파일 최적화 수행 (find.gif: 1.89MB -> 973KB, trending.gif -> 807KB)
  - webpack 플러그인을 통해 이미지 압축 수행

**2 필요한 것만 요청하기**
- [x] 페이지별 리소스 분리
  - 동적 import 및 React.lazy & React.Suspense 활용
  - tree-shaking이 적용되지 않는 react-icons 패키지를 tree-shaking이 적용되는 @react-icons/all-files 패키지로 변경(동동 PR 참고)
  - react-icon 을 url-loader 를 통해 inline 으로 컴포넌트에 삽입되도록 설정

**3 같은 건 매번 새로 요청하지 않기**
- [x] CloudFront 캐시 설정
  - 최대한 자원 자체의 캐싱 정책을 따르되 Cache-Control 헤더 속성이 없어서 캐싱 정책이 적용되지 않은 파일엔 24시간의 TTL이 적용되도록 설정
    ![image](https://user-images.githubusercontent.com/32982670/131369921-e1efae85-b106-412b-b3ad-fe321690e6b4.png)

- [x] GIPHY의 trending API를 Search 페이지에 들어올 때마다 새로 요청하지 않아야 한다.
  - gif 정보들을 담는 리스트를 전역 상태로 전환(이전 검색 결과를 페이지 컴포넌트가 언마운트 된 이후에도 유지하도록 수정)
  - gif 정보들이 담긴 상태값인 리스트가 비어 있지 않으면 새롭게 gif 리스트를 요청하지 않도록 수정

**4 중요한 자원은 미리 다운로드 받기**
- [x] hero.webp, JosefinSans-Italic.woff2 파일에 preload 적용
  ![image](https://user-images.githubusercontent.com/32982670/131369627-7b24555c-494b-41aa-a183-2a7ca8cdd2ac.png)
- [x] Home 페이지 LCP 이후 Search 페이지에 필요한 chunk 자바스크립트 파일 미리 다운로드

**5 최소한의 변경만 일으키기**
- [x] 검색 결과 > 추가 로드시 추가된 목록만 렌더되어야 한다.
  - 리스트 상태에 따라 동적으로 렌더링 되는 GifItem 컴포넌트에 React.Memo 적용
- [x] LayoutShift 없이 hover 애니메이션이 일어나야 한다.
  - CSS 속성 중 리플로우를 트리거하는 top 속성을 변경하는 애니메이션이 transform: translateY 를 변경하도록 수정

**6 기타 최적화**

- [x] 필요한 웹폰트 서브셋만을 다운로드
- [x] font-display 를 fallback 으로 설정하여 이용자가 fallback 폰트 텍스트를 보지 않고 바로 웹폰트 텍스트를 보도록 함

## 🧐 공유


