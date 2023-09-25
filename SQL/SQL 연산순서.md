SQL에서 연산순서는 FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY의 순서로 이뤄집니다.

FROM : 어떤 테이블을 시작으로~
WHERE : 그 테이블 내에서 '조건'을 정하고자 할 때 쓰는 명령어입니다.
WHERE도 있고 CASE도 있고 WHEN도 있는데요.
GROUP BY : FROM, WHERE을 마친 후 결과들을 그룹화 해라
HAVING : 그룹화된 것 내에서도 조건을 정하고자 할 때 사용
SELECT : 위 절차를 다 마친 후, 해당되는 칼럼들의 결과를 출력하라
ORDER BY : 그 때 출력 결과를 오름차순 정렬할건지, 내림차순 정렬할건지 적어라 (기재하지 않으면 오름차순이 디폴트)