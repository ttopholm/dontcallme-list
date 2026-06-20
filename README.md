# dontcallme-list

Offentlige spam-nummerlister til [DontCallMe](https://dontcallme.eu)-appen.
Hentes direkte af appen som rå CSV.

## Lister (tiers)

| Fil | Hvornår et nummer er med |
|-----|--------------------------|
| `lists/strict.csv` | Kun højt-verificerede numre — færrest falske positive |
| `lists/standard.csv` | Moderat tærskel — **app-default** |
| `lists/aggressive.csv` | Alt rated som spam — flest numre, flere falske positive |

Listerne **genereres automatisk** af **dontcallme.eu** ud fra community-ratings
(eksport dagligt). Redigér dem ikke i hånden — de overskrives. Bidrag i stedet
ved at rapportere på dontcallme.eu, eller tilføj curaterede/myndigheds-numre i
[`seeds/`](seeds/). Se [CONTRIBUTING.md](CONTRIBUTING.md).

## Format

Én linje pr. regel, `number,label`:

- **eksakt**: `+4570123456`
- **range**: `+4570123400..+4570123499` (delimiter `..`)
- **præfiks**: `+4570*` (alle der starter med `+4570`)

Tom label → appen viser "Spam". Linjer der starter med `#` er kommentarer.
Se [CONTRIBUTING.md](CONTRIBUTING.md). NB: på iOS udfoldes ranges (op til 10.000)
og præfiks/store ranges springes over (CallKit kan kun eksakte numre); Android
matcher alle tre i realtid.

## Brug i appen

App-default peger på `lists/standard.csv`. I appen kan du vælge tier
(Streng/Standard/Aggressiv) eller indtaste din **egen CSV-URL**.

Rå-URL'er:
```
https://raw.githubusercontent.com/ttopholm/dontcallme-list/main/lists/strict.csv
https://raw.githubusercontent.com/ttopholm/dontcallme-list/main/lists/standard.csv
https://raw.githubusercontent.com/ttopholm/dontcallme-list/main/lists/aggressive.csv
```
