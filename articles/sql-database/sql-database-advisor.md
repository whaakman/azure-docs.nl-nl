---
title: Prestaties aanbevelingen - Azure SQL Database | Microsoft Docs
description: Azure SQL-Database bevat aanbevelingen voor de SQL-databases die u kunnen de huidige queryprestaties verbeteren.
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 4cee9fd9072e2db9a5f90a84b7683a76bba2504d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34644015"
---
# <a name="performance-recommendations-for-sql-database"></a>Aanbevelingen voor prestaties voor SQL-Database

Azure SQL Database leert en aanpast aan uw toepassing. Het biedt aangepaste aanbevelingen waarmee u kunt de prestaties van uw SQL-databases. SQL-Database voortdurend evalueert en analyseert de gebruiksgeschiedenis van uw SQL-databases. De aanbevelingen die worden verstrekt zijn gebaseerd op database-unieke werkbelasting patronen en prestaties te verbeteren.

> [!TIP]
> [Automatische afstemming](sql-database-automatic-tuning.md) is de aanbevolen methode voor het afstemmen van prestaties. [Intelligent Insights](sql-database-intelligent-insights.md) is de aanbevolen methode voor het bewaken van de prestaties. 
>

## <a name="create-index-recommendations"></a>Indexaanbevelingen maken
SQL-Database voortdurend bewaakt de query's die worden uitgevoerd en identificeert de indexen die de prestaties kunnen verbeteren. Nadat er voldoende vertrouwen dat een bepaalde index ontbreekt, is een nieuwe **Create index** aanbeveling wordt gemaakt.

 Azure SQL Database bouwt vertrouwen door het schatten van de prestatieverbetering die de index via tijd zou doen. Afhankelijk van de geschatte prestatieverbetering te bereiken, aanbevelingen gecategoriseerd als hoog, Gemiddeld of laag. 

Indexen die zijn gemaakt met behulp van de aanbevelingen zijn altijd gemarkeerd als indexen automatisch gemaakt. U kunt zien welke indexen worden automatisch gemaakt door te kijken naar de weergave sys.indexes. Automatisch gemaakte indexen blokkeren niet ALTER/RENAME-opdrachten. 

Als u probeert te verwijderen van de kolom met een index gemaakt met een automatisch overnemen, wordt de opdracht doorgegeven. De index automatisch gemaakt is met de opdracht ook verwijderd. Gewone indexen blokkeren de opdracht ALTER/RENAME op kolommen die zijn geïndexeerd.

Nadat de indexaanbeveling maken is toegepast, worden de prestaties van de query's met de basislijn vergeleken in Azure SQL Database. Als de nieuwe index prestaties verbeterde, de aanbeveling is gemarkeerd als voltooid en het rapport impact is beschikbaar. Als de index niet de prestaties te verbeteren, is het automatisch hersteld. SQL-Database maakt gebruik van dit proces om ervoor te zorgen dat de prestaties van de database voor het verbeteren van aanbevelingen.

Alle **index maken** aanbeveling heeft een back-off-beleid dat het toepassen van de aanbeveling als hoog is het brongebruik van een database of de groep niet is toegestaan. De back-uitgeschakeld beleid houdt account CPU, IO-gegevens, i/o-logboek en beschikbare opslag. 

Als de CPU, IO-gegevens of i/o-logboek hoger dan 80% in de vorige 30 minuten is, wordt de indexaanbeveling maken uitgesteld. Als de beschikbare opslag dan 10% zijn nadat de index is gemaakt, wordt de aanbeveling verplaatst naar een foutstatus. Als u na een paar dagen, automatische afstemming nog steeds gelooft dat de index gemaakt worden kan, wordt het proces opnieuw wordt gestart. 

Dit proces wordt herhaald totdat er is onvoldoende opslagruimte beschikbaar maken van een index of totdat de index wordt niet beschouwd als nuttige meer.

## <a name="drop-index-recommendations"></a>Aanbevelingen voor indexen verwijderen
Naast het herkennen van ontbrekende indexen, analyseert de SQL-Database voortdurend de prestaties van de bestaande indexen. Als er een index wordt niet gebruikt, Azure SQL Database het beste neer te zetten. Verwijderen van een index wordt in beide gevallen aangeraden:
* De index is een duplicaat van een andere index (dezelfde geïndexeerd en opgenomen kolom, partitie-schema en filters).
* De index is niet gebruikt voor een langere periode (93 dagen).

Aanbevelingen voor indexen verwijderen worden ook de controle na implementatie doorlopen. Als de prestaties beter, is het rapport impact beschikbaar. Als de prestaties afnemen, wordt de aanbeveling hersteld.


## <a name="parameterize-queries-recommendations"></a>Aanbevelingen voor query's voorzien
*Query's voorzien* aanbevelingen worden weergegeven wanneer u een of meer query's die zijn voortdurend wordt gecompileerd maar end hebt met hetzelfde abonnement voor de query kan worden uitgevoerd. Deze voorwaarde maakt een kans om toe te passen geforceerde parameterisering. Geforceerde parameterisering kan op zijn beurt queryplannen worden in de cache opgeslagen en opnieuw in de toekomst worden gebruikt die zorgt voor betere prestaties en vermindert Resourcegebruik. 

Elke query die wordt verleend op basis van SQL Server in eerste instantie moet worden gecompileerd voor het genereren van een plan kan worden uitgevoerd. Elk gegenereerde plan is toegevoegd aan het cachegeheugen plan. Bij een volgende uitvoering van dezelfde query opnieuw kunnen gebruiken voor dit plan uit de cache, zodat de aanvullende compilatie. 

Query's met niet-geparametriseerde waarden kunnen leiden tot prestatieoverhead omdat het uitvoeringsplan telkens wanneer de niet-geparametriseerde waarden verschillend zijn is gecompileerd. In veel gevallen genereren de dezelfde query's met verschillende parameterwaarden de dezelfde uitvoering plannen. Deze plannen, zijn echter nog steeds afzonderlijk toegevoegd aan het cachegeheugen plan. 

Het proces voor het compileren van uitvoering plannen database bronnen worden gebruikt, verhoogt de query opgegeven tijdsduur en loopt over de plancache. Deze gebeurtenissen veroorzaken op zijn beurt plannen om te worden verwijderd uit de cache. Dit gedrag SQL Server kan worden gewijzigd door de optie geforceerde parameterisering op de database. 

Als u de gevolgen van deze aanbeveling schatten, kunt worden u geleverd bij een vergelijking tussen de werkelijke CPU-gebruik en het verwachte CPU-gebruik (alsof de aanbeveling zijn toegepast). Deze aanbeveling kunt u krijgen van CPU-gebruik. Zo kunt u query duur verkorten en overhead voor de plancache, wat betekent dat meer van de abonnementen in de cache blijven kunt en opnieuw worden gebruikt. U kunt deze aanbeveling snel toepassen door het selecteren van de **toepassen** opdracht. 

Nadat u deze aanbeveling toegepast, kunnen de geforceerde parameterisering binnen enkele minuten op uw database. Het bewakingsproces ongeveer 24 uur duurt starten Na deze periode ziet u het validatierapport. Dit rapport geeft het CPU-gebruik van uw database 24 uur voordat en nadat de aanbeveling is toegepast. SQL Database Advisor is een beveiligingsmechanisme dat de toegepaste aanbeveling automatisch teruggezet of prestaties regressie is gedetecteerd.

## <a name="fix-schema-issues-recommendations-preview"></a>Los problemen met aanbevelingen voor schema (preview)

> [!IMPORTANT]
> Microsoft is momenteel het 'Schema probleem op te lossen' aanbevelingen. Het is raadzaam dat u [Intelligent Insights](sql-database-intelligent-insights.md) voor het bewaken van uw prestatieproblemen met de database, inclusief schema problemen die de aanbevelingen 'Schema probleem op te lossen' eerder besproken.
> 

**Los de problemen schema** aanbevelingen worden weergegeven wanneer de service SQL Database meldingen een afwijkingsdetectie in het nummer van de schema-gerelateerde SQL fouten die op de SQL-database plaatsvinden. Deze aanbeveling wordt doorgaans weergegeven wanneer de database meerdere fouten over schema (ongeldige kolomnaam, Ongeldige objectnaam, enzovoort) binnen een uur optreedt.

'Problemen met het schema' vormen een klasse syntaxisfouten in SQL Server. Ze optreden wanneer de definitie van de SQL-query en de definitie van het databaseschema niet zijn uitgelijnd. Bijvoorbeeld, een van de kolommen die door de query wordt verwacht mogelijk ontbreken in de doeltabel of vice versa. 

De aanbeveling 'Schema probleem op te lossen' wordt weergegeven wanneer de service Azure SQL Database meldingen een afwijkingsdetectie in het nummer van de schema-gerelateerde SQL fouten die op de SQL-database plaatsvinden. De volgende tabel ziet u de fouten die betrekking op schema problemen hebben:

| SQL-foutcode | Bericht |
| --- | --- |
| 201 |Procedure of functie '*'verwacht dat de parameter'*', die niet is opgegeven. |
| 207 |Ongeldige kolomnaam ' *'. |
| 208 |Ongeldige objectnaam ' *'. |
| 213 |Kolomnaam of -nummer van opgegeven waarden komt niet overeen met tabeldefinitie. |
| 2812 |Kan de opgeslagen procedure niet vinden ' *'. |
| 8144 |Procedure of functie * heeft te veel argumenten opgegeven. |

## <a name="next-steps"></a>Volgende stappen
Bewaken van uw aanbevelingen en blijven toepassen om de prestaties te verfijnen. Database-werkbelastingen zijn dynamisch en continu wijzigen. SQL Database Advisor blijft om te controleren en aanbevelingen die potentieel prestaties van uw database kunt verbeteren. 

* Zie voor meer informatie over automatische afstemming van de database-indexen en queryplannen uitvoering [automatische afstemming van Azure SQL Database](sql-database-automatic-tuning.md).
* Zie voor meer informatie over het controleren van de prestaties van de database met geautomatiseerde diagnostische gegevens en analyse van de hoofdmap oorzaak van prestatieproblemen automatisch [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md).
*  Zie voor meer informatie over het gebruik van prestaties aanbevelingen in de Azure portal [prestaties aanbevelingen in de Azure portal](sql-database-advisor-portal.md).
* Zie [inzichten voor queryprestaties](sql-database-query-performance.md) leren kennen en de invloed op de prestaties van uw eerste query's weergeven.


