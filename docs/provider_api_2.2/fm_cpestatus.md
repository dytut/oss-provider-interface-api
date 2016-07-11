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
