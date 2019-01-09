---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 9a39abf77a7396302f93e5a423271402b7c3edb3
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54084001"
---
Sleutel transacties (maximum aantal transacties dat is toegestaan in 10 seconden kluis per regio<sup>1</sup>):

|Type sleutel|HSM-sleutel<br>Sleutel maken|HSM-sleutel<br>Alle andere transacties|Software-key<br>Sleutel maken|Software-key<br>Alle andere transacties|
|:---|---:|---:|---:|---:|
|RSA 2048-bits|5|1000|10|2000|
|RSA 3072-bit|5|250|10|500|
|RSA 4096-bits|5|125|10|250|
|ECC-P-256|5|1000|10|2000|
|ECC-P-384|5|1000|10|2000|
|ECC-P-521|5|1000|10|2000|
|ECC-SECP256K1|5|1000|10|2000|
|

> [!NOTE]
> De bovenstaande drempelwaarden worden gewogen en afdwingen op hun som is. U kunt doen 125 RSA-HSM - 4k-bewerkingen en 0 RSA-HSM - 2k, of 124 RSA-HSM - 4k en 16 RSA-HSM - 2 kB. Daarna wordt in het hetzelfde interval op 10 seconden, een andere bewerking zorgt ervoor dat een uitzondering van de client AKV.

> [!NOTE]
> Als u de onderstaande tabel bekijkt, ziet u dat voor software-back-sleutels is het mogelijk toe 2000 transacties per 10 seconden en voor HSM back sleutels is mogelijk om 1000 transacties per 10 seconden toe. De verhouding van software-back-transacties voor sleutels van 3072 2048 sleutels is 500/2000 of 0.4. Dit betekent dat als een klant kiest, wordt er 500 3072 sleutel transacties over 10 seconden, ze hun maximale limiet is bereikt en andere sleutelbewerkingen niet doen. 
   
|Type sleutel  | Softwaresleutel |HSM-sleutel  |
|---------|---------|---------|
|RSA 2048-bits     |    2000     |   1000    |
|RSA 3072-bit     |     500    |    250     |
|RSA 4096-bits     |    125     |    250     |
|ECC-P-256     |    2000     |  1000     |
|ECC-P-384     |    2000     |  1000     |
|ECC-P-521     |    2000     |  1000     |
|ECC-SECP256K1     |    2000     |  1000     |


Geheimen, door beheerde Opslagaccountsleutels en vault-transacties:
| Type transacties | Maximum aantal transacties dat is toegestaan in 10 seconden kluis per regio<sup>1</sup> |
| --- | --- |
| Alle transacties |2000 |
|

Zie [Azure Key Vault-beperkingsrichtlijnen](../articles/key-vault/key-vault-ovw-throttling.md) voor meer informatie over het verwerken van beperkingen wanneer deze limieten worden overschreden.

<sup>1</sup> er is een brede, door het abonnement limiet voor alle transactietypen die 5 x per sleutelkluis limiet is. Bijvoorbeeld, HSM - andere transacties per abonnement zijn beperkt tot 5000 transacties over 10 seconden per abonnement.
