---
title: Beveiligingskenmerken voor Azure-services
description: Een controlelijst met algemene beveiligingskenmerken voor het evalueren van Azure Service Fabric
services: security
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: security
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: mbaldwin
ms.openlocfilehash: 94681351758f34cc53c425f3207660bdb09f6494
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64717782"
---
# <a name="common-security-attributes-for-azure-services"></a>Algemene beveiligingskenmerken voor Azure-services

Beveiliging is geïntegreerd in elk aspect van een Azure-service. In dit artikel worden de algemene beveiligingskenmerken voor geselecteerde Azure-services verzameld. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]


## <a name="azure-backupbackupbackup-security-attributesmd"></a>[Azure Backup](../backup/backup-security-attributes.md)

### <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja | Met behulp van storage service-versleuteling voor opslagaccounts. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Nee | Met behulp van HTTPS. |
| Versleuteling verwerking (CMK, BYOK, enz.)| Nee |  |
| Versleuteling op kolom (Azure Data Services)| Nee |  |
| API-aanroepen die zijn versleuteld| Ja |  |

### <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Nee |  |
| Ondersteuning voor VNet-injectie| Nee |  |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja | Geforceerde tunnels wordt ondersteund voor VM-back-up. Geforceerde tunneling wordt niet ondersteund voor workloads die worden uitgevoerd in virtuele machines. |
| Geforceerde tunneling ondersteuning| Nee |  |

### <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Log Analytics wordt ondersteund via Logboeken met diagnostische gegevens. Zie de Monitor voor Azure Backup beveiligde werkbelastingen met behulp van Log Analytics (https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) voor meer informatie. |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Verificatie is via Azure Active Directory. |
| Autorisatie| Ja | Klanten die zijn gemaakt en ingebouwde RBAC-rollen worden gebruikt. Zie Use Role-Based Access Control voor het beheren van Azure Backup herstelpunten (/ azure/back-up/back-up-rbac-rs-kluis) voor meer informatie. |


### <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Alle acties van de klant geactiveerd vanuit Azure portal worden geregistreerd in de activiteitenlogboeken. |
| Gegevens vlak logboekregistratie en controle| Nee | Azure Backup-gegevenslaag kan niet rechtstreeks worden bereikt.  |

### <a name="configuration-management"></a>Configuration Management

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja|  |

## <a name="azure-key-vaultkey-vaultkey-vault-security-attributesmd"></a>[Azure Key Vault](../key-vault/key-vault-security-attributes.md)

### <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja | Alle objecten zijn versleuteld. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | Alle communicatie gaat via versleutelde API-aanroepen |
| Versleuteling verwerking (CMK, BYOK, enz.)| Ja | Klant beheert alle sleutels in hun Key Vault. Wanneer een hardware security module (HSM) back-sleutels zijn opgegeven, wordt een FIPS Level 2 HSM beveiligt de sleutel, het certificaat of het geheim. |
| Versleuteling op kolom (Azure Data Services)| N/A |  |
| API-aanroepen die zijn versleuteld| Ja | Met behulp van HTTPS. |

### <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja | Met behulp van service-eindpunten voor Virtueelnetwerk (VNet). |
| Ondersteuning voor VNet-injectie| Nee |  |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja | Met behulp van VNet-firewallregels. |
| Geforceerde tunneling ondersteuning| Nee |  |

### <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Met behulp van Log Analytics. |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Verificatie is via Azure Active Directory. |
| Autorisatie| Ja | Met behulp van toegangsbeleid voor Key Vault. |


### <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Beheer/beheer vlak logboekregistratie en controle| Ja | Met behulp van Log Analytics. |
| Gegevens vlak logboekregistratie en controle| Ja | Met behulp van Log Analytics. |

### <a name="access-controls"></a>Besturingselementen voor toegang

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Beheer/beheer vlak-toegangsbeheer | Ja | Toegangsbeheer op basis van rollen (RBAC) in Azure Resource Manager |
| Gegevens vlak toegangsbeheer (op het serviceniveau van elke) | Ja | Toegangsbeleid voor Key Vault |

## <a name="azure-service-fabricservice-fabricservice-fabric-security-attributesmd"></a>[Azure Service Fabric](../service-fabric/service-fabric-security-attributes.md)

### <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja | De klant is eigenaar van het cluster en de schaalset voor virtuele machine (VM) het cluster is gebouwd op. Azure disk encryption kan worden ingeschakeld op de virtuele-machineschaalset. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja |  |
| Versleuteling verwerking (CMK, BYOK, enz.)| Ja | De klant is eigenaar van het cluster en de schaalset voor virtuele machine (VM) het cluster is gebouwd op. Azure disk encryption kan worden ingeschakeld op de virtuele-machineschaalset. |
| Versleuteling op kolom (Azure Data Services)| N/A |  |
| API-aanroepen die zijn versleuteld| Ja | Service Fabric-API-aanroepen worden gemaakt via Azure Resource Manager. Een geldig JSON webtoken (JWT) is vereist. |

### <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja |  |
| Ondersteuning voor VNet-injectie| Ja |  |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja | Met behulp van netwerken netwerkbeveiligingsgroepen (nsg's). |
| Geforceerde tunneling ondersteuning| Ja | Azure-netwerk bieden geforceerde tunneling. |

### <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Met behulp van Azure-ondersteuning en ondersteuning voor externe bewaking. |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Verificatie is via Azure Active Directory. |
| Autorisatie| Ja | Identiteits- en toegangsbeheer management (IAM) voor aanroepen via SFRP. Aanroepen rechtstreeks naar het eindpunt cluster ondersteunt twee rollen: Gebruiker en beheerder. De klant kan de API's worden toegewezen aan een van beide rollen. |


### <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Alle bewerkingen voor de controlelaag uitvoeren door processen voor controle en -goedkeuringen. |
| Gegevens vlak logboekregistratie en controle| N/A | De klant is eigenaar van het cluster.  |

### <a name="configuration-management"></a>Configuration Management

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Configuratie van de service is samengesteld en geïmplementeerd met behulp van Azure implementeren. De code (toepassings- en runtime) is samengesteld met behulp van Azure bouwen.
 |

## <a name="azure-storage"></a>Azure Storage

### <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja |  |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | Ondersteuning voor standaard HTTPS/TLS-mechanismen.  Gebruikers kunnen ook gegevens versleutelen voordat deze worden verzonden naar de service. |
| Versleuteling verwerking (CMK, BYOK, enz.)| Ja | Zie [Storage Service Encryption door de klant beheerde sleutels in Azure Key Vault](../storage/common/storage-service-encryption-customer-managed-keys.md).|
| Versleuteling op kolom (Azure Data Services)| N/A |  |
| API-aanroepen die zijn versleuteld| Ja |  |

### <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja |  |
| Ondersteuning voor VNet-injectie| N/A |  |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja | |
| Geforceerde tunneling ondersteuning| N/A |  |

### <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Azure Monitor-metrische gegevens beschikbaar zijn Logboeken vanaf preview nu |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Azure Active Directory, gedeelde sleutel, token voor gedeelde toegang. |
| Autorisatie| Ja | Ondersteuning voor autorisatie via RBAC POSIX ACL's en SAS-Tokens |


### <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle | Ja | Azure Resource Manager-activiteitenlogboek |
| Gegevens vlak logboekregistratie en controle| Ja | Diagnostische logboeken en vanaf preview logboekregistratie van Azure Monitor  |

### <a name="configuration-management"></a>Configuration Management

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Ondersteuning voor Resource Provider versiebeheer via Azure Resource Manager-API 's |