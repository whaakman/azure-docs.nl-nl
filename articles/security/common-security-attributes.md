---
title: Beveiligings kenmerken voor Azure-Services
description: Een controle lijst met algemene beveiligings kenmerken voor het evalueren van Azure Service Fabric
services: security
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: security
ms.topic: conceptual
ms.date: 07/11/2019
ms.author: mbaldwin
ms.openlocfilehash: d45e28175412b574432adb59cf700568c9a7fb39
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68304256"
---
# <a name="security-attributes-for-azure-services"></a>Beveiligings kenmerken voor Azure-Services

In dit artikel worden de algemene beveiligings kenmerken voor geselecteerde Azure-Services verzameld. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="api-managementapi-managementapi-management-security-attributesmd"></a>[API Management](../api-management/api-management-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | Gevoelige gegevens zoals certificaten, sleutels en geheime naam waarden worden versleuteld met Service-beheerde exemplaren per service-exemplaar. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling) | Ja | [Express route](../expressroute/index.yml) en VNet-versleuteling worden door [Azure-netwerken](../virtual-network/index.yml)verschaft. |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Nee | Alle versleutelings sleutels zijn per service-exemplaar en worden beheerd door de service. |
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | Aanroepen van beheer vlak worden gedaan via [Azure Resource Manager](../azure-resource-manager/index.yml) via TLS. Er is een geldig JSON-webtoken (JWT) vereist.  Aanroepen voor gegevens vlak kunnen worden beveiligd met TLS en een van de ondersteunde verificatie mechanismen (bijvoorbeeld client certificaat of JWT).
 |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Nee | |
| Ondersteuning voor VNet-injectie| Ja | |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | Respectievelijk netwerk beveiligings groepen (NSG) en Azure-toepassing gateway (of ander software apparaat) gebruiken. |
| Ondersteuning voor geforceerde tunneling| Ja | Azure-netwerken bieden geforceerde tunneling. |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | |
| Authorization| Ja | |


### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | [Azure Monitor activiteiten logboeken](../azure-monitor/platform/activity-logs-overview.md) |
| Logboek registratie en controle van het gegevens vlak| Ja | [Azure monitor Diagnostische logboeken](../azure-monitor/platform/diagnostic-logs-overview.md) en (optioneel) [Azure-toepassing Insights](../azure-monitor/app/app-insights-overview.md).  |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | De [Azure API Management DevOps Resource Kit](https://aka.ms/apimdevops) gebruiken |

### <a name="vulnerability-scans-false-positives"></a>Beveiligings probleem scant onjuiste positieven

In deze sectie worden veelvoorkomende beveiligings problemen beschreven, die geen invloed hebben op Azure API Management.

| Dit               | Description                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ticketbleed (CVE-2016-9244) | Ticketbleed is een beveiligings probleem in de implementatie van de TLS SessionTicket-extensie die is gevonden in sommige F5-producten. Hiermee kan het lekken (' verbloeden ') van Maxi maal 31 bytes aan gegevens van niet-geïnitialiseerd geheugen. Dit wordt veroorzaakt door de TLS-stack opvulling van een sessie-ID, door gegeven van de client, met gegevens om deze 32 bits lang te maken. |


## <a name="app-serviceapp-serviceapp-service-security-attributesmd"></a>[App Service](../app-service/app-service-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | Ja | Inhoud van website bestand wordt opgeslagen in Azure Storage, die de inhoud automatisch versleutelt. Zie [Azure Storage versleuteling voor Data-at-rest](../storage/common/storage-service-encryption.md).<br><br>Door de klant verstrekte geheimen worden op rest versleuteld. De geheimen worden op rest versleuteld terwijl ze zijn opgeslagen in App Service configuratie databases.<br><br>Lokaal gekoppelde schijven kunnen eventueel worden gebruikt als tijdelijke opslag door websites (D:\Local en% TMP%). Lokaal gekoppelde schijven zijn niet versleuteld op rest. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling) | Ja | Klanten kunnen websites zo configureren dat ze HTTPS vereisen en gebruiken voor inkomend verkeer. Zie het blog bericht [hoe u een Azure app service alleen https maakt](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/). |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Ja | Klanten kunnen ervoor kiezen om toepassings geheimen op te slaan in Key Vault en deze op te halen tijdens runtime. Zie [Key Vault verwijzingen gebruiken voor app service en Azure functions (preview)](../app-service/app-service-key-vault-references.md).|
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | Beheer aanroepen om App Service te configureren, worden uitgevoerd via [Azure Resource Manager](../azure-resource-manager/index.yml) -aanroepen via https. |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja | Momenteel beschikbaar als Preview voor App Service. Zie [Azure app service toegangs beperkingen](../app-service/app-service-ip-restrictions.md). |
| Ondersteuning voor VNet-injectie| Ja | App Service omgevingen zijn persoonlijke implementaties van App Service toegewezen aan één klant die is geïnjecteerd in het virtuele netwerk van een klant. Zie [Inleiding tot de app service omgevingen](../app-service/environment/intro.md). |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | Voor de open bare multi tenant-variatie van App Service kunnen klanten netwerk-Acl's (IP-beperkingen) configureren om toegestaan binnenkomend verkeer te vergren delen.  Zie [Azure app service toegangs beperkingen](../app-service/app-service-ip-restrictions.md).  App Service omgevingen worden rechtstreeks geïmplementeerd in virtuele netwerken en kunnen daarom worden beveiligd met Nsg's. |
| Ondersteuning voor geforceerde tunneling| Ja | App Service omgevingen kunnen worden geïmplementeerd in het virtuele netwerk van een klant waar geforceerde tunneling is geconfigureerd. Klanten moeten de instructies volgen in [uw app service Environment configureren met geforceerde tunneling](../app-service/environment/forced-tunnel-support.md). |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | App Service integreert met Application Insights voor talen die Application Insights ondersteunen (volledige .NET Framework, .NET core, Java en node. JS).  Zie [Azure app service prestaties bewaken](../azure-monitor/app/azure-web-apps.md). Met App Service worden metrische gegevens van toepassingen ook naar Azure Monitor verzonden. Zie [apps bewaken in azure app service](../app-service/web-sites-monitor.md). |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Klanten kunnen toepassingen bouwen op App Service die automatisch worden geïntegreerd met [Azure Active Directory (Azure AD)](../active-directory/index.yml) , evenals andere OAuth-compatibele id-providers. Zie [verificatie en autorisatie in azure app service](../app-service/overview-authentication-authorization.md). Voor beheer toegang tot App Service assets wordt alle toegang bepaald door een combi natie van door Azure AD geverifieerde Principal en Azure Resource Manager RBAC-rollen. |
| Authorization| Ja | Voor beheer toegang tot App Service assets wordt alle toegang bepaald door een combi natie van door Azure AD geverifieerde Principal en Azure Resource Manager RBAC-rollen.  |


### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Alle beheer bewerkingen die op App Service objecten worden uitgevoerd, vindt plaats via [Azure Resource Manager](../azure-resource-manager/index.yml). Historische logboeken van deze bewerkingen zijn beschikbaar in de portal en via de CLI. Zie [Azure Resource Manager van de resource provider bewerkingen](../role-based-access-control/resource-provider-operations.md#microsoftweb) en [AZ monitor Activity-Log](/cli/azure/monitor/activity-log). |
| Logboek registratie en controle van het gegevens vlak | Nee | Het gegevens vlak voor App Service is een externe bestands share met de inhoud van de geïmplementeerde website van de klant.  De externe bestands share kan niet worden gecontroleerd. |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | Voor beheer bewerkingen kan de status van een App Service configuratie worden geëxporteerd als een Azure Resource Manager sjabloon en de versie in de loop van de tijd. Voor runtime bewerkingen kunnen klanten meerdere verschillende live versies van een toepassing onderhouden met behulp van de functie App Service implementatie sleuven. | 



## <a name="azure-resource-managerazure-resource-managerazure-resource-manager-security-attributesmd"></a>[Azure Resource Manager](../azure-resource-manager/azure-resource-manager-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | Ja |  |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling) | Ja | HTTPS/TLS. |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| N/A | Azure Resource Manager slaat geen klant inhoud op, alleen gegevens beheren. |
| Versleuteling op kolom niveau (Azure Data Services)| Ja | |
| Versleutelde API-aanroepen| Ja | |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Nee | |
| Ondersteuning voor VNet-injectie| Ja | |
| Ondersteuning voor netwerk isolatie en firewalling| Nee |  |
| Ondersteuning voor geforceerde tunneling| Nee |  |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Nee | |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | [Azure Active Directory](/azure/active-directory) gebaseerd.|
| Authorization| Ja | |


### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Met activiteiten logboeken worden alle schrijf bewerkingen (PUT, POST, DELETE) die zijn uitgevoerd op uw resources beschikbaar gesteld. Zie [activiteiten logboeken weer geven om acties op resources te controleren](../azure-resource-manager/resource-group-audit.md). |
| Logboek registratie en controle van het gegevens vlak| N/A | |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja |  |


## <a name="azure-backupbackupbackup-security-attributesmd"></a>[Azure Backup](../backup/backup-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies)| Ja | Storage-service versleuteling gebruiken voor opslag accounts. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling) | Nee | HTTPS gebruiken. |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Nee |  |
| Versleuteling op kolom niveau (Azure Data Services)| Nee |  |
| Versleutelde API-aanroepen| Ja |  |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Nee |  |
| Ondersteuning voor VNet-injectie| Nee |  |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | Geforceerde tunneling wordt ondersteund voor VM-back-ups. Geforceerde tunneling wordt niet ondersteund voor werk belastingen die in Vm's worden uitgevoerd. |
| Ondersteuning voor geforceerde tunneling| Nee |  |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Log Analytics wordt ondersteund via Diagnostische logboeken. Zie [Azure backup beveiligde werk belastingen bewaken met behulp van log Analytics](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) voor meer informatie. |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Verificatie is via Azure Active Directory. |
| Authorization| Ja | Door de klant gemaakte en ingebouwde RBAC-rollen worden gebruikt. Zie [Access Control op basis van rollen gebruiken om Azure backup herstel punten te beheren](/azure/backup/backup-rbac-rs-vault) voor meer informatie. |


### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Alle door de klant geactiveerde acties van de Azure Portal worden vastgelegd in activiteiten Logboeken. |
| Logboek registratie en controle van het gegevens vlak| Nee | Azure Backup gegevens vlak kan niet rechtstreeks worden bereikt.  |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja|  |

## <a name="cosmos-dbcosmos-dbcosmos-db-security-attributesmd"></a>[Cosmos DB](../cosmos-db/cosmos-db-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | Ja | Alle Cosmos DB data bases en back-ups worden standaard versleuteld. Zie [gegevens versleuteling in azure Cosmos DB](../cosmos-db/database-encryption-at-rest.md). Versleuteling aan de server zijde met door de klant beheerde sleutels wordt niet ondersteund. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in Vnet-versleuteling en VNet-VNet-versleuteling)| Ja | Alle Azure Cosmos DB gegevens worden tijdens de overdracht versleuteld. |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Nee |  |
| Versleuteling op kolom niveau (Azure Data Services)| Ja | Alleen in de Table API Premium. Niet alle Api's ondersteunen deze functie. Zie [Inleiding tot Azure Cosmos DB: Table-API](../cosmos-db/table-introduction.md). |
| Versleutelde API-aanroepen| Ja | Alle verbindingen met Azure Cosmos DB ondersteuning voor HTTPS. Azure Cosmos DB ondersteunt ook TLS 1,2-verbindingen, maar dit is nog niet afgedwongen. Als klanten minder TLS op hun eind niveau uitschakelen, kunnen ze ervoor zorgen dat ze verbinding maken met Cosmos DB.  |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja |  |
| ondersteuning voor vNET-injectie| Ja | Met het VNet-service-eind punt kunt u een Azure Cosmos DB-account configureren om alleen toegang toe te staan vanuit een specifiek subnet van een virtueel netwerk (VNet). U kunt ook VNet-toegang combi neren met firewall regels.  Zie [toegang tot Azure Cosmos DB van virtuele netwerken](../cosmos-db/vnet-service-endpoint.md). |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | Met firewall ondersteuning kunt u uw Azure Cosmos-account zodanig configureren dat toegang alleen is toegestaan vanuit een goedgekeurde set van IP-adressen, een bereik van IP-adressen en/of Cloud Services. Zie [IP-Firewall configureren in azure Cosmos DB](../cosmos-db/how-to-configure-firewall.md).|
| Ondersteuning voor geforceerde tunneling | Ja | Kan worden geconfigureerd aan de client zijde op het VNET waar de virtuele machines zich bevinden.   |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Alle aanvragen die naar Azure Cosmos DB worden verzonden, worden geregistreerd. [Azure-bewaking](../azure-monitor/overview.md), metrische gegevens van Azure, Azure audit logging wordt ondersteund.  U kunt informatie vastleggen die overeenkomt met gegevens vlak aanvragen, runtime statistieken voor query's, query tekst, MongoDB aanvragen. U kunt ook waarschuwingen instellen. |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Ja op het account niveau van de data base. op het niveau van het gegevensvlak Cosmos DB maakt gebruik van bron tokens en toegang tot sleutels. |
| Authorization| Ja | Ondersteund in het Azure Cosmos-account met hoofd sleutels (primair en secundair) en bron tokens. U kunt alleen lees-en schrijf toegang krijgen tot gegevens met hoofd sleutels. Met bron tokens kunt u beperkte tijd toegang tot resources, zoals documenten en containers, toestaan. |

### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle van beheer plannen| Ja | Azure-activiteiten logboek voor bewerkingen op account niveau, zoals firewalls, VNets, sleutels toegang en IAM. |
| Logboek registratie en controle van het gegevens vlak | Ja | Diagnostische gegevens voor controle van de logboek registratie voor bewerkingen op container niveau, zoals het maken van een container, het inrichten van de door Voer, het indexeren van beleid en ruwe bewerkingen op documenten. |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Nee  | | 

### <a name="additional-security-attributes-for-cosmos-db"></a>Aanvullende beveiligings kenmerken voor Cosmos DB

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Cross Origin Resource Sharing (CORS) | Ja | Zie [Configure cross-Origin Resource Sharing (CORS)](../cosmos-db/how-to-configure-cross-origin-resource-sharing.md). |


## <a name="event-hubsevent-hubsevent-hubs-security-attributesmd"></a>[Event Hubs](../event-hubs/event-hubs-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) |  Ja | |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling) | Ja | |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Nee |  |
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja |  |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja |  |
| ondersteuning voor vNET-injectie| Nee | |
| Ondersteuning voor netwerk isolatie en firewalling| Ja |  |
| Ondersteuning voor geforceerde tunneling| Nee |  |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | |
| Authorization|  Ja | |


### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja |  |
| Logboek registratie en controle van het gegevens vlak| Ja |   |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | |


## <a name="expressrouteexpressrouteexpressroute-security-attributesmd"></a>[ExpressRoute](../expressroute/expressroute-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) |  N/A | ExpressRoute slaat geen klant gegevens op. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling) | Nee | |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| N/A |  |
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml) en HTTPS. |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| N/A |  |
| ondersteuning voor vNET-injectie| N/A | |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | Elke klant is opgenomen in een eigen routerings domein en is getunneld naar het eigen VNet |
| Ondersteuning voor geforceerde tunneling| N/A | Via Border Gateway Protocol (BGP). |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Zie [ExpressRoute bewaking, metrische gegevens en waarschuwingen](../expressroute/expressroute-monitoring-metrics-alerts.md).|

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Service account voor de gateway voor micro soft (GWM) (controller); Just-in-time-toegang voor dev en OP. |
| Authorization|  Ja |Service account voor de gateway voor micro soft (GWM) (controller); Just-in-time-toegang voor dev en OP. |


### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen| 
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja |  |
| Logboek registratie en controle van het gegevens vlak| Nee |   |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | Via de netwerk resource provider (NRP). |


## <a name="key-vaultkey-vaultkey-vault-security-attributesmd"></a>[Key Vault](../key-vault/key-vault-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | Ja | Alle objecten zijn versleuteld. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling) | Ja | Alle communicatie via versleutelde API-aanroepen |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Ja | De klant beheert alle sleutels in hun Key Vault. Wanneer er ondersteunde sleutels voor de Hardware Security module (HSM) worden opgegeven, wordt de sleutel, het certificaat of het geheim beschermd door een HSM met FIPS Level 2. |
| Versleuteling op kolom niveau (Azure Data Services)| N/A |  |
| Versleutelde API-aanroepen| Ja | HTTPS gebruiken. |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja | Het gebruik van de service-eind punten van Virtual Network (VNet). |
| Ondersteuning voor VNet-injectie| Nee |  |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | Met VNet-firewall regels. |
| Ondersteuning voor geforceerde tunneling| Nee |  |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Log Analytics gebruiken. |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Verificatie is via Azure Active Directory. |
| Authorization| Ja | Key Vault toegangs beleid gebruiken. |

### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle van beheer/beheer vlak| Ja | Log Analytics gebruiken. |
| Logboek registratie en controle van het gegevens vlak| Ja | Log Analytics gebruiken. |

### <a name="access-controls"></a>Toegangs beheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Toegangs beheer voor het besturings element/beheer vlak | Ja | Toegangsbeheer op basis van rollen (RBAC) in Azure Resource Manager |
| Toegangs beheer voor gegevens vlak (op elk service niveau) | Ja | Toegangs beleid Key Vault |

## <a name="load-balancerload-balancerload-balancer-security-attributesmd"></a>[Load balancer](../load-balancer/load-balancer-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | N/A | |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| N/A | |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| N/A | |
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | Via de [Azure Resource Manager](../azure-resource-manager/index.yml). |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| N/A | |
| Ondersteuning voor VNet-injectie| N/A | . |
| Ondersteuning voor netwerk isolatie en firewalling| N/A |  |
| Ondersteuning voor geforceerde tunneling| N/A | |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Zie [Azure monitor-logboeken voor open bare basis Load Balancer](../load-balancer/load-balancer-monitor-log.md). |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| N/A |  |
| Authorization| N/A |  |

### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Zie [Azure monitor-logboeken voor open bare basis Load Balancer](../load-balancer/load-balancer-monitor-log.md). |
| Logboek registratie en controle van het gegevens vlak | N/A |  |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| N/A |  | 

## <a name="service-bus-messagingservice-bus-messagingservice-bus-messaging-security-attributesmd"></a>[Service Bus berichten](../service-bus-messaging/service-bus-messaging-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) |  Ja voor versleuteling aan de server zijde is standaard ingesteld op rest. | Door de klant beheerde sleutels en BYOK worden nog niet ondersteund. Versleuteling aan client zijde is de verantwoordelijkheid van de client |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling) | Ja | Ondersteunt het standaard HTTPS/TLS-mechanisme. |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Nee |   |
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | API-aanroepen worden gedaan via [Azure Resource Manager](../azure-resource-manager/index.yml) en HTTPS. |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja (alleen Premium-laag) | VNet-service-eind punten worden alleen ondersteund voor [Service Bus-laag Premium](../service-bus-messaging/service-bus-premium-messaging.md) . |
| Ondersteuning voor VNet-injectie| Nee | |
| Ondersteuning voor netwerk isolatie en firewalling| Ja (alleen Premium-laag) |  |
| Ondersteuning voor geforceerde tunneling| Nee |  |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Ondersteund via [Azure monitor en waarschuwingen](../service-bus-messaging/service-bus-metrics-azure-monitor.md). |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Beheerd via [Azure Active Directory Managed Service Identity](../service-bus-messaging/service-bus-managed-service-identity.md); Zie [Service Bus verificatie en autorisatie](../service-bus-messaging/service-bus-authentication-and-authorization.md).|
| Authorization| Ja | Ondersteunt autorisatie via [RBAC](../service-bus-messaging/service-bus-role-based-access-control.md) (preview) en SAS-token; Zie [Service Bus verificatie en autorisatie](../service-bus-messaging/service-bus-authentication-and-authorization.md). |


### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Er zijn bewerkings logboeken beschikbaar. Zie [Service Bus Diagnostische logboeken](../service-bus-messaging/service-bus-diagnostic-logs.md).  |
| Logboek registratie en controle van het gegevens vlak| Nee |  |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | Ondersteunt versie beheer van de resource provider via de [Azure Resource Manager-API](/rest/api/resources/).|


## <a name="service-bus-relayservice-bus-relayservice-bus-relay-security-attributesmd"></a>[Service Bus Relay](../service-bus-relay/service-bus-relay-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) |  N/A | Relay is een WebSocket en er worden geen gegevens bewaard. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling) | Ja | Voor service is TLS vereist. |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Nee | Maakt gebruik van alleen micro soft TLS-certificaten.  |
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | HTTPS. |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Nee |  |
| Ondersteuning voor netwerk isolatie en firewalling| Nee |  |
| Ondersteuning voor geforceerde tunneling| N/A | Relay is de TLS-tunnel  |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Via SAS. |
| Authorization|  Ja | Via SAS. |

### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml). |
| Logboek registratie en controle van het gegevens vlak| Ja | Geslaagde/mislukte verbindingen en Logboeken.  |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml).|

## <a name="service-fabricservice-fabricservice-fabric-security-attributesmd"></a>[Service Fabric](../service-fabric/service-fabric-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | Ja | De klant is eigenaar van het cluster en de schaalset voor virtuele machines waarop het cluster is gebouwd. Azure Disk Encryption kan worden ingeschakeld voor de schaalset van de virtuele machine. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling) | Ja | De klant is eigenaar van het cluster en de schaalset voor virtuele machines waarop het cluster is gebouwd. Azure Disk Encryption kan worden ingeschakeld voor de schaalset van de virtuele machine. |
| Versleuteling op kolom niveau (Azure Data Services)| N/A |  |
| Versleutelde API-aanroepen| Ja | Service Fabric-API-aanroepen worden gedaan via Azure Resource Manager. Er is een geldig JSON-webtoken (JWT) vereist. |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja |  |
| Ondersteuning voor VNet-injectie| Ja |  |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | Met behulp van netwerk beveiligings groepen (NSG). |
| Ondersteuning voor geforceerde tunneling| Ja | Azure-netwerken bieden geforceerde tunneling. |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Azure-controle ondersteuning en ondersteuning van derden gebruiken. |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Verificatie is via Azure Active Directory. |
| Authorization| Ja | Identiteits-en toegangs beheer (IAM) voor aanroepen via SFRP. Aanroepen rechtstreeks naar het cluster eindpunt ondersteunen twee rollen: Gebruiker en beheerder. De klant kan de Api's toewijzen aan een van beide rollen. |

### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Alle bewerkingen van het besturings vlak worden uitgevoerd via processen voor controle en goed keuringen. |
| Logboek registratie en controle van het gegevens vlak| N/A | De klant is eigenaar van het cluster.  |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | |

## <a name="sql-databasesql-databasesql-database-security-attributesmd"></a>[SQL Database](../sql-database/sql-database-security-attributes.md)

SQL Database bevatten zowel [één data base](../sql-database/sql-database-single-index.yml) als een [beheerd exemplaar](../sql-database/sql-database-managed-instance.md). De volgende vermeldingen zijn van toepassing op beide aanbiedingen, tenzij anders vermeld.

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | Ja | Wordt ' versleuteling-in-gebruik ' genoemd, zoals beschreven in het artikel [Always encrypted](../sql-database/sql-database-always-encrypted.md). Versleuteling aan de server zijde maakt gebruik van [transparante gegevens versleuteling](../sql-database/transparent-data-encryption-azure-sql.md).|
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling) | Ja | HTTPS gebruiken. |
| Versleutelings verwerking, zoals CMK of BYOK| Ja | Door service beheerde en door de klant beheerde sleutel verwerking worden aangeboden. Deze laatste wordt aangeboden via [Azure Key Vault](../key-vault/index.yml). |
| Versleuteling op kolom niveau van Azure Data Services| Ja | Via [Always encrypted](../sql-database/sql-database-always-encrypted.md). |
| Versleutelde API-aanroepen| Ja | HTTPS/SSL gebruiken. |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja | Is alleen van toepassing op [één data base](../sql-database/sql-database-single-index.yml) . |
| Ondersteuning voor Azure Virtual Network injectie| Ja | Is alleen van toepassing op een [beheerd exemplaar](../sql-database/sql-database-managed-instance.md) . |
| Netwerk isolatie en firewall ondersteuning| Ja | Firewall op database niveau en op server niveau. Netwerk isolatie geldt alleen voor [beheerde exemplaren](../sql-database/sql-database-managed-instance.md) . |
| Ondersteuning voor geforceerde tunneling| Ja | [Beheerde instantie](../sql-database/sql-database-managed-instance.md) via een [ExpressRoute](../expressroute/index.yml) -VPN. |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking, zoals Log Analytics of Application Insights| Ja | SecureSphere, de SIEM-oplossing van Imperva, wordt ook ondersteund via de integratie van [Azure Event hubs](../event-hubs/index.yml) via [SQL auditing](../sql-database/sql-database-auditing.md). |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Azure Active Directory (Azure AD) |
| Authorization| Ja | Geen |

### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Ja voor sommige gebeurtenissen alleen |
| Logboek registratie en controle op gegevens vlak | Ja | Via [SQL audit](../sql-database/sql-database-auditing.md) |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer, zoals versie beheer van de configuratie| Nee  | Geen |

### <a name="additional-security-attributes-for-sql-database"></a>Aanvullende beveiligings kenmerken voor SQL Database

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Preventieve: evaluatie van beveiligings problemen | Ja | Zie [SQL-evaluatie service voor beveiligings problemen helpt u bij het identificeren van een database kwets](../sql-database/sql-vulnerability-assessment.md)baarheid. |
| Preventief: gegevens detectie en-classificatie  | Ja | Zie [Azure SQL database en SQL Data Warehouse gegevens detectie & classificatie](../sql-database/sql-database-data-discovery-and-classification.md). |
| Detectie: detectie van bedreigingen | Ja | Zie [Advanced Threat Protection voor Azure SQL database](../sql-database/sql-database-threat-detection-overview.md). |


## <a name="storagestoragecommonstorage-security-attributesmd"></a>[Storage](../storage/common/storage-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | Ja |  |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling) | Ja | Ondersteuning voor standaard HTTPS/TLS-mechanismen.  Gebruikers kunnen ook gegevens versleutelen voordat deze naar de service worden verzonden. |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Ja | Zie [Storage service Encryption door de klant beheerde sleutels gebruiken in azure Key Vault](../storage/common/storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Versleuteling op kolom niveau (Azure Data Services)| N/A |  |
| Versleutelde API-aanroepen| Ja |  |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja |  |
| Ondersteuning voor VNet-injectie| N/A |  |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | |
| Ondersteuning voor geforceerde tunneling| N/A |  |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Azure Monitor metrische gegevens nu beschikbaar, logboeken beginnen met preview |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Azure Active Directory, gedeelde sleutel, gedeeld toegangs token. |
| Authorization| Ja | Ondersteuning voor autorisatie via RBAC, POSIX Acl's en SAS-tokens |

### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer | Ja | Azure Resource Manager activiteiten logboek |
| Logboek registratie en controle van het gegevens vlak| Ja | Diagnostische logboeken voor services en Azure Monitor logboek registratie starten  |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | Ondersteuning van resource provider versie beheer via Azure Resource Manager-Api's |

## <a name="virtual-machines-and-virtual-machine-scale-sets"></a>Virtual Machines en Virtual Machine Scale Sets

[Virtuele Linux](../virtual-machines/windows/virtual-machines-windows-security-attributes.md)-machines[Windows](../virtual-machines/windows/virtual-machines-windows-security-attributes.md) | [VM-schaal sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-security-attributes.md)  | 


### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | Ja | Zie [een virtuele Linux-machine versleutelen in azure](/azure/virtual-machines/linux/encrypt-disks) en [virtuele schijven op een Windows-VM](/azure/virtual-machines/windows/encrypt-disks)versleutelen. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| Ja | Azure Virtual Machines ondersteunt [ExpressRoute](/azure/expressroute) -en VNET-versleuteling. Zie [in-transit versleuteling in vm's](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Ja | Door de klant beheerde sleutels is een ondersteund Azure-versleutelings scenario. Zie [Azure Encryption Overview](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms)(Engelstalig).|
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | Via HTTPS en SSL. |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja | |
| Ondersteuning voor VNet-injectie| Ja | . |
| Ondersteuning voor netwerk isolatie en firewalling| Ja |  |
| Ondersteuning voor geforceerde tunneling| Ja | Zie [geforceerde tunneling configureren met het Azure Resource Manager-implementatie model](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Zie [een virtuele Linux-machine bewaken en bijwerken in azure](/azure/virtual-machines/linux/tutorial-monitoring) en [een virtuele Windows-machine bewaken en bijwerken in azure](/azure/virtual-machines/windows/tutorial-monitoring). |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja |  |
| Authorization| Ja |  |


### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja |  |
| Logboek registratie en controle van het gegevens vlak | Nee |  |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja |  | 


## <a name="vpn-gatewayvpn-gatewayvpn-gateway-security-attributesmd"></a>[VPN Gateway](../vpn-gateway/vpn-gateway-security-attributes.md)

### <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | N/A | Klant gegevens van VPN-gateway-door voer worden niet opgeslagen |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| Ja | VPN-gateway versleutelt klant pakketten tussen Azure VPN-gateways en klant-on-premises VPN-apparaten (S2S) of VPN-clients (P2S). VPN-gateways ondersteunen ook VNet-naar-VNet-versleuteling. |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Nee | Door de klant opgegeven vooraf gedeelde sleutels worden op rest versleuteld. maar is nog niet geïntegreerd met CMK. |
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml) en https  |

### <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| N/A | |
| Ondersteuning voor VNet-injectie| N/A | . |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | VPN-gateways zijn toegewezen VM-exemplaren voor elke klant Virtual Network  |
| Ondersteuning voor geforceerde tunneling| Ja |  |

### <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Zie [Azure monitor Diagnostische logboeken/waarschuwing](../vpn-gateway/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md) & [Azure monitor metrische gegevens/waarschuwing](../vpn-gateway/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).  |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) voor het beheren van de service en het configureren van de Azure VPN-gateway. |
| Authorization| Ja | Ondersteuning voor autorisatie via [RBAC](../role-based-access-control/overview.md). |

### <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Azure Resource Manager activiteiten logboek. |
| Logboek registratie en controle van het gegevens vlak | Ja | [Diagnostische logboeken Azure monitor](../azure-resource-manager/resource-group-audit.md) voor logboek registratie en controle van de VPN-verbinding. |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | Voor beheer bewerkingen kan de status van een configuratie van een Azure VPN-gateway worden geëxporteerd als een Azure Resource Manager sjabloon en in de loop van de tijd. | 

