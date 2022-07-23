---
title: "[MariaDB] DB 모델링"
category: MariaDB
---

# 4. DB 모델링

## 4.1 프로젝트의 진행 단계

#### 프로젝트

- 현실세계 업무를 컴퓨터 시스템으로 옮겨 놓는 과정
- 대규모 프로그램을 작성하기 위한 전체 과정
- 큰 규모의 프로그램 작업
- 계획, 분석, 설계도 작업을 포함한 프로그래밍

#### Waterfall Model

> 프로젝트 계획 > **업무 분석** > **시스템 설계** > 프로그램 구현 > 테스트 > 유지보수

## 4.2 DB 모델링

### 4.2.1 DB 모델링 개념

- 현실 세계에서 사용되는 작업을 DBMS의 DB 개체로 옮기기 위한 과정
- 현실에서 쓰이는 것을 테이블화
- 구현하고자 하는 업무에 대한 폭 넓고 정확한 지식 필요

### 4.2.2 DB 모델링 실습

>  MacOS : mySQL workbench 사용

- 웹사이트의 게시판을 활용해서 회원들이 글을 쓰고, 댓글을 사용하고 파일을 등록(Upload)할 수 있는 데이터베이스 모델링하기
1. 업무 파악
        - 회원 등록(회원 일련번호, 회원 ID, 회원 PW, 회원 이름, 회원 성별, 회원 이메일, 회원 전화번호, 회원 주소)
        - 게시판에 등록(글 일련번호, 제목, 등록 내용, 누가(회원), 언제(작성날짜), 조회수, #비밀글 설정 유무)
        - 댓글 등록(댓글 일련번호, 회원ID(작성자), 댓글 내용, 댓글 작성날짜, #댓글 비밀번호)
        - 파일 등록(파일 등록자, 파일명, 파일저장위치, 파일등록날짜, #비밀번호)
    2. DB 생성
    3. 테이블 작성
        - userTbl(**member_Num(auto_increment)**, memberID(VARCHAR(10), memberPW(VARCHAR(15), memberNAME(VARCHAR(15), ...)
        - boardTbl(**board_Num(auto_increment)**, RepDeep, Title(VARCHAR(100), Content(VARCHAR(4000), memberID, makeDATE, ...)
        - replyTbl(**reply_Num(auto_increment)**, *memberID*, repContent, ..., *board_Num*)
        - fileTbl
