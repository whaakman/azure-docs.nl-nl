---
title: Beveiligings kenmerken voor Azure API Management
description: Een controle lijst met beveiligings kenmerken voor het evalueren van API Management
services: api-management
author: msmbaldwin
manager: barbkess
ms.service: api-management
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 2049e2349e3a25ebd4d3f4db19ec47bbaeb067de
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442287"
---
# <a name="security-attributes-for-api-management"></a>Beveiligings kenmerken voor API Management

In dit artikel worden de beveiligings kenmerken gedocumenteerd die zijn ingebouwd in API Management.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies)| Ja (alleen versleuteling aan de service zijde) | Gevoelige gegevens zoals certificaten, sleutels en geheime naam waarden worden versleuteld met Service-beheerde exemplaren per service-exemplaar. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| Ja | [Express route](../expressroute/index.yml) en VNet-versleuteling worden door [Azure-netwerken](../virtual-network/index.yml)verschaft. |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Nee | Alle versleutelings sleutels zijn per service-exemplaar en worden beheerd door de service. |
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | Aanroepen van beheer vlak worden gedaan via [Azure Resource Manager](../azure-resource-manager/index.yml) via TLS. Er is een geldig JSON-webtoken (JWT) vereist.  Aanroepen voor gegevens vlak kunnen worden beveiligd met TLS en een van de ondersteunde verificatie mechanismen (bijvoorbeeld client certificaat of JWT).
 |

## <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Nee | |
| Ondersteuning voor VNet-injectie| Ja | |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | Respectievelijk netwerk beveiligings groepen (NSG) en Azure-toepassing gateway (of ander software apparaat) gebruiken. |
| Ondersteuning voor geforceerde tunneling| Ja | Azure-netwerken bieden geforceerde tunneling. |

## <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | |
| Authorization| Ja | |


## <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | [Azure Monitor activiteiten logboeken](../azure-monitor/platform/activity-logs-overview.md) |
| Logboek registratie en controle van het gegevens vlak| Ja | [Azure monitor Diagnostische logboeken](../azure-monitor/platform/diagnostic-logs-overview.md) en (optioneel) [Azure-toepassing Insights](../azure-monitor/app/app-insights-overview.md).  |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | De [Azure API Management DevOps Resource Kit](https://aka.ms/apimdevops) gebruiken |

## <a name="vulnerability-scans-false-positives"></a>Beveiligings probleem scant onjuiste positieven

In deze sectie worden veelvoorkomende beveiligings problemen beschreven, die geen invloed hebben op Azure API Management.

| Beveiligingsprobleem               | Description                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ticketbleed (CVE-2016-9244) | Ticketbleed is een beveiligings probleem in de implementatie van de TLS SessionTicket-extensie die is gevonden in sommige F5-producten. Hiermee kan het lekken (' verbloeden ') van Maxi maal 31 bytes aan gegevens van niet-geïnitialiseerd geheugen. Dit wordt veroorzaakt door de TLS-stack opvulling van een sessie-ID, door gegeven van de client, met gegevens om deze 32 bits lang te maken. |
