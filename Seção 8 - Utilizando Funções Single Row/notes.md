# Fundamentos SQL: Utilizando Funções Single Row

## Funções SQL Single Row

São funções que retornam **um** valor para cada linha exibida pelo seu `SELECT`

```SQL
nome_função[(arg1, arg2,...)]
```

## Tabela DUAL

É uma tabela que sempre vai existir no banco de dados Oracle e ela só possui uma linha. Logo, quando usamos ela o resultado também será apenas uma linha.

```SQL
FROM DUAL
```

## Funções que manipulam caracteres

-   `LOWER(arg)` retorna o argumento em letras minúsculas
-   `UPPER(arg)` retorna o argumento em letras maiúsculas
-   `INITCAP(arg)` retorna a primeira letra maiúscula para cada palavra do argumento
-   `CONCAT(arg1,arg2,...)` concatena 2 ou mais argumentos
-   `SUBSTR(arg,posicao_inicial,tamanho)` retorna a sub string baseada nos parametros `posicao_incial` e `tamanho` passados
-   `LENGTH(arg)` retorna o tamanho do argumento
-   `INSTR(arg,arg_procurado)` retorna a posição inicial do `argumento_procurado`
-   `LPAD(arg,tamanho_total,caractere_preechimento)` alinha o argumento a direita de acordo com o `tamanho_total` informado e completa a esquerda com o `caractere_preenchimento`. Lembrar do **L** de left.
-   `RPAD(arg,tamanho_total,caractere_preechimento)` alinha o argumento a esquerda de acordo com o `tamanho_total` informado e completa a direita com o `caractere_preenchimento`. Lembrar do **R** de right.
-   `REPLACE(arg,procurado,substituicao)` procura no argumento o `elemento_procurado` e troca pelo `substituicao`
-   `TRIM(caractere FROM arg)` remove o `caractere` da direita e esquerda do argumento
-   `RTRIM(arg,caractere)` remove `caractere` da direita argumento
-   `LTRIM(arg,caractere)` remove `caractere` da esquerda argumento

## Funções tipo NUMBER

-   `ROUND(arg,digitos_precisao)` retorna o argumento com a quantidades de `digitos_precisao` após a vírgula
-   `TRUNC(arg, digitos_precisao)` trunca o argumento sem fazer o arredondamento com a quantidade de `digitos_precisao` informada
-   `MOD(dividendo,divisor)` retorna o resto da divisão do dividendo pelo divisor
-   `ABS(arg)` retorna o valor absoluto do argumento
-   `SQRT(arg)` retorna a raíz quadrada do argumento

## Funções tipo DATE

Definido pelo DBA através do parâmetro `NLS_DATE_FORMAT`. No Brasil normalmente o formato default de exibição de das é definido para `DD/MM/YY` ou `DD/MM/RR`

### Cálculos com datas

|     Operação     |   Resultado    | Descrição                                 |
| :--------------: | :------------: | ----------------------------------------- |
|  Data + Número   |      Data      | Adiciona um número de dias a data         |
|  Data - Número   |      Data      | Subtrai um número de dias a data          |
|   Data - Data    | Número de dias | Subtrai uma data a partir de outra        |
| Data + Número/24 |      Data      | Adiciona um número de horas para uma data |

### Outras função tipo DATE

-   `SYSDATE` retorna a data atual
-   `MONTHS_BETWEEN(arg1, arg2)` Número de meses entre duas datas
-   `ADD_MONTHS(arg1,arg2)` retorna o resto da divisão do dividendo pelo divisor
-   `NEXT_DAY(arg1, arg2)` próximo dia a uma data especificada
-   `LAST_DAY(arg)` Útimo dia do mês
-   `ROUND(arg1, arg2)` Arredonda a data
-   `TRUNC(arg1, arg2)` Trunca a data
