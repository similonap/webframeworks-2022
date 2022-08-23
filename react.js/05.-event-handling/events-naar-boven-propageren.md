---
description: >-
  In React vloeit data via props "naar beneden". Met de juiste technieken kan
  data ook terug naar boven gestuurd worden.
---

# events naar boven propageren

Tot nu hebben we props en state gezien om informatie bij te houden en door te geven tussen verschillende componenten. Maar we missen nog een belangrijk concept om communicatie tussen componenten te laten gebeuren. Data moet niet alleen **naar beneden vloeien** (via props), maar moet ook terug **naar boven gaan:**

![](<../../.gitbook/assets/data flow.png>)

Props kunnen alleen informatie naar beneden doorgeven. Om communicatie terug naar boven mogelijk te maken, geeft een oudercomponent **via een extra prop** een functie mee die gedefinieerd is in de scope van die oudercomponent. Deze functie heet een **callback handler**. In het voorbeeld hierboven zal dus een functie moeten meegegeven worden van `App` naar `InputView`. Deze functie zal dienen om het toevoegen van één game af te handelen.

Voor `InputView` deze functie kan aanvaarden, moet een functie van dit type toegevoegd worden aan de interface voor de props van `InputView`:

```typescript
interface InputViewProps {
  onAdd: (game: Game) => void
}
```

Vervolgens voegen we deze properties toe aan de `InputView` component:

```typescript
const InputView = ({onAdd} : InputViewProps) => {
    ...
}
```

Nu kan de functie meegegeven worden:

```typescript
const App = () => {
  ...
  const handleAdd = (game: Game) => {
    console.log(game);
  }

  return (
    <div>
      <Header />
      <List games={games} />
      <InputView onAdd={handleAdd}/>
    </div>
  );
}
```

Nu moet de code voor `InputView` deze handler activeren wanneer er daadwerkelijk een element wordt toegevoegd. Dit wil zeggen: wanneer er op de knop om toe te voegen wordt geklikt. Op dit moment wordt een `Game` aangemaakt. Deze wordt dan meegegeven aan `onAdd`:

```typescript
const handleClick: MouseEventHandler<HTMLInputElement> = (event) => {
  onAdd({
    name: name,
    releaseYear: year,
    sales: 0
  })
}
```

Om nu de applicatie volledig functioneel te maken, moet de game worden toegevoegd aan de lijst met data in `App`. Omdat deze lijst kan veranderen doorheen de tijd, moet hij eerst ook omgezet worden naar state van `App`.

```typescript
const [games, setGames] = useState<Game[]>([
  {
    id: 0,
    name: "World of Warcraft",
    releaseYear: 2004,
    sales: 0
  },
  {
    id: 1,
    name: "Valheim",
    releaseYear: 2021,
    sales: 0
  },
  {
    id: 2,
    name: "Minecraft",
    releaseYear: 2011,
    sales: 0
  }
]);
```

Je eerste impuls is misschien om de `games` array rechtstreeks aan te passen.&#x20;

```typescript
const handleAdd = (game: Game) => {
  games.push(game); // FOUT!
}
```

Maar een aanpassing aan de state moet via de setterfunctie verlopen. We gebruiken hier de spread operator om de inhoud van de games array te nemen en een element toe te voegen.

```typescript
const handleAdd = (game: Game) => {
  setGames([...games, game])
}
```

Als je nu op de add knop drukt wordt de games state aangepast en dan zal ook de `List` component opnieuw gerenderd worden. Dit komt omdat `games` daar wordt meegegeven als prop. Als de props van een component updaten, dan wordt de component zelf ook opnieuw gerenderd.

Momenteel hebben we nog wel geen waarde voor `id` aan deze game gegeven, want `InputView` had geen zicht op de volledige lijst en kon dus geen ongebruikte waarde bepalen. We kunnen hier het maximum berekenen van alle `id` waarden van alle games en deze met `1` verhogen.

```typescript
const handleAdd = (game: Game) => {
  let max = Math.max(...games.map((g) => g.id!),0) + 1;
  game.id = max;
  setGames([...games, game])
}
```

Onderstaande CodePen de samenwerking van props en callback handlers:

{% embed url="https://codepen.io/microdegree-webontwikkeling-ap/pen/PoQwNJj?editors=1011" %}
