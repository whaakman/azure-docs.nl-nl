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
ms.openlocfilehash: d8ce3c995e8e0f20ed6d694f481cc5fc9fde4fa7
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66000154"
---
# <a name="security-attributes-for-azure-service-bus-relay"></a>Beveiligingskenmerken voor Azure Service Bus Relay

In dit artikel worden de beveiligingskenmerken die is ingebouwd in Azure Service Bus Relay.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>|  N/A | Relay is een web socket en gegevens niet bewaard is gebleven. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | Service is TLS vereist. |
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
