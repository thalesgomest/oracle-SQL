# Fundamentos SQL: Consultando dados utilizando SQL SELECT

## SQL SELECT

Capacidades do comando SQL SELECT:

-   Projeção: Seleção de colunas
-   Seleção: Seleção de linhas
-   União de dados de tabelas distintas: **JOIN**

## DESCRIBE

Utilize o comando `DESCRIBE` para exibir a estrutura da tabela

```SQL
DESC[RIBE] tabela
```

## Alias

Na linguagem SQL podemos utilizar um alias (pseudônimo ou nome de correlação) para dar um nome a uma tabela ou a uma coluna um nome temporário.

```SQL
SELECT first_name AS nome, last_name AS sobrenome, salary AS salário
FROM employees;
```

A palavra **AS** é opcional

```SQL
SELECT first_name nome, last_name  sobrenome, salary salário
FROM employees;
```

Quando meu cabeçalho (alias) possuir mais de uma palavra ou letras maiúsculas e minúsculas, eu preciso escrever meu alias com aspas duplas

```SQL
SELECT first_name "Nome", last_name "Sobrenome", salary "Salário ($)", commission_pct "Percentual de comissão"
FROM   employees;
```

## Operador de concatenação ( || )

É representado por dois pipes. Sua função é ligar colunas ou strings de caracteres com outras colunas ou strings de caracteres

```SQL
SELECT first_name || ' ' || last_name || ', data de admissão: ' || hire_date "Funcionário"
FROM   employees;
```

## Operador alternativo de aspas ( q' )

Representado pela letra q (_quotes_), seguido de aspas + operador alternativo de concatenação.

```SQL
SELECT department_name || q'[ Department's Manager Id: ]'|| manager_id "Departamento e Gerente"
FROM departments;
```

O operador alternativo para aspas nesse caso é o `[]`, mas poderia ser qualquer caractere.

## DISTINCT

Por default as consultas exibem todas as linhas retornadas, incluindo as linhas duplicadas. A cláusula `DISTINCT` é utilizada para retornar apenas uma ocorrência de campos que contenham dados repetidos. Esse recurso é útil para eliminar a duplicidade de informações e, assim, recuperar apenas o que será utilizado pela aplicação.

```SQL
SELECT DISTINCT department_id
FROM employees;
```
