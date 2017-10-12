---
title: Demo in eigen tempo over Serviceoverzicht | Microsoft Docs
description: Serviceoverzicht is een oplossing in Operations Management Suite (OMS) die automatisch toepassingsonderdelen op Windows- en Linux-systemen detecteert en de communicatie tussen services toewijst.  Dit is een demo in eigen tempo over hoe u Serviceoverzicht kunt gebruiken om een gesimuleerd probleem in een webtoepassing te identificeren en te diagnosticeren.
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: bwren
ms.openlocfilehash: c3548d24c74f8ad865b22d6af3490d0b5cc77a84
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="operations-management-suite-oms-self-paced-demo---service-map"></a>OMS-demo (Operations Management Suited-demo) in eigen tempo - Serviceoverzicht
Deze demo in eigen tempo laat zien hoe u de oplossing [Serviceoverzicht](operations-management-suite-service-map.md) in Operations Management Suite (OMS) kunt gebruiken om een gesimuleerd probleem in een webtoepassing te identificeren en te diagnosticeren.  Serviceoverzicht ontdekt automatisch toepassingsonderdelen op Windows- en Linux-systemen en wijst de communicatie tussen services toe.  De oplossing voegt ook gegevens samen die zijn verzameld door andere OMS-services, om u te helpen bij het analyseren van prestaties en het identificeren van problemen.  U gaat ook gebruikmaken van [zoeken in opdrachten in Log Analytics](../log-analytics/log-analytics-log-searches.md), waarmee u de verzamelde gegevens verder kunt bestuderen om de hoofdoorzaak van een probleem te achterhalen.


## <a name="scenario-description"></a>Scenariobeschrijving
U hebt zojuist een melding ontvangen dat de ACME Customer Portal-toepassing prestatieproblemen heeft.  De enige informatie die u hebt, is dat de problemen vandaag om circa 4:00 AM PST zijn begonnen.  U weet niet precies van welke onderdelen de portal nog meer afhankelijk is dan van een reeks webservers.  

## <a name="components-and-features-used"></a>Gebruikte onderdelen en functies
- [Oplossing Serviceoverzicht](operations-management-suite-service-map.md)
- [Zoeken in logboeken van Log Analytics](../log-analytics/log-analytics-log-searches.md)


## <a name="walk-through"></a>Overzicht

### <a name="1-connect-to-the-oms-experience-center"></a>1. Verbinding maken met het OMS Experience Center
In dit overzicht wordt gebruikgemaakt van het [Operations Management Suite Experience Center](https://experience.mms.microsoft.com/), waarmee u beschikt over een volledige OMS-omgeving met voorbeeldgegevens. U begint met deze koppeling te volgen, uw gegevens in te voeren en het scenario **Insight and Analytics** te selecteren.


### <a name="2-start-service-map"></a>2. Serviceoverzicht starten
Start de oplossing Serviceoverzicht door op de tegel **Serviceoverzicht** te klikken.

![Tegel Serviceoverzicht](media/operations-management-suite-walkthrough-servicemap/tile.png)

De Serviceoverzicht-console wordt geopend.  In het linkerdeelvenster ziet u een lijst van computers in uw omgeving waarop de Serviceoverzicht-agent is geïnstalleerd.  In deze lijst selecteert u de computer die u wilt weergeven.

![Lijst met computers](media/operations-management-suite-walkthrough-servicemap/computer-list.png)


### <a name="3-view-computer"></a>3. Computer weergeven
We weten dat de webservers AcmeWFE001 en AcmeWFE002 heten, dus dit is een plausibele plaats om te starten.  Klik op **AcmeWFE001**.  Nu wordt het overzicht weergegeven voor AcmeWFE001 en alle bijbehorende afhankelijkheden.  U ziet welke processen op de geselecteerde computer worden uitgevoerd en met welke externe services deze communiceren.

![Webserver](media/operations-management-suite-walkthrough-servicemap/web-server.png)

Omdat we meer willen weten over de prestaties van de webtoepassing, klikt u op het proces **AcmeAppPool (IIS App Pool)**.  De details van dit proces worden weergegeven en de afhankelijkheden worden gemarkeerd.  

![Pool van toepassingen](media/operations-management-suite-walkthrough-servicemap/app-pool.png)


### <a name="4-change-time-window"></a>4. Tijdvenster wijzigen

We hebben vernomen dat het probleem om 4:00 uur vanochtend is begonnen. Daarom gaan we kijken wat er rond dat tijdstip is gebeurd. Klik op **Tijdsbereik** en verander de tijd in 4:00 AM PST (behoud de huidige datum en pas de waarde aan uw tijdzone aan) met een duur van 20 minuten.

![Tijdkiezer](./media/operations-management-suite-walkthrough-servicemap/time-picker.png)


### <a name="5-view-alert"></a>5. Waarschuwing weergeven

U ziet nu dat er bij de afhankelijkheid **acmetomcat** een waarschuwing wordt weergegeven; dit zou dus het probleem kunnen zijn.  Klik op het waarschuwingspictogram in **acmetomcat** om de details van de waarschuwing weer te geven.  U ziet dat het CPU-gebruik kritiek is en daarom gaat u dit gegeven uitbreiden voor meer details.  Dit is waarschijnlijk de oorzaak van de slechte prestaties. 

![Waarschuwing](./media/operations-management-suite-walkthrough-servicemap/alert.png)


### <a name="6-view-performance"></a>6. Prestaties weergeven

U gaat **acmetomcat** nader bekijken.  Klik in de rechterbovenhoek van **acmetomcat** en selecteer **Servertoewijzing delen** om de details en afhankelijkheden van deze computer weer te geven. Nu kunt u deze prestatiemeteritems iets beter bekijken om ons vermoeden te verifiëren.  Ga naar het tabblad **Prestaties** om de [prestatiemeteritems te bekijken die door Log Analytics zijn verzameld](../log-analytics/log-analytics-data-sources-performance-counters.md) gedurende het tijdsbereik.  U ziet periodieke pieken voor de processor en het geheugen.

![Prestaties](./media/operations-management-suite-walkthrough-servicemap/performance.png)


### <a name="7-view-change-tracking"></a>7. Wijzigingen bijhouden weergeven
Nu gaat u uitzoeken waardoor deze pieken in het gebruik worden veroorzaakt.  Klik op het tabblad **Samenvatting**.  Hier vindt u informatie over de computer die door OMS is verzameld, zoals mislukte verbindingen, kritieke waarschuwingen en wijzigingen in de software.  Secties met relevante recente informatie zijn al uitgevouwen en u kunt ook alle overige secties uitvouwen als u de betreffende informatie wilt inspecteren.


Vouw **Wijzigingen bijhouden** open als dit nog niet is gebeurd.  Hier vindt u informatie die door de oplossing [Wijzigingen bijhouden](../log-analytics/log-analytics-change-tracking.md) is verzameld.  Het lijkt erop dat er in dit tijdvenster een softwarewijziging heeft plaatsgevonden.  Klik op **Software** voor meer details.  Even na 4:00 uur is er een back-upproces aan de computer toegevoegd. Dit lijkt de oorzaak te zijn van het overmatig gebruik van resources.

![Wijzigingen bijhouden](./media/operations-management-suite-walkthrough-servicemap/change-tracking.png)



### <a name="8-view-details-in-log-search"></a>8. Details weergeven in Zoeken in logboeken
U kunt dit verder controleren door de gedetailleerde prestatiegegevens te bekijken die zijn verzameld in de Log Analytics-opslagplaats.  Klik nogmaals op het tabblad **Waarschuwingen** en vervolgens op een van de waarschuwingen in **Hoog CPU-verbruik**.  Klik op **Weergeven in Zoeken in logboeken**.  Hiermee opent u het venster Zoeken in logboeken, waar u [zoekopdrachten](../log-analytics/log-analytics-log-searches.md) kunt starten naar gegevens die in de opslagplaats zijn opgeslagen.  Serviceoverzicht heeft al een query voor ons ingevuld voor het ophalen van de waarschuwing waarin we zijn geïnteresseerd.  

![Zoekopdrachten in logboeken](./media/operations-management-suite-walkthrough-servicemap/log-search.png)


### <a name="9-open-saved-search"></a>9. Opgeslagen zoekopdracht openen
Misschien kunt u meer informatie krijgen over de prestatieverzameling die deze waarschuwing heeft gegenereerd, om te controleren of de problemen inderdaad zijn veroorzaakt door dit back-upproces.  Wijzig het tijdsbereik in **6 uur**.  Klik vervolgens op **Favorieten** en blader omlaag naar de opgeslagen zoekopdrachten voor **Serviceoverzicht**.  Dit zijn query's die specifiek voor deze analyse zijn gemaakt.  Klik op **Top 5 Processes by CPU for acmetomcat**.

![Opgeslagen zoekopdracht](./media/operations-management-suite-walkthrough-servicemap/saved-search.png)


Deze query retourneert een lijst van de vijf processen met het meest intensieve processorgebruik op **acmetomcat**.  U kunt de query inspecteren voor een inleiding tot de querytaal die wordt gebruikt voor zoeken in logboeken.  Als u geïnteresseerd bent in de processen op andere computers, kunt u de query wijzigen om die informatie op te halen.

In dit geval ziet u dat het back-upproces consistent ongeveer 60% van de CPU van de toepassingsserver verbruikt.  Het is duidelijk dat dit nieuwe proces verantwoordelijk is voor het prestatieprobleem.  De oplossing is natuurlijk om deze nieuwe back-upsoftware van de toepassingsserver te verwijderen.  Met behulp van Desired State Configuration (DSC), dat door Azure Automation wordt beheerd, kunt u zelfs beleidsregels definiëren die ervoor zorgen dat dit proces nooit wordt uitgevoerd op dergelijke kritieke systemen.


## <a name="summary-points"></a>Overzichtspunten
- [Serviceoverzicht](operations-management-suite-service-map.md) geeft een beeld van de gehele toepassing, zelfs als u niet alle servers en afhankelijkheden kent.
- Serviceoverzicht toont gegevens die door andere OMS-oplossingen zijn verzameld, om u te helpen bij het identificeren van problemen met uw toepassing en de onderliggende infrastructuur.
- Met [Zoeken in logboeken](../log-analytics/log-analytics-log-searches.md) kunt u inzoomen op specifieke gegevens die zijn verzameld in de Log Analytics-opslagplaats.    

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Serviceoverzicht](operations-management-suite-service-map.md).
- Serviceoverzicht [implementeren en configureren](operations-management-suite-service-map-configure.md).
- Meer informatie over [Log Analytics](../log-analytics/log-analytics-overview.md), dat is vereist voor Serviceoverzicht en dat de operationele gegevens bewaart die door agents zijn opgeslagen.