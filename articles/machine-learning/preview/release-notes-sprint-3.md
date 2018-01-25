---
title: Azure ML-Workbench release-opmerkingen voor sprint 3 januari 2018
description: In dit document worden de updates voor de sprint 3-release van Azure ML
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/22/2018
ms.openlocfilehash: f75fcec3b722563949b6553f17c4f3db3e223675
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="sprint-3---january-2018"></a>Sprint 3 - januari 2018 

#### <a name="version-number-01171218263"></a>Versienummer: 0.1.1712.18263

>Dit is hoe u kunt [Zoek het versienummer](https://docs.microsoft.com/en-us/azure/machine-learning/preview/known-issues-and-troubleshooting-guide).

Welkom bij de vierde update van Azure Machine Learning-Workbench. Hier volgen de updates en verbeteringen in deze sprint. Veel van deze updates worden gedaan als direct resultaat van feedback van gebruikers. 

## <a name="notable-new-features-and-changes"></a>Belangrijke nieuwe functies en wijzigingen
- Updates voor de verificatie-stack zorgt ervoor dat de selectie en -account voor aanmelding bij het opstarten

## <a name="detailed-updates"></a>Gedetailleerde Updates
Hier volgt een lijst met gedetailleerde updates voor elk onderdeel gebied in Azure Machine Learning in deze sprint.

### <a name="workbench"></a>Workbench
- Mogelijkheid om te installeren/verwijderen van de app vanuit programma's toevoegen/verwijderen
- Updates voor de verificatie-stack zorgt ervoor dat de selectie en -account voor aanmelding bij het opstarten
- Verbeterde ervaring voor eenmalige aanmelding op (SSO) in Windows
- Gebruikers die deel uitmaken van meerdere tenants met andere referenties zich nu aanmelden bij de Workbench

#### <a name="ui"></a>UI
- Algemene verbeteringen en oplossingen voor problemen

### <a name="notebooks"></a>Laptops
- Algemene verbeteringen en oplossingen voor problemen

### <a name="data-preparation"></a>Gegevensvoorbereiding 
- Verbeterde automatische suggesties tijdens het uitvoeren van transformaties door voorbeeld
- Verbeterde algoritme voor het patroon frequentie inspector
- Voorbeeldgegevens en feedback verzenden tijdens het uitvoeren van de voorbeeld-transformaties ![afbeelding van de koppeling van de feedback verzenden op afgeleid kolom transformatie](media/release-notes-sprint-3/SendFeedbackFromDeriveColumn.png)
- Spark Runtime-verbeteringen
- Scala is Pyspark vervangen
- Vaste onvermogen om de gegevens niet van toepassing sluiten voor de Time Series-Inspector 
- De tijd van de loopt vast voor de uitvoering van de gegevens Prep voor HDI opgelost

### <a name="model-management-cli-updates"></a>Model Management CLI-updates 
  - Eigendom van het abonnement is niet meer vereist voor het inrichten van resources. Inzender aan de resourcegroep worden voor toegang tot voldoende zijn voor het instellen van de implementatieomgeving.
  - Ingeschakelde lokale omgeving gratis abonnementen instellen 
