---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 0e55c372c6f5dc3484bd64cf4f328479d2d0b245
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553403"
---
## <a name="key-transactions-maximum-transactions-allowed-in-10-seconds-per-vault-per-regionsup1sup"></a>Sleutel transacties (maximum aantal transacties die zijn toegestaan in 10 seconden kluis per regio<sup>1</sup>):

|Type sleutel|HSM-sleutel<br>Sleutel maken|HSM-sleutel<br>Alle andere transacties|Softwaresleutel<br>Sleutel maken|Softwaresleutel<br>Alle andere transacties|
|:---|---:|---:|---:|---:|
|RSA 2048-bits|5|1000|10|2,000|
|RSA 3.072-bits|5|250|10|500|
|RSA 4.096-bits|5|125|10|250|
|ECC P-256|5|1000|10|2,000|
|ECC P-384|5|1000|10|2,000|
|ECC P-521|5|1000|10|2,000|
|ECC SECP256K1|5|1000|10|2,000|

> [!NOTE]
> In de vorige tabel ziet u dat voor RSA 2048-bits softwaresleutels, 2000 GET transacties per 10 seconden zijn toegestaan. Voor RSA 2048-bits HSM-sleutels, zijn 1000 GET transacties per 10 seconden toegestaan.
>
> De drempelwaarden voor regeling worden gewogen en afdwingen op hun som is. Bijvoorbeeld, zoals weergegeven in de vorige tabel is wanneer u GET-bewerkingen op RSA HSM-sleutels, het acht keer duurder 4.096-bits sleutels in vergelijking met 2048-bits sleutels te gebruiken. Dat komt doordat 1.000/125 = 8.
>
> In een bepaald interval van 10 seconden, een Azure Key Vault-client kunt doen *slechts één* van de volgende bewerkingen voordat er een `429` beperking van HTTP-statuscode:
> - 2000 RSA 2048-bits softwaresleutel GET-transacties
> - 1.000 transacties van RSA 2048-bits HSM-sleutel ophalen
> - Transacties van 125 4.096 RSA-bits HSM-sleutel ophalen
> - 124 4.096 RSA-bits-HSM-sleutel GET-transacties en transacties van 8 RSA 2048-bits HSM-sleutel ophalen

## <a name="secrets-managed-storage-account-keys-and-vault-transactions"></a>Geheimen, door beheerde opslagaccountsleutels en vault-transacties:
| Type transacties | Maximum aantal transacties die zijn toegestaan in 10 seconden kluis per regio<sup>1</sup> |
| --- | --- |
| Alle transacties |2,000 |

Zie voor meer informatie over het verwerken van beperkingen wanneer deze limieten worden overschreden [Azure Key Vault-beperkingsrichtlijnen](../articles/key-vault/key-vault-ovw-throttling.md).

<sup>1</sup> een limiet voor brede, door het abonnement voor alle transactietypen is vijf keer per sleutelkluis limiet. HSM-andere transacties per abonnement zijn bijvoorbeeld beperkt tot 5000 transacties over 10 seconden per abonnement.
