---
title: "[Data Analytics] 데이터 구조(Data Structure)"
category: Data Analytics
use_math: true
---

# Data Structure
데이터를 모아서 관리하는 방식

## Python, Pandas, Numpy의 데이터 구조

**Python**
- String
- **List**
- Tuple
- Dictionary : 모양, 순서가 없기 때문에 어쩔 수 없이 key:value 구조
- Casting(변환) : Type Casting, Structure Casting

    ```python
    a1 = np.array([1, 3, 5, 7, 9]) : 리스트를 넘파이의 array로 캐스팅
    print(array)
    # array([1, 3, 5, 7, 9]) : 메타데이터
    ```

**Pandas**
- dataFrame
- series

**Numpy**
- Array

## Data → Computer (0, 1) ⇒ Memory(RAM)

### Address / Index
Memory(물리적 공간)에 부여한 Address(논리적인) 구조

### bit(binary digit)
컴퓨터가 처리할 수 있는 데이터의 크기

- 속도의 개념은 아님
- Address의 크기
    - 32bit = $2^{32}$, 4GB
    - 64bit = $2^{64}$, 16EB
    - 영어 1 Byte, 한글 2 Byte


## Module
변수, 함수등을 모아둔 .py 파일

- import : 파일(*.py)을 메모리에 올려주는 역할
- 내가 원하는 함수를 만들어서 사용할 수 있음

## Package
모듈을 디렉토리 형식으로 구조화한 것

- 모듈을 넣어둔 디렉토리명 = 패키지명
- AI 분야에서는 패키지를 활용하는 일이 많을 것
- `__init__.py` 가 있어야 함
- `__all__` : import * 했을 때 어떤 모듈을 임포트할 지 정의

### 1. Numpy
다차원 행렬(Array)을 효과적으로 처리하는 패키지

#### 특징
- python에서 수학/과학 연산을 위한 **다차원 행렬 객체 지원**
- NumPy의 다차원 배열 자료형인 **array**는 scipy, pandas 등 다양한 파이썬 패키지의 기본 자료형으로 사용됨
- For 문과 같이 반복적인 연산 작업을 **배열 단위**로 처리해 효율적인 코딩이 가능함

    **scalar**
    - 크기는 있으나, 방향이 없음
    - 0D Array(0차원 행렬)
    - Rank0 Tensor(축이 없는 텐서)
    
    **vector**
    - 크기와 방향성이 있음
    - 1D Array(1차원 행렬)
    - Rank1 Tensor(축이 1개인 텐서)
    
    **matrix**
    - 2D Array
    - Rank2 Tensor(축이 2개인 텐서)
    
    **array**
    - 3D Array
    - Rank3 Tensor(축이 3개인 텐서)

### 2. Pandas
- 외부의 정형 데이터(주로)를 파이썬으로 불러들이기 위한 패키지
- 데이터 분석에 필수적인 자료구조를 제공하는 라이브러리
- 데이터 구조는 R-DB의 테이블 구조와 유사함

#### 특징
- 로드된 데이터는 Data Frame으로 불림(2개 이상의 Series)
- Series 단위로는 데이터의 형식이 일정함
- DataFrame은 Table 형태로 데이터를 다룰 수 있어 가장 직관적임
- Series와 DataFrame은 색인(index)을 기준으로 값을 저장하는 자료구조이며, 잘못된 정렬을 사전에 방지하고 이를 기준으로 손쉽게 값에 접근할 수 있음
- Join 등 관계연산을 지원해, SQL을 사용하는 효과를 누릴 수 있음
- 2차원 데이터까지 좋음(3차원부터는 Numpy 활용 필요)
- CSV : meta data 없이 데이터만 있는 파일

#### 정형 & 비정형 데이터
1. 정형 데이터 : 정해진 형태로 DB에 들어감
2. 비정형 데이터 : 카카오톡 메시지와 같이 문자, 숫자, 이미지, 링크, 문서, 음성 등 여러가지를 주고 받기 때문에 크기와 타입을 정할 수 없는 데이터

### 3. Seaborn & Matplotlib
시각화 패키지

## Data로부터 파생되는 일들
1. Planning
2. Engineering
    - 수집, 저장, 가공, 전처리
    - Hadoop, spark, ...
3. **Analytics**
    - **Statistics(DM), AI Modeling**
    - **R / Python**