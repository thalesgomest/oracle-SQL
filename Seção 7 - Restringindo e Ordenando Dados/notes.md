# Fundamentos SQL: Restringindo e Ordenando Dados

## Cláusula WHERE

Selecione as linhas que serão retornadas utilizando a cláusula `WHERE`

```SQL
SELECT *|{[DISTINCT] coluna|expressão [alais],...}
FROM tabela
[WHERE condição(s)]
```

Exemplo:

```SQL
SELECT employee_id, last_name, job_id, department_id
FROM employees
WHERE department_id = 60;
```

### Operadores de comparação

| Operador         | Significado                         |
| ---------------- | ----------------------------------- |
| =                | Igual a                             |
| >                | Maior que                           |
| >=               | Maior ou igual a                    |
| <                | Menor                               |
| <=               | Menor ou igual a                    |
| <>               | Diferente                           |
| BETWEEN...AND... | Entre dois valores (incluindo)      |
| IN(set)          | Que pertence a uma lista de valores |
| LIKE             | Possui um padrão de carater         |
| IS NULL          | É um valor nulo                     |

-   O operador LIKE é usado para realizar pesquisas de valores que coincidem com os padrões utilizando caracteres curingas (wildcards). As condições de pesquisa podem conter caracteres ou números:
    -   O caracter `%` combina com zero ou mais caracteres
    -   O caracter `_` combina com um e somente um caractere

A busca a seguir retorna somente os employees que começam com as letras Sa seguido de qualquer outro conjunto de caracteres.

```SQL
SELECT first_name, last_name, job_id
FROM employees
WHERE first_name LIKE 'Sa%';
```

A busca a seguir retorna os empregados cujo sobrenome tenha a primeira letra um caractere qualquer, a segunda letra igual a `a`, seguido de qualquer outro conjunto de caracteres.

```SQL
SELECT first_name, last_name
FROM employees
WHERE last_name LIKE '_a%';
```

## Comparações com NULL

Para fazer uma comparação com `NULL` utiliza-se a expressão `IS NULL`

```SQL
SELECT last_name, manager_id
FROM employees
WHERE manager_id IS NULL;
```

## Operadoradores Lógicos para condições

| Operador | RETORNO                                                                                                             |
| -------- | ------------------------------------------------------------------------------------------------------------------- |
| AND      | Retorna TRUE se ambas as condiçoes são verdadeiras                                                                  |
| OR       | Retorna TRUE se pelo menos uma das condições for verdadeira                                                         |
| NOT      | Retorna TRUE se a condição é falsa<br>Retorna FALSE se a condição é verdadeira<br>Retorna NULL se a condição é NULL |

## Cláusula ORDER BY

Usada para ordenar as linhas recuperadas

-   `ASC`: Ordena ascendente, default
-   `DESC`: Ordena descendente

A cláusula `ORDER BY` é a última no comando `SELECT`

```SQL
SELECT last_name, job_id, department_id, hire_date
FROM employees
ORDER BY hire_date DESC;
```

## Variável de substituição (`&`)

-   Utilize uma variável prefixada com um `&` para solicitar um prompt para o usuário digitar um valor.

```SQL
SELECT employee_id, last_name, salary, department_id
FROM employees
WHERE employee_id = &employee_id;
```

-   Utilize `&&` se você deseja reutilizar o valor da variável sem solitar um prompt para o usuário cada vez que referencia a variável.

-   Utilize áspas simples `''` para valores do tipo caractere

## Utilizando o comando DEFINE e UNDEFINE

Os comando `DEFINE` e `UNDEFINE` são usados respectivamente para criar/atribuir e remover a atribuição de um valor para uma variável
