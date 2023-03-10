# SQL Fundamentos: Utilizando Funções de Conversão e Expressões Condicionais

## Objetivos

-   Descrever os vários tipos de funções de conversão disponíveis no SQL
-   Utilizar as funções `TO_CHAR`, `TO_NUMBER` e `TO_DATE`
-   Expressões condicionais em um comando `SELECT`

## Tipos de Conversão

1. CONVERSÃO IMPLÍCITA

    |        DE        |       PARA       |
    | :--------------: | :--------------: |
    | VARCHAR2 ou CHAR |      NUMBER      |
    | VARCHAR2 ou CHAR |       DATE       |
    |      NUMBER      | VARCHAR2 ou CHAR |
    |       DATE       | VARCHAR2 ou CHAR |

2. CONVERSÃO EXPLÍCITA

    ![image](https://user-images.githubusercontent.com/97575616/213059331-ec09ad29-6e08-4d59-9a28-5baae352deb3.png)

### Função TO_CHAR com datas

```SQL
TO_CHAR(date, 'formato')
```

### Elementos de modelo de formatação de data

| FORMATO                  | RESULTADO                                             |
| ------------------------ | ----------------------------------------------------- |
| YYYY ou RRRR             | Ano com 4 dígitos                                     |
| MM                       | Mês com 2 dígitos                                     |
| DD                       | Dia do mês com 2 dígitos                              |
| MONTH                    | Nome do mês com 9 caracteres                          |
| MON                      | Nome do mês abreviado com 3 caracteres                |
| DAY                      | Dia da semana com 9 caracteres                        |
| DY                       | Dia da semana abreviado com 3 caracteres              |
| D                        | Dia da semana de 1 a 7                                |
| YEAR                     | Ano soletrado (em inglês)                             |
| CC                       | Século                                                |
| AC ou DC                 | Exibe se a data é Antes de Cristo ou Depois de Cristo |
| HH ou HH12               | Hora de 1 a 12                                        |
| HH24                     | Hora de 0 a 23                                        |
| MI                       | Minuto                                                |
| SS                       | Segundo                                               |
| Espaço, vírgula ou ponto | Espaços, vírgula ou ponto são inseridos no formato    |
| Texto                    | Insere o texto entre aspas duplas no formato          |
| SP                       | Números soletrados (inglês)                           |
| TH                       | Números em ordinal (inglês)                           |

**Exemplo**

```SQL
SELECT last_name,TO_CHAR(hire_date, 'DD/MM/YYYY  HH24:MI:SS') DT_ADMISSÂO
FROM employees;
```

### Função TO_CHAR com números

| FORMATO | RESULTADO                                                                                           |
| ------- | --------------------------------------------------------------------------------------------------- |
| 9       | Número com supressão de zeros a esquerda                                                            |
| 0       | Número incluindo zeros a partir da esquerda, na posição onde foi colocado o elemento de formato (0) |
| $       | Símbolo de moeda ($)                                                                                |
| L       | Símbolo de moeda definido pelo parâmetro NLS_CURRENCY                                               |
| .       | Decimal(.)                                                                                          |
| Milhar  | (,)                                                                                                 |
| D       | Símbolo de decimal definido de acordo com o parâmetro do banco de dados                             |
| G       | Símbolo de milhar definido de acordo com o parâmetro do banco de dados                              |

**Exemplo**

```SQL
SELECT first_name, last_name, TO_CHAR(salary, 'L99G999G999D99') SALARIO
FROM employees;
```

### Função TO_NUMBER

```SQL
TO_NUMBER(char[,'Formato'])
```

### Função TO_DATE

```SQL
TO_DATE(char[,'Formato'])
```

## Funções Aninhadas

Funções single-row podem ser aninhadas em vários níveis. A resolução é feita do níivel mais interno para o mais externo.

```SQL
F3(F2(F1(col,arg1),arg2),arg3)
```

**Exemplo**

```SQL
SELECT first_name, last_name, ROUND(MONTHS_BETWEEN(SYSDATE, hire_date),0) NUMERO_MESES
FROM   employees
WHERE  hire_date = TO_DATE('17/06/2003','DD/MM/YYYY');
```

## Funções Genéricas

-   `NVL(expr1, expr2)` <br>Se o argumento da expr1 for nulo, ele usa o argumento da expr2. Se não usa o próprio argumento da expr1
-   `NVL2(expr1, expr2, expr3)` <br>O primeiro argumento for nulo, ele usa o argumento da expr2. Se não for nulo, ele usa o argumento da expr3.
-   `NULLIF(expr1, expr2)` <br>Se os dois argumentos foram iguais ela retorna `NULL` se não ela retorna o primeiro
-   `COALESCE(expr1, expr2,...,exprn)` <br>Recebe uma lista de argumentos e retorna o primeiro argumento diferente de nulo.

## Expressões Condicionais

### Expressão Case

```SQL
CASE expr WHEN expr1 THEN
		return_expr1
		[WHEN expr2 THEN
		return_expr2
		WHEN exprn THEN
		return_exprn
		ELSE
		else_expr]
END alias;
```

**Exemplo**

```SQL
SELECT last_name, job_id, salary,
                          CASE job_id
                             WHEN 'IT_PROG'
                               THEN 1.10*salary
                             WHEN 'ST_CLERK'
                               THEN 1.15*salary
                             WHEN 'SA_REP'
                               THEN 1.20*salary
                             ELSE salary
                           END "NOVO SALARIO"
FROM employees;
```

### Função DECODE

```SQL
DECODE (col|expr, arg1, resulta1
		[,arg2, result2,...,]
		[,default])
```

**Exemplo**

```SQL
SELECT last_name, job_id, salary,
DECODE(job_id, 'IT_PROG' , 1.10*salary,
               'ST_CLERK', 1.15*salary,
               'SA_REP'  , 1.20*salary
                         , salary) "NOVO SALARIO"
FROM employees;
```
