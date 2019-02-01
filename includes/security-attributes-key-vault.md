---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 01/31/2019
ms.author: mbaldwin
ms.openlocfilehash: 7ac0072b10489deb1b18ea737301ab698dce9f89
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513503"
---
## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja | Alle objecten zijn versleuteld. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>Vnet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | Alle communicatie gaat via versleutelde API-aanroepen |
| Versleuteling sleutel verwerken (CMK, BYOK, enz.)| Ja | Klant beheert alle sleutels in hun Key Vault. Wanneer een hardware security module (HSM) ondersteund sleutels zijn specifiecd, wordt een FIPS Level 2 HSM beveiligt de sleutel, het certificaat of het geheim. |
| Versleuteling op kolom (Azure-gegevensservices)| N/A |  |
| API-aanroepen die zijn versleuteld| Ja | Met behulp van HTTPS. |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja | Met behulp van service-eindpunten voor Virtueelnetwerk (Vnet). |
| ondersteuning voor vNET-injectie| Nee |  |
| Netwerkisolatie / netwerkfunctie ondersteuning| Ja | Met behulp van Vnet-firewallregels. |
| Ondersteuning voor geforceerde tunneling | Nee |  |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enzovoort)| Ja | Met behulp van Log Analytics. |

## <a name="iam-support"></a>IAM-ondersteuning

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Toegangsbeheer - verificatie| Ja | Verificatie is via Azure Active Directory. |
| Toegangsbeheer - autorisatie| Ja | Met behulp van toegangsbeleid voor Key Vault. |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Beheer/beheer plannen logboekregistratie en controle| Ja | Met behulp van Log Analytics. |
| Gegevens vlak logboekregistratie en controle| Ja | Met behulp van Log Analytics. |

## <a name="access-controls"></a>Besturingselementen voor toegang

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Beheer/beheer vlak-toegangsbeheer | Ja | Toegangsbeheer op basis van rollen (RBAC) in Azure Resource Manager |
| Gegevens vlak toegangsbeheer (op het serviceniveau van elke) | Ja | Toegangsbeleid voor Key Vault |