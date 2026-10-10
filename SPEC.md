# JSON to SQL for Oracle Database Specification

Este arquivo é uma especificação de como gerar o app para converter JSON geraldo pelo DBeaver em SQL.

Nome do app: JSON to SQL for Oracle DB

## Requisitos

- Todo app precisa ser implementado no frontend usando somente HTML, CSS e JavaScript;
- O app terá somente uma página `index.html` com CSS e javascrpt embutido nela;
- Sempre use fontes como Roboto, Helvetica, etc. Não use fontes que divirja do design Material UI;
- Sempre use icones de libs gratúitas como Material Symbols, Font Awesome ou Bootstrap Icons;
- Por padrão o app terá o idioma Português do Brasil;

## O que não fazer ou usar

- Não use frameworks como Angular, React ou Vue;

## Interface

- DIV content usando flex layout na vertical:
    - Text area para inserir o JSON de entrada gerado pelo DBeaver;
    - Input text para inserir o schema e nome da tabela. Exemplo de entrada: `public.my_table`;
    - Input text para inserir nomes das colunas separas por virgula do WHERE. Esse campo só será exibido se checkbox de update estiver marcado;
    - DIV com checkboxs e botões usando flex layout na horizontal:
        - Checkbox para escolher se a saida generará INSERTO INTO ou UPDATE SET.
        - Checkbox para escolher se SQL gerado vai ter colunas e valores separados por linha;
        - Botão para colar entrada no text area;
        - Botão para colar entrada no text area, mas detectar chave valores internos do JSON;
        - Botão para converter entrada JSON em SQL;
        - Botão para copiar saida para o clipboard;
        - Botão turbo para colar com detecção, converter e copiar;
    - Text area para mostrar o SQL gerado usando a library `highlight.js`;

## Lógica de negócio

- JSON de entrada:

```
{
    "MY_INTEGER" : 1,
    "MY_INT" : 2,
    "MY_SMALLINT" : 3,
    "MY_NUMBER" : 4,
    "MY_NUMBER_INTEGER" : 5,
    "MY_NUMBER_DECIMAL" : 6,
    "MY_NUMBER_FLOAT" : 7,
    "MY_FLOAT_PRECISION" : 8,
    "MY_BINARY_FLOAT" : 9.0,
    "MY_BINARY_DOUBLE" : 10.0,
    "MY_REAL" : 11,
    "MY_DOUBLE_PRECISION" : 12,
    "MY_VARCHAR2" : "13",
    "MY_VARCHAR2_BYTES" : "14",
    "MY_VARCHAR2_CHARS" : "15",
    "MY_CHAR" : "16        ",
    "MY_NVARCHAR2" : "17",
    "MY_NCHAR" : "18        ",
    "MY_RAW" : "19",
    "MY_DATE" : "2026-01-20T03:00:00.000Z",
    "MY_TIMESTAMP" : "2026-01-21T13:20:30.123Z",
    "MY_TIMESTAMP_0" : "2026-01-22T13:20:30.000Z",
    "MY_TIMESTAMP_9" : "2026-01-23T13:20:30.123Z",
    "MY_TIMESTAMP_TZ" : "2026-01-24 10:20:30.123 -0300",
    "MY_TIMESTAMP_LOCAL" : "2026-01-25T16:20:30.123Z",
    "MY_INTERVAL_YEAR" : "26-6",
    "MY_INTERVAL_YEAR_4" : "27-6",
    "MY_INTERVAL_DAY" : "28 10:20:30.0",
    "MY_INTERVAL_DAY_4" : "29 10:20:30.123456",
    "MY_BLOB" : "30",
    "MY_CLOB" : "31",
    "MY_NCLOB" : "32",
    "MY_BFILE" : null
}
```

- JSON de entrada bruto:

```
{
"select * from public.my_table": [
	{
		"MY_INTEGER" : 1,
		"MY_INT" : 2,
		"MY_SMALLINT" : 3,
		"MY_NUMBER" : 4,
		"MY_NUMBER_INTEGER" : 5,
		"MY_NUMBER_DECIMAL" : 6,
		"MY_NUMBER_FLOAT" : 7,
		"MY_FLOAT_PRECISION" : 8,
		"MY_BINARY_FLOAT" : 9.0,
		"MY_BINARY_DOUBLE" : 10.0,
		"MY_REAL" : 11,
		"MY_DOUBLE_PRECISION" : 12,
		"MY_VARCHAR2" : "13",
		"MY_VARCHAR2_BYTES" : "14",
		"MY_VARCHAR2_CHARS" : "15",
		"MY_CHAR" : "16        ",
		"MY_NVARCHAR2" : "17",
		"MY_NCHAR" : "18        ",
		"MY_RAW" : "19",
		"MY_DATE" : "2026-01-20T03:00:00.000Z",
		"MY_TIMESTAMP" : "2026-01-21T13:20:30.123Z",
		"MY_TIMESTAMP_0" : "2026-01-22T13:20:30.000Z",
		"MY_TIMESTAMP_9" : "2026-01-23T13:20:30.123Z",
		"MY_TIMESTAMP_TZ" : "2026-01-24 10:20:30.123 -0300",
		"MY_TIMESTAMP_LOCAL" : "2026-01-25T16:20:30.123Z",
		"MY_INTERVAL_YEAR" : "26-6",
		"MY_INTERVAL_YEAR_4" : "27-6",
		"MY_INTERVAL_DAY" : "28 10:20:30.0",
		"MY_INTERVAL_DAY_4" : "29 10:20:30.123456",
		"MY_BLOB" : "30",
		"MY_CLOB" : "31",
		"MY_NCLOB" : "32",
		"MY_BFILE" : null
	}
]}
```

- SQL INSERT INTO de saída:

```
INSERT INTO PUBLIC.MY_TABLE (
    MY_INTEGER,MY_INT,MY_SMALLINT,MY_NUMBER,MY_NUMBER_INTEGER,MY_NUMBER_DECIMAL,MY_NUMBER_FLOAT,MY_FLOAT_PRECISION,MY_BINARY_FLOAT,MY_BINARY_DOUBLE,MY_REAL,MY_DOUBLE_PRECISION,MY_VARCHAR2,MY_VARCHAR2_BYTES,MY_VARCHAR2_CHARS,MY_CHAR,MY_NVARCHAR2,MY_NCHAR,MY_RAW,MY_DATE,MY_TIMESTAMP,MY_TIMESTAMP_0,MY_TIMESTAMP_9,MY_TIMESTAMP_TZ,MY_TIMESTAMP_LOCAL,MY_INTERVAL_YEAR,MY_INTERVAL_YEAR_4,MY_INTERVAL_DAY,MY_INTERVAL_DAY_4,MY_BLOB,MY_CLOB,MY_NCLOB,MY_BFILE
) VALUES (
    1,2,3,4,5,6,7,8,9.0,10.0,11,12,'13','14','15','16        ','17','18        ','3139',TIMESTAMP '2026-01-20 00:00:00.000000',TIMESTAMP '2026-01-21 10:20:30.123000',TIMESTAMP '2026-01-22 10:20:30.000000',TIMESTAMP '2026-01-23 10:20:30.123000',TIMESTAMP '2026-01-24 10:20:30.123000000 -03:00',TIMESTAMP '2026-01-25 13:20:30.123456','26-6','27-6','28 10:20:30.0','29 10:20:30.123456','30','31','32',NULL
);
```

- Exemplo de SQL INSERT INTO de saída quando checkout separado por linha marcado:

```
INSERT INTO PUBLIC.MY_TABLE (
    MY_INTEGER,
    MY_INT,
    ...
) VALUES (
    1,
    2,
    ...
);
```

- Exemplo de SQL UPDATE SET (campo Where foi passado o valor `MY_INTEGER`):

```
UPDATE public.my_table SET
    MY_INT = 2, MY_SMALLINT = 3, MY_NUMBER = 4, MY_NUMBER_INTEGER = 5, MY_NUMBER_DECIMAL = 6, MY_NUMBER_FLOAT = 7, MY_FLOAT_PRECISION = 8, MY_BINARY_FLOAT = 9, MY_BINARY_DOUBLE = 10, MY_REAL = 11, MY_DOUBLE_PRECISION = 12, MY_VARCHAR2 = '13', MY_VARCHAR2_BYTES = '14', MY_VARCHAR2_CHARS = '15', MY_CHAR = '16        ', MY_NVARCHAR2 = '17', MY_NCHAR = '18        ', MY_RAW = '19', MY_DATE = TIMESTAMP '2026-01-20 00:00:00.000000', MY_TIMESTAMP = TIMESTAMP '2026-01-21 10:20:30.123000', MY_TIMESTAMP_0 = TIMESTAMP '2026-01-22 10:20:30.000000', MY_TIMESTAMP_9 = TIMESTAMP '2026-01-23 10:20:30.123000', MY_TIMESTAMP_TZ = TIMESTAMP '2026-01-24 10:20:30.123000000 -03:00', MY_TIMESTAMP_LOCAL = TIMESTAMP '2026-01-25 13:20:30.123456', MY_INTERVAL_YEAR = '26-6', MY_INTERVAL_YEAR_4 = '27-6', MY_INTERVAL_DAY = '28 10:20:30.0', MY_INTERVAL_DAY_4 = '29 10:20:30.123456', MY_BLOB = '30', MY_CLOB = '31', MY_NCLOB = '32', MY_BFILE = NULL
WHERE
    MY_INTEGER = 1;
```
