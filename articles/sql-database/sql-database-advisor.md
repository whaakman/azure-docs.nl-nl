---
title: Aanbevelingen voor prestaties - Azure SQL Database | Microsoft Docs
description: Azure SQL-Database bevat aanbevelingen voor uw SQL-databases die u kunnen de huidige queryprestaties verbeteren.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik
manager: craigg
ms.date: 12/19/2018
ms.openlocfilehash: 6dd404ec0ab9e88c9e5bc4af4b939b3aa1f2e675
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55560869"
---
# <a name="performance-recommendations-for-sql-database"></a>Aanbevelingen voor prestaties voor SQL-Database

Azure SQL Database leert herkennen en past zich aan met uw toepassing. Het biedt gepersonaliseerde aanbevelingen waarmee u kunt de prestaties van uw SQL-databases. SQL Database continu beoordeelt en analyseert de gebruiksgeschiedenis van uw SQL-databases. De aanbevelingen die worden verstrekt zijn gebaseerd op database-unieke werkbelasting patronen en prestaties te verbeteren.

> [!TIP]
> [Automatisch afstemmen](sql-database-automatic-tuning.md) is de aanbevolen methode om enkele van de meest voorkomende prestatieproblemen met de database automatisch afstemmen. [Query uitvoeren op prestatie-inzichten](sql-database-query-performance.md) is de aanbevolen methode voor basic Azure SQL Database-prestaties behoeften op het gebied. [Azure SQL Analytics](../azure-monitor/insights/azure-sql.md) is de aanbevolen methode voor geavanceerde bewaking van prestaties van de database op schaal, met ingebouwde intelligentie voor het oplossen van prestaties automatisch.
>

## <a name="create-index-recommendations"></a>Indexaanbevelingen maken
SQL Database continu bewaakt de query's die worden uitgevoerd en identificeert de indexen die de prestaties kunnen verbeteren. Nadat er onvoldoende vertrouwen is dat een bepaalde index ontbreekt, is een nieuwe **Create index** aanbeveling is gemaakt.

 Azure SQL Database bouwt vertrouwen door het schatten van de index via tijd brengen wilt de prestatieverbetering te bereiken. Afhankelijk van de geschatte prestatieverbetering te bereiken, worden aanbevelingen worden gecategoriseerd als hoog, Gemiddeld of laag. 

Indexen die zijn gemaakt met behulp van de aanbevelingen zijn altijd gemarkeerd als indexen automatisch is gemaakt. U kunt zien welke indexen worden automatisch gemaakt door te kijken naar de weergave sys.indexes. Automatisch gemaakte indexen blokkeren niet ALTER/RENAME-opdrachten. 

Als u probeert te verwijderen van de kolom met een automatisch gemaakte index erboven, wordt de opdracht wordt doorgegeven. De index automatisch is gemaakt is met de opdracht ook verwijderd. Gewone indexen blokkeren de opdracht ALTER/wijzigen in kolommen die zijn geïndexeerd.

Nadat de index maken aanbeveling is toegepast, worden de prestaties van de query's met de basislijnprestaties vergeleken in Azure SQL Database. Als de nieuwe index prestaties verbeterde, de aanbeveling is gemarkeerd als voltooid en het rapport op het milieu is beschikbaar. Als de index is niet de prestaties te verbeteren, is het automatisch teruggezet. SQL-Database maakt gebruik van dit proces om ervoor te zorgen dat de prestaties van de database voor het verbeteren van aanbevelingen.

Alle **index maken** aanbeveling heeft een back-off-beleid dat niet is toegestaan toepassen van de aanbeveling als het Resourcegebruik van een database of pool hoog is. Het beleid voor back-off houdt rekening CPU, gegevens-i/o-logboek-IO en beschikbare opslag. 

Als de CPU, gegevens-IO en logboek-IO hoger dan 80% in de vorige 30 minuten is, wordt de aanbeveling voor het maken van index uitgesteld. Als de beschikbare opslag dan 10% zijn zal nadat de index is gemaakt, wordt de aanbeveling krijgt een foutstatus. Als u na een paar dagen, automatisch afstemmen nog steeds ervan overtuigd is dat de index gemaakt worden kan, wordt het proces opnieuw wordt gestart. 

Dit proces wordt herhaald totdat er is onvoldoende opslagruimte beschikbaar om een index te maken, of de index wordt niet meer beschouwd als nuttig.

## <a name="drop-index-recommendations"></a>DROP indexaanbevelingen
Naast het detecteren van ontbrekende indexen, analyseert SQL-Database voortdurend de prestaties van bestaande indexen. Als een index niet gebruikt wordt, raadt Azure SQL Database te slepen en neerzetten. Verwijderen van een index wordt aanbevolen in twee gevallen:
* De index is een duplicaat van een andere index (dezelfde geïndexeerd en opgenomen kolom, partitie-schema en filters).
* De index is niet gebruikt voor een langere periode (93 dagen).

De controle na implementatie doorlopen DROP indexaanbevelingen ook. Als de prestaties worden verbeterd, vindt u het rapport op. Als de prestaties minder, wordt de aanbeveling is hersteld.


## <a name="parameterize-queries-recommendations"></a>Aanbevelingen voor query's parameteriseren
*Query's parameteriseren* aanbevelingen worden weergegeven wanneer u een of meer query's die zijn voortdurend wordt gecompileerd, maar het einde van met het plan voor uitvoering van dezelfde query. Deze voorwaarde maakt een kans om toe te passen geforceerde parameterisering. Geforceerde parameterisering kan op zijn beurt queryplannen worden in de cache opgeslagen en opnieuw in de toekomst worden gebruikt, die zorgt voor betere prestaties en vermindert Resourcegebruik. 

Elke query die wordt uitgegeven voor de SQL Server in eerste instantie moet worden gecompileerd voor het genereren van een planning worden uitgevoerd. Elke gegenereerde plan wordt toegevoegd aan de plancache. Volgende uitvoeringen van dezelfde query opnieuw kunnen gebruiken voor dit plan is uit de cache, die de noodzaak voor het compileren van extra elimineert. 

Query's met niet-geparametriseerde waarden kunnen leiden tot prestatieoverhead omdat het abonnement kan worden uitgevoerd telkens wanneer de waarden niet-geparametriseerde verschillend zijn is gecompileerd. In veel gevallen de dezelfde query's met andere parameterwaarden genereren van de dezelfde plannen voor uitvoering. Deze abonnementen worden echter nog steeds afzonderlijk toegevoegd aan de plancache. 

Het proces voor het compileren van uitvoering maakt gebruik van de database-resources, verhoogt de query duur en buiten de plancache. Deze gebeurtenissen, dat op zijn beurt plannen uit de cache worden verwijderd. Dit gedrag SQL Server kan worden gewijzigd door de optie voor geforceerde parameterisering van de database. 

Als u wilt, kunt u de gevolgen van deze aanbeveling wordt u hebt opgegeven met een vergelijking tussen de werkelijke CPU-gebruik en het verwachte CPU-gebruik (zoals als de aanbeveling zijn toegepast). Aan de hand van deze aanbeveling kunt u CPU-besparingen krijgen. Zo kunt u minder queryduur van de en overhead voor de plancache, wat betekent dat meer van de abonnementen in de cache blijven kan en opnieuw worden gebruikt. U kunt deze aanbeveling snel toepassen door het selecteren van de **toepassen** opdracht. 

Nadat u deze aanbeveling toepast, kan deze geforceerde parameterisering binnen enkele minuten in uw database. Er wordt gestart met het bewakingsproces ongeveer 24 uur duurt. Als deze tijd is verstreken, kunt u het rapport zien. Dit rapport bevat het CPU-gebruik van uw database 24 uur vóór en na de aanbeveling is toegepast. SQL Database Advisor is een beveiligingsmechanisme dat de toegepaste aanbeveling automatisch worden hersteld als prestaties regressie is gedetecteerd.

## <a name="fix-schema-issues-recommendations-preview"></a>Aanbevelingen voor problemen met schema (preview) oplossen

> [!IMPORTANT]
> Microsoft is momenteel niet meer ondersteund 'Schemaprobleem op te lossen' aanbevelingen. Het is raadzaam dat u [Intelligent Insights](sql-database-intelligent-insights.md) voor het bewaken van uw prestatieproblemen met de database, met inbegrip van schema-problemen die de aanbevelingen 'Schemaprobleem op te lossen' eerder besproken.
> 

**Schemaproblemen oplossen** aanbevelingen worden weergegeven wanneer de service SQL Database meldingen een anomalie in het nummer van de schema-fouten met betrekking tot SQL die op uw SQL-database plaatsvinden. Deze aanbeveling wordt doorgaans weergegeven wanneer de database meerdere fouten over schema (ongeldige kolomnaam, Ongeldige objectnaam, enzovoort) binnen een uur.

'Problemen met het schema' vormen een klasse van syntaxisfouten in SQL Server. Ze zich voordoen wanneer de definitie van de SQL-query en de definitie van schema van de database niet uitgelijnd. Bijvoorbeeld, een van de kolommen die wordt verwacht door de query mogelijk ontbreekt in de doeltabel of vice versa. 

De aanbeveling voor 'Schemaprobleem op te lossen' wordt weergegeven wanneer de service Azure SQL Database meldingen een anomalie in het nummer van de schema-fouten met betrekking tot SQL die op uw SQL-database plaatsvinden. De volgende tabel ziet u de fouten die zijn gerelateerd aan schema problemen:

| SQL-foutcode | Bericht |
| --- | --- |
| 201 |Procedure of functie '*'wordt verwacht dat de parameter'*', die niet is opgegeven. |
| 207 |Ongeldige kolomnaam ' *'. |
| 208 |Ongeldige objectnaam ' *'. |
| 213 |Kolomnaam of -nummer van opgegeven waarden komt niet overeen met tabeldefinitie. |
| 2812 |Kan de opgeslagen procedure niet vinden ' *'. |
| 8144 |Procedure of functie * zijn te veel argumenten opgegeven. |

## <a name="next-steps"></a>Volgende stappen
Bewaak uw aanbevelingen en doorgaan om toe te passen om prestaties te zetten. Databaseworkloads zijn dynamisch en veranderen voortdurend. SQL Database Advisor blijft bewaakt en doet aanbevelingen die u kunnen mogelijk de verbeteren de prestaties van uw database. 

* Zie voor meer informatie over automatisch afstemmen van de database-indexen en query-uitvoering plannen [Azure SQL Database automatisch afstemmen](sql-database-automatic-tuning.md).
* Zie voor meer informatie over het controleren van de prestaties van de database met geautomatiseerde diagnostische gegevens en analyses van hoofdoorzaken van problemen met prestaties automatisch [Intelligent Insights van Azure SQL](sql-database-intelligent-insights.md).
*  Zie voor meer informatie over het gebruik van aanbevelingen voor prestaties in Azure portal [aanbevelingen voor prestaties in Azure portal](sql-database-advisor-portal.md).
* Zie [Query prestatie-inzichten](sql-database-query-performance.md) en meer informatie over de invloed op de prestaties van uw belangrijkste query's weergeven.


