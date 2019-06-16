---
title: Beveiligingskenmerken voor Azure VPN-Gateway
description: Een controlelijst met beveiligingsinstellingen voor het evalueren van de Azure VPN-Gateway
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: 0e58e7b3f77d9bb673e300aa60ad07ca9dba5153
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083130"
---
# <a name="security-attributes-for-azure-vpn-gateway"></a>Beveiligingskenmerken voor Azure VPN-Gateway

In dit artikel worden de algemene beveiligingskenmerken die zijn ingebouwd in de Azure VPN-Gateway.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]


## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest (zoals versleuteling op de server, server-side-versleuteling met de klant beheerde sleutels en andere versleutelingsfuncties) | N/A | Klantgegevens voor VPN-gateway doorvoer, slaat geen gegevens van de klant |
| Versleuteling tijdens overdracht (zoals ExpressRoute-codering in VNet-versleuteling en versleuteling van de VNet-VNet)| Ja | VPN-gateway versleutelen klant pakketten tussen Azure VPN-gateways en on-premises VPN-apparaten (S2S) van de klant of VPN-clients (P2S). VPN-gateways bieden ook ondersteuning voor VNet-naar-VNet-versleuteling. |
| Versleuteling verwerking (CMK, BYOK, enz.)| Nee | Opgegeven door de klant vooraf-gedeelde sleutels zijn versleuteld in rust; maar niet geïntegreerd met CMK nog. |
| Versleuteling op kolom (Azure Data Services)| N/A | |
| API-aanroepen die zijn versleuteld| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml) - en HTTPS  |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| N/A | |
| Ondersteuning voor VNet-injectie| N/A | . |
| Ondersteuning voor netwerkisolatie en Firewalling| Ja | VPN-gateways zijn toegewezen VM-exemplaren voor elke klant Virtueelnetwerk  |
| Geforceerde tunneling ondersteuning| Ja |  |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Zie [Azure Monitor Diagnostics-logboeken/waarschuwing](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md) & [Azure Monitor Metrics/waarschuwing](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).  |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) voor het beheren van de service en de Azure VPN-gateway configureren. |
| Autorisatie| Ja | Ondersteuning voor autorisatie via [RBAC](../role-based-access-control/overview.md). |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Azure Resource Manager-activiteitenlogboek. |
| Gegevens vlak logboekregistratie en controle | Ja | [Diagnostische logboeken in Azure Monitor](../azure-resource-manager/resource-group-audit.md) voor VPN-connectiviteit logboekregistratie en controle. |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Voor management-bewerkingen, kan de status van de configuratie van een Azure VPN-gateway worden geëxporteerd als een Azure Resource Manager-sjabloon en versies na verloop van tijd. | 