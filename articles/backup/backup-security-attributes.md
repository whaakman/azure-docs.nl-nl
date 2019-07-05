---
title: Algemene beveiligingskenmerken voor Azure Backup
description: Een controlelijst met algemene beveiligingskenmerken voor het evalueren van Azure Backup
services: backup
author: utraghuv
manager: barbkess
ms.service: backup
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 595cc4eff70e2df4cb6d7f1d6a0c1a2748b34bf2
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565621"
---
# <a name="security-attributes-for-azure-backup"></a>Beveiligingskenmerken voor Azure Backup

In dit artikel worden de beveiligingskenmerken die is ingebouwd in Azure Backup. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja | Met behulp van storage service-versleuteling voor opslagaccounts. |
| Versleuteling tijdens overdracht:<ul><li>Express route-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Nee | Met behulp van HTTPS. |
| Versleuteling verwerking (CMK, BYOK, enz.)| Nee |  |
| Versleuteling op kolom (Azure Data Services)| Nee |  |
| API-aanroepen die zijn versleuteld| Ja |  |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Nee |  |
| Ondersteuning voor VNet-injectie| Nee |  |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja | Geforceerde tunnels wordt ondersteund voor VM-back-up. Geforceerde tunneling wordt niet ondersteund voor workloads die worden uitgevoerd in virtuele machines. |
| Geforceerde tunneling ondersteuning| Nee |  |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | Log Analytics wordt ondersteund via Logboeken met diagnostische gegevens. Zie [Monitor Azure Backup beveiligde werkbelastingen met behulp van Log Analytics](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) voor meer informatie. |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Verificatie is via Azure Active Directory. |
| Autorisatie| Ja | Klanten die zijn gemaakt en ingebouwde RBAC-rollen worden gebruikt. Zie [Use Role-Based toegangsbeheer voor het beheren van Azure Backup-herstelpunten](/azure/backup/backup-rbac-rs-vault) voor meer informatie. |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Alle acties van de klant geactiveerd vanuit Azure portal worden geregistreerd in de activiteitenlogboeken. |
| Gegevens vlak logboekregistratie en controle| Nee | Azure Backup-gegevenslaag kan niet rechtstreeks worden bereikt.  |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Ja|  |