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
