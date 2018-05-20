---
title: Overzicht van Azure DevOps Project | Microsoft Docs
description: Ontdek de waarde van Azure DevOps Project
services: devops-project
documentationcenter: ''
author: mlearned
manager: douge
editor: mlearned
ms.assetid: ''
ms.service: devops-project
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 05/03/2018
ms.author: mlearned
ms.openlocfilehash: b87cb14fc707d14638c2d6a52af6f295276a2279
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="overview-of-azure-devops-project"></a>Overzicht van Azure DevOps Project

Azure DevOps Project zorgt ervoor dat u eenvoudig aan de slag kunt met Azure. Met de DevOps-projectresource kunt u met enkele eenvoudige stappen vanuit Azure Portal uw favoriete type app starten in de Azure-service van uw keuze. DevOps Project stelt alles in wat u nodig hebt voor het ontwikkelen, implementeren en controleren van uw toepassing.
Met het DevOps Project-dashboard kunt u het doorvoeren, compileren en implementeren van code controleren, allemaal vanuit één weergave in Azure Portal.

## <a name="why-should-i-use-the-azure-devops-project"></a>Waarom zou ik Azure DevOps Project gebruiken?

Azure DevOps Project automatiseert de configuratie van een volledige pijplijn voor continue integratie (CI) en continue levering (CD) naar Azure.  U kunt uw bestaande code als uitgangspunt nemen of een van de aangeboden voorbeeldtoepassingen gebruiken om deze vervolgens snel in verschillende Azure-services te implementeren, zoals in Virtual Machines, App Service, Azure Container Service, Azure SQL Database en Azure Service Fabric.  

Het Azure DevOps-project verricht alle taken voor de eerste configuratie van een DevOps-pijplijn, met inbegrip van het instellen van de initiële Git-opslagplaats, het configureren van de CI/CD-pijplijn, het maken van een Application Insights-resource voor controledoeleinden en het genereren van één enkele weergave van de complete oplossing via een Azure DevOps Project-dashboard in Azure Portal.

U kunt Azure DevOps Project gebruiken voor het volgende:

* Snel een toepassing implementeren in Azure
* De installatie van een VSTS CI/CD-pijplijn automatiseren
* Het DevOps-project als sjabloon gebruiken om te zien en begrijpen hoe u CI/CD naar Azure correct instelt met VSTS
* Leren werken met een CI/CD-pijplijn naar Azure, om de release-pijplijn vervolgens verder aan te passen op basis van uw specifieke scenario's

## <a name="how-do-i-use-the-azure-devops-project"></a>Hoe gebruik ik Azure DevOps Project?

Azure DevOps Project is beschikbaar via Azure Portal.  U maakt een Azure-DevOps-projectresource in de portal, net zoals u een gewone Azure-resource maakt.  Het DevOps-project werkt zoals een wizard, wat inhoudt dat de verschillende configuratieopties achtereenvolgens worden aangeboden.  

U kiest een aantal configuratieopties om de eerste installatie uit te voeren.  Het gaat om deze opties:

* Gebruik de bijgeleverde voorbeeld-app of uw eigen code
* Selecteer een taal voor de app
* Kies een app-framework op basis van de taal
* Selecteer een Azure-service (implementatiedoel)
* VSTS-account (nieuw of bestaand)
* Kies uw Azure-abonnement
* Selecteer de locatie van Azure-services
* Kies uit verschillende prijscategorieën voor Azure-services

Nadat u Azure DevOps Project hebt gebruikt, kunt u alle resources ook vanaf één locatie verwijderen, namelijk vanuit het Azure DevOps Project-dashboard in Azure Portal.

## <a name="azure-devops-project-and-vsts-integration"></a>Azure DevOps Project en VSTS-integratie

DevOps-projecten maken gebruik van VSTS.  Het DevOps-project automatiseert alle taken die nodig zijn in VSTS om CI/CD naar Azure in te stellen.  Er wordt een Git-opslagplaats gemaakt in een nieuw of bestaand VSTS-account.  Het DevOps-project slaat een voorbeeldtoepassing of uw bestaande code op in een nieuwe Git-opslagplaats.  Tijdens de automatisering wordt ook een CI-trigger voor de build gemaakt, zodat elke nieuwe codedoorvoer een build tot gevolg heeft.  Het DevOps-project maakt ook een CD-trigger en implementeert elke nieuwe geslaagde build in de Azure-service van uw keuze.  De build- en releasedefinities kunnen worden aangepast voor aanvullende scenario's.  U kunt de build- en releasedefinities ook klonen voor gebruik in andere projecten.

Nadat het DevOps-project is gemaakt, kunt u het volgende doen:

* De build- en releasepijplijn aanpassen
* Met behulp van pull-aanvragen de codestroom beheren en een hoge kwaliteit garanderen
* Elke doorvoer (commit) testen en compileren voordat u de code samenvoegt om de kwaliteit nog verder te verbeteren
* De achterstand en problemen van het project samen met de toepassing controleren

## <a name="how-do-i-start-using-the-azure-devops-project"></a>Hoe ga ik aan de slag met Azure DevOps Project?

* [Aan de slag met Azure DevOps Project](https://docs.microsoft.com/vsts/build-release/actions/azure-devops-project-github) (Engelstalig)

## <a name="azure-devops-project-videos"></a>Video's over Azure DevOps Project

* [CI/CD maken met Azure DevOps Project](https://channel9.msdn.com/Events/Connect/2017/T174/player/) (Engelstalig)
