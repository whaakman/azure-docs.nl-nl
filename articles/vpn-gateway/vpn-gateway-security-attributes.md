---
title: Beveiligings kenmerken voor Azure VPN Gateway
description: Een controle lijst met beveiligings kenmerken voor het evalueren van Azure VPN Gateway
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: c355f70975f441609304a4c9ee2ead75f0e0ce25
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444590"
---
# <a name="security-attributes-for-azure-vpn-gateway"></a>Beveiligings kenmerken voor Azure VPN Gateway

In dit artikel worden de beveiligings kenmerken gedocumenteerd die zijn ingebouwd in azure VPN Gateway.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]


## <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies) | N/A | Klant gegevens van VPN-gateway-door voer worden niet opgeslagen |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| Ja | VPN-gateway versleutelt klant pakketten tussen Azure VPN-gateways en klant-on-premises VPN-apparaten (S2S) of VPN-clients (P2S). VPN-gateways ondersteunen ook VNet-naar-VNet-versleuteling. |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Nee | Door de klant opgegeven vooraf gedeelde sleutels worden op rest versleuteld. maar is nog niet geïntegreerd met CMK. |
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml) en https  |

## <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| N/A | |
| Ondersteuning voor VNet-injectie| N/A | . |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | VPN-gateways zijn toegewezen VM-exemplaren voor elke klant Virtual Network  |
| Ondersteuning voor geforceerde tunneling| Ja |  |

## <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Zie [Azure monitor Diagnostische logboeken/waarschuwing](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md) & [Azure monitor metrische gegevens/waarschuwing](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).  |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) voor het beheren van de service en het configureren van de Azure VPN-gateway. |
| Authorization| Ja | Ondersteuning voor autorisatie via [RBAC](../role-based-access-control/overview.md). |


## <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Azure Resource Manager activiteiten logboek. |
| Logboek registratie en controle van het gegevens vlak | Ja | [Diagnostische logboeken Azure monitor](../azure-resource-manager/resource-group-audit.md) voor logboek registratie en controle van de VPN-verbinding. |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | Voor beheer bewerkingen kan de status van een configuratie van een Azure VPN-gateway worden geëxporteerd als een Azure Resource Manager sjabloon en in de loop van de tijd. | 