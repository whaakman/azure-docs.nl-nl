---
title: Overzicht van verbonden factory's in Azure IoT Suite | Microsoft Docs
description: Een beschrijving van de vooraf geconfigureerde oplossing voor verbonden factory's van Azure IoT Suite.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: d502c8e2e2715899279f6ebcf7ed89c19a1bb9a6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-the-connected-factory-preconfigured-solution"></a>Aan de slag met de vooraf geconfigureerde oplossing voor verbonden factory's

[Vooraf geconfigureerde oplossingen][lnk-preconfigured-solutions] voor Azure IoT Suite zijn voorzien van meerdere Azure IoT-services om totaaloplossingen te leveren voor het implementeren van algemene IoT-bedrijfsscenario's. De vooraf geconfigureerde oplossing voor *verbonden factory's* maakt verbinding met en controleert uw industriële apparaten. U kunt de oplossing gebruiken om de gegevensstroom van uw apparaten te analyseren en operationele productiviteit en winstgevendheid te bevorderen.

In deze zelfstudie leert u hoe u de vooraf geconfigureerde oplossing voor verbonden factory's inricht. Hierbij maakt u ook kennis met de basisfuncties van de vooraf geconfigureerde oplossing. U hebt toegang tot veel van deze functies via het *oplossingsdashboard* dat als onderdeel van de vooraf geconfigureerde oplossing wordt geïmplementeerd:

![dashboard van de vooraf geconfigureerde oplossing voor verbonden factory's][img-cf-home]

U hebt een actief Azure-abonnement nodig om deze zelfstudie te voltooien.

> [!NOTE]
> Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure][lnk_free_trial] voor meer informatie.
> 
> 

## <a name="provision-the-solution"></a>De oplossing inrichten

1. Meld u aan bij azureiotsuite.com met de referenties van uw Azure-account en klik op **+** om een oplossing te maken.
2. Klik op de tegel **Verbonden factory** op **Selecteren**.
3. Voer een **oplossingsnaam** in voor uw verbonden vooraf geconfigureerde oplossing.
4. Selecteer het **abonnement** dat en de **regio** die u wilt gebruiken voor het inrichten van de oplossing.
5. Klik op **Oplossing maken** om het inrichtingsproces te starten. Doorgaans duurt het enkele minuten om dit proces uit te voeren.

### <a name="while-you-wait-for-the-provisioning-process-to-complete"></a>Doe het volgende terwijl u wacht tot het inrichtingsproces is voltooid

1. Klik op de tegel voor uw oplossing met de status **Inrichten**.
2. Tijdens de implementatie van Azure-services in uw Azure-abonnement verschijnen verschillende **inrichtingstatuswaarden**.
3. Nadat het inrichten is voltooid, verandert de status in **Gereed**.
4. Klik op de tegel om de details van uw oplossing in het rechterdeelvenster weer te geven.

> [!NOTE]
> Als er problemen zijn met de implementatie van de vooraf geconfigureerde oplossing, leest u [Machtigingen op azureiotsuite.com][lnk-permissions] en de [veelgestelde vragen over Connected Factory](iot-suite-faq-cf.md). Als de problemen zich blijven voordoen, maakt u een serviceticket aan in de [portal][lnk-portal].

Zijn er voor uw oplossing bepaalde details niet vermeld, die u wel verwacht had te zien? Geef ons suggesties voor functies op [User Voice](https://feedback.azure.com/forums/321918-azure-iot).

## <a name="scenario-overview"></a>Overzicht van scenario's

Wanneer u de vooraf geconfigureerde oplossing voor verbonden factory's implementeert, wordt deze vooraf ingevuld met de resources waarmee u een algemeen industrieel scenario kunt doorlopen. In dit scenario rapporteren verschillende factory's die zijn verbonden met de oplossing, de gegevenswaarden die vereist zijn voor het berekenen van de algemene apparatuurefficiëntie (overall equipment efficiency, OEE) en de key performance indicators (KPI's). De volgende gedeelten laten u zien hoe u:

* de factory, de productielijnen, de OEE van stations en de KPI-waarden controleert;
* de telemetriegegevens analyseert die van deze apparaten worden gegenereerd, met behulp van Time Series Insights in Azure;
* op waarschuwingen reageert om problemen op te lossen.

Een belangrijke functie van dit scenario is dat u al deze acties extern kunt uitvoeren vanuit het dashboard van de oplossing. U hebt geen fysieke toegang tot de apparaten nodig.

## <a name="view-the-solution-dashboard"></a>Het oplossingsdashboard bekijken

Vanaf het dashboard van de oplossing kunt u de geïmplementeerde oplossing beheren. Het is een hiërarchische weergave van een overkoepelende factoryconfiguratie. U kunt bijvoorbeeld de OEE en KPI's weergeven, nieuwe knooppunten voor telemetrie publiceren en waarschuwingen uitvoeren.

1. Wanneer het inrichten is voltooid en op de tegel voor uw vooraf geconfigureerde oplossing de status **Gereed** wordt weergegeven, kiest u **Starten** om uw portal voor de oplossing voor verbonden factory's te openen op een nieuw tabblad.

    ![De vooraf geconfigureerde oplossing starten][img-launch-solution]

1. Standaard ziet u in de oplossingsportal het *dashboard*. U kunt naar andere gebieden van de portal navigeren met het menu aan de linkerkant van de pagina.

    ![Dashboard van de vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-menu]

Het dashboard bevat de volgende informatie:

* Een paneel met een **factorylijst** waarin de status, locatie en huidige productieconfiguratie in de oplossing worden weergegeven. Wanneer u de oplossing voor het eerst uitvoert, zijn er een paar gesimuleerde apparaten. De simulatie van de productielijn bestaat uit drie echte OPC UA-servers per productielijn die gesimuleerde taken uitvoeren en gegevens delen. Zie de [veelgestelde vragen over Connected Factory](iot-suite-faq-cf.md) voor meer informatie over OPC UA.
* Een **kaart** die de locatie aangeeft van elk apparaat dat met de oplossing is verbonden. De oplossing kan de Bing Kaarten-API gebruiken om informatie op de kaart tekenen. Als de Bing Kaarten Enterprise-API is ingeschakeld voor uw abonnement, wordt deze functie automatisch gebruikt. Als dit niet het geval is, leest u de [veelgestelde vragen][lnk-faq] voor meer informatie over hoe u de kaart dynamisch kunt maken.
* Een paneel **Waarschuwingen** dat waarschuwingen weergeeft die worden gegenereerd wanneer een telemetrie- of OEE-/KPI-waarde een bepaalde drempelwaarde overschrijdt.
* Een paneel **Overall Equipment Efficiency** dat de OEE-waarden weergeeft voor de hele onderneming of voor de factory, de productielijn of het station die/dat u bekijkt. Deze waarde wordt geaggregeerd van de stationsweergave naar het ondernemingsniveau. De OEE-gegevens en de bijbehorende elementen kunnen nader worden geanalyseerd.
* Een paneel **Key Performance Indicators** dat het aantal geproduceerde eenheden weergeeft en de energie die wordt verbruikt door de hele onderneming of door de factory, de productielijn of het station die/dat u bekijkt. Deze waarden worden geaggregeerd van de stationsweergave naar het ondernemingsniveau.

## <a name="view-factories"></a>Factory's weergeven

Het paneel *Factory's* geeft de geografische locatie van alle factory's in de oplossing weer, samen met hun status en de huidige productieconfiguratie. U kunt in de lijst met locaties naar de andere niveaus in de oplossingshiërarchie navigeren. De rijen in de lijst zijn hyperlinks die gekoppeld zijn aan details van de productielijnen op die locatie. U kunt vervolgens inzoomen op de details van de productielijn en de weergave op stationsniveau. U kunt ook een filter toepassen op de lijst.

![Factory's van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-factories] 

1. Het **paneel Factory's** bevat de lijst met factory's voor deze oplossing.

2. De factorylijst geeft aanvankelijk aan dat bij het inrichtingsproces zes factory's zijn gemaakt. U kunt extra gesimuleerde en fysieke apparaten aan de oplossing toevoegen.

3. Als u de details van een factory wil bekijken, klikt u op de rij in de factorylijst.

4. Als u de details van een productielijn wil bekijken, klikt u op de rij in de lijst.

5. Als u de gepubliceerde OPC UA-knooppunten van een station in de productielijn wilt weergeven, klikt u op de rij in de lijst.

6. Als u details voor een bepaald knooppunt in het station wilt bekijken, klikt u op de rij in de lijst. Met deze actie wordt het contextpaneel met Time Series Insights-visualisaties gestart. Klik op de grafieken voor verdere analyse in de Time Series Insights-verkenner.

## <a name="view-map"></a>Kaart weergeven

Als u met uw abonnement toegang hebt tot de Bing Kaarten-API, wordt op de kaart *Factory's* de geografische locatie en de status van alle factory's in de oplossing weergegeven. Klik op de locaties op de kaart om in te zoomen op de locatiedetails.

![Kaart van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-map]

## <a name="view-alerts"></a>Waarschuwingen weergeven

Het paneel **Waarschuwing** geeft de waarschuwingen weer die worden gegenereerd wanneer een gemelde waarde of een berekende OEE-/KPI-waarde de geconfigureerde drempelwaarde overschrijdt. Dit paneel geeft waarschuwingen weer van elk niveau van de hiërarchie, van de weergave op stationsniveau tot de overkoepelende weergave. De waarschuwingen bevatten een beschrijving van de waarschuwing, de datum, de tijd, de locatie en het aantal instanties. Aan de hand van de Time Series Insights-gegevens krijgt u inzicht in de gegevens die de waarschuwing hebben veroorzaakt. Waar van toepassing worden de Time Series Insights-gegevens weergegeven in de waarschuwingen. Als u beheerder bent, kunt u standaardacties uitvoeren voor de waarschuwingen, zoals:

* De waarschuwing sluiten.
* De waarschuwing accepteren.

Eventueel kunt u complexere acties uitvoeren. Voor het OPC UA-drukknooppunt van de assembly kunt u bijvoorbeeld het volgende doen:

* Ondersteunende informatie weergeven op een webpagina in een nieuw browservenster.
* De oorzaak van de waarschuwing oplossen door een OPC UA-methode op het apparaat aan te roepen.
* De beschikbaarheid van de standaardacties onderdrukken.

    ![Waarschuwingen van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-alerts]

> [!NOTE]
> Deze waarschuwingen worden gegenereerd door regels die zijn opgegeven in een configuratiebestand in de vooraf geconfigureerde oplossing. Deze regels kunnen waarschuwingen genereren wanneer de OEE- of KPI-gegevens of de waarden van het OPC UA-knooppunt de geconfigureerde drempelwaarde overschrijden.

1. Het **paneel Waarschuwingen** geeft de waarschuwingen weer die in deze oplossing worden gegenereerd.

2. Klik op de waarschuwing in het paneel Waarschuwingen om de details van een waarschuwing weer te geven.

3. Klik op de grafiek in het paneel Waarschuwingen om de Time Series Insights-verkenner te openen en de waarschuwingsgegevens verder te analyseren.

4. In het paneel Waarschuwingen zijn verschillende acties voor de waarschuwing beschikbaar. Kies de gewenste optie en klik op de knop voor het uitvoeren van de actie.

## <a name="view-overall-equipment-efficiency"></a>Algemene apparatuurefficiëntie weergeven

De OEE bepaalt de efficiëntie van het productieproces met behulp van operationele parameters gerelateerd aan cruciale productie. OEE is een standaardmeeteenheid binnen de industrie die wordt berekend door de beschikbaarheidswaarde te vermenigvuldigen met de prestatiewaarde en de kwaliteitswaarde: OEE = beschikbaarheid x prestaties x kwaliteit.

![OEE van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-oee]

1. Als u de OEE voor een bepaald niveau in de hiërarchie wilt bekijken, gaat u naar de weergave die u nodig hebt. De OEE voor deze weergave wordt in het paneel weergegeven samen met de elementen die onderdeel uitmaken van het OEE-percentage.

2. Voor een nadere analyse van de OEE voor een willekeurig niveau in de hiërarchiegegevens klikt u op het OEE-percentage, het beschikbaarheidspercentage, het prestatiepercentage of het kwaliteitspercentage. Er wordt een contextpaneel weergegeven met Time Series Insights-visualisaties die gegevens van het afgelopen uur, de afgelopen 24 uur en de afgelopen 7 dagen tonen.

    ![TSI-visualisatie van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-tsi-visualization]

3. Klik op de grafiek in het paneel Waarschuwingen om de waarschuwingsgegevens verder te analyseren. Met deze actie wordt de Time Series Insights-verkenner geopend.

    ![TSI-verkenner van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-tsi-explorer]

## <a name="view-key-performance-indicators"></a>Key performance indicators weergeven

De oplossing biedt twee key performance indicators: *eenheden per uur* en *energieverbruik in kWh*.

![KPI van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-kpi]

1. Als u het aantal eenheden per uur of het energieverbruik voor een bepaald niveau in de hiërarchie wilt bekijken, gaat u naar de weergave die u nodig hebt. Het aantal eenheden per uur en het energieverbruik worden in het paneel weergegeven.

2. Als u het aantal eenheden per uur of het energieverbruik verder wilt analyseren voor een willekeurig niveau in de hiërarchie, klikt u op de meter in het paneel **Key Performance Indicators**. Er wordt een contextpaneel weergegeven met Time Series Insights-visualisaties waarmee u gegevens van het afgelopen uur, de afgelopen 24 uur en de afgelopen 7 dagen kunt bekijken.

## <a name="scenario-review"></a>Samenvatting van scenario

In dit scenario hebt u de OEE- en KPI-waarden van uw factory's gecontroleerd in het dashboard. Vervolgens hebt u Time Series Insights gebruikt om meer informatie te krijgen voor het verder inzoomen op de telemetriegegevens voor OEE en KPI's om te helpen bij het detecteren van afwijkingen. U hebt ook het paneel Waarschuwingen gebruikt om problemen met uw factory's te bekijken en de beschikbare acties gebruikt om de waarschuwing te verhelpen.

## <a name="other-features"></a>Andere functies

De volgende gedeelten beschrijven een aantal extra functies van de oplossing voor verbonden factory's die niet aan bod zijn gekomen in het voorgaande scenario.

## <a name="apply-filters"></a>Filters toepassen

1. Klik op de **dubbele punthaak** om een lijst met beschikbare filters weer te geven in het paneel met factorylocaties of het paneel Waarschuwingen.

2. Het paneel Filters wordt weergegeven. 

    ![Filters van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-alert-filter]

3. Kies het filter dat u nodig hebt. U kunt ook vrije tekst typen in de filtervelden.

4. Het filter wordt vervolgens toegepast. U ziet de filterstatus ook in het dashboard en wel in de vorm van een trechter die wordt weergegeven in de tabellen met factory's en waarschuwingen.

    ![Filters van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-alert-filter-funnel]

    > [!NOTE]
    > Een actief filter heeft geen invloed op de weergegeven OEE- en KPI-waarden; het filtert alleen de inhoud van de lijst.

5. Als u een filter wilt wissen, klikt u op de trechter en vervolgens op Filter in het filtercontextpaneel. De tekst **Alle** wordt weergegeven in de tabellen met factory's en waarschuwingen.

## <a name="browse-an-opc-ua-server"></a>Door een OPC UA-server bladeren

Wanneer u de vooraf geconfigureerde oplossing implementeert, worden er automatisch gesimuleerde OPC UA-servers ingericht die u met de oplossingsbrowser kunt doorbladeren. Deze servers zijn *gesimuleerde OPC UA-servers*. Gesimuleerde servers maken het voor u gemakkelijk om te experimenteren met een vooraf geconfigureerde oplossing zonder dat u echte, fysieke servers hoeft te implementeren. Raadpleeg de zelfstudie [Connect your OPC UA device to the connected factory preconfigured solution][lnk-connect-cf] (Uw OPC UA-apparaat koppelen aan de vooraf geconfigureerde oplossing voor verbonden factory's) als u een echte OPC UA-server aan de oplossing wilt koppelen.

1. Klik op het **factorypictogram** in de navigatiebalk van het dashboard.

    ![Serverbrowser van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-server-browser]

2. Kies een van de servers uit de vooraf geconfigureerde lijst. Deze lijst bevat de servers die in de vooraf geconfigureerde oplossing zijn geïmplementeerd.

    ![Serverselectie van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-server-choice]

3. Klik op **Verbinden**. Er wordt een beveiligingsdialoogvenster weergegeven. Tijdens de simulatie is het veilig om op **Doorgaan** te klikken

4. Klik op een van de knooppunten in de serverstructuur om deze uit te vouwen. Naast knooppunten die telemetrie publiceren, staat een vinkje.

    ![Serverstructuur van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-server-tree]

5. Klik met de rechtermuisknop op een item om het knooppunt te lezen, te schrijven, te publiceren of aan te roepen. Welke acties er beschikbaar zijn, is afhankelijk van uw machtigingen en de kenmerken van het knooppunt. Met de optie voor lezen wordt een contextpaneel weergegeven met de waarde van dat knooppunt. Met de optie voor schrijven wordt een contextpaneel weergegeven waarin u een nieuwe waarde kunt invoeren. Met de optie voor aanroepen wordt een knooppunt weergegeven waarin u de parameters voor het aanroepen kunt invoeren.

## <a name="publish-a-node"></a>Een knooppunt publiceren

Wanneer u door een *gesimuleerde OPC UA-server* bladert, kunt u ook nieuwe knooppunten publiceren. Vervolgens kunt u de telemetrie van deze knooppunten in de oplossing analyseren. De *gesimuleerde OPC UA-servers* maken het gemakkelijk om te experimenteren met een vooraf geconfigureerde oplossing zonder dat u echte, fysieke apparaten hoeft te implementeren.

1. Blader naar een knooppunt in de browserstructuur van de OPC UA-server die u wilt publiceren.

2. Klik met de rechtermuisknop op het knooppunt.

3. Kies **Publiceren**.

    ![Verbonden factory publiceert knooppunt][cf-img-publish-node]

4. Er word een contextpaneel weergegeven waarin staat dat het publiceren is voltooid. Het knooppunt wordt weergegeven met een vinkje in de weergave op stationsniveau.

    ![Succesvolle publicatie van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-publish-success]

## <a name="command-and-control"></a>Opdracht en controle

Met de verbonden factory kunt u uw industriële apparaten rechtstreeks vanuit de cloud beheren en bedienen. U kunt deze functie gebruiken om te reageren op waarschuwingen die door het apparaat worden gegenereerd. U kunt bijvoorbeeld vanuit de cloud een opdracht naar het apparaat verzenden. U vindt de beschikbare opdrachten in het knooppunt **StationCommands** in de browserstructuur van de OPC UA-servers. In dit scenario opent u een overdrukventiel op de verzamelplaats van een productielijn in München. U moet de rol van **beheerder** hebben voor de implementatie van de vooraf geconfigureerde oplossing om de opdracht- en bedieningsfuncties te kunnen gebruiken.

1. Blader naar het knooppunt **StationCommands** in de browserstructuur van de OPC UA-server.

2. Kies de opdracht die u wilt gebruiken.

3. Klik met de rechtermuisknop op het knooppunt.

4. Kies **Aanroepen**.

    ![Aanroepopdracht van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-call-command]

5. Er wordt een contextpaneel weergegeven waarin staat welke methode u gaat aanroepen en, indien van toepassing, details van de parameter.

6. Kies **Aanroepen**.

    ![Aanroepcontext van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-call-context]

7. Het contextpaneel wordt bijgewerkt met de melding dat het aanroepen van de methode is voltooid. U kunt controleren of de oproep is voltooid door de waarde van het drukknooppunt te lezen, die na de aanroep is bijgewerkt.

    ![Aanroepen voltooid van vooraf geconfigureerde oplossing voor verbonden factory's][cf-img-call-success]


## <a name="behind-the-scenes"></a>Achter de schermen

Wanneer u een vooraf geconfigureerde oplossing implementeert, maakt het implementatieproces meerdere resources in het door u geselecteerde Azure-abonnement. U kunt deze resources weergeven in Azure [Portal][lnk-portal]. Het implementatieproces maakt een **resourcegroep** met een naam die is gebaseerd op de naam die u voor uw vooraf geconfigureerde oplossing hebt gekozen:

![Vooraf geconfigureerde oplossing in Azure Portal][img-cf-portal]

U kunt de instellingen van elke resource weergeven door deze te selecteren in de lijst met resources in de resourcegroep.

U kunt ook de broncode voor de vooraf geconfigureerde oplossing weergeven. De broncode van de vooraf geconfigureerde oplossing voor verbonden factory's bevindt zich in de GitHub-opslagplaats [azure-iot-connected-factory][lnk-cfgithub]:

Wanneer u klaar bent, kunt u de vooraf geconfigureerde oplossing verwijderen uit uw Azure-abonnement op de site [azureiotsuite.com][lnk-azureiotsuite]. Met de site kunt u gemakkelijk resources verwijderen die werden aangevoerd toen u de vooraf geconfigureerde oplossing hebt gemaakt.

> [!NOTE]
> Verwijder de oplossing op de site [azureiotsuite.com][lnk-azureiotsuite]. Zo zorgt u ervoor dat alles met betrekking tot de vooraf geconfigureerde oplossing wordt verwijderd. Verwijder de resourcegroep niet in de portal.

## <a name="next-steps"></a>Volgende stappen

Nu u een werkende vooraf geconfigureerde oplossing hebt geïmplementeerd, kunt u doorgaan met IoT Suite door de volgende artikels te lezen:

* [Overzicht van de vooraf geconfigureerde oplossing voor verbonden factory's][lnk-rm-walkthrough]
* [Uw apparaat koppelen aan de vooraf geconfigureerde oplossing voor verbonden factory's][lnk-connect-cf]
* [Machtigingen op de site azureiotsuite.com][lnk-permissions]

[img-cf-home]:media/iot-suite-connected-factory-overview/cf-dashboard.png
[img-launch-solution]: media/iot-suite-connected-factory-overview/launch-cf.png
[cf-img-menu]: media/iot-suite-connected-factory-overview/cf-dashboard-menu.png
[cf-img-factories]:media/iot-suite-connected-factory-overview/cf-dashboard-factory.png
[cf-img-map]:media/iot-suite-connected-factory-overview/cf-dashboard-map.png
[cf-img-alerts]:media/iot-suite-connected-factory-overview/cf-dashboard-alerts.png
[cf-img-oee]:media/iot-suite-connected-factory-overview/cf-dashboard-oee.png
[cf-img-kpi]:media/iot-suite-connected-factory-overview/cf-dashboard-kpi.png
[cf-img-tsi-visualization]:media/iot-suite-connected-factory-overview/cf-dashboard-tsi.png
[cf-img-tsi-explorer]:media/iot-suite-connected-factory-overview/tsi-explorer.png
[cf-img-server-browser]: media/iot-suite-connected-factory-overview/cf-dashboard-browser.png
[cf-img-server-choice]: media/iot-suite-connected-factory-overview/cf-select-server.png
[cf-img-server-tree]:media/iot-suite-connected-factory-overview/cf-server-tree.png
[cf-img-publish-node]:media/iot-suite-connected-factory-overview/cf-publish-node1.png
[cf-img-publish-success]:media/iot-suite-connected-factory-overview/cf-publish-success.png
[cf-img-call-command]:media/iot-suite-connected-factory-overview/cf-command-and-control-call.png
[cf-img-call-context]:media/iot-suite-connected-factory-overview/cf-command-and-control-call-button.png
[cf-img-call-success]:media/iot-suite-connected-factory-overview/cf-command-and-control-succeed.png
[img-cf-portal]:media/iot-suite-connected-factory-overview/cf-resource-group.png
[cf-img-alert-filter]:media/iot-suite-connected-factory-overview/cf-filter.png
[cf-img-alert-filter-funnel]:media/iot-suite-connected-factory-overview/cf-filter-funnel.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-portal]: http://portal.azure.com/
[lnk-cfgithub]: https://github.com/Azure/azure-iot-connected-factory
[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md