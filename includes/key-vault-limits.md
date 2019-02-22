---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: a8979edf94c0dd0271293feb28c18530faeba09c
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56660009"
---
## <a name="key-transactions-max-transactions-allowed-in-10-seconds-per-vault-per-regionsup1sup"></a>Sleutel transacties (maximum aantal transacties dat is toegestaan in 10 seconden kluis per regio<sup>1</sup>):

|Type sleutel|HSM-sleutel<br>Sleutel maken|HSM-sleutel<br>Alle andere transacties|Software-key<br>Sleutel maken|Software-key<br>Alle andere transacties|
|:---|---:|---:|---:|---:|
|RSA 2048-bits|5|1000|10|2000|
|RSA 3072-bit|5|250|10|500|
|RSA 4096-bit|5|125|10|250|
|ECC P-256|5|1000|10|2000|
|ECC P-384|5|1000|10|2000|
|ECC P-521|5|1000|10|2000|
|ECC SECP256K1|5|1000|10|2000|

> [!NOTE]
> In de bovenstaande tabel ziet u dat voor RSA 2048-bits-software-sleutels, we 2000 GET transacties per 10 seconden toestaan en dat voor RSA 2048-bits-HSM-sleutels, we 1000 GET transacties per 10 seconden toestaan.
>
> Houd er rekening mee dat de drempelwaarden voor regeling worden gewogen en afdwingen op hun som is. Bijvoorbeeld in de bovenstaande tabel kunt zien we dat bij het uitvoeren van GET-bewerkingen op RSA HSM-sleutels, het is 8 keer duurder 4096-bits sleutels in vergelijking met 2048-bits sleutels te gebruiken (sinds 1000/125 = 8). Dus in een bepaald interval van 10 seconden, een AKV-client kan doen exact één van de volgende zonder een `429` beperking van HTTP-statuscode:
> - 2000 RSA 2048-bits sleutel software GET transacties, **of**
> - 1000 RSA 2048-bits-HSM-sleutel ophalen transacties **of**
> - RSA 4096-bits HSM-sleutel GET-transacties, 125, **of**
> - 124 RSA 4096-bits HSM-sleutel ophalen-transacties en transacties van 8 RSA 2048-bits HSM-sleutel ophalen.

## <a name="secrets-managed-storage-account-keys-and-vault-transactions"></a>Geheimen, door beheerde Opslagaccountsleutels en vault-transacties:
| Type transacties | Maximum aantal transacties dat is toegestaan in 10 seconden kluis per regio<sup>1</sup> |
| --- | --- |
| Alle transacties |2000 |

Zie [Azure Key Vault-beperkingsrichtlijnen](../articles/key-vault/key-vault-ovw-throttling.md) voor meer informatie over het verwerken van beperkingen wanneer deze limieten worden overschreden.

<sup>1</sup> er is een brede, door het abonnement limiet voor alle transactietypen die 5 x per sleutelkluis limiet is. Bijvoorbeeld, HSM - andere transacties per abonnement zijn beperkt tot 5000 transacties over 10 seconden per abonnement.
