# JSON to SQL for Oracle DB

[English](README.md) | [Português](README.pt-BR.md)

![Licença GPLv3](https://img.shields.io/badge/Licen%C3%A7a-GPLv3-blue.svg)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
![Oracle](https://img.shields.io/badge/Oracle-SQL-F80000?logo=oracle&logoColor=white)
![Sem build](https://img.shields.io/badge/build-n%C3%A3o%20necess%C3%A1rio-brightgreen)
![Dependências](https://img.shields.io/badge/depend%C3%AAncias-CDN%20apenas-informational)

Aplicação web simples (frontend puro) para converter JSON exportado pelo **DBeaver** em comandos SQL compatíveis com **Oracle Database**.

Todo o app roda em uma única página (`index.html`) com HTML, CSS e JavaScript embutidos — sem frameworks e sem etapa de build.

## Funcionalidades

- **Conversão para `INSERT INTO`** a partir de um objeto JSON (ou lista de objetos).
- **Conversão para `UPDATE SET`** com cláusula `WHERE` baseada nas colunas informadas.
- **INSERT enumerado**: gera um `INSERT INTO` usando as colunas do JSON como modelo, mas com valores novos enumerados sequencialmente (a partir de 1), preservando o formato/tipo de cada campo.
- **Detecção de JSON bruto do DBeaver**: aceita o formato `{ "select ... from tabela": [ { ... } ] }`, extraindo os registros e preenchendo automaticamente o nome da tabela quando possível.
- **Formatação multiline**: opção para separar colunas e valores por linha.
- **Preservação de precisão numérica**: números como `9.0` são mantidos como no JSON original.
- **Conversão de datas/horários**:
  - ISO 8601 UTC (`...Z`) é convertido para `TIMESTAMP` ajustado para UTC-3 (horário de Brasília).
  - Timestamps com fuso explícito (`YYYY-MM-DD HH:MM:SS[.fff] ±HH:MM`) são emitidos como literais `TIMESTAMP` preservando o offset.
- **Suporte a múltiplos registros**: quando o JSON contém uma lista, um comando SQL é gerado por objeto.
- **Destaque de sintaxe** do SQL gerado com [highlight.js](https://highlightjs.org/).
- **Ações rápidas**: colar, colar com detecção, copiar saída e modo **Turbo** (colar com detecção + converter + copiar).

## Como usar

1. Abra o arquivo `index.html` em um navegador moderno.
2. Cole o JSON de entrada no campo **"JSON de entrada (gerado pelo DBeaver)"**.
   - Use **Colar** para colar diretamente, ou **Colar com detecção** para interpretar o formato bruto do DBeaver e preencher a tabela.
3. Informe o **schema e nome da tabela** (ex.: `public.my_table`).
4. Escolha o tipo de saída:
   - **Desmarcado**: gera `INSERT INTO`.
   - **Gerar UPDATE SET**: gera `UPDATE ... SET ... WHERE ...` (informe as colunas do `WHERE`, separadas por vírgula).
5. (Opcional) Marque **Colunas e valores separados por linha** para formatar o SQL em múltiplas linhas.
6. Clique em **Converter** para gerar o SQL, ou em **INSERT enumerado** para gerar um insert com valores sequenciais.
7. Use **Copiar saída** para copiar o SQL, ou **Turbo** para executar colar com detecção, converter e copiar em um único passo.

> **Nota:** as ações de colar/copiar dependem da permissão de acesso à área de transferência do navegador. Se o acesso for negado, cole ou copie manualmente (`Ctrl+C` / `Ctrl+V`).

## Formatos de entrada suportados

**Objeto simples:**

```json
{
  "MY_INTEGER": 1,
  "MY_VARCHAR2": "texto",
  "MY_DATE": "2026-01-20T03:00:00.000Z"
}
```

**Formato bruto do DBeaver** (a chave é usada para detectar a tabela):

```json
{
  "select * from public.my_table": [
    { "MY_INTEGER": 1, "MY_VARCHAR2": "texto" }
  ]
}
```

Também são aceitas listas de objetos diretamente no nível raiz.

## Regras de conversão de valores

| Tipo no JSON | Saída SQL |
| --- | --- |
| `null` | `NULL` |
| Número (inteiro/decimal) | Valor numérico preservado (ex.: `9.0`) |
| Booleano | `1` (`true`) ou `0` (`false`) |
| String ISO UTC (`...Z`) | `TIMESTAMP 'YYYY-MM-DD HH:MM:SS.ffffff'` (ajustado para UTC-3) |
| String com offset (`... ±HH:MM`) | `TIMESTAMP 'YYYY-MM-DD HH:MM:SS.fffffffff ±HH:MM'` |
| Demais strings | Texto entre aspas simples (`'`) com escape de aspas internas |
| Objetos/arrays | Serializados como string JSON entre aspas simples |

## INSERT enumerado

O botão **INSERT enumerado** gera sempre um `INSERT INTO`, ignorando o modo de `UPDATE`, e substitui os valores por uma sequência a partir de `1` na ordem das colunas:

- Números e booleanos → valor numérico enumerado.
- Strings → número entre aspas simples (preservando espaços à direita, se houver).
- Datas/horários → literal `TIMESTAMP` enumerado (o primeiro no formato `TIMESTAMP '2026-01-01 00:00:00.000000'`, avançando conforme a ordem).
- `null` → permanece `NULL`.

O JSON é usado apenas como modelo para inferir o formato de cada valor.

## Tecnologias

- HTML, CSS e JavaScript puros (arquivo único `index.html`)
- [highlight.js](https://highlightjs.org/) via CDN para destaque de sintaxe SQL
- [Roboto](https://fonts.google.com/specimen/Roboto) e [Material Symbols](https://fonts.google.com/icons) via Google Fonts

## Estrutura do projeto

```text
.
├── index.html      # Aplicação (interface, estilos e lógica)
├── SPEC.md         # Especificação do comportamento esperado
├── README.md       # Documentation (English)
├── README.pt-BR.md # Este arquivo (Português)
└── LICENSE         # Licença GPLv3
```

## Licença

Distribuído sob a licença **GNU GPL v3.0**. Consulte o arquivo [LICENSE](LICENSE) para mais detalhes.
