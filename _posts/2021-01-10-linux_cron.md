---
title: "[Linux] CRON"
category: Linux
---

### CRON

- 정기적으로 명령을 실행시켜주는 도구 or 소프트웨어

- 사용 방법

    - crontab 표현 방법

        - ```
            crontab -e
            ```

            - `mm hh dom mon dow command`

            - 예

                - ```
                    */1 * * * * date >> date.log 2>&1
                    ```

                     : 1분마다 로그 기록

                    - `2>&1` : 표준 에러를 표준 출력으로 리다이렉션, date.log 파일에 함께 저장됨

    - crontab 확인 방법

        - `tail FILENAME` : 파일을 감시하고 있다가, 파일의 뒤쪽에 텍스트가 추가되면 Refresh를 해줌

- 사례 : 웹페이지에서 글을 쓰면 서버에서 사용자에게 메일을 보내는 프로그램

    1. 사용자가 데이터를 전송하면 서버는 저장이 되었다고 기록하고, 바로 완료되었다고 답을 사용자에게 보냄
    2. 서버에 설치된 cron이 정기적으로 saved라는 정보를 확인해서 정보가 추가되었다면, Backgroud로 사용자에게 이메일을 보내는 작업을 진행함

