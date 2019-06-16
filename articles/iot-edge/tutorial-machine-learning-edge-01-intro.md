---
title: Gedetailleerd overzicht van Machine Learning in Azure IoT Edge | Microsoft Docs
description: Een op hoog niveau zelfstudie laat zien hoe u via de verschillende taken die nodig zijn voor het maken van een end-to-end voor machine learning op het edge-scenario.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 916e48752431be41ff150c2ac84e66eb1e98e81f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057748"
---
# <a name="tutorial-an-end-to-end-solution-using-azure-machine-learning-and-iot-edge"></a>Zelfstudie: Een end-to-end-oplossing met behulp van Azure Machine Learning en IoT Edge

IoT-toepassingen willen vaak, profiteren van de intelligente cloud en de intelligente edge. In deze zelfstudie helpen wij u bij een machine learning-model met gegevens die worden verzameld van IoT-apparaten in de cloud, training dat model implementeert naar IoT Edge, onderhouden en regelmatig verfijnen van het model.

Het primaire doel van deze zelfstudie is om te introduceren de verwerking van IoT-gegevens met machine learning, specifiek voor het edge-apparaten. Terwijl we veel aspecten van een algemene machine learning-werkstroom touch, is in deze zelfstudie niet bedoeld als diepgaande Inleiding tot machine learning. Als een aanvraag in-punt, probeer we niet te maken van een uiterst geoptimaliseerde model van de use-case-we doen net voldoende ter illustratie van het proces voor het maken en gebruiken van een levensvatbare model voor de verwerking van de IoT-gegevens.

## <a name="target-audience-and-roles"></a>Doelgroep en rollen

Deze reeks artikelen is bedoeld voor ontwikkelaars zonder eerdere ervaring in IoT-ontwikkeling- of machine learning. Gebruik machine learning-aan de rand moet kennis van hoe u verbinding maken met een breed scala van technologieën. Daarom bestaat deze zelfstudie uit een volledige end-to-end scenario om te demonstreren op één manier lid worden van deze technologieën bij elkaar voor een IoT-oplossing. In een omgeving met echte, kunnen deze taken worden verdeeld over verschillende personen met verschillende specialisaties. Bijvoorbeeld, ontwikkelaars zou zich richten op het apparaat of cloud-code, terwijl gegevenswetenschappers de analytics-modellen ontworpen. Om in te schakelen op een afzonderlijke ontwikkelaar van deze zelfstudie te voltooien, zijn er aanvullende richtlijnen met inzichten zijn beschikbaar en koppelingen naar meer informatie die we hopen dat u voldoende zijn om te begrijpen wat is wordt voltooid, en daarom is.

U kunt ook samen met collega's met verschillende rollen samen de zelfstudie volgen om uw volledige kennis over te dragen, en meer als team hoe dingen bij elkaar passen.

In beide gevallen geeft elk artikel in deze zelfstudie om te zien van de lezer, de rol van de gebruiker. Deze rollen zijn onder andere:

* Ontwikkeling van cloud (met inbegrip van een cloud-ontwikkelaars die werken in een DevOps-capaciteit)
* Gegevensanalyse

## <a name="use-case-predictive-maintenance"></a>Toepassing: Voorspellend onderhoud

We in dit scenario gebaseerd op een use-case weergegeven op de Conferentie op Prognostics en Health Management (PHM08) in 2008. Het doel is om de resterende levensduur (RUL) van een set turbofan vliegtuig-engines. Deze gegevens is gegenereerd met behulp van C-MAPSS, de commerciële versie van de software MAPSS (modulaire Aero Propulsion System simulatie). Deze software biedt een flexibel turbofan engine simulatie omgeving om te simuleren eenvoudig de status, controle en engine-parameters.

De gegevens die worden gebruikt in deze zelfstudie is afkomstig uit de [Turbofan engine verslechtering van de simulatie data set](https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/#turbofan).

Uit het bestand Leesmij-bestand:

***Experimentele Scenario***

*Data-sets bestaan uit meerdere multidimensionale tijdreeksen. Elke set gegevens verder wordt onderverdeeld in trainings- en testset subsets. Elke tijdreeksen afkomstig is van een andere engine – dat wil zeggen, de gegevens kunnen worden beschouwd als te worden van een wagenpark engines van hetzelfde type. Elke motor begint met verschillende graden van de eerste slijtage en productie-variant is onbekend voor de gebruiker. Deze slijtage en variatie wordt beschouwd als normale, dat wil zeggen, deze wordt niet beschouwd als een fouttoestand. Er zijn drie operationele instellingen waarvoor een aanzienlijke invloed op de prestaties van de engine. Deze instellingen zijn ook opgenomen in de gegevens. De gegevens is besmet met sensor ruis.*

*De engine functioneert normaal aan het begin van elke tijdreeksen en ontwikkelt een storing op een bepaald moment tijdens de reeks. In de trainingsset groeit de fout in grootte tot systeemfout is opgetreden. In de test is ingesteld eindigt de tijdreeksen enige tijd voordat systeemfout is opgetreden. Het doel van de concurrentie is om te voorspellen van het aantal resterende operationele cycli vóór de fout in de test is ingesteld, dat wil zeggen, het aantal operationele cycli na de laatste cyclus waarop de engine blijft werken. Ook een vector van true resterende bruikbare levensduur (RUL) waarden opgegeven voor de testgegevens.*

Omdat de gegevens is gepubliceerd voor een competitie, zijn verschillende methoden voor het afleiden van machine learning-modellen onafhankelijk van elkaar gepubliceerd. Wij vinden dat voorbeelden studie kan handig zijn in meer informatie over het proces en redeneren die betrokken zijn bij het maken van een specifieke machine learning-model. Zie bijvoorbeeld:

[Vliegtuig engine fout voorspellingsmodel](https://github.com/jancervenka/turbofan_failure) door GitHub gebruiker jancervenka.

[Degradatie van de turbofan engine](https://github.com/hankroark/Turbofan-Engine-Degradation) door GitHub gebruiker hankroark.

## <a name="process"></a>Process

De volgende afbeelding ziet u de ruwe stappen die we in deze zelfstudie volgen:

![Architectuurdiagram voor processtappen](media/tutorial-machine-learning-edge-01-intro/tutorial-steps-overview.png)

1. **Trainingsgegevens verzamelen**: Het proces wordt gestart door trainingsgegevens te verzamelen. In sommige gevallen worden gegevens heeft al zijn verzameld en is beschikbaar in een database of in de vorm van gegevensbestanden. In andere gevallen, met name voor IoT-scenario's, moet de gegevens worden verzameld van IoT-apparaten en sensoren en opgeslagen in de cloud.

   Er wordt ervan uitgegaan dat u een verzameling van turbofan-engines, geen hebt, zodat de projectbestanden bevatten een eenvoudige apparaatsimulator waarmee de gegevens van de NASA-apparaat naar de cloud verzendt.

1. **Gegevens voorbereiden**. In de meeste gevallen moet de onbewerkte gegevens verzameld van apparaten en sensoren voorbereiding voor machine learning. Deze stap kan betrekking hebben op gegevens opschonen, gegevens formatteren of sleutel te ' injecteren ' aanvullende informatie machine learning kunt uitschakelen.

   Voor onze gegevens vliegtuig engine machine omvat gegevensvoorbereiding het berekenen van de expliciete time fout bij tijden voor elk gegevenspunt in de steekproef op basis van de werkelijke opmerkingen over de gegevens. Deze informatie kunt de machine learning-algoritme voor het vinden van correlatie tussen de werkelijke sensor data patronen en de verwachte resterende levensduur van de engine. Deze stap is zeer domeinspecifieke.

1. **Een machine learning-model bouwt**. Op basis van de voorbereide gegevens, kunnen we nu experimenteren met andere machine learning-algoritmen en parameteriseringen trainen van modellen en de resultaten met elkaar vergelijken.

   In dit geval vergelijken we voor het testen van de voorspelde resultaten berekend door het model met de werkelijke resultaten waargenomen in een verzameling van engines. In Azure Machine Learning, kunnen we de verschillende iteraties van modellen die we in een model-register maken beheren.

1. **Het model implementeren**. Zodra we een model dat voldoet aan onze criteria voor succes hebben, kunnen we overstappen naar de implementatie. Dat omvat het overschrijven van het model in een web service-app die kan worden ingevoerd met gegevens met behulp van REST-aanroepen en het retourneren van resultaten van de analyse. De service-web-app wordt vervolgens verpakt in een docker-container op zijn beurt kan worden geïmplementeerd in de cloud of als een IoT Edge-module. In dit voorbeeld richten we op IoT Edge-implementatie.

1. **Onderhouden en het model te verfijnen**. Ons werk is niet uitgevoerd wanneer het model is geïmplementeerd. In veel gevallen willen we doorgaan met het verzamelen van gegevens en deze gegevens periodiek te uploaden naar de cloud. We kunnen deze gegevens vervolgens gebruiken opnieuw trainen en het model, die we vervolgens op IoT Edge implementeren kunt verfijnen.

## <a name="prerequisites"></a>Vereisten

Als u wilt de zelfstudie hebt voltooid, moet u toegang tot een Azure-abonnement waarin u bevoegd bent om resources te maken. Aantal van de services die worden gebruikt in deze zelfstudie rekening Azure kosten in gebracht. Als u nog geen Azure-abonnement, u mogelijk aan de slag met een [gratis Azure-Account](https://azure.microsoft.com/offers/ms-azr-0044p/).

U moet ook een virtuele machine met PowerShell is geïnstalleerd waar u scripts om in te stellen van een virtuele Machine van Azure als uw ontwikkelcomputer kunt uitvoeren.

In dit document gebruiken we de volgende set hulpprogramma's:

* Een Azure IoT hub voor het vastleggen van gegevens

* Azure-laptops als onze belangrijkste front-end voor het voorbereiden van gegevens en machine learning experimenten. Python code uitvoeren in een notitieblok in een subset van de voorbeeldgegevens is een uitstekende manier om snel iteratieve en interactieve doorlooptijd tijdens de gegevensvoorbereiding van. Jupyter-notebooks kunnen ook worden gebruikt om voor te bereiden scripts om uit te voeren op schaal in een compute-back-end.

* Azure Machine Learning als een back-end voor machine learning op schaal en voor machine learning-installatiekopie genereren. Wij station de Azure Machine Learning back-end met behulp van scripts hebt voorbereid en getest in Jupyter-notebooks.

* Azure IoT Edge voor de toepassing uit de cloud van de installatiekopie van een machine learning

Er zijn natuurlijk, andere opties beschikbaar. In bepaalde scenario's, bijvoorbeeld kunnen IoT Central worden gebruikt als een alternatief zonder code om vast te leggen van eerste training van IoT-apparaten.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is onderverdeeld in de volgende secties:

1. Stel uw ontwikkelcomputer en Azure-services.
2. Genereer het trainen van gegevens voor de machine learning-module.
3. Trainen en implementeren van de machine learning-module.
4. Een IoT Edge-apparaat om te fungeren als een transparante gateway configureren.
5. Maken en implementeren van IoT Edge-modules.
6. Gegevens verzenden naar uw IoT Edge-apparaat.

Doorgaan met het volgende artikel voor het instellen van een ontwikkelcomputer en Azure-resources inrichten.

> [!div class="nextstepaction"]
> [Instellen van een omgeving voor machine learning op IoT Edge](tutorial-machine-learning-edge-02-prepare-environment.md)
