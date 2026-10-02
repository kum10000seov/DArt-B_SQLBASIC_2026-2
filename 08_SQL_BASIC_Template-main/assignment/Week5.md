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

## 📚 Section 5. 다량의 자료를 연결 : `JOIN`

| 강의 | 내용 | 핵심 키워드 |
| :---: | --- | --- |
| `5-2` | JOIN 이해하기 | `JOIN`, `Key`, 테이블 연결 |
| `5-3` | 다양한 JOIN 방법 | `INNER`, `LEFT`, `RIGHT`, `FULL`, `CROSS` |
| `5-4` | JOIN 쿼리 작성하기 | `FROM`, `JOIN`, `ON`, `AS` |
| `5-5` | JOIN을 처음 공부할 때 헷갈렸던 부분 | 기준 테이블, 다중 JOIN, 컬럼 선택, `NULL` |

---

## 📑 목차

1. [JOIN 기본 개념](#join-basic)
2. [JOIN이 필요한 이유](#join-why)
3. [JOIN의 종류](#join-type)
   - [INNER JOIN](#inner-join)
   - [LEFT JOIN](#left-join)
   - [RIGHT JOIN](#right-join)
   - [FULL JOIN](#full-join)
   - [CROSS JOIN](#cross-join)
4. [JOIN 선택 기준](#join-choice)
5. [JOIN 쿼리 작성 흐름](#join-flow)
6. [JOIN 기본 문법](#join-syntax)
7. [BigQuery JOIN 예시](#join-bigquery)
8. [여러 테이블 JOIN](#multi-join)
9. [JOIN에서 헷갈리기 쉬운 부분](#join-caution)
10. [NULL](#join-null)
11. [JOIN 핵심 정리](#join-summary)

---

## 🗺️ 전체 흐름

```text
테이블 확인
    ↓
기준 테이블 결정
    ↓
JOIN Key 확인
    ↓
JOIN 종류 결정
    ↓
예상 결과 작성
    ↓
JOIN 쿼리 작성
    ↓
결과 확인
```

---

<a id="join-basic"></a>

# 01. JOIN 기본 개념

`JOIN`  
→ 서로 다른 데이터 테이블을 연결하는 문법

### 핵심

- 서로 다른 Table 연결
- 공통 컬럼 `Key`를 기준으로 연결
- 보통 `id` 값 활용
- 특정 범위(Date 등)를 기준으로 연결하는 경우도 존재
- JOIN 문법보다 **테이블 구조 파악**이 중요

```text
Table A
   │
   │ 공통 Key
   ↓
Table B
```

---

## 🔑 JOIN Key

두 테이블을 연결할 수 있는 공통 값

예시

```text
trainer_pokemon.trainer_id
              =
trainer.id
```

| Table | Key |
| --- | --- |
| `trainer` | `id` |
| `trainer_pokemon` | `trainer_id` |
| `pokemon` | `id` |

### 연결 구조

```text
trainer
   │
   │ id = trainer_id
   ↓
trainer_pokemon
   │
   │ pokemon_id = id
   ↓
pokemon
```

> [!IMPORTANT]
> JOIN 전 가장 먼저 확인  
> → **어떤 컬럼을 Key로 연결할 것인지**

---

<a id="join-why"></a>

# 02. JOIN이 필요한 이유

관계형 데이터베이스(RDBMS)

→ 데이터를 여러 Table로 분리해서 저장

### 이유

- 데이터 중복 최소화
- 각 Table별 역할 분리
- 필요할 때 JOIN해서 사용

```text
User Table
→ 사용자 정보

Order Table
→ 주문 정보

Product Table
→ 상품 정보
```

### 데이터 저장 구조

```text
분리된 여러 Table
        ↓
       JOIN
        ↓
분석에 필요한 데이터
```

| 관점 | 특징 |
| --- | --- |
| 데이터 저장 | 여러 Table로 분리 |
| 목적 | 데이터 중복 최소화 |
| 데이터 분석 | 필요한 Table JOIN |
| 데이터 웨어하우스 | JOIN + 연산 후 데이터 마트 생성 |

---

<a id="join-type"></a>

# 03. JOIN의 종류

| JOIN | 기준 | 결과 |
| :---: | --- | --- |
| `INNER JOIN` | 양쪽 | 공통 데이터만 |
| `LEFT JOIN` | 왼쪽 | 왼쪽 데이터 전부 유지 |
| `RIGHT JOIN` | 오른쪽 | 오른쪽 데이터 전부 유지 |
| `FULL JOIN` | 양쪽 | 양쪽 데이터 전부 유지 |
| `CROSS JOIN` | 없음 | 모든 행의 조합 |

---

## 예시 Table

### Table A

| Key | 값 |
| :---: | :---: |
| 1 | 가 |
| 2 | 나 |
| 3 | 다 |

### Table B

| Key | 값 |
| :---: | :---: |
| 1 | A |
| 2 | B |
| 4 | C |

공통 Key

```text
1, 2
```

---

## JOIN별 결과

| JOIN | 결과 Key |
| :---: | :---: |
| `INNER JOIN` | 1, 2 |
| `LEFT JOIN` | 1, 2, 3 |
| `RIGHT JOIN` | 1, 2, 4 |
| `FULL JOIN` | 1, 2, 3, 4 |
| `CROSS JOIN` | 3 × 3 = 9개 조합 |

---

<a id="inner-join"></a>

## 03.1 INNER JOIN

두 Table에 **공통으로 존재하는 값만 연결**

```text
Table A       Table B

   1 ───────── 1
   2 ───────── 2
   3           4

결과
→ 1, 2
```

### Query

```sql
SELECT
    A.col1,
    B.col2
FROM table_a AS A
INNER JOIN table_b AS B
    ON A.key = B.key;
```

### 핵심

```text
A에 존재
+
B에 존재
=
결과에 포함
```

즉,

```text
교집합
```

---

<a id="left-join"></a>

## 03.2 LEFT JOIN

**왼쪽 Table 기준**

왼쪽 Table의 데이터 전부 유지

```text
Table A       Table B

   1 ───────── 1
   2 ───────── 2
   3 ───────── NULL

결과
→ 1, 2, 3
```

### Query

```sql
SELECT
    A.col1,
    B.col2
FROM table_a AS A
LEFT JOIN table_b AS B
    ON A.key = B.key;
```

### 핵심

- 왼쪽 Table = 기준 Table
- 왼쪽 데이터 모두 유지
- 오른쪽에 연결값 없으면 `NULL`

> [!TIP]
> JOIN이 헷갈릴 경우  
> → `LEFT JOIN` 중심으로 먼저 이해

---

<a id="right-join"></a>

## 03.3 RIGHT JOIN

**오른쪽 Table 기준**

```text
Table A       Table B

   1 ───────── 1
   2 ───────── 2
NULL ───────── 4

결과
→ 1, 2, 4
```

### Query

```sql
SELECT
    A.col1,
    B.col2
FROM table_a AS A
RIGHT JOIN table_b AS B
    ON A.key = B.key;
```

### 핵심

```text
LEFT JOIN의 반대
```

---

<a id="full-join"></a>

## 03.4 FULL JOIN

양쪽 Table의 데이터 모두 유지

```text
1 → A, B 모두 존재
2 → A, B 모두 존재
3 → A에만 존재
4 → B에만 존재

결과
→ 1, 2, 3, 4
```

### Query

```sql
SELECT
    A.col1,
    B.col2
FROM table_a AS A
FULL JOIN table_b AS B
    ON A.key = B.key;
```

연결값 없는 부분

```text
→ NULL
```

---

<a id="cross-join"></a>

## 03.5 CROSS JOIN

두 Table의 **모든 행을 서로 조합**

```text
Table A = 3행
Table B = 3행

3 × 3
=
9행
```

### Query

```sql
SELECT
    A.col1,
    B.col2
FROM table_a AS A
CROSS JOIN table_b AS B;
```

### 특징

- 공통 Key 불필요
- `ON` 사용 X
- 모든 경우의 수 생성

---

## ON 사용 여부

| JOIN | `ON` |
| :---: | :---: |
| `INNER JOIN` | O |
| `LEFT JOIN` | O |
| `RIGHT JOIN` | O |
| `FULL JOIN` | O |
| `CROSS JOIN` | X |

---

<a id="join-choice"></a>

# 04. JOIN 선택 기준

JOIN 종류

→ 얻고 싶은 결과 기준으로 선택

| 목적 | JOIN |
| --- | :---: |
| 공통 데이터만 추출 | `INNER JOIN` |
| 왼쪽 기준 유지 | `LEFT JOIN` |
| 오른쪽 기준 유지 | `RIGHT JOIN` |
| 양쪽 데이터 유지 | `FULL JOIN` |
| 모든 조합 생성 | `CROSS JOIN` |

### 간단한 판단

```text
교집합 필요
    ↓
INNER JOIN
```

```text
기준 Table 유지
    ↓
LEFT JOIN
```

```text
모든 조합 필요
    ↓
CROSS JOIN
```

> [!IMPORTANT]
> JOIN 종류부터 선택 X  
> → **원하는 결과 형태부터 예상**

---

<a id="join-flow"></a>

# 05. JOIN 쿼리 작성 흐름

```text
① Table 확인
      ↓
② 기준 Table 정의
      ↓
③ JOIN Key 찾기
      ↓
④ 결과 예상
      ↓
⑤ Query 작성
      ↓
⑥ 결과 확인
```

| 단계 | 내용 |
| :---: | --- |
| Table 확인 | 저장된 데이터와 컬럼 확인 |
| 기준 Table 정의 | Base Table 결정 |
| JOIN Key | `ON`에 사용할 공통 Key 확인 |
| 결과 예상 | 결과 Table 미리 예상 |
| Query 작성 | JOIN SQL 작성 |
| 검증 | 예상 결과와 실제 결과 비교 |

### 가장 중요한 흐름

```text
어떤 데이터를 구할 것인가?
        ↓
기준 Table은 무엇인가?
        ↓
연결할 Table은 무엇인가?
        ↓
공통 Key는 무엇인가?
        ↓
결과가 어떻게 생길 것인가?
        ↓
Query 작성
```

---

<a id="join-syntax"></a>

# 06. JOIN 기본 문법

```sql
SELECT
    A.col1,
    A.col2,
    B.col11,
    B.col12
FROM table1 AS A
LEFT JOIN table2 AS B
    ON A.key = B.key;
```

### 구조

```text
FROM
→ 기준 Table

JOIN
→ 연결할 Table

ON
→ 연결할 Key

AS
→ Table 별칭
```

| 문법 | 역할 |
| :---: | --- |
| `FROM` | 기준 Table |
| `JOIN` | 연결할 Table |
| `ON` | 연결 조건 |
| `AS` | Alias |

---

## Alias

긴 Table 이름을 짧게 사용

```sql
FROM table1 AS A
LEFT JOIN table2 AS B
    ON A.key = B.key;
```

이후

```sql
A.col1
B.col2
```

형태로 사용

### 장점

- 코드 길이 감소
- 컬럼 출처 구분
- JOIN Query 가독성 증가

---

<a id="join-bigquery"></a>

# 07. BigQuery JOIN 예시

교안 기준

`trainer_pokemon`을 Base Table로 사용

---

## ① trainer 연결

```sql
SELECT
    tp.*,
    t.*
FROM basic.trainer_pokemon AS tp
LEFT JOIN basic.trainer AS t
    ON tp.trainer_id = t.id;
```

### Key

```text
tp.trainer_id
      =
t.id
```

---

## ② pokemon 추가 연결

```sql
SELECT
    tp.*,
    t.*,
    p.*
FROM basic.trainer_pokemon AS tp
LEFT JOIN basic.trainer AS t
    ON tp.trainer_id = t.id
LEFT JOIN basic.pokemon AS p
    ON tp.pokemon_id = p.id;
```

### 구조

```text
trainer_pokemon
      │
      ├── trainer_id = trainer.id
      │
      └── pokemon_id = pokemon.id
```

### Base Table

```text
trainer_pokemon
```

필요한 정보를 오른쪽 Table에서 계속 추가

---

<a id="multi-join"></a>

# 08. 여러 테이블 JOIN

여러 Table 연속 JOIN 가능

```sql
SELECT
    table_a.col1,
    table_b.col2,
    table_c.col3
FROM table_a
LEFT JOIN table_b
    ON table_a.key = table_b.key
LEFT JOIN table_c
    ON table_a.key = table_c.key;
```

### 구조

```text
             ┌── Table B
             │
Table A ─────┤
             │
             └── Table C
```

### 유의점

- JOIN 개수 자체의 한계 없음
- 불필요하게 많은 Table 연결 여부 확인
- 필요한 Table만 연결

---

<a id="join-caution"></a>

# 09. JOIN에서 헷갈리기 쉬운 부분

## ① 어떤 JOIN 사용?

```text
교집합
→ INNER JOIN

기준 Table 유지
→ LEFT JOIN

모든 조합
→ CROSS JOIN
```

처음에는 `LEFT JOIN` 중심으로 연습

---

## ② 어떤 Table을 왼쪽에 배치?

`LEFT JOIN`

→ **기준 Table을 왼쪽에 배치**

```sql
FROM base_table AS A
LEFT JOIN additional_table AS B
    ON A.key = B.key;
```

```text
Base Table
    │
    │ LEFT JOIN
    ↓
추가 정보
```

---

## ③ 여러 Table 연결 가능?

가능

```sql
FROM table_a
LEFT JOIN table_b
    ON table_a.key = table_b.key
LEFT JOIN table_c
    ON table_a.key = table_c.key;
```

단,

```text
필요한 JOIN인지 확인
```

---

## ④ 모든 컬럼 선택 필요?

필요 없음

### JOIN 확인 단계

```sql
SELECT
    table_a.*,
    table_b.*
FROM table_a
LEFT JOIN table_b
    ON table_a.key = table_b.key;
```

`*`

→ 모든 컬럼 확인

---

### 실제 사용 단계

필요한 컬럼만 선택

```sql
SELECT
    table_a.id,
    table_a.col1,
    table_b.col2
FROM table_a
LEFT JOIN table_b
    ON table_a.key = table_b.key;
```

### BigQuery 유의점

- 불필요한 컬럼 제외
- 처리 데이터 감소
- 비용 절감 가능
- `id`는 Unique 여부 확인에 자주 활용

---

<a id="join-null"></a>

# 10. NULL

`NULL`

→ 값이 없거나 알 수 없는 상태

```text
NULL ≠ 0

NULL ≠ ""

NULL ≠ 공백
```

### JOIN에서 NULL 발생

한쪽 Table에 연결할 값이 없는 경우

```text
Table A       Table B

1 ─────────── 1
2 ─────────── 2
3 ─────────── 없음

LEFT JOIN 결과

1 | 값
2 | 값
3 | NULL
```

---

## 예시

### Table A

| Key | 이름 |
| :---: | :---: |
| 1 | 가 |
| 2 | 나 |
| 3 | 다 |

### Table B

| Key | 등급 |
| :---: | :---: |
| 1 | A |
| 2 | B |

### LEFT JOIN 결과

| Key | 이름 | 등급 |
| :---: | :---: | :---: |
| 1 | 가 | A |
| 2 | 나 | B |
| 3 | 다 | `NULL` |

Key `3`

→ Table B에서 연결값 없음  
→ `NULL`

---

## NULL 비교

| 값 | 의미 |
| :---: | --- |
| `0` | 숫자 0 |
| `''` | 빈 문자열 |
| 공백 | 공백 문자 |
| `NULL` | 값 자체 없음 / 알 수 없음 |

---

<a id="join-summary"></a>

# 11. JOIN 핵심 정리

| 개념 | 핵심 |
| --- | --- |
| `JOIN` | 여러 Table 연결 |
| `Key` | 연결 기준 컬럼 |
| `Base Table` | JOIN 기준 Table |
| `INNER JOIN` | 교집합 |
| `LEFT JOIN` | 왼쪽 기준 |
| `RIGHT JOIN` | 오른쪽 기준 |
| `FULL JOIN` | 양쪽 모두 |
| `CROSS JOIN` | 모든 조합 |
| `ON` | JOIN Key 지정 |
| `AS` | Alias |
| `NULL` | 값 없음 |

---

## 🔥 JOIN 기본 형태

```sql
SELECT
    A.col1,
    B.col2
FROM table_a AS A
LEFT JOIN table_b AS B
    ON A.key = B.key;
```

```text
SELECT
→ 필요한 컬럼

FROM
→ 기준 Table

JOIN
→ 연결할 Table

ON
→ 공통 Key
```

---

## ✅ JOIN 작성 순서

```text
Table 확인
    ↓
Base Table 결정
    ↓
JOIN Key 확인
    ↓
JOIN 종류 선택
    ↓
결과 예상
    ↓
Query 작성
    ↓
결과 확인
```

---

## 📌 한 줄 정리

```text
JOIN
= 기준 Table + 공통 Key + 필요한 다른 Table의 정보
```
---

# 2️⃣ 수행 인증란

<img width="283" height="230" alt="image" src="https://github.com/user-attachments/assets/061d482d-d97d-45d2-b2a1-fef1356450f9" />


---

# 3️⃣ 확인 문제

프로그래머스는 로그인이 필요하므로, 로그인 후 문제 풀이를 진행해주세요.

## 🧩 문제 1

문제 링크: [자동차 대여 기록에서 장기/단기 대여 구분하기](https://school.programmers.co.kr/learn/courses/30/lessons/151138)

풀이 과정:

```
- 장기/단기 대여를 나눈 기준:
- 사용한 날짜 계산 방식:
- CASE WHEN으로 만든 컬럼:
```

<!-- 정답을 맞추게 되면, 정답입니다. 이 부분을 캡처해서 이 주석을 지우시고 첨부해주시면 됩니다. -->

## 🧩 문제 2

문제 링크: [한 해에 잡은 물고기 수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/298516)

풀이 과정:

```
- 문제에서 요구한 연도:
- 사용한 날짜 조건:
- 집계한 대상:
```

<!-- 정답을 맞추게 되면, 정답입니다. 이 부분을 캡처해서 이 주석을 지우시고 첨부해주시면 됩니다. -->

## 🧩 문제 3

문제 링크: [조건에 부합하는 중고거래 상태 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/164672)

풀이 과정:

```
- 날짜 조건:
- CASE WHEN으로 바꾼 값:
- ELSE에 해당하는 경우:
- 정렬 기준:
```

<!-- 정답을 맞추게 되면, 정답입니다. 이 부분을 캡처해서 이 주석을 지우시고 첨부해주시면 됩니다. -->

## 🧩 문제 4

문제 링크: [자동차 평균 대여 기간 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/157342)

풀이 과정:

```
- GROUP BY 기준:
- 평균을 계산한 방식:
- HAVING에 사용한 조건:
- 처음 헷갈렸던 점:
```

<!-- 정답을 맞추게 되면, 정답입니다. 이 부분을 캡처해서 이 주석을 지우시고 첨부해주시면 됩니다. -->

---

# 4️⃣ 이번 주 회고

```
1. 날짜 함수 중 가장 헷갈린 함수:
2. CASE WHEN을 사용할 때 기억해야 할 문법:
3. 날짜/시간 데이터나 조건문을 활용해보고 싶은 분석 상황:
```

수고하셨습니다!




