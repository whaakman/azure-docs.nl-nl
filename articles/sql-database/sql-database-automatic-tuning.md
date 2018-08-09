---
title: Azure SQL Database - functie automatisch afstemmen | Microsoft Docs
description: Azure SQL-Database SQL-query analyseert en automatisch wordt aangepast aan de werkbelasting voor gebruikers.
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: v-daljep
ms.reviewer: carlrab
ms.openlocfilehash: dd6e8f5f46e9fdf6887cc2a0b0c7b15bbd00fabd
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39626196"
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Automatisch afstemmen in Azure SQL Database

Azure SQL Database automatisch afstemmen biedt piekprestaties en stabiele workloads via continue prestaties afstemmen met behulp van kunstmatige intelligentie.

Automatisch afstemmen is een volledig beheerde service die gebruikmaakt van ingebouwde intelligentie voor het continu bewaken van query's uitgevoerd op een database en deze automatisch de prestaties worden verbeterd. Dit wordt bewerkstelligd via dynamisch aan te passen database naar de werkbelastingen te wijzigen en toepassen van aanbevelingen voor afstemming. Automatisch afstemmen horizontaal leert van alle databases op Azure via kunstmatige intelligentie en dynamisch verbetert de acties. Hoe langer een Azure SQL Database wordt uitgevoerd met automatisch afstemmen op, hoe beter wordt uitgevoerd.

Azure SQL Database automatisch afstemmen is mogelijk een van de belangrijkste functies die u inschakelen kunt voor stabiele en hoog presterende werkbelastingen.

## <a name="what-can-automatic-tuning-do-for-you"></a>Wat kunt u automatisch afstemmen doen?

- Prestaties automatisch afstemmen van Azure SQL-Databases
- Automatische verificatie van de prestaties verbeteren
- Deze worden teruggedraaid omdat en zelf correctie
- Geschiedenislogboek afstemmen
- Actie T-SQL-scripts voor handmatige implementaties afstemmen
- Proactieve werkbelasting prestatiebewaking
- De schaal vergroten mogelijkheid op honderden of duizenden databases
- Positief effect op de DevOps-resources en de totale eigendomskosten

## <a name="safe-reliable-and-proven"></a>Veilige, betrouwbare en bewezen

Afstemmen bewerkingen die worden toegepast op Azure SQL-Databases zijn volledig veilig is voor de prestaties van uw meest veeleisende workloads. Het systeem is ontworpen met zorg niet te leiden tot problemen met de werkbelasting van de gebruiker. Aanbevelingen voor automatische afstemming worden alleen op de tijden van een laag gebruik toegepast. Automatisch afstemmen bewerkingen voor het beveiligen van de prestaties van de werkbelastingen kunt ook tijdelijk uitschakelen door het systeem. In dit geval wordt "Uitgeschakeld door het systeem" bericht weergegeven in Azure portal. Automatisch afstemmen beschouwt workloads met de hoogste prioriteit voor de resource.

Mechanismen voor automatisch afstemmen zijn volwassen en zijn is perfect op honderden of duizenden databases in Azure worden uitgevoerd. Automatische afstemming bewerkingen toegepast worden automatisch gecontroleerd om ervoor te zorgen dat er een positieve verbetering in de prestaties van de werkbelastingen. Aanbevelingen voor verminderde prestaties zijn dynamisch gedetecteerd en onmiddellijk teruggedraaid. Via het afstemmen geschiedenislogboek is er duidelijk tracering voor het afstemmen van verbeteringen aangebracht aan elke Azure SQL-Database. 

![Hoe werkt automatisch afstemmen](./media/sql-database-automatic-tuning/how-does-automatic-tuning-work.png)

Azure SQL Database automatisch afstemmen, is de kernlogica delen met de SQL Server engine voor automatisch afstemmen. Zie voor meer technische informatie over het mechanisme voor ingebouwde intelligentie, [automatisch afstemmen van SQL Server](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).

## <a name="use-automatic-tuning"></a>Gebruik van automatisch afstemmen

Automatisch afstemmen moet handmatig worden ingeschakeld voor uw abonnement. Inschakelen van automatisch afstemmen met behulp van Azure portal [automatisch instellen inschakelen](sql-database-automatic-tuning-enable.md).

Automatisch afstemmen kunt autonoom werken via automatisch toepassen van aanbevelingen voor afstemming, met inbegrip van geautomatiseerde verificatie van de prestaties verbeteren. 

Voor meer controle wilt, automatische toepassing van aanbevelingen voor het afstemmen kan worden uitgeschakeld en aanbevelingen voor het afstemmen kunt handmatig toepassen via Azure portal. Het is ook mogelijk met gebruik van de oplossing automatisch aanbevelingen voor afstemming alleen weergeven en ze handmatig toepassen via scripts en hulpprogramma's van uw keuze. 

Zie de ingesloten video voor een overzicht van de werking van automatische afstemming werkt en voor typische gebruiksscenario's:


> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Improve-Azure-SQL-Database-Performance-with-Automatic-Tuning/player]
>

## <a name="automatic-tuning-options"></a>Opties voor automatisch afstemmen

Opties voor automatisch afstemmen in Azure SQL Database beschikbaar zijn:
 1. **CREATE INDEX** -identificeert indexen die prestaties van uw workload kunnen verbeteren, indexen en wordt automatisch gecontroleerd dat de prestaties van query's zijn verbeterd.
 2. **DROP INDEX** -redundant en dubbele Hiermee worden indexen geïdentificeerd die niet zijn gebruikt voor een lange periode-indexen. Houd er rekening mee dat deze optie is niet compatibel is met toepassingen die gebruikmaken van partitie schakelen en de index-hints.
 3. **LAATSTE goede PLAN forceren** -SQL-query's met behulp van uitvoeringsplan die langzamer is dan de vorige goed plan en query's met behulp van de laatst bekende goede planning in plaats van de verminderde plan identificeert.

Azure SQL Database identificeert **CREATE INDEX**, **DROP INDEX**, en **FORCE laatste goede PLAN** aanbevelingen die u kunnen uw database te optimaliseren en geeft ze in Azure portal. Meer informatie over de identificatie van indexen die moeten worden gewijzigd op [indexaanbevelingen te vinden in Azure portal](sql-database-advisor-portal.md). Aanbevelingen met behulp van de portal handmatig toepassen of u kunt laten Azure SQL Database voor het automatisch toepassen van aanbevelingen, workload controleren na de wijziging te controleren of dat de aanbeveling voor de prestaties van uw workload verbeterd. 

Opties voor automatisch afstemmen kunnen onafhankelijk van elkaar zijn ingeschakeld of uitgeschakeld per database, of ze kunnen worden geconfigureerd op de logische servers en toegepast op elke database die u neemt instellingen over van de server. Logische servers kunnen standaardinstellingen van Azure voor de instellingen voor automatisch afstemmen overnemen. Standaardinstellingen van Azure op dit moment zijn ingesteld op FORCE_LAST_GOOD_PLAN is ingeschakeld, CREATE_INDEX is ingeschakeld en DROP_INDEX is uitgeschakeld.

Configureren van automatische afstemming van de opties op een server en -instellingen voor databases die behoren tot de bovenliggende server overnemen is een aanbevolen methode voor het configureren van automatisch afstemmen als het beheer van de opties voor automatisch afstemmen voor een groot aantal databases vereenvoudigt.

## <a name="next-steps"></a>Volgende stappen

- Inschakelen van automatisch afstemmen in Azure SQL Database voor het beheren van uw workload [automatisch instellen inschakelen](sql-database-automatic-tuning-enable.md).
- Zie voor het handmatig controleren en automatisch aanbevelingen voor het afstemmen van toepassing, [zoeken en toepassen van aanbevelingen voor prestaties](sql-database-advisor-portal.md).
- Zie voor meer informatie over het bouwen van e-mailmeldingen voor aanbevelingen voor automatische afstemming, [e-mailmeldingen voor automatisch afstemmen](sql-database-automatic-tuning-email-notifications.md).
- Zie voor meer informatie over ingebouwde intelligentie die wordt gebruikt in het automatisch afstemmen, [kunstmatige intelligentie afgestemd Azure SQL-Databases](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Zie voor meer informatie over de werking van automatische afstemming werkt in Azure SQL Database en SQL server 2017, [automatisch afstemmen van SQL Server](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).
