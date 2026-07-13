---
description: "Generate test cases from requirements. Use when you want to create TC documents from a PRD, user story, or requirement ID."
name: "Create Test Case from Requirements"
argument-hint: "Optional quick input — you will be asked for details interactively"
agent: "agent"
---

Follow the rules defined in [test-case-standard.instructions.md](../.github/instructions/test-case-standard.instructions.md).

## Step 1 — Gather inputs

Before generating anything, ask the user for the following. Ask all questions in a single message:

1. **Dokument PRD** — podaj ścieżkę do pliku PRD 
2. **Nazwa sekcji** — podaj nazwę sekcji lub modułu z PRD, który ma być pokryty (np. `5.1 Autoryzacja`, `5.3 Koszyk`).
3. **Numery wymagań** *(opcjonalnie)* — podaj konkretne ID wymagań do pokrycia (np. `REG-01, REG-02`). Jeśli puste, wygeneruję TC dla całej sekcji.

Wait for the user's answers before proceeding.

## Step 2 — Read the PRD

Read the PRD file indicated by the user. If no path was given, read. Focus on the specified section and, if requirement IDs were provided, on those requirements only.

## Step 3 — Generate test cases

For each requirement in scope, produce:

1. **At least one happy-path (Functional) test case**
2. **At least one negative test case** (invalid input, missing data, unauthorized access)
3. **At least one edge case** where applicable

Each test case must follow this exact template:

```markdown
### TC-[MODUŁ]-[NR] — [Tytuł: co testujemy, max 80 znaków]

| Pole              | Wartość                                      |
|-------------------|----------------------------------------------|
| **ID**            | TC-[MODUŁ]-[NR]                              |
| **Priorytet**     | [Critical / High / Medium / Low]             |
| **Typ**           | [Functional / Negative / Edge Case / Smoke / Regression] |
| **Powiązanie**    | [US-XX / REQ-XX]                             |

**Warunki wstępne:**
- [Stan środowiska / konto / rola użytkownika]

**Dane testowe:** *(pomiń sekcję jeśli TC nie wymaga konkretnych wartości)*
| Zmienna       | Wartość             |
|---------------|---------------------|
| [zmienna_1]   | [konkretna wartość] |

**Kroki testowe:**

| # | Akcja                         | Oczekiwany rezultat               |
|---|-------------------------------|-----------------------------------|
| 1 | [Konkretna akcja]             | [Mierzalny wynik]                 |

**Notatki:** *(opcjonalne)*
```

## Coverage matrix

After the test cases, append a coverage matrix:

```markdown
## Macierz pokrycia

| TC ID | Powiązanie | Priorytet | Typ         |
|-------|------------|-----------|-------------|
| ...   | ...        | ...       | ...         |
```

## Rules

- Use Polish language for all test case content.
- ID module segment must match the functional area (e.g., `LOGIN`, `CART`, `CHECKOUT`, `PROFILE`).
- Number sequentially within the module starting from `001`.
- Preconditions must specify the user role (Gość / Klient zarejestrowany) based on the requirement.
- Do not invent features outside the product scope defined in the PRD.
