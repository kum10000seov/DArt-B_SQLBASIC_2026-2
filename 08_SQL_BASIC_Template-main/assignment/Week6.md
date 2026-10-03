# 📘 SQL_BASIC 6주차 정규 과제

SQL_BASIC 정규 과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고, 핵심 개념을 정리한 뒤 간단한 SQL 문제를 직접 풀어보는 방식으로 진행합니다.

이번 주는 SQL 입문 과정의 핵심 고비인 `JOIN`을 학습합니다.

JOIN은 여러 테이블을 연결해서 분석 질문에 답하기 위해 반드시 필요한 개념입니다. 이번 주 과제에서는 정답 코드뿐 아니라 **어떤 테이블을 어떤 키로 연결했는지**를 꼭 기록해주세요.

완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**👀 수행 인증란은 필수입니다.**

---

## 📚 SQL_BASIC_6th_TIL

### 섹션 6. 다량의 자료를 연결 : JOIN

### 5-2. JOIN 이해하기

### 5-3. 다양한 JOIN 방법

### 5-4. JOIN 쿼리 작성하기

### 5-5. JOIN을 처음 공부할 때 헷갈리는 부분

---

## ✨ 선택 강의

- 5-6. JOIN 연습문제: JOIN 문제를 더 연습하고 싶을 때 선택 수강

---

## 🏁 전체 강의 수강 계획

| 주차 | 필수 강의 범위 | 선택 강의 | 완료 여부 |
| --- | --- | --- | --- |
| 1주차 | 1-1 ~ 2-1 | 1 | ✅ |
| 2주차 | 2-2 ~ 2-3 | 2-4 | ✅ |
| 3주차 | 2-5, 2-7 ~ 2-8 | 2-6 | ✅ |
| 4주차 | 3-2 ~ 4-3 | 3-4 | ✅ |
| 5주차 | 4-4 ~ 4-6 | 4-5, 4-7 | ✅ |
| 6주차 | 5-2 ~ 5-5 | 5-6 | ✅ |
| 7주차 | 필수 강의 없음 | 6-2, 6-3, 6-4, 6-5 | 🍽️ |

---

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념 정리

## Section 5. 다량의 자료를 연결 : `JOIN`

| 강의 | 내용 | 핵심 키워드 |
| :---: | --- | --- |
| `5-2` | JOIN 이해하기 | `JOIN`, `Key`, 테이블 연결 |
| `5-3` | 다양한 JOIN 방법 | `INNER`, `LEFT`, `RIGHT`, `FULL`, `CROSS` |
| `5-4` | JOIN 쿼리 작성하기 | `FROM`, `JOIN`, `ON`, `AS` |
| `5-5` | JOIN을 처음 공부할 때 헷갈렸던 부분 | 기준 테이블, 다중 JOIN, 컬럼 선택, `NULL` |

> 📖 PDF 기준 : **340 ~ 389쪽**

---

## 📑 목차

1. [JOIN 기본 개념](#join-basic)
2. [JOIN이 필요한 이유](#join-why)
3. [JOIN의 종류](#join-type)
4. [JOIN 선택 기준](#join-choice)
5. [JOIN 쿼리 작성 흐름](#join-flow)
6. [JOIN 기본 문법](#join-syntax)
7. [여러 테이블 JOIN](#multi-join)
8. [JOIN에서 헷갈리기 쉬운 부분](#join-caution)
9. [NULL](#join-null)
10. [JOIN 핵심 정리](#join-summary)

---

<a id="join-basic"></a>

# 01. JOIN 기본 개념

`JOIN`  
→ 서로 다른 데이터 테이블을 연결하는 문법

### 핵심

- 서로 다른 Table 연결
- 공통 컬럼 `Key` 기준으로 연결
- 보통 `id` 값을 Key로 활용
- 특정 범위(Date 등)를 기준으로 연결 가능
- 문법보다 Table 구조와 관계 파악이 중요

```text
Table A
   │
   │ 공통 Key
   ↓
Table B
```

### JOIN Key

두 Table을 연결하는 공통 컬럼

```text
Table A.key
     =
Table B.key
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
- Table별 역할 분리
- 필요한 경우 JOIN으로 연결
- 분석에 필요한 형태로 재구성

```text
분리된 여러 Table
        ↓
       JOIN
        ↓
분석용 데이터
```

| 관점 | 특징 |
| --- | --- |
| 데이터 저장 | 여러 Table로 분리 |
| 목적 | 데이터 중복 최소화 |
| 데이터 분석 | 필요한 Table JOIN |
| 데이터 웨어하우스 | JOIN + 연산 후 데이터 마트 구성 |

---

<a id="join-type"></a>

# 03. JOIN의 종류

| JOIN | 기준 | 결과 |
| :---: | --- | --- |
| `INNER JOIN` | 양쪽 | 공통 데이터만 |
| `LEFT JOIN` | 왼쪽 | 왼쪽 데이터 모두 유지 |
| `RIGHT JOIN` | 오른쪽 | 오른쪽 데이터 모두 유지 |
| `FULL JOIN` | 양쪽 | 양쪽 데이터 모두 유지 |
| `CROSS JOIN` | 없음 | 모든 행 조합 |

---

## 03.1 INNER JOIN

두 Table에 공통으로 존재하는 데이터만 연결

```text
A ∩ B
```

```sql
SELECT
    A.col1,
    B.col2
FROM table_a AS A
INNER JOIN table_b AS B
    ON A.key = B.key;
```

### 핵심

- 교집합
- 양쪽 모두 Key 존재 필요
- 일치하지 않는 데이터 제외

---

## 03.2 LEFT JOIN

왼쪽 Table 기준으로 연결

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
- 오른쪽 연결값 없으면 `NULL`

> [!TIP]
> 처음 학습 시 `LEFT JOIN` 중심으로 이해

---

## 03.3 RIGHT JOIN

오른쪽 Table 기준으로 연결

```sql
SELECT
    A.col1,
    B.col2
FROM table_a AS A
RIGHT JOIN table_b AS B
    ON A.key = B.key;
```

### 핵심

- 오른쪽 데이터 모두 유지
- `LEFT JOIN`과 기준 방향 반대

---

## 03.4 FULL JOIN

양쪽 Table의 데이터 모두 유지

```sql
SELECT
    A.col1,
    B.col2
FROM table_a AS A
FULL JOIN table_b AS B
    ON A.key = B.key;
```

### 핵심

- 양쪽 데이터 모두 포함
- 연결값 없는 부분 → `NULL`

---

## 03.5 CROSS JOIN

두 Table의 모든 행을 서로 조합

```sql
SELECT
    A.col1,
    B.col2
FROM table_a AS A
CROSS JOIN table_b AS B;
```

### 핵심

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

| 목적 | JOIN |
| --- | :---: |
| 공통 데이터만 | `INNER JOIN` |
| 왼쪽 기준 유지 | `LEFT JOIN` |
| 오른쪽 기준 유지 | `RIGHT JOIN` |
| 양쪽 모두 유지 | `FULL JOIN` |
| 모든 조합 | `CROSS JOIN` |

```text
교집합
→ INNER JOIN

기준 Table 유지
→ LEFT / RIGHT JOIN

양쪽 모두
→ FULL JOIN

모든 조합
→ CROSS JOIN
```

> [!IMPORTANT]
> JOIN 종류부터 선택 X  
> → 원하는 결과 형태부터 결정

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
⑥ 결과 검증
```

| 단계 | 내용 |
| :---: | --- |
| Table 확인 | 저장 데이터와 컬럼 확인 |
| 기준 Table | Base Table 결정 |
| JOIN Key | `ON`에 사용할 공통 Key 확인 |
| 결과 예상 | 결과 Table 구조 미리 확인 |
| Query 작성 | JOIN SQL 작성 |
| 결과 검증 | 예상 결과와 실제 결과 비교 |

### 핵심 흐름

```text
무엇을 구할 것인가?
        ↓
기준 Table은?
        ↓
연결할 Table은?
        ↓
공통 Key는?
        ↓
결과 형태는?
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
    B.col1,
    B.col2
FROM table_a AS A
LEFT JOIN table_b AS B
    ON A.key = B.key;
```

| 문법 | 역할 |
| :---: | --- |
| `FROM` | 기준 Table |
| `JOIN` | 연결할 Table |
| `ON` | 연결 조건 |
| `AS` | Alias |

---

## Alias

긴 Table 이름을 짧게 표현

```sql
FROM table_a AS A
LEFT JOIN table_b AS B
    ON A.key = B.key;
```

### 장점

- 코드 길이 감소
- 컬럼 출처 구분
- JOIN Query 가독성 향상

---

<a id="multi-join"></a>

# 07. 여러 테이블 JOIN

여러 Table 연속 연결 가능

```sql
SELECT
    A.col1,
    B.col2,
    C.col3
FROM table_a AS A
LEFT JOIN table_b AS B
    ON A.key = B.key
LEFT JOIN table_c AS C
    ON A.key = C.key;
```

```text
             ┌── Table B
             │
Table A ─────┤
             │
             └── Table C
```

### 유의점

- JOIN 개수 자체에 제한 없음
- 불필요하게 많은 JOIN 여부 확인
- 필요한 Table만 연결
- 각 JOIN의 Key 정확히 확인

---

<a id="join-caution"></a>

# 08. JOIN에서 헷갈리기 쉬운 부분

## ① 어떤 JOIN 사용?

```text
교집합
→ INNER JOIN

기준 Table 유지
→ LEFT JOIN

모든 조합
→ CROSS JOIN
```

---

## ② 어떤 Table을 왼쪽에 배치?

`LEFT JOIN`

→ 기준 Table을 왼쪽에 배치

```sql
FROM base_table AS A
LEFT JOIN additional_table AS B
    ON A.key = B.key;
```

```text
Base Table
    ↓
추가 정보 연결
```

---

## ③ 여러 Table 연결 가능?

가능

```sql
FROM table_a AS A
LEFT JOIN table_b AS B
    ON A.key = B.key
LEFT JOIN table_c AS C
    ON A.key = C.key;
```

단,

- 필요한 JOIN인지 확인
- Key 중복 여부 확인
- 예상 행 수와 실제 행 수 비교

---

## ④ 모든 컬럼 선택 필요?

필요 없음

### JOIN 확인 단계

```sql
SELECT
    A.*,
    B.*
FROM table_a AS A
LEFT JOIN table_b AS B
    ON A.key = B.key;
```

### 실제 사용 단계

필요한 컬럼만 선택

```sql
SELECT
    A.id,
    A.col1,
    B.col2
FROM table_a AS A
LEFT JOIN table_b AS B
    ON A.key = B.key;
```

### BigQuery 유의점

- 불필요한 컬럼 제외
- 처리 데이터 감소
- 비용 절감 가능
- `id` → Unique 여부 확인에 활용

---

<a id="join-null"></a>

# 09. NULL

`NULL`

→ 값이 없거나 알 수 없는 상태

```text
NULL ≠ 0
NULL ≠ ""
NULL ≠ 공백
```

### JOIN에서 NULL 발생

```text
기준 Table에는 값 존재
+
연결 Table에는 일치하는 Key 없음

→ NULL
```

| 값 | 의미 |
| :---: | --- |
| `0` | 숫자 0 |
| `''` | 빈 문자열 |
| 공백 | 공백 문자 |
| `NULL` | 값 자체 없음 / 알 수 없음 |

---

<a id="join-summary"></a>

# 10. JOIN 핵심 정리

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

## 🔥 JOIN 기본 구조

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
→ 연결 Table

ON
→ 공통 Key
```

---

## JOIN 작성 순서

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
결과 검증
```

---

## 최종압축

```text
JOIN == 기준 Table + 공통 Key + 필요한 다른 Table의 정보
```

---

# 2️⃣ 수행 인증란

아래 중 하나 이상을 첨부해주세요.

- 강의 수강 화면 캡처
- JOIN 문제 풀이 정답 화면 캡처
- JOIN 전후 결과 row 수를 비교한 캡처 또는 메모

---

# 3️⃣ 확인 문제

프로그래머스는 로그인이 필요하므로, 로그인 후 문제 풀이를 진행해주세요.

## 🧩 문제 1

문제 링크: [조건에 맞는 도서와 저자 리스트 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/144854)

풀이 과정:

```
- 사용한 테이블:
- JOIN key:
- 날짜를 출력한 형식:
```

<!-- 정답을 맞추게 되면, 정답입니다. 이 부분을 캡처해서 이 주석을 지우시고 첨부해주시면 됩니다. -->

## 🧩 문제 2

문제 링크: [상품 별 오프라인 매출 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131533)

풀이 과정:

```
- 연결한 테이블:
- JOIN key:
- 집계한 값:
```

<!-- 정답을 맞추게 되면, 정답입니다. 이 부분을 캡처해서 이 주석을 지우시고 첨부해주시면 됩니다. -->

## 🧩 문제 3

문제 링크: [없어진 기록 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/59042)

풀이 과정:

```
- 기준이 되는 왼쪽 테이블:
- LEFT JOIN이 필요한 이유:
- NULL이 발생하는 경우:
```

<!-- 정답을 맞추게 되면, 정답입니다. 이 부분을 캡처해서 이 주석을 지우시고 첨부해주시면 됩니다. -->

## 🧩 문제 4

문제 링크: [있었는데요 없었습니다](https://school.programmers.co.kr/learn/courses/30/lessons/59043)

풀이 과정:

```
- 사용한 테이블:
- JOIN key:
- 비교한 날짜 컬럼:
- 정렬 기준:
```

<!-- 정답을 맞추게 되면, 정답입니다. 이 부분을 캡처해서 이 주석을 지우시고 첨부해주시면 됩니다. -->

---

# 4️⃣ 참고 자료

JOIN 개념이 헷갈린다면 아래 자료를 함께 참고해도 좋습니다.

1. https://data-marketing-bk.tistory.com/entry/SQL-JOIN-%ED%95%9C-%EB%B0%A9%EC%97%90-%EC%A0%95%EB%A6%AC-%EA%B0%9C%EB%85%90%EB%B6%80%ED%84%B0-%EC%BD%94%EB%93%9C%EA%B9%8C%EC%A7%80-%EC%9D%B4%EA%B2%83%EB%A7%8C-%EB%B3%B4%EC%9E%90
2. https://velog.io/@wijoonwu/JOIN

---

# 5️⃣ 이번 주 회고

```
1. INNER JOIN과 LEFT JOIN의 차이를 어떻게 이해했는지:
2. JOIN 문제를 풀 때 가장 먼저 확인해야 할 것:
3. JOIN 결과가 예상과 달랐던 경험이 있다면:
```

수고하셨습니다!




