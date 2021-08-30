## 🔥 결과

- 배포한 CloudFront 접근 경로: https://d1raor9vjuxux6.cloudfront.net/
- 개선 전후 성능 측정 결과
    - 개선 전 (S3)
    ![image](https://user-images.githubusercontent.com/32982670/131308749-c38c32ed-9cd0-40bb-8135-091fa912a59b.png)

    - 개선 후 (CloudFront)
    ![image](https://user-images.githubusercontent.com/32982670/131308850-000fe291-f648-4438-8583-72fe080dffce.png)


## ✅ 개선 작업 목록
/* 각 요구사항을 위해 어떤 개선 작업을 진행했는지 적어주세요 
   코드 변경사항으로 확인하기 어려운 CloudFront 설정 사항 등은 리뷰어가 확인할 수 있게 스크린샷이나 적용한 항목들을 적어주면 좋겠지요? 🙂
*/

**1 요청 크기 줄이기**
- [ ]  소스코드 크기 줄이기
- [ ]  이미지 크기 줄이기

**2 필요한 것만 요청하기**
- [ ]  페이지별 리소스 분리

**3 같은 건 매번 새로 요청하지 않기**
- [ ]  CloudFront 캐시 설정
- [ ]  GIPHY의 trending API를 Search 페이지에 들어올 때마다 새로 요청하지 않아야 한다.

**4 최소한의 변경만 일으키기**
- [ ]  검색 결과 > 추가 로드시 추가된 목록만 렌더되어야 한다.
- [ ]  LayoutShift 없이 hover 애니메이션이 일어나야 한다.

## 🧐 공유
/* 작업하면서 든 생각, 질문, 새롭게 학습하거나 시도해본 내용 등등 공유할 사항이 있다면 자유롭게 적어주세요 */
