---
description: >-
  Wanneer een gebruiker een actie onderneemt, is het niet wenselijk dat een
  webapplicatie blokkeert tot de actie is afgerond.
---

# asynchroon opslaan via knop

In een klassieke webpagina zorgt het versturen van een formulier ervoor dat de browser een nieuwe (versie van de) pagina laadt. In een single-page application wordt dit herladen zo veel mogelijk vermeden, want het maakt de gebruikerservaring veel onaangenamer. We kunnen dit oplossen door, bij het verzenden van het formulier, de lokale state aan te passen en de state op de serverkant asynchroon aan te passen.

We gebruiken om dit te demonstreren een kleine voorbeeldapplicatie die de studenten in een klas oplijst en die toestaat nieuwe studenten toe te voegen. Het wordt aangeraden dat je de code zelf mee uitschrijft, maar onderaan deze pagina vind je een volledige implementatie terug:

{% hint style="warning" %}
Tussendoor geven we niet alle imports en zeggen we niet welke packages je moet installeren. Probeer de code zelf aan de praat te krijgen.
{% endhint %}

Om studenten voor te stellen, gebruiken we een interface `Student`. Deze plaatsen we in de `src` folder van een nieuwe React applicatie, in een bestand `datatypes.ts`:

```typescript
export interface Student {
    name: string;
    uuid: string;
}
```

Onze applicatie beheert een lijst van studenten, dus (omdat we voor dit voorbeeld niet met aparte pagina's werken) aan de `App` component voegen we volgende hook toe: `const [students, setStudents] = useState<Student[]>([]);`

We willen dat alle gekende studenten weergegeven worden in een tabel, dus we vervangen de TSX-expressie voor de app door volgende TSX-expressie:

```tsx
    <div className="App">
      <h1>Studenten</h1>
      <h2>Een student toevoegen</h2>
      <p>Dit is voor de volgende stap.</p>
      <h2>Reeds geregistreerde studenten</h2>
      <table>
        <thead>
          <tr>
            <th>Naam</th>
            <th>ID</th>
          </tr>
        </thead>
        <tbody>{students.map(s => {
          return (
          <tr key={s.uuid}>
            <td>{s.name}</td>
            <td>{s.uuid}</td>
          </tr>
          );
        })}</tbody>
      </table>
    </div>
```

Test alvast eens uit of je code tot hiertoe klopt. Je kan dit bijvoorbeeld doen door de initiële state aan te passen zodat er een paar studenten automatisch in de applicatie staan.

Nu zullen we een formulier voorzien om een nieuwe student te kunnen toevoegen. Omdat dit formulier echt een op zichzelf staand onderdeel is, maken we er een nieuwe component voor, die we opslaan onder `src/components/StudentForm.tsx`. Het formulier moet de lijst met studenten kunnen updaten, dus we zorgen dat `students` en `setStudents` meegegeven kunnen worden aan de nieuwe component:

```typescript
export interface StudentFormProps {
    students: Student[];
    setStudents: (students: Student[]) => void;
}

export function StudentForm(props: StudentFormProps) {
    const [studentName, setStudentName] = useState<string>('');
    return (
        <form onSubmit={(event) => {
            event.preventDefault();
            const uuid = uuidv4();
            props.setStudents([...props.students, {name: studentName, uuid: uuid}]);
            setStudentName('');
        }}>
            <label htmlFor="name"></label>
            <input
            type="text"
            id="name"
            name="name"
            value={studentName}
            onChange={(e) => setStudentName(e.target.value)} />
            <input type="submit" value="student opslaan" />
        </form>
    )
}
```

In de TSX-expressie in de `App` component kunnen we nu het `p`-element vervangen door: `<StudentForm students={students} setStudents={setStudents} />`. Dit staat toe de tabel in te vullen, maar de studenten worden nog niet opgeslagen.

Om dit op te lossen, maken we een bestand aan dat we kunnen gebruiken met JSON server. We noemen het `db.json` en we mogen het om het even waar op ons systeem bewaren:

```json
{
    "students": []
}
```

Gebruik dit bestand als "backend" door in de terminal naar de map van `db.json` te navigeren en JSON server als volgt op te starten: `json-server --port 3001 --watch db.json --id uuid`.

Om een nieuwe student op te slaan, doen we een POST request. We wachten niet voor dit request is afgerond, maar we laten de applicatie verder werken in afwachting van een response. Dit is het principe achter een `Promise`, maar we definiëren een `async` functie om dit makkelijker uit te schrijven:

```typescript
async function storeStudent(student: Student) {
    try {
        const response = await fetch("http://localhost:3001/students/",
            {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(student),
                referrer: "about:client"
            });
        if (!response.ok) {
            alert('Wel een antwoord, maar niet het verwachte. De student is niet opgeslagen.');
        }
    }
    catch (error) {
        alert("Er is misschien iets misgelopen bij het opslaan van de student.")
    }
}
```

We zorgen dat deze code uitvoert door de `onSubmit` handler aan te passen met volgende nieuwe code (waarbij de oude call naar `setStudents` vervangen mag worden):

```typescript
const newStudent = {name: studentName, uuid: uuid};
/* deze instructie werkt asynchroon
 * dat houdt in dat props.setStudents al zal uitvoeren terwijl het POST request nog onderweg is */
storeStudent(newStudent);
props.setStudents([...props.students, newStudent]);
```

Om na te gaan dat alles werkt, moet je enkele studenten toevoegen en met een text editor `db.json` openen. Je zou dezelfde studenten moeten zien die je hebt toegevoegd.

Het React project (zonder `db.json`) vind je hieronder. Denk eraan eerst `npm install` uit te voeren:

{% file src="../../.gitbook/assets/asynchroon-opslaan.zip" %}

{% hint style="info" %}
Nu zien we in de applicatie toch nog niet dat de data wordt opgeslagen? Dat klopt. Probeer als oefening zelf [de data ook in te laden bij de start van je applicatie](data-inladen-bij-start-van-een-applicatie.md). Dit wordt in de grote projecten voor de frontend module wel gedaan.
{% endhint %}
