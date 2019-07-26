---
title: Beveiligings kenmerken voor Azure Service Bus relay
description: Een controle lijst met beveiligings kenmerken voor het evalueren van Azure Service Bus relay
services: service-bus-relay
ms.service: service-bus-relay
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: mbaldwin
ms.openlocfilehash: 2047f64914d4a286e6de38b7b2c8524d98eba562
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443885"
---
# <a name="security-attributes-for-azure-service-bus-relay"></a>Beveiligings kenmerken voor Azure Service Bus relay

In dit artikel worden de beveiligings kenmerken gedocumenteerd die zijn ingebouwd in Azure Service Bus relay.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies)|  N/A | Relay is een WebSocket en er worden geen gegevens bewaard. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| Ja | Voor service is TLS vereist. |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Nee | Maakt gebruik van alleen micro soft TLS-certificaten.  |
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja | HTTPS. |

## <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Nee |  |
| Ondersteuning voor netwerk isolatie en firewalling| Nee |  |
| Ondersteuning voor geforceerde tunneling| N/A | Relay is de TLS-tunnel  |

## <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Via SAS. |
| Authorization|  Ja | Via SAS. |


## <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml). |
| Logboek registratie en controle van het gegevens vlak| Ja | Geslaagde/mislukte verbindingen en Logboeken.  |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | Via [Azure Resource Manager](../azure-resource-manager/index.yml).|
