---
title: Beveiligings kenmerken voor Azure Backup
description: Een controle lijst met beveiligings kenmerken voor het evalueren van Azure Backup
author: utraghuv
manager: barbkess
ms.service: backup
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: c2500c6c9ff6882e521f4edce02426a92a0bd39f
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68464927"
---
# <a name="security-attributes-for-azure-backup"></a>Beveiligings kenmerken voor Azure Backup

In dit artikel worden de beveiligings kenmerken gedocumenteerd die zijn ingebouwd in Azure Backup. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest (zoals versleuteling aan server zijde, versleuteling aan server zijde met door de klant beheerde sleutels en andere versleutelings functies)| Ja | Storage-service versleuteling gebruiken voor opslag accounts. |
| Versleuteling in transit (zoals ExpressRoute-versleuteling, in VNet-versleuteling en VNet-VNet-versleuteling)| Nee | HTTPS gebruiken. |
| Verwerking van versleutelings sleutels (CMK, BYOK, enz.)| Nee |  |
| Versleuteling op kolom niveau (Azure Data Services)| Nee |  |
| Versleutelde API-aanroepen| Ja |  |

## <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Nee |  |
| Ondersteuning voor VNet-injectie| Nee |  |
| Ondersteuning voor netwerk isolatie en firewalling| Ja | Geforceerde tunneling wordt ondersteund voor VM-back-ups. Geforceerde tunneling wordt niet ondersteund voor werk belastingen die in Vm's worden uitgevoerd. |
| Ondersteuning voor geforceerde tunneling| Nee |  |

## <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking (log Analytics, app Insights, enz.)| Ja | Log Analytics wordt ondersteund via Diagnostische logboeken. Zie [Azure backup beveiligde werk belastingen bewaken met behulp van log Analytics](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) voor meer informatie. |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Verificatie is via Azure Active Directory. |
| Authorization| Ja | Door de klant gemaakte en ingebouwde RBAC-rollen worden gebruikt. Zie [Access Control op basis van rollen gebruiken om Azure backup herstel punten te beheren](/azure/backup/backup-rbac-rs-vault) voor meer informatie. |


## <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Alle door de klant geactiveerde acties van de Azure Portal worden vastgelegd in activiteiten Logboeken. |
| Logboek registratie en controle van het gegevens vlak| Nee | Azure Backup gegevens vlak kan niet rechtstreeks worden bereikt.  |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer (versie van configuratie, enz.)| Ja|  |