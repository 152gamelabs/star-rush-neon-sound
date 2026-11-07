# Star Rush Neon 오디오 연결표

요청 기획·OGG 변환과 실제 게임 이벤트 연결은 별개다. 요청30개 중 시험슬롯9개만 현재 연결됐다. 새21개 역할은 제작 가능 / 게임 연결 예정이다.

| 현재 요청 ID | 호출 | 실제 범위 |
|---|---|---|
| sfx-glass-impact | AddSpark → Impact(false) | 모든 비범퍼와 벽 접촉. 향후 사각 variant0 기준음으로 분리 |
| sfx-bumper-impact | AddSpark → Impact(true) | 분홍 원형 범퍼 |
| sfx-link-charge | Fall/ChargedEdge 및 Capture 끝 → Charge | 연결·재충전·압축 시작 공용 |
| sfx-ignition-blast | ChargeExplosion → Ignite | 첫 중심 점화 |
| sfx-cascade-burst | Chain → Burst | .16초 cascade tick; 파괴 블록과 정확히1:1 아님 |
| sfx-energy-collect | Chain→Collecting → Collect | 수집 시작1회. 입자별·상단도착별 아님 |
| sfx-star-upgrade | Chain 레벨 변화 → Upgrade | 현재 수집 시작과 같은 프레임 |
| sfx-ui-touch | ShellButton/slider commit → Ui | HUD pause/튜토리얼/키보드 일부 미연결 |
| bgm-game-chamber | Awake / musicSource | 타이틀과 인게임 공용 |

## 다음 연결 작업

- AddSpark가 TileKind와 Rules.SquareVariant(tile)를 오디오에 전달한다. variant0은 sfx-glass-impact, 1~5는 cut/octagon/trapezoid/pentagon/shield, Mover와 좌우 Slope는 독립슬롯. 벽(tile==null)도 분리한다. 한 접촉에서 공통음과 고유음 중복 금지.
- 미준비 주황의 Capturing 진입(pulse==null)은 흡수1회. 준비된 점화는 압축 후 폭발. CaptureTime=.24초, 압축=.34~.68초.
- OnMenuChanged에 타이틀/전장 음악 전환. 일시정지 시 곡을 처음부터 재시작하지 않는다. 첫 납품은 완전 루프만.
- 점화준비 false→true, Windup→Falling, 상승, 위험임계 진입, 패배확정, 신기록저장성공, UI취소/오류 조건을 requests.json apply.hook/notes대로 연결한다.
- 광고보상은 실제지급·저장성공 후1회. 광고닫힘만으로 재생하지 않는다. 서비스 미연결이므로 P2.

형태는 ID%6으로 구분한다. 좌우 경사도 회전하므로 음색은 종류 정체성을 나타내며 실제 반사방향을 보장하지 않는다. 여러 제출파일은 비교용이다. 현재 게임은 슬롯당 선택파일1개이며 자동랜덤·세기별다중레이어는 별도 구현이다.

## 외부 테스트 / 출시 적용

신규 출력은 OGG Vorbis다. v0.3.3 이상 OGG 대응 시험판을 사용하고 이전 WAV 전용 v0.3.2와 혼용하지 않는다. 이전 WAV manifest 호환은 유지한다. schemaVersion1, revision, clips[{slot,path,gainDb,pitchSemitones,loop,loopStart,loopEnd}] 계약은 같으며 path는 preview 안의 clips/*.ogg 상대경로다.

60초보다 긴 음악은 v0.3.4 이상 실행판을 사용한다. 음악600초·게임용OGG32MiB, 효과음12초·12MiB 한도를 요청/보드/Unity에서 함께 적용한다. 원본 업로드 한도는256MiB다. 장면 사진·무음 영상은 보드 설명용이며 게임 오디오 어댑터나 자동 동기 재생에 사용하지 않는다.

음량·트림·페이드·피치는 변환본에 반영하고 manifest gain/pitch는0으로 둔다. 음악 지정 반복구간을 자른 파일 전체를 반복하며 인트로+부분루프 엔진으로 설명하지 않는다. 완성된 세대만 교체하고 손상·경로이탈은 거부한다. 실제 플레이 저장과 원본은 보존한다.

8효과음보이스+음악1개, 중요도·쿨다운·duck·음소거·백그라운드 정책을 유지한다. 승인된 OGG를 requests.json target에 임포트하고 실제 이벤트를 연결한 뒤 모바일 지연·메모리·반복·최대폭발중첩·포커스복구를 검증한다. 시험 성공은 정식 applied가 아니며 개발자가 최종검증 뒤 기록한다.
