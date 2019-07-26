---
title: Beveiligings kenmerken voor Azure Load Balancer
description: Een controle lijst met beveiligings kenmerken voor het evalueren van Load Balancer
services: load-balancer
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mbaldwin
ms.openlocfilehash: 7d40618d5f4fde4a2b3fdfbde8a6de0a049ce3b6
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440872"
---
# <a name="security-attributes-for-azure-load-balancer"></a>Beveiligings kenmerken voor Azure Load Balancer

In dit artikel worden de beveiligings kenmerken gedocumenteerd die zijn ingebouwd in Azure Load Balancer.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | N/A | |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| N/A | |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| N/A | |
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | Via de [Azure Resource Manager](../azure-resource-manager/index.yml). |

## <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| N/A | |
| Ondersteuning voor VNet-injectie| N/A | . |
| Ondersteuning voor netwerk isolatie en firewalling| N/A |  |
| Ondersteuning voor geforceerde tunneling| N/A | |

## <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Zie [Azure monitor-logboeken voor open bare basis Load Balancer](load-balancer-monitor-log.md). |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| N/A |  |
| Authorization| N/A |  |


## <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Zie [Azure monitor-logboeken voor open bare basis Load Balancer](load-balancer-monitor-log.md). |
| Logboek registratie en controle van het gegevens vlak | N/A |  |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| N/A |  | 
