---
title: Beveiligings kenmerken voor Azure SQL Database
description: Een controle lijst met beveiligings kenmerken voor het evalueren van Azure SQL Database
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: 1318b3e433224b009b76458b12e82c9bcf94bb7a
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444395"
---
# <a name="security-attributes-for-azure-sql-database"></a>Beveiligings kenmerken voor Azure SQL Database

In dit artikel worden de beveiligings kenmerken gedocumenteerd die in Azure SQL Database zijn ingebouwd.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

SQL Database bevatten zowel [één data base](sql-database-single-index.yml) als een [beheerd exemplaar](sql-database-managed-instance.md). De volgende vermeldingen zijn van toepassing op beide aanbiedingen, tenzij anders vermeld.

## <a name="preventative"></a>Preventie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Versleuteling bij rest:<ul><li>Versleuteling aan de serverzijde</li><li>Versleuteling aan de server zijde met door de klant beheerde sleutels</li><li>Andere versleutelings functies, zoals client zijde of Always Encrypted</ul>| Ja | Wordt ' versleuteling-in-gebruik ' genoemd, zoals beschreven in het artikel [Always encrypted](sql-database-always-encrypted.md). Versleuteling aan de server zijde maakt gebruik van [transparante gegevens versleuteling](transparent-data-encryption-azure-sql.md).|
| Versleuteling in transit:<ul><li>Azure ExpressRoute-versleuteling</li><li>Versleuteling in een virtueel netwerk</li><li>Versleuteling tussen virtuele netwerken</ul>| Ja | HTTPS gebruiken. |
| Versleutelings verwerking, zoals CMK of BYOK| Ja | Door service beheerde en door de klant beheerde sleutel verwerking worden aangeboden. Deze laatste wordt aangeboden via [Azure Key Vault](../key-vault/index.yml). |
| Versleuteling op kolom niveau van Azure Data Services| Ja | Via [Always encrypted](sql-database-always-encrypted.md). |
| Versleutelde API-aanroepen| Ja | HTTPS/SSL gebruiken. |

## <a name="network-segmentation"></a>Netwerk segmentatie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen |
|---|---|--|
| Ondersteuning voor service-eind punten| Ja | Is alleen van toepassing op [één data base](sql-database-single-index.yml) . |
| Ondersteuning voor Azure Virtual Network injectie| Ja | Is alleen van toepassing op een [beheerd exemplaar](sql-database-managed-instance.md) . |
| Netwerk isolatie en firewall ondersteuning| Ja | Firewall op database niveau en op server niveau. Netwerk isolatie geldt alleen voor [beheerde exemplaren](sql-database-managed-instance.md) . |
| Ondersteuning voor geforceerde tunneling| Ja | [Beheerde instantie](sql-database-managed-instance.md) via een [ExpressRoute](../expressroute/index.yml) -VPN. |

## <a name="detection"></a>Detectie

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor Azure-bewaking, zoals Log Analytics of Application Insights| Ja | SecureSphere, de SIEM-oplossing van Imperva, wordt ook ondersteund via de integratie van [Azure Event hubs](../event-hubs/index.yml) via [SQL auditing](sql-database-auditing.md). |

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Authentication| Ja | Azure Active Directory (Azure AD) |
| Authorization| Ja | Geen |

## <a name="audit-trail"></a>Audittrail

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Logboek registratie en controle op het vlak van controle en beheer| Ja | Ja voor sommige gebeurtenissen alleen |
| Logboek registratie en controle op gegevens vlak | Ja | Via [SQL audit](sql-database-auditing.md) |

## <a name="configuration-management"></a>Configuratiebeheer

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Ondersteuning voor configuratie beheer, zoals versie beheer van de configuratie| Nee  | Geen |

## <a name="additional-security-attributes-for-sql-database"></a>Aanvullende beveiligings kenmerken voor SQL Database

| Beveiligings kenmerk | Ja/Nee | Opmerkingen|
|---|---|--|
| Preventieve: evaluatie van beveiligings problemen | Ja | Zie [SQL-evaluatie service voor beveiligings problemen helpt u bij het identificeren van een database kwets](sql-vulnerability-assessment.md)baarheid. |
| Preventief: gegevens detectie en-classificatie  | Ja | Zie [Azure SQL database en SQL Data Warehouse gegevens detectie & classificatie](sql-database-data-discovery-and-classification.md). |
| Detectie: detectie van bedreigingen | Ja | Zie [Advanced Threat Protection voor Azure SQL database](sql-database-threat-detection-overview.md). |
