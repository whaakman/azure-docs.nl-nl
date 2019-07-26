---
title: Beveiligings kenmerken voor Azure ExpressRoute
description: Een controle lijst met beveiligings kenmerken voor het evalueren van Azure ExpressRoute
services: expressroute
ms.service: expressroute
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: mbaldwin
ms.openlocfilehash: c9a46497c18b99ad7774036fd92e63d024b47045
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442188"
---
# <a name="security-attributes-for-azure-expressroute"></a>Beveiligings kenmerken voor Azure ExpressRoute

In dit artikel worden de beveiligings kenmerken gedocumenteerd die zijn ingebouwd in azure ExpressRoute.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies)|  N/A | ExpressRoute slaat geen klant gegevens op. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| Nee | |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| N/A |  |
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml) en HTTPS. |

## <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| N/A |  |
| Ondersteuning voor VNet-injectie| N/A | |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | Elke klant is opgenomen in een eigen routerings domein en is getunneld naar het eigen VNet |
| Ondersteuning voor geforceerde tunneling| N/A | Via Border Gateway Protocol (BGP). |

## <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Zie [ExpressRoute bewaking, metrische gegevens en waarschuwingen](expressroute-monitoring-metrics-alerts.md).|

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Service account voor de gateway voor micro soft (GWM) (controller); Just-in-time-toegang voor dev en OP. |
| Authorization|  Ja |Service account voor de gateway voor micro soft (GWM) (controller); Just-in-time-toegang voor dev en OP. |


## <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen| 
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja |  |
| Logboek registratie en controle van het gegevens vlak| Nee |   |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | Via de netwerk resource provider (NRP). |
