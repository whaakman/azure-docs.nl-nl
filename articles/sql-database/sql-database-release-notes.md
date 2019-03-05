---
title: Opmerkingen bij de Release van de Azure SQL Database | Microsoft Docs
description: Meer informatie over de nieuwe functies en verbeteringen in de Azure SQL Database-service en in de documentatie van Azure SQL Database
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.subservice: service
ms.devlang: ''
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: carlrab
ms.openlocfilehash: af0fd591393f9f187a75f79fea980d41085df5cb
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57342442"
---
# <a name="sql-database-release-notes"></a>Opmerkingen bij de release van SQL-Database

In dit artikel bevat de nieuwe functies en verbeteringen in de service SQL Database en in de documentatie van SQL-Database. Zie voor verbeteringen aan andere Azure-services, [Service-updates](https://azure.microsoft.com/updates).

## <a name="march-2019"></a>Maart 2019

### <a name="service-improvements"></a>Verbeteringen van de services

| Verbeteringen van de services | Details |
| --- | --- |

### <a name="documentation-improvements"></a>Verbeteringen in de documentatie bij

| Verbeteringen in de documentatie bij | Details |
| --- | --- |
| Toegevoegde log-limieten voor individuele databases|Zie voor meer informatie, [Single database vCore-resourcebeperkingen](sql-database-vcore-resource-limits-single-databases.md)|
| Toegevoegde log-limieten voor elastische pools en gepoolde databases|Zie voor meer informatie, [vCore resourcelimieten elastische pools](sql-database-vcore-resource-limits-elastic-pools.md)|
| Toegevoegde Transaction log tarief governance| Nieuwe inhoud toegevoegd [Transaction log tarief governance](sql-database-resource-limits-database-server.md#transaction-log-rate-governance)|
| Bijgewerkte PowerShell-voorbeelden voor individuele databases en elastische pools az.sql modules gebruiken | Zie voor meer informatie, [PowerShell-voorbeelden voor individuele databases en elastische pools](sql-database-powershell-samples.md#single-database-and-elastic-pools).

## <a name="february-2019"></a>Februari 2019

### <a name="service-improvements"></a>Verbeteringen van de services

| Verbeteringen van de services | Details |
| --- | --- |
|Het maken van een hervatbare index online is nu algemeen beschikbaar| Zie voor meer informatie, [Index maken](https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql)|
|Ondersteuning voor beheerd exemplaar voor routetabellen verbeterd| Zie voor meer informatie, [vereisten](sql-database-managed-instance-connectivity-architecture.md#network-requirements)|

### <a name="documentation-improvements"></a>Verbeteringen in de documentatie bij

| Verbeteringen in de documentatie bij | Details |
| --- | --- |
|Toevoegen voor het beheerde exemplaar implementatie optie verduidelijkingen|Groot aantal artikelen voor het verduidelijken van toepasselijkheid op individuele databases, elastische pool en implementatieopties voor beheerd exemplaar bijgewerkt |
|Bijgewerkte tempdb-grootten voor DTU gebaseerde aankoopmodel | Zie voor meer informatie, [Tempdb-database in SQL-Database](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database?view=sql-server-2017#tempdb-database-in-sql-database)|
|Bijgewerkte importeren en exporteren met bacpac-bestand voor de ondersteuning van het beheerde exemplaar| Zie voor meer informatie, [importeren vanuit BACPAC](sql-database-import.md) en [exporteren naar BACPAC](sql-database-export.md) |

## <a name="january-2019"></a>Januari 2019

### <a name="service-improvements"></a>Verbeteringen van de services

| Verbeteringen van de services | Details |
| --- | --- |
| Als u meer granulatie opties voor compute-resources | Servicelagen voor algemeen gebruik en bedrijfskritiek [enkelvoudige databases](sql-database-vcore-resource-limits-single-databases.md) en [elastische pools](sql-database-vcore-resource-limits-elastic-pools.md) beschikt nu over meer fijnmazig compute-opties.|
| Controlerecords bekijken voor het beheerde exemplaar in de Azure-portal | Weergeven van [controlerecords voor beheerde exemplaren](sql-database-managed-instance-auditing.md) in Azure portal nu wordt ondersteund.|
| Geavanceerde threat detectiefunctie gewijzigd in de geavanceerde beveiliging van gegevens | Geavanceerde threat detectiefunctie gewijzigd in [gegevensbeveiliging geavanceerde](sql-advanced-threat-protection.md) voor individuele databases, elastische pools en beheerde exemplaren |
| &nbsp; |

### <a name="documentation-improvements"></a>Verbeteringen in de documentatie bij

| Verbeteringen in de documentatie bij | Details |
| --- | --- |
| Beheerde exemplaren en transactionele replicatie | Toegevoegde artikel over het gebruik van [transactionele replicatie met beheerde exemplaren](replication-with-sql-database-managed-instance.md) |
| Extra Azure AD met beheerd exemplaar zelfstudie | Dit [Azure AD met beheerd exemplaar](sql-database-managed-instance-aad-security-tutorial.md) zelfstudie ziet u moet configureren en testen beheerd exemplaar security met behulp van Azure AD-aanmeldingen. |
| Bijgewerkte inhoud voor het automatiseren van de taak met behulp van Transact-SQL-scripts | Bijgewerkt en er wordt duidelijk gemaakt inhoud voor het gebruik van [taak automatisering met behulp van Transact-SQL-scripts](sql-database-job-automation-overview.md) voor individuele databases, elastische pools en beheerde exemplaren |
| Inhoud van de beveiliging voor beheerde exemplaren bijgewerkt | Bijgewerkt en er wordt duidelijk gemaakt inhoud voor de [beveiligingsmodel voor beheerde exemplaren](sql-database-security-overview.md), en verschillen met het beveiligingsmodel voor individuele databases en elastische pools |
| Vernieuwd alle snelstartgidsen en zelfstudies | Alle snelstartgidsen en zelfstudies in de [documentatie](https://docs.microsoft.com/azure/sql-database) zijn bijgewerkt en vernieuwd zodat deze overeenkomen met wijzigingen in de Azure-portal |
| Overzicht van toegevoegde snelstartgidsen | Een overzicht van Snelstartgids voor toegevoegd [enkelvoudige databases](sql-database-quickstart-guide.md) en voor [instanties die worden beheerd](sql-database-managed-instance-quickstart-guide.md) |
| Toegevoegde SQL-Database verklarende woordenlijst | Dit [verklarende woordenlijst](sql-database-glossary-terms.md) artikel bevat een definitieve lijst met de bepalingen van de SQL-Database en koppelingen naar de primaire conceptuele pagina waarin wordt uitgelegd van de term in context. |
| &nbsp; |

## <a name="contribute-to-content-improvement"></a>Bijdragen aan inhoud verbetering

De Azure SQL-documentatieset is open-source. In het openbaar werken biedt verschillende voordelen:

- Opensourceopslagplaatsen planning in om op te halen van feedback over welke documenten het meest nodig zijn zijn.
- Opensourceopslagplaatsen in de open voor het publiceren van de inhoud van het nuttigst op onze eerste release.
- Open-source-opslagplaatsen bijwerken in de geopend zodat u gemakkelijk het continu verbeteren van de inhoud.

Als u wilt bijdragen aan inhoud in de documentatie bij Azure SQL Database, Zie de [Microsoft-Docs Inzender handleiding overzicht](https://docs.microsoft.com/en-us/contribute/). De gebruikerservaring op [docs.microsoft.com](https://docs.microsoft.com/) integreert [GitHub](https://github.com/) werkstromen rechtstreeks aan u nog gemakkelijker. Begin met [bewerken van het document dat u bekijkt](https://docs.microsoft.com/en-us/contribute/#quick-edits-to-existing-documents). Of u kunt helpen door [nieuwe onderwerpen te beoordelen](https://docs.microsoft.com/en-us/contribute/#review-open-prs), of [kwaliteitsproblemen maken](https://docs.microsoft.com/en-us/contribute/#create-quality-issues).
