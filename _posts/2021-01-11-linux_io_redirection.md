---
title: "[Linux] 입출력 리다이렉션(IO Redirection)"
category: Linux
---

# IO Redirection

![]("https://static.thegeekstuff.com/wp-content/uploads/2010/11/filesystem-structure.png")

- Redirection : 출력되는 방향을 다른 곳으로 돌려서 파일에 저장시키는 것(원래는 화면에 출력)
- Output 예
    - `COMMAND > FILENAME` : 출력 결과를 파일에 저장(덮어쓰기)
    - `COMMAND >> FILENAME` : 출력 결과를 파일에 저장(Append)
    - `COMMAND 2> FILENAME` : 에러를 파일에 저장
    - `COMMAND > FILENAME 2> FILENAME` : 출력 결과를 파일에 저장 + 에러를 파일에 저장
- Input 예
    - `cat < FILENAME`  : 파일에 저장되어 있는 내용을 입력으로 받음
        - `cat FILENAME` : 대부분 쓰는 방식
    - `cat << FILENAME` : 파일에 저장되어 있는 내용을 입력으로 받음(Append)
    - `head -nN FILENAME` : N줄만 출력
- `mail EMAIL_ADDRESS << END_TEXT` : END_TEXT가 나오면 입력이 끝남

