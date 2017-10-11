---
title: Voorspeld onderhoud vooraf geconfigureerde oplossing | Microsoft Docs
description: Een beschrijving van de vooraf geconfigureerde oplossing van Azure IoT Suite voor voorspeld onderhoud.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: b370b3d7-2ce5-4906-9818-3aeedd471ee3
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 8bad198488c4940a83eb32ec02122a91d47ca86c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Overzicht van de vooraf geconfigureerde oplossing voor voorspeld onderhoud

De [vooraf geconfigureerde oplossing][lnk_preconfigured_solutions] voor *voorspellend onderhoud* is een van de vooraf geconfigureerde oplossingen van [Microsoft Azure IoT Suite][lnk_iot_suite]. Deze oplossing integreert het in realtime verzamelen van telemetriegegevens met een voorspellend model dat is gemaakt met behulp van [Azure Machine Learning][lnk-machine-learning].

Met Azure IoT Suite kunt u snel en eenvoudig verbinding maken met assets en deze controleren, en telemetriegegevens in realtime analyseren in dashboards en visualisaties. De oplossing voor predictief onderhoud bevat dashboards en visualisaties om u nieuwe bedrijfsinformatie te bieden waardoor u efficiënter kunt werken en meer inkomsten kunt genereren.

## <a name="the-scenario"></a>Het scenario

Fabrikam is een regionale luchtvaartmaatschappij die zich toelegt op het leveren van een uitstekende klantervaring tegen concurrerende prijzen. Een oorzaak van vertragingen zijn onderhoudsproblemen, waarbij het onderhoud van vliegtuigmotoren een bijzondere uitdaging vormt. Motorproblemen tijdens de vlucht moeten koste wat kost worden voorkomen. Fabrikam inspecteert om die reden regelmatig de motoren en volgt een planning voor het plegen van onderhoud. Vliegtuigmotoren slijten echter niet allemaal even snel. Soms wordt er onnodig onderhoud uitgevoerd op motoren. En wat belangrijker is, er doen zich soms problemen voor die ervoor zorgen dat een vliegtuig aan de grond moet blijven totdat het onderhoud is uitgevoerd. Deze problemen kunnen erg kostbaar zijn, in het bijzonder als een vliegtuig zich op een locatie bevindt waar de juiste technici of onderdelen niet beschikbaar zijn.

De motoren van de vliegtuigen van Fabrikam zijn uitgerust met sensoren die de toestand van de motor tijdens de vlucht in de gaten houden. Fabrikam maakt gebruik van de oplossing voor predictief onderhoud voor het verzamelen van de sensorgegevens die tijdens de vlucht worden verzameld. Na jarenlang operationele gegevens van engines te hebben verzameld, hebben de gegevensanalisten van Fabrikam een model samengesteld waarmee ze de nog resterende bruikbare levensduur van een vliegtuigmotor kunnen voorspellen. Het model gebruikt een correlatie tussen gegevens van vier van de motorsensoren en de slijtage van de motor die uiteindelijk tot problemen leidt. Hoewel Fabrikam doorgaat met het uitvoeren van regelmatige inspecties om de veiligheid te garanderen, kan het bedrijf nu de modellen gebruiken om na elke vlucht de resterende bruikbare levensduur van elke motor te berekenen. Het model gebruikt de telemetrie die tijdens de vlucht vanuit de machines is verzameld. Fabrikam is nu in staat om toekomstige probleempunten te voorspellen en om van tevoren onderhoud en reparatiewerkzaamheden te plannen.

> [!NOTE]
> Het model van de oplossing maakt gebruik van de reële slijtagegegevens van de motor.

Door het punt te voorspellen waarop onderhoud vereist is, kan Fabrikam zijn activiteiten zo optimaliseren dat de kosten worden verminderd.

Onderhoudscoördinatoren werken met planners:

- Om onderhoud zo te plannen dat dit samenvalt met een moment waarop een vliegtuig op een bepaalde locatie aan de grond staat.
- Om ervoor te zorgen dat er voldoende tijd is om het vliegtuig buiten bedrijf te stellen zonder het vliegschema te verstoren.
- Om technici zo in te plannen dat het onderhoud van het vliegtuig efficiënt en wordt uitgevoerd zonder dat er vertragingen ontstaan.

Magazijnbeheerders ontvangen de onderhoudsplannen zodat zij hun bestelproces en hun voorraad met reserveonderdelen kunnen optimaliseren.

Door deze activiteiten is Fabrikam in staat om de tijd die vliegtuigen aan de grond staan te minimaliseren, de operationele kosten te verlagen en kan het bedrijf de veiligheid van de passagiers en bemanning garanderen.

Als u beter wilt begrijpen welke mogelijkheden [Azure IoT Suite][lnk_iot_suite] biedt die klanten nodig hebben om het potentieel van voorspellend onderhoud te realiseren, bekijkt u deze [infographic][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Hoe de oplossing voor voorspeld onderhoud is gebouwd

De oplossing gebruikt een bestaand Azure Machine Learning-model dat als sjabloon beschikbaar is om te laten zien hoe deze mogelijkheden werken op basis van telemetriegegevens die zijn verzameld via IoT Suite-services. Microsoft heeft een [regressiemodel][lnk_regression_model] van een vliegtuigmotor ontwikkeld op basis van algemeen beschikbare gegevens<sup>\[1\]</sup>, evenals richtlijnen met stappen voor het gebruik van het model.

De Azure IoT-oplossing voor predictief onderhoud maakt gebruik van het regressiemodel dat op basis van deze sjabloon is gemaakt. Het model is geïmplementeerd in uw Azure-abonnement en is toegankelijk via een automatisch gegenereerde API. De oplossing omvat een subset met de testgegevens voor 4 (van in totaal 100) motoren en de 4 (van in totaal 21) gegevensstromen van sensoren. Deze gegevens leveren een accuraat resultaat op van het getrainde model.

*\[1\] A. Saxena en K. Goebel (2008). 'Turbofan Engine Degradation Simulation Data Set', NASA Ames Prognostics Data Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*

## <a name="get-started-with-predictive-maintenance"></a>Aan de slag met voorspellend onderhoud

In deze zelfstudie ziet u hoe u de oplossing voor voorspellend onderhoud inricht. U maakt u kennis met de basisfuncties van de oplossing voor voorspellend onderhoud. U krijgt toegang tot deze functies via het oplossingsdashboard dat samen met de vooraf geconfigureerde oplossing het volgende implementeert.

U hebt een actief Azure-abonnement nodig om deze zelfstudie te voltooien.

> [!NOTE]
> Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure][lnk_free_trial] voor meer informatie.

1. Meld u aan bij [azureiotsuite.com][lnk-azureiotsuite] met de referenties van uw Azure-account en klik op **+** om een oplossing te maken.
1. Klik in de tegel **Voorspellend onderhoud** op **Selecteren**.
1. Voer een **Naam van oplossing** in voor uw vooraf geconfigureerde oplossing voor voorspellend onderhoud.
1. Selecteer de **regio** die en het **abonnement** dat u wilt gebruiken voor het inrichten van de oplossing.
1. Klik op **Oplossing maken** om het inrichtingsproces te starten. Doorgaans duurt het enkele minuten om dit proces uit te voeren.

### <a name="wait-for-the-provisioning-process-to-complete"></a>Wacht tot het inrichtingsproces is voltooid.

1. Klik op de tegel voor uw oplossing met de status **Inrichten**.
1. Tijdens de implementatie van Azure-services in uw Azure-abonnement verschijnen verschillende **inrichtingstatuswaarden**.
1. Nadat het inrichten is voltooid, verandert de status in **Gereed**.
1. Klik op de tegel om de details van uw oplossing in het rechterdeelvenster weer te geven. In dit deelvenster kunt u het oplossingsdashboard starten en toegang krijgen tot de Machine Learning-werkruimte.

> [!NOTE]
> Als er problemen zijn met de implementatie van de vooraf geconfigureerde oplossing, leest u [Machtigingen op azureiotsuite.com][lnk-permissions] en de [veelgestelde vragen][lnk-faq]. Als de problemen zich blijven voordoen, maakt u een serviceticket aan in de [portal][lnk-portal].

Zijn er voor uw oplossing bepaalde details niet vermeld, die u wel verwacht had te zien? Geef ons suggesties voor functies op [User Voice](https://feedback.azure.com/forums/321918-azure-iot).

## <a name="view-the-solution"></a>De oplossing bekijken

Deze sectie helpt u bij de gebruikersinterface van de oplossing.

### <a name="predictive-maintenance-dashboard"></a>Dashboard voorspeld onderhoud

Voor deze pagina in de webtoepassing wordt gebruikgemaakt van PowerBI JavaScript-besturingselementen (zie de [PowerBI-opslagplaats voor visualisaties][lnk-powerbi]) voor het visualiseren van:

* De uitvoergegevens van de Stream Analytics-jobs in Blob Storage.
* De resterende levensduur en aantal cycli per vliegtuigmotor.

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Het gedrag van de cloudoplossing observeren

Navigeer in Azure Portal naar de resourcegroep met de naam van de oplossing die u hebt gekozen om de ingerichte resources weer te geven.

![][img-resource-group]

Wanneer u de vooraf geconfigureerde oplossing inricht, krijgt u een e-mailbericht met een koppeling naar de Machine Learning-werkruimte. U kunt ook naar de Machine Learning-werkruimte navigeren vanaf de [azureiotsuite.com][lnk-azureiotsuite]-pagina voor de ingerichte oplossing. Op deze pagina is een tegel beschikbaar wanneer de oplossing de status **Gereed** heeft.

![][img-machine-learning]

In de oplossingsportal kunt u zien dat het voorbeeld is ingericht met vier gesimuleerde apparaten die twee vliegtuigen met twee motoren per vliegtuig vertegenwoordigen, elk met vier sensoren. Wanneer u voor het eerst naar de oplossingsportal navigeert, wordt de simulatie gestopt.

![][img-simulation-stopped]

Klik op **Simulatie starten** om te beginnen met de simulatie. Het dashboard wordt ingevuld met de sensorgeschiedenis, de resterende levensduur, de cycli en de geschiedenis van de resterende levensduur.

![][img-simulation-running]

Wanneer de resterende levensduur minder dan 160 is (een willekeurige drempelwaarde gekozen ter illustratie), verschijnt in de oplossingsportal een waarschuwingssymbool naast de weergegeven resterende levensduur. De oplossingsportal geeft ook de vliegtuigmotor geel gemarkeerd weer. Zoals u merkt, vertonen de waarden van de resterende levensduur een algemene neerwaartse trend, maar stijgen en dalen die waarden vaak. Dit gedrag wordt veroorzaakt door de verschillende lengten van de cycli en de nauwkeurigheid van het model.

![][img-simulation-warning]

Het duurt ongeveer 35 minuten voordat de volledige simulatie 148 cycli heeft voltooid. Na ongeveer 5 minuten wordt voor het eerst de drempelwaarde van 160 voor de resterende levensduur bereikt. De drempelwaarde voor beide motoren wordt na ongeveer 8 minuten bereikt.

De simulatie wordt uitgevoerd voor de volledige gegevensset voor 148 cycli en bepaalt dan het eindresultaat voor de resterende levensduur en het aantal cycli.

U kunt de simulatie op elk punt stoppen, maar wanneer u op **Simulatie starten** klikt, wordt de simulatie opnieuw vanaf het begin van de gegevensset uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

Meer informatie hoe Azure IoT werkt in scenario's voor voorspellend onderhoud vindt u in [Capture value from the Internet of Things][lnk_capture_value] (Waarde toevoegen via het Internet of Things).

Volg een [walkthrough][lnk-predictive-walkthrough] over de oplossing voor predictief onderhoud.

U kunt ook enkele van de andere functies en mogelijkheden van de vooraf geconfigureerde IoT Suite-oplossingen verkennen:

* [Veelgestelde vragen over IoT Suite][lnk-faq]
* [Fundamentele IoT-beveiliging][lnk-security-groundup]

[img-resource-group]: media/iot-suite-predictive-overview/resource-group.png
[img-simulation-stopped]: media/iot-suite-predictive-overview/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-overview/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-overview/simulation-warning.png
[img-machine-learning]: media/iot-suite-predictive-overview/machine-learning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3

[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-permissions]: iot-suite-permissions.md
[lnk-portal]: http://portal.azure.com/
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/