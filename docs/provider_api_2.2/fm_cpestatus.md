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
			"link" : "DOWN"
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
            	Lista på det portar som finns på CPE:n
            </td>
        </tr>
     </tbody>
</table>
