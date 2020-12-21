>DECODE 函数

```
SQL> select ename,job, sal,decode(job,'PRESIDENT',1.1*sal,
  2                                   'MANAGER',1.2*sal,
  3                                   'CLERK',1.3*sal,
  4                                           1.4*sal)涨后薪水
  5  from emp;

ENAME      JOB             SAL       涨后薪水
---------- --------- --------- ----------
SMITH      CLERK        800.00       1040
ALLEN      SALESMAN    1600.00       2240
WARD       SALESMAN    1250.00       1750
JONES      MANAGER     2975.00       3570
MARTIN     SALESMAN    1250.00       1750
BLAKE      MANAGER     2850.00       3420
CLARK      MANAGER     2450.00       2940
KING       PRESIDENT   5000.00       5500
TURNER     SALESMAN    1500.00       2100
JAMES      CLERK        950.00       1235
FORD       ANALYST     3000.00       4200
MILLER     CLERK       1400.00       1820
jack_1234              2000.00       2800

13 rows selected
```