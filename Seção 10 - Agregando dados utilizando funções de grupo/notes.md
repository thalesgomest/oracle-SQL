# SQL Fundamentos - Agregando dados utilizando funções de grupo

## Objetivos

-   Conhecer as principais funções de grupo disponíveis no Oracle Database
-   Utilizar as funções de grupo
-   Agrupar dados utilizando a cláusula `GROUP BY`
-   Incluir ou excluir dados nulos utilizando a cláusula `HAVING`

### Principais Funções de Grupo

| Função   | Descrição                                        |
| -------- | ------------------------------------------------ |
| COUNT    | Retorna o número de linhas de uma coluna         |
| SUM      | Retorna a soma de valores de uma coluna          |
| AVG      | Retorna a média de valores de uma coluna         |
| MIN      | Retorna o menor valor de uma coluna              |
| MAX      | Retorna o maior valor de uma coluna              |
| VARIANCE | Retorna a variância de valores de uma coluna     |
| STDDEV   | Retorna o desvio padrão de valores de uma coluna |

### Sintaxe para utilização das funções de grupo

```sql
SELECT funçao_grupo(coluna),...
FROM tabela
[WHERE condição]
[ORDER BY coluna];
```

### Tratamento de NULOS em Funções de Grupo

O Oracle Database não considera valores nulos em funções de grupo, ou seja, se uma coluna possuir valores nulos, o Oracle Database não irá considerar esses valores na função de grupo. Logo a média aritmética de uma coluna que possui valores nulos será diferente da média aritmética de uma coluna que não possui valores nulos.

```sql
SELECT AVG(commission_pct)
FROM   employees;
```

A seguir, temos um exemplo de como tratar valores nulos em funções de grupo. Neste exemplo, utilizamos a função `NVL` para substituir os valores nulos por zero.

```sql
SELECT AVG(NVL(commission_pct, 0))
FROM   employees;
```

### Criando Grupos utilizando a Cláusula `GROUP BY`

A cláusula `GROUP BY` é utilizada para agrupar dados de uma tabela. A cláusula `GROUP BY` é utilizada em conjunto com as funções de grupo.

```sql
SELECT coluna, função_grupo(coluna)
FROM   tabela
[WHERE condição]
[GROUP BY expressão_group_by]
[ORDER BY coluna];
```

-   Regra: Se o comando `SELECT` utiliza Grupos, então todas as ou expresões na lista da cláusula `SELECT` que não estão em uma função de grupo devem estar na cláusula `GROUP BY`.

```sql
SELECT department_id, job_id, SUM(salary)
FROM employees
GROUP BY department_id, job_id
ORDER BY department_id, job_id;
```

Neste exemplo os únicos dados que eu posso exibir na cláusula `SELECT` são os dados que estão na cláusula `GROUP BY`.

### Excluindo dados nulos utilizando a cláusula `HAVING`

A cláusula `WHERE` não pode referenciar funções de grupo. Para isso, utilizamos a cláusula `HAVING`.

```sql
SELECT department_id, MAX(salary)
FROM employees
GROUP BY department_id
HAVING MAX(salary)>10000;
```

## Sequência Lógica

1. `WHERE` <br> Seleciona as linahs a serem recuperadas
2. `GROUP BY` <br> Agrupa as linhas a serem recuperadas. Forma os grupos
3. `HAVING` <br> Seleciona os grupos a serem recuperados
4. Exibir colunas ou expressões do `SELECT` ordenando pelo critério da cláusula `ORDER BY`

Abaixo temos um exemplo que utiliza todas as cláusulas da nossa sequência lógica.

```sql
SELECT job_id, SUM(salary) TOTAL
FROM   employees
WHERE  job_id <> 'SA_REP'
GROUP BY job_id
HAVING   SUM(salary) > 10000
ORDER BY SUM(salary);
```

## Aninhando Funções de Grupo

É possível aninhar funções de grupo. Por exemplo, podemos utilizar a função `AVG` para calcular a média de salários agrupados por departamento e depois utilizar a função `MAX` para calcular o maior salário de cada departamento.

```sql
SELECT department_id, MAX(AVG(salary))
FROM employees
GROUP BY department_id;
```
