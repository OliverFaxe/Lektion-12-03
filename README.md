# Lektion-12-03

# Introduktion till API:er, Fetch och JSON


## Vad är JSON?

- **JSON står för JavaScript Object Notation** och är ett lättläst format för att representera data. 

- **Struktur**: JSON består av nyckel-värde-par och är väldigt likt JavaScript men används för att skicka data mellan olika system.

```json
{
    "name": "John Doe",
    "age": 30,
    "email": "john.doe@hotmail.com",
    "skills": ["JS", "HTML", "CSS"]
}
```

**Varför JSON?** 

- Lätt att läsa
- Plattformoberoende: JSON fungerar oavsett vilket programmeringsspråk som används.
- Nästan alla moderna API:er använder JSON för att skicka och ta emot data. 

### Konvertera mellan JSON och JavaScript

Eftersom JS och JSON är så närbesläktat med varandra så förstår JS JSON per default. Alltså, om en JS-app tar emot JSON data så kan den direkt (implicit) konvertera det till giltig javascript. Dock så kan inte andra språket det utan vi måste konvertera där emellan. Så om data ska skicka iväg från en JS-app så måste det göras om till JSON först. Detta görs med två befentliga metoder.

`JSON.stringify( data ) => string`

Denna metoden konverterar JS-data till giltig JSON, alltså till en lång sträng.

`JSON.parse( string ) => JavaScript`

Konverterar JSON tillbaks till giltig JavaScript. (JavaScript gör det per default)


## Vad är ett API?

- Ett **API** är som en brygga mellan olika program som gör att de kan prata med varandra. Ett API döljer komplixiteten i hur ett eller fler system fungerar och erbjuder istället en tydlig "manual / kommunikationsväg" för hur andra system ska interagera med det.

### Vardagsexempel

- **Flygapp**: När du söker efter ett flyg så kommer appen att hämta data från mängder av olika flyg- och resebolag för att försöka matcha med din sökning. Då är det de olika bolagens API:er som flygappen skickar förfrågan till.

- **Väderapp***: Din väderapp hämtar information från olika API:er som i sin tur hämtar data från sensorer och databaser.

### Hur funkar ett API (kortfattat)

1. Tar emot en begäran eller förfrågan efter data. _( request )_ från en klient. 
2. Tolkar och skickar vidare requesten till rätt resurser.
3. Returnerar ett svar, _(response)_ oftast i form av JSON till klienten. Sen kan klienten göra vad den vill med den här data som den får tillbaka.

### Typer av API:er

1.  **REST API, Representation State Transfer**
    - Den vanligaste typen
    - Bygger på HTTP-protokollet och använder sig utav standard metoderna GET _(hämta data från server)_, POST _(skapa ny data på servern)_, PUT _(uppdatera data på servern)_ och DELETE. _(ta bort data från servern)_.

    - **Exempel**
        - `GET / users`: Hämtar en lista av användare
        - `POST / users`: Lägg till en ny användare
        - `PUT / users`: Uppdatera användare
        - `DELETE / users`: Ta bort användare

2. **GraphQL**
    - Alternativ till REST där klienten specificerar exakt vilken data som behövs.
    - **Exempel**: Klienten frågar efter användarnamn och e-post för en specifik användare istället för att få all data.

3. **SOAP (Simple Object Access Protocol)**
    - Ett äldre API-format som använder sig av XML.
    - Mer komplext än REST och används oftast i äldre system.

4. **Lokala API:er** 
    - API:er som används för kommunikation mellan program, till exempel lokalt på datorn, mellan frontend och backend i utvecklingsmiljö. Men även mellan Word och filsystemet på datorn. Alltså sparar ett dokument på datorn.

5. **Öppna och privata API:er**
    - **Öppna API:er**: kräver oftast ingen autentisering, till exempel väderappar som man använder dagligen.
    - **Stängda API:er**: Kräver autentisering och kan användas inom organisationer. Men även publika API:er som vill begränsa användningen på olika sätt.

## Fetch API

Fetch är ett modernt verktyg som är gjort för att kunna göra hämtningar av (_oftast extern_) data som tar lite tid. JS kan ta lite tid och det vill vi motverka med Fetch. Fetch kan inleda hämtningar utan låsa applikationen. 

Fetch är till för att göra nätverksförfrågningar över HTTP och är en modern lösning mot det äldre XML HTTP-request.

### Nedbrytning av Fetch
- **Returnerar ett Promise**: ett löfte om att data kommer att komma så småningom.

- **Promise-baserad asynkroniskt mönster**: Fetch ger oss tillgång till metoder som vi kan använda när vår data är klar eller ett error har skett. `.then()` och `.catch()` används för att reagera på när hämtningen är klar, eller om något gick fel. En av dessa kommer alltid att anropas.

- **Simplifierad syntax**: Syntaxen är enklare att använda än den äldre metoden XLM-HTTP-Request.

- **Flexibilitet med ett options-objekt**: Fetch tar emot flera parametrar, bland annat URL som vi vill skicka förfrågan till men även ett valfritt options-objekt som vi kan använda för att konfiguerar vårt request innan det skickas iväg.

- **Response objekt**: Detta objekt är det som returneras av Fetch och innehåller all information som vi behöver, bland annat statusen på hämtningen och data som vi efterfrågade.

- **Async / Await-kompabilitet**: Fetch fungerar med async/await per default och erbjuder en alternativ syntax till de två metoderna .then och .catch.

Fetch är ett dynamiskt och mångsidigt verktyg som gör det enkelt och effektivt att göra HTTP hämtningar i JavaScript.

Att det är löftest-baserat _(promise-based)_ gör att får kod förenklas och det blir effektivare att skriva asynkronisk kod.

### Syntax Fetch

- Med .then syntax
```js
fetch(url, options?)
    .then((response) => respons.json())
    .then((data) => { console.log(data) })
    .catch((error) => {console.log(error)})
```

Ett klassiskt exempel av en fetch. `fetch()` returnerar ett response om det går igenom. Detta respons fångas upp av den första .then-metoden som i sin tur returnerar själva data som ligger inbäddad i response-objektet. Denna data tar den andra `.then` hand om och loggar ut.

Skulle ett fel ske i processen så kommer det fångas upp i `catch()` där vi kan utföra någon form av logik för att hantera det.

- Med async/await

```js
async function fetchSomething() {
    try {
        const response = await fetch(url, options?);
        const data = await response.json();
        console.log(data);
    } catch (error){
        console.log(error)
    }
} 
```

URL är adressen till datan du efterfrågar medans "options" innehåller inställningar för requesten.