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
ms.openlocfilehash: 71e87a3295a9cd73dca2f97b3fa04a5d5ff76f84
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798694"
---
# <a name="security-attributes-for-azure-sql-database"></a>Beveiligingskenmerken voor Azure SQL Database

In dit artikel worden de algemene beveiligingskenmerken die zijn ingebouwd in Azure SQL Database.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

SQL Database voorziet in beide [individuele database](sql-database-single-index.yml) en [beheerd exemplaar](sql-database-managed-instance.md). De volgende vermeldingen zijn van toepassing op beide aanbiedingen, behalve indien anders vermeld.

## <a name="preventative"></a>Preventative

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling-at-rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling op de server met de klant beheerde sleutels</li><li>Andere versleutelingsfuncties, zoals client- of Always Encrypted</ul>| Ja | Met de naam 'versleuteling in-gebruik,' zoals beschreven in het artikel [Always Encrypted](sql-database-always-encrypted.md). Maakt gebruik van server-side encryption [transparante gegevensversleuteling](transparent-data-encryption-azure-sql.md).|
| Versleuteling tijdens overdracht:<ul><li>Azure ExpressRoute-versleuteling</li><li>Versleuteling in een virtueel netwerk</li><li>Versleuteling tussen virtuele netwerken</ul>| Ja | Met behulp van HTTPS. |
| Versleutelingssleutel verwerken, zoals CMK of BYOK| Ja | Verwerking van zowel service beheerd door de klant beheerde sleutels worden aangeboden. De laatste wordt aangeboden via [Azure Key Vault](../key-vault/index.yml). |
| Versleuteling op kolomniveau wordt geleverd door Azure-gegevensservices| Ja | Via [Always Encrypted](sql-database-always-encrypted.md). |
| Versleutelde API-aanroepen| Ja | Met behulp van HTTPS/SSL. |

## <a name="network-segmentation"></a>Segmentatie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eindpunt| Ja | Is van toepassing op [individuele database](sql-database-single-index.yml) alleen. |
| Ondersteuning van Azure Virtual Network-injectie| Ja | Is van toepassing op [beheerd exemplaar](sql-database-managed-instance.md) alleen. |
| Netwerkisolatie en firewall-ondersteuning| Ja | Firewallregels op databaseniveau en serverniveau. Netwerkisolatie is bedoeld voor [beheerd exemplaar](sql-database-managed-instance.md) alleen. |
| Geforceerde tunneling ondersteuning| Ja | [Beheerd exemplaar](sql-database-managed-instance.md) via een [ExpressRoute](../expressroute/index.yml) VPN. |

## <a name="detection"></a>Detectie

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Azure-ondersteuning, zoals Log Analytics of Application Insights bewaken| Ja | SecureSphere, de SIEM-oplossing van Imperva, wordt ook ondersteund via [Azure Event Hubs](../event-hubs/index.yml) integratie via [controleren voor SQL](sql-database-auditing.md). |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Azure Active Directory (Azure AD) |
| Authorization| Ja | Geen |

## <a name="audit-trail"></a>Audittrail

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Controlevlak en het beheervlak logboekregistratie en controle| Ja | Ja voor sommige gebeurtenissen alleen |
| Data plane-logboekregistratie en controle | Ja | Via [SQL-controle](sql-database-auditing.md) |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Configuratiebeheer ondersteuning, zoals versiebeheer van configuratie| Nee  | Geen |

## <a name="additional-security-attributes-for-sql-database"></a>Kenmerken voor extra beveiliging voor SQL-Database

| Beveiligingskenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Preventative: evaluatie van beveiligingsproblemen | Ja | Zie [evaluatie van beveiligingsproblemen SQL service kunt u identificeren databaseproblemen](sql-vulnerability-assessment.md). |
| Voorkomen: gegevensdetectie en classificatie  | Ja | Zie [Azure SQL Database en SQL Data Warehouse gegevensdetectie en classificatie](sql-database-data-discovery-and-classification.md). |
| Detectie: detectie van bedreigingen | Ja | Zie [Advanced Threat Protection voor Azure SQL Database](sql-database-threat-detection-overview.md). |
