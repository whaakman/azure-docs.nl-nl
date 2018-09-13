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
ms.openlocfilehash: 999ac78427ae1202db584efeb7de538bcc1248c7
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714799"
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Automatisch afstemmen in Azure SQL Database

Azure SQL Database automatisch afstemmen biedt prestaties en stabiele piekworkloads via continue prestaties afstemmen op basis van AI en machine learning.

Automatisch afstemmen is een volledig beheerde intelligente prestaties-service die gebruikmaakt van ingebouwde intelligentie voor het continu bewaken van query's uitgevoerd op een database en deze automatisch de prestaties worden verbeterd. Dit wordt bewerkstelligd via dynamisch aan te passen database naar de werkbelastingen te wijzigen en toepassen van aanbevelingen voor afstemming. Automatisch afstemmen horizontaal leert van alle databases op Azure via AI en dynamisch verbetert de acties. Hoe langer een Azure SQL Database wordt uitgevoerd met automatisch afstemmen op, hoe beter wordt uitgevoerd.

Azure SQL Database automatisch afstemmen is mogelijk een van de belangrijkste functies die u inschakelen kunt voor stabiele en hoog presterende workloads van databases.

## <a name="what-can-automatic-tuning-do-for-you"></a>Wat kunt u automatisch afstemmen doen?

- Prestaties automatisch afstemmen van Azure SQL-databases
- Automatische verificatie van de prestaties verbeteren
- Deze worden teruggedraaid omdat en zelf correctie
- Afstemmingsgeschiedenis
- Actie T-SQL-scripts voor handmatige implementaties afstemmen
- Proactieve werkbelasting prestatiebewaking
- De schaal vergroten mogelijkheid op honderden of duizenden databases
- Positief effect op de DevOps-resources en de totale eigendomskosten

## <a name="safe-reliable-and-proven"></a>Veilige, betrouwbare en bewezen

Afstemmen van bewerkingen die worden toegepast op Azure SQL-databases zijn volledig veilig is voor de prestaties van uw meest veeleisende workloads. Het systeem is ontworpen met zorg niet te leiden tot problemen met de werkbelasting van de gebruiker. Aanbevelingen voor automatische afstemming worden alleen op de tijden van een laag gebruik toegepast. Automatisch afstemmen bewerkingen voor het beveiligen van de prestaties van de werkbelastingen kunt ook tijdelijk uitschakelen door het systeem. In dit geval wordt "Uitgeschakeld door het systeem" bericht weergegeven in Azure portal. Automatisch afstemmen beschouwt workloads met de hoogste prioriteit voor de resource.

Mechanismen voor automatisch afstemmen zijn volwassen en zijn is perfect voor verschillende miljoen databases die worden uitgevoerd op Azure. Automatische afstemming bewerkingen toegepast worden automatisch gecontroleerd om ervoor te zorgen dat er een positieve verbetering in de prestaties van de werkbelastingen. Aanbevelingen voor verminderde prestaties zijn dynamisch gedetecteerd en onmiddellijk teruggedraaid. Via de afstemmen geschiedenis vastgelegd, bestaat er een duidelijke trace van verbeteringen aangebracht aan elke Azure SQL-Database afstemmen. 

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
 2. **DROP INDEX** -identificeert redundante en dubbele indexen per dag, met uitzondering van unieke indexen en indexen die gedurende een lange periode niet zijn gebruikt (> 90 dagen). Houd er rekening mee dat deze optie is niet compatibel is met toepassingen die gebruikmaken van partitie schakelen en de index-hints.
 3. **LAATSTE goede PLAN forceren** -SQL-query's met behulp van uitvoeringsplan die langzamer is dan de vorige goed plan en query's met behulp van de laatst bekende goede planning in plaats van de verminderde plan identificeert.

Automatisch afstemmen identificeert **CREATE INDEX**, **DROP INDEX**, en **FORCE laatste goede PLAN** aanbevelingen die u kunnen de databaseprestaties van uw te optimaliseren en waarin wordt getoond in [Azure-portal](sql-database-advisor-portal.md), en wordt aangegeven dat ze via [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current) en [REST-API](https://docs.microsoft.com/rest/api/sql/serverautomatictuning).

Aanbevelingen voor afstemming via de portal handmatig toepassen of u kunt automatisch afstemmen autonoom toepassen van aanbevelingen voor afstemming voor u. De voordelen van het systeem autonoom laten toepassen van aanbevelingen voor afstemming voor u is dat in een dergelijk geval automatisch valideert Er bestaat een positieve winst op de werkbelastingsprestaties of anders als er een regressie wordt gedetecteerd, wordt automatisch overgeschakeld de aanbeveling afstemmen. Houd er rekening mee dat in het geval van query's beïnvloed door de aanbevelingen die niet vaak worden uitgevoerd voor het afstemmen, de validatiefase maximaal 72 kunnen uur en standaard. Als u handmatig afstemmen toepast zijn aanbevelingen, de prestaties van de automatische validatie en terugboeking mechanismen niet beschikbaar.

Opties voor automatisch afstemmen kunnen onafhankelijk van elkaar zijn ingeschakeld of uitgeschakeld per database, of ze kunnen worden geconfigureerd op de logische servers en toegepast op elke database die u neemt instellingen over van de server. Logische servers kunnen standaardinstellingen van Azure voor de instellingen voor automatisch afstemmen overnemen. Standaardinstellingen van Azure op dit moment zijn ingesteld op FORCE_LAST_GOOD_PLAN is ingeschakeld, CREATE_INDEX is ingeschakeld en DROP_INDEX is uitgeschakeld.

Configureren van automatische afstemming van de opties op een server en -instellingen voor databases die behoren tot de bovenliggende server overnemen is een aanbevolen methode voor het configureren van automatisch afstemmen als het beheer van de opties voor automatisch afstemmen voor een groot aantal databases vereenvoudigt.

## <a name="next-steps"></a>Volgende stappen

- Inschakelen van automatisch afstemmen in Azure SQL Database voor het beheren van uw workload [automatisch instellen inschakelen](sql-database-automatic-tuning-enable.md).
- Zie voor het handmatig controleren en automatisch aanbevelingen voor het afstemmen van toepassing, [zoeken en toepassen van aanbevelingen voor prestaties](sql-database-advisor-portal.md).
- Zie voor meer informatie over het gebruik van T-SQL om te passen en automatisch afstemmen aanbevelingen weergeven, [beheren automatisch afstemmen via T-SQL](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/).
- Zie voor meer informatie over het bouwen van e-mailmeldingen voor aanbevelingen voor automatische afstemming, [e-mailmeldingen voor automatisch afstemmen](sql-database-automatic-tuning-email-notifications.md).
- Zie voor meer informatie over ingebouwde intelligentie die wordt gebruikt in het automatisch afstemmen, [kunstmatige intelligentie afgestemd Azure SQL-databases](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Zie voor meer informatie over de werking van automatische afstemming werkt in Azure SQL Database en SQL server 2017, [automatisch afstemmen van SQL Server](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).
