# 시은의 공휴일 안내

한국천문연구원 특일 정보 Open API를 활용하는 6주차 실습 사이트입니다.

- 저장소: https://github.com/202300442-se/open-api-site
- 공개 사이트: https://202300442-se.github.io/open-api-site/
- 현재 상태(2026-10-07): 실제 API 조회 및 GitHub Pages 배포 성공. 공개 URL HTTP 200 확인. 2026년 10월 공휴일 3건 표시. 자동 검사 24개 통과, 모바일 390px·PC 1440px 가로 넘침 없음.
- 출처: https://www.data.go.kr/data/15012690/openapi.do
- 수업 팁은 실습자료 sample.csv의 예시 콘텐츠입니다.

## 동작 방식

GitHub Actions → Secret에서 인증키 읽기 → API 조회 → HTML·JSON 생성 → GitHub Pages 배포.
방문자의 브라우저는 생성된 결과만 읽습니다. 인증키는 HTML과 JSON에 포함되지 않습니다.

## GitHub 설정 (설정 완료 / 재현 방법)

1. 202300442-se 계정에서 Public 저장소 open-api-site를 만듭니다.
2. 이 폴더의 파일을 저장소 최상위에 올립니다. open-api-site 폴더 자체를 중첩 업로드하지 않습니다.
3. Settings → Pages → Build and deployment → Source를 GitHub Actions로 선택합니다.
4. Settings → Secrets and variables → Actions → Secrets → New repository secret에서 이름 DATA_GO_KR_KEY와 일반 인증키(Decoding)를 등록합니다.
5. .github/workflows/open_api_site.yml을 마지막으로 올리고 main 브랜치에 Commit changes 합니다.
6. Actions의 Open API Site에서 build와 deploy 성공을 확인한 뒤 Pages 주소를 엽니다.

GitHub 웹에서 숨김 폴더를 올리기 어렵다면 Add file → Create new file에서
`.github/workflows/open_api_site.yml`을 파일 이름으로 입력하고 제공된 YAML 내용을 붙여 넣습니다.
`workflows`는 반드시 복수형입니다. 저장소 이름 open-api-site는 파일 경로에 다시 넣지 않습니다.

## 로컬 검증

Python 3.13 기준, 별도 외부 라이브러리가 필요하지 않습니다.

```sh
python -m unittest discover -s tests -v
python main.py --api-fixture fixtures/special_days_sample.json --output _site/index.html
```

위 두 번째 명령은 실제 API가 아닌 모의 응답을 사용합니다. 생성된 `_site/index.html`을 브라우저로 열어 볼 수 있습니다.
실제 API 조회는 GitHub Secret 설정 후 Actions에서 수행합니다. 키를 파일이나 명령문에 직접 넣지 않습니다.

## 수정과 갱신

- 제목·소개·출처: site.json
- 수업 팁: sample.csv
- 페이지 구성: main.py
- API 처리: data_go_kr.py, special_days.py
- 배포 절차: .github/workflows/open_api_site.yml
- 선택 사항: 공개된 Google Sheet CSV 주소를 Actions Variables의 SHEET_CSV_URL로 등록합니다. 미등록 시 sample.csv를 사용합니다.
- main에 커밋하거나 Actions → Run workflow를 누르면 다시 조회하고 배포합니다.
- 현재 매일 자동 갱신은 꺼져 있습니다. 수동 배포 성공 후 YAML의 schedule 주석을 해제할 수 있습니다.
- 사이트에 표시하는 달은 마지막 빌드 시점의 한국시간 기준입니다.

## 오류 확인

인증키가 없거나 API에 오류가 있으면 build가 실패하고 이전 배포가 유지됩니다.
공공데이터포털에서 한국천문연구원_특일 정보 활용 신청 상태를 확인하세요.
Actions 로그의 ServiceKey는 ***로 숨겨져야 합니다. 실제 키를 README, 코드, 이슈, 커밋에 넣지 마세요.

원본 실습자료 상세 설명은 CLASS_README.md를 참고하세요.
