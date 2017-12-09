---
title: Azure SQL Database - automatische afstemming | Microsoft Docs
description: Azure SQL Database SQL-query analyseert en automatisch wordt aangepast aan de werkbelasting van de gebruiker.
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: drasumic
editor: danimir
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 11/08/2017
ms.author: jovanpop
ms.openlocfilehash: 50d1548637ecc6862f680f73e333e8d7f8d20e39
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
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
 1. **CREATE INDEX** die identificeert de indexen die de prestaties van uw werkbelasting mogelijk verbeterd, maakt de indexen en controleert of dat ze de prestaties van de query's verbeteren.
 2. **DROP INDEX** die redundante en dubbele indexen en indexen die niet werden gebruikt in de lange periode identificeert.
 3. **LAATSTE goed PLAN FORCE** SQL-query's die gebruikmaken van uitvoeringsplan die lager is dan de vorige goed plan identificeert en maakt gebruik van de laatste bekende goede planning in plaats van het regressed plan.

Azure SQL Database identificeert **CREATE INDEX**, **DROP INDEX**, en **FORCE laatste goede plannen** aanbevelingen die u kunnen de database te optimaliseren en wordt deze weergegeven in Azure-portal. Meer informatie over de identificatie van de indexen die moeten worden gewijzigd op [indexaanbevelingen vinden in Azure portal](sql-database-advisor-portal.md). Met behulp van de portal aanbevelingen handmatig toepassen of Azure SQL Database voor het automatisch toepassen van aanbevelingen, werkbelasting bewaken na herstart van de wijziging laten en verifiëren dat de aanbeveling de prestaties van uw werkbelasting verbeterd.

Automatische afstemming van opties kan worden afzonderlijk ingeschakeld of uitgeschakeld per database, of ze kunnen worden geconfigureerd op de logische server en toegepast op elke database die u neemt instellingen over van de server. Configureren van automatische afstemming van de opties op de server en overnemen van de instellingen van de databases op de server, wordt aanbevolen methode voor het configureren van automatische afstemming omdat het beheer van de opties voor automatische afstemmen op een groot aantal databases vereenvoudigt.

## <a name="next-steps"></a>Volgende stappen

- Zie inschakelen van automatische afstemming in Azure SQL Database voor het beheren van uw werkbelasting [inschakelen automatische afstemming](sql-database-automatic-tuning-enable.md).
- Handmatig wilt bekijken en toepassen van automatische afstemming van aanbevelingen, Zie [zoeken en toepassen van aanbevelingen voor prestaties](sql-database-advisor-portal.md).
- Zie voor meer informatie over ingebouwde intelligentie die worden gebruikt in automatische afstemming, [kunstmatige Intelligence verbetert de prestatie Azure SQL-Databases](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Zie voor meer informatie over hoe automatische afstemmen werkt in Azure SQL Database en SQL server 2017 [automatische afstemming van SQL Server](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).
