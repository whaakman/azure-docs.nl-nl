---
title: Beveiligingskenmerken voor Azure Load Balancer
description: Een controlelijst met beveiligingsinstellingen voor het evalueren van de Load Balancer
services: load-balancer
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mbaldwin
ms.openlocfilehash: 1892b918ed35221bc73d5070d5a73ecc359aa4c2
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800079"
---
# <a name="security-attributes-for-azure-load-balancer"></a>Beveiligingskenmerken voor Azure Load Balancer

In dit artikel worden de algemene beveiligingskenmerken die zijn ingebouwd in Azure Load Balancer.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest (zoals versleuteling op de server, server-side-versleuteling met de klant beheerde sleutels en andere versleutelingsfuncties) | N/A | |
| Versleuteling tijdens overdracht (zoals ExpressRoute-codering in VNet-versleuteling en versleuteling van de VNet-VNet)| N/A | |
| Versleuteling verwerking (CMK, BYOK, enz.)| N/A | |
| Versleuteling op kolom (Azure Data Services)| N/A | |
| API-aanroepen die zijn versleuteld| Ja | Via de [Azure Resource Manager](../azure-resource-manager/index.yml). |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| N/A | |
| Ondersteuning voor VNet-injectie| N/A | . |
| Ondersteuning voor netwerkisolatie en Firewalling| N/A |  |
| Geforceerde tunneling ondersteuning| N/A | |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Zie [Azure Monitor-logboeken voor openbare Basic Load Balancer](load-balancer-monitor-log.md). |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| N/A |  |
| Authorization| N/A |  |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Zie [Azure Monitor-logboeken voor openbare Basic Load Balancer](load-balancer-monitor-log.md). |
| Gegevens vlak logboekregistratie en controle | N/A |  |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| N/A |  | 
