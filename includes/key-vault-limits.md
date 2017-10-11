Transacties sleutel (maximum aantal transacties dat is toegestaan in 10 seconden kluis per regio<sup>1</sup>):

|sleuteltype|HSM-sleutel<br>Sleutel maken|HSM-sleutel<br>Alle andere transacties|Software-sleutel<br>Sleutel maken|Software-sleutel<br>Alle andere transacties|
|:---|---:|---:|---:|---:|
|RSA 2048-bits|5|1000|10|2000|
|RSA 3072-bits|5|250|10|500|
|RSA 4096 bits|5|125|10|250|
|

Geheimen, Opslagaccountsleutels beheerd en kluis transacties:
| Type transacties | Maximum aantal transacties dat is toegestaan in 10 seconden kluis per regio<sup>1</sup> |
| --- | --- |
| Alle transacties |2000 |
|

<sup>1</sup> er is een abonnement wide limiet voor alle transactietypen die 5 x per sleutelkluis limiet is. Bijvoorbeeld HSM - andere transacties per abonnement zijn beperkt tot 5000 transacties in 10 seconden per abonnement.
