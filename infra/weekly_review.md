1. 서비스 정의 다시한번 확인하기.
ㄴ 누가 쓰는가, 무엇을 위해 쓰는가 
누가 쓰는가 -> 관심있어 하는 매물의 적정 시세를 검증이 필요한 사용자가 사용한다. 
무엇을 위해 쓰는가 -> 합리적인 구매가격 의사결정을 위해 또는 허위 매물을 사전 방지하기 위해 사용된다

ㄴ 서비스 한문장으로 정의한것 다시한번 살펴보기

2. 서비스 정의에 맞게 잘 구현된건지 검토하고, 개선이 필요한것은 개선하기

3. 새로운 사람이 이 프로젝트를 받아서 실행 배포할 때 필요한 문서 만들기

실행 순서
=========
1) pip install -r requirements.txt
2) python 01_crawl_and_preprocess.py
   -> cars_processed.csv 생성 (이미 있으면 새 매물을 덧붙여 누적)
3) python 02_price_regression.py
   -> price_model.joblib(예측 모델), recommend_pool.csv(추천 후보 + 적정가) 생성
4) python app.py
   -> http://localhost:8000 (사용자 화면), http://localhost:8000/docs (API 문서)
   - POST /recommend : 예산/조건 -> 추천 매물 목록
   - POST /predict   : 차량 정보 -> 신차대비가격(%) 예측

컨테이너 / 배포
===============
    docker compose up -d --build              # http://localhost:8000
    docker compose run --rm pipeline          # 재학습 파이프라인 1회 (app 과 볼륨 공유, 재시작 불필요)
    python smoke_test.py [주소]               # 화면/예측/추천/클릭 동작 확인
- AWS(EC2 + docker compose) 배포 구성, 자동 배포(GitHub Actions), 운영/롤백 방법은 infra/README.md

4. 다른 팀원이 해당 문서를 보고 배포까지 수행하기

5. 새로운 배포환경에서 서비스 실행하고 잘 동작하는지 확인하기

ㄴ 배포과정에서 직접 수행하거나 대응해야 할 것이 있다면 수행하고, 문서에도 업데이트 하기

6. 변경사항 PR 만들고 팀원간 리뷰 수행하기
   - PR 본문에 변경사항에 대한 설명 포함하기 
-변경사항 없음-
7. (시간이 있다면) 다음주 발표시 어떻게 보여줄지 시나리오 준비하기

8. Q & A 에 PR 주소 남기기