# Immutability

**Immutability** of **onveranderlijkheid** is een heel belangrijk concept dat je onder de knie moet hebben. Eigenlijk is het een heel eenvoudig concept:&#x20;

_"als een variabele immutable is dan mag die na het aanmaken nooit meer aangepast worden"_

Dit wil dus zeggen als je toch een wijziging wilt maken, dat je dan een nieuwe instantie van die array of object moet aanmaken. Je zal dus moeten gebruik maken van allerlei operatoren die je hiervoor al geleerd hebt.

### Readonly

Wil je dat een bepaalde variabele niet aanpasbaar is, en dus immutable dan moet je het`readonly` keyword gebruiken:

```typescript
interface User {
    readonly name: string;
    readonly age:  number;
}
let user : User = {
    name: "John",
    age:  30
};

user.age = 40; // Cannot assign to 'age' because it is a read-only property.
```

Je kan ook een array readonly maken zodat er na de creatie ervan geen nieuwe elementen meer kunnen toegevoegd worden:

```typescript
let numbers : readonly number[] = [1,2,3];
numbers.push(4); // Property 'push' does not exist on type 'readonly number[]'.
```

### Element toevoegen aan een array

Je hebt tot nu toe altijd geleerd om elementen toe te voegen aan een array via de `push` array methode.

```typescript
let numbers : readonly number[] = [1,2,3];
numbers.push(4);
```

Zoals je hierboven hebt gezien zal dit niet gaan met een `readonly number[]` data type.

Willen we toch dingen toevoegen aan zo'n array dan moeten we een kopie maken van deze array met daarachter het nieuwe element aan toegevoegd.&#x20;

#### Achteraan de array toevoegen:

```typescript
let numbers: readonly number[] = [1,2,3];
numbers = [...numbers,4];
console.log(numbers);
```

#### Vooraan de array toevoegen:

```typescript
let numbers: readonly number[] = [1,2,3];
numbers = [4,...numbers;
console.log(numbers);
```

#### **Op een bepaalde index toevoegen:**

Hiervoor heb je de array functie `slice` nodig.&#x20;

Willen we een element op index 2 toevoegen (op de 3de plaats) dan doen we dit op de volgende manier:

<pre class="language-typescript"><code class="lang-typescript"><strong>let numbers: readonly string[] = ["een","twee","vier"];
</strong>numbers = [...numbers.slice(0, 2), "drie", ...numbers.slice(2)];
console.log(numbers); // ["een","twee","drie","vier"</code></pre>

Je zou dit kunnen veralgemenen in een zelfgeschreven functie

```typescript
const addElementAtIndex = (array: readonly string[], index: number, element: string) => {
  return [...numbers.slice(0, index), element, ...numbers.slice(index)];
}

let numbers: readonly string[] = ["een", "twee", "vier"];
console.log(addElementAtIndex(numbers, 2, "drie")); // ["een","twee","drie","vier"]
console.log(addElementAtIndex(numbers, 0, "nul")); // ["een","twee","drie","vier"]
```

[![Edit Immutable Slice](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/immutable-slice-mgzc46?fontsize=14\&hidenavigation=1\&theme=dark)

### Element verwijderen van een array

Hetzelfde geldt voor het verwijderen van elementen van een immutable array. Je zou aan de hand van `splice` (niet te vergissen met slice) het element kunnen verwijderen. Maar dit gaat niet met een immutable array.

```typescript
let numbers: readonly string[] = [ "nul", "een", "twee" ];

numbers.splice(0, 1); // Property 'splice' does not exist on type 'readonly string[]'
```

Om een nieuwe array aan te maken waar dit element niet in voorkomt kan je dit op twee manieren doen:

#### Verwijderen met slice&#x20;

Je neemt het eerste deel van de array dat je wil behouden gevolgd door het deel dat net achter het element komt dat je wil verwijderen. Wil je bijvoorbeeld het element op index 2 verwijderen kan je dat als volgt doen

```typescript
let numbers: readonly string[] = [ "nul", "een", "twee","drie" ];

numbers = [...numbers.slice(0,2), ...numbers.slice(3)];

console.log(numbers); // ["nul","een","drie"]
```

of in een functie gegoten:

```typescript
const removeElementAtIndex = (array: readonly string[], index: number) => [...numbers.slice(0,index), ...numbers.slice(index+1)];
```

[![Edit Immutable Remove At Index](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/immutable-remove-at-index-fqsc8j?fontsize=14\&hidenavigation=1\&theme=dark)

**Verwijderen met filter**

Je gebruikt de filter methode van de array om alle elementen te filteren waarvan de index niet overeenkomt met de index die je wil verwijderen. Willen we het element op index 2 verwijderen doen we dit als volgt:

<pre class="language-typescript"><code class="lang-typescript"><strong>let numbers: readonly string[] = [ "nul", "een", "twee","drie" ];
</strong>
numbers = numbers.filter((number,index) => index !== 2)

console.log(numbers);</code></pre>

of weer in een aparte functie:

```typescript
const removeElementAtIndex = (array: readonly string[], index: number) => arrays.filter((number,i) => i !== index)
```

[![Edit Immutable Remove At Index (met filter)](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/immutable-remove-at-index-met-filter-h6byw7?fontsize=14\&hidenavigation=1\&theme=dark)

### Element aanpassen in een array

Je hebt het ondertussen door. Als een array immutable is kunnen we niet zomaar de index notatie gebruiken om de array aan te passen:

```typescript
let numbers: readonly string[] = [ "zes", "een", "twee","drie" ];

numbers[0] = "nul"; // Index signature in type 'readonly string[]' only permits reading.ts(2542)
```

We kunnen dit oplossen aan de hand van de `map` functie. Willen we het element op index 0 aanpassen naar "nul" dan kunnen we dit op het volgende manier doen.

```typescript
let numbers: readonly string[] = [ "zes", "een", "twee","drie" ];

numbers = numbers.map((val,i) => (i == 0) ? "nul" : val);

console.log(numbers);
```

of weer in een aparte functie:

```typescript
const updateElementAtIndex = (array: readonly string[],index: number, newValue: string) => array.map((val, i) => (i == index ? newValue : val));
```

[![Edit Immutable Update at index](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/immutable-update-at-index-rzhr3g?fontsize=14\&hidenavigation=1\&theme=dark)

![](../.gitbook/assets/eed378f31f23235c4bedad9f1dcbf293.gif)
