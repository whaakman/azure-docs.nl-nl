---
title: Algemene beveiligingskenmerken voor Azure Resource Manager
description: Een controlelijst met algemene beveiligingskenmerken voor het evalueren van Azure Resource Manager
services: azure-resource-manager
author: msmbaldwin
manager: barbkess
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: mbaldwin
ms.openlocfilehash: ebc39dcd9fe921c794add48cc677a799841cbb78
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943628"
---
# <a name="common-security-attributes-for-azure-resource-manager"></a>Algemene beveiligingskenmerken voor Azure Resource Manager

Beveiliging is geïntegreerd in elk aspect van een Azure-service. In dit artikel worden de algemene beveiligingskenmerken gebouwd in Azure Resource Manager.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja |  |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>Vnet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | HTTPS/TLS. |
| Versleuteling verwerking (CMK, BYOK, enz.)| N/A | Azure Resource Manager slaat geen inhoud van de klant, alleen de gegevens van het besturingselement. |
| Versleuteling op kolom (Azure Data Services)| Ja | |
| API-aanroepen die zijn versleuteld| Ja | |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Nee | |
| Ondersteuning voor VNet-injectie| Ja | |
| Netwerkisolatie en ondersteuning netwerkfunctie| Nee |  |
| Geforceerde tunneling ondersteuning| Nee |  |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Nee | |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | [Azure Active Directory](/azure/active-directory) op basis van.|
| Autorisatie| Ja | |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Activiteitenlogboeken geven alle bewerkingen (PUT, POST, DELETE) die worden uitgevoerd op uw resources; schrijven Zie [activiteitenlogboeken om te controleren van acties op resources bekijken](resource-group-audit.md). |
| Gegevens vlak logboekregistratie en controle| N/A | |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja |  |
