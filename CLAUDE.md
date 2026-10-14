# How to add notes to this repo

This repo holds Arun's handwritten interview-prep notes, typed up as Markdown. When Arun shares photos of new notes, follow these rules.

## Keep the notes as written

- Do not rewrite, reword, summarize or add explanations. Keep Arun's wording, examples, diagrams and order.
- Fix obvious spelling slips only (e.g. "Anonymaus" → "Anonymous").
- Expand shorthand in code: `S.O.P(...)` → `System.out.println(...)`, `PSVM` → `public static void main(String[] args)`.
- Fix small code slips so snippets compile (missing `()`, `;`, `};`), and tell Arun which ones you fixed.
- Leave out lines that are clearly unfinished or struck through, and mention them.
- If a word can't be read, ask instead of guessing.

## Format

- Hand-drawn diagrams and flow arrows → plain-text diagrams in a code block (use `↓`, `→`, `⇕`).
- Two-column comparisons and lists of operations → Markdown tables.
- Code → ```java blocks; keep Arun's `// o/p` comments.

## Structure

- One folder per subject, lower-case with hyphens: `java-modern-features/`, `java-basics/`, `react/`, `spring/`.
- One file per topic inside the folder, numbered in the order of the notes: `01-topic-name.md`, `02-...`.
- Each folder has a `README.md` listing its topics as links.
- New notes on an existing subject → add the next numbered file and update that folder's README.
- A new subject → new folder, and add or update its row in the top-level `README.md` table (replace "Coming soon" with the link).

## Finishing

- Commit with a short message such as "Add React notes: hooks".
- Push to `main` on https://github.com/arunkumar9791/SoftwareEngineer-Interview-Preparation-Kit.
- Check `git status` is clean and the remote has the commit.
