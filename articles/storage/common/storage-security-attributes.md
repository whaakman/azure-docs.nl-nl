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
ms.openlocfilehash: 922273e3805004f6af068ea748c16f5675810144
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66001466"
---
# <a name="security-attributes-for-azure-storage"></a>Beveiligingskenmerken voor Azure Storage

In dit artikel worden de beveiligingskenmerken die is ingebouwd in Azure Storage. 

[!INCLUDE [Security Attributes Header](../../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja |  |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | Ondersteuning voor standaard HTTPS/TLS-mechanismen.  Gebruikers kunnen ook gegevens versleutelen voordat deze worden verzonden naar de service. |
| Versleuteling verwerking (CMK, BYOK, enz.)| Ja | Zie [Storage Service Encryption door de klant beheerde sleutels in Azure Key Vault](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Versleuteling op kolom (Azure Data Services)| N/A |  |
| API-aanroepen die zijn versleuteld| Ja |  |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja |  |
| Ondersteuning voor VNet-injectie| N/A |  |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja | |
| Geforceerde tunneling ondersteuning| N/A |  |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Azure Monitor-metrische gegevens beschikbaar zijn Logboeken vanaf preview nu |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Azure Active Directory, gedeelde sleutel, token voor gedeelde toegang. |
| Autorisatie| Ja | Ondersteuning voor autorisatie via RBAC POSIX ACL's en SAS-Tokens |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle | Ja | Azure Resource Manager-activiteitenlogboek |
| Gegevens vlak logboekregistratie en controle| Ja | Diagnostische logboeken en vanaf preview logboekregistratie van Azure Monitor  |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja | Ondersteuning voor Resource Provider versiebeheer via Azure Resource Manager-API 's |