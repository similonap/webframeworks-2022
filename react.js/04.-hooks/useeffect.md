---
description: >-
  Componenten zijn hoofdzakelijk bezig met het renderen van de UI. Acties die
  andere wijzigingen hebben kunnen soms ook nuttig zijn.
---

# useEffect

## Inleiding

Componenten moeten vaak bepaalde acties uitvoeren die niet beperkt zijn tot het renderen van HTML. Dergelijke acties heten **side-effects**. Typische side-effects zijn:

* het zetten van een title aan de hand van `document.title`
* het gebruiken van timers met `setInterval` en `setTimeout`
* het meten van de hoogte, breedte of positie van bepaalde elementen in de DOM
* het zetten of lezen van waarden op harde schijf
* het lezen van of schrijven naar externe web services

## Voorbeelden

### Voorbeeld: gebruik maken van local storage

Local storage laat toe bepaalde configuraties of user settings bij te houden in de browser van de gebruiker. Local storage kan bijvoorbeeld gebruikt worden om de inhoud van een veld op te slaan, zodat dit terug kan worden ingevuld wanneer de gebruiker de pagina opnieuw opent.

We kunnen deze waarde alvast uitlezen bij het zetten van de default waarde van onze state. Veronderstel bijvoorbeeld een veld `name` voor de naam van de gebruiker:

```typescript
const [name, setName] = useState<string>(localStorage.getItem('name') ?? '');
```

{% hint style="info" %}
De operator `??` staat toe een defaultwaarde te voorzien. Als de waarde langs de linkerkant `null` of `undefined` is, wordt de waarde langs de rechterkant gebruikt.
{% endhint %}

Door middel van een side-effect kunnen we elke wijziging aan de state variabele `name` opslaan. Hiervoor gebruiken we de `useEffect` hook om het side-effect te triggeren telkens de `name` state wordt aangepast:

```typescript
  useEffect(() => {
    localStorage.setItem('name', name);
  },[name]);
```

De `useEffect` beschikt over twee parameters. De eerste parameter is een functie die het effect uitvoert. In dit geval slaat deze functie de waarde van `name` op in de lokale opslag van de browser. De tweede parameter is een array van **dependencies**. Dit is een lijst van variabelen (state variabelen of props). Enkel wanneer een van deze variabelen aangepast wordt, wordt het side-effect uitgevoerd. In dit voorbeeld zal dus, iedere keer `name` aangepast wordt, de waarde opnieuw opgeslagen worden.&#x20;

### Voorbeeld: dynamisch instellen van de paginatitel

Om de titel van de pagina dynamisch in te stellen, doe je het volgende:

```typescript
useEffect(() => {
  // hier kan ook code staan die een veranderlijke waarde voor de titel bepaalt
  document.title = 'Games Store'
},[]);
```

In dit geval is de dependency array leeg, want de titel wordt enkel ingesteld bij aanmaak. Verdere aanpassingen aan de data van de component wijzigen de titel niet.

## Details van de functieparameter

De functie die het side-effect uitvoert heeft ofwel **géén** returnwaarde ofwel **een andere functie**. Die andere functie is een **cleanup** functie. Ze maakt het side-effect ongedaan. Dit kan bijvoorbeeld zijn om een geactiveerde timer stop te zetten, om een open connectie met een externe dienst af te sluiten, enzovoort.

## Details van de dependency array

Twee belangrijke dingen die je zeker moet weten:

* Als je de array argument weglaat zal het side-effect bij elke render van de component opnieuw worden uitgevoerd.
* Als je een lege array meegeeft zal het side-effect enkel getriggerd worden wanneer de component voor de eerste keer gerenderd wordt. Dit kan handig zijn voor het inlezen van data uit een externe bron.



##
