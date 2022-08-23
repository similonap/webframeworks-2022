---
description: >-
  React componenten renderen HTML. Door deze te stylen, verbeteren we de
  gebruikerservaring van de applicatie.
---

# Componenten stylen

Er zijn verschillende manieren om stylesheets toe te passen op React applicaties. De twee belangrijkste zijn:

* **CSS-in-CSS** (CSS Modules)
* **CSS-in-JS**

Elke aanpak heeft zijn voor- en nadelen.

### CSS Modules

CSS Modules is de aanpak die het dichtst aanleunt bij traditionele css bestanden zoals deze gebruikt worden in statische HTML-pagina's. Alle elementen krijgen een `class` toegewezen en krijgen een style toegewezen via een CSS bestand. In React gebruiken we niet `class` maar `className`.

In React willen we dat componenten zo veel mogelijk op zichzelf gebruikt kunnen worden. Zo kan één component zonder aanpassingen in verschillende applicaties gebruikt worden. Daarom maken we per component een apart CSS bestand aan. Het is technisch mogelijk om alles in één algemene file `index.css` te plaatsen, maar dan is de CSS-code voor één component gekoppeld aan die van alle andere componenten.

#### rechtstreeks importeren

CSS bestanden toevoegen aan een React component gebeurt door deze te importeren:

```typescript
import './App.css';
```

Dit CSS bestand kan bijvoorbeeld de volgende inhoud hebben:

```css
.container {
    padding: 10px
}
```

We kunnen dan bijvoorbeeld onze `<div>` tags stylen als volgt

```typescript
<div className="container">
      <Header/>
      <List games={games} />
      <InputView onAdd={handleAdd} />
</div>
```

Het nadeel van deze manier is dat we moeten opletten dat al deze CSS bestanden niet dezelfde namen voor klassen gebruiken. Als dit wel gebeurt zullen deze CSS classes met elkaar botsen en zal de ene de andere overschrijven.

#### werken met CSS modules

Om het probleem van herhaalde klassen op te lossen, kunnen we gebruik maken van CSS modules. Het gebruik van modules is zeer gelijkaardig aan dat van gewone CSS bestanden, maar modules zijn geïsoleerd van elkaar. Om gebruik te maken van de CSS Modules moet het CSS bestand de naam krijgen van de component, gevolgd door `.module.css`&#x20;

Als we nu gebruik willen maken van CSS modules in de `App` component, moeten we de CSS file hernoemen naar `App.module.css` en moeten we de manier van importeren aanpassen als volgt:

```typescript
import styles from './App.module.css';
```

In plaats van de `className` property rechstreeks in te vullen, halen we deze uit het `styles` object:

```tsx
  <div className={styles.container}>
    <Header />
    <List games={games} />
    <InputView onAdd={handleAdd} />
  </div>
```

### CSS-in-JS

De manier van styling die we hierboven hebben toegepast is zeer gelijkaardig aan de werkwijze voor statische HTML-pagina's. In React kan je de styles ook rechtstreeks ook in JavaScript definiëren. Het voordeel van deze manier is dat we niet afhankelijk zijn van aparte CSS-bestanden.

De eerste manier om dit te doen is via **inline styles**. Je geeft rechtstreeks de styles mee aan de hand van de `style` property.&#x20;

```typescript
const Header = () => {
  return (
    <h1 style={{fontSize: '22pt', borderBottom: '2px solid black', textTransform: 'uppercase'}}>Welcome to the H2O Game shop</h1>
  );
}
```

Je ziet dat we hier niet de schrijfwijze `font-size`, `border-bottom` of `text-transform` gebruiken, die we wel in een CSS-bestand zouden gebruiken. De regel is hier dat we de camel case varianten gebruiken van CSS properties. Dit komt omdat JavaScript `-` interpreteert als een minteken en niet als geldig onderdeel van een identifier. Zo wordt bv`font-size` dus `fontSize`.

In plaats van het object rechtstreeks mee te geven aan de property kan je hier ook een variabele voor definiëren:

```typescript
import { CSSProperties } from 'react';

const headerStyle : CSSProperties = {
  fontSize: '22pt', borderBottom: '2px solid black', textTransform: 'uppercase'
}

const Header = () => {
  return (
    <h1 style={headerStyle}>Welcome to the H2O Game shop</h1>
  );
}

export default Header;
```

Niet alle dingen die je kan gebruiken in CSS kan je in CSS-in-JS gebruiken. Dingen zoals animaties zijn hierdoor niet eenvoudig te implementeren. Daarom wordt er vaak voor CSS-in-JS gebruik gemaakt van de library `styled-components`.

Je kan deze eenvoudigweg installeren door volgende commando's uit te voeren:

```bash
npm install styled-components
npm install @types/styled-components
```

Deze functionaliteit kan je dan importeren op de volgende manier:

```typescript
import styled from 'styled-components'
```

Zo kunnen we onze header component ook als volgt stylen:

```typescript
import styled from 'styled-components'

// merk op: backticks
const TitleHeader = styled.div`
  font-size: 22pt;
  border-bottom: 2px solid black;
  text-transform: uppercase;
`;

const Header = () => {
  return (
    <TitleHeader>Welcome to the H2O Game shop</TitleHeader>
  );
}

export default Header;
```

Je merkt op dat we hier wel de standaard CSS properties kunnen gebruiken. Door middel van `styled-components` kunnen we zelfs meer geavanceerde functionaliteit gebruiken, zoals animaties:

```typescript
import styled, { keyframes } from 'styled-components'

const fadeIn = keyframes`
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
`;

const TitleHeader = styled.div`
  font-size: 22pt;
  border-bottom: 2px solid black;
  text-transform: uppercase;
  animation: 1s ${fadeIn} ease-out;
`;

const Header = () => {
  return (
    <TitleHeader>Welcome to the H2O Game shop</TitleHeader>
  );
}

export default Header;
```

### UI Frameworks

Er zijn een tal van frameworks die het gebruik van zelf CSS schrijven minimaliseren. Je maakt daar gebruik van herbruikbare componenten die allemaal al voor jou geschreven zijn. De meest bekende zijn:

* [https://material-ui.com/](https://material-ui.com/)
* [https://react-bootstrap.github.io/](https://react-bootstrap.github.io/)
* [https://tailwindcss.com/](https://tailwindcss.com/)

Deze vereenvoudigen het ontwikkelproces drastisch en zorgen ervoor dat er een bepaald design systeem wordt gebruikt. Dit zorgt ervoor dat het ontwikkelen van webapplicaties drastisch kan versneld worden.&#x20;
