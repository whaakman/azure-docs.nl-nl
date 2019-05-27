---
title: Beveiligingskenmerken voor Azure SQL Database
description: Een controlelijst met beveiligingsinstellingen voor het evalueren van Azure SQL Database
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: 6c495456a5a3295abe5460ff6b5586e41fab2d95
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66001043"
---
# <a name="security-attributes-for-azure-sql-database"></a>Beveiligingskenmerken voor Azure SQL Database

In dit artikel worden de algemene beveiligingskenmerken die zijn ingebouwd in Azure SQL Database.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

Azure SQL Database voorziet in beide [individuele database](sql-database-single-index.yml) en [beheerd exemplaar](sql-database-managed-instance.md). De onderstaande vermeldingen van toepassing op beide aanbiedingen, behalve indien anders vermeld.

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties (zoals client-side altijd versleuteld, enz.)</ul>| Ja | Aangeduid als 'versleuteling in-gebruik', zoals beschreven in het artikel [Always Encrypted](sql-database-always-encrypted.md). Versleuteling aan de serverzijde van de service maakt gebruik van [transparante gegevensversleuteling](transparent-data-encryption-azure-sql.md) (TDE).|
| Versleuteling tijdens overdracht:<ul><li>ExpressRoute-versleuteling</li><li>VNet-versleuteling</li><li>VNet-VNet-versleuteling</ul>| Ja | Met behulp van HTTPS. |
| Versleuteling verwerking (CMK, BYOK, enz.)| Ja | Verwerking van zowel service beheerd door de klant beheerde sleutels worden aangeboden (de laatste via [Azure Key Vault](../key-vault/index.yml). |
| Versleuteling op kolom (Azure Data Services)| Ja | Via [Always Encrypted](sql-database-always-encrypted.md). |
| API-aanroepen die zijn versleuteld| Ja | Met behulp van HTTPS/SSL. |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja | Is van toepassing op [individuele database](sql-database-single-index.yml) alleen. |
| Ondersteuning voor VNet-injectie| Ja | Is van toepassing op [beheerd exemplaar](sql-database-managed-instance.md) alleen. |
| Netwerkisolatie en ondersteuning netwerkfunctie| Ja | Firewall op beide database - en serverniveau; netwerkisolatie voor [beheerd exemplaar](sql-database-managed-instance.md) alleen |
| Geforceerde tunneling ondersteuning| Ja | [beheerd exemplaar](sql-database-managed-instance.md) via [Azure ExpressRoute](../expressroute/index.yml) VPN |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure monitoring ondersteuning (Log analytics, Application insights, enz.)| Ja | De SIEM-oplossing van derden van Imperva (SecureSphere) wordt ook ondersteund, via [Azure Event Hubs](../event-hubs/index.yml) integratie via [SQL audit](sql-database-auditing.md). |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Verificatie| Ja | Azure Active Directory. |
| Autorisatie| Ja |  |


## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controle en beheer vlak logboekregistratie en controle| Ja | Ja voor alleen bepaalde gebeurtenissen. |
| Gegevens vlak logboekregistratie en controle | Ja | Via [SQL audit](sql-database-auditing.md). |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuration management-ondersteuning (versiebeheer van de configuratie, enz.)| Nee  | | 

## <a name="additional-security-attributes-for-sql-database"></a>Kenmerken voor extra beveiliging voor SQL-Database

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Preventative: evaluatie van beveiligingsproblemen | Ja | Zie [evaluatie van beveiligingsproblemen SQL service kunt u identificeren databaseproblemen](sql-vulnerability-assessment.md). |
| Voorkomen: gegevensdetectie en classificatie  | Ja | Zie [Azure SQL Database en SQL Data Warehouse gegevensdetectie en classificatie](sql-database-data-discovery-and-classification.md). |
| Detectie: detectie van bedreigingen | Ja | Zie [Advanced Threat Protection voor Azure SQL Database](sql-database-threat-detection-overview.md). |
