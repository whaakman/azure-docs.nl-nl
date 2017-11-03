---
title: Azure ML-Workbench release-opmerkingen voor sprint 0 oktober 2017
description: In dit document worden de updates voor de release sprint 0 van Azure ML
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 10/12/2017
ms.openlocfilehash: 37e0a4461e8a0de631a6194a1eb8cc16b610954f
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2017
---
# <a name="sprint-0---october-2017"></a>Sprint 0 - oktober 2017 

**Versienummer: 0.1.1710.04013**

Welkom bij de eerste update van Azure Machine Learning Workbench na de initiële openbare preview op de Conferentie Microsoft Ignite 2017. De belangrijkste updates in deze release zijn de betrouwbaarheid en stabilization worden opgelost.  Enkele van de kritieke problemen die we behandeld:

## <a name="new-features"></a>Nieuwe functies
- Mac OS hoge Sierra wordt nu ondersteund.

## <a name="bug-fixes"></a>Oplossingen voor problemen
### <a name="workbench-experience"></a>Workbench ervaring
- Slepen en neerzetten is een bestand in de Workbench zorgt ervoor dat de Workbench vastloopt.
- Het terminalvenster in VS-Code is geconfigureerd als een IDE voor Workbench worden niet herkend door _az ml_ opdrachten.

### <a name="workbench-authentication"></a>Workbench verificatie
We een aantal updates voor het verbeteren van verschillende aanmelding en verificatie problemen die worden gerapporteerd aangebracht.
- Verificatievenster houdt verschijnen-up, met name wanneer de verbinding met Internet is niet stabiel is.
- Problemen met verbeterde betrouwbaarheid rond verificatietokens.
- In sommige gevallen wordt verificatievenster tweemaal verschijnt.
- Hoofdvenster Workbench nog steeds weergegeven 'verificatie' bericht wanneer het verificatieproces daadwerkelijk is voltooid en het pop-updialoogvenster al gesloten.
- Als er geen verbinding met Internet, verschijnt de verificatiedialoog met een leeg scherm.

### <a name="data-preparation"></a>Gegevensvoorbereiding 
- Wanneer een specifieke waarde is gefilterd, worden fouten en de ontbrekende waarden ook gefilterd.
- Wijzigen van een strategie voor steekproeven Hiermee verwijdert u de volgende bestaande joinbewerkingen.
- Vervangen van een ontbrekende waarde wordt transformatie geen rekening NaN.
- De typeverwijzing datum genereert uitzondering wanneer null-waarde gedetecteerd.

### <a name="job-execution"></a>Uitvoeren van taak
- Er is geen duidelijke foutmelding wanneer de taakuitvoering is mislukt voor het uploaden van de projectmap omdat de limiet is overschreden.
- Als de werkmap voor Python-script van de gebruiker wordt gewijzigd, worden de bestanden die zijn geschreven naar uitvoer mappen worden niet bijgehouden. 
- Als het actief Azure-abonnement anders dan het huidige project behoort is, resulteert de verzending van de taak een fout 403.
- Wanneer Docker niet aanwezig is, wordt er is geen duidelijke foutbericht wordt geretourneerd als gebruiker probeert te Docker gebruiken als een doel kan worden uitgevoerd.
- .runconfig bestand wordt niet automatisch opgeslagen wanneer de gebruiker klikt op _uitvoeren_ knop.

### <a name="jupyter-notebook"></a>Jupyter Notebook
- Laptop-server starten niet als gebruiker met bepaalde typen aanmelding gebruikt.
- Foutberichten voor laptop-server doen niet surface in logboeken zichtbaar voor gebruiker.

### <a name="azure-portal"></a>Azure Portal
- Het donkere thema van Azure portal zorgt ervoor dat Model-Management-blade een zwart vak wordt weergegeven.

### <a name="operationalization"></a>Uitoefening
- Hergebruik van een manifest voor het bijwerken van een webservice zorgt ervoor dat een nieuwe Docker-installatiekopie gebouwd met een willekeurige naam.
- Weblogboeken service kunnen niet worden opgehaald uit Kubernetes cluster.
- Misleidend foutbericht wordt afgedrukt wanneer de gebruiker probeert te maken van een Model-Management-account of een ML Compute-account en problemen met machtigingen optreedt.
