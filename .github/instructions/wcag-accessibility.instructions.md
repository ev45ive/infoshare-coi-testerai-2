---
description: "Use when writing, reviewing, or generating accessibility test cases, WCAG audits, a11y checklists, or testing UI components for accessibility compliance. Covers WCAG 2.1 AA criteria, priority rules, testing methods, and test case patterns."
---

# WCAG Accessibility Standards

## Wymagany poziom zgodności

Domyślny cel projektu: **WCAG 2.1 poziom AA**.
Poziom A jest absolutnym minimum — nie jest wystarczający dla zgodności prawnej (Dyrektywa UE 2019/882).

## Priorytety kryteriów do testowania

### Priorytet Critical (blokujące dostępność)

| Kryterium | Opis | Metoda weryfikacji |
|-----------|------|--------------------|
| 1.1.1 (A) | Alternatywy tekstowe dla obrazów | Inspekcja atrybutu `alt`; czytnik ekranu |
| 2.1.1 (A) | Pełna obsługa klawiatury | Test manualny Tab/Enter/Escape |
| 2.1.2 (A) | Brak pułapki klawiaturowej | Test manualny — czy można opuścić każdy komponent |
| 4.1.2 (A) | Nazwa, rola, wartość | Inspekcja ARIA (`role`, `aria-label`, `aria-expanded`) |
| 1.4.3 (AA) | Kontrast tekstu ≥ 4.5:1 | Narzędzie: Colour Contrast Analyser / axe DevTools |

### Priorytet High (poważnie utrudniają dostęp)

| Kryterium | Opis | Metoda weryfikacji |
|-----------|------|--------------------|
| 1.3.1 (A) | Informacje i relacje (semantyczny HTML) | Inspekcja kodu; czytnik ekranu |
| 2.4.3 (A) | Logiczna kolejność fokusu | Test manualny Tab po stronie |
| 2.4.7 (AA) | Widoczny wskaźnik fokusu | Test manualny — widoczny outline przy nawigacji klawiaturą |
| 3.3.1 (A) | Identyfikacja błędów w formularzach | Test walidacji + czytnik ekranu |
| 3.3.2 (A) | Etykiety i instrukcje (`<label>`) | Inspekcja powiązania label↔input |
| 4.1.1 (A) | Poprawny kod HTML (brak duplikatów ID) | Walidator W3C / axe |

### Priorytet Medium

| Kryterium | Opis | Metoda weryfikacji |
|-----------|------|--------------------|
| 1.3.3 (A) | Brak polegania wyłącznie na kolorze/kształcie | Inspekcja wizualna |
| 1.4.1 (A) | Kolor nie jedynym nośnikiem informacji | Symulacja deuteranopii (np. Chromium DevTools) |
| 1.4.4 (AA) | Zmiana rozmiaru tekstu 200% bez utraty treści | Test w przeglądarce: Ctrl++ do 200% |
| 1.4.11 (AA) | Kontrast komponentów UI ≥ 3:1 | axe DevTools / ręczna inspekcja |
| 2.4.1 (A) | Skip links (pomijanie bloków) | Test Tab na początku strony |
| 2.4.2 (A) | Opisowy tytuł strony `<title>` | Inspekcja `<head>` |
| 3.1.1 (A) | Język strony (`lang` na `<html>`) | Inspekcja kodu |
| 4.1.3 (AA) | Komunikaty statusu dostępne (`aria-live`) | Czytnik ekranu |

## Format TC dla dostępności

Każdy accessibility TC musi zawierać:

- **ID** — format `TC-A11Y-[NR]` (np. `TC-A11Y-001`)
- **Kryterium WCAG** — numer + poziom (np. `2.1.1 (A)`)
- **Komponent** — konkretny element UI (np. `Modal #kontakt`, `Formularz logowania`)
- **Metoda** — `Manualny` | `Automatyczny (axe)` | `Czytnik ekranu (NVDA/VoiceOver)`
- Standardowe pola z `test-case-standard.instructions.md`

## Narzędzia

- **axe DevTools** (Chrome/Firefox) — automatyczne wykrywanie naruszeń WCAG
- **NVDA** (Windows) / **VoiceOver** (macOS/iOS) — testy z czytnikiem ekranu
- **Colour Contrast Analyser** — weryfikacja kontrastów
- **W3C Validator** — poprawność HTML
- **Lighthouse** (zakładka Accessibility) — szybki audyt automatyczny

## Wzorzec TC: Obsługa klawiatury

```markdown
**ID:** TC-A11Y-001
**Tytuł:** Modal dostępny przez klawiaturę
**Kryterium WCAG:** 2.1.1 (A), 2.1.2 (A)
**Komponent:** Modal „Potwierdź usunięcie"
**Metoda:** Manualny
**Priorytet:** Critical

**Warunki wstępne:** Użytkownik na stronie, modal zamknięty, klawiatura jako jedyne urządzenie.

**Kroki:**
1. Tab do przycisku „Usuń" i naciśnij Enter — modal się otwiera.
2. Sprawdź czy fokus przeniósł się wewnątrz modala.
3. Tab przez wszystkie elementy modala.
4. Naciśnij Escape.

**Oczekiwany rezultat:**
1. Modal otwiera się po Enter.
2. Fokus jest wewnątrz modala (nie można wyjść Tab poza modal).
3. Każdy element interaktywny jest osiągalny Tab-em.
4. Modal zamyka się, fokus wraca do przycisku „Usuń".
```

## Anti-patterns

- Nie testuj dostępności tylko narzędziami automatycznymi — pokrywają ~30% kryteriów.
- Nie pomijaj testów czytnika ekranu dla formularzy i modali.
- Nie zakładaj że `aria-label` zastępuje semantyczny HTML — traktuj ARIA jako uzupełnienie.
- Nie generuj TC bez wskazania konkretnego komponentu UI.
