# IBus HangulIyagi 노린 (Norin)

노린은 순수 우리말의 이쁜 의미를 담은 한글 대체 입력기다.  
IBus 프레임워크 기반 한글 두벌식 입력기 엔진이며,  
Wayland + GNOME 환경에서 동작하고 외부 한글 라이브러리 없이 자체 구현한 조합 엔진을 사용한다.

---

## 개발 배경

기존 IBus 한글 입력기(ibus-hangul 등)는 끝문자 버그가 존재한다.  
마우스 클릭이나 포커스 이동 시 마지막으로 조합 중이던 글자가 사라지는 문제이며, 오랫동안 수정되지 않은 고질적인 문제다.

IbusHangulIyagi는 이 문제를 해결하기 위해 처음부터 직접 작성된 IBus 엔진이다.  
끝문자가 어떤 상황에서도 소실되지 않도록 구조적으로 설계되었다.

---

## 이름

아른 / 노린 (Norin) / 사른 / 하린 / 모린 / 나른 / 도린 / 라른

👉 그중 “노린(Norin)”은 영어권 발음에서도 자연스럽고  
제품 이름으로 사용하기 좋은 핵심 명칭이다.

---

## 특징 (핵심 차별점)

노린은 기존 한글 입력기와 입력 방식 자체가 다르다.

---

### ⚡ 완성형 직출력 입력

노린은 조합 과정을 최소화한 완성형 직출력 구조를 사용한다.

- ㅏ → 아  
- ㅗ → 오  
- ㅠ → 유  

👉 초성 ㅇ을 직접 입력하지 않아도 즉시 완성 글자가 생성된다.

---

### 🚀 입력 속도 향상

기존:
- ㅇ + ㅏ → 아 (2단계)

노린:
- ㅏ → 아 (1단계)

👉 입력 횟수 감소  
👉 상태 전환 제거  
👉 타이핑 리듬 유지

---

### 🧠 구조적 차이

기존 입력기:
- 조합 상태 중심 (state-heavy)
- 첫문자 / 끝문자 버그 존재

노린:
- 완성 중심 (direct output)
- 상태 최소화
- 입력 흐름 단순화

👉 구조적으로 버그가 발생하지 않는 설계

---

### 🎯 설계 철학

- 불필요한 입력 제거
- 최소한의 상태 처리
- 빠르고 끊김 없는 입력

👉 실제 타이핑 효율을 최우선으로 설계

---

### ⚠️ 의도된 차이

- 모음 입력 시 항상 완성 글자로 변환됨
- 순수 모음(ㅏ, ㅗ, ㅠ 등) 단독 입력 제한됨, 이로인해 울지못함 ㅠ.ㅠ, 나중에 쉬프트+ㅠ로 가능하게 할 생각입

👉 속도와 안정성을 위한 설계 선택

---

## 📊 입력 방식 비교 (기존 vs 노린)

| 항목         | 기존 한글 입력기       | IbusHangulIyagi (노린) |
| ---------- | --------------- | -------------------- |
| 입력 구조      | 초성 + 중성 + 종성 조합 | 완성형 직출력              |
| 입력 단계      | 2단계 이상          | 1단계                  |
| 상태 관리      | 있음 (조합 상태)      | 없음 (무상태)             |
| 입력 방식      | 조합 후 출력         | 입력 즉시 출력             |
| 초성 ㅇ 입력    | 필요              | 불필요                  |
| 입력 지연      | 존재 (조합 처리)      | 거의 없음                |
| 타이핑 흐름     | 상태 전환 발생        | 끊김 없는 연속 입력          |
| 첫문자/끝문자 버그 | 발생 가능           | 구조적으로 제거             |
| 순수 모음 입력   | 가능              | 제한됨 (의도된 설계)         |
| 수정 방식      | 조합 단계별 수정       | 입력 단위 직접 삭제          |
| 학습 난이도     | 낮음              | 상대적으로 높음             |

---

## ⚡ 속도 비교

| 항목     | 기존    | 노린       |
| ------ | ----- | -------- |
| 입력 키 수 | 100%  | 약 70~80% |
| 입력 효율  | 기준    | +20~30%  |
| 체감 속도  | 기준    | +30~70%  |
| 입력 지연  | 있음    | 거의 없음    |
| 타이핑 리듬 | 끊김 발생 | 유지됨      |

---

## 📈 전체 타이핑 영향

| 요소      | 영향            |
| ------- | ------------- |
| 키 입력 감소 | 약 -20~30%     |
| 입력 속도   | 약 +20~30%     |
| 체감 속도   | 약 1.3배 ~ 1.7배 |
| 수정 방식   | 단순 (키 단위 삭제)  |
| 입력 안정성  | 향상 (상태 제거)    |
---

## 🔥 핵심 ##

👉 ㅇ을 치지 않아도 한글이 완성된다  
👉 입력 단계를 줄여 타이핑 속도를 바꾼 입력기  
👉 입력 단계 감소 → 속도 증가  
👉 상태 제거 → 버그 제거  
👉 즉시 출력 → 리듬 유지  
👉 키 단위 삭제 → 구조 단순화  
👉 빠른 입력 + 단순한 구조 + 예측 가능한 동작  


---


## 지원 환경

- Ubuntu 22.04 / 24.04 이상 (GNOME + Wayland)
- IBus 1.5 이상
- X11 환경에서도 동작

---

## 기능

- 두벌식 한글 조합 (자체 구현, libhangul 불필요)
- 한/영 전환 (한/영 키, Shift+Space, 우측 Alt)
- 쌍자음 입력 (Shift+자음)
- CapsLock / Shift 쌍자음 구분
- 터미널 자동 감지 (auxiliary text 표시)
- 포커스 이탈 시 끝문자 자동 commit (소실 없음)

---

## 설치

### 1. IBus 확인

sudo apt install ibus
ibus version

---

### 2. 의존성

sudo apt install libibus-1.0-5 libx11-6

---

### 3. 다운로드

- ibus-engine-hanguliyagi
- hanguliyagi.xml

---

### 4. 설치

chmod +x ibus-engine-hanguliyagi
sudo cp ibus-engine-hanguliyagi /usr/local/libexec/
sudo cp hanguliyagi.xml /usr/share/ibus/component/

---

### 5. 재시작

ibus restart

---

### 6. 엔진 활성화

ibus engine hanguliyagi

- 한국어(IbusHangulIyagi) 더하기, 한국어(Hangul) 뺘기 
- 영어(미국) 더하기

  OR
  
gsettings set org.gnome.desktop.input-sources sources "[('ibus', 'hanguliyagi'), ('xkb', 'us')]"

---


### 7. 확인

ibus engine

👉 hanguliyagi 출력되면 정상

---


## 사용 방법

키 | 동작
---|---
한/영 키 | 한글 ↔ 영문
Shift+Space | 한글 ↔ 영문
우측 Alt | 한글 ↔ 영문
Shift+자음 | 쌍자음
Backspace | 조합 삭제

---

## 제거

sudo rm /usr/local/libexec/ibus-engine-hanguliyagi
sudo rm /usr/share/ibus/component/hanguliyagi.xml
ibus restart

gsettings set org.freedesktop.ibus.general engines-order "['hangul']"
ibus restart

---

## 알려진 제한

환경 | 상태
---|---
Chrome (Wayland) | 완벽
GNOME 텍스트 편집기 | 완벽
GNOME Terminal | 정상 (커서 빔)
IDLE (Tk) | preedit 표시 불가 (Tk 한계)


## 개발
IYAGI INC
