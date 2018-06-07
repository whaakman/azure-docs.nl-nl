---
title: Azure SQL Database - automatische afstemming | Microsoft Docs
description: Azure SQL Database SQL-query analyseert en automatisch wordt aangepast aan de werkbelasting van de gebruiker.
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: jovanpop
ms.openlocfilehash: bef8d01bd4c220fac595177089088ff64ee3bc3b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34646640"
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Automatische afstemming in Azure SQL Database

Azure SQL Database automatische afstemming biedt optimale prestaties en stabiele werkbelastingen via continue prestaties afstemmen met behulp van de kunstmatige Intelligence.

Automatische afstemming is een volledig beheerde service die gebruikmaakt van ingebouwde intelligentie continu query's die worden uitgevoerd op een database bewaken en automatisch verbetert de prestaties. Dit wordt bewerkstelligd met dynamisch aanpassing van de database naar de werkbelastingen wijzigen en toepassen van aanbevelingen afstemmen. Automatische afstemming horizontaal leert van alle databases op Azure via kunstmatige Intelligence en het bijbehorende afstemmen acties dynamisch verbetert. Hoe langer een Azure SQL Database met automatische afstemming op wordt uitgevoerd, hoe beter worden uitgevoerd.

Azure SQL Database automatische afstemming is mogelijk een van de belangrijkste functies die u inschakelen kunt om te bieden stabiel en piek werkbelastingen uitvoeren.

## <a name="what-can-automatic-tuning-do-for-you"></a>Hoe automatische afstemming kan voor u?

- Geautomatiseerde prestaties afstemmen van Azure SQL-Databases
- Automatische verificatie van de prestaties verbeteren
- Geautomatiseerde terugdraaien en zelf correctie
- Geschiedenislogboek afstemmen
- Actie T-SQL-scripts voor handmatige implementaties afstemmen
- Proactieve werkbelasting prestatiebewaking
- Mogelijkheid op honderden of duizenden databases uitbreiden
- Positieve invloed op de DevOps-resources en de totale eigendomskosten

## <a name="safe-reliable-and-proven"></a>Veilige, betrouwbare en beproefde

Prestatieafstemming bewerkingen die worden toegepast op Azure SQL-Databases zijn volledig veilig voor de prestaties van uw meest intensieve werkbelastingen. Het systeem is niet op de werkbelasting van de gebruiker verstoren zorgvuldig ontworpen. Geautomatiseerde afstemmen aanbevelingen worden alleen toegepast tijdens de tijd van een laag gebruik. Automatische afstemmen operations ter bescherming van de prestaties van de workload kan ook tijdelijk uitschakelen door het systeem. In dit geval wordt 'Uitgeschakeld door het systeem' bericht weergegeven in Azure-portal. Automatische afstemming beschouwt werkbelastingen met de hoogste prioriteit voor de resource.

Mechanismen voor automatische afstemmen zijn volwassen en hebben is rechtsgevolg heeft op honderden of duizenden databases die worden uitgevoerd op Azure. Automatische afstemmen bewerkingen toegepast worden automatisch gecontroleerd om te controleren of dat er is een positieve verbetering van de prestaties van de workload. Prestaties opgelost aanbevelingen zijn dynamisch gedetecteerd en onmiddellijk ongedaan gemaakt. Via het afstemmen geschiedenislogboek is er een duidelijke trace van het afstemmen van verbeteringen van elke Azure SQL-Database. 

![Hoe werkt automatische afstemmen](./media/sql-database-automatic-tuning/how-does-automatic-tuning-work.png)

Azure SQL Database automatische afstemming, wordt de corelogica delen met de automatische afstemmen engine van SQL Server. Zie voor aanvullende technische informatie over de ingebouwde intelligentie wijze [automatische afstemming van SQL Server](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).

## <a name="use-automatic-tuning"></a>Gebruik automatische afstemming

Automatische afstemming moet handmatig worden ingeschakeld voor uw abonnement. Zie inschakelen van automatische afstemming met Azure portal [inschakelen automatische afstemming](sql-database-automatic-tuning-enable.md).

Automatische afstemming kan autonoom werken via het automatisch toepassen van aanbevelingen voor afstemmen, met inbegrip van automatische verificatie van de prestaties verbeteren. 

Automatische toepassing van aanbevelingen afstemmen kan worden uitgeschakeld voor meer controle en aanbevelingen afstemmen kan worden handmatig toegepast via Azure portal. Het is ook mogelijk om te gebruiken van de oplossing geautomatiseerde afstemmen aanbevelingen alleen weergeven en deze handmatig toepassen via scripts en hulpprogramma's van uw keuze. 

Zie voor een overzicht van de werking van automatische afstemmen werkt en typische gebruiksscenario's de ingesloten video:


> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Improve-Azure-SQL-Database-Performance-with-Automatic-Tuning/player]
>

## <a name="automatic-tuning-options"></a>Opties voor automatische afstemmen

Opties voor automatische afstemmen in Azure SQL Database beschikbaar zijn:
 1. **CREATE INDEX** -indexen die de prestaties van uw werkbelasting mogelijk verbeterd, maakt indexen en automatisch verifieert dat de prestaties van query's is verbeterd identificeert.
 2. **DROP INDEX** -identificeert overbodige en dubbele indexen en indexen die niet zijn gebruikt voor een heel lange periode. Opmerking: deze optie is niet compatibel met toepassingen die gebruikmaken van partitie schakelen en de index-hints.
 3. **LAATSTE goed PLAN FORCE** -SQL-query's met behulp van uitvoeringsplan die lager ligt dan de vorige goed plan en query's met de laatste bekende goede planning in plaats van het plan regressed identificeert.

Azure SQL Database identificeert **CREATE INDEX**, **DROP INDEX**, en **FORCE laatste goede plannen** aanbevelingen die u kunnen de database te optimaliseren en wordt deze weergegeven in Azure-portal. Meer informatie over de identificatie van de indexen die moeten worden gewijzigd op [indexaanbevelingen vinden in Azure portal](sql-database-advisor-portal.md). Met behulp van de portal aanbevelingen handmatig toepassen of Azure SQL Database voor het automatisch toepassen van aanbevelingen, werkbelasting bewaken na herstart van de wijziging laten en verifiëren dat de aanbeveling de prestaties van uw werkbelasting verbeterd. 

Opties voor automatische afstemmen kunnen onafhankelijk zijn ingeschakeld of uitgeschakeld per database, of ze kunnen worden geconfigureerd op de logische servers en toegepast op elke database die u neemt instellingen over van de server. Logische servers kunnen Azure standaardwaarden voor instellingen voor automatische afstemmen overnemen. Azure standaardwaarden op dit moment zijn ingesteld voor FORCE_LAST_GOOD_PLAN is ingeschakeld, CREATE_INDEX is ingeschakeld en DROP_INDEX is uitgeschakeld.

Configureren van automatische afstemming van de opties op een server en -instellingen voor databases die horen bij de bovenliggende server overnemen is een aanbevolen methode voor het configureren van automatische afstemming zoals vereenvoudigt het beheer van de opties voor automatische afstemmen voor een groot aantal databases.

## <a name="next-steps"></a>Volgende stappen

- Zie inschakelen van automatische afstemming in Azure SQL Database voor het beheren van uw werkbelasting [inschakelen automatische afstemming](sql-database-automatic-tuning-enable.md).
- Handmatig wilt bekijken en toepassen van automatische afstemming van aanbevelingen, Zie [zoeken en toepassen van aanbevelingen voor prestaties](sql-database-advisor-portal.md).
- Zie voor meer informatie over het bouwen van e-mailmeldingen voor automatische afstemmen aanbevelingen, [e-mailmeldingen voor automatische afstemming](sql-database-automatic-tuning-email-notifications.md)
- Zie voor meer informatie over ingebouwde intelligentie die worden gebruikt in automatische afstemming, [kunstmatige Intelligence verbetert de prestatie Azure SQL-Databases](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Zie voor meer informatie over hoe automatische afstemmen werkt in Azure SQL Database en SQL server 2017, [automatische afstemming van SQL Server](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).
