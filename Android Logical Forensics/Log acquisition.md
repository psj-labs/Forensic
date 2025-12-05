# ADB를 이용해 모바일 기기 시스템 이벤트 로그를 수집

본 실습은 Android 기반 모바일 기기를 ADB(Android Debug Bridge)로 연결하여 시스템 이벤트 로그(Logcat)를 수집하고, 이를 텍스트 파일 형태로 저장하는 과정을 학습하는 것을 목표로 한다. 해당 실험은 모바일 기기 포렌식의 가장 기본적인 단계인 로그 수집(Acquisition)에 초점을 둔다.

## Requirements
- Parrot OS (Linux)
- Android 모바일 기기
- USB 데이터 케이블
- ADB 설치
- 개발자 옵션 활성화
- USB 디버깅 활성화

## Preparation
모바일 기기에서 개발자 옵션 활성화  
설정 → 휴대전화 정보 → 빌드 번호 7회 터치

USB 디버깅 활성화  
설정 → 개발자 옵션 → USB 디버깅 ON

USB 케이블로 휴대폰과 노트북 연결 후, 화면에 뜨는 USB 디버깅 허용 팝업에서 “허용” 선택

## Device Connection Check
디바이스 연결 상태 확인:

```bash
┌─[user@parrot]─[~]
└──╼ $adb devices
```

정상 출력 예:
```bash
List of devices attached

R59W900M4NR	device
```

표시 상태가 `device`이면 연결 성공

## System Log Acquisition
시스템 로그 전체 덤프 저장:

```bash
┌─[user@parrot]─[~]
└──╼ $adb logcat -d > system_logs.txt
```
로그 버퍼에 남아 있던 모든 시스템 이벤트 기록이 `system_logs.txt` 파일로 저장됨

## Log File Viewing
터미널에서 열람:

```bash
less system_logs.txt
```

파일 탐색기로 열기:

```bash
xdg-open system_logs.txt
```

### 로그 항목 중 일부 설명

## 1. 화면 상태 변경 기록

```text
PowerManagerService: displayReady: true
ViewRootImpl: onDisplayChanged
DisplayManagerService: requestDisplayStateInternal
```

**화면 켜짐 / 꺼짐 / Doze(Always-On Display) 상태 변화 로그**

- 단말 화면 활성/비활성 전환 기록
- 대기모드 진입 시간 추적 가능
- 사용자 단말 사용 시점 추론 가능

---

---

## 2. 잠금화면/충전 정보 로그

```text
KeyguardSecIndicationPolicy: 55분 후 충전이 완료됩니다
```

**배터리 상태 및 충전 예측 정보 표시 기록**

- 충전 연결·유지 상황
- 충전 진행 상태
- 특정 시점 사용 위치 추론 가능

---

---

## 3. 시간 갱신 이벤트 로그

```text
DigitalHorizontalClockView: onTimeChanged : 오후 3:22
```

**단말 시계 시스템 동작 기록**

- 1분 단위 타임틱 이벤트
- 시스템 작동 정상 여부 확인
- 시간 기반 로그 싱크 맞추는 기준점 역할

---

---

## 4. 앱 알림 및 백그라운드 활동 로그

```text
HoneySpace.BadgeDataSourceImpl: Package=com.google.android.youtube
```

**앱 알림 배지 및 백그라운드 이벤트 기록**

- 알림 발생 App 식별 가능
- 특정 앱 활동 추적 단서 제공

---

---

## 5. 앱 화면 변동(Activity) 기록

```text
ViewRootImpl[MainActivity]: onDisplayChanged
```

**앱 전환 / 화면 표시 이벤트**

- 앱 실행 시점 추정
- 화면 상태 변화 추적 가능

---

---

## 6. 인증 및 바이오메트릭 서비스 로그

```text
bauth_FPBAuthService: FPBAuthService
```

**Samsung 지문/인증 서비스 관련 로그**

- 지문 인증 요청 여부
- 잠금 해제 시점 추정 단서




## Summary
본 실습 수행 내용:
- ADB 연결 확인
- Logcat 시스템 이벤트 로그 덤프
- 텍스트 파일 형태로 증거 자료 보존

이는 모바일 기기 포렌식 절차 중 **증거 수집(Acquisition) 단계**에 해당함

## 알림
본 실습은 개인 소유 기기를 대상으로 학습 목적으로만 진행되었으며, 타인의 기기에 대한 무단 접근은 불법 행위에 해당함
