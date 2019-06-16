---
title: Beveiligingskenmerken voor Azure App Service
description: Een controlelijst met beveiligingsinstellingen voor het evalueren van Azure App Service
services: app-service
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbaldwin
ms.openlocfilehash: e38e1479c1e094f691e8f22f0a48f9342cae5cf3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66475079"
---
# <a name="security-attributes-for-azure-app-service"></a>Beveiligingskenmerken voor Azure App Service

In dit artikel worden de algemene beveiligingskenmerken die zijn ingebouwd in Azure App Service.

[!INCLUDE [Security attributes header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest (zoals versleuteling op de server, server-side-versleuteling met de klant beheerde sleutels en andere versleutelingsfuncties) | Ja | Website-inhoud wordt opgeslagen in Azure Storage, codeert automatisch de inhoud in rust. Zie [Azure Storage-versleuteling van data-at-rest](../storage/common/storage-service-encryption.md).<br><br>Geheimen van de klant worden verstrekt worden in rust versleuteld. De geheimen zijn versleuteld in rust terwijl ze zijn opgeslagen in App Service-configuratie-databases.<br><br>Lokaal gekoppelde schijven kunnen desgewenst worden gebruikt als tijdelijke opslag door websites (D:\local en % TMP %). Lokaal gekoppelde schijven zijn niet versleuteld in rust. |
| Versleuteling tijdens overdracht (zoals ExpressRoute-codering in VNet-versleuteling en versleuteling van de VNet-VNet)| Ja | Klanten kunnen web sites vereisen en het gebruik van HTTPS voor binnenkomend verkeer te configureren. Zie het blogbericht [hoe u een Azure App Service enkel HTTPS](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/). |
| Versleuteling verwerking (CMK, BYOK, enz.)| Ja | Klanten kunnen kiezen voor de toepassingsgeheimen in Key Vault opslaan en halen ze tijdens runtime. Zie [gebruik Key Vault naslaginformatie over App Service en Azure Functions (preview)](app-service-key-vault-references.md).|
| Versleuteling op kolom (Azure Data Services)| N/A | |
| API-aanroepen die zijn versleuteld| Ja | Van beheeraanroepen naar App Service configureren plaatsvinden [Azure Resource Manager](../azure-resource-manager/index.yml) aanroepen via HTTPS. |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja | Op dit moment beschikbaar zijn in Preview-versie voor App Service. Zie [toegangsbeperkingen voor Azure App Service](app-service-ip-restrictions.md). |
| Ondersteuning voor VNet-injectie| Ja | App Service-omgevingen zijn persoonlijke implementaties van App Service speciaal voor één klant opgenomen in het virtuele netwerk van een klant. Zie [Inleiding tot de App Service-omgevingen](environment/intro.md). |
| Ondersteuning voor netwerkisolatie en Firewalling| Ja | Klanten kunnen netwerk ACL's (IP-beperkingen) voor toegestaan binnenkomend verkeer configureren voor de openbare multitenant variatie van App Service.  Zie [toegangsbeperkingen voor Azure App Service](app-service-ip-restrictions.md).  App Service-omgevingen worden geïmplementeerd in virtuele netwerken en daarom kunnen worden beveiligd met nsg's. |
| Geforceerde tunneling ondersteuning| Ja | App Service-omgevingen kunnen worden geïmplementeerd in een klant-netwerk waarin de geforceerde tunneling is geconfigureerd. Klanten moeten Volg de aanwijzingen in [uw App Service Environment configureren met geforceerde tunnels](environment/forced-tunnel-support.md). |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | App Service kan worden geïntegreerd met Application Insights voor talen die ondersteuning bieden voor Application Insights (volledige .NET Framework, .NET Core, Java en Node.JS).  Zie [Monitor Azure App Service-prestaties](../azure-monitor/app/azure-web-apps.md). App Service verzendt ook metrische toepassingsgegevens in Azure Monitor. Zie [apps bewaken in Azure App Service](web-sites-monitor.md). |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Klanten kunnen toepassingen bouwen in App Service die automatisch zijn geïntegreerd met [Azure Active Directory (Azure AD)](../active-directory/index.yml) en andere compatibele id-providers die OAuth; Zie [verificatie en autorisatie in Azure App Service](overview-authentication-authorization.md). Voor van beheertoegang tot App Service-activa, worden alle toegang wordt beheerd door een combinatie van Azure AD geverifieerd principal en Azure Resource Manager RBAC-rollen. |
| Autorisatie| Ja | Voor van beheertoegang tot App Service-activa, worden alle toegang wordt beheerd door een combinatie van Azure AD geverifieerd principal en Azure Resource Manager RBAC-rollen.  |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Alle bewerkingen uitgevoerd op App Service-objecten plaatsvinden [Azure Resource Manager](../azure-resource-manager/index.yml). Historische logboeken van deze bewerkingen zijn beschikbaar in de portal en via de CLI; Zie [Azure Resource Manager-resourceproviderbewerkingen](../role-based-access-control/resource-provider-operations.md#microsoftweb) en [az monitor-activiteitenlogboek](/cli/azure/monitor/activity-log). |
| Gegevens vlak logboekregistratie en controle | Nee | De gegevenslaag voor App Service is een externe bestandsshare die van de klant geïmplementeerde website-inhoud bevat.  Er is geen controle van de externe bestandsshare. |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Voor management-bewerkingen, kan de status van de configuratie van een App Service worden geëxporteerd als een Azure Resource Manager-sjabloon en versies na verloop van tijd. Klanten kunnen meerdere live verschillende versies van een toepassing met behulp van de functie App Service-implementatie sleuven onderhouden voor runtime-bewerkingen. | 

