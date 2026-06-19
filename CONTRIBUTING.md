# Bidrag til spam-listen

Denne mappe indeholder `numbers.csv` — listen af telefonnumre som DontCallMe-appen
blokerer/identificerer. Appen henter filen fra et endpoint (rå CSV-URL), så når et
nummer tilføjes her og ændringen er live, opdateres appen ved næste refresh.

## Format

Én linje pr. nummer:

```csv
number,label
+4570123456,Spam
+4532123456,Telemarketing
+4538123456,
```

- **`number`** (påkrævet) — én af tre former:
  - **eksakt**: `+4570123456` (national form `70123456` accepteres og normaliseres)
  - **range**: `+4570123400..+4570123499` (delimiter `..`, begge ender med)
  - **præfiks**: `+4570*` (alle numre der starter med `+4570`)
- **`label`** (valgfri): kort tekst der vises ved identifikation. Tom label → appen
  viser `Spam`.

### Platform-forskel på range/præfiks
- **Android** matcher alle tre former i realtid.
- **iOS** kan kun eksakte numre: en range *udfoldes* til enkelt-numre (op til
  10.000), men **præfiks og meget store ranges springes over på iOS** (de virker
  kun på Android). Brug eksakte numre eller små ranges hvis det skal virke på begge.

Følgende håndteres automatisk af appen, så vær ikke bange for det:
- tomme linjer og `#`-kommentarer ignoreres,
- mellemrum og separatorer (`-`, `.`, `()`) trimmes,
- ugyldige numre frasorteres (valideret med libphonenumber),
- dubletter fjernes.

## Sådan tilføjer du et nummer

1. Bekræft at det er et reelt spam-/telemarketing-/svindelnummer — gerne med en
   kort begrundelse i din PR (fx "ringede 5×, optog, telemarketing").
2. Tilføj én linje under `number,label`-headeren i `numbers.csv`.
3. Brug E.164 (`+45…`) når du kan, og en kort, saglig label.
4. Lav en pull request (eller commit, hvis du har adgang).

## Vigtigt — vær ansvarlig

- **Tilføj kun numre du har konkret grund til at mistænke.** Listen kan blokere
  opkald for andre brugere — et forkert nummer rammer en uskyldig.
- **Ingen privatpersoner i en konflikt, ingen chikane.** Det her er mod
  spam/telemarketing/svindel, ikke et redskab til at genere nogen.
- Tag fejl af et nummer? Fjern linjen i en PR — det rettes ved næste refresh.

## Kvalitet

- Hold labels korte og neutrale (`Spam`, `Telemarketing`, `Svindel`, `Robocall`).
- Undgå dubletter (appen fjerner dem, men en ren liste er nemmere at vedligeholde).
- Sortér gerne, men det er ikke påkrævet — appen sorterer selv til iOS.
