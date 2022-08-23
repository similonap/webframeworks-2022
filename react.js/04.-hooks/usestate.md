---
description: >-
  State en props behoren tot de meest essentiële concepten van React. State
  wordt gebruikt om componenten eigen data te laten bijhouden doorheen de tijd.
---

# useState

State wordt gebruikt om informatie bij te houden en deze aan te passen over de looptijd van een applicatie. Het gaat dus om informatie die niet van een oudercomponent komt, maar die door de component zelf beheerd wordt.

We gebruiken hiervoor het voorbeeld van`InputView`. We kunnen state bijvoorbeeld gebruiken om te zorgen dat de tekst die door de gebruiker wordt ingetypt ergens anders op de pagina verschijnt. We starten vanaf volgende definitie:

```typescript
const InputView = () => {
  const handleChange: ChangeEventHandler<HTMLInputElement> = (event) => {
    console.log(event.target.value);
  }
  return (
    <input type="text" id="name" onChange={handleChange} />
  )
}
```

We zouden **foutief** kunnen veronderstellen dat we dit probleem kunnen oplossen door een variabele te maken die de ingetypte tekst opslaat. Een verstaanbare poging is deze:

```typescript
// DEZE CODE IS FOUT
const InputView = () => {
  let name = "";
  const handleChange: ChangeEventHandler<HTMLInputElement> = (event) => {
    name = event.target.value;
  }
  return (
    <>
      <input type="text" id="name" onChange={handleChange} />
      <p>
        The name you typed is {name}
      </p>
    </>
  )
}
```

Wijzigingen in het input veld hebben **geen effect** op de rest van de pagina. Elke render (en er vindt er niet eens een plaats wanneer de handler de waarde wijzigt) runt de code voor de component opnieuw. Er wordt dus telkens een nieuwe variabele `name` met beginwaarde `""` aangemaakt.

In plaats van een gewone variabele is een state variabele nodig. **Bij een wijziging hiervan wordt de component hertekend en de waarde wordt bijgehouden over uitvoeringen heen.** Deze variabele kan aangemaakt worden door middel van de `useState` hook.

```typescript
const [name, setName] = useState<string>('');
```

De `useState` functie heeft als argument een initiële state. Dit is de start waarde die de state zal krijgen als de component voor de eerste keer gerenderd wordt. De functie geeft een array terug met twee elementen in: het eerste element is de huidige state en het tweede element is een functie waarmee je de state kan **en moet** aanpassen. We geven aan welk type onze state zal bevatten door `<string>` mee te geven aan de useState functie.

Het voorbeeld kan dus herschreven worden als volgt:

```typescript
const InputView = () => {
  const [name, setName] = useState<string>('');

  const handleChange: ChangeEventHandler<HTMLInputElement> = (event) => {
    setName(event.target.value);
  }

  return (
    <>
      <input type="text" id="name" onChange={handleChange} />
      <p>
        The name you typed is {name}
      </p>
    </>
  )
}
```

Een wijziging in het invoerveld leidt er nu toe dat `setName` wordt opgeroepen. Dit past de variabele aan en zorgt dat de component opnieuw rendert.

{% hint style="info" %}
Als je graag wil weten wanneer je component wordt gerenderd kan je in de code van de `InputView` component een console.log() statement zetten die afprint dat het component is gerenderd. Zo kan je in je browser developer tools zien op welk moment het component terug opnieuw gerenderd wordt.
{% endhint %}

Ten slotte zorgen we er voor dat ook andere code de state kan wijzigen en dat dit weerspiegeld zal worden in het inputveld:

```typescript
const InputView = () => {
  const [name, setName] = useState<string>('');

  const handleChange: ChangeEventHandler<HTMLInputElement> = (event) => {
    setName(event.target.value);
  }

  return (
    <>
      <input type="text" id="name" onChange={handleChange} value={name} />
      <p>
        The name you typed is {name}
      </p>
    </>
  )
}
```

Het `value` attribuut wordt ingesteld op de huidige waarde van de state. Zo zorgen we ervoor dat het inputveld altijd up-to-date is met de huidige waarde van de state. Dit noemen ze in react **controlled components.** Op deze manier kunnen andere manieren voorzien worden om het inputveld te wijzigen, zoals een knop om de input leeg te maken, een handler die ingetypte tekst automatisch omzet naar hoofdletters, enzovoort.
