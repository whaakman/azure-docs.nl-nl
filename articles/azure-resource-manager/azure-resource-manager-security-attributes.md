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
ms.openlocfilehash: a771d4c2ae22b7bf149c13c80fe5286ef52a4545
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002259"
---
# <a name="security-attributes-for-azure-resource-manager"></a>Beveiligingskenmerken voor Azure Resource Manager

In dit artikel worden de beveiligingskenmerken gebouwd in Azure Resource Manager.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja |  |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | HTTPS/TLS. |
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
