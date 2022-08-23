# Sets

Soms wil je voorkomen dat bepaalde waarden meerdere keren voorkomen in een lijst. Je zou hiervoor elke keer kunnen nakijken of een bepaalde waarde al in de array zit zoals hieronder:

```typescript
let users: string[] = [];

let pushIfNotExists = (username: string) => {
  if (!users.includes(username)) {
    users.push(username);
  }
};

pushIfNotExists("Andie");
pushIfNotExists("Sven");
pushIfNotExists("Andie");

console.log(users); // ["Andie","Sven"]
```

[![Edit Sets (without Set)](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/sets-without-set-yhkdt0?fontsize=14\&hidenavigation=1\&theme=dark)

Dit is uiteraard een beetje veel code en ook zeer inefficiënt!&#x20;

We willen hier gebruik maken van een Set. Een set is een soort lijst die alleen maar unieke waarden kan bevatten.&#x20;

```typescript
let users: Set<string> = new Set();

users.add("Andie");
users.add("Sven");
users.add("Andie");

console.log(Array.from(users));
```

[![Edit Sets](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/sets-fq1nlg?fontsize=14\&hidenavigation=1\&theme=dark)

Wil je deze lijst gebruiken zoals een array moet je deze wel nog terug omzetten naar een Array aan de hand van de `Array.from` functie.

Een concreet voorbeeld waar het gebruik van een `Set` interessant kan zijn is het volgende.

```typescript
let animals : Animal[] = [
  {
    name:"Lion",
    category: "carnivore"
  },
  {
    name:"dog",
    category:"pet"
  },
  {
    name:"cat",
    category:"pet"
  },
  {
    name:"wolf",
    category:"carnivore"
  }
];
```

Stel dat we van deze dieren een lijst willen hebben van alle categorieën. We kunnen dit doen aan de hand van de map functie

```typescript
animals.map(animal => animal.category);
// ["carnivore","pet","pet","carnivore"]
```

Je ziet hierboven dat deze lijst dubbele waarden bevat. Willen we er nu voor zorgen dat alle dubbele waarden uit deze lijst worden gehaald kunnen we gebruik maken van een handig truukje met `Set`. We gebruiken de Set constructor om deze array om te zetten naar een `Set` en vervolgens zetten we deze terug om naar een Array aan de hand van de `Array.from` functie

```typescript
Array.from(new Set(animals.map((animal) => animal.category)));
// ["carnivore","pet"]
```

Het volledige voorbeeld:

```typescript
interface Animal {
  name: string;
  category: string;
}

let animals: Animal[] = [
  {
    name: "Lion",
    category: "carnivore"
  },
  {
    name: "dog",
    category: "pet"
  },
  {
    name: "cat",
    category: "pet"
  },
  {
    name: "wolf",
    category: "carnivore"
  }
];

console.log(Array.from(new Set(animals.map((animal) => animal.category))));
```

[![Edit Sets - Animals](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/sets-animals-pd5ikq?fontsize=14\&hidenavigation=1\&theme=dark)

