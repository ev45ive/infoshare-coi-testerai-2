---
description: "Interactively generate test cases from a PRD section, step by step with user approval at each stage. Use when you want to create TC documents from requirements."
name: "Create Test Case from Requirements"
argument-hint: "Optional quick input — you will be asked for details interactively"
agent: "agent"
---

Follow the rules defined in [test-case-standard.instructions.md](../.github/instructions/test-case-standard.instructions.md).

All responses and generated content must be in **Polish**.

---

## STEP 1 — Gather inputs

Ask the user all three questions in a **single message**:

1. **Dokument PRD** — podaj ścieżkę do pliku PRD (domyślnie: `docs/product/product-requirements.md`).
2. **Nazwa sekcji** — podaj nazwę sekcji lub modułu do pokrycia (np. `5.1 Autoryzacja`, `5.3 Koszyk`).
3. **Numery wymagań** *(opcjonalnie)* — konkretne ID wymagań (np. `REG-01, REG-02`). Jeśli puste — pokryj całą sekcję.

**Czekaj na odpowiedź użytkownika przed przejściem dalej.**

---

## STEP 2 — Read the PRD

Read the PRD file the user specified (or `docs/product/product-requirements.md` if not given).
Focus only on the indicated section and, if IDs were provided, only on those requirements.

---

## STEP 3 — Plan scenarios with todo list

Based on the requirements in scope, use the `manage_todo_list` tool to create a todo list.

Each todo item represents **one test case scenario** in the format:
`TC-[MODUŁ]-[NR] — [krótki tytuł]`

Plan at minimum:
- jeden scenariusz happy-path (Functional) per wymaganie
- jeden scenariusz negatywny (Negative) per wymaganie
- jeden scenariusz brzegowy (Edge Case) tam gdzie ma sens

Mark the **first scenario** as `in-progress`. Show the full list to the user so they can see the plan before proceeding.

---

## STEP 4 — Iterative scenario authoring loop

Repeat the following loop for each scenario on the todo list:

### 4a — Propose fields

For the current scenario (marked `in-progress`), propose the full TC content in three clearly separated blocks:

**Blok 1 — Metadane:**
```
ID, Priorytet, Typ, Powiązanie
```

**Blok 2 — Warunki wstępne i dane testowe:**
```
Warunki wstępne (lista)
Dane testowe (tabela, jeśli potrzebne)
```

**Blok 3 — Kroki testowe:**
```
Tabela kroków z oczekiwanymi rezultatami
```

After presenting the three blocks, ask:
> „Czy zatwierdzasz ten scenariusz? Możesz zatwierdzić (`tak`) lub podać poprawki do konkretnego bloku."

### 4b — Wait and revise

- If the user approves (`tak` or equivalent) → proceed to 4c.
- If the user suggests changes → apply them to the indicated block(s) and re-present **only the changed blocks**. Repeat 4b.

### 4c — Mark done and advance

- Mark the current todo item as `completed`.
- Mark the next scenario as `in-progress`.
- Return to 4a for the next scenario.

Continue until all scenarios are `completed`.

---

## STEP 5 — Save to file

When all scenarios are approved, ask the user in a single message:

> „Wszystkie scenariusze zatwierdzone. Gdzie zapisać plik?
> Domyślna ścieżka: `uczestnik/[NR]-[nazwa-sekcji].usecase.md`
> Wpisz ścieżkę lub naciśnij Enter, żeby użyć domyślnej."

Use the user-provided path (or the default). Write the file with:
1. A header block (tytuł, data, sekcja PRD, lista pokrytych wymagań).
2. All approved test cases using the template below.
3. A coverage matrix at the end.

### TC template

```markdown
### TC-[MODUŁ]-[NR] — [Tytuł, max 80 znaków]

| Pole           | Wartość                                                        |
|----------------|----------------------------------------------------------------|
| **ID**         | TC-[MODUŁ]-[NR]                                               |
| **Priorytet**  | [Critical / High / Medium / Low]                              |
| **Typ**        | [Functional / Negative / Edge Case / Smoke / Regression]      |
| **Powiązanie** | [US-XX / REQ-XX]                                              |

**Warunki wstępne:**
- [Stan środowiska / rola użytkownika]

**Dane testowe:** *(pomiń jeśli TC nie wymaga konkretnych wartości)*
| Zmienna     | Wartość             |
|-------------|---------------------|
| [zmienna_1] | [konkretna wartość] |

**Kroki testowe:**

| # | Akcja                     | Oczekiwany rezultat   |
|---|---------------------------|-----------------------|
| 1 | [Konkretna akcja]         | [Mierzalny wynik]     |

**Notatki:** *(opcjonalne)*
```

### Coverage matrix template

```markdown
## Macierz pokrycia

| TC ID | Powiązanie | Priorytet | Typ |
|-------|------------|-----------|-----|
| ...   | ...        | ...       | ... |
```

---

## STEP 6 — Continue or finish

After saving, ask:
> „Czy chcesz wygenerować kolejny zestaw przypadków testowych dla innej sekcji lub wymagań? (tak / nie)"

- If `tak` → return to **STEP 1**.
- If `nie` → end the session with a short summary of all generated files.
