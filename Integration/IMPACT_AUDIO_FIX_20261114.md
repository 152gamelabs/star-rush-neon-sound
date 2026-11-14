# 결정 타격음 분리 — 2026-11-14

## 원인과 수정
기존 Fall → AddSpark → Impact(strength, bool bumper)는 벽과 모든 비범퍼 결정을 슬롯 0으로 보냈다. Impact의 Pitch()는 5단계 피치를 순환했다.
현재는 실제 TileKind와 렌더/물리에 쓰는 Rules.SquareVariant(tile)를 사용한다. 충돌마다 AudioSource.pitch를 고정값(기본 1)으로 설정한다. 미지정 모양은 syntheticEffects의 원래 기본음을 쓰며 사각 preview로 대체하지 않는다. null clip은 재생하지 않는다.
폭발 등 별도 효과의 피치 연출은 이번 범위에서 변경하지 않았다. 사운드보드 pitchSemitones는 기존대로 OGG 변환에 구워지며 런타임에서 중복 적용하지 않는다.

## 슬롯
|모양/접촉|슬롯|
|---|---|
|사각, SquareVariant 0|sfx-glass-impact|
|육각 절삭, variant 1|sfx-crystal-cut|
|팔각, variant 2 및 LegacyOctagon|sfx-crystal-octagon|
|사다리꼴, variant 3|sfx-crystal-trapezoid|
|오각, variant 4|sfx-crystal-pentagon|
|방패형, variant 5|sfx-crystal-shield|
|범퍼|sfx-bumper-impact|
|가로/세로 이동 마름모|sfx-mover-impact|
|좌/우 경사|sfx-slope-left-impact / sfx-slope-right-impact|
|점화 블록 접촉|sfx-orange-absorb|
|벽|sfx-wall-impact|

기존 18개 슬롯의 인덱스는 유지하고 10개를 뒤에 추가했다. 기존 요청 ID와 제출 파일을 유지했다. 루트 및 Development의 soundboard.project.json에 같은 ID를 등록했다. 로컬 테스트 게임 경로를 수정 빌드로 연결했다. GitHub 릴리스는 아직 갱신하지 않았다.

## 검증
Unity 6000.3.27f1 배치 컴파일/Windows 빌드: 오류 0, 경고 0.
분리된 저장 경로의 실행판에서 -neonUiSmoke -neonImpactAudio 검증 통과:
- 사각만 override: 실제 AudioSource clip은 사각만 Sound preview, 다른 5모양은 Glass contact 기본 합성음.
- 사각/육각 별도 음원과 사각 교체: 육각 AudioClip 그대로 유지.
- AudioSource pitch를 1.7로 오염시킨 뒤 접촉 재생: 1.0으로 재설정.
- 같은 사각 반복: 1.0 고정; 명시적 1.12 설정 유지.
- 벽/범퍼 슬롯 매핑 검사.
로그: ../Verification이 아닌 게임 루트 Verification/ImpactAudioQA.log 및 WindowsImpactAudio-20261114.log.
테스트 음원은 구별되는 주파수의 합성 WAV다. 디자이너 원본을 변경하지 않았다.
에디터 Play 수동 청취, 디자이너 외장하드에서 OGG 왕복 테스트와 원격 배포는 미실시다.

## 수정 파일
- UnityProject/Assets/Scripts/NeonAudio.cs
- UnityProject/Assets/Scripts/NeonAudioPreview.cs
- UnityProject/Assets/Scripts/NeonGame.cs
- UnityProject/Assets/Scripts/NeonImpactAudioSmoke.cs 및 .meta
- UnityProject/Assets/Scripts/NeonUiSmoke.cs
- soundboard.project.json (Development 및 게임 루트)

기존 진행 저장·음원 제출·preview manifest는 수정하지 않았다. 변경 전 백업은 게임 루트 Backups/AudioImpact-20261114.
