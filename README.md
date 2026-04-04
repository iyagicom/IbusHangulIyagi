# IBus Hangul Iyagi

## 개요
기존 한글 입력 방식(조합)을 버리고, 입력 단계를 줄인 완성형 한글 입력기입니다.  
10년 이상 방치된 끝문자 소실 문제를 구조적으로 해결했습니다.

---

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

---

## 특징

- 끝문자 자동 처리
- 첫문자 입력 문제 해결
- 모음 단독 입력 지원
- 빠른 타이핑 최적화

---

## 개발

IYAGI INC
