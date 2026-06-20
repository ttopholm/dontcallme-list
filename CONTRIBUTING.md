# Bidrag til DontCallMe-listen

Listerne i `lists/` **auto-genereres** af [dontcallme.eu](https://dontcallme.eu)
ud fra community-rapporter (vægtet efter hvor mange uafhængige kilder der har
markeret et nummer). Du bidrager derfor **ikke** ved at redigere `lists/*.csv` —
de bliver overskrevet ved næste eksport. Brug i stedet én af vejene nedenfor.

## 1. Rapportér et nummer (nemmest — alle kan)

1. Gå til [dontcallme.eu](https://dontcallme.eu) og slå nummeret op (du behøver
   ikke taste landekode — vælg land i feltet).
2. Tryk **Rapportér** og vælg kategori (spam / telemarketing / svindel / robocall).
3. Det lander i databasen med det samme. **Konti vægter mere end anonyme
   rapporter**, og et nummer kommer i listerne når nok uafhængige rapporter peger
   på det (strict ≥ 6, standard ≥ 3, aggressive ≥ 1 i vægt).

Det er den primære vej og kræver ingen PR.

## 2. Curaterede / myndigheds-numre (bulk)

Har du numre fra en **offentliggjort, troværdig kilde** (Forbrugerombudsmanden,
politiet, bankernes svindel-advarsler m.m.)?

1. Tilføj dem til en CSV i [`seeds/`](seeds/) (`number,category,source,note`) og
   lav en pull request — husk kilden i `note`.
2. En maintainer kører `scripts/import-seeds.mjs` (dontcallme-eu), som indsætter
   dem i DB'en; eksporten samler dem op i `lists/`.

Se [`seeds/README.md`](seeds/README.md) for format og detaljer.

> **Ingen scraping** af lukkede databaser (fx konkurrenters opslagstjenester) —
> det er både en juridisk (EU-databaseret/GDPR) og etisk no-go. Kun offentligt
> tilgængelige kilder.

## Vær ansvarlig

- **Rapportér kun numre du har konkret grund til at mistænke.** Listen kan
  blokere opkald for andre — et forkert nummer rammer en uskyldig.
- **Ingen privatpersoner i en konflikt, ingen chikane.** Det her er mod
  spam/telemarketing/svindel, ikke et redskab til at genere nogen.
- Er det dit eget nummer der er havnet på listen ved en fejl? Brug **"Er det dit
  nummer?"** på nummer-siden (appel) eller skriv til hello@dontcallme.eu.

## Format (kun til reference — output)

`lists/*.csv` er `number,label`, fx:

```csv
number,label
+4570123456,Spam
```

Numre er E.164. Ranges (`A..B`) og præfiks (`+4570*`) understøttes i app-laget
(Android matcher alle i realtid; iOS udfolder ranges og springer præfiks/store
ranges over, da CallKit kun kan eksakte numre).
