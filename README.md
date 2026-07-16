# 포켓몬GO CP 조회 디스코드 봇

`/포켓몬 이름:이상해씨` 처럼 입력하면 **레벨 20 / 25, IV 100%(15/15/15) 기준 CP**를
바로 알려주는 디스코드 봇입니다. 레이드·부화·필드리서치는 레벨이 20 또는 25로
고정되고 최소 IV가 10/10/10이라, 이 두 레벨의 CP 예측이 실전에서 가장 유용합니다.

## 폴더 구조
```
pokecp_bot/
├── build_dataset.py   # 종족값(PoGoAPI) + 한글 이름(PokeAPI) 병합 → data/pokemon_kr.json 생성
├── bot.py             # 디스코드 봇 본체 (/포켓몬 명령어)
├── requirements.txt
└── data/
    └── pokemon_kr.json  # build_dataset.py 실행 후 생성됨
```

## 1. 설치
```bash
cd pokecp_bot
pip install -r requirements.txt
```

## 2. 데이터셋 생성 (최초 1회)
```bash
python build_dataset.py
```
- PoGoAPI에서 전체 포켓몬 종족값을 가져오고,
- PokeAPI에서 각 포켓몬의 **공식 한국어 이름**을 가져와 병합합니다.
- 완료되면 `data/pokemon_kr.json`이 생성됩니다. (약 1000종 조회라 몇 분 걸릴 수 있어요)
- 새 세대 포켓몬이 나오면 이 스크립트만 다시 돌리면 됩니다.

## 3. 봇 토큰 설정
프로젝트 루트에 `.env` 파일을 만들고:
```
DISCORD_BOT_TOKEN=여기에_봇_토큰
```
(디스코드 개발자 포털에서 애플리케이션 생성 → Bot 탭에서 토큰 발급,
 OAuth2 URL Generator에서 `bot`, `applications.commands` 스코프 체크해서 서버 초대)

## 4. 실행
```bash
python bot.py
```

## 사용 예시
```
/포켓몬 이름:이상해씨
```
→ 레벨20 IV100% CP, 레벨25 IV100% CP, 그리고 최소치(IV 66%)까지 임베드로 응답합니다.

## 참고
- 종족값 오타/유사 이름 입력 시 자동으로 가장 비슷한 이름을 찾아 알려줍니다.
- 메가진화/알로라/가라르 등 폼 변형은 이번 버전에는 포함하지 않았습니다 (기본 폼만).
  필요하시면 `build_dataset.py`의 폼 필터링 부분을 조정해서 확장할 수 있습니다.
- CP 계산 공식: `CP = floor((공격+IV) × √(방어+IV) × √(체력+IV) × CPM² / 10)`
