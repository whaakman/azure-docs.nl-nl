---
title: Beveiligings kenmerken voor Azure App Service
description: Een controle lijst met beveiligings kenmerken voor het evalueren van Azure App Service
services: app-service
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbaldwin
ms.openlocfilehash: 299262610c027529749840720f46d6dc97a07b21
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442258"
---
# <a name="security-attributes-for-azure-app-service"></a>Beveiligings kenmerken voor Azure App Service

In dit artikel worden de beveiligings kenmerken gedocumenteerd die zijn ingebouwd in Azure App Service.

[!INCLUDE [Security attributes header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | Ja | Inhoud van website bestand wordt opgeslagen in Azure Storage, die de inhoud automatisch versleutelt. Zie [Azure Storage versleuteling voor Data-at-rest](../storage/common/storage-service-encryption.md).<br><br>Door de klant verstrekte geheimen worden op rest versleuteld. De geheimen worden op rest versleuteld terwijl ze zijn opgeslagen in App Service configuratie databases.<br><br>Lokaal gekoppelde schijven kunnen eventueel worden gebruikt als tijdelijke opslag door websites (D:\Local en% TMP%). Lokaal gekoppelde schijven zijn niet versleuteld op rest. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| Ja | Klanten kunnen websites zo configureren dat ze HTTPS vereisen en gebruiken voor inkomend verkeer. Zie het blog bericht [hoe u een Azure app service alleen https maakt](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/). |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Ja | Klanten kunnen ervoor kiezen om toepassings geheimen op te slaan in Key Vault en deze op te halen tijdens runtime. Zie [Key Vault verwijzingen gebruiken voor app service en Azure functions (preview)](app-service-key-vault-references.md).|
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | Beheer aanroepen om App Service te configureren, worden uitgevoerd via [Azure Resource Manager](../azure-resource-manager/index.yml) -aanroepen via https. |

## <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja | Momenteel beschikbaar als Preview voor App Service. Zie [Azure app service toegangs beperkingen](app-service-ip-restrictions.md). |
| Ondersteuning voor VNet-injectie| Ja | App Service omgevingen zijn persoonlijke implementaties van App Service toegewezen aan één klant die is geïnjecteerd in het virtuele netwerk van een klant. Zie [Inleiding tot de app service omgevingen](environment/intro.md). |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | Voor de open bare multi tenant-variatie van App Service kunnen klanten netwerk-Acl's (IP-beperkingen) configureren om toegestaan binnenkomend verkeer te vergren delen.  Zie [Azure app service toegangs beperkingen](app-service-ip-restrictions.md).  App Service omgevingen worden rechtstreeks geïmplementeerd in virtuele netwerken en kunnen daarom worden beveiligd met Nsg's. |
| Ondersteuning voor geforceerde tunneling| Ja | App Service omgevingen kunnen worden geïmplementeerd in het virtuele netwerk van een klant waar geforceerde tunneling is geconfigureerd. Klanten moeten de instructies volgen in [uw app service Environment configureren met geforceerde tunneling](environment/forced-tunnel-support.md). |

## <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | App Service integreert met Application Insights voor talen die Application Insights ondersteunen (volledige .NET Framework, .NET core, Java en node. JS).  Zie [Azure app service prestaties bewaken](../azure-monitor/app/azure-web-apps.md). Met App Service worden metrische gegevens van toepassingen ook naar Azure Monitor verzonden. Zie [apps bewaken in azure app service](web-sites-monitor.md). |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Klanten kunnen toepassingen bouwen op App Service die automatisch worden geïntegreerd met [Azure Active Directory (Azure AD)](../active-directory/index.yml) , evenals andere OAuth-compatibele id-providers. Zie [verificatie en autorisatie in azure app service](overview-authentication-authorization.md). Voor beheer toegang tot App Service assets wordt alle toegang bepaald door een combi natie van door Azure AD geverifieerde Principal en Azure Resource Manager RBAC-rollen. |
| Authorization| Ja | Voor beheer toegang tot App Service assets wordt alle toegang bepaald door een combi natie van door Azure AD geverifieerde Principal en Azure Resource Manager RBAC-rollen.  |


## <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Alle beheer bewerkingen die op App Service objecten worden uitgevoerd, vindt plaats via [Azure Resource Manager](../azure-resource-manager/index.yml). Historische logboeken van deze bewerkingen zijn beschikbaar in de portal en via de CLI. Zie [Azure Resource Manager van de resource provider bewerkingen](../role-based-access-control/resource-provider-operations.md#microsoftweb) en [AZ monitor Activity-Log](/cli/azure/monitor/activity-log). |
| Logboek registratie en controle van het gegevens vlak | Nee | Het gegevens vlak voor App Service is een externe bestands share met de inhoud van de geïmplementeerde website van de klant.  De externe bestands share kan niet worden gecontroleerd. |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | Voor beheer bewerkingen kan de status van een App Service configuratie worden geëxporteerd als een Azure Resource Manager sjabloon en de versie in de loop van de tijd. Voor runtime bewerkingen kunnen klanten meerdere verschillende live versies van een toepassing onderhouden met behulp van de functie App Service implementatie sleuven. | 

