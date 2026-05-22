# Changelog

Format wg [Keep a Changelog](https://keepachangelog.com/pl/1.1.0/). Wersjonowanie [SemVer](https://semver.org/lang/pl/).

## [0.1.0-alpha] - 2026-05-22

### Dodane
- **citation-grounding-pl** - mechaniczny weryfikator cytatu dla tekstow prawnych. Skrypt `ground-citations.mjs` (zero-dep ESM): normalizacja cudzyslowow/myslnikow/bialych znakow, obsluga luk `[...]`, statusy ZWERYFIKOWANY / ZMODYFIKOWANY / NIEZWERYFIKOWANY / BRAK_ZRODLA, exit 1 = blokada. Smoke-test 4 przypadkow.
- **adversarial-legal-review-pl** - kontradyktoryjny stress-test pisma wysokiej stawki (builder / attacker / synthesizer / verifier + kontrola 10-punktowa). Bramka kosztu (high-stakes vs rutyna).
- **legal-ai-audit-bundle** - paczka audytowa AI Act art. 12. Skrypt `assemble-bundle.mjs` (zero-dep ESM): manifest + SHA256 + INDEX.md, wykrywanie brakujacych artefaktow, ostrzezenie dla high-stakes. Smoke-test.
- Standard AGENTS.md + wskaznik CLAUDE.md, README, LICENSE (Apache 2.0).

### Uwagi
- Oba skrypty obsluguja BOM w plikach JSON (PowerShell `Out-File utf8`).
- Powolania AI Act zweryfikowane: art. 12 (rejestry), art. 14 (nadzor), art. 50 (poinformowanie o AI). RODO art. 4 pkt 5 (pseudonimizacja).
- Cherry-pick patternu z AnttiHero/lavern (Apache 2.0); kod i prompty napisane od zera.
