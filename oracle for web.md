## oracle 实现分页查询  （基于一定的嵌套查询）
# 没有order by 排序方法
select * from (
   select rownum as rowno, T.* from k_task where Filght_date between to_date('20060501','yyyymm') and rownum <=20
 ) tbale_alls.rowno >=10

# 有order by排序的方法 （但是查询的范围大时）
SELECT *
 FROM (SELECT TT.*, ROWNUM AS ROWNO
      FROM (Select t.*
          from k_task T
          where flight_date between to_date('20060501', 'yyyymmdd') and
             to_date('20060531', 'yyyymmdd')
          ORDER BY FACT_UP_TIME, flight_no) TT
     WHERE ROWNUM <= 20) TABLE_ALIAS
where TABLE_ALIAS.rowno >= 10