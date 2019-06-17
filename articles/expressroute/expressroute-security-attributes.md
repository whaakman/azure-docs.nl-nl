---
title: Algemene beveiligingskenmerken voor Azure ExpressRoute
description: Een controlelijst met algemene beveiligingskenmerken voor het evalueren van Azure ExpressRoute
services: expressroute
ms.service: expressroute
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: mbaldwin
ms.openlocfilehash: d6156715fb87831d465197fd8eec59d245221e48
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083273"
---
# <a name="common-security-attributes-for-azure-expressroute"></a>Algemene beveiligingskenmerken voor Azure ExpressRoute

Beveiliging is geïntegreerd in elk aspect van een Azure-service. In dit artikel worden de algemene beveiligingskenmerken die zijn ingebouwd in Azure ExpressRoute.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>|  N/A | ExpressRoute slaat geen gegevens van de klant. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>Vnet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Nee | |
| Versleuteling verwerking (CMK, BYOK, enz.)| N/A |  |
| Versleuteling op kolom (Azure Data Services)| N/A | |
| API-aanroepen die zijn versleuteld| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml) - en HTTPS. |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| N/A |  |
| ondersteuning voor vNET-injectie| N/A | |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja | Elke klant is opgenomen in een eigen routeringsdomein en tunnels wordt verzonden naar een eigen VNet |
| Geforceerde tunneling ondersteuning| N/A | Via Border Gateway Protocol (BGP). |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Zie [ExpressRoute-bewaking, metrische gegevens en waarschuwingen](expressroute-monitoring-metrics-alerts.md).|

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Service-account voor de Gateway voor Microsoft (GWM) (controller); Toegang voor ontwikkel- en OP. Just-in-Time (JIT) |
| Autorisatie|  Ja |Service-account voor de Gateway voor Microsoft (GWM) (controller); Toegang voor ontwikkel- en OP. Just-in-Time (JIT) |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen| 
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja |  |
| Gegevens vlak logboekregistratie en controle| Nee |   |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Via de Netwerkresourceprovider (NRP). |
