# 청취 컨텍스트

현재 게임은 `UnityProject/Assets/Scripts/NeonAudio.cs`의 합성음으로 동작한다. 게임에서 녹음한 플레이 WAV나 외부 구매 음원은 이 폴더에 없다.

`current-synth/`에는 현재 `Tone()` 8종과 `Ambient()`의 합성 함수를 별도 도구로 재현한 참고 WAV가 있다. 모두 22,050 Hz, 모노 PCM16이다. 실제 Unity AudioSource의 gain, pitch, 중복 제한, 음악 duck, 게임 타이밍까지 녹음한 자료는 아니다. 부동소수점 반올림 때문에 Unity 출력과 비트 단위 일치를 보장하지 않는다. 생성 내역·원본 함수 해시·길이·peak는 `current-synth/manifest.json`을 확인한다.

| 파일 | 현재 합성 참고 |
|---|---|
| `00-glass-contact.wav` | 일반 결정 접촉 |
| `01-soft-bumper.wav` | 원형 범퍼 |
| `02-link-charge.wav` | 선 충전 |
| `03-star-ignition.wav` | 점화 한방 |
| `04-energy-arrival.wav` | 에너지 회수 |
| `05-ui-touch.wav` | 메뉴 선택 |
| `06-star-strengthened.wav` | 별 강화 |
| `07-cascade-burst.wav` | 순차 폭발 |
| `08-chamber-ambience.wav` | 12초 공용 앰비언스 |

기존 소리 비교는 해당 WAV를 로컬 플레이어로 듣는다. 보드에 단독 참고음 전환 UI가 없는 경우 새 제출물 대신 이 파일을 제출하지 않는다. scene/loop_over 미리듣기에는 앰비언스만 낮은 음량으로 연결되어 있다.

보드의 `preview.contextFiles`에 연결된 파일만 배경으로 재생한다. 제출물과 컨텍스트를 혼동하지 않으며, 없는 파일을 임의 음악으로 대체하지 않는다. 참고 음원을 포함한 모든 사운드가 첫 재생 전 사용자 조작을 필요로 한다.
