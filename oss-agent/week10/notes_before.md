# week09 `tools.py` 함수 정리 (수정 전)

## 개요

- KOBIS(영화진흥위원회) 오픈 API의 **일별 박스오피스** 엔드포인트를 호출하는 도구 모음
- `requests`로 HTTP 요청을 보내고, API 키는 `secret.py`의 `API_KEY`에서 가져온다
- 공통 요청 URL: `BASE_URL` = `https://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json`

## 함수 목록

### 1. `get_boxoffice_rank_1(date)`

| 항목 | 내용 |
|---|---|
| 역할 | 주어진 날짜의 박스오피스 **1위 영화 제목**을 반환 |
| 입력 | `date` (str) – 8자리 날짜, 예: `"20260101"` |
| 출력 | (str) 1위 영화 제목, 예: `"오디세이"` |

동작 순서:
1. `key`, `targetDt` 파라미터로 API 호출 (timeout 10초)
2. 응답 JSON에서 `boxOfficeResult` → `dailyBoxOfficeList` 꺼내기
3. 리스트 첫 번째 항목의 `movieNm` 반환

### 2. `get_movie_audience(date, title)`

| 항목 | 내용 |
|---|---|
| 역할 | 주어진 날짜에 특정 영화의 **일일 관객 수**를 반환 |
| 입력 | `date` (str) – 8자리 날짜 / `title` (str) – 영화 제목 |
| 출력 | (str) 천 단위 쉼표가 붙은 관객 수, 예: `"123,456명"` <br> 순위에 없으면: `"'제목' 은(는) 날짜 순위에 없습니다."` |

동작 순서:
1. 같은 방식으로 API 호출 후 `dailyBoxOfficeList` 꺼내기
2. 리스트를 돌며 `movieNm`이 `title`과 **정확히 일치**하는 영화 찾기
3. 찾으면 `audiCnt`를 정수로 바꿔 `"{관객수:,}명"` 형식으로 반환
4. 못 찾으면 안내 문자열 반환

## 눈에 띄는 점 (개선 후보)

- `get_movie_audience`에는 docstring이 없다 (`get_boxoffice_rank_1`에는 있음)
- 두 함수가 API 호출 + 리스트 추출 코드를 **중복**해서 가지고 있다
- 예외 처리가 없다: 네트워크 오류, 잘못된 날짜, 빈 리스트(`movies[0]`에서 `IndexError`) 등에서 그대로 에러가 난다
- `get_boxoffice_rank_1`의 docstring 예시 날짜 `"20260000"`은 실제로 존재하지 않는 날짜다
- 제목 비교가 완전 일치라서 띄어쓰기·부분 일치는 찾지 못한다
