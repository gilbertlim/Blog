---
title: "[Linux] 패키지 매니저(Package Manager)"
category: Linux
---

# Package Manager

- 패키지 매니저 : 커맨드 라인의 세계에서 앱스토어에 해당하는 소프트웨어
    - 패캐지 설치, 삭제, 관리 목적
    - 기본으로 탑재된 패키지 : `ls`, `mkdir`, ...
- 예
    - `apt`
        - `sudo apt-get update;` : 설치할 수 있는 패키지의 최신 목록 다운로드
        - `sudo apt-cache search NAME` :패키지 검색
        - `sudo apt-get install NAME` : 패키지 다운로드
        - `sudo apt-get upgrade NAME` : 패키지 업그레이드
        - `sudo apt-get remove NAME` : 패키지 삭제
- Homebrew (Mac OS)
    - 맥OS 전용 패키지 매니저
    - 예
        - `brew`
            - `brew update`
            - `brew search NAME`
            - `brew install NAME`
            - `brew upgrade NAME`
            - `brew uninstall NAME`

### File Download

- `wget -O FILENAME URL` : URL을 통한 다운로드 후 파일명 지정

### Version Control System

- Git : 버전 관리 시스템의 한 종류