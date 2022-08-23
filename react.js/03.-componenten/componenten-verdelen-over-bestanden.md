---
description: >-
  Componenten zijn niet gebonden aan een bepaalde bestandenstructuur, maar het
  is zinvol hier afspraken rond te maken.
---

# Componenten verdelen over bestanden

In [onze eerste React applicatie](../../01.-eerste-react-applicatie.md#je-eerste-component) was alle logica vervat in één component, namelijk de functie `App` in het bestand `App.tsx`. Als alle code in hetzelfde bestand staat, is het moeilijk om de code terug te vinden die op een gegeven moment belangrijk is. Bovendien is het dan ook niet mogelijk één component te herbruiken zonder de code van andere componenten over te nemen.

In React zijn componenten niet gekoppeld aan een specifieke bestandenstructuur. Toch zijn er bepaalde, veel toegepaste, afspraken. Deze worden hier toegelicht.

{% hint style="info" %}
Er bestaan ook componenten die _geen_ functie zijn, maar in moderne React code worden functiegebaseerde componenten sterk aangeraden.
{% endhint %}

### Aparte bestanden per component

Een eerste verbetering bestaat erin elke component in een eigen bestand te plaatsen. Componenten kunnen dan op dezelfde manier geïmporteerd worden als andere functies. Indien er bijvoorbeeld componenten `App`, `Header`, `InputView`, `List` en `ListItem` zijn, worden deze ondergebracht in `App.tsx`, `Header.tsx`, `InputView.tsx`, `List.tsx` en `ListItem.tsx`.

Het bestand `List.tsx` kan er dan als volgt uitzien:

```typescript
import React from "react";
import ListItem from "./ListItem";
import { Game } from "./types";

interface ListProps {
  games: Game[]
}

const List = ({ games }: ListProps) => {
  return (
    <ul>{games.map((game: Game) => {
      return <ListItem game={game}/>
    })}
    </ul>
  );
}

export default List;
```

### Van component files naar React folders

Naarmate een project groeit, worden componenten complexer. Er worden styles en tests  toegevoegd. Je zou de vorige structuur kunnen blijven volgen en deze bestanden naast de component bestanden plaatsen. Vaak worden deze bestanden ook nog in een folder components geplaatst om het onderscheid te maken met andere code.

De bestandenstructuur ziet er dan bijvoorbeeld zo uit:

```
src
└── components
    ├── App.css
    ├── App.test.tsx
    ├── App.tsx
    ├── List.css
    ├── List.test.tsx
    └── List.tsx
```

Op termijn is deze aanpak niet houdbaar. Daarom verkiest men bij grotere projecten vaak één directory per component, als volgt:&#x20;

```
src
└── components
    ├── App
    │   ├── App.css
    │   ├── App.test.tsx
    │   └── App.tsx
    └── List
        ├── List.css
        ├── List.test.tsx
        └── List.tsx
```

De bestandsnamen in dit voorbeeld zijn zuiver ter illustratie. Afhankelijk van het gebruikte test framework, de techniek om componenten te stylen enzovoort kunnen er andere afspraken gevolgd worden. Hoe dan ook staan folders toe tijdens het ontwikkelen niet-relevante bestanden te verbergen.&#x20;

In sommige gevallen horen componenten sterk samen. In het voorbeeld bovenaan deze pagina werd gebruikt gemaakt van `ListItem`. Deze wordt enkel gebruikt als onderdeel van de `List` component. Meestal worden zo'n componenten samen in één folder geplaatst, als volgt:

```
src
└── components
    ├── App
    │   ├── App.css
    │   ├── App.test.tsx
    │   └── App.tsx
    └── List
        ├── List.css
        ├── List.test.tsx
        ├── List.tsx
        ├── ListItem.css
        ├── ListItem.test.tsx
        └── ListItem.tsx
```
