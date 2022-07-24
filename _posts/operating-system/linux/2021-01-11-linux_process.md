---
title: "[Linux] 프로세스(Process)"
category: Linux
---

# Process

### 프로세스의 개념

- Processor, Memory, Storage
    - Processor : 속도↑↑↑
    - Memory : 가격↑ 용량↓ 속도↑
    - Storage : 가격↓ 용량↑ 속도↓
    - 상호보완관계이므로 둘 다 필요함
- 컴퓨터의 구조(아키텍쳐)
    1. Storage에 설치된 프로그램을 읽어서 Memory에 적재시킴
    2. Memory에 올라와 있는 프로그램을 CPU가 읽어서 적혀있는대로 동작하여 데이터를 처리함
- 프로그램
    - 명령어(Command)가 컴퓨터 상에 존재하는 방식
    - 파일 형태로 저장되어 있는 것
- 프로세스
    - 프로세서에 의해 처리되고 있는 상태에 있는 것들
    - 실행되고 있는 상태의 프로그램

### 커맨드를 활용한 프로세스 확인

- `ps` : 프로세스 리스트 보기

- `ps aux` : 모든 프로세스 보기

- `ps aux | grep NAME` : 모든 프로세스 중 특정 프로세스 찾기

- `sudo kill PID` : 프로세스 강제 종료

- `sudo top` : 프로세스 리스트(UI 좋음)

- `sudo htop`

     : 프로세스 리스트(UI 더 좋음)

    - Command : 프로세스가 어떤 명령으로 실행되었는지 확인 가능
    - RES : 물리적인 메모리의 사용량
    - Load average
        - CPU 점유율 평균(1분, 5분, 15분)
        - 1.67 : 프로세서 8개 중 1.67개만 사용되고 있다는 뜻