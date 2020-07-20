# Funções úteis (ORACLE SQL)

## Funções relacionadas a valores nulos

### COALESCE

*   Retorna o primeiro elemento não nulo da lista.
*   Se todos os elementos são nulos, retorna nulo.

```sql
SELECT COALESCE(null, 'Hallo', null) FROM DUAL;
-- >> Hallo
```

### NVL

*   Caso o valor seja nulo, retorna um default, caso contrário, retorna o valor.

```sql
SELECT nvl(null, 10) FROM DUAL;
-- >> 10
SELECT nvl(null, 'Valor nulo') FROM DUAL;
-- >> Valor nulo
```

### NVL2

*   Caso o valor informado não seja nulo, retorna o 2º argumento, caso contrário, retorna o 3º argumento.

```sql
SELECT NVL2(10.0, 'Not null', 'Null') FROM DUAL;
-- >> Not null
SELECT NVL2(null, 'Not null', 'Null') FROM DUAL;
-- >> Null
```

## Funções relacionadas a números

### ABS

*   Retorna o valor absoluto do número.

```sql
SELECT ABS(-15) FROM DUAL;
-- >> 15
```

### CEIL

*   Arredonda o número para cima.

```sql
SELECT CEIL(10.3) FROM DUAL;
-- >> 11
```

### FLOOR

*   Arredonda o número para baixo.

```sql
SELECT FLOOR(10.3) FROM DUAL;
-- >> 10
```

### MOD

*   Retorna o resto da divisão entre dois números.
*   Há outra função chamada `REMAINDER`, a diferença é que `MOD` utiliza `FLOOR` e `REMAINDER` utiliza `ROUND`.

```sql
SELECT MOD(10, 3) FROM DUAL;
-- >> 1
```

### NANVL

*   Retorna o primeiro valor que é número (float ou double), isto é, não é NaN.
*   Útil para mapear NaN para NULL.

```sql
select NANVL(10.3, 0.7) from dual;
-- >> 10.3
```

### ROUND

*   Retorna o número arredondado com a precisão informada.
*   Caso não informe a precisão, assume-se 0.

```sql
select ROUND(1.5) FROM DUAL;
-- >> 2
select ROUND(15.17, 1) FROM DUAL;
-- >> 15.2
SELECT ROUND(14.932, -1) FROM DUAL; -- Arredonda 1.4932 e multiplica por 10
-- >> 10
```

### TRUNC

*   Retorna o número truncado (quebrado) para a quantidade de casas decimais.

```sql
SELECT TRUNC(15.79,1) "Truncate" FROM DUAL;
-- >> 15.7
```
