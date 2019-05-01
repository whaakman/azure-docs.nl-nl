---
title: Algemene beveiligingskenmerken voor Azure Service Bus Relay
description: Een controlelijst met algemene beveiligingskenmerken voor het evalueren van Azure Service Bus Relay
services: service-bus-relay
ms.service: service-bus-relay
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: mbaldwin
ms.openlocfilehash: f8827ac290393c9f394c3b13149555a1a2aa6df9
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64927490"
---
# <a name="common-security-attributes-for-azure-service-bus-relay"></a>Algemene beveiligingskenmerken voor Azure Service Bus Relay

Beveiliging is geïntegreerd in elk aspect van een Azure-service. In dit artikel worden de algemene beveiligingskenmerken die zijn ingebouwd in Azure Service Bus Relay.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>|  N/A | Relay is een web socket en gegevens niet bewaard is gebleven. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>Vnet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | Service is TLS vereist. |
| Versleuteling verwerking (CMK, BYOK, enz.)| Nee | Alleen Microsoft TLS-certificaten gebruikt.  |
| Versleuteling op kolom (Azure Data Services)| N/A | |
| API-aanroepen die zijn versleuteld| Ja | HTTPS. |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Nee |  |
| Netwerkisolatie en ondersteuning netwerkfunctie| Nee |  |
| Geforceerde tunneling ondersteuning| N/A | Relay is de TLS-tunnel  |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Via SAS. |
| Autorisatie|  Ja | Via SAS. |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml). |
| Gegevens vlak logboekregistratie en controle| Ja | Verbinding geslaagd / mislukt en fouten en in het logboek geregistreerd.  |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml).|
