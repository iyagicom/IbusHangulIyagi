# IbusHangulIyagi

IBus 프레임워크 기반 한글 두벌식 입력기 엔진.  
Wayland + GNOME 환경에서 동작하며, 외부 한글 라이브러리 없이 자체 구현한 조합 엔진을 사용합니다.

**개발:** IYAGI INC

---

## 개발 배경

기존 IBus 한글 입력기(ibus-hangul 등)는 **끝문자 버그**가 있습니다.  
마우스 클릭이나 포커스 이동 시 마지막으로 조합 중이던 글자가 사라지는 문제로,  
오랫동안 수정되지 않은 고질적인 버그입니다.

IbusHangulIyagi는 이 끝문자 버그를 해결하기 위해 처음부터 직접 작성한 IBus 엔진입니다.  
끝문자가 어떤 상황에서도 소실되지 않도록 구현했습니다.

---

## 지원 환경

- Ubuntu 22.04 / 24.04 이상 (GNOME + Wayland)
- IBus 1.5 이상
- X11 환경에서도 동작하나, Wayland GNOME이 주 대상

---

## 기능

- 두벌식 한글 조합 (자체 구현, libhangul 불필요)
- 한/영 전환: **한/영 키**, **Shift+Space**, **우측 Alt**
- 쌍자음 입력 (Shift+자음)
- CapsLock과 Shift 쌍자음 정확히 구분
- 터미널 자동 감지 → auxiliary text(팝업) 방식으로 preedit 표시
- 포커스 이탈 시 끝문자 자동 commit (끝문자 소실 버그 없음)

---

## 설치

### 1. IBus 설치 확인

IBus가 없으면 먼저 설치합니다.

```bash
sudo apt install ibus
```

Ubuntu GNOME은 기본적으로 IBus가 설치되어 있습니다.  
설치 여부 확인:
```bash
ibus version
```

### 2. 런타임 의존성 설치

```bash
sudo apt install libibus-1.0-5 libx11-6
```

### 3. 파일 다운로드

릴리즈 페이지에서 아래 두 파일을 다운로드합니다:

- `ibus-engine-hanguliyagi` — 실행 파일
- `hanguliyagi.xml` — IBus 컴포넌트 등록 파일

### 4. 바이너리 및 XML 설치

```bash
# 실행 권한 부여
chmod +x ibus-engine-hanguliyagi

# 바이너리를 시스템 경로에 복사
sudo cp ibus-engine-hanguliyagi /usr/local/libexec/

# IBus가 엔진을 인식할 수 있도록 XML 등록 (최초 1회)
sudo cp hanguliyagi.xml /usr/share/ibus/component/
```

### 5. IBus 재시작

```bash
ibus restart
```

### 6. 엔진 활성화 (gsettings 등록)

IBus는 설치만으로 엔진이 자동 활성화되지 않습니다.  
아래 명령으로 현재 세션 적용과 재부팅 후 자동 유지를 모두 설정합니다.

```bash
# 현재 세션에 즉시 적용
ibus engine hanguliyagi

# 재부팅 후에도 자동으로 이 엔진 사용
gsettings set org.freedesktop.ibus.general preload-engines "['hanguliyagi']"
gsettings set org.freedesktop.ibus.general engines-order "['hanguliyagi']"
```

| gsettings 키 | 역할 |
|---|---|
| `preload-engines` | IBus 데몬 시작 시 이 엔진을 미리 로드 |
| `engines-order` | 여러 엔진 중 사용 순서 지정 (첫 번째가 기본값) |

> 다른 한글 엔진(ibus-hangul 등)과 함께 쓸 경우:  
> `"['hanguliyagi', 'hangul']"` 형식으로 목록에 추가합니다.

### 7. 활성화 확인

```bash
ibus engine
```

`hanguliyagi` 가 출력되면 정상입니다.  
`No global engine` 오류가 나오면 6단계의 `ibus engine hanguliyagi` 를 다시 실행하세요.

---

## GNOME 입력 소스 설정

GNOME의 입력 소스(Super+Space 전환 목록)와 IBus 엔진은 별개입니다.  
GNOME 입력 소스에 한국어가 등록되어 있으면 IBus 엔진과 충돌하여 한/영 전환이 동작하지 않을 수 있습니다.

1. **GNOME 설정 → 키보드 → 입력 소스** 열기
2. 한국어(Korean) 항목이 있으면 **삭제**
3. 영어(English, 영어(미국) 등) 하나만 남겨둡니다

> GNOME 입력 소스를 비워도 IbusHangulIyagi의 한/영 전환은 정상 동작합니다.  
> 한/영 전환은 GNOME이 아닌 IBus 엔진 내부에서 처리됩니다.

---

## 사용 방법

| 키 | 동작 |
|----|------|
| **한/영 키** | 한글 ↔ 영문 전환 |
| **Shift+Space** | 한글 ↔ 영문 전환 |
| **우측 Alt** | 한글 ↔ 영문 전환 |
| **Shift+자음** | 쌍자음 입력 (ㄲ, ㄸ, ㅃ, ㅆ, ㅉ) |
| **Backspace** | 조합 중인 글자 한 획씩 삭제 |

기본 시작 모드는 **한글 모드**입니다.  
영문이 필요하면 한/영 키로 전환 후 입력하세요.

---

## 제거

```bash
sudo rm /usr/local/libexec/ibus-engine-hanguliyagi
sudo rm /usr/share/ibus/component/hanguliyagi.xml
ibus restart
```

gsettings 초기화:
```bash
gsettings reset org.freedesktop.ibus.general preload-engines
gsettings reset org.freedesktop.ibus.general engines-order
```

---

## 알려진 제한

| 환경 | 상태 |
|------|------|
| Chrome (Wayland) | 완벽 동작 |
| GNOME 텍스트 편집기 | 완벽 동작 |
| GNOME Terminal | 완벽 동작 |
| IDLE (Python/Tk) | preedit 풍선 표시 안 됨 (Tk 한계, 수정 불가) |
