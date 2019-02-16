---
author: msmbaldwin
ms.service: service-fabric
ms.topic: include
ms.date: 01/31/2019
ms.author: mbaldwin
ms.openlocfilehash: 179d87a0c1af587148f1b5ffa2cad8085ef0886f
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56306873"
---
## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja | De klant is eigenaar van het cluster en de schaalset voor virtuele machine (VM) het cluster is gebouwd op. Azure disk encryption kan worden ingeschakeld op de VM-schaalset. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>Vnet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja |  |
| Versleuteling sleutel verwerken (CMK, BYOK, enz.)| Ja | De klant is eigenaar van het cluster en de schaalset voor virtuele machine (VM) het cluster is gebouwd op. Azure disk encryption kan worden ingeschakeld op de VM-schaalset. |
| Versleuteling op kolom (Azure-gegevensservices)| N/A |  |
| API-aanroepen die zijn versleuteld| Ja | Service Fabric-API-aanroepen worden gemaakt via Azure Resource Manager. Een geldig JSON webtoken (JWT) is vereist. |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja |  |
| ondersteuning voor vNET-injectie| Ja |  |
| Netwerkisolatie / netwerkfunctie ondersteuning| Ja | Met behulp van netwerken netwerkbeveiligingsgroepen (nsg's). |
| Ondersteuning voor geforceerde tunneling | Ja | Azure-netwerk bieden geforceerde tunneling. |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enzovoort)| Ja | Met behulp van Azure-ondersteuning en ondersteuning voor externe bewaking. |

## <a name="iam-support"></a>IAM-ondersteuning

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Toegangsbeheer - verificatie| Ja | Verificatie is via Azure Active Directory. |
| Toegangsbeheer - autorisatie| Ja | Identiteits- en toegangsbeheer management (IAM) voor aanroepen via SFRP. Aanroepen rechtstreeks naar het eindpunt cluster ondersteunt twee rollen: Gebruiker en beheerder. De klant kan de API's worden toegewezen aan een van beide rollen. |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Beheer/beheer plannen logboekregistratie en controle| Ja | Alle bewerkingen voor de controlelaag uitvoeren door processen voor controle en -goedkeuringen. |
| Gegevens vlak logboekregistratie en controle| N/A | De klant is eigenaar van het cluster.  |

## <a name="configuration-management"></a>Configuration Management

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie enzovoort)| Ja | Configuratie van de service is samengesteld en geïmplementeerd met behulp van Azure implementeren. De code (toepassings- en runtime) is samengesteld met behulp van Azure bouwen.
 |
