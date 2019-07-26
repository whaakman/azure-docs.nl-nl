---
title: Beveiligings kenmerken voor Azure Resource Manager
description: Een controle lijst met beveiligings kenmerken voor het evalueren van Azure Resource Manager
services: azure-resource-manager
author: msmbaldwin
manager: barbkess
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: mbaldwin
ms.openlocfilehash: e3bfb79c54ff57adfa947f2dd0100f6c05c7af9f
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444156"
---
# <a name="security-attributes-for-azure-resource-manager"></a>Beveiligings kenmerken voor Azure Resource Manager

In dit artikel worden de beveiligings kenmerken gedocumenteerd die zijn ingebouwd in Azure Resource Manager.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies)| Ja |  |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| Ja | HTTPS/TLS. |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| N/A | Azure Resource Manager slaat geen klant inhoud op, alleen gegevens beheren. |
| Versleuteling op kolom niveau (Azure Data Services)| Ja | |
| Versleutelde API-aanroepen| Ja | |

## <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Nee | |
| Ondersteuning voor VNet-injectie| Ja | |
| Ondersteuning voor netwerk isolatie en firewalling| Nee |  |
| Ondersteuning voor geforceerde tunneling| Nee |  |

## <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Nee | |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | [Azure Active Directory](/azure/active-directory) gebaseerd.|
| Authorization| Ja | |


## <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Met activiteiten logboeken worden alle schrijf bewerkingen (PUT, POST, DELETE) die zijn uitgevoerd op uw resources beschikbaar gesteld. Zie [activiteiten logboeken weer geven om acties op resources te controleren](resource-group-audit.md). |
| Logboek registratie en controle van het gegevens vlak| N/A | |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja |  |
