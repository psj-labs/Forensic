# PhotoRec

- PhotoRec은 TestDisk 패키지에 포함된 파일 복구 도구입니다
- TestDisk 역시 디스크 복구 도구이지만, 두 도구는 목적과 동작 방식에서 차이가 있습니다.

- PhotoRec은 파일 이름이나 디렉터리 구조를 신뢰하지 않고, 파일 내부의 시그니처(signature)를 기준으로 RAW 스캔을 수행합니다.
- 이 방식 덕분에 파일시스템이 손상되었거나 포맷된 경우에도 파일 복구가 가능합니다

## 주요 특징

- 파일시스템 무시 후 시그니처 기반 복구
- USB, SD 카드, 외장하드 등 이동식 저장장치에 적합

## TestDisk와 PhotoRec의 차이

- TestDisk  
  - 파티션 테이블, 부트 섹터 복구  
  - 디스크 구조 복구 중심

- PhotoRec  
  - 개별 파일 복구  
  - 파일 시그니처 기반 RAW 복구


## PhotoRec 설치 확인

- PhotoRec은 TestDisk 설치 시 함께 설치됩니다.
- 설치가 정상적으로 되었는지 확인하기 위해 다음 명령어를 실행합니다.

```text
photorec --version
```

- 버전 정보가 출력되면 정상적으로 설치된 상태입니다.


## 실습 목표

- 실습 목표는 삭제된 USB 디스크에서 과거 파일을 복구하는 것입니다. 
- 실습 전 USB에는 사진 파일 1장만 남겨둔 상태입니다. 

## 실습 환경

- OS: Parrot OS (가상머신)
- USB: SanDisk Ultra
- 파일시스템: FAT32

### USB 장치 정보는 아래와 같습니다.

![usb information](/photorec/imgs/usb%20information.png)


## PhotoRec 실행

- 다음 명령어로 PhotoRec을 실행합니다.

```text
sudo photorec
```

- 실행하면 현재 시스템에 연결된 디스크 목록이 출력됩니다.

![device list](/photorec/imgs/device%20list.png)

- 표시된 디스크 중 다음 항목을 선택합니다.

```text
Disk /dev/sda - 15GB / 14GiB (RO) - SanDisk SanDisk Ultra
```

해당 항목의 의미는 다음과 같습니다.

- /dev/sda : 현재 연결된 USB 전체 디스크
- 15GB / 14GiB : USB 용량
- (RO) : Read Only, 읽기 전용 상태로 복구 작업에 안전
- SanDisk Ultra : USB 모델명

아래 항목은 복구 대상이 아닙니다.

```text
Disk /dev/vda - 68GB / 64GiB (RO)
```

- 이 디스크는 Parrot OS가 설치된 가상머신의 메인 가상 디스크이며, 물리 USB가 아닙니다.

- 필자는 SanDisk USB에 저장되어 있던 파일을 복구할 것이므로 `/dev/sda` 항목을 선택한 뒤 `Enter` 키를 눌러 다음 단계로 이동합니다.


## 파티션 선택

- 다음 화면에서 파티션 선택 화면이 출력됩니다.

![partition](/photorec/imgs/partition.png)

- 다음과같이 두가지 항목이 존재합니다.

```text
No partition [Whole disk]  
- 파티션 구조를 무시  
- 디스크 전체 RAW 스캔  
- 파티션 정보가 손상된 경우 사용
```
```text
1 P FAT32 LBA  
- 정상적인 FAT32 파티션  
- 일반적인 USB 포맷 상태  
- 가장 권장되는 선택지
```

- 본 실습에서는 `1 P FAT32 LBA`를 선택합니다.

## 파일시스템 유형 선택

- 다음으로 파일시스템 유형을 선택합니다.

![fileSystem type](/photorec/imgs/fileSystem%20type.png)

- 다음과같이 두가지 항목이 존재합니다

```text
ext2/ext3/ext4 filesystem  
- 리눅스 전용 파일시스템  
- 내부 HDD, SSD에서 주로 사용
```
```text

Other (FAT/NTFS/HFS+/ReiserFS/…)  
- FAT32, exFAT, NTFS, macOS HFS+  
- USB, SD 카드, 외장하드에 사용
```

- 본 실습 USB는 `FAT32`이므로 `Other`를 선택한 후 `Enter` 키를 누릅니다.


## 스캔 범위 선택

- 다음은 스캔 범위 선택 화면입니다.

![scan range](/photorec/imgs/scan%20range.png)

```text
Free  
- 삭제된 영역만 스캔  
- 빠르고 결과가 깔끔함  
- 실습 및 일반 복구에 적합
```

```text
Whole  
- 파티션 전체 스캔  
- 중복 파일 다수 발생  
- 매우 느림
```
- 삭제된 데이터 복구가 목적이므로 `Free`를 선택합니다.

## 복구 파일 저장 위치 선택

복구된 파일을 저장할 위치를 선택합니다.

![save location](/photorec/imgs/save%20location.png)

- 현재 위치는 `/home/user/Desktop` 이며, `.`을 선택해 이 위치에 복구 파일을 저장합니다.

- 이 화면에서는 `Enter`가 아니라 `C` 키를 눌러 저장 위치를 확정해야 합니다.

## 복구 진행 및 결과 확인

- 복구가 시작되면 다음과 같은 화면이 출력됩니다.

![recovering](/photorec/imgs/recovering.png)

- 약 3분 후 다음 메시지가 출력되며 복구가 완료됩니다.

```text
434 files saved in /home/user/Desktop/recup_dir directory.  
Recovery completed.
```

복구된 파일이 저장된 경로는 다음과 같습니다.

```text
/home/user/Desktop/recup_dir
```

![result](/photorec/imgs/result.png)

- *처음에는 사진 한 장만 존재하던 USB에서 `엑셀, PDF, PPT, 이미지 파일` 등 과거 삭제되었던 모든 파일이 복구된 것을 확인할 수 있습니다.*

## 예시 이미지

- 복구된 이미지 파일 중 하나의 속성은 다음과 같습니다.

![example](/photorec/imgs/example.png)

- 문서 작성 시점은 2025년 12월 13일이지만, `2013년`에 삭제된 파일 역시 정상적으로 복구됨을 확인할 수 있습니다.

- Created: 파일이 복구된 시점
- Accessed: 사용자가 파일에 접근한 시점

## 요약

- *PhotoRec은 파일시스템 정보가 손상되었거나 삭제된 상황에서도 파일 시그니처 분석을 통해 과거 데이터를 효과적으로 복구할 수 있는 강력한 포렌식 도구입니다.*
