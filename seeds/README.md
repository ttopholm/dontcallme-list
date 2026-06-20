# seeds/ — curaterede kilder

Numre fra **offentliggjorte** kilder (myndigheds-, bank- og politi-advarsler om
spam/svindel) der seeder DontCallMe-listen.

Hvorfor en separat mappe: `lists/*.csv` **auto-genereres** af eksport-jobbet ud
fra DB'en (community-ratings). Manuelle ændringer i `lists/` overskrives. Derfor
samles curaterede numre her, importeres til DB'en, og dukker så op i `lists/`.

## Format (`number,category,source,note`)

```csv
number,category,source,note
+4570123456,telemarketing,forbrugerombudsmanden,"Advarsel 2026-06"
70123457,spam,politi
```

- **number** — E.164 (`+45…`) eller lokalt (normaliseres, default DK).
- **category** — `spam | telemarketing | scam | robocall | other` (default `spam`).
- **source** — attribution; bliver til en stabil nøgle så samme (nummer, kilde)
  kun tæller én gang. To kilder for samme nummer → mere vægt.
- **note** — valgfri; angiv gerne dato/link til den offentlige advarsel.

## Sådan kommer de i listerne

En maintainer kører importen (kræver DB-adgang):

```bash
DATABASE_URL=… node scripts/import-seeds.mjs \
  https://raw.githubusercontent.com/ttopholm/dontcallme-list/main/seeds/authorities.csv
```

Eksport-jobbet (dagligt) regenererer `lists/*.csv`. Se `docs/seeding.md` i
dontcallme-eu for detaljer og flags (`--weight`, `--dry`, …).

## Regler

- Kun **offentligt tilgængelige, troværdige** kilder — medsend kilden i `note`.
- **Ingen scraping** af lukkede databaser (juridisk + etisk no-go).
- Brug allowlist (admin) til numre der aldrig må blokeres (alarm/myndighed).
