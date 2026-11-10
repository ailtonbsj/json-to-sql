# JSON to SQL for Oracle DB

[English](README.md) | [Português](README.pt-BR.md)

![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
![Oracle](https://img.shields.io/badge/Oracle-SQL-F80000?logo=oracle&logoColor=white)
![No build required](https://img.shields.io/badge/build-not%20required-brightgreen)
![Dependencies](https://img.shields.io/badge/dependencies-CDN%20only-informational)

A simple web app (pure frontend) that converts JSON exported from **DBeaver** into SQL statements compatible with **Oracle Database**.

The entire app runs in a single page (`index.html`) with embedded HTML, CSS, and JavaScript — no frameworks and no build step.

## Features

- **`INSERT INTO` generation** from a JSON object (or a list of objects).
- **`UPDATE SET` generation** with a `WHERE` clause based on user-specified columns.
- **Enumerated INSERT**: generates an `INSERT INTO` using the JSON columns as a template, but with new sequential values (starting at 1) while preserving each field's format/type.
- **DBeaver raw JSON detection**: accepts the `{ "select ... from table": [ { ... } ] }` format, extracts the records, and auto-fills the table name when possible.
- **Multiline formatting**: option to place columns and values on separate lines.
- **Numeric precision preserved**: numbers such as `9.0` are kept exactly as in the original JSON.
- **Date/time conversion**:
  - ISO 8601 UTC strings (`...Z`) are converted to `TIMESTAMP` literals adjusted to UTC-3 (Brasília time).
  - Timestamps with an explicit offset (`YYYY-MM-DD HH:MM:SS[.fff] ±HH:MM`) are emitted as `TIMESTAMP` literals preserving the offset.
- **Multiple records support**: when the input JSON is a list, one SQL statement is generated per object.
- **Syntax highlighting** for the generated SQL using [highlight.js](https://highlightjs.org/).
- **Quick actions**: Paste, Paste with detection, Copy output, and **Turbo** mode (paste with detection + convert + copy).

## How to use

1. Open `index.html` in a modern web browser.
2. Paste the input JSON into the **"JSON de entrada (gerado pelo DBeaver)"** field.
   - Use **Colar** to paste directly, or **Colar com detecção** to parse DBeaver's raw format and auto-fill the table name.
3. Enter the **schema and table name** (e.g. `public.my_table`).
4. Choose the output type:
   - **Unchecked**: generates `INSERT INTO`.
   - **Gerar UPDATE SET**: generates `UPDATE ... SET ... WHERE ...` (provide the `WHERE` columns, comma-separated).
5. (Optional) Check **Colunas e valores separados por linha** to format the SQL across multiple lines.
6. Click **Converter** to generate the SQL, or **INSERT enumerado** to generate an insert with sequential values.
7. Use **Copiar saída** to copy the SQL, or **Turbo** to run paste with detection, convert, and copy in a single step.

> **Note:** Paste/copy actions depend on clipboard permissions in your browser. If access is denied, paste or copy manually (`Ctrl+C` / `Ctrl+V`).

## Supported input formats

**Simple object:**

```json
{
  "MY_INTEGER": 1,
  "MY_VARCHAR2": "text",
  "MY_DATE": "2026-01-20T03:00:00.000Z"
}
```

**DBeaver raw format** (the key is used to detect the table name):

```json
{
  "select * from public.my_table": [
    { "MY_INTEGER": 1, "MY_VARCHAR2": "text" }
  ]
}
```

Lists of objects at the root level are also accepted.

## Value conversion rules

| JSON type | SQL output |
| --- | --- |
| `null` | `NULL` |
| Number (integer/decimal) | Numeric value preserved (e.g. `9.0`) |
| Boolean | `1` (`true`) or `0` (`false`) |
| ISO UTC string (`...Z`) | `TIMESTAMP 'YYYY-MM-DD HH:MM:SS.ffffff'` (adjusted to UTC-3) |
| String with offset (`... ±HH:MM`) | `TIMESTAMP 'YYYY-MM-DD HH:MM:SS.fffffffff ±HH:MM'` |
| Other strings | Single-quoted text (`'`) with internal quotes escaped |
| Objects/arrays | Serialized as a JSON string wrapped in single quotes |

## Enumerated INSERT

The **INSERT enumerado** button always generates an `INSERT INTO`, regardless of the `UPDATE` mode, and replaces values with a sequence starting at `1` in column order:

- Numbers and booleans → enumerated numeric value.
- Strings → number wrapped in single quotes (preserving trailing spaces, if any).
- Dates/times → enumerated `TIMESTAMP` literal (the first in the format `TIMESTAMP '2026-01-01 00:00:00.000000'`, advancing with the sequence).
- `null` → remains `NULL`.

The JSON is used only as a template to infer each value's format.

## Technologies

- Plain HTML, CSS, and JavaScript (single `index.html` file)
- [highlight.js](https://highlightjs.org/) via CDN for SQL syntax highlighting
- [Roboto](https://fonts.google.com/specimen/Roboto) and [Material Symbols](https://fonts.google.com/icons) via Google Fonts

## Project structure

```text
.
├── index.html      # Application (UI, styles, and logic)
├── SPEC.md         # Expected behavior specification
├── README.md       # Documentation (English)
├── README.pt-BR.md # Documentation (Português)
└── LICENSE         # GPLv3 license
```

## License

Distributed under the **GNU GPL v3.0** license. See the [LICENSE](LICENSE) file for more details.
