---
description: >-
  Single-page applicaties zijn een recentere ontwikkeling dan applicaties die
  uit meerdere pagina's bestaan.
---

# single page applications

In deze cursus gaan we voornamelijk kijken naar het meest gebruikte Single Page Applications framework namelijk **React.js** (ontwikkeld door Meta). Er zijn nog vele andere SPA-frameworks zoals Angular (door Google) en Vue.js. Deze frameworks laten het toe om eenvoudig uitgebreidere applicaties te maken dan met gewone JavaScript en algemene frameworks zoals jQuery.

![Het logo van React](../.gitbook/assets/react-image.png)

In het verleden werden pagina's nog volledig opgebouwd op de server zelf. Zo had je bijvoorbeeld PHP of node.js in combinatie met Express. In deze traditionele aanpak bezocht de gebruiker een URL om een pagina in te laden en werd het opbouwen van de pagina voornamelijk op de server zelf gedaan. Voor elke pagina die de gebruiker wou bezoeken, was dus ook een nieuwe GET request nodig. JavaScript werd toen voornamelijk gebruikt voor simpele animaties en interacties of geavanceerde styling die niet met CSS alleen te doen was. Hiervoor was de JavaScript library jQuery uiterst geschikt.

![In de traditionele levenscyclus van een webapplicatie vraagt de client voortdurend nieuwe pagina's. In de SPA lifecycle vraagt de client voortdurend data voor pagina's waarover hij al beschikt.](../.gitbook/assets/SPA-lifecycle-image.png)

Bij single page applicaties verloopt de interactie tussen client en server heel anders. De focus wordt verlegd naar de client. De gebruiker bezoekt maar 1 URL en krijgt maar 1 HTML-bestand terug met bijbehorende JavaScript files. Nadat deze allemaal ingeladen zijn wordt de eerste "applicatiepagina" getoond aan de gebruiker. Deze toont hetzelfde soort info dat in de traditionele aanpak op één HTML-pagina zou staan, maar ze stemt niet meer overeen met een HTML-bestand. De naam **single page** application wijst er dus op dat er maar één HTML-bestand is. Hij betekent niet dat je website niet opgedeeld kan zijn in applicatiepagina's die je via links bereikt.

{% hint style="info" %}
Merk op dat wij hier een onderscheid maken tussen "HTML-pagina's" en "applicatiepagina's". Dat wordt niet overal gedaan, maar normaal is duidelijk uit de context wat bedoeld wordt.
{% endhint %}

Veranderlijke data wordt niet meteen gedownload. Deze worden volledig via **asynchrone API calls** (ook "**AJAX calls**" genoemd) vanuit de client opgevraagd. Als je bijvoorbeeld een lijst van producten zou willen tonen vanuit je SPA, zal de paginastructuur meteen verschijnen, maar zullen de producten zelf iets later inladen. Dit komt omdat de client een API call moet doen om te vragen _welke_ producten er precies zijn.
