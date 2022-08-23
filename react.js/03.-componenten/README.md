---
description: >-
  Componenten staan toe een applicatie op te bouwen uit herbruikbare bouwstenen
  die in isolatie opgebouwd en getest kunnen worden.
---

# 03. componenten

Code die gegenereerd wordt door create-react-app bevat één hoofdfunctie: `App`. Het is mogelijk alle code onder te brengen in deze functie, maar dan is het niet meer mogelijk (bijvoorbeeld in een later project) een onderdeeltje van de applicatie te herbruiken. Het is beter de applicatie op te splitsen in kleine herbruikbare blokjes. Deze herbruikbare blokjes heten **componenten.**

### **Componenten afsplitsen**

We illustreren dit met onderstaande applicatie, die een lijst van games toont:

{% embed url="https://codepen.io/microdegree-webontwikkeling-ap/pen/qBxEBqm" %}

{% hint style="info" %}
Deze applicatie is niet erg interactief, dus je zou ze als statische HTML-pagina kunnen schrijven. Uitgebreidere code zou de lijst met games uit een database kunnen inladen of zou het mogelijk maken games toe te voegen,...
{% endhint %}

De lijst met games is een deel van de website dat als één geheel beschouwd kan worden. Daarom is het zinvol deze af te zonderen in een component. We noemen deze `List`:

{% embed url="https://codepen.io/microdegree-webontwikkeling-ap/pen/KKQwKXK" %}

Een component is hier geschreven als functie die TSX teruggeeft als returnwaarde. We hebben deze TSX verplaatst van de `App` component naar de `List` component.

De component gebruiken in `App` kan door hem te behandelen als een HTML-tag. Daarom staat er `<List />`.

### Props

De `List` component hierboven is afhankelijk van de variabele `games`. Hij is dus nog niet echt herbruikbaar, want deze variabele is gedefinieerd in een bredere scope. In React kan dit opgelost worden door van de lijst met games een input van `List` te maken. Deze input wordt dan voorzien door de component die de lijst aanmaakt. Dergelijke inputs van een component heten **properties** of **props**.

Het volgende voorbeeld toont de nodige stappen om de lijst met games om te zetten naar een prop. Eerst verwijderen we de variabele `games` uit de globale scope en maken we deze lokaal binnen `App` als volgt:

```typescript
const App = () => {
  // staat nu binnen App
  const games: Game[] = [
    ...
  ]
  // rest van de code is ongewijzigd  
  return (
    ...
  );
}
```

Nu geeft de `List` component een foutmelding omdat de games variabele niet in scope is. We kunnen dit oplossen door de `games` variabele door te geven aan de `List` component aan de hand van een prop. In TSX ziet dit er uit alsof er een HTML-attribuut is voor de `List` component, met als waarde een TypeScript lijst in plaats van een string.

```typescript
return (
  <div>
    <Header/>
    <List games={games}/>
    <InputView/>
  </div>
);
```

Alle props worden gebundeld in één object. De properties van de `List` component moeten dan nog beschreven worden aan de hand van een interface. We geven deze interface meestal de naam van de component gevolgd door `Props`:

```typescript
interface ListProps {
  games: Game[]
}
```

Als je deze interface dan wil gebruiken in je component kan je deze als volgt gebruiken.&#x20;

```typescript
const List = (props: ListProps) => {
  return (
      <ul>{props.games.map((game: Game) => {
        ....
      })}
      </ul>
  );
}
```

Onderstaande CodePen toont de omgezette code:

{% embed url="https://codepen.io/microdegree-webontwikkeling-ap/pen/PoQwZZM" %}

Het is mogelijk de code verder op te delen in componenten om het overzicht te bewaren. Zo kunnen we een component `ListItem` introduceren die verantwoordelijk is voor het tonen van 1 item van de lijst.

```typescript
interface ListItemProps {
  game: Game
}

const ListItem = (props: ListItemProps) => {
  const game = props.game;
  return (
    <div key={game.id}>
      <div>{game.name}</div>
      <div>{game.releaseYear}</div>
      <div>{game.sales}</div>
    </div>
  )
}
```

Dit staat toe `List` verder te vereenvoudigen:

```typescript
const List = (props: ListProps) => {
  return (
    <ul>{props.games.map((game: Game) => {
      return <ListItem game={game}/>
    })}
    </ul>
  );
}
```
