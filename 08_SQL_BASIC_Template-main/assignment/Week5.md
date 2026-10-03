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


---

# 2️⃣ 수행 인증란




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




