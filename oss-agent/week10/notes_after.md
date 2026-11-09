작성: Claude Code / 확인: (이름)

# week09 tools.py 함수 정리

## 개요

- KOBIS(영화진흥위원회) 오픈 API의 일별 박스오피스 정보를 가져오는 도구 모음임
- `requests`로 HTTP 요청을 보냄
- API 키는 `secret.py`의 `API_KEY`에서 가져옴
- 두 함수 모두 같은 주소(`BASE_URL`)를 씀
  - `https://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json`

## 함수 1. get_boxoffice_rank_1(date)

- 역할: 그 날짜 박스오피스 1위 영화 제목을 알려줌
- 받는 값: `date` (문자열, 8자리 날짜, 예: `"20260101"`)
- 돌려주는 값: 1위 영화 제목 (문자열, 예: `"오디세이"`)
- 동작 순서
  - `key`, `targetDt`를 넣어 API를 호출함 (제한 시간 10초)
  - 응답에서 `boxOfficeResult` → `dailyBoxOfficeList` 목록을 꺼냄
  - 목록 첫 번째 영화의 `movieNm`(제목)을 돌려줌

## 함수 2. get_movie_audience(date, title)

- 역할: 그 날짜에 특정 영화의 하루 관객 수를 알려줌
- 받는 값
  - `date` (문자열, 8자리 날짜)
  - `title` (문자열, 영화 제목)
- 돌려주는 값
  - 찾으면: 천 단위 쉼표를 붙인 관객 수 (예: `"123,456명"`)
  - 못 찾으면: `"'제목' 은(는) 날짜 순위에 없습니다."` 안내 문장
- 동작 순서
  - 함수 1과 같은 방식으로 API를 호출하고 목록을 꺼냄
  - 목록을 하나씩 돌며 제목이 `title`과 똑같은 영화를 찾음
  - 찾으면 `audiCnt`(관객 수)를 숫자로 바꿔 형식에 맞춰 돌려줌

## 눈에 띄는 점

- `get_movie_audience`에는 설명(docstring)이 없음
- 두 함수에 API 호출 코드가 똑같이 반복됨
- 오류 처리가 없음 (네트워크 오류, 잘못된 날짜, 빈 목록이면 그대로 에러 남)
- 함수 1 설명의 예시 날짜 `"20260000"`은 실제로 없는 날짜임
- 제목이 완전히 같아야만 찾음 (띄어쓰기가 다르면 못 찾음)

## 확인할 것

- 맨 위 "확인: (이름)"에 확인한 사람 이름 넣기
- 실제 API로 두 함수를 한 번씩 실행해 설명과 결과가 맞는지 보기
