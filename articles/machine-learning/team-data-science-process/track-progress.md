---
title: De uitvoering van de gegevens wetenschappelijke projecten - Azure Machine Learning | Microsoft Docs
description: Een wetenschappelijk gegevens kunt hoe de voortgang van een wetenschappelijke gegevensproject volgen.
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: deguhath
ms.openlocfilehash: ae0a32e15d86e8c2a9f8359a8caf1d205bd019f3
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="track-progress-of-data-science-projects"></a>Voortgang van de gegevens wetenschappelijke projecten bijhouden

Groepsbeheerders voor wetenschappelijke gegevens, team leads en project leads nodig voor het bijhouden van de voortgang van hun teamprojecten, welke werk is uitgevoerd op deze en door wie en blijven op de takenlijsten. 

## <a name="vsts-dashboards"></a>VSTS dashboards
Als u Visual Studio Team Services (VSTS) gebruikt, bent u kunnen worden opgebouwd dashboards om de activiteiten en de werkitems die zijn gekoppeld aan een bepaald project flexibele te houden. 

Zie de volgende sets met instructies voor meer informatie over het maken en aanpassen van dashboards en widgets in Visual Studio Team Services:

- [Toevoegen en beheren van dashboards](https://docs.microsoft.com/vsts/report/dashboards/dashboards)
- [Widgets toevoegen aan een dashboard](https://docs.microsoft.com/vsts/report/dashboards/add-widget-to-dashboard).

## <a name="example-dashboard"></a>Voorbeeld van dashboard

Hier volgt een eenvoudig voorbeeld dashboard die is ontwikkeld voor het bijhouden van de activiteiten sprint van een flexibele gegevens wetenschap-project, evenals het aantal doorvoeringen in de bijbehorende opslagplaatsen. De **linksboven** paneel wordt weergegeven:

- de aftelling van de huidige sprint 
- het aantal doorvoeringen voor elke opslagplaats in de afgelopen 7 dagen
- het werkitem voor specifieke gebruikers. 

De resterende panelen tonen de cumulatieve stroomdiagram (CFD), burndown en burnup voor een project:

- **Linksonder**: CFD de hoeveelheid werk in een bepaalde status goedgekeurde grijs weergegeven, vastgelegd in blauw en wordt uitgevoerd in het groen weergegeven.
- **Top rechts**: burndown grafiek het werk om te voltooien en de resterende tijd).
- **Rechtsonder**: burnup grafiek het werk dat ten opzichte van de totale hoeveelheid werk is voltooid.

![dashboard](./media/track-progress/dashboard.png)

Zie voor een beschrijving van het bouwen van deze grafieken, de snelstartgidsen en zelfstudies op [Dashboards](https://docs.microsoft.com/vsts/report/dashboards/).
 
## <a name="next-steps"></a>Volgende stappen

Scenario's die de stappen in het proces voor het demonstreren **specifieke scenario's** worden ook gegeven. Ze worden weergegeven en gekoppeld aan de miniatuur beschrijvingen in de [voorbeeld-scenario's](walkthroughs.md) artikel. Ze te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken. 
