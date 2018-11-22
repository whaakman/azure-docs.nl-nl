---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: ed0c387f9785336fbf18b3fd3c0cd9a7b09df633
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279574"
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

Geheimen, door beheerde Opslagaccountsleutels en vault-transacties:
| Type transacties | Maximum aantal transacties dat is toegestaan in 10 seconden kluis per regio<sup>1</sup> |
| --- | --- |
| Alle transacties |2000 |
|

Zie [Azure Key Vault-beperkingsrichtlijnen](../articles/key-vault/key-vault-ovw-throttling.md) voor meer informatie over het verwerken van beperkingen wanneer deze limieten worden overschreden.

<sup>1</sup> er is een brede, door het abonnement limiet voor alle transactietypen die 5 x per sleutelkluis limiet is. Bijvoorbeeld, HSM - andere transacties per abonnement zijn beperkt tot 5000 transacties over 10 seconden per abonnement.
