---
title: Algemene beveiligingskenmerken voor Azure Storage
description: Een controlelijst met algemene beveiligingskenmerken voor het evalueren van Azure Storage
services: storage
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: storage
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 33765d57f1a0e5788011b2d9d9c2f57d06713ddb
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680686"
---
# <a name="common-security-attributes-for-azure-storage"></a>Algemene beveiligingskenmerken voor Azure Storage

Beveiliging is geïntegreerd in elk aspect van een Azure-service. In dit artikel worden de algemene beveiligingskenmerken die zijn ingebouwd in Azure Storage. 

[!INCLUDE [Security Attributes Header](../../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja |  |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>Vnet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | Ondersteuning voor standaard HTTPS/TLS-mechanismen.  Gebruikers kunnen ook gegevens versleutelen voordat deze worden verzonden naar de service. |
| Versleuteling sleutel verwerken (CMK, BYOK, enz.)| Ja | Zie [Storage Service Encryption door de klant beheerde sleutels in Azure Key Vault](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Versleuteling op kolom (Azure-gegevensservices)| N/A |  |
| API-aanroepen die zijn versleuteld| Ja |  |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja |  |
| ondersteuning voor vNET-injectie| N/A |  |
| Netwerkisolatie / netwerkfunctie ondersteuning| Ja | |
| Ondersteuning voor geforceerde tunneling | N/A |  |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Azure Monitor-metrische gegevens beschikbaar zijn Logboeken vanaf preview nu |

## <a name="iam-support"></a>IAM-ondersteuning

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Toegangsbeheer - verificatie| Ja | Azure Active Directory, gedeelde sleutel, token voor gedeelde toegang. |
| Toegangsbeheer - autorisatie| Ja | Ondersteuning voor autorisatie via RBAC POSIX ACL's en SAS-Tokens |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Beheer/beheer plannen logboekregistratie en controle | Ja | Azure Resource Manager-activiteitenlogboek |
| Gegevens vlak logboekregistratie en controle| Ja | Diagnostische logboeken en vanaf preview logboekregistratie van Azure Monitor  |

## <a name="configuration-management"></a>Configuration Management

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Ondersteuning voor Resource Provider versiebeheer via Azure Resource Manager-API 's |