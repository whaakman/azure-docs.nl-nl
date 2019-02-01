---
author: msmbaldwin
ms.service: backup
ms.topic: include
ms.date: 01/31/2019
ms.author: mbaldwin
ms.openlocfilehash: 5a4cc3ea822c5613fc7a50b8e370d6846b683721
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513510"
---
## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja | Met behulp van storage service-versleuteling voor opslagaccounts. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>Vnet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Nee | Met behulp van HTTPS. |
| Versleuteling sleutel verwerken (CMK, BYOK, enz.)| Nee |  |
| Versleuteling op kolom (Azure-gegevensservices)| Nee |  |
| API-aanroepen die zijn versleuteld| Ja |  |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Nee |  |
| ondersteuning voor vNET-injectie| Nee |  |
| Netwerkisolatie / netwerkfunctie ondersteuning| Ja | Geforceerde tunnels wordt ondersteund voor VM-back-up. Geforceerde tunneling wordt niet ondersteund voor workloads die worden uitgevoerd in virtuele machines. |
| Ondersteuning voor geforceerde tunneling | Nee |  |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enzovoort)| Ja | Log Analytics wordt ondersteund via Logboeken met diagnostische gegevens. Zie de Monitor voor Azure Backup beveiligde werkbelastingen met behulp van Log Analytics (https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) voor meer informatie. |

## <a name="iam-support"></a>IAM-ondersteuning

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Toegangsbeheer - verificatie| Ja | Verificatie is via Azure Active Directory. |
| Toegangsbeheer - autorisatie| Ja | Klanten die zijn gemaakt en ingebouwde RBAC-rollen worden gebruikt. Zie Use Role-Based Access Control voor het beheren van Azure Backup-herstelpunten (https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault) voor meer informatie. |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Beheer/beheer plannen logboekregistratie en controle| Ja | Alle acties van de klant geactiveerd vanuit Azure portal worden geregistreerd in de activiteitenlogboeken. |
| Gegevens vlak logboekregistratie en controle| Nee | Azure Backup-gegevenslaag kan niet rechtstreeks worden bereikt.  |

## <a name="configuration-management"></a>Configuration Management

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie enzovoort)| Ja|  |