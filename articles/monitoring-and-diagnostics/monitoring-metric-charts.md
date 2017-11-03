---
title: Monitor voor Azure Metrics Explorer | Microsoft Docs
description: Meer informatie over nieuwe functies in Azure Monitor Metrics Explorer
author: vgorbenko
manager: Victor.Mushkatin
editor: mrbullwinkle
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2397596a-071f-4d49-8893-bec5f735bd7b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: vitaly.gorbenko
ms.openlocfilehash: 537dd6d64fe49093dd73d8040cde5a9153a7bd5c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-monitor-metrics-explorer"></a>Monitor voor Azure Metrics Explorer

Deze procedures voor beschrijving van de volgende generatie Azure Monitor metrische gegevens voor grafieken ervaring die momenteel is in de openbare preview. De nieuwe ervaring biedt ondersteuning voor rendering grafieken voor multidimensionale metrische gegevens en basismetrieken waarvoor geen dimensies. Diagrammen die overlay metrische gegevens van verschillende brontypen, meerdere resourcegroepen en abonnementen kunnen worden uitgezet. De grafieken multidimensionale metrische gegevens kunnen worden aangepast door dimensiefilters zijn toegepast, evenals een groepering. Een grafiek, met inbegrip van aangepaste grafieken kan worden vastgemaakt aan dashboards.

Als u naar informatie over de oude ervaring die uitsluitend basismetrieken waarvoor geen dimensies zoekt ondersteunt, Zie de sectie het gebruiksrecht heeft 'Toegang tot metrische gegevens via de portal' de [overzicht van metrische gegevens van Microsoft Azure handleiding](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics).

## <a name="what-is-azure-monitor-metrics-explorer"></a>Wat is Azure Monitor Metrics Explorer?

Azure Metrics Explorer van de Monitor is een onderdeel van de Microsoft Azure-portal die kan tekenen van grafieken, visueel correleren van trends en pieken onderzoeken en daalt in de metrische waarden. Metrics Explorer is een essentiële startpunt voor het onderzoeken van verschillende prestaties en van beschikbaarheidsproblemen met uw toepassingen en infrastructuur gehost in Azure of door bewaken van de Azure-services wordt gecontroleerd. 

## <a name="what-are-metrics-in-azure"></a>Wat zijn de metrische gegevens in Azure?

Metrische gegevens in Microsoft Azure zijn de reeks meetwaarden en aantallen die worden verzameld en opgeslagen gedurende een bepaalde periode. Er zijn standaard (of 'platform') metrische gegevens en aangepaste metrische gegevens. De metrische gegevens die standaard worden aan u geleverd door het Azure-platform zelf. Standaard metrische gegevens weerspiegelen de statistieken van de status en informatie over het gebruik van uw Azure-resources. Terwijl aangepaste metrische gegevens naar Azure worden verzonden door uw toepassingen met behulp van de [Application Insights-API voor aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics). Aangepaste metrische gegevens worden opgeslagen in de Application Insights-resources samen met andere specifieke metrische gegevens voor een toepassing.

## <a name="what-are-multi-dimensional-metrics"></a>Wat zijn multidimensionale metrische gegevens?

Veel van de Azure resources nu tonen multidimensionale metrische gegevens. Deze metrische gegevens bijhouden meerdere reeksen met waarden voor een of veel benoemde dimensies. Een metriek 'beschikbare schijfruimte' kan bijvoorbeeld een dimensie met de naam 'Schijf' met waarden 'C:', 'D:', waardoor de beschikbare schijfruimte op alle stations of voor elke schijf afzonderlijk weer zou hebben. 

Het volgende voorbeeld ziet u twee gegevenssets voor een hypothetische metriek 'Netwerkdoorvoer' genoemd. De eerste gegevensset heeft geen dimensies. De tweede gegevensset ziet u de waarden met twee dimensies, 'IP-adres' en 'Richting':

### <a name="network-throughput"></a>Netwerkdoorvoer
(Deze metriek heeft geen dimensies)

 |tijdstempel        | Metrische waarde | 
   | ------------- |:-------------| 
   | 8/9/2017 8:14 | 1,331.8 kbps | 
   | 8/9/2017 8:15 | 1,141.4 kbps |
   | 8/9/2017 8:16 | 1,110.2 kbps |

Met deze metriek niet-dimensionale kunt alleen een eenvoudige vraag antwoord, zoals 'Wat is mijn netwerkdoorvoer op een bepaald moment?'

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>Netwerkdoorvoer + twee dimensies (IP '-' en 'Richting')

| tijdstempel          | Dimensie '-IP | Dimensie 'Richting' | Metrische waarde| 
   | ------------- |:-----------------|:------------------- |:-----------|  
   | 8/9/2017 8:14 | IP = "192.168.5.2" | Richting = "Verzenden"    | 646.5 kbps |
   | 8/9/2017 8:14 | IP = "192.168.5.2" | Richting = 'Ontvangen' | 420.1 kbps |
   | 8/9/2017 8:14 | IP = "10.24.2.15"  | Richting = "Verzenden"    | 150,0 kbps | 
   | 8/9/2017 8:14 | IP = "10.24.2.15"  | Richting = 'Ontvangen' | Met 115,2 kbps |
   | 8/9/2017 8:15 | IP = "192.168.5.2" | Richting = "Verzenden"    | 515.2 kbps |
   | 8/9/2017 8:15 | IP = "192.168.5.2" | Richting = 'Ontvangen' | 371.1 kbps |
   | 8/9/2017 8:15 | IP = "10.24.2.15"  | Richting = "Verzenden"    | 155.0 kbps |
   | 8/9/2017 8:15 | IP = "10.24.2.15"  | Richting = 'Ontvangen' | 100.1 kbps |

Met deze metriek kan beantwoorden vragen zoals 'Wat is de doorvoer van het netwerk voor elk IP-adres?' en "hoeveel gegevens zijn verzonden en ontvangen?" Multidimensionale metrische gegevens uitvoeren vergeleken met niet-dimensionale metrische gegevens van aanvullende analytische en diagnostische waarde. 

## <a name="how-do-i-create-a-new-chart"></a>Hoe maak ik een nieuwe grafiek

   > [!NOTE]
   > Sommige van de functies van de ervaring van de oude metrische gegevens nog niet beschikbaar in de nieuwe Metrics Explorer. Terwijl de nieuwe ervaring preview wordt, kunt u doorgaan met de oude (niet-dimensionale) metrische gegevens weergave van Azure-Monitor. 

1. De Azure portal openen
2. Navigeer naar de nieuwe **Monitor** tabblad en selecteer vervolgens **metrische gegevens (preview)**.

   ![De voorbeeldafbeelding metrische gegevens](./media/monitoring-metric-charts/001.png)

3. De **metrische selector** automatisch voor u geopend worden. Kies een bron in de lijst om de bijbehorende metrische gegevens weer te geven. Alleen bronnen met metrische gegevens worden weergegeven in de lijst.

   ![De voorbeeldafbeelding metrische gegevens](./media/monitoring-metric-charts/002.png)

   > [!NOTE]
   >Als u meer dan één Azure-abonnement hebt,-Filter in Metrics Explorer worden uit de resources in alle abonnementen die zijn geselecteerd in de Portal-instellingen > door de lijst met abonnementen. Als u wilt wijzigen, klikt u op de Portal tandwielpictogram Instellingenpictogram boven op het scherm en selecteer welke abonnementen u wilt gebruiken.

4. Voor bepaalde typen resources (dat wil zeggen Storage-Accounts en virtuele Machines), voordat u een metriek selecteert moet u een **Sub Service**. Elke sub-service heeft een eigen set van metrische gegevens die relevant zijn voor deze service sub alleen en niet voor andere sub-services.

   Elke Azure-opslag heeft bijvoorbeeld metrische gegevens voor subservices 'Blobs', 'Files', 'Wachtrijen' en 'Tabellen', alle onderdelen van het opslagaccount zijn. De metriek ' aantal wachtrij bericht ' is echter natuurlijk van toepassing op de subservice 'Wachtrij' en niet op een andere opslag account subservices.

   ![De voorbeeldafbeelding metrische gegevens](./media/monitoring-metric-charts/003.png)

5. Selecteer een waarde in de lijst. Als u een gedeeltelijke naam van de metrische gegevens die u wilt, kunt u begint te typen in een gefilterde lijst met beschikbare metrische gegevens te zien:

   ![De voorbeeldafbeelding metrische gegevens](./media/monitoring-metric-charts/004.png)

6. Na het selecteren van een metriek wordt de grafiek weergeven met de standaard aggregatie voor de geselecteerde waarde. Op dit moment hoeft u alleen op weg van de **metrische gegevens selector** om deze te sluiten. U kunt eventueel ook de grafiek overschakelen naar een andere aggregatie. Voor bepaalde maatstaven voor kunt aggregatie overschakelen u kiezen welke waarde u wilt zien op de grafiek. U kunt bijvoorbeeld schakelen tussen de gemiddelde, minimale en maximale waarden. 

7. Door te klikken op het pictogram metriek toevoegen ![metrische pictogram](./media/monitoring-metric-charts/icon001.png) en stap 3-6 herhalende kunt u meer metrische gegevens kunt toevoegen in hetzelfde diagram.

   > [!NOTE]
   > Wilt u waarschijnlijk geen metrische gegevens met verschillende eenheden (dat wil zeggen 'milliseconden' en 'kilobytes') of met aanmerkelijk verschilde scale hebben op één grafiek. Overweeg in plaats daarvan het gebruik van meerdere grafieken. Klik op de knop grafiek toevoegen voor het maken van meerdere grafieken in Metrics Explorer.

## <a name="how-do-i-apply-filters-to-the-charts"></a>Hoe ik filters toepassen op de grafieken?

U kunt filters toepassen op de grafieken die metrische gegevens met dimensies weergeven. Bijvoorbeeld, als de metriek 'transactieaantal' een dimensie heeft, 'Reactietype', waarmee wordt aangegeven of het antwoord van transacties is geslaagd of mislukt vervolgens filteren op deze dimensie zou tekenen een regel grafiek voor alleen geslaagde (of alleen mislukte) transacties. 

### <a name="to-add-a-filter"></a>Een filter toevoegen:

1. Klik op het pictogram Filter toevoegen ![filterpictogram](./media/monitoring-metric-charts/icon002.png) boven de grafiek

2. Selecteer welke dimensie (eigenschap) die u wilt filteren

   ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/006.png)

3. Selecteer welke dimensiewaarden die u opnemen wilt, wanneer het uitzetten van de grafiek (in dit voorbeeld ziet de geslaagde opslagtransacties uitgefilterd):

   ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/007.png)

4. Na het selecteren van de filterwaarden, klikt u op weg van de Selector Filter te sluiten. Nu de grafiek wordt weergegeven hoeveel opslagtransacties is mislukt:

   ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/008.png)

5. U kunt de stappen 1-4 tot meerdere filters toepassen op de dezelfde grafieken herhalen.

## <a name="how-do-i-segment-a-chart"></a>Hoe ik een grafiek segmenteren?

U kunt een metriek gesplitst in dimensie visualiseren hoe verschillende segmenten van de metrische gegevens vergelijken ten opzichte van elkaar en identificeren van de afgelegen segmenten van een dimensie. 

### <a name="to-segment-a-chart"></a>Een grafiek in segmenten:

1. Klik op het pictogram groepering toevoegen  ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/icon003.png) boven het diagram.
 
   > [!NOTE]
   > U kunt meerdere filters, maar slechts één groepering hebben op één grafiek.

2. Kies een dimensie die u wilt segmenteren van uw grafiek: 

   ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/010.png)

   De grafiek toont nu nu meerdere regels, één voor elk segment van dimensie:

   ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/012.png)

3. Klikt u op weg van de **groepering Selector** om deze te sluiten.

   > [!NOTE]
   > Filteren en groeperen op dezelfde dimensie gebruiken voor het verbergen van de segmenten die zijn niet relevant voor uw scenario en grafieken gemakkelijker te lezen.

## <a name="how-do-i-pin-charts-to-dashboards"></a>Hoe ik grafieken aan dashboards vastmaken?

Mogelijk wilt u na het configureren van de grafieken toe te voegen aan de dashboards zodat u kunt opnieuw mogelijk in de context van andere bewaking telemetrie bekijken of met uw team delen. 

Een geconfigureerde grafiek aan een dashboard vastmaken:

Na het configureren van de grafiek, klikt u op de **grafiek acties** menu van rechts bovenste hoek van de grafiek en klik op **vastmaken aan dashboard**.

   ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/013.png)

## <a name="next-steps"></a>Volgende stappen

  Lees [maken van aangepaste dashboards van de KPI](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) voor meer informatie over de aanbevolen procedures voor het maken van bruikbare dashboards met metrische gegevens.