# 📘 SQL_BASIC 5주차 정규 과제

SQL_BASIC 정규 과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고, 핵심 개념을 정리한 뒤 간단한 SQL 문제를 직접 풀어보는 방식으로 진행합니다.

이번 주는 날짜/시간 데이터와 조건문을 학습합니다. 특히 `CASE WHEN`은 SQL 문제 풀이와 데이터 분석에서 자주 사용되므로, 직접 분류 기준을 만들고 결과를 확인하는 연습을 해주세요.

완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**👀 수행 인증란은 필수입니다.**

---

## 📚 SQL_BASIC_5th_TIL

### 섹션 5. 데이터 탐색 - 변환

### 4-4. 날짜 및 시간 데이터 이해하기

### 4-6. 조건문(CASE WHEN, IF)

---

## ✨ 선택 강의

- 4-5. 시간 데이터 연습문제: 날짜/시간 함수를 더 연습하고 싶을 때 선택 수강
- 4-7. 조건문 연습문제: CASE WHEN과 IF를 더 연습하고 싶을 때 선택 수강

---

## 🏁 전체 강의 수강 계획

| 주차 | 필수 강의 범위 | 선택 강의 | 완료 여부 |
| --- | --- | --- | --- |
| 1주차 | 1-1 ~ 2-1 | 1 | ✅ |
| 2주차 | 2-2 ~ 2-3 | 2-4 | ✅ |
| 3주차 | 2-5, 2-7 ~ 2-8 | 2-6 | ✅ |
| 4주차 | 3-2 ~ 4-3 | 3-4 | ✅ |
| 5주차 | 4-4 ~ 4-6 | 4-5, 4-7 | ✅ |
| 6주차 | 5-2 ~ 5-5 | 5-6 | 🍽️ |
| 7주차 | 필수 강의 없음 | 6-2, 6-3, 6-4, 6-5 | 🍽️ |

---

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념 정리

## 📑 목차

1. [날짜 및 시간 데이터 타입](#date-time-type)
2. [Time Zone과 UTC](#timezone)
3. [Millisecond · Microsecond](#millisecond)
4. [시간 데이터 타입 변환](#datetime-convert)
5. [주요 DATETIME 함수](#datetime-function)
6. [DATETIME 타입 변환 함수](#parse-format)
7. [LAST_DAY · DATETIME_DIFF](#lastday-diff)
8. [날짜 함수 선택 기준](#datetime-summary)
9. [조건문 함수](#condition)
10. [CASE WHEN](#case-when)
11. [IF](#if)
12. [CASE WHEN vs IF](#case-if)
13. [컬럼 변환 전체 정리](#column-summary)

---

<a id="date-time-type"></a>

## 01. 날짜 및 시간 데이터 타입

날짜·시간 데이터 처리의 핵심

- 데이터 타입 파악
- Time Zone 이해
- 타입 변환
- 필요한 시간 부분 추출
- 시간 단위 자르기
- 두 시간의 차이 계산

### 주요 타입

| 타입 | 저장 내용 | Time Zone | 형태 |
| :---: | --- | :---: | --- |
| `DATE` | 날짜 | X | `YYYY-MM-DD` |
| `TIME` | 시간 | X | `HH:MM:SS` |
| `DATETIME` | 날짜 + 시간 | X | `YYYY-MM-DD HH:MM:SS` |
| `TIMESTAMP` | 특정 시점 | O | UTC 기준 시점 |

### 구조

`DATE` + `TIME` → `DATETIME`

`DATETIME` → 날짜와 시간만 표현  
`TIMESTAMP` → Time Zone을 고려한 특정 시점 표현

> [!IMPORTANT]
> 날짜 함수 사용 전 가장 먼저 확인  
> → 현재 컬럼의 타입이 `DATE`, `DATETIME`, `TIMESTAMP` 중 무엇인지 확인

---

<a id="timezone"></a>

## 02. Time Zone과 UTC

### Time Zone

특정 지역의 표준 시간대

| 개념 | 의미 |
| --- | --- |
| `GMT` | Greenwich Mean Time |
| `UTC` | 국제 표준 시간 |
| 한국 시간 | `UTC + 9` |
| `DATETIME` | Time Zone 정보 X |
| `TIMESTAMP` | Time Zone 정보 O |

### 핵심 관계

```text
UTC
 ↓ +9시간
한국 시간
```

`TIMESTAMP`를 지역 시간 기준의 `DATETIME`으로 변환할 경우 Time Zone 지정 필요

---

<a id="millisecond"></a>

## 03. Millisecond · Microsecond

초보다 작은 시간 단위

| 단위 | 크기 |
| :---: | --- |
| Second | 1초 |
| Millisecond `ms` | 1/1,000초 |
| Microsecond `μs` | 1/1,000,000초 |

### BigQuery 변환 함수

| 함수 | 역할 |
| --- | --- |
| `TIMESTAMP_MILLIS()` | Millisecond → TIMESTAMP |
| `TIMESTAMP_MICROS()` | Microsecond → TIMESTAMP |
| `DATETIME()` | TIMESTAMP → DATETIME |

```sql
SELECT
    TIMESTAMP_MILLIS(millisecond_col) AS timestamp_from_ms,
    TIMESTAMP_MICROS(microsecond_col) AS timestamp_from_us,
    DATETIME(timestamp_col, 'Asia/Seoul') AS datetime_kst
FROM your_table;
```

### 변환 흐름

`Millisecond / Microsecond` → `TIMESTAMP` → `DATETIME`

> [!TIP]
> 실제 Table에서 시간이 `TIMESTAMP` 또는 숫자형 시간값으로 저장된 경우 존재  
> → 분석 전 데이터 타입 확인 필수

---

<a id="datetime-convert"></a>

## 04. 시간 데이터 타입 변환

### TIMESTAMP ↔ DATETIME

| 변환 | 사용 |
| --- | --- |
| 현재 TIMESTAMP | `CURRENT_TIMESTAMP()` |
| TIMESTAMP → DATETIME | `DATETIME(timestamp, time_zone)` |
| 현재 DATETIME | `CURRENT_DATETIME()` |
| 현재 날짜 | `CURRENT_DATE()` |

```sql
SELECT
    CURRENT_TIMESTAMP() AS current_timestamp,
    CURRENT_DATE('Asia/Seoul') AS current_date,
    CURRENT_DATETIME('Asia/Seoul') AS current_datetime,
    DATETIME(timestamp_col, 'Asia/Seoul') AS datetime_col
FROM your_table;
```

### 유의점

- `TIMESTAMP` → 특정 시점 중심
- `DATETIME` → 날짜 + 시간 중심
- 지역 시간 필요 시 `'Asia/Seoul'` 등 Time Zone 지정

---

<a id="datetime-function"></a>

## 05. 주요 DATETIME 함수

### 함수 한눈에 보기

| 함수 | 역할 | 핵심 |
| --- | --- | --- |
| `CURRENT_DATETIME()` | 현재 DATETIME | 현재 시간 |
| `EXTRACT()` | 특정 부분 추출 | 연·월·일·시간 등 |
| `DATETIME_TRUNC()` | 특정 단위로 자르기 | 시간 단위 정리 |
| `PARSE_DATETIME()` | 문자열 → DATETIME | 타입 변환 |
| `FORMAT_DATETIME()` | DATETIME → 문자열 | 출력 형식 변환 |
| `LAST_DAY()` | 마지막 날짜 반환 | 월·주 마지막 날 |
| `DATETIME_DIFF()` | 두 DATETIME 차이 | 기간 계산 |

---

### 5-1. CURRENT_DATETIME

현재 DATETIME 확인

```sql
SELECT
    CURRENT_DATE() AS current_date,
    CURRENT_DATE('Asia/Seoul') AS asia_date,
    CURRENT_DATETIME() AS current_datetime,
    CURRENT_DATETIME('Asia/Seoul') AS asia_datetime;
```

`CURRENT_DATETIME([time_zone])`

→ Time Zone 생략 가능  
→ 지역 기준 필요 시 Time Zone 지정

---

### 5-2. EXTRACT

DATETIME에서 필요한 부분만 **추출**

| 추출 대상 | 문법 |
| --- | --- |
| 날짜 | `EXTRACT(DATE FROM datetime_col)` |
| 연도 | `EXTRACT(YEAR FROM datetime_col)` |
| 월 | `EXTRACT(MONTH FROM datetime_col)` |
| 일 | `EXTRACT(DAY FROM datetime_col)` |
| 시간 | `EXTRACT(HOUR FROM datetime_col)` |
| 분 | `EXTRACT(MINUTE FROM datetime_col)` |
| 요일 | `EXTRACT(DAYOFWEEK FROM datetime_col)` |

```sql
SELECT
    EXTRACT(DATE FROM datetime_col) AS date,
    EXTRACT(YEAR FROM datetime_col) AS year,
    EXTRACT(MONTH FROM datetime_col) AS month,
    EXTRACT(DAY FROM datetime_col) AS day,
    EXTRACT(HOUR FROM datetime_col) AS hour,
    EXTRACT(MINUTE FROM datetime_col) AS minute
FROM your_table;
```

### DAYOFWEEK

`EXTRACT(DAYOFWEEK FROM datetime_col)`

- 일요일부터 시작
- `1 ~ 7` 반환

> [!TIP]
> 날짜·시간 전체가 필요 X  
> → 필요한 숫자만 꺼낼 때 `EXTRACT`

---

### 5-3. DATETIME_TRUNC

DATETIME을 지정한 단위로 **자르기**

| 기준 | 문법 |
| :---: | --- |
| 연 | `DATETIME_TRUNC(datetime_col, YEAR)` |
| 월 | `DATETIME_TRUNC(datetime_col, MONTH)` |
| 일 | `DATETIME_TRUNC(datetime_col, DAY)` |
| 시간 | `DATETIME_TRUNC(datetime_col, HOUR)` |

```sql
SELECT
    DATETIME_TRUNC(datetime_col, YEAR) AS year_trunc,
    DATETIME_TRUNC(datetime_col, MONTH) AS month_trunc,
    DATETIME_TRUNC(datetime_col, DAY) AS day_trunc,
    DATETIME_TRUNC(datetime_col, HOUR) AS hour_trunc
FROM your_table;
```

### EXTRACT vs DATETIME_TRUNC

| 구분 | `EXTRACT` | `DATETIME_TRUNC` |
| --- | --- | --- |
| 목적 | 특정 부분 추출 | 특정 단위까지 자르기 |
| 결과 | 숫자·날짜 등 특정 요소 | DATETIME 형태 유지 |
| 활용 | 연도, 월, 시간 등의 값 필요 | 시간 단위 집계 |
| 판단 기준 | 특정 값만 필요한가? | DATETIME 형태를 유지할 것인가? |

`EXTRACT` → 필요한 **부분 꺼내기**  
`DATETIME_TRUNC` → 필요한 **단위까지 자르기**

---

<a id="parse-format"></a>

## 06. DATETIME 타입 변환 함수

### PARSE_DATETIME

문자열 → `DATETIME`

**문법**

`PARSE_DATETIME('문자열 형식', 문자열)`

```sql
SELECT
    PARSE_DATETIME(
        '%Y-%m-%d %H:%M:%S',
        datetime_string
    ) AS datetime_col
FROM your_table;
```

### FORMAT_DATETIME

`DATETIME` → 문자열

**문법**

`FORMAT_DATETIME('출력 형식', datetime_col)`

```sql
SELECT
    FORMAT_DATETIME('%c', datetime_col) AS formatted_datetime
FROM your_table;
```

### 두 함수 비교

| 함수 | 입력 | 출력 |
| :---: | --- | --- |
| `PARSE_DATETIME` | 문자열 | DATETIME |
| `FORMAT_DATETIME` | DATETIME | 문자열 |

```text
문자열
  ↓ PARSE_DATETIME
DATETIME
  ↓ FORMAT_DATETIME
문자열
```

### Format Elements

`%Y`, `%m`, `%d`, `%H` 등으로 날짜·시간 형식 지정

> [!NOTE]
> Format Elements 전부 암기 X  
> → 필요할 때 공식 문서 확인 후 적용

---

<a id="lastday-diff"></a>

## 07. LAST_DAY · DATETIME_DIFF

### 7-1. LAST_DAY

특정 기간의 마지막 날짜 반환

| 문법 | 의미 |
| --- | --- |
| `LAST_DAY(datetime_col)` | 월의 마지막 날 |
| `LAST_DAY(datetime_col, MONTH)` | 월의 마지막 날 |
| `LAST_DAY(datetime_col, WEEK)` | 주의 마지막 날 |
| `LAST_DAY(datetime_col, WEEK(SUNDAY))` | 일요일 기준 주 |
| `LAST_DAY(datetime_col, WEEK(MONDAY))` | 월요일 기준 주 |

월말 등 특정 기간의 마지막 날짜 계산 시 활용

---

### 7-2. DATETIME_DIFF

두 DATETIME 사이의 차이 계산

**기본 구조**

`DATETIME_DIFF(첫 번째 DATETIME, 두 번째 DATETIME, 단위)`

```sql
SELECT
    DATETIME_DIFF(end_datetime, start_datetime, DAY) AS day_diff,
    DATETIME_DIFF(end_datetime, start_datetime, WEEK) AS week_diff,
    DATETIME_DIFF(end_datetime, start_datetime, MONTH) AS month_diff
FROM your_table;
```

| 단위 | 의미 |
| :---: | --- |
| `DAY` | 일 차이 |
| `WEEK` | 주 차이 |
| `MONTH` | 월 차이 |

> [!IMPORTANT]
> 첫 번째 값 - 두 번째 값 구조  
> → DATETIME 순서가 바뀌면 결과 방향도 변경

---

<a id="datetime-summary"></a>

## 08. 날짜 함수 선택 기준

### 필요한 작업 → 함수 선택

| 필요한 작업 | 함수 |
| --- | --- |
| 현재 날짜 | `CURRENT_DATE()` |
| 현재 DATETIME | `CURRENT_DATETIME()` |
| TIMESTAMP 변환 | `TIMESTAMP_MILLIS()`, `TIMESTAMP_MICROS()` |
| DATETIME에서 일부 추출 | `EXTRACT()` |
| 시간 단위로 자르기 | `DATETIME_TRUNC()` |
| 문자열 → DATETIME | `PARSE_DATETIME()` |
| DATETIME → 문자열 | `FORMAT_DATETIME()` |
| 마지막 날짜 | `LAST_DAY()` |
| 두 DATETIME 차이 | `DATETIME_DIFF()` |

### 선택 흐름

```text
현재 시간 필요 ─────────────→ CURRENT_DATETIME

특정 부분만 필요 ──────────→ EXTRACT

특정 시간 단위로 정리 ─────→ DATETIME_TRUNC

문자열을 시간 타입으로 ─────→ PARSE_DATETIME

시간 타입을 문자열로 ───────→ FORMAT_DATETIME

기간의 마지막 날짜 ─────────→ LAST_DAY

두 시간의 차이 ─────────────→ DATETIME_DIFF
```

> [!TIP]
> 대표 함수 중심으로 기억  
> → 세부 문법은 필요할 때 공식 문서 확인

---

<a id="condition"></a>

## 09. 조건문 함수

조건에 따라 서로 다른 값을 출력하거나 새로운 컬럼 생성

### 기본 개념

```text
조건 충족
   ├─ YES → 값 A
   └─ NO  → 값 B
```

### 조건문 사용 목적

- 조건에 따른 분기 처리
- 기존 값을 새로운 범주로 변환
- 여러 카테고리 통합
- 분석 목적에 맞는 새 컬럼 생성

### 주요 조건문

| 함수 | 사용 상황 |
| :---: | --- |
| `CASE WHEN` | 여러 조건 |
| `IF` | 단일 조건 |

---

<a id="case-when"></a>

## 10. CASE WHEN

여러 조건을 순서대로 확인할 때 사용

### 기본 문법

```sql
SELECT
    CASE
        WHEN 조건1 THEN 결과1
        WHEN 조건2 THEN 결과2
        ELSE 그_외_결과
    END AS 새로운_컬럼
FROM your_table;
```

### 구조

| 문법 | 역할 |
| :---: | --- |
| `CASE` | 조건문 시작 |
| `WHEN` | 조건 |
| `THEN` | 조건이 참일 때 결과 |
| `ELSE` | 어느 조건에도 해당하지 않을 때 |
| `END` | 조건문 종료 |
| `AS` | 새 컬럼명 지정 |

### 처리 순서

```text
WHEN 조건1 확인
      ↓ 거짓
WHEN 조건2 확인
      ↓ 거짓
ELSE
```

조건1이 참이면 그 결과 반환 후 종료

> [!CAUTION]
> 여러 `WHEN` 조건에 동시에 해당 가능  
> → **위에 작성된 조건 우선**

### CASE WHEN 핵심

- 여러 조건 처리에 적합
- 위에서 아래로 순서대로 판단
- 조건 순서 중요
- `END` 누락 주의
- 결과 컬럼에 `AS`로 이름 부여

---

<a id="if"></a>

## 11. IF

하나의 조건을 간단하게 처리할 때 사용

### 문법

`IF(조건, 참일 때 값, 거짓일 때 값) AS 새로운_컬럼`

```sql
SELECT
    IF(
        조건,
        참일_때_값,
        거짓일_때_값
    ) AS 새로운_컬럼
FROM your_table;
```

### 구조

| 위치 | 의미 |
| :---: | --- |
| 첫 번째 | 조건 |
| 두 번째 | True 결과 |
| 세 번째 | False 결과 |

```text
IF(
   조건,
   True 결과,
   False 결과
)
```

---

<a id="case-if"></a>

## 12. CASE WHEN vs IF

| 구분 | `CASE WHEN` | `IF` |
| --- | --- | --- |
| 조건 수 | 여러 조건 | 단일 조건 |
| 구조 | 여러 `WHEN` 사용 | True / False |
| 조건 순서 | 중요 | 단일 조건 |
| 활용 | 여러 범주 분류 | 두 가지 결과 분기 |

### 선택 기준

```text
조건이 여러 개
→ CASE WHEN

조건이 하나
→ IF
```

> [!IMPORTANT]
> 조건이 복잡할수록 `CASE WHEN`  
> 단순 True / False 구분은 `IF`

---

<a id="column-summary"></a>

## 13. 컬럼 변환 전체 정리

<img width="610" height="284" alt="image" src="https://github.com/user-attachments/assets/31fe7445-e817-4cc5-8004-e752e6c73d17" />

### 기본 SQL 흐름

```sql
SELECT
    컬럼1,
    컬럼2,
    변환된_컬럼
FROM 테이블
WHERE 조건
GROUP BY 집계할_컬럼;
```

### 데이터 타입

| 종류 | 내용 |
| :---: | --- |
| 숫자 | 숫자 연산 |
| 문자 | 문자열 처리 |
| 시간·날짜 | 날짜 및 시간 처리 |
| Bool | 참 / 거짓 |

### 컬럼 변환 도구

| 영역 | 주요 문법 / 함수 |
| --- | --- |
| 숫자 | 사칙연산, `SAFE_DIVIDE` |
| 문자 | `CONCAT`, `SPLIT`, `REPLACE`, `TRIM`, `UPPER` |
| 시간·날짜 | `EXTRACT`, `DATETIME_TRUNC`, `PARSE_DATETIME` |
| 데이터 타입 변경 | 필요한 타입으로 변환 |
| 조건에 따른 변경 | `CASE WHEN`, `IF` |


### 전체 흐름

```text
원본 데이터
   ↓
데이터 타입 확인
   ↓
필요한 컬럼 변환
   ├─ 숫자 연산
   ├─ 문자열 처리
   ├─ 날짜·시간 처리
   └─ 조건문 처리
   ↓
WHERE 조건 적용
   ↓
GROUP BY 집계
   ↓
분석용 데이터
```

---

## 🔖 핵심 치트시트

| 목적 | 문법 |
| --- | --- |
| 날짜만 저장 | `DATE` |
| 날짜 + 시간 | `DATETIME` |
| 특정 시점 + Time Zone | `TIMESTAMP` |
| 현재 시간 | `CURRENT_DATETIME()` |
| 날짜·시간 일부 추출 | `EXTRACT()` |
| 시간 단위 자르기 | `DATETIME_TRUNC()` |
| 문자열 → DATETIME | `PARSE_DATETIME()` |
| DATETIME → 문자열 | `FORMAT_DATETIME()` |
| 마지막 날짜 | `LAST_DAY()` |
| 두 시간 차이 | `DATETIME_DIFF()` |
| 여러 조건 | `CASE WHEN` |
| 단일 조건 | `IF` |

---

## ✅ 최종 흐름 정리

```text
날짜·시간 데이터
→ 타입부터 확인
→ 필요한 부분 추출 / 변환 / 차이 계산

조건문
→ 조건 개수 확인
→ 여러 조건 : CASE WHEN
→ 단일 조건 : IF

CASE WHEN
→ 위에서 아래로 판단
→ 겹치는 조건은 앞선 WHEN 우선
```

---

# 2️⃣ 수행 인증란

<img width="278" height="313" alt="image" src="https://github.com/user-attachments/assets/d16623c3-6548-4e59-ab0e-e296b7411422" />



---

# 3️⃣ 확인 문제

프로그래머스는 로그인이 필요하므로, 로그인 후 문제 풀이를 진행해주세요.

## 🧩 문제 1

문제 링크: [자동차 대여 기록에서 장기/단기 대여 구분하기](https://school.programmers.co.kr/learn/courses/30/lessons/151138)

풀이 과정:

```
- 장기/단기 대여를 나눈 기준: 대여 기간이 30일 이상이면 장기 대여, 30일 미만이면 단기 대여
- 사용한 날짜 계산 방식: DATEDIFF(END_DATE, START_DATE) + 1
- CASE WHEN으로 만든 컬럼: RENT_TYPE
```
<img width="574" height="449" alt="image" src="https://github.com/user-attachments/assets/aeb35002-bf4f-4a80-8993-78d4f025c00a" />


## 🧩 문제 2

문제 링크: [한 해에 잡은 물고기 수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/298516)

풀이 과정:

```
- 문제에서 요구한 연도: 2021년
- 사용한 날짜 조건: EXTRACT(YEAR FROM TIME) = 2021
- 집계한 대상: 조건에 해당하는 물고기 수를 COUNT(*)로 집계
```
<img width="371" height="446" alt="image" src="https://github.com/user-attachments/assets/556fd9d2-92a4-437e-aaea-3b5bdea35ddb" />


## 🧩 문제 3

문제 링크: [조건에 부합하는 중고거래 상태 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/164672)

풀이 과정:

```
- 날짜 조건: `CREATED_DATE = '2022-10-05'`
- CASE WHEN으로 바꾼 값: `SALE → 판매중`, `RESERVED → 예약중`, `DONE → 거래완료`
- ELSE에 해당하는 경우: 문제에서 지정된 세 상태만 사용하므로 별도 처리 없음
- 정렬 기준: `BOARD_ID` 기준 내림차순 `DESC`
```
<img width="425" height="443" alt="image" src="https://github.com/user-attachments/assets/a6142992-4f00-4287-85cb-1a8456b99a13" />


## 🧩 문제 4

문제 링크: [자동차 평균 대여 기간 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/157342)

풀이 과정:

```
- GROUP BY 기준: CAR_ID
- 평균을 계산한 방식: AVG(DATEDIFF(END_DATE, START_DATE) + 1) 후 ROUND() 적용
- HAVING에 사용한 조건: 평균 대여 기간이 7일 이상
- 처음 헷갈렸던 점: 대여 기간 계산 시 시작일과 종료일을 모두 포함하므로 DATEDIFF() + 1 처리 필요
```
<img width="565" height="450" alt="image" src="https://github.com/user-attachments/assets/a91864d4-79e8-4ec0-b84e-1518ee099cf1" />


---

# 4️⃣ 이번 주 회고

1. 날짜 함수 중 가장 헷갈린 함수:  
   `DATEDIFF`  
   → 두 날짜의 차이만 계산하므로 실제 대여 기간 계산 시 시작일을 포함하기 위해 `+ 1` 필요

2. CASE WHEN을 사용할 때 기억해야 할 문법:  
   `CASE WHEN 조건 THEN 결과 ELSE 결과 END AS 컬럼명`  
   → 여러 조건이 있을 경우 위에서부터 순서대로 확인

3. 날짜/시간 데이터나 조건문을 활용해보고 싶은 분석 상황:  
   대여 시작일과 종료일을 활용한 이용 기간 분석, 특정 기간별 이용량 비교, 이용 기간에 따른 장기/단기 이용자 분류

수고하셨습니다!




