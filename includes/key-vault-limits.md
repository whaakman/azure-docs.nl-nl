Transacties sleutel (maximum aantal transacties dat is toegestaan in 10 seconden kluis per regio<sup>1</sup>):

|Sleuteltype|HSM-sleutel<br>Sleutel maken|HSM-sleutel<br>Alle andere transacties|Software-key<br>Sleutel maken|Software-key<br>Alle andere transacties|
|:---|---:|---:|---:|---:|
|RSA 2048-bit|5|1000|10|2000|
|RSA 3072-bit|5|250|10|500|
|RSA 4096-bit|5|125|10|250|
|ECC P-256|5|1000|10|2000|
|ECC P-384|5|1000|10|2000|
|ECC P-521|5|1000|10|2000|
|ECC SECP256K1|5|1000|10|2000|
|

Geheimen, Opslagaccountsleutels beheerd en kluis transacties:
| Type transacties | Maximum aantal transacties dat is toegestaan in 10 seconden kluis per regio<sup>1</sup> |
| --- | --- |
| Alle transacties |2000 |
|

Zie [Azure Key Vault richtlijnen beperking](../articles/key-vault/key-vault-ovw-throttling.md) voor informatie over het afhandelen van beperking wanneer deze limiet is overschreden.

<sup>1</sup> er is een abonnement wide limiet voor alle transactietypen die 5 x per sleutelkluis limiet is. Bijvoorbeeld HSM - andere transacties per abonnement zijn beperkt tot 5000 transacties in 10 seconden per abonnement.
