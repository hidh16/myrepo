# boxoffice — KOBIS 박스오피스 조회 스킬

## 하는 일
특정 날짜의 박스오피스 1위 영화를 물으면 그 영화 제목을 알려 줌 (예: "11월 14일 1위 영화 뭐야?")
특정 날짜에 어떤 영화를 본 관객 수를 물으면 그 수를 알려 줌 (예: "11월 14일에 ○○ 몇 명 봤어?")
날짜는 8자리(예: 20261114)로 바꿔 조회하고, 자료가 없거나 요청이 거절되면 그 안내 문장을 그대로 전함
## 들어 있는 파일
- .claude/skills/boxoffice/SKILL.md
- .claude/skills/boxoffice/scripts/boxoffice.py
- .claude/skills/boxoffice/LICENSE

## 필요한 것
스킬 폴더 말고도 week12 폴더(스크립트를 실행하는 폴더)에 아래 두 파일이 있어야 함
tools.py: KOBIS 에 실제로 요청을 보내는 함수(get_boxoffice_rank_1, get_movie_audience)가 들어 있음
secret.py: 자기 KOBIS 키를 담는 파일. 저장소에는 키가 없으므로 받는 사람이 직접 만들어야 함 (아래 "키" 참고)
스크립트가 지금 폴더에서 tools.py 를 찾으므로, 반드시 week12 폴더에서 실행해야 함
파이썬 3 이 필요함 (만든 사람은 3.12 로 실행함)
패키지(package, 추가로 설치하는 파이썬 묶음)는 requests 하나만 필요함
설치: pip install requests

## 설치
boxoffice 폴더를 week12 폴더 안의 .claude/skills/ 아래에 둠
최종 위치: week12/.claude/skills/boxoffice/
같은 week12 폴더에 tools.py 와 secret.py 를 둠 (위 "필요한 것" 참고)
pip install requests 로 패키지를 설치함
week12 폴더에서 Claude Code 를 실행함
Claude Code 가 .claude/skills/ 안의 스킬을 스스로 찾아 읽음
박스오피스를 물어보면 Claude Code 가 이 스킬을 골라 씀
바로 쓰고 싶으면 /boxoffice 라고 입력해도 됨
스크립트를 실행해도 되는지 물으면 명령을 확인하고 허락함 (아래 "권한" 참고)

## 바깥으로 요청하는 곳
영화진흥위원회 KOBIS 오픈 API(공개 데이터 창구) 한 곳에만 요청함
주소: https://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json
보내는 값: 키(key)와 날짜(targetDt, 8자리)
요청은 tools.py 가 보내고, 10초 안에 답이 없으면 멈춤
인터넷에 연결되지 않으면 "네트워크에 연결하지 못했습니다." 라고 나옴

## 키
이 저장소에는 키가 들어 있지 않음
자기 키를 secret.py 에 넣을 것

## 권한
SKILL.md 에 allowed-tools(미리 허락해 두는 도구 목록) 항목이 없음
따라서 미리 허락된 도구가 없고, Claude Code 의 기본 권한 설정을 그대로 따름
실행할 때 Claude Code 가 아래와 같은 명령을 실행해도 되는지 물음
python .claude/skills/boxoffice/scripts/boxoffice.py rank 날짜
python .claude/skills/boxoffice/scripts/boxoffice.py audience 날짜 "영화제목"
명령 내용을 확인한 뒤 허락하면 됨. 매번 묻지 않게 하려면 이 명령을 허락 목록에 넣으면 됨

## 라이선스
MIT 라이선스(license, 사용 허락 조건)를 따름
누구나 무료로 쓰고, 고치고, 다시 배포할 수 있음
단, 다시 배포할 때 저작권 표시와 라이선스 문장을 함께 넣어야 함
프로그램은 있는 그대로 제공되며, 문제가 생겨도 만든 사람이 책임지지 않음
라이선스 파일 위치: .claude/skills/boxoffice/LICENSE.txt
