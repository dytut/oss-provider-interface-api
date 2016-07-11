# Search API

Search API är en sökfunktion för addresser och accesser. Till skillnad mot Feasability är sökningen enbart aktuell data och är till för att användas av tjänsteleverantörer som t.ex. ej vill cacha addressdata på sin sida, eller för att snabbt kunna söka efter ändringar hos KO utan att behöve omsynkronisera 100000+ resultat.

### Exempel

Request:
```http
POST /api/2.2/search/ HTTP/1.1
Content-Type: application/json

{
  "accessId": "STTA0001",
  "streetName": "Testvägen",
  "streetNumber": "100",
  "streetLittera": "",
  "postalCode": "10000",
  "city": "Ankeborg",
  "countryCode": "SE",
  "premisesType": "MDU_APARTMENT",
  "mduApartmentNumber": "1001",
  "mduDistinguisher": "12121212",
  "outlet": "A-11-14",
  "population": "Hemsöhem",
  "cadastral": "Hemsö 123",
}
```

Response:
Se [Feasability] (feasability.md)

## Fältbeskrivningar
* `null` är inte ett giltigt värde för något fält.
* Fält markerade med _obligatoriskt_ får inte vara tomma strängen (`""`)

<table>
    <tbody>
        <tr>
            <td><strong>Fält</strong></td>
            <td><strong>Förklaring</strong></td>
            <td><strong>Sökval</strong></td>
        </tr>
        <tr>
            <td>
                <code>accessId</code>
            </td>
            <td>
                Ett, per kommunikationsoperatör, unikt ID på en access.<br>Vanligtvis förväntas all kommunikation om en avlämningspunkt ske med samma AccessId. Får enbart bestå av tecknen a-z, A-Z, 0-9, '-' och '.'. <em>text, obligatoriskt, max 32 tecken, [a-zA-Z0-9-.]+</em>
            </td>
            <td>
            	Sökval: <em>Fritextsökning via * i början och slut. En sökning på aab123 matchar enbart aab123, *b12* matchar aab123 och bbb127</em>
            </td>
        </tr>
        <tr>
            <td>
                <code>streetName</code>
            </td>
            <td>
                Gatunamn.<br> I fallet "Kungsgatan 10G" är StreetName "Kungsgatan". <em>text, obligatoriskt</em>
            </td>
        </tr>
        <tr>
            <td>
                <code>streetNumber</code>
            </td>
            <td>
                Gatunummer.<br> I fallet "Kungsgatan 10G" är StreetNumber "10". I fallet "Lantvägen 550-70" är StreetNumber "550". Enbart siffror. <em>text</em>
            </td>
        </tr>
        <tr>
            <td>
                <code>streetLittera</code>
            </td>
            <td>
                Gatubokstav/Uppgång.<br> I fallet "Kungsgatan 10G" är StreetLittera "G". I fallet "Lantvägen 550-70" är StreetLittera "70". <em>text</em>
            </td>
        </tr>
        <tr>
            <td>
                <code>postalCode</code>
            </td>
            <td>
                Postnummer. Exempelvis "41369". Min 10000, max 99999. Alltid fem siffror. <em>text, obligatoriskt</em>
            </td>
        </tr>
        <tr>
            <td>
                <code>city</code>
            </td>
            <td>
                Postort. Exempelvis "Göteborg". <em>text, obligatoriskt</em>
            </td>
        </tr>
        <tr>
            <td>
                <code>countryCode</code>
            </td>
            <td>
                Landskod. Följer ISO 3166-1 för landskoder. Exempel: "SE" för Sverige. <em>text, ISO 3166-1, obligatoriskt</em>
            </td>
        </tr>
        <tr>
            <td>
                <code>premisesType</code>
            </td>
            <td>
                PremisesType beskriver avlämningspunktens lokal. Vid MDU_APARTMENT och MDU_COMMON måste MduDistinguisher eller MduApartmentNumber vara populerade.<em>obligatoriskt</em>
<dl>
<dt>MDU_APARTMENT</dt><dd>Lägenhet i flerbostadshus. Delad fastighetsbeteckning.</dd>
<dt>MDU_COMMON</dt><dd>Gemensamt utrymme i flerbostadshus. Delad fastighetsbeteckning.</dd>
<dt>RESIDENTIAL_HOUSE</dt><dd>Bostad som har egen fastighetsbeteckning.</dd>
<dt>COMMERCIAL</dt><dd>Lokal, men utan tillträde från allmänheten. Till exempel ett kontor.</dd>
<dt>PUBLIC</dt><dd>Inrättning dit allmänheten har tillträde. Till exempel en restaurang eller ett gym.</dd>
<dt>UNKNOWN</dt><dd>Okänd.</dd>
</dl>
            </td>
        </tr>
        <tr>
            <td>
                <code>mduApartmentNumber</code>
            </td>
            <td>
								Lägenhetsnummer enligt Lantmäteriet. Används för att tillsammans med en adress identifiera en unik access. I fallet när kund vill beställa tjänster kan de inte aktiveras hos KO utan att Com Hem har fastställt vilket AccessID kunden har. Genom att unikt identifiera lägenheten med mduApartmentNumber eller mduDistinguisher kan Com Hem fastställa exakt vilken access som skall aktiveras. <em>text, 4 digits, obligatoriskt<sup>1</sup></em><br>
								<br>
								Exempel: 1101, 0901, 1201, 1213.<br>
								<br>
                [1] En av <code>mduApartmentNumber</code>, <code>mduDistinguisher</code> måste finnas om <code>premisesType</code> är <code>"MDU_APARTMENT"</code>.
            </td>
        </tr>
        <tr>
            <td>
                <code>mduDistinguisher</code>
            </td>
            <td>
                Alternativ lokalbeteckning som identifierar lägenheten unikt per adress. Behöver inte följa Lantmäteriets format. <em>text, obligatoriskt<sup>2</sup></em><br>
								<br>
								Exempel: 28, 65113, 1234-1919.<br>
								<br>
	              [2] En av <code>mduApartmentNumber</code>, <code>mduDistinguisher</code> måste finnas om <code>premisesType</code> är <code>"MDU_APARTMENT"</code>.
            </td>
        </tr>
        <tr>
            <td>
                <code>outlet</code>
            </td>
            <td>
                Uttagsnummer som identifierar porten i lägenhet/villa. Typiskt är porten hos slutkund märkt med uttagsnummer. Outlet behöver vara unikt per adress. <em>text</em>
            </td>
        </tr>
        <tr>
            <td>
                <code>population</code>
            </td>
            <td>
                Anger delbestånd i hela beståndet. Hela beståndet hämtas alltid in, men det kan filtreras och göras säljbart i olika etapper. <em>text</em>
            </td>
        </tr>
        <tr>
            <td>
                <code>cadastral</code>
            </td>
            <td>
                Anger fastighetsbeteckning. <em>text</em>
            </td>
        </tr>
    </tbody>
</table>
