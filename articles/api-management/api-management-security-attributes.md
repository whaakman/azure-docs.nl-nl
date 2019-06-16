---
title: Beveiligingskenmerken voor Azure API Management
description: Een controlelijst met algemene beveiligingskenmerken voor het evalueren van API Management
services: api-management
author: msmbaldwin
manager: barbkess
ms.service: api-management
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 3b5826d472b80179c5eb76e0e3a6b1c7ee282487
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66001094"
---
# <a name="security-attributes-for-api-management"></a>Beveiligingskenmerken voor API Management

In dit artikel worden de beveiligingskenmerken die is ingebouwd in API Management.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja (alleen voor versleuteling van de service-aan de serverzijde) | Gevoelige gegevens, zoals certificaten, sleutels en waarden voor de naam van geheim zijn versleuteld met service-beheerd, per service-exemplaar sleutels. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | [Express Route](../expressroute/index.yml) en VNet-versleuteling geleverd door [Azure-netwerken](../virtual-network/index.yml). |
| Versleuteling verwerking (CMK, BYOK, enz.)| Nee | Alle versleutelingssleutels zijn per service-exemplaar en service die wordt beheerd. |
| Versleuteling op kolom (Azure Data Services)| N/A | |
| API-aanroepen die zijn versleuteld| Ja | Vlak beheeraanroepen worden aangeleverd via [Azure Resource Manager](../azure-resource-manager/index.yml) via TLS. Een geldig JSON webtoken (JWT) is vereist.  Gegevens vlak aanroepen kunnen worden beveiligd met TLS- en een van de ondersteunde verificatiemechanismen (bijvoorbeeld clientcertificaat of JWT).
 |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Nee | |
| Ondersteuning voor VNet-injectie| Ja | |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja | Met behulp van netwerken netwerkbeveiligingsgroepen (nsg's) en Azure Application Gateway (of andere software-apparaat) respectievelijk. |
| Geforceerde tunneling ondersteuning| Ja | Azure-netwerk bieden geforceerde tunneling. |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | |
| Autorisatie| Ja | |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | [Activiteitenlogboeken van Azure Monitor](../azure-monitor/platform/activity-logs-overview.md) |
| Gegevens vlak logboekregistratie en controle| Ja | [Diagnostische logboeken in Azure Monitor](../azure-monitor/platform/diagnostic-logs-overview.md) en (optioneel) [Azure Application Insights](../azure-monitor/app/app-insights-overview.md).  |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Met behulp van de [Azure API Management DevOps resourcekit](https://aka.ms/apimdevops) |

## <a name="vulnerability-scans-false-positives"></a>Beveiligingslek met betrekking tot scant fout-positieven

In deze sectie worden algemene beveiligingsproblemen, dat niet van invloed zijn op de Azure API Management.

| Beveiligingslek in               | Description                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ticketbleed (CVE-2016-9244) | Ticketbleed is door een beveiligingslek in de implementatie van de gevonden in sommige producten F5 SessionTicket TLS-extensie. Hierdoor kan het lekken van gegevens ('uiterst') van de tot 31 bytes van gegevens uit het geheugen niet geïnitialiseerd. Dit wordt veroorzaakt door de TLS-stack opvulling van een sessie-ID van de client, met de gegevens gemakkelijk 32-bits lang is doorgegeven. |
