# Lazy evaluation

Een belangrijk concept om te kennen in TypeScript is het concept van Lazy Evaluation. Dit wil zeggen dat een expressie niet helemaal wordt geëvalueerd als het voorgaande deel al undefined/null of false is.

Voorbeeld:

```typescript
let fetchThis = () => {
  return undefined;
};

let doSomething = () => {
};

fetchThis() && doSomething();
```

Als `fetchThis` undefined teruggeeft (of false of null) dan zal `doSomething` nooit worden uitgevoerd.&#x20;

Zo kan je bijvoorbeeld enkel de `console.log` functie aanroepen als de waarde die je wil afprinten gedefinieerd is:

```typescript
let value1: string | undefined = undefined;
let value2: string = "Hello World";

value1 && console.log(value1); // Print niets af
value2 && console.log(value2); // Hello World
```

We kunnen hier ook het probleem van "Object possibly undefined" mee oplossen:

```typescript
interface Size {
  width: number;
  height: number;
}
interface Options {
  title: string;
  size?: Size;
}

let options: Options = {
  title: "Hello World"
  // size: {
  //   width: 100,
  //   height: 100
  // }
};

options.size && console.log(options.size.width);
```

[![Edit Conditional Console.log](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/conditional-console-log-qmemp9?fontsize=14\&hidenavigation=1\&theme=dark)
