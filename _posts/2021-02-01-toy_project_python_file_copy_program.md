---
title: "[Toy project] Python 파일 복사 프로그램"
category: Toy project
---

## 1. 개요

> 파이썬 파일 복사 프로그램 작성(mycopy.py)
>
> 커맨드 예 :  `C:\\ > python mycopy.py -cp fromfile.txt tofile.txt`

<br>

## 2. 개발 명세

### 2.1 프로그램 사양

1. Python 3.8.3
2. mycopy.py (3KB)

3. mac OS, Linux OS, Window OS compatible

<br>

### 2.2 주요 기능

#### **2.2.1 파일 복사 커맨드**

> `python mycopy.py -cp [복사할 txt 파일] [붙여넣을 txt파일]`

<br>

#### **2.2.2 커맨드 가이드**

> 커맨드가 틀렸을 때, 올바르게 입력할 수 있도록 쉘에서 즉시 안내 문구를 출력함

- **인자가 모두 입력되지 않은 경우**, '커맨드 형식 안내'

  - 인자 3개 미입력

	  ![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa847c0d7-4815-4d59-a5e9-c86413f9ec4a%2FUntitled.png?table=block&id=61426362-fa7e-4efe-8d15-1aecf3cc8301&width=3600&userId=&cache=v2)

  - 인자 2개 미입력

	  ![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F7409d7b3-40d7-44e9-8faf-846a20849313%2FUntitled.png?table=block&id=ecb97401-654b-42fc-9d30-5e261f0ee32e&width=3610&userId=&cache=v2)

  - 인자 1개 미입력

	  ![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F5c533cf2-1937-4e65-83f6-44c61f1d71a1%2FUntitled.png?table=block&id=07ca4dde-8b05-412f-9345-79a996d312e0&width=3610&userId=&cache=v2)

  <br>

- 인자가 모두 입력되었으나 **txt 파일이 아니거나 입력한 txt 파일이 없는 경우**, 'txt 파일 리스트 안내'
    ![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F1926a16b-14c0-4990-94d2-b7e424ba0df8%2FUntitled.png?table=block&id=35cc4084-4eb0-48c5-bece-44d5f1995573&width=3730&userId=&cache=v2)

    ![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F49885130-9be8-43e7-8515-d3ed590879f1%2FUntitled.png?table=block&id=cd78c41c-5ed6-48c5-b4e9-acef49a7abee&width=3720&userId=&cache=v2)

<br>

#### **2.2.3 실행 결과 안내**

> 복사한 파일의 내용을 출력하는 질문을 한 뒤 응하는 경우 출력을, 응하지 않는 경우 프로그램을 종료함

- 질문에 응하는 경우, '파일 내용 출력'

	![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8c3d92d4-0f44-4b1d-bcee-1520412b2e76%2FUntitled.png?table=block&id=86e9a154-638c-46bc-a4de-7daeb1a0783a&width=3720&userId=&cache=v2)

- 질문에 응하지 않는 경우, '프로그램 종료 문구 출력'
    ![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ff523a820-123b-4d23-b409-639e927e76af%2FUntitled.png?table=block&id=627140cc-4a02-4421-a425-a8127338ba8a&width=3720&userId=&cache=v2)

<br>

#### 2.2.4 최종 실행 결과 화면

![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fcc8d3e81-8814-4588-bbd0-02acc1c28c23%2FUntitled.png?table=block&id=3d45c17b-5abf-4731-8cf1-0768e20d6b6b&width=3830&userId=&cache=v2)

<br>

## 3. 소스 코드

```python
# 모듈
import sys
import os

# 상수
FILENAME = 'mycopy.py'
OPTION = '-cp'

# 예외처리를 직접 만들기 위한 클래스
class FileTypeError(Exception):
    pass

# 예외 처리 구문
try:
    # sys.argv = ['mycopy', '-cp', '복사할 txt파일', '붙여넣을 txt파일']
    # sys.argv의 원소를 각각 변수로 초기화 하는 과정에서 리스트에 누락이 있으면, IndexError 발생
    option = sys.argv[1] # 옵션
    f_copy = sys.argv[2] # 복사할 파일
    f_paste = sys.argv[3] # 붙여넣을 파일

    if option == OPTION: # 옵션이 정확히 들어왔을 경우
        if f_copy[-4:] == '.txt' and f_paste[-4:] == '.txt': 
        # 복사/붙여넣을 파일의 형식이 .txt인 경우
            with open(f_copy, 'rt', encoding="utf-8") as f: 
                text = f.read() # 복사할 파일을 읽어 텍스트를 변수로 저장
            with open(f_paste, 'wt', encoding="utf-8") as f: 
                f.write(text)# 붙여넣을 파일을 열어 텍스트 복사

            # 파일 복사 완료 후 내용 출력여부 확인
            answer = input('파일 복사가 완료되었습니다. 복사한 파일의 내용을 확인하시겠습니까?(Y/N) : ')
            if answer == 'Y' or answer == 'y' or answer == 'ㅛ': # Y
                print('파일명) {}' .format(f_paste))
                print('내용)')
                with open(f_paste, 'rt', encoding="utf-8") as f: 
                    read = f.read() # 파일을 열어 저장된 내용 출력
                print(read)
            else: # N
                print('파일 복사 프로그램을 종료합니다.') # 프로그램 종료

        else: # 복사/붙여넣을 파일의 형식이 .txt가 아닌 경우
            raise FileTypeError() # 직접 생성한 파일형식에러 강제 발생

    else: # 옵션이 정확하게 들어오지 않았을 경우
        raise IndexError # IndexError 강제 발생

# 에러 처리 구문
except IndexError: # sys.argv 리스트에 값이 하나라도 누락되었을 경우 에러 발생
    print('커맨드 형식이 올바르지 않습니다. 아래 형식을 참조하여 다시 입력하세요.')
    print('커맨드 형식 : python {0} {1} [복사할 txt파일] [붙여넣을 txt파일]' .format(FILENAME, OPTION))

except (FileTypeError, FileNotFoundError): # txt파일이 아니거나 복사할 파일이 없는 경우
    print('txt파일이 없거나 파일의 형식이 올바르지 않습니다.')
    print('현재 경로에 있는 \'*.txt\' 파일 목록은 아래와 같습니다.')
    print('-----------------------------------------------------')

    # OS별 현재 경로의 .txt 파일 리스트 출력
    if os.name == 'nt': # Window OS
        os.system('dir /b | findstr .txt')
    else: # Linux OS, Mac OS
        os.system('ls | grep .txt')
```