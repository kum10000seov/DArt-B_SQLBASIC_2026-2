# 📘 SQL_BASIC 3주차 정규 과제

SQL_BASIC 정규 과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고, 핵심 개념을 정리한 뒤 간단한 SQL 문제를 직접 풀어보는 방식으로 진행합니다.

이번 주는 집계 함수와 `GROUP BY`, `HAVING`을 학습합니다.

완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**👀 수행 인증란은 필수입니다.**

---

## 📚 SQL_BASIC_3rd_TIL

### 섹션 3. 데이터 탐색 - 조건, 추출, 요약

### 2-5. 집계(GROUP BY + HAVING + SUM/COUNT)

### 2-7. 정리

### 2-8. 새로운 집계 함수 소개(GROUP BY ALL, 2024-02-26에 나온 함수)

---

## ✨ 선택 강의

- 2-6. 연습 문제: 집계와 조건 조회를 더 연습하고 싶을 때 선택 수강

---

## 🏁 전체 강의 수강 계획

| 주차 | 필수 강의 범위 | 선택 강의 | 완료 여부 |
| --- | --- | --- | --- |
| 1주차 | 1-1 ~ 2-1 | 1 | ✅ |
| 2주차 | 2-2 ~ 2-3 | 2-4 | ✅ |
| 3주차 | 2-5, 2-7 ~ 2-8 | 2-6 | ✅ |
| 4주차 | 3-2 ~ 4-3 | 3-4 | 🍽️ |
| 5주차 | 4-4 ~ 4-6 | 4-5, 4-7 | 🍽️ |
| 6주차 | 5-2 ~ 5-5 | 5-6 | 🍽️ |
| 7주차 | 필수 강의 없음 | 6-2, 6-3, 6-4, 6-5 | 🍽️ |

---

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념 정리

아래 키워드 중 중요하다고 생각한 개념을 2개 이상 골라 짧게 정리해주세요. 3개보다 더 많이 정리하고 싶다면 자유롭게 항목을 추가해도 좋습니다.

이번 주 키워드:
- COUNT
- SUM
- AVG
- MAX
- MIN
- GROUP BY
- HAVING
- 집계 기준

## 01.

```
개념 이름: WHERE
개념 설명: Table에 바로 조건을 설정하고 싶을 경우 사용
예시 쿼리: SELECT
            컬럼1, 컬럼2,
            COUNT(컬럼1) AS col1_count
          FROM <table>
          WHERE
            컬럼1 >= 3
```

## 02.

```
개념 이름: HAVING
개념 설명: GROUP BY한 후 조건을 설정하고 싶은 경우 사용
예시 쿼리: SELECT
            컬럼1, 컬럼2,
            COUNT(컬럼1) AS col1_count
           FROM <table>
           GROUP BY 컬럼1, 컬럼2
           HAVING
             col1_count >3    
```

## (선택) 03.

```
개념 이름: GROUP BY, ORDER BY
개념 설명: GROUP BY는 같은 값끼리 모아서 그룹화하기 위한 함수이고, ORDER BY는 정렬을 위한 함수로서 기본값으로는 오름차순(작은것부터)이나 내림차순(큰것부터) 정렬이 필요할 경우 DESC를 입력토록 한다.
예시 쿼리: SELECT
            type1,
            COUNT(id) AS cnt
           FROM basic.pokemon
           GROUP BY
            type 1
           HAVING cnt >= 10
           ORDER BY cnt DESC    
```

---

# 2️⃣ 수행 인증란

아래 중 하나 이상을 첨부해주세요.

<img width="284" height="67" alt="image" src="https://github.com/user-attachments/assets/0fabb6d8-e28e-4c37-93d4-0e65f8eb51ea" />
<img width="282" height="119" alt="image" src="https://github.com/user-attachments/assets/66facf96-15f5-496d-b920-36b6e9ec5658" />


---

# 3️⃣ 확인 문제

프로그래머스는 로그인이 필요하므로, 로그인 후 문제 풀이를 진행해주세요.

## 🧩 문제 1

문제 링크: [최댓값 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/59415)

풀이 과정:

```
- 문제 요구사항: 가장 최근에 들어온 동물이 들어온 "시점"
- 사용한 SQL 절
SELECT DATETIME AS 시간
FROM ANIMAL_INS
ORDER BY DATETIME DESC
LIMIT 1;
- 새로 배운 점: 가장 최근의 시간이라는 것을 SQL로 표현하자면 내림차순으로서 가장 먼저 나오는 시간 1개이다.
```

<img width="1265" height="694" alt="image" src="https://github.com/user-attachments/assets/4a47bdfa-bdfd-4076-8791-35fc6b6fe5ff" />

## 🧩 문제 2

문제 링크: [가장 비싼 상품 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131697)

풀이 과정:

```
- 사용한 집계 함수: MAX
- 집계 대상 컬럼: PRICE
- 결과를 검증한 방법: ORDER BY PRICE DESC LIMIT 1로 정렬 후 최상단 1개 행의 판매가를 직접 조회하여 MAX(PRICE) 결과값과 일치하는지 확인
```

<img width="1265" height="695" alt="image" src="https://github.com/user-attachments/assets/f711bf9e-a7fb-495a-ac00-e01a16f57762" />


## 🧩 문제 3

문제 링크: [고양이와 개는 몇 마리 있을까](https://school.programmers.co.kr/learn/courses/30/lessons/59040)

풀이 과정:

```
- 그룹화 기준:
- WHERE와 HAVING 중 사용한 절:
- 처음 틀렸다면 틀린 이유:
- 새로 배운 SQL 패턴:
```

<!-- 정답을 맞추게 되면, 정답입니다. 이 부분을 캡처해서 이 주석을 지우시고 첨부해주시면 됩니다. -->

---

# 4️⃣ 이번 주 회고

```
1. 문제를 SQL로 옮길 때 가장 어려웠던 부분:
2. WHERE와 HAVING의 차이를 어떻게 이해했는지:
3. 다음 주에 더 연습하고 싶은 문제 유형:
```

수고하셨습니다!




