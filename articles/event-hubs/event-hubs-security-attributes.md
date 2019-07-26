---
title: Beveiligings kenmerken voor Azure Event Hubs
description: Een controle lijst met beveiligings kenmerken voor het evalueren van Azure Event Hubs
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: mbaldwin
ms.openlocfilehash: 2262609de774eb2b1334215bf46968b5554ed691
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442458"
---
# <a name="security-attributes-for-azure-event-hubs"></a>Beveiligings kenmerken voor Azure Event Hubs

In dit artikel worden de beveiligings kenmerken gedocumenteerd die zijn ingebouwd in azure Event Hubs.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies)|  Ja | |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| Ja | |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Nee |  |
| Versleuteling op kolom niveau (Azure Data Services)| N/A | |
| Versleutelde API-aanroepen| Ja |  |

## <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja |  |
| Ondersteuning voor VNet-injectie| Nee | |
| Ondersteuning voor netwerk isolatie en firewalling| Ja |  |
| Ondersteuning voor geforceerde tunneling| Nee |  |

## <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | |
| Authorization|  Ja | |


## <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja |  |
| Logboek registratie en controle van het gegevens vlak| Ja |   |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja | |
