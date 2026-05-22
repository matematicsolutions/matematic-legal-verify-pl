# matematic-legal-verify-pl - warstwa weryfikacji outputu AI prawnego

Trzy otwarte skille **Claude Code**, ktore pilnuja, zeby wynik pracy AI nadawal sie do wyslania do klienta lub sadu: **czy cytaty sa prawdziwe**, **czy teza wytrzymuje atak** i **czy zostal slad zgodny z AI Act**.

To nie jest narzedzie do *pisania* pism. To warstwa, ktora sprawdza pisma **zanim wyjda za drzwi**.

## Trzy skille

| Skill | Co robi | Skrypt |
|---|---|---|
| **citation-grounding-pl** | Mechaniczny weryfikator cytatu - string-matchem sprawdza, czy kazdy cytat z orzeczenia / ustawy / umowy faktycznie istnieje w zrodle. Brak trafienia = potencjalna halucynacja, blokada. | `ground-citations.mjs` |
| **adversarial-legal-review-pl** | Czerwony zespol dla pisma wysokiej stawki - builder buduje teze, attacker ja atakuje kontr-orzecznictwem, synthesizer godzi, verifier robi kontrole koncowa. Z bramka kosztu. | - |
| **legal-ai-audit-bundle** | Pakuje deliverable + slad rozumowania + raport cytatow + log kosztu w jeden folder z manifestem i hashami SHA256 - artefakt zgodny z AI Act art. 12. | `assemble-bundle.mjs` |

Skille sa **composable**: grounding jest punktem kontrolnym wewnatrz adversarial-review, a oba zasilaja audit-bundle dowodem.

## Dla kogo

- **Adwokat / radca** - kontrola cytatow w opinii przed wyslaniem do klienta (odpowiedzialnosc zawodowa, PoA art. 6).
- **Prawnik transakcyjny** - stress-test memo DD / M&A zanim zrobi to druga strona.
- **Inspektor ochrony danych / compliance** - dowod nalezytej starannosci i record-keeping dla AI Act (art. 12, 14, 50).
- **Kazdy, kto wkleja output LLM do pisma** - bo model potrafi wygenerowac fragment wyroku, ktorego nigdy nie bylo.

## Filozofia

1. **Cytat niezweryfikowany mechanicznie = cytat zmyslony, dopoki nie udowodnisz inaczej.** Weryfikacja jest string-matchem, nie pytaniem do modelu "czy pasuje".
2. **Lepiej, zeby slabosc tezy znalazl Twoj wlasny agent niz przeciwnik na rozprawie.** Kontradyktoryjnosc wbudowana w proces.
3. **Jesli nie potrafisz odtworzyc, jak AI doszlo do wyniku, nie powinienes tego wyniku wysylac.** Slad rozumowania to nie biurokracja, to odpowiedzialnosc.
4. **RODO-safe** - skrypty dzialaja lokalnie (Node, zero sieci); materialy objete tajemnica zawodowa pseudonimizuj wczesniej przez [let-it-be](https://github.com/matematicsolutions/matematic-anonimizacja-pl). Mapa pseudonimizacji nigdy nie trafia do paczki audytowej.

## Pozycjonowanie wzgledem reszty stacku

- **To nie [Patron](https://github.com/matematicsolutions/patron).** Patron pracuje nad sprawa (research / drafting / RAG). Te skille **sprawdzaja** dowolny output prawny, niezaleznie od tego, kto go wyprodukowal.
- **To nie [matematic-contract-review-pl](https://github.com/matematicsolutions/matematic-contract-review-pl).** Tamten skill ma wbudowana walidacje cytatu *przy ekstrakcji z umow*. `citation-grounding-pl` to ta sama idea **wyciagnieta jako osobne, ogolne narzedzie** dzialajace na dowolnym tekscie prawnym.
- **Companion:** [saos-orzecznictwo](https://github.com/matematicsolutions/mcp-saos), [mcp-nsa](https://github.com/matematicsolutions/mcp-nsa), [mcp-eu-sparql](https://github.com/matematicsolutions/mcp-eu-sparql) (pobieranie zrodel do weryfikacji), [let-it-be](https://github.com/matematicsolutions/matematic-anonimizacja-pl) (pseudonimizacja PRZED).

## Instalacja (Claude Code)

```bash
cd ~/.claude/skills/
git clone https://github.com/matematicsolutions/matematic-legal-verify-pl
# Linux/macOS - symlink trzech skilli:
ln -s matematic-legal-verify-pl/skills/citation-grounding-pl citation-grounding-pl
ln -s matematic-legal-verify-pl/skills/adversarial-legal-review-pl adversarial-legal-review-pl
ln -s matematic-legal-verify-pl/skills/legal-ai-audit-bundle legal-ai-audit-bundle
```

Na Windows zamiast symlinka - kopia trzech folderow `skills/*` do `~/.claude/skills/`.

Skrypty wymagaja **Node.js** (zero zaleznosci, czysty ESM). Sprawdz: `node --version`.

## Status

`v0.1.0-alpha`. Skrypty maja smoke-testy (patrz [CHANGELOG.md](CHANGELOG.md)); semantyka prawna (sygnatury, formaty cytatow) zwalidowana na przykladach syntetycznych, nie na pelnym korpusie orzecznictwa. Zglaszaj luki przez Issues.

## Licencja i atrybucja

- **Apache 2.0** - patrz [LICENSE](LICENSE). Mozesz wziac, modyfikowac, sprzedawac wdrozenie; wymagamy atrybucji.
- Pattern (mechanical grounding, debate + 3-layer verification, audit-bundle): cherry-pick z [AnttiHero/lavern](https://github.com/AnttiHero/lavern) (Apache 2.0, snapshot 2026-05-20). **Kod, prompty i reguly napisane od zera** pod polskie realia (cudzyslowy, sygnatury, CELEX, AI Act, RODO). Nie skopiowano kodu ani 67 promptow Lavern (semantyka US common law).

Cytowanie: *MateMatic Solutions (2026), matematic-legal-verify-pl - warstwa weryfikacji outputu AI prawnego, https://github.com/matematicsolutions/matematic-legal-verify-pl, Apache 2.0.*
