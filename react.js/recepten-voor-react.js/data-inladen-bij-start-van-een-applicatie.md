---
description: >-
  Om externe data op te halen en weer te geven in een React app, moet je met een
  aantal zaken rekening houden.
---

# data inladen bij start van een applicatie

Single page applicaties scheiden bijna altijd hun applicatiecode van hun data. De applicatiecode is stabiel, maar de data waarmee een gebruiker werkt kan voortdurend veranderen. Vaak wordt ze bijgehouden in een database. Dit is mogelijk omdat de applicatiecode de data inlaadt wanneer nodig. Omdat dit patroon vaak voorkomt en omdat er enkele aandachtspuntjes bij zijn, doen we dit steeds volgens een vertrouwd stappenplan.

Hieronder krijg je een samenvatting van alle onderdelen en voorbeeldcode die je kan aanpassen aan je eigen noden:

### 1. (een) interface(s) voor je datatype(s)

De data die je wil inladen is van een bepaalde soort. In TypeScript definiëren we hier een interface voor.

Veronderstel dat we een prijslijst van producten willen laten zien, dan kunnen we bijvoorbeeld volgende interface voorzien:

```typescript
interface Product {
  name: string,
  cost: number
}
```

### 2. state om de data bij te houden

De data zal eenmalig ingeladen worden en wordt dan bijgehouden en geüpdatet zo lang de applicatie loopt. Hiervoor gebruiken we state. Normaal is het type een lijst van objecten met de eerder voorziene interface.

Bijvoorbeeld:

```typescript
const [products, setProducts] = useState<Product[]>([]);
```

### 3. een side-effect om de data in te laden bij opstart

Om te begrijpen hoe dit werkt, brengen we eerst een paar vaststellingen samen.

* De data ophalen gaat verder dan het verwerken van props om HTML te renderen. Het is dus een side-effect en we zullen `useEffect` gebruiken.
* Het effect loopt enkel bij opstart van de applicatie, dus `useEffect` wordt voorzien van een lege dependency array.
* Er zal geen cleanup nodig zijn voor een volgende update van de component, want de data wordt eenmalig opgevraagd aan de externe dienst en daarna wordt deze niet verder bezet. De functie die we meegeven aan useEffect heeft dus **geen returnwaarde**.
* Data opvragen doen we door middel van de functie `fetch`. Deze is asynchroon. Het kan dus even duren voor er een antwoord komt en het return type is `Promise<Response>`.

#### Zonder error handling

Samen zorgen deze factoren er voor dat deze code op een nogal ongewone manier geschreven moet worden. Intuïtief verwachten nieuwkomers vaak een implementatie zoals deze:

```typescript
useEffect(async () => { // nodig omdat deze functie await gebruikt
  const response = await fetch('http://localhost:3001/products');
  const json = await response.json() as Product[];
  setProducts(json);
}, []);
```

**Dit werkt niet.** Asynchrone functies leveren altijd een `Promise` op en `useEffect` aanvaardt dit niet. De meegegeven functie mag dus niet `async` zijn, maar moet wel gebruik maken van `fetch` en van de methode `json` van een `Response`. Een oplossing is om **wel gebruik te maken van een asynchrone methode, maar het side-effect niet te laten wachten**. Dit kan door de asynchrone operaties en het instellen van de data te groeperen in één asynchrone functie (waarin de operaties wel op elkaar wachten), maar niet te wachten op het resultaat van deze gegroepeerde functie. Dit ziet er zo uit:

```typescript
async function fetchAndSetProducts() {
  const response = await fetch('http://localhost:3001/products');
  const json = await response.json() as Product[];
  setProducts(json); // zonder return
}

useEffect(() => {
  fetchAndSetProducts(); // zonder await
}, []);
```

#### Met error handling

In de praktijk kan het voorvallen dat de oproep van `fetch` geen antwoord kan produceren. Bovendien kan het zijn dat er wel een antwoord wordt geproduceerd, maar dat dit geen gunstig antwoord is. Wat er in beide scenario's moet gebeuren is afhankelijk van de applicatie. We zullen hier veronderstellen dat de gebruiker gewoon verwittigd moet worden. Dit kan als volgt:

```typescript
async function fetchAndSetProducts() {
  const response = await fetch('http://localhost:3001/products');
  if (!response.ok) {
    throw new Error("Antwoord gekregen, maar niet het verwachte.");
  }
  const json = await response.json() as Product[];
  setProducts(json);
}

useEffect(() => {
  fetchAndSetProducts().catch((reason) => alert(reason));
}, []);
```
