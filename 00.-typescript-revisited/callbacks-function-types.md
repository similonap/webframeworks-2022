# Callbacks/Function types

Een callback is een functie (functie A) die wordt meegegeven als parameter van een andere functie (functie B). Deze functie (B) zal dan de meegegeven functie (A) uitvoeren.

Dit ziet er in code als volgt uit:

```typescript
interface Callback {
    (): void
}

let functionA = (functionB: Callback) => {
    functionB();
}

let functionB: Callback = () => {
    console.log("Function B executed")
}

functionA(functionB);
```

Wil je geen interface aanmaken kan je ook gebruik maken van TypeScript types rechtstreeks in de functie signature.

```typescript
let functionA = (functionB: () => void) => {
    functionB();
}
```

Een voorbeeld van zo'n callback functie kan je hieronder vinden

```typescript
let sum = (a: number, b: number, callback: (sum: number) => void) => {
  callback(a + b);
};

let printNumber = (number: number) => {
  console.log(number);
};

sum(1, 2, printNumber);
```

[![Edit Callback functions](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/callback-functions-dcbzbm?fontsize=14\&hidenavigation=1\&theme=dark)



