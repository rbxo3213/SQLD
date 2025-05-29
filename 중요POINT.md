# RANGE 와 ROW 의 차이점

- ORDER BY SALARY RANGE UNBOUNDED PRECEDING -> 다른행이더라도 SALARY값이 같으면, 모두 합한 누적합을 구함
- SALARY열의 값이 1000, 2000, 2000 일때, 누적합이 1000, 3000, 5000 이 되는 게 아니라, 1000, 5000, 5000 이 됨.
- 그러면 1000, 3000, 5000 이 되게 하려면??
- ROW UNBOUNDED PRECEDING 을 사용하면 됨. ROW 는 물리적인 행 기준 임!!
- ORDER BY SALARY ROW UNBOUNDED PRECEDING 하면 1000, 3000, 5000 누적합 행이 나오게 됨.

# 걍 외우자

1. RATIO_TO_REPORT: 파티션 별 합계에서 차지하는 비율을 구하는 함수, SQL SERVER(MSSQL) 에서는 지원 X

- SCORE/SUM(SCORE) = "SCORE/SUM" 이 행과 같은 결과를 출력함

2. PERCENT_RANK: 해당 파티션의 맨 위 행을 0, 맨 아래 행을 1로 놓고, 현재 행이 위치하는 백분위 순위 값을 구하는 함수. MSSQL지원X

- PERCENT_RANK = (RANK-1)/(COUNT-1)

3. CUME_DIST: 해당 파티션에서의 누적 백분율을 구하는 함수(CUMULATIVE), 결과값은 0보다 '크고' 1보다 작거나 같은 값을 가짐. MSSQL 지원 X

- CUME_DIST = COUNT/TOTAL_COUNT

4. NTILE: 주어진 수 만큼 행들을 n등분한 후 현재 행에 해당하는 등급을 구하는 함수.

- CUME_DIST만 0초과. RATIO_TO_REPORT와 PERCENT_RANK는 0이상(0포함)

# 계층 함수

- CONNECT_BY_ROOT 컬럼 : 루트 노드의 주어진 컬럼 값을 반환
- CONNECT_BY_ISLEAF : 가장 하위노드인 경우 1을 반환하고 그 외에는 0을 반환 - LEAF 노드 여부
- ORDER SIBLINGS BY 절을 사용해야 같은 레벨끼리 정렬된다.

# 그외

- ROWNUM은 슈도 컬럼으로 OVER절 붙이면 에러 발생.

- 참조 무결성 제약 조건인 RESTRICT와 NO ACTION은 시점에서 차이가 있음. RESTRINT: 즉시, NO ACTION: 커밋시

- CTAS는 컬럼명, 컬럼타입을 다시 명시하지 않아도 되는 장점이 있으나, 제약조건은 NOT NULL만 복사되고 FOREIGN KEY, PRIMARY KEY등 다시 지정해줘야함.

## DDL

- ALTER에서, ADD, MODIFY는 COLUMN 없이 사용, DROP, RENAME은 COLUMN과 같이 사용해야 함. DROP COLUMN, RENAME COLUMN
- MODIFY는 ALTER TABLE EMPLOYEE MODIFY (HP NUMBER DEFAULT 1 NOT NULL); 이런식으로 씀.
  - HP의 타입을 NUMBER로, DEFAULT값은 1, NOT NULL 제약조건
- RENAME은 ALTER TABLE EMPLOYEE RENAME COLUMN HP TO HP_NO; 이런식으로 씀. <- 컬럼명 변경
- RENAME TABLE TEACHER TO NEW_TEACHER; <- 이건 테이블명 변경

- PRIMARY KEY(기본키) 는 테이블 당 하나씩만 생성할 수 있고, 생성을 하지 않는 것도 가능함!!(필수 아니라는 뜻)
