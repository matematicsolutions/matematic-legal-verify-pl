# AGENTS.md - matematic-legal-verify-pl

Plik standardu [agents.md](https://agents.md) (Linux Foundation / Agentic AI Foundation) - kanoniczne instrukcje dla agentow AI pracujacych z tym repozytorium. Czytany natywnie przez Cursor, Codex (OpenAI), Jules (Google), Devin / Windsurf, Aider, Amp, Factory, GitHub Copilot.

## Cel projektu

`matematic-legal-verify-pl` to trzy otwarte **skille Claude Code** tworzace warstwe weryfikacji outputu AI prawnego:

1. **citation-grounding-pl** - mechaniczny weryfikator cytatu (string-match cytatu wobec zrodla; brak trafienia = halucynacja = blokada).
2. **adversarial-legal-review-pl** - kontradyktoryjny stress-test pisma wysokiej stawki (builder / attacker / synthesizer / verifier), z bramka kosztu.
3. **legal-ai-audit-bundle** - paczka audytowa AI Act art. 12 (manifest + SHA256 + INDEX.md).

Inspiracja cherry-pick: [AnttiHero/lavern](https://github.com/AnttiHero/lavern) (Apache 2.0). Tresc, prompty i skrypty **napisane od zera** pod polskie realia (RODO, PoA art. 6, AI Act). Nie skopiowano kodu ani promptow Lavern.

**To nie [Patron](https://github.com/matematicsolutions/patron)** - Patron pracuje nad sprawa; te skille sprawdzaja dowolny output prawny. **To nie [contract-review-pl](https://github.com/matematicsolutions/matematic-contract-review-pl)** - tam walidacja cytatu jest przy ekstrakcji z umow, tu jest osobnym ogolnym narzedziem.

## Kontekst MateMatic (TWARDE OGRANICZENIA)

Repo prowadzi [MateMatic Solutions](https://matematicsolutions.com).

- **Tajemnica zawodowa** (PoA art. 6, URP art. 3) - skrypty dzialaja **lokalnie** (Node, zero sieci). Materialy objete tajemnica pseudonimizuj przez [let-it-be](https://github.com/matematicsolutions/matematic-anonimizacja-pl) PRZED wyslaniem czegokolwiek do modelu.
- **RODO art. 4 pkt 5** - mapa pseudonimizacji to klucz re-identyfikacji (dane osobowe); **NIGDY** nie trafia do paczki audytowej ani do repo. Trzymana osobno, dostep ograniczony.
- **AI Act** - powolania weryfikowane co do numeru artykulu: art. 12 (prowadzenie rejestrow), art. 14 (nadzor czlowieka), art. 50 (poinformowanie, ze tresc powstala z udzialem AI). NIE myl art. 13 (informacja dostawcy dla podmiotu stosujacego) z art. 50.
- **Vendor-neutral** - skille nie zakladaja konkretnego modelu; rozumowanie i debata dzialaja z dowolnym providerem (decyzja kancelarii).

## Struktura repo

```
skills/
  citation-grounding-pl/
    SKILL.md
    scripts/ground-citations.mjs      - deterministyczny weryfikator (zero-dep ESM)
  adversarial-legal-review-pl/
    SKILL.md                          - orkiestracja 4 rol (bez skryptu)
  legal-ai-audit-bundle/
    SKILL.md
    scripts/assemble-bundle.mjs       - skladanie paczki + SHA256 (zero-dep ESM)
README.md     - opis dla ludzi
LICENSE       - Apache 2.0
AGENTS.md     - ten plik
CLAUDE.md     - wskaznik z @AGENTS.md import dla Claude Code
CHANGELOG.md  - historia wersji
```

## Build i test

Repo nie ma kompilacji - skille Markdown + skrypty Node (czysty ESM, zero zaleznosci).

"Test" = smoke-test skryptow:

```bash
# weryfikator cytatu - przygotuj task.json (patrz SKILL.md "Format zadania")
node skills/citation-grounding-pl/scripts/ground-citations.mjs task.json   # exit 1 = blokada

# paczka audytowa - przygotuj descriptor.json (patrz SKILL.md "Format descriptora")
node skills/legal-ai-audit-bundle/scripts/assemble-bundle.mjs descriptor.json ./out
```

Oba skrypty obsluguja BOM w plikach JSON (PowerShell `Out-File utf8` dopisuje BOM) i zwracaja exit code (1 = problem wymagajacy decyzji czlowieka).

## Zasady pisania skilli

- **Polski jezyk** - skille rozmawiaja z prawnikiem po polsku; frontmatter `description` triggeruje po polskich frazach.
- **Powolania prawne sprawdzane PRZED napisaniem** - numer artykulu AI Act / RODO / KPC weryfikowany, nie zgadywany. Jeden zly numer podwaza autorytet narzedzia, ktorego celem jest wlasnie eliminacja halucynacji.
- **Determinizm tam, gdzie to mozliwe** - weryfikacja cytatu i hashe to skrypty, nie ocena modelu "na oko". Cala wartosc w przeciwdzialaniu halucynacjom lezy w mechanicznosci.
- **Bez polskich znakow w commit messages** (konwencja organizacji).
- **Marko-pl 2x runda + humanizer-pl** przed kazdym commitem zmieniajacym tresc SKILL.md / README.md.

## Czego NIE robic (twarde reguly)

- **NIE przepuszczaj cytatu bez trafienia w zrodle** - status `NIEZWERYFIKOWANY` blokuje publikacje. Nigdy milczaco.
- **NIE wysylaj niezanonimizowanych danych osobowych do modelu** - pseudonimizacja PRZED.
- **NIE wkladaj mapy pseudonimizacji do paczki audytowej** - to klucz re-identyfikacji.
- **NIE uruchamiaj pelnej debaty adversarial dla rutyny** - bramka kosztu; dla zwyklych zapytan zwykly review.
- **NIE powoluj sie na artykul AI Act / RODO bez sprawdzenia numeru.**

## Zrodla prawdy (kolejnosc czytania)

1. [README.md](./README.md) - opis dla ludzi
2. [skills/citation-grounding-pl/SKILL.md](./skills/citation-grounding-pl/SKILL.md)
3. [skills/adversarial-legal-review-pl/SKILL.md](./skills/adversarial-legal-review-pl/SKILL.md)
4. [skills/legal-ai-audit-bundle/SKILL.md](./skills/legal-ai-audit-bundle/SKILL.md)
5. [CHANGELOG.md](./CHANGELOG.md)

## Kompatybilnosc agentow

Pattern (mechaniczna weryfikacja cytatu, kontradyktoryjna debata, audit-bundle) jest **agent-agnostic** - mozesz przepisac SKILL.md pod Cursor / Codex / Devin adaptujac frontmatter. Skrypty Node dzialaja niezaleznie od agenta. Dla Claude Code istnieje [CLAUDE.md](./CLAUDE.md) importujacy ten plik przez `@AGENTS.md`.

## Licencja i atrybucja

- **Apache 2.0** - patrz [LICENSE](./LICENSE). Mozesz wziac, modyfikowac, sprzedawac wdrozenie. Wymagamy atrybucji.
- Pattern: cherry-pick z [AnttiHero/lavern](https://github.com/AnttiHero/lavern) (Apache 2.0, snapshot 2026-05-20). Tresc napisana od zera.

Cytowanie: *MateMatic Solutions (2026), matematic-legal-verify-pl, https://github.com/matematicsolutions/matematic-legal-verify-pl, Apache 2.0.*
