# IbusHangulIyagi 노린(Norin)

노린(Norin)은 한글(hangul)을 대신하는 새로운 이름으로,  
기존 입력 방식의 한계를 넘기 위해 만들어진 **완성형 한글 입력기**입니다.

IBus 프레임워크 기반 두벌식 입력기 엔진으로,  
Wayland + GNOME 환경에서 동작하며 외부 한글 라이브러리 없이  
자체 구현한 입력 엔진을 사용합니다.

👉 **ㅇ을 치지 않아도 한글이 완성됩니다.**

---

## 개발 배경

기존 IBus 한글 입력기(ibus-hangul 등)는 오랫동안 해결되지 않은 문제가 있습니다.

- 조합 중 마지막 글자가 사라지는 **끝문자 버그**
- 입력 상태에 따라 발생하는 **첫문자/조합 꼬임**

특히 포커스 이동이나 마우스 클릭 시  
마지막 글자가 사라지는 문제는 10년 이상 유지된 고질적인 문제입니다.

노린(Norin)은 이 문제를 근본적으로 해결하기 위해  
기존 코드를 수정하는 방식이 아닌, **엔진을 처음부터 새로 설계**했습니다.

👉 결과:  
**어떤 상황에서도 글자가 사라지지 않습니다.**

---

## 핵심 특징

노린은 기존 입력기와 **구조 자체가 다릅니다.**

---

### ⚡ 완성형 직출력 입력

기존 입력기는 모든 글자를 “조합”으로 처리합니다.

- `ㅇ + ㅏ → 아`
- 항상 조합 상태를 거침

노린은 불필요한 조합 과정을 제거했습니다.

- `ㅏ → 아`
- `ㅗ → 오`
- `ㅠ → 유`

👉 초성 `ㅇ` 입력 없이 **즉시 완성 글자 출력**

---

### 🚀 입력 속도

기존 입력기:
- 2단계 입력 (`ㅇ + 모음`)

노린:
- 1단계 입력 (모음 단독)

👉 입력 횟수 감소  
👉 상태 전환 제거  
👉 타이핑 흐름 유지  

👉 **체감 속도가 확연히 달라집니다**

---

### 🧠 구조적 안정성

기존 입력기:
- 복잡한 조합 상태
- 다양한 edge case 존재
- 첫문자 / 끝문자 문제 발생

노린:
- 단순한 상태 구조
- 직접 출력 중심
- 조합 의존도 최소화

👉 **버그가 발생하지 않는 구조**

---

### 🎯 설계 철학

노린은 “표준 입력기”가 아니라  
**실제 타이핑 효율을 위한 입력기**입니다.

- 불필요한 입력 제거
- 상태 최소화
- 빠르고 끊김 없는 흐름

👉 **속도와 안정성을 위해 일부 기존 규칙을 과감히 제거**

---

### ⚠️ 의도된 차이

- 모음 입력 시 항상 완성 글자로 변환됨
- 순수 모음(ㅏ, ㅗ, ㅠ 등) 단독 입력은 제한됨

👉 이는 버그가 아니라  
👉 **속도와 단순화를 위한 설계 선택**

---

## 지원 환경

- Ubuntu 22.04 / 24.04 이상 (GNOME + Wayland)
- IBus 1.5 이상
- X11 환경에서도 동작 가능

---

## 기능

- 두벌식 한글 입력 (자체 구현 엔진, libhangul 불필요)
- 한/영 전환:
  - 한/영 키
  - Shift + Space
  - 우측 Alt
- 쌍자음 입력 (Shift + 자음)
- CapsLock / Shift 정확한 구분
- 터미널 자동 감지 → preedit 보조 표시
- 포커스 이탈 시 자동 commit (끝문자 소실 없음)

---

## 설치

### 1. IBus 설치

```bash
sudo apt install ibus


## 설치 및 설정

### 확인
ibus version

### 2. 의존성 설치
sudo apt install libibus-1.0-5 libx11-6

### 3. 파일 다운로드
- ibus-engine-hanguliyagi
- hanguliyagi.xml

### 4. 설치
chmod +x ibus-engine-hanguliyagi
sudo cp ibus-engine-hanguliyagi /usr/local/libexec/
sudo cp hanguliyagi.xml /usr/share/ibus/component/

### 5. IBus 재시작
ibus restart

### 6. 엔진 활성화
ibus engine hanguliyagi
gsettings set org.gnome.desktop.input-sources sources "[('ibus', 'hanguliyagi')]"

### 7. 확인
ibus engine

---

## GNOME 설정 주의

GNOME 입력 소스와 IBus는 별개입니다.

- 한국어 입력 소스 제거
- 영어만 유지

👉 한/영 전환은 IBus 엔진 내부에서 처리됩니다.

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

---

## 알려진 제한

환경 | 상태
---|---
Chrome (Wayland) | 완벽
GNOME 텍스트 편집기 | 완벽
GNOME Terminal | 정상 동작
IDLE (Tk) | preedit 표시 불가
```
---

### 개발: IYAGI INC
