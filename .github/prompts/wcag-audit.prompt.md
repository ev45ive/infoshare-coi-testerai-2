---
description: "Interaktywny audyt dostępności WCAG 2.1 AA — prowadzi testera krok po kroku przez kryteria, zbiera wyniki i generuje końcowy raport."
name: "WCAG Audit"
argument-hint: "Nazwa komponentu lub strony do audytu (opcjonalnie)"
agent: "agent"
---

Przeprowadź interaktywny audyt dostępności WCAG 2.1 AA według zasad z [wcag-accessibility.instructions.md](../.github/instructions/wcag-accessibility.instructions.md).

## Krok 1 — Zbierz kontekst

Zanim zaczniesz, zapytaj testera o:

1. **Co audytujemy?** — nazwa strony, komponentu lub user story (jeśli nie podano jako argument)
2. **Kontekst** — czy jest dostępny któryś z poniższych zasobów? (poproś o załączenie lub link):
   - Wymagania / user story
   - Use case (np. plik `.usecase.md` z folderu `uczestnik/`)
   - Makieta (wireframe / screenshot)
3. **Środowisko** — URL działającej aplikacji lub komponent do inspekcji kodu?

Poczekaj na odpowiedź testera zanim przejdziesz do kroku 2.

---

## Krok 2 — Audyt sekcja po sekcji

Przejdź przez każdą z czterech zasad WCAG **po kolei**. Dla każdej zasady:
- Wypisz kryteria do sprawdzenia (z priorytetem Critical/High/Medium)
- Zapytaj testera o wynik każdego kryterium: ✅ Spełnione / ❌ Naruszenie / ⚠️ Do weryfikacji / N/A
- Przy naruszeniu (❌) zapytaj o opis i kroki reprodukcji
- Poczekaj na odpowiedź przed przejściem do kolejnej zasady

### Zasada 1: Percepcja (Perceivable)

Sprawdź kryteria w kolejności priorytetu:

| # | Kryterium | Priorytet |
|---|-----------|-----------|
| 1 | 1.1.1 — Alt dla obrazów | Critical |
| 2 | 1.4.3 — Kontrast tekstu ≥ 4.5:1 | Critical |
| 3 | 1.3.1 — Semantyczny HTML (nagłówki, listy, tabele) | High |
| 4 | 1.3.3 — Brak polegania wyłącznie na kolorze/kształcie | Medium |
| 5 | 1.4.1 — Kolor nie jedynym nośnikiem informacji | Medium |
| 6 | 1.4.4 — Zmiana rozmiaru 200% bez utraty treści | Medium |
| 7 | 1.4.11 — Kontrast UI ≥ 3:1 | Medium |

### Zasada 2: Funkcjonalność (Operable)

| # | Kryterium | Priorytet |
|---|-----------|-----------|
| 1 | 2.1.1 — Pełna obsługa klawiatury | Critical |
| 2 | 2.1.2 — Brak pułapki klawiaturowej | Critical |
| 3 | 2.4.3 — Logiczna kolejność fokusu | High |
| 4 | 2.4.7 — Widoczny wskaźnik fokusu | High |
| 5 | 2.4.1 — Skip links | Medium |
| 6 | 2.4.2 — Opisowy `<title>` | Medium |

### Zasada 3: Zrozumiałość (Understandable)

| # | Kryterium | Priorytet |
|---|-----------|-----------|
| 1 | 3.3.1 — Identyfikacja błędów w formularzach | High |
| 2 | 3.3.2 — Etykiety i instrukcje (`<label>`) | High |
| 3 | 3.1.1 — Język strony (`lang`) | Medium |

### Zasada 4: Niezawodność (Robust)

| # | Kryterium | Priorytet |
|---|-----------|-----------|
| 1 | 4.1.2 — Nazwa, rola, wartość (ARIA) | Critical |
| 2 | 4.1.1 — Poprawny HTML (brak duplikatów ID) | High |
| 3 | 4.1.3 — Komunikaty statusu (`aria-live`) | Medium |

---

## Krok 3 — Wygeneruj raport końcowy

Po zebraniu wyników ze wszystkich czterech zasad wygeneruj raport w poniższym formacie:

```markdown
# Raport Audytu Dostępności WCAG 2.1 AA

**Komponent / Strona:** [nazwa]
**Data audytu:** [data]
**Audytor:** [imię lub rola]
**Środowisko:** [URL / branch / wersja]

---

## Podsumowanie

| Status | Liczba |
|--------|--------|
| ✅ Spełnione | X |
| ❌ Naruszenia | X |
| ⚠️ Do weryfikacji | X |
| N/A | X |
| **Łącznie** | X |

**Ogólna ocena:** [PASS / FAIL / WYMAGA WERYFIKACJI]

---

## Naruszenia krytyczne i wysokie

### ❌ [Kryterium WCAG] — [Krótki tytuł]

- **Priorytet:** Critical / High
- **Komponent:** [element UI]
- **Opis:** [co jest nie tak]
- **Kroki reprodukcji:**
  1. ...
- **Rekomendacja:** [sugerowana poprawka]

---

## Wyniki szczegółowe

### Zasada 1: Percepcja
| Kryterium | Status | Uwagi |
|-----------|--------|-------|
| 1.1.1 | ✅ / ❌ / ⚠️ | |
...

### Zasada 2: Funkcjonalność
...

### Zasada 3: Zrozumiałość
...

### Zasada 4: Niezawodność
...

---

## Następne kroki

1. [Lista defektów do zgłoszenia z priorytetem]
```
