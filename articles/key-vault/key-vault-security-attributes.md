---
title: Beveiligings kenmerken voor Azure Key Vault
description: Een controle lijst met beveiligings kenmerken voor het evalueren van Azure Key Vault
services: key-vault
author: msmbaldwin
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 836d025c5bc69da9606c9a6172ac6a43caaaf29b
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444008"
---
# <a name="security-attributes-for-azure-key-vault"></a>Beveiligings kenmerken voor Azure Key Vault

In dit artikel worden de beveiligings kenmerken gedocumenteerd die zijn ingebouwd in Azure Key Vault. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies)| Ja | Alle objecten zijn versleuteld. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| Ja | Alle communicatie via versleutelde API-aanroepen |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Ja | De klant beheert alle sleutels in hun Key Vault. Wanneer er ondersteunde sleutels voor de Hardware Security module (HSM) worden opgegeven, wordt de sleutel, het certificaat of het geheim beschermd door een HSM met FIPS Level 2. |
| Versleuteling op kolom niveau (Azure Data Services)| N/A |  |
| Versleutelde API-aanroepen| Ja | HTTPS gebruiken. |

## <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja | Het gebruik van de service-eind punten van Virtual Network (VNet). |
| Ondersteuning voor VNet-injectie| Nee |  |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | Met VNet-firewall regels. |
| Ondersteuning voor geforceerde tunneling| Nee |  |

## <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Log Analytics gebruiken. |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Verificatie is via Azure Active Directory. |
| Authorization| Ja | Key Vault toegangs beleid gebruiken. |


## <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle van beheer/beheer vlak| Ja | Log Analytics gebruiken. |
| Logboek registratie en controle van het gegevens vlak| Ja | Log Analytics gebruiken. |

## <a name="access-controls"></a>Besturingselementen voor toegang

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Toegangs beheer voor het besturings element/beheer vlak | Ja | Toegangsbeheer op basis van rollen (RBAC) in Azure Resource Manager |
| Toegangs beheer voor gegevens vlak (op elk service niveau) | Ja | Toegangs beleid Key Vault |