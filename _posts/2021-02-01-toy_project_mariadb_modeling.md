## 1. 개요

> 게시판 사이트 데이터베이스 모델링 및 테이블 작성
>
> 회원들이 글을 쓰고, 댓글을 사용하고, 파일을 등록할 수 있는 웹사이트 게시판

<br>

## 2. 개발 명세

### 2.1 프로그램 사양

1. MariaDB 15.1
2. MySQL Workbench : Mac OS 전용 mySQL 유틸리티(ERD, SQL)
3. Sequel Pro : Mac OS 전용 mySQL 유틸리티(데이터 입력)

<br>

### 2.2 ERD(Entity Relationship Diagram)

> 게시판 관련 업무파악 후 객체 간 관계를 나타낸 ERD 작성

![ERD](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Feb5e7d12-55af-4d85-9a30-8069530b32bd%2FERD.png?table=block&id=4c219a48-015a-49e3-b885-5f5094c0a5df&width=1720&userId=&cache=v2)

<br>

### 2.3 테이블 명세

> 테이블별 필드, 데이터형식, 테이블 설명

- DB명 : boardDB

- 회원 테이블(memberTbl)

    > 회원 정보가 담긴 테이블
    >
    > 한명의 회원이 여러개의 게시글을 쓰고, 댓글을 달고, 파일을 첨부하는 관계(1:N)

    | 필드명        | 데이터 형식                       | 설명                                        |
    | ------------- | --------------------------------- | ------------------------------------------- |
    | id            | INT, NOT NULL, AUTO_INCREMENT, PK | PK                                          |
    | member_id     | VARCHAR(20), NOT NULL, UNIQUE     | 회원 아이디                                 |
    | member_pw     | VARCHAR(16), NOT NULL             | 회원 비밀번호                               |
    | first_name    | VARCHAR(10), NOT NULL             | 회원 이름                                   |
    | last_name     | VARCHAR(10), NOT NULL             | 회원 이름                                   |
    | brith         | DATE, NOT NULL                    | 회원 생년월일                               |
    | gender        | ENUM('M','F'), NOT NULL           | 회원 성별                                   |
    | email         | VARCHAR(50), NOT NULL, UNIQUE     | 회원 이메일                                 |
    | phone_number  | VARCHAR(11), NOT NULL, UNIQUE     | 회원 휴대전화번호                           |
    | address       | VARCHAR(100), NULL                | 회원 자택 주소                              |
    | join_date     | DATETIME, NOT NULL                | 회원가입일                                  |
    | photo_address | VARCHAR(1000), NULL               | 프로필 사진이 저장된 주소(외부 저장소 사용) |
    | visit_count   | INT, NOT NULL                     | 사이트 방문 수                              |

- 게시판 테이블(boardTbl)

    > 게시판 정보가 담긴 테이블
    >
    > 한 개의 게시판에 여러개의 게시글이 존재하는 관계(1:N)

    | 필드명      | 데이터 형식                       | 설명        |
    | ----------- | --------------------------------- | ----------- |
    | id          | INT, NOT NULL, AUTO_INCREMENT, PK | PK          |
    | board_title | VARCHAR(50), NOT NULL, UNIQUE     | 게시판 이름 |
    
- 게시글 테이블(postTbl)

    > 게시판별 게시글 정보가 담긴 테이블
    >
    > 게시글 테이블은 게시판 테이블과 회원 테이블을 참조함

    | 필드명           | 데이터 형식                       | 설명                                 |
    | ---------------- | --------------------------------- | ------------------------------------ |
    | id               | INT,NOT NULL,  AUTO_INCREMENT, PK | PK                                   |
    | board_post_id    | INT, NOT NULL                     | 게시판 ID, FK(boardTbl.id 참조)      |
    | post_title       | VARCHAR(50), NOT NULL             | 게시글 제목                          |
    | post_content     | LONGTEXT, NOT NULL                | 게시글 내용                          |
    | post_writer      | INT, NOT NULL                     | 게시글 작성자, FK(memberTbl.id 참조) |
    | post_pw          | CHAR(4), NOT NULL                 | 게시글 비밀번호                      |
    | post_like        | INT, NOT NULL                     | 좋아요 수                            |
    | watch_count      | INT, NOT NULL                     | 조회 수                              |
    | post_ip          | CHAR(15), NOT NULL                | 게시글 작성자 IP                     |
    | tags             | VARCHAR(45), NULL                 | 게시글 태그                          |
    | post_create_date | DATETIME, NOT NULL                | 게시글 작성일 및 시간                |
    | post_update_date | DATETIME, NULL                    | 게시글 수정일 및 시간                |
    | post_delete_date | DATETIME, NULL                    | 게시글 삭제일 및 시간                |

- 댓글 테이블(commentTbl)

    > 게시글에 작성된 댓글의 정보가 담긴 테이블
    >
    > 댓글 테이블은 게시글 테이블과 회원 테이블을 참조함

    | 필드명              | 데이터 형식                   | 설명                               |
    | ------------------- | ----------------------------- | ---------------------------------- |
    | id                  | INT, NOT NULL, AUTO_INCREMENT | PK                                 |
    | post_comment_id     | INT, NOT NULL                 | 게시글 ID, FK(postTbl.id 참조)     |
    | comment_writer      | INT, NOT NULL                 | 댓글 작성자, FK(memberTbl.id 참조) |
    | comment_content     | MEDIUMTEXT, NOT NULL          | 댓글 내용                          |
    | comment_ip          | CHAR(15), NOT NULL            | 댓글 작성자 IP                     |
    | comment_create_date | DATETIME, NOT NULL            | 댓글 작성일 및 시간                |
    | comment_update_date | DATETIME, NULL                | 댓글 수정일 및 시간                |
    | comment_delete_date | DATETIME, NULL                | 댓글 삭제일 및 시간                |

- 첨부 테이블(attachmentTbl)

    > 게시글에 첨부된 파일의 정보가 담긴 테이블
    >
    > 첨부 테이블은 게시글 테이블과 회원 테이블을 참조함

    | 필드명                 | 데이터 형식                   | 설명                                 |
    | ---------------------- | ----------------------------- | ------------------------------------ |
    | id                     | INT, NOT NULL, AUTO_INCREMENT | PK                                   |
    | post_attachment_id     | INT, NOT NULL                 | 게시글 ID, FK(postTbl.id 참조)       |
    | member_attachment_id   | INT, NOT NULL                 | 파일 첨부자, FK(memberTbl.id 참조)   |
    | file_address           | VARCHAR(100), NOT NULL        | 파일이 저장된 주소(외부 저장소 사용) |
    | attachment_create_date | DATETIME, NOT NULL            | 파일 등록일 및 시간                  |
    | attachment_update_date | DATETIME, NULL                | 파일 수정일 및 시간                  |
| attachment_delete_date | DATETIME, NULL                | 파일 삭제일 및 시간                  |

<br>

### 2.4 DB 및 테이블 생성

> SQL문을 활용하여 DB 및 테이블 생성

```mariadb
-- -----------------------------------------------------
-- Schema boardDB
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `boardDB` DEFAULT CHARACTER SET utf8 ;
USE `boardDB` ;

-- -----------------------------------------------------
-- Table `boardDB`.`memberTbl`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `boardDB`.`memberTbl` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `member_id` VARCHAR(20) NOT NULL,
  `member_pw` VARCHAR(16) NOT NULL,
  `first_name` VARCHAR(10) NOT NULL,
  `last_name` VARCHAR(10) NOT NULL,
  `birth` DATE NOT NULL,
  `gender` ENUM('M', 'F') NOT NULL,
  `email` VARCHAR(50) NOT NULL,
  `phone_number` VARCHAR(11) NOT NULL,
  `address` VARCHAR(100) NULL,
  `join_date` DATETIME NOT NULL,
  `photo_address` VARCHAR(1000) NULL,
  `visit_count` INT NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `member_id_UNIQUE` (`member_id` ASC) VISIBLE,
  UNIQUE INDEX `email_UNIQUE` (`email` ASC) VISIBLE,
  UNIQUE INDEX `phone_number_UNIQUE` (`phone_number` ASC) VISIBLE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `boardDB`.`boardTbl`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `boardDB`.`boardTbl` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `board_title` VARCHAR(50) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `board_title_UNIQUE` (`board_title` ASC) VISIBLE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `boardDB`.`postTbl`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `boardDB`.`postTbl` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `board_post_id` INT NOT NULL,
  `post_title` VARCHAR(50) NOT NULL,
  `post_content` LONGTEXT NOT NULL,
  `post_writer` INT NOT NULL,
  `post_pw` CHAR(4) NOT NULL,
  `post_like` INT NOT NULL,
  `watch_count` INT NOT NULL,
  `post_ip` CHAR(15) NOT NULL,
  `tags` VARCHAR(45) NULL,
  `post_create_date` DATETIME NOT NULL,
  `post_update_date` DATETIME NULL,
  `post_delete_date` DATETIME NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_member_post_writer` (`post_writer` ASC) VISIBLE,
  INDEX `fk_board_post_id` (`board_post_id` ASC) VISIBLE,
  CONSTRAINT `fk_member_post_writer`
    FOREIGN KEY (`post_writer`)
    REFERENCES `boardDB`.`memberTbl` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_board_post_id`
    FOREIGN KEY (`board_post_id`)
    REFERENCES `boardDB`.`boardTbl` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `boardDB`.`commentTbl`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `boardDB`.`commentTbl` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `post_comment_id` INT NOT NULL,
  `comment_writer` INT NOT NULL,
  `comment_content` MEDIUMTEXT NOT NULL,
  `comment_ip` CHAR(15) NOT NULL,
  `comment_create_date` DATETIME NOT NULL,
  `comment_update_date` DATETIME NULL,
  `comment_delete_date` DATETIME NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_post_comment_id` (`post_comment_id` ASC) VISIBLE,
  INDEX `fk_member_comment_writer` (`comment_writer` ASC) VISIBLE,
  CONSTRAINT `fk_post_comment_id`
    FOREIGN KEY (`post_comment_id`)
    REFERENCES `boardDB`.`postTbl` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_member_comment_writer`
    FOREIGN KEY (`comment_writer`)
    REFERENCES `boardDB`.`memberTbl` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `boardDB`.`attachmentTbl`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `boardDB`.`attachmentTbl` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `post_attachment_id` INT NOT NULL,
  `member_attachment_id` INT NOT NULL,
  `file_address` VARCHAR(100) NOT NULL,
  `file_create_date` DATETIME NOT NULL,
  `file_update_date` DATETIME NULL,
  `file_delete_date` DATETIME NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_member_attachment_id` (`member_attachment_id` ASC) VISIBLE,
  INDEX `fk_post_attachment_id` (`post_attachment_id` ASC) VISIBLE,
  CONSTRAINT `fk_member_attachment_id`
    FOREIGN KEY (`member_attachment_id`)
    REFERENCES `boardDB`.`memberTbl` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_post_attachment_id`
    FOREIGN KEY (`post_attachment_id`)
    REFERENCES `boardDB`.`postTbl` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;
```

<br>

### 2.5 JOIN QUERY

> 1. 필드 제약조건에 맞는 테스트 데이터 입력
>
> 2. 테이블에 제약조건 정상작동여부 및 의도한대로 테이블 설계가 되었는지 JOIN을 통해 확인

- 전체 회원의 정보 출력

    ```mariadb
    SELECT * FROM boardTbl;
    ```

    ![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb972e577-ae18-4e25-bbb1-d82cdeab04bc%2FUntitled.png?table=block&id=63826b0b-8210-4d85-8bfb-99f1a3e36002&width=3740&userId=&cache=v2)

- '임성진'이라는 사람이 작성한 '글의 제목'과, '댓글의 내용'을 출력

    ```mariadb
    SELECT CONCAT(M.last_name, M.first_name) AS '이름', P.post_title AS '게시글 제목', C.comment_content AS '댓글 내용'
    		FROM memberTbl M INNER JOIN postTbl P
    		ON M.id = P.post_writer
            INNER JOIN commentTbl C
            ON M.id = C.comment_writer;
    ```

    ![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F235d2b5c-27f0-4703-8f4c-1dbf03f84b02%2FUntitled.png?table=block&id=59dd5354-e6cf-4863-b16b-7848997e479b&width=3720&userId=&cache=v2)

- '임성진'이라는 사람이 첨부한 '파일이 서버에 저장된 주소'와 '생성된 일자'를 출력하고, 해당 파일이 첨부된 게시판의 이름을 출력 

    ```mariadb
    SELECT CONCAT(M.last_name, M.first_name) AS '이름', A.file_address AS '파일 주소', A.file_create_date AS '파일 생성일', P.post_title AS '게시글 제목'
    		FROM memberTbl M INNER JOIN attachmentTbl A
    		ON M.id = A.member_attachment_id
            INNER JOIN postTbl P
            ON A.post_attachment_id = P.id;
    ```

    ![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F4d3c9ef5-cac6-42b5-af13-645851975484%2FUntitled.png?table=block&id=18718f0a-9781-4aaa-9d9a-823220170998&width=3730&userId=&cache=v2)

- '임성진'이라는 사람이 작성한 글, 댓글, 첨부파일 수를 출력

    ``` mariadb
    SELECT CONCAT(M.last_name, M.first_name) AS '이름', count(P.post_writer) AS '게시글 수', count(C.comment_writer) AS '댓글 수', count(A.member_attachment_id) AS '첨부 수'
    		FROM memberTbl M INNER JOIN postTbl P
            ON M.id = P.post_writer
            INNER JOIN commentTbl C
            ON M.id = C.comment_writer
            INNER JOIN AttachmentTbl A
            ON M.id = A.member_attachment_id;
    ```

    ![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F65f6ed70-0bed-4bc7-9ae4-5cc486447da7%2FUntitled.png?table=block&id=9a3e3966-405a-4539-90b4-32106c5d6f1c&width=3720&userId=&cache=v2)

