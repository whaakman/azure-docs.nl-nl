---
title: Algemene beveiligingskenmerken voor Azure Key Vault
description: Een controlelijst met algemene beveiligingskenmerken voor het evalueren van Azure Key Vault
services: key-vault
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 3ccfc38136ba3e8ec7c6130658032b7565988e5c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60461401"
---
# <a name="common-security-attributes-for-azure-key-vault"></a>Algemene beveiligingskenmerken voor Azure Key Vault

Beveiliging is geïntegreerd in elk aspect van een Azure-service. In dit artikel worden de algemene beveiligingskenmerken die zijn ingebouwd in Azure Key Vault. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja | Alle objecten zijn versleuteld. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>Vnet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | Alle communicatie gaat via versleutelde API-aanroepen |
| Versleuteling verwerking (CMK, BYOK, enz.)| Ja | Alle sleutels in hun Key Vault wordt bepaald door de klant. Wanneer een hardware security module (HSM) back-sleutels zijn opgegeven, wordt een FIPS Level 2 HSM beveiligt de sleutel, het certificaat of het geheim. |
| Versleuteling op kolom (Azure Data Services)| N/A |  |
| API-aanroepen die zijn versleuteld| Ja | Met behulp van HTTPS. |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja | Met behulp van service-eindpunten voor Virtueelnetwerk (Vnet). |
| ondersteuning voor vNET-injectie| Nee |  |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja | Met behulp van Vnet-firewallregels. |
| Geforceerde tunneling ondersteuning| Nee |  |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Met behulp van Log Analytics. |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Verificatie is via Azure Active Directory. |
| Autorisatie| Ja | Met behulp van toegangsbeleid voor Key Vault. |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Beheer/beheer vlak logboekregistratie en controle| Ja | Met behulp van Log Analytics. |
| Gegevens vlak logboekregistratie en controle| Ja | Met behulp van Log Analytics. |

## <a name="access-controls"></a>Besturingselementen voor toegang

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Beheer/beheer vlak-toegangsbeheer | Ja | Toegangsbeheer op basis van rollen (RBAC) in Azure Resource Manager |
| Gegevens vlak toegangsbeheer (op het serviceniveau van elke) | Ja | Toegangsbeleid voor Key Vault |