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

## Funções relacionadas a strings

### INITCAP

*   Retorna a string original com o primeiro caracter de todas as palavras em maiúsculo.

```sql
SELECT INITCAP('that Is a teste') FROM DUAL;
-- >> That Is A Teste
```

### INSTR

*   Retorna a posição de ocorrência da n-ésima substring. Caso não encontre, retorna zero.
*   1º argumento numérico diz referência ao início da procura.
*   2º argumento numérico indica qual a ocorrência.

```sql
SELECT INSTR('Hello Hello', 'el', 1, 2) FROM DUAL;
-- >> 8
```

### LENGTH

*   Retorna o tamanho de uma string.

```sql
SELECT LENGTH('Teste do Teste!') FROM DUAL;
-- >> 15
```

### LOWER & UPPER

*   LOWER: retorna a string toda em minúsculo.
*   UPPER: retorna a string toda em maiúsculo.

```sql
SELECT LOWER('HELLO, FRIENDS') FROM DUAL;
-- >> hello, friends
SELECT UPPER('hello, FRIENDS') FROM DUAL;
-- >> HELLO, FRIENDS
```

### LPAD & RPAD

*   LPAD: retorna uma string preenchida com caracteres à esquerda até completar n caracteres.
*   RPAD: retorna uma string preenchida com caracteres à direita até completar n caracteres.

```sql
SELECT LPAD('Ohayo!', 9, '*!') FROM DUAL;
-- >> *!*Ohayo!
SELECT RPAD('Ohayo!', 9, '!') FROM DUAL;
-- >> Ohayo!!!!
```

### LTRIM & RTRIM

*   LTRIM: retorna uma string com remoção de um conjunto de caracteres à esquerda.
*   RTRIM: retorna uma string com remoção de um conjunto de caracteres à direita.
*   TRIM: retorna uma string com remoção de um conjunto de caracteres.
    *   Opções: LEADING, TRAILING, BOTH.
*   Caso o conjunto de caracteres seja omitido, assume-se um espaço em branco.

```sql
SELECT LTRIM('T Teste de Aceitação', 'Teste de ') FROM DUAL;
-- >> Aceitação
SELECT RTRIM('###Walle-Sama###', '#') FROM DUAL; 
-- >> ###Walle-Sama
SELECT TRIM(BOTH '#' FROM '###Walle-Sama###') FROM DUAL;
```

### REGEXP_REPLACE

*   Retorna uma string com substituições que coincidem com a regexp.

*   Referência: https://docs.oracle.com/cd/B19306_01/server.102/b14200/functions130.htm#i1305521

```sql
-- Nesse caso, poderia colocar 0-9 no lugar de [:digit:].
SELECT
    REGEXP_REPLACE('555.555.5555', '([[:digit:]]{3})\.([[:digit:]]{3})\.([[:digit:]]{4})', '(\1) \2-\3')
FROM
    DUAL;
-- >> (555) 555-5555
```


### REGEXP_SUBSTR

*   Retorna a string a qual coincide com a regexp, diferente da `REGEXP_INSTR` que retorna a posição, esse método retorna a própria string.

```sql
-- Texto entre duas vírgulas
SELECT
    REGEXP_SUBSTR('500 Oracle Parkway, Redwood Shores, CA', ',[^,]+,')
FROM
    DUAL;
-- >> , Redwood Shores,
```

### SUBSTR

*   Retorna uma substring de uma string.
*   Necessário passar o índice inicial da substring.
*   A extensão (tamanho) da substring é opcional.
*   Lembrar que a string em PL/SQL começa no índice 1.

```sql
-- Substring que começa no índice 1 e tem extensão de 4 caracteres.
SELECT SUBSTR('just a test.', 1, 4) FROM DUAL;
-- >> just
-- Transformar a primeira letra em maísculo
SELECT UPPER(SUBSTR('just a test.', 1, 1))||SUBSTR('just a test', 2) FROM DUAL;
-- >> Just a test.
```

## Funções relacionadas a datas

### ADD_MONTHS

*   Retorna uma data acrescida de n meses.

```sql
SELECT ADD_MONTHS(SYSDATE, 2) FROM DUAL;
-- >> 09/22/2020 (Query no dia 07/22/2020)
-- SELECT SYSDATE+1 FROM DUAL; -- adiciona um dia
```

### TO_CHAR

*   Retorna uma data (string) formatada de acordo com um padrão.

```sql
SELECT TO_CHAR(SYSTIMESTAMP, 'DD-MM-YYYY HH24:MI:SS') FROM DUAL;
-- >> 22-07-2020 14:11:32 (sem timezone)
```

### EXTRACT

*   Retorna a extração de um período de uma data.
*   Opções: YEAR, MONTH, DAY, HOUR, MINUTE, SECOND.

```sql
SELECT EXTRACT(DAY FROM SYSDATE) FROM DUAL;
-- >> 22 (Query no dia 07/22/2020)
SELECT EXTRACT(HOUR FROM SYSTIMESTAMP) FROM DUAL;
-- >> 14 (Query no dia 07/22/2020 às 11h08, sem timezone)
```

## Funções auxiliares

### GREATEST & LEAST

```sql
SELECT GREATEST ('HARRY', 'HARRIOT', 'HAROLD') FROM DUAL;
-- >> HARRY
SELECT LEAST (SYSDATE-1, SYSDATE, SYSDATE+1) FROM DUAL;
-- >> 07/21/2020 (Query no dia 07/22/2020)
```
