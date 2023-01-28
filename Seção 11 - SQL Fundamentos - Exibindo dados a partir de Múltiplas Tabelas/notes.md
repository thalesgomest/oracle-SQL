# SQL Fundamentos: Exibindo dados a partir de Múltiplas Tabelas

## Tipos de Joins compatíveis com SQL ANSI 1999

### Qualificando nomes de colunas ambíguos

Para nomes ambíguos em tabelas distintas podemos utilizar o nome da tabela como prefixo. O uso de prefixos também é útil para melhorar a performance de consultas.

```sql
SELECT employees.employee_id, employees.last_name,
       employees.department_id, departments.department_name
FROM   employees JOIN departments
       ON (employees.department_id = departments.department_id);
```

Ao invés de prefixos com o nome completo da tabela podemos usar o alias da tabela.

```sql
SELECT e.employee_id, e.last_name, e.department_id, d.department_name
FROM   employees e JOIN departments d
ON     (e.department_id = d.department_id);
```

### Natural Joins

O uso de natural join é uma forma mais simples de fazer joins. O natural join faz o join entre as tabelas e automaticamente seleciona as colunas com o mesmo nome. Vale lembrar que se as colunas possuirem o mesmo nome porem de tipos diferentes o natural join não irá funcionar.

```sql
SELECT  department_id, department_name, location_id, city
FROM    departments
NATURAL JOIN locations;
```

Não é comum em desenvolvimento utilizar o natural join, pois ele pode trazer resultados inesperados.

### Join usando a cláusula USING

```sql
SELECT e.employee_id, e.last_name, d.location_id, department_id, d.department_name
FROM employees e
  JOIN departments d USING (department_id);
```

Não é comum em desenvolvimento utilizar a clásula `USING`. Não é possível prefixar uma coluna utilizada na cláusula `USING`.
Tanto a cláusula `USING` quanto o `NATURAL JOIN` são conhecimentos necessários mais pensando na certificação em Oracle Database do que para o dia a dia de desenvolvimento.

### Join usando a cláusula ON

A cláusula `ON` é a mais utilizada em desenvolvimento. Ela é mais flexível e permite a utilização de prefixos.

```sql
SELECT tabela.coluna, tabela.coluna
FROM tabela JOIN tabela
ON (condição_join);
```

### Join utilizando várias tabelas com a cláusula ON

```sql
SELECT e.employee_id, j.job_title, d.department_name, l.city, l.state_province, l.country_id
FROM employees e
  JOIN jobs        j ON (e.job_id = j.job_id)
  JOIN departments d ON (e.department_id = d.department_id)
  JOIN locations   l ON (d.location_id = l.location_id)
ORDER BY e.employee_id;
```

### Incluindo condições adicionais a condição de Join na cláusula WHERE

```sql
SELECT e.employee_id, e.last_name, e.salary, e.department_id, d.department_name
FROM employees e JOIN departments d
ON  (e.department_id = d.department_id)
WHERE (e.salary BETWEEN 10000 AND 15000);
```

### Self Join utilizando a cláusula ON

O self join é utilizado quando precisamos fazer um join entre uma tabela com ela mesma. O self join é muito utilizado em consultas de hierarquia.

```sql
SELECT empregado.employee_id "Id empregado", empregado.last_name "Sobrenome empregado",
       gerente.employee_id "Id gerente", gerente.last_name "Sobrenome gerente"
FROM employees empregado JOIN employees gerente
ON (empregado.manager_id = gerente.employee_id)
ORDER BY empregado.employee_id;
```

## Nonequijoins

São joins que não utilizam a cláusula `ON` para fazer o join. São utilizados para fazer joins entre tabelas que não possuem uma coluna em comum. Isto é, são joins onde a condição de join não é uma igualdade.

```sql
SELECT   e.employee_id, e.salary, j.grade_level, j.lowest_sal, j.highest_sal
FROM     employees e
  JOIN   job_grades j
     ON  NVL(e.salary,0) BETWEEN j.lowest_sal AND j.highest_sal
ORDER BY e.salary;
```

## INNER Join

O inner join é o tipo de join mais utilizado. Ele retorna apenas os registros que satisfazem a condição de join. A palavra `INNER` é opcional. Todos os exemplos anteriores utilizam o `INNER JOIN`.

## OUTER Join

O outer join retorna todos os registros de uma tabela, mesmo que não exista correspondência na outra tabela. Existem 4 tipos de outer join: `LEFT OUTER JOIN`, `RIGHT OUTER JOIN`, `FULL OUTER JOIN` e `CROSS OUTER JOIN`.

### LEFT OUTER JOIN

O left outer join retorna todos os registros da tabela da esquerda, mesmo que não exista correspondência na tabela da direita.

```sql
SELECT e.first_name, e.last_name, d.department_id, d.department_name
FROM employees e LEFT OUTER JOIN departments d
     ON (e.department_id = d.department_id)
ORDER BY d.department_id;
```

Esta consulta retornará todos os funcionários, mesmo que não estejam alocados em nenhum departamento (empregados que não tem departamento).

### RIGHT OUTER JOIN

O right outer join retorna todos os registros da tabela da direita, mesmo que não exista correspondência na tabela da esquerda.

```sql
SELECT d.department_id, d.department_name, e.first_name, e.last_name
FROM employees e RIGHT OUTER JOIN departments d
     ON (e.department_id = d.department_id)
ORDER BY d.department_id;
```

Esta consulta retornará todos os departamentos, mesmo que não tenham nenhum funcionário alocado.

### FULL OUTER JOIN

O full outer join retorna todos os registros de ambas as tabelas, mesmo que não exista correspondência entre elas.

```sql
SELECT d.department_id, d.department_name, e.first_name, e.last_name
FROM   employees e FULL OUTER JOIN departments d
     ON (e.department_id = d.department_id)
ORDER BY d.department_id;
```

Esta consulta retornará todos os departamentos e todos os funcionários, mesmo que não tenham correspondência entre eles.

### CROSS OUTER JOIN

O cross outer join retorna todos os registros de ambas as tabelas, mesmo que não exista correspondência entre elas. A diferença entre o cross outer join e o full outer join é que o cross outer join não retorna registros nulos.

## Produto Cartesiano - CROSS JOIN

O produto cartesiano é o resultado de um join entre duas tabelas sem condição de join. O resultado é o produto entre o número de registros de cada tabela.
A cláusula `CROSS JOIN` produz um produto Cartesiano entre duas tabelas (muitos para muitos)

```sql
SELECT last_name, department_name
FROM   employees
  CROSS JOIN departments;
```

Esta consulta irá retornar a combinação de todos os funcionários com todos os departamentos.

## Joins utilizando sintaxe Oracle

### Equijoins

A diferença é que a condição de ligação não fica na cláusula `FROM` na condição `ON` mas sim na cláusula `WHERE`.

```sql
SELECT e.employee_id, e.last_name, e.department_id, d.department_id, d.location_id
FROM   employees e,
       departments d
WHERE  (e.department_id = d.department_id)
ORDER BY e.department_id;
```

### Joins entre várias tabelas utilizando Sintaxe Oracle

```sql
SELECT e.employee_id, j.job_title, d.department_name, l.city, l.state_province, l.country_id
FROM   employees e,
       jobs j,
       departments d,
       locations l
WHERE (e.job_id = j.job_id)               AND
      (d.department_id = e.department_id) AND
      (d.location_id = l.location_id)
ORDER BY e.employee_id;
```

Pode-se incluior condições adicionais à condição de Join utilizando `AND`.

### Nonequijoins utilizando sintaxe Oracle

```sql
SELECT e.employee_id, e.salary, j.grade_level, j.lowest_sal, j.highest_sal
FROM   employees e,
       job_grades j
WHERE  NVL(e.salary,0) BETWEEN j.lowest_sal AND j.highest_sal
ORDER BY e.salary;
```

### Outer Join utilizando sintaxe Oracle

Na sintxe Oracle, o outer join é feito utilizando o símbolo `(+)` após a tabela da esquerda ou da direita.

```sql
SELECT e.first_name, e.last_name, d.department_id, d.department_name
FROM   employees e,
       departments d
WHERE  e.department_id = d.department_id(+)
ORDER BY e.department_id;
```

```sql
SELECT e.first_name, e.last_name, d.department_id, d.department_name
FROM   employees e,
       departments d
WHERE  e.department_id(+) = d.department_id
ORDER BY e.first_name;
```

### Produto Cartesiano

Na sintaxe Oracle, basta esquecer a cláusula `WHERE` para se produzir um produto cartesiano.

```sql
SELECT e.employee_id, e.first_name, e.last_name, j.job_id, j.job_title
FROM   employees e, jobs j;
```

Porém, o produto cartesiano é um erro grave. Normalmente nós não queremos gerar um produto cartesiano. Para corrigir o mesmo, basta incluir uma condição de join.

```sql
SELECT e.employee_id, e.first_name, e.last_name, j.job_id, j.job_title
FROM   employees e, jobs j
WHERE  e.job_id = j.job_id;
```
