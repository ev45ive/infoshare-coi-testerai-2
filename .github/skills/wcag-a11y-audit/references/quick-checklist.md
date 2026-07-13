# Quick Checklist — WCAG 2.1 AA

Użyj gdy potrzebujesz szybkiej weryfikacji bez pełnego audytu.  
Dla pełnego workflow załaduj SKILL.md.

## Critical (sprawdź zawsze)

- [ ] Każdy obraz ma `alt` (pusty `alt=""` dla dekoracyjnych)
- [ ] Kontrast tekstu ≥ 4.5:1 (narzędzie: axe / Colour Contrast Analyser)
- [ ] Wszystkie funkcje dostępne klawiaturą (Tab / Enter / Escape)
- [ ] Brak pułapki klawiaturowej (można opuścić każdy komponent)
- [ ] Atrybuty ARIA poprawne: `role`, `aria-label`, `aria-expanded`

## High (sprawdź przed każdym release)

- [ ] Struktura semantyczna: nagłówki `h1–h6` w logicznej kolejności
- [ ] Fokus widoczny przy nawigacji klawiaturą (nie usunięto `outline`)
- [ ] Kolejność fokusu Tab odpowiada wizualnemu układowi
- [ ] Formularze: każde `<input>` ma powiązane `<label>`
- [ ] Błędy w formularzach opisane tekstowo (nie tylko kolorem)
- [ ] Brak zduplikowanych `id` w HTML

## Medium (sprawdź przed audytem zewnętrznym)

- [ ] Kolor nie jest jedynym nośnikiem informacji (ikona / tekst jako uzupełnienie)
- [ ] Powiększenie 200% nie powoduje utraty treści ani poziomego scrollowania
- [ ] Skip link „Przejdź do treści" na początku strony
- [ ] `<html lang="pl">` ustawiony
- [ ] `<title>` strony opisowy i unikalny
- [ ] Komunikaty dynamiczne ogłaszane przez `aria-live`

## Narzędzia

| Narzędzie | Co wykrywa | Gdzie |
|-----------|------------|-------|
| axe DevTools | ~30% naruszeń automatycznie | Chrome/Firefox extension |
| Lighthouse | Szybki przegląd + score | DevTools → Lighthouse tab |
| Colour Contrast Analyser | Kontrast tekstu i UI | Standalone app |
| NVDA / VoiceOver | Testy z czytnikiem ekranu | Windows / macOS |
| W3C Validator | Błędy HTML | validator.w3.org |
