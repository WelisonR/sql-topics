# Funções Analíticas

Características das funções analíticas:

*   São aplicadas após joins, WHERE, GROUP BY e HAVING, porém, antes do último ORDER BY executado.
*   Evitam a redução de linhas, isto é, apresenta os dados linha a linha, diferente de uma cláusula `GROUP BY` que agrupa os dados.

Sintaxe:

```sql
analytic_function([ arguments ]) OVER (analytic_clause)


analytic_clause: [ query_partition_clause ] [ order_by_clause [ windowing_clause ] ]
```

Alguns casos:

*   `AVG(sal) OVER ()`: faz a média de todas as linhas.
*   `AVG(sal) OVER (PARTITION BY deptno)`: a média será SOBRE a partição departamento, isto é, apresentará a média salarial do departamento.
*   `FIRST_VALUE(sal IGNORE NULLS) OVER (PARTITION BY deptno ORDER BY sal ASC NULLS LAST)`: se a ordem importar, utilizar `ORDER BY`.
    *   Nesse caso `NULLS LAST` é desnessário, faria-se necessário se a ordenação fosse decrescente.

Referências:

*   https://oracle-base.com/articles/misc/analytic-functions
*   https://www.youtube.com/watch?v=TMGh9H0iqb0

## RANK

*   Realiza uma enumeração.
*   Observar que, caso a coluna tenha dois valores iguais (empate), ambas estão no mesmo `RANK` e o próximo número será o `atual+2` (há gap).

### Exemplo de Código

*   Exemplo simples:

```sql
SELECT deptno, empno, ename, sal,
    RANK() OVER (ORDER BY sal) AS row_rank
FROM emp;

-- DEPTNO	EMPNO	    ENAME	    SAL	    ROW_RANK
-- 20      7369        SMITH       800         1
-- 30      7900        JAMES       950         2
-- 20      7876        ADAMS       1100        3
-- 30      7521        WARD        1250        4
-- 30      7654        MARTIN      1250        4
-- 10      7934        MILLER      1300        6
-- 30      7844        TURNER      1500        7
-- 30      7499        ALLEN       1600        8
-- 10      7782        CLARK       2450        9
-- 30      7698        BLAKE       2850        10

```

*   Exemplo com a utilização de `PARTITION`:

```sql
SELECT *
FROM (SELECT deptno, empno, ename, sal,
        RANK() OVER (PARTITION BY deptno ORDER BY sal) AS row_rank
    FROM emp)
WHERE row_rank <= 1;

-- Equivalente: SELECT MIN(sal) FROM emp GROUP BY deptno;

-- DEPTNO   EMPNO       ENAME       SAL     ROW_RANK
-- 10       7934        MILLER      1300        1
-- 20       7369        SMITH       800         1
-- 30       7900        JAMES       950         1
```

## DENSE RANK

*   Realiza uma enumeração.
*   Não deixa lacunas (gaps) na enumeração caso houver linhas com o mesmo valor para dadas colunas.

### Exemplo de código

```sql
SELECT deptno, empno, ename, sal,
    DENSE_RANK() OVER (ORDER BY sal) AS row_rank
FROM emp;

-- DEPTNO	EMPNO	    ENAME	    SAL	    ROW_RANK
-- 20      7369        SMITH       800         1
-- 30      7900        JAMES       950         2
-- 20      7876        ADAMS       1100        3
-- 30      7521        WARD        1250        4
-- 30      7654        MARTIN      1250        4
-- 10      7934        MILLER      1300        5
-- 30      7844        TURNER      1500        6
-- 30      7499        ALLEN       1600        7
-- 10      7782        CLARK       2450        8
-- 30      7698        BLAKE       2850        9
```

## ROW_NUMBER

*   Realiza uma enumeração sequencial mesmo se houver empates.

### Exemplo de código

```sql
SELECT deptno, empno, ename, sal,
    ROW_NUMBER() OVER (ORDER BY sal, hiredate) AS row_RANK
FROM emp;

-- DEPTNO	EMPNO	    ENAME	    SAL	    ROW_RANK
-- 20      7369        SMITH       800         1
-- 30      7900        JAMES       950         2
-- 20      7876        ADAMS       1100        3
-- 30      7521        WARD        1250        4
-- 30      7654        MARTIN      1250        5
-- 10      7934        MILLER      1300        6
-- 30      7844        TURNER      1500        7
-- 30      7499        ALLEN       1600        8
-- 10      7782        CLARK       2450        9
-- 30      7698        BLAKE       2850        10
```