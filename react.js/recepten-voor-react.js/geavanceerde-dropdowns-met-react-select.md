---
description: React Select voorziet dropdown elementen van handige extra functionaliteit.
---

# geavanceerde dropdowns met React Select

Een dropdown, ofwel een `select` component in TSX, biedt de gebruiker de mogelijkheid om één of meerdere keuzes te maken uit een lijst van opties. Onderstaande CodePen toont dit aan:

TODO: omzetten naar CodePen

```
function App() {

  const [selectedValue, setSelectedValue] = useState<string>("o1");

  return (
    <div className="App">
      <select
      name="exampleselect"
      id="exampleselect"
      value={selectedValue}
      onChange={e => setSelectedValue(e.target.value)}>
        <option value="o1">optie 1</option>
        <option value="o2">optie 2</option>
      </select>
      <p>De geselecteerde waarde is {selectedValue}.</p>
    </div>
  );
}
```

Deze component laat een lijst met alle opties zien wanneer je er op klikt. Hij werkt dus vooral goed wanneer de gebruiker de opties niet uit het hoofd kent en wanneer er niet _te_ veel opties zijn. Als de gebruiker de opties uit het hoofd kent, is het vaak sneller om ze uit te typen, maar het gedrag van de select in de browser is dan niet erg intuïtief. Als er erg veel opties zijn, is er geen snelle manier om te filteren.

[React Select](https://react-select.com/home) combineert de functionaliteit van een select met een input voor tekst om deze problemen op te lossen. Deze component kan alles wat de gewone `select` kan, maar kan onder andere ook:

* getoonde waarden op een intuïtieve manier filteren aan de hand van ingetypte tekst
* opties inladen vanaf een externe databron, zodat ze niet in de code moeten staan en zodat de gebruiker het inladen ook niet zelf hoeft te programmeren
* de gebruiker de mogelijkheid geven om een eigen antwoord in te vullen als de getoonde opties niet volstaan

Bovendien biedt deze component veel layoutopties, inclusief animaties. Wij zullen hier niet alle mogelijkheden verkennen, maar we zullen bekijken hoe we deze component kunnen gebruiken om een combinatie van een `select` en een `input` van een teksttype te verkrijgen voor een betere gebruikerservaring.

Een veel voorkomende situatie waarin een combinatie van een dropdown en een text input goed past, is een component die de gebruiker toestaat een woonplaats te selecteren. Er is een eindige lijst mogelijkheden, maar deze is wel erg lang. Bovendien worden niet altijd dezelfde namen voor gemeentes gehanteerd. Zo kan in het ene systeem "Heverlee" staan terwijl in een ander systeem "Leuven" staat. Het kan ook zijn dat namen van steden vertaald zijn of dat de naam wat anders geschreven wordt dan verwacht.

In wat volgt, zullen we een component maken die dit probleem oplost. Het wordt sterk aangeraden dat je de stappen mee volgt. Onderaan de pagina vind je een afgewerkte versie van het project.

TODO: caveat onvolledige imports,...

installatie

Start eerst een nieuw React.js project op en noem het `gemeentes`. Om de component te gebruiken, moet je hem eerst installeren: `npm i react-select@5.4.0`. Omdat ondersteuning voor TypeScript ingebouwd is, hoef je geen extra types te installeren.

een component maken

Maak een bestand, src/components/CitySelect.tsx en definieer daarin volgende component:

```
export function CitySelect() {
    return <p>Hier komt de select.</p>
}
```

Pas nu je App-component aan zodat de TSX-expressie alleen dit bevat:

```
<div className="App">
      <CitySelect />
    </div>
```

Start je applicatie en controleer dat de nieuwe component alvast zichtbaar is. Normaal zie je de juiste tekst verschijnen.

Maak nu, boven de functie voor de CitySelect component, een variabele met daarin de mogelijke steden. Deze lijst komt uit een publieke bron en is niet volledig, maar lang genoeg om een idee te geven:

```
const belgianCities: string[] = [
    "Antwerp",
    "Ghent",
    "Charleroi",
    "Liege",
    "Brussels",
    "Bruges",
    "Schaarbeek",
    "Anderlecht",
    "Namur",
    "Leuven",
    "Mons",
    "Sint-Jans-Molenbeek",
    "Mechelen",
    "Elsene",
    "Aalst",
    "Ukkel",
    "La Louviere",
    "Hasselt",
    "Kortrijk",
    "Sint-Niklaas",
    "Oostende",
    "Tournai",
    "Genk",
    "Seraing",
    "Roeselare",
    "Mouscron",
    "Verviers",
    "Vorst",
    "Jette",
    "Beveren",
    "Etterbeek",
    "Dendermonde",
    "Beringen",
    "Turnhout",
    "Vilvoorde",
    "Heist-op-den-Berg",
    "Dilbeek",
    "Lokeren",
    "Sint-Truiden"
]
```

Nu kunnen we deze opties aan de gebruiker tonen door een instantie van de React Select component te maken, waarbij we voor de property `options` een lijst meegeven. Deze lijst bevat objecten met properties `value` en `label`. Om de code duidelijk te houden, voorzien we hiervoor een interface. We declareren deze ergens buiten de functie voor de component zelf:

```
interface CitySelectOption {
    value: string;
    label: string;
}
```

We willen dat de volledige naam getoond wordt aan de gebruiker, maar we noteren de achterliggende waarde in kleine letters en beperken ze tot 10 karakters. Dat is niet echt nodig, maar het zal de werking van de component verderop beter demonstreren. Declareer de lijst met opties daarom als volgt voor de functie voor de eigen component:

```
const selectOptions: CitySelectOption[] = belgianCities.map(s => {
    return {value: s.toLocaleLowerCase().substring(0,10), label: s}
});
```

Vul nu de functie voor de eigen component in als volgt:

```
export function CitySelect() {
    return (
        <Select options={selectOptions} />
    )
}
```

Test je applicatie. Normaal heb je nu de keuze uit de gemeentes. Je kan ook een stuk tekst intypen dat voorkomt **in het label, ongeacht gebruik van accenten en hoofdletters** om waarden te filteren. Dat wordt bijvoorbeeld duidelijk als je "eek" intypt. Je krijgt onder andere "Sint-Jans Molenbeek" te zien. De value bevat dit stuk tekst niet, maar het label bevat het wel.

Om te demonstreren wat er gebeurt wanneer we een item uit de lijst kiezen, voegen we een simpele onChange handler toe aan de Select component:

```
<Select
        options={selectOptions}
        onChange={e => console.log(e)}/>
```

Als we in de developer console kijken, merken we dat `e` een `CitySelectOption` is wanneer we iets selecteren. Als we dan ook rekening houden met het feit dat een ontbrekende keuze wordt voorgesteld door `null`, kunnen we de keuze van de gebruiker bijhouden als state. Voeg volgende code toe aan je component: `const [selectedCity, setSelectedCity] = useState<CitySelectOption|null>(null);`

Voeg ook een `onChange` handler toe die de geselecteerde stad wijzigt. Als je nu je applicatie herlaadt, start je met een lege dropdown, maar kan je wel kiezen voor een van de steden. Om alles uit te testen, vervang je de TSX-expressie door een fragment met daarin de `Select` component `en` een paragraaf die de naam van de geselecteerde stad toont:

```
<>
        <Select
        options={selectOptions}
        onChange={e => setSelectedCity(e)}/>
        { selectedCity ?
        <p>Je selecteerde {selectedCity.label}</p> :
        <p>Geen stad geselecteerd.</p> }
        </>
```

{% file src="../../.gitbook/assets/gemeentes.zip" %}
