---
title: De voortgang van de data science-projecten - Team Data Science Process
description: Hoe data science groepsbeheerders, lease-team en leads project kunnen de voortgang van een data science-project te volgen.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/28/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: a9616a4f80a3105118b82ce3f4106a65fdd9ddfe
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53134298"
---
# <a name="tracking-the-progress-of-data-science-projects"></a>De voortgang van de data science-projecten

Groepsbeheerders voor Data science, team leads en project leads nodig om de voortgang van hun projecten te houden, welke werk is uitgevoerd op deze en door wie en blijft op de to-do-lijsten. 

## <a name="azure-devops-dashboards"></a>Azure DevOps-dashboards
Als u van Azure DevOps gebruikmaakt, bent u kunt dashboards om bij te houden van de activiteiten en de werkitems die zijn gekoppeld aan een bepaald flexibele project bouwen. 

Zie voor meer informatie over het maken en aanpassen van dashboards en widgets voor Azure DevOps, de volgende sets met instructies:

- [Toevoegen en beheren van dashboards](https://docs.microsoft.com/azure/devops/report/dashboards/dashboards)
- [Widgets toevoegen aan een dashboard](https://docs.microsoft.com/azure/devops/report/dashboards/add-widget-to-dashboard).

## <a name="example-dashboard"></a>Voorbeeld van dashboard

Hier volgt een eenvoudig voorbeeld-dashboard dat is gebouwd voor het volgen van de sprint activiteiten van een flexibele data science-project, evenals het aantal doorvoeracties naar gekoppelde opslagplaatsen. De **linksboven** deelvenster wordt weergegeven:

- het aftellen van de huidige sprint 
- het aantal doorvoeringen voor elke opslagplaats in de afgelopen 7 dagen
- het werkitem voor specifieke gebruikers. 

De resterende panelen weergeven de cumulatieve stroomdiagram (CFD), burndown en burnup voor een project:

- **Links onderin**: CFD de hoeveelheid werk in een bepaalde status, goedgekeurde grijs weergegeven, vastgelegd in het blauw en wordt uitgevoerd in het groen weergegeven.
- **Rechts boven**: burndown grafiek het werk om te voltooien en de resterende tijd).
- **Onderaan, rechts**: burnup grafiek het werk dat ten opzichte van de totale hoeveelheid werk is voltooid.

![dashboard](./media/track-progress/dashboard.png)

Zie voor een beschrijving van het bouwen van deze grafieken, de snelstartgidsen en zelfstudies op [Dashboards](https://docs.microsoft.com/azure/devops/report/dashboards/).
 
## <a name="next-steps"></a>Volgende stappen

Scenario's die laten zien van alle de stappen in het proces voor het **specifieke scenario's** worden ook gegeven. Ze worden weergegeven en die is gekoppeld met miniaturen beschrijvingen in de [voorbeeld walkthroughs](walkthroughs.md) artikel. Ze laten zien hoe u naar de cloud, on-premises hulpprogramma's en services combineren in een werkstroom of een pijplijn te maken van een intelligente toepassingen. 
