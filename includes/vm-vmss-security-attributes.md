---
author: msmbaldwin
ms.service: virtual-machines
ms.topic: include
ms.date: 07/10/2019
ms.author: mbaldwin
ms.openlocfilehash: df11493fa9663d3fcbf0a2f74a5acbead55a25fb
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800274"
---
## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest (zoals versleuteling op de server, server-side-versleuteling met de klant beheerde sleutels en andere versleutelingsfuncties) | Ja | Zie [versleutelen van een virtuele Linux-machine in Azure](/azure/virtual-machines/linux/encrypt-disks.md) en [virtuele schijven op een Windows-VM versleutelen](/azure/virtual-machines/windows/encrypt-disks.md). |
| Versleuteling tijdens overdracht (zoals ExpressRoute-codering in VNet-versleuteling en versleuteling van de VNet-VNet)| Ja | Azure virtuele Machines ondersteunt [ExpressRoute](/azure/expressroute) en VNET-versleuteling. Zie [codering in virtuele machines In transit](/azure/security/security-azure-encryption-overview.md#in-transit-encryption-in-vms). |
| Versleuteling verwerking (CMK, BYOK, enz.)| Ja | Door de klant beheerde sleutels is een ondersteunde Azure-codering scenario; Zie [overzicht van Azure gegevensversleuteling](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms.md).|
| Versleuteling op kolom (Azure Data Services)| N/A | |
| API-aanroepen die zijn versleuteld| Ja | Via HTTPS en SSL. |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja | |
| Ondersteuning voor VNet-injectie| Ja | . |
| Ondersteuning voor netwerkisolatie en Firewalling| Ja |  |
| Geforceerde tunneling ondersteuning| Ja | Zie [geforceerde tunneling met het implementatiemodel Azure Resource Manager configureren](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm.md). |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Zie [controleren en bijwerken van een virtuele Linux-machine in Azure](/azure/virtual-machines/linux/tutorial-monitoring.md) en [controleren en bijwerken van een Windows-machine in Azure](/azure/virtual-machines/windows/tutorial-monitoring.md). |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja |  |
| Authorization| Ja |  |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja |  |
| Gegevens vlak logboekregistratie en controle | Nee |  |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja |  | 
