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
ms.openlocfilehash: 460d8756c437a1212aef054cf069be2bccac8c8a
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66001367"
---
# <a name="security-attributes-for-azure-services"></a>Beveiligingskenmerken voor Azure-services

In dit artikel worden de algemene beveiligingskenmerken voor geselecteerde Azure-services verzameld. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="azure-api-managementapi-managementapi-management-security-attributesmd"></a>[Azure API Management](../api-management/api-management-security-attributes.md)

### <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja (alleen voor versleuteling van de service-aan de serverzijde) | Gevoelige gegevens, zoals certificaten, sleutels en waarden voor de naam van geheim zijn versleuteld met service-beheerd, per service-exemplaar sleutels. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | [Express Route](../expressroute/index.yml) en VNet-versleuteling geleverd door [Azure-netwerken](../virtual-network/index.yml). |
| Versleuteling verwerking (CMK, BYOK, enz.)| Nee | Alle versleutelingssleutels zijn per service-exemplaar en service die wordt beheerd. |
| Versleuteling op kolom (Azure Data Services)| N/A | |
| API-aanroepen die zijn versleuteld| Ja | Vlak beheeraanroepen worden aangeleverd via [Azure Resource Manager](../azure-resource-manager/index.yml) via TLS. Een geldig JSON webtoken (JWT) is vereist.  Gegevens vlak aanroepen kunnen worden beveiligd met TLS en een van de ondersteunde verificatiemechanismen (bijvoorbeeld clientcertificaat of JWT).
 |

### <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Nee | |
| Ondersteuning voor VNet-injectie| Ja | |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja | Met behulp van netwerken netwerkbeveiligingsgroepen (nsg's) en Azure Application Gateway (of andere software-apparaat) respectievelijk. |
| Geforceerde tunneling ondersteuning| Ja | Azure-netwerk bieden geforceerde tunneling. |

### <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | |
| Autorisatie| Ja | |


### <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | [Activiteitenlogboeken van Azure Monitor](../azure-monitor/platform/activity-logs-overview.md) |
| Gegevens vlak logboekregistratie en controle| Ja | [Diagnostische logboeken in Azure Monitor](../azure-monitor/platform/diagnostic-logs-overview.md) en (optioneel) [Azure Application Insights](../azure-monitor/app/app-insights-overview.md).  |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Met behulp van de [Azure API Management DevOps resourcekit](https://aka.ms/apimdevops) |


## <a name="azure-app-serviceapp-serviceapp-service-security-attributesmd"></a>[Azure App Service](../app-service/app-service-security-attributes.md)

### <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest (zoals versleuteling op de server, server-side-versleuteling met de klant beheerde sleutels en andere versleutelingsfuncties) | Ja | Website-inhoud wordt opgeslagen in Azure Storage, codeert automatisch de inhoud in rust. Zie [Azure Storage-versleuteling van data-at-rest](../storage/common/storage-service-encryption.md).<br><br>Geheimen van de klant worden verstrekt worden in rust versleuteld. De geheimen zijn versleuteld in rust terwijl ze zijn opgeslagen in App Service-configuratie-databases.<br><br>Lokaal gekoppelde schijven kunnen desgewenst worden gebruikt als tijdelijke opslag door websites (D:\local en % TMP %). Lokaal gekoppelde schijven zijn niet versleuteld in rust. |
| Versleuteling tijdens overdracht (zoals ExpressRoute-codering in VNet-versleuteling en versleuteling van de VNet-VNet)| Ja | Klanten kunnen web sites vereisen en het gebruik van HTTPS voor binnenkomend verkeer te configureren. Zie het blogbericht [hoe u een Azure App Service enkel HTTPS](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/). |
| Versleuteling verwerking (CMK, BYOK, enz.)| Ja | Klanten kunnen kiezen voor de toepassingsgeheimen in Key Vault opslaan en halen ze tijdens runtime. Zie [gebruik Key Vault naslaginformatie over App Service en Azure Functions (preview)](../app-service/app-service-key-vault-references.md).|
| Versleuteling op kolom (Azure Data Services)| N/A | |
| API-aanroepen die zijn versleuteld| Ja | Van beheeraanroepen naar App Service configureren plaatsvinden [Azure Resource Manager](../azure-resource-manager/index.yml) aanroepen via HTTPS. |

### <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja | Op dit moment beschikbaar zijn in Preview-versie voor App Service. Zie [toegangsbeperkingen voor Azure App Service](../app-service/app-service-ip-restrictions.md). |
| Ondersteuning voor VNet-injectie| Ja | App Service-omgevingen zijn persoonlijke implementaties van App Service speciaal voor één klant opgenomen in het virtuele netwerk van een klant. Zie [Inleiding tot de App Service-omgevingen](../app-service/environment/intro.md). |
| Ondersteuning voor netwerkisolatie en Firewalling| Ja | Klanten kunnen netwerk ACL's (IP-beperkingen) voor toegestaan binnenkomend verkeer configureren voor de openbare multitenant variatie van App Service.  Zie [toegangsbeperkingen voor Azure App Service](../app-service/app-service-ip-restrictions.md).  App Service-omgevingen worden geïmplementeerd in virtuele netwerken en daarom kunnen worden beveiligd met nsg's. |
| Geforceerde tunneling ondersteuning| Ja | App Service-omgevingen kunnen worden geïmplementeerd in een klant-netwerk waarin de geforceerde tunneling is geconfigureerd. Klanten moeten Volg de aanwijzingen in [uw App Service Environment configureren met geforceerde tunnels](../app-service/environment/forced-tunnel-support.md). |

### <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | App Service kan worden geïntegreerd met Application Insights voor talen die ondersteuning bieden voor Application Insights (volledige .NET Framework, .NET Core, Java en Node.JS).  Zie [Monitor Azure App Service-prestaties](../azure-monitor/app/azure-web-apps.md). App Service verzendt ook metrische toepassingsgegevens in Azure Monitor. Zie [apps bewaken in Azure App Service](../app-service/web-sites-monitor.md). |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Klanten kunnen toepassingen bouwen in App Service die automatisch zijn geïntegreerd met [Azure Active Directory (Azure AD)](../active-directory/index.md) en andere compatibele id-providers die OAuth; Zie [verificatie en autorisatie in Azure App Service](../app-service/overview-authentication-authorization.md). Voor van beheertoegang tot App Service-activa, worden alle toegang wordt beheerd door een combinatie van Azure AD geverifieerd principal en Azure Resource Manager RBAC-rollen. |
| Autorisatie| Ja | Voor van beheertoegang tot App Service-activa, worden alle toegang wordt beheerd door een combinatie van Azure AD geverifieerd principal en Azure Resource Manager RBAC-rollen.  |


### <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Alle bewerkingen uitgevoerd op App Service-objecten plaatsvinden [Azure Resource Manager](../azure-resource-manager/index.yml). Historische logboeken van deze bewerkingen zijn beschikbaar in de portal en via de CLI; Zie [Azure Resource Manager-resourceproviderbewerkingen](../role-based-access-control/resource-provider-operations.md#microsoftweb) en [az monitor-activiteitenlogboek](/cli/azure/monitor/activity-log). |
| Gegevens vlak logboekregistratie en controle | Nee | De gegevenslaag voor App Service is een externe bestandsshare die een klant s geïmplementeerd website-inhoud bevat.  Er is geen controle van de externe bestandsshare. |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Voor management-bewerkingen, kan de status van de configuratie van een App Service worden geëxporteerd als een Azure Resource Manager-sjabloon en versies na verloop van tijd. Klanten kunnen meerdere live verschillende versies van een toepassing met behulp van de functie App Service-implementatie sleuven onderhouden voor runtime-bewerkingen. | 


## <a name="azure-resource-managerazure-resource-managerazure-resource-manager-security-attributesmd"></a>[Azure Resource Manager](../azure-resource-manager/azure-resource-manager-security-attributes.md)

### <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja |  |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | HTTPS/TLS. |
| Versleuteling verwerking (CMK, BYOK, enz.)| N/A | Azure Resource Manager slaat geen inhoud van de klant, alleen de gegevens van het besturingselement. |
| Versleuteling op kolom (Azure Data Services)| Ja | |
| API-aanroepen die zijn versleuteld| Ja | |

### <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Nee | |
| Ondersteuning voor VNet-injectie| Ja | |
| Netwerkisolatie en ondersteuning netwerkfunctie| Nee |  |
| Geforceerde tunneling ondersteuning| Nee |  |

### <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Nee | |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | [Azure Active Directory](/azure/active-directory) op basis van.|
| Autorisatie| Ja | |


### <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Activiteitenlogboeken geven alle bewerkingen (PUT, POST, DELETE) die worden uitgevoerd op uw resources; schrijven Zie [activiteitenlogboeken om te controleren van acties op resources bekijken](../azure-resource-manager/resource-group-audit.md). |
| Gegevens vlak logboekregistratie en controle| N/A | |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja |  |


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

### <a name="configuration-management"></a>Configuratiebeheer

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

## <a name="azure-service-bus-messagingservice-bus-messagingservice-bus-messaging-security-attributesmd"></a>[Azure Service Bus Messaging](../service-bus-messaging/service-bus-messaging-security-attributes.md)

### <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>|  Ja voor serverzijde versleuteling-at-rest standaard. | Beheerd door de klant sleutels en BYOK zijn nog niet ondersteund. Versleuteling aan de clientzijde is de verantwoordelijkheid van de client |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | Biedt ondersteuning voor standaard HTTPS/TLS-mechanisme. |
| Versleuteling verwerking (CMK, BYOK, enz.)| Nee |   |
| Versleuteling op kolom (Azure Data Services)| N/A | |
| API-aanroepen die zijn versleuteld| Ja | API-aanroepen worden gedaan via [Azure Resource Manager](../azure-resource-manager/index.yml) - en HTTPS. |

### <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja (alleen Premium-laag) | VNet-service-eindpunten worden ondersteund voor [Service Bus Premium-laag](../service-bus-messaging/service-bus-premium-messaging.md) alleen. |
| Ondersteuning voor VNet-injectie| Nee | |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja (alleen Premium-laag) |  |
| Geforceerde tunneling ondersteuning| Nee |  |

### <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Ondersteund via [Azure Monitor en waarschuwingen](../service-bus-messaging/service-bus-metrics-azure-monitor.md). |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Beheerd via [Azure Active Directory beheerde Service-identiteit](../service-bus-messaging/service-bus-managed-service-identity.md); Zie [Service Bus-verificatie en autorisatie](../service-bus-messaging/service-bus-authentication-and-authorization.md).|
| Autorisatie| Ja | Biedt ondersteuning voor autorisatie via [RBAC](../service-bus-messaging/service-bus-role-based-access-control.md) (Preview) en SAS-token; Zie [Service Bus-verificatie en autorisatie](../service-bus-messaging/service-bus-authentication-and-authorization.md). |

### <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Operations-logboeken zijn beschikbaar. Zie [diagnostische logboeken van Service Bus](../service-bus-messaging/service-bus-diagnostic-logs.md).  |
| Gegevens vlak logboekregistratie en controle| Nee |  |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Biedt ondersteuning voor resource provider versiebeheer via de [Azure Resource Manager API](/rest/api/resources/).|


## <a name="azure-service-bus-relayservice-bus-relayservice-bus-relay-security-attributesmd"></a>[Azure Service Bus Relay](../service-bus-relay/service-bus-relay-security-attributes.md)

### <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>|  N/A | Relay is een web socket en gegevens niet bewaard is gebleven. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | Service is TLS vereist. |
| Versleuteling verwerking (CMK, BYOK, enz.)| Nee | Alleen Microsoft TLS-certificaten gebruikt.  |
| Versleuteling op kolom (Azure Data Services)| N/A | |
| API-aanroepen die zijn versleuteld| Ja | HTTPS. |

### <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Nee |  |
| Netwerkisolatie en ondersteuning netwerkfunctie| Nee |  |
| Geforceerde tunneling ondersteuning| N/A | Relay is de TLS-tunnel  |

### <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Via SAS. |
| Autorisatie|  Ja | Via SAS. |


### <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml). |
| Gegevens vlak logboekregistratie en controle| Ja | Verbinding geslaagd / mislukt en fouten en in het logboek geregistreerd.  |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml).|


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

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Configuratie van de service is samengesteld en geïmplementeerd met behulp van Azure implementeren. De code (toepassings- en runtime) is samengesteld met behulp van Azure bouwen.
 |

## <a name="azure-sql-databasesql-databasesql-database-security-attributesmd"></a>[Azure SQL Database](../sql-database/sql-database-security-attributes.md)

### <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja | Aangeduid als 'versleuteling in-gebruik', zoals beschreven in het artikel [Always Encrypted](../sql-database/sql-database-always-encrypted.md). Versleuteling aan de serverzijde van de service maakt gebruik van [transparante gegevensversleuteling](../sql-database/transparent-data-encryption-azure-sql.md) (TDE).|
| Versleuteling tijdens overdracht:<ul><li>ExpressRoute-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | Met behulp van HTTPS. |
| Versleuteling verwerking (CMK, BYOK, enz.)| Ja | Verwerking van zowel service beheerd door de klant beheerde sleutels worden aangeboden (de laatste via [Azure Key Vault](../key-vault/index.yml). |
| Versleuteling op kolom (Azure Data Services)| Ja | Via [Always Encrypted](../sql-database/sql-database-always-encrypted.md). |
| API-aanroepen die zijn versleuteld| Ja | Met behulp van HTTPS/SSL. |

### <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja | Is van toepassing op [individuele database](../sql-database/sql-database-single-index.yml) alleen. |
| Ondersteuning voor VNet-injectie| Ja | Is van toepassing op [beheerd exemplaar](../sql-database/sql-database-managed-instance.md) alleen. |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja | Firewall op beide database - en serverniveau; netwerkisolatie voor [beheerd exemplaar](../sql-database/sql-database-managed-instance.md) alleen |
| Geforceerde tunneling ondersteuning| Ja | [beheerd exemplaar](../sql-database/sql-database-managed-instance.md) via [Azure ExpressRoute](../expressroute/index.yml) VPN |

### <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | De SIEM-oplossing van derden van Imperva (SecureSphere) wordt ook ondersteund, via [Azure Event Hubs](../event-hubs/index.yml) integratie via [SQL audit](../sql-database/sql-database-auditing.md). |

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Azure Active Directory. |
| Autorisatie| Ja |  |


### <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Ja voor alleen bepaalde gebeurtenissen. |
| Gegevens vlak logboekregistratie en controle | Ja | Via [SQL audit](../sql-database/sql-database-auditing.md). |

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Nee  | | 

### <a name="additional-security-attributes-for-sql-database"></a>Kenmerken voor extra beveiliging voor SQL-Database

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Preventative: evaluatie van beveiligingsproblemen | Ja | Zie [evaluatie van beveiligingsproblemen SQL service kunt u identificeren databaseproblemen](../sql-database/sql-vulnerability-assessment.md). |
| Voorkomen: gegevensdetectie en classificatie  | Ja | Zie [Azure SQL Database en SQL Data Warehouse gegevensdetectie en classificatie](../sql-database/sql-database-data-discovery-and-classification.md). |
| Detectie: detectie van bedreigingen | Ja | Zie [Advanced Threat Protection voor Azure SQL Database](../sql-database/sql-database-threat-detection-overview.md). |

## <a name="azure-storagestoragecommonstorage-security-attributesmd"></a>[Azure Storage](../storage/common/storage-security-attributes.md)

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

### <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Ondersteuning voor Resource Provider versiebeheer via Azure Resource Manager-API 's |