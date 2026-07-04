# Compiler Expression Analyzer & Optimizer

An interactive web application that visualizes the **front-end phases of a compiler** — from raw arithmetic expressions to optimized intermediate code. Built as an educational tool for **Compiler Design** courses.

Enter an expression like `a = b * c + b * c - d` and step through tokenization, parsing, AST construction, DAG generation, three-address code (TAC), and optimization — each phase rendered with animated, interactive visualizations.

## Features

| Phase | What it shows |
|-------|----------------|
| **Lexical Analysis** | Token stream with identifiers, operators, numbers, and delimiters |
| **Parse Tree** | Full grammar derivation tree with animated growth |
| **Syntax Tree (AST)** | Simplified abstract syntax tree without redundant grammar nodes |
| **DAG** | Directed acyclic graph with merged common subexpressions (CSE) |
| **Three-Address Code** | TAC, quadruples, and triples intermediate representations |
| **Optimization** | Constant folding, CSE, and dead code elimination with before/after diff |

Additional pages:

- **Analyzer** — Monaco code editor, sample expressions, tabbed phase views
- **About** — Pipeline walkthrough with examples for each compiler phase
- **History** — Past analyses (browser localStorage; optional MySQL backend)

## Tech Stack

- [Next.js 16](https://nextjs.org/) (App Router) + React 19 + TypeScript
- [Tailwind CSS 4](https://tailwindcss.com/) for styling
- [React Flow](https://reactflow.dev/) (`@xyflow/react`) for tree and DAG graphs
- [Monaco Editor](https://microsoft.github.io/monaco-editor/) for the expression input
- [Framer Motion](https://www.framer.com/motion/) for animations
- [MySQL](https://www.mysql.com/) (optional) for persistent analysis history

## Project Structure

```
.
├── LICENSE
├── README.md
├── database/
│   └── schema.sql            # MySQL schema (optional)
├── public/
├── src/
│   ├── app/                  # Pages and API routes
│   │   ├── page.tsx          # Landing page
│   │   ├── analyzer/         # Main analyzer UI
│   │   ├── about/            # Compiler pipeline reference
│   │   ├── history/          # Analysis history
│   │   └── api/
│   │       ├── analyze/      # POST — run full compiler pipeline
│   │       └── history/      # GET/POST — optional DB persistence
│   ├── components/
│   │   ├── analyzer/         # Tokens, trees, DAG, TAC, optimization views
│   │   └── shared/           # Navbar, layout, background effects
│   └── lib/
│       ├── compiler/         # Core compiler logic
│       │   ├── tokenizer.ts
│       │   ├── parser.ts
│       │   ├── dag.ts
│       │   ├── tac.ts
│       │   └── optimizer.ts
│       └── db.ts             # Optional MySQL connection
├── .env.example
└── package.json
```

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) 18 or later
- npm (comes with Node.js)

### Install and run

```bash
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

### Build for production

```bash
npm run build
npm start
```

## Supported Grammar

The parser uses recursive descent for arithmetic assignment expressions:

```
Statement  →  id = Expression
Expression →  Term ((+ | -) Term)*
Term       →  Factor ((* | /) Factor)*
Factor     →  ( Expression ) | id | number
```

**Example expressions:**

- `a = b * c + b * c`
- `x = a + b * c - d`
- `y = (a + b) * (a + b)`
- `z = 2 * 3 + 4 * 5`

## Optional: MySQL History

The analyzer works fully without a database. History is stored in the browser by default.

To enable server-side persistence:

1. Install and start MySQL.
2. Create the database and table:

   ```bash
   mysql -u root -p < database/schema.sql
   ```

3. Copy the environment template and fill in your credentials:

   ```bash
   cp .env.example .env.local
   ```

4. Restart the dev server.

## Compiler Pipeline

```
Source Code → Tokens → Parse Tree → AST → DAG → TAC → Optimized Code
```

Each step is implemented in `src/lib/compiler/` and exposed through the `/api/analyze` endpoint.

## License

This project is licensed under the [GNU General Public License v3.0](LICENSE).
