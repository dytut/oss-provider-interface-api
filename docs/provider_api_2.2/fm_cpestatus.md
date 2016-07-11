# Fault Management: CPE Status API

CPE Status API används för felsökning av tjänster. Det erbjuder aktuell information of kunds CPE.

### Exempel

Anrop:

```http
GET /api/2.2/accesses/STTA0001/cpe HTTP/1.1
```

Svar när ingen cpe hittas/ej nåbar:
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
	"cpe": "DOWN"
}
```

Svar vid aktiv CPE:
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
	"cpe": "UP",
	"since": "2014-08-14T14:09:23Z",
	"ports": [
		{
			"description": "e1",
			"link" : "UP",
			"mbps" : 100,
			"clients": [
				{
					"mac": "DE:AD:BE:EF:00:01",
					"ip": "10.10.1.181"
				}
			]
		},
		{
			"description": "e2",
			"link" : "DOWN",
			"mbps" : 0,
			"clients" : []
		}
	]
}
```


## Fältbeskrivningar

* `null` eller `""` är inte ett giltiga värden för något fält.

<table>
    <tbody>
        <tr>
            <td><strong>Fält</strong></td>
            <td><strong>Förklaring</strong></td>
        </tr>
		<tr>
            <td>
                <code>cpe</code>
            </td>
            <td>
				Anger status på cpe.<br>
				Giltiga värden är "UP" och "DOWN".
            </td>
        </tr>
        <tr>
            <td>
                <code>since</code>
            </td>
            <td>
				Anger tidpunkt då CPE:n gick up och sedan när den haft länk
				Formatet är "YYYY-MM-DDThh:mm:ssZ" enligt ISO-8601.
				Avslutande "Z" innebär att tiden alltid är i UTC.
            </td>
        </tr>
        <tr>
        	<td>
                <code>ports</code>
            </td>
            <td>
            	Lista på de portar som finns på CPE:n
            </td>
        </tr>
        <tr>
      		<td>
                <code>ports/description</code>
            </td>
            <td>
            	CPE:ns beskrivining av porten. Kan vara nummer, eller beskrivning (fa1, e2, port1). Textfält.
            </td>
        </tr>
        <tr>
      		<td>
                <code>ports/link</code>
            </td>
            <td>
            	Anger status på länken.
				Giltiga värden är "UP" och "DOWN".
            </td>
        </tr>
        <tr>
      		<td>
                <code>ports/mbps</code>
            </td>
            <td>
        		Anger länkens faktiskta länkhastighet.
				Enheten är mbps, giltiga värden är: 10, 100, 1000.
            </td>
        </tr>
        <tr>
      		<td>
                <code>ports/clients</code>
            </td>
            <td>
        		Lista på anslutna enheter till cpe-porten
            </td>
        </tr>
        <tr>
      		<td>
                <code>ports/clients/mac</code>
            </td>
            <td>
	        	MAC-adress på enheten.
				MAC-adressen representeras genom 6 par av hexadecimala siffror konkatenerade med kolon ':'. Den skall utelstutande vara i uppercase.
				MAC-adressen är alltid exakt 17 tecken lång.
				Exempel: "DE:AD:BE:EF:00:01".
            </td>
        </tr>
                <tr>
      		<td>
                <code>ports/clients/ip</code>
            </td>
            <td>
	        	Tilldelad IPv4-adress på enheten.
				IP-adress representeras som fyra decimala tal mellan 0 och 255 konkatenerade med punkter'.'.
				Noden skall enbart finnas när enheten har en ipv4-adress.
				Exempel: "10.101.1.181".
            </td>
        </tr>
        
        
     </tbody>
</table>
