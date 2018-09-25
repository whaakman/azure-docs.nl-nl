---
title: Azure Monitor metrics explorer
description: Meer informatie over nieuwe functies in Azure Monitor Metrics Explorer
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2017
ms.author: vitaly.gorbenko
ms.component: metrics
ms.openlocfilehash: 21b0029ff12915c8416ad2366fbf6c45ddfaa288
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978414"
---
# <a name="azure-monitor-metrics-explorer"></a>Azure Monitor Metrics Explorer

Deze procedures voor beschrijving van de volgende generatie Azure Monitor Metrics voor grafieken-ervaring die momenteel in openbare preview. De nieuwe ervaring biedt ondersteuning voor rendering grafieken voor zowel multidimensionale metrische gegevens en eenvoudige metrische gegevens met er zijn geen dimensies. U kunt diagrammen die overlay metrische gegevens van andere resourcetypen, meerdere resourcegroepen en abonnementen. De grafieken met multi-dimensionale metrische gegevens kunnen worden aangepast door dimensiefilters zijn toegepast, evenals groeperen. Een grafiek, met inbegrip van aangepaste grafieken kan worden vastgemaakt aan dashboards.

Als u naar informatie over de oude ervaring die alleen ondersteuning biedt voor eenvoudige metrische gegevens met er zijn geen dimensies zoekt,. Zie de sectie 'Toegang tot metrische gegevens via de portal' de [overzicht van de metrische gegevens van de Microsoft Azure handleiding](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics).

## <a name="what-is-azure-monitor-metrics-explorer"></a>Wat is Azure Monitor Metrics Explorer?

Azure Monitor Metrics Explorer is een onderdeel van de Microsoft Azure-portal die u kunt grafieken te tekenen, visueel correleren van trends en onderzoeken van pieken en dalen in de metrische waarden. Metrics Explorer is een essentiële beginpunt voor het onderzoeken van verschillende prestaties en van beschikbaarheidsproblemen met uw toepassingen en infrastructuur die wordt gehost in Azure of bewaakt door Azure Monitor-services. 

## <a name="what-are-metrics-in-azure"></a>Wat zijn metrische gegevens in Azure?

Metrische gegevens in Microsoft Azure, zijn de reeks meetwaarden en aantallen die worden verzameld en opgeslagen na verloop van tijd. Er zijn metrische gegevens voor standard (of 'platform') en aangepaste metrische gegevens. De standaard metrische gegevens worden aan u vertrekt door de Azure-platform zelf. Standaard metrische gegevens weer de status en gebruik statistische gegevens van uw Azure-resources. Terwijl aangepaste metrische gegevens naar Azure worden verzonden door uw toepassingen met behulp van de [Application Insights-API voor aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics). Aangepaste metrische gegevens worden opgeslagen in de Application Insights-resources samen met andere specifieke metrische gegevens voor een toepassing.



## <a name="how-do-i-create-a-new-chart"></a>Hoe kan ik een nieuwe grafiek maken?

   > [!NOTE]
   > Enkele van de functies van de oude ervaring voor metrische gegevens nog niet beschikbaar in de nieuwe Metrics Explorer. Terwijl de nieuwe ervaring in preview is, kunt u doorgaan met behulp van de oude (niet-dimensionale) weergave voor metrische gegevens van Azure Monitor. 

1. De Azure-portal openen
2. Navigeer naar de nieuwe **Monitor** tabblad, en selecteer vervolgens **metrische gegevens (preview)**.

   ![De voorbeeldafbeelding metrische gegevens](./media/monitoring-metric-charts/0001.png)

3. De **metrische selector** automatisch worden geopend voor u. Kies een resource in de lijst om de bijbehorende metrische gegevens weer te geven. Alleen bronnen met metrische gegevens worden weergegeven in de lijst.

   ![De voorbeeldafbeelding metrische gegevens](./media/monitoring-metric-charts/0002.png)

   > [!NOTE]
   >Als u meer dan één Azure-abonnement hebt, Metrics Explorer worden de resources in alle abonnementen die zijn geselecteerd in de Portal-instellingen -> Filter door de abonnementenlijst met. Als u wilt wijzigen, klikt u op het tandwielpictogram voor instellingen van Portal boven op het scherm en selecteer welke abonnementen die u wilt gebruiken.

4. Voor sommige typen resources (dat wil zeggen Storage-Accounts en virtuele Machines), voordat u selecteert een metrische waarde moet u een **Namespace**. Elke naamruimte heeft een eigen set metrische gegevens die relevant zijn voor alleen deze naamruimte, en niet voor andere naamruimten.

   Elke Azure-Storage heeft bijvoorbeeld metrische gegevens voor subservices 'Blobs', 'Files', 'Wachtrijen' en 'Tabellen', alle onderdelen van het storage-account zijn. De metriek "aantal Wachtrijberichten" is echter op een natuurlijke manier van toepassing op de subservice 'Wachtrij' en niet op een andere opslag account subservices.

   ![De voorbeeldafbeelding metrische gegevens](./media/monitoring-metric-charts/0003.png)

5. Selecteer een metrische waarde in de lijst. Als u een gedeeltelijke naam van de metrische gegevens die u wilt weten, kunt u beginnen te typen in een gefilterde lijst met beschikbare metrische gegevens zien:

   ![De voorbeeldafbeelding metrische gegevens](./media/monitoring-metric-charts/0004.png)

6. Na het selecteren van een metrische waarde, worden de grafiek weergegeven met de standaardaggregatie voor de geselecteerde metrische gegevens. Op dit punt hoeft u alleen op weg van de **metrische gegevens selector** om deze te sluiten. U kunt eventueel ook de grafiek overschakelen naar een andere aggregatie. Voor sommige metrische gegevens kunt schakelen tussen aggregatie u kiezen welke waarde u wilt zien op de grafiek. U kunt bijvoorbeeld schakelen tussen de gemiddelde, minimale en maximale waarden. 

7. Door te klikken op het pictogram metrische gegevens toevoegen ![pictogram van metrische gegevens](./media/monitoring-metric-charts/icon001.png) en stap 3-6 herhalende kunt u meer metrische gegevens in dezelfde grafiek toevoegen.

   > [!NOTE]
   > Normaal gesproken u niet metrische gegevens met verschillende eenheden (dat wil zeggen "milliseconden" en 'kilobytes') of met aanmerkelijk verschillende schalen in een grafiek hebt. In plaats daarvan kunt u overwegen meerdere diagrammen. Klik op de knop grafiek toevoegen aan meerdere diagrammen maken in Metrics Explorer.

## <a name="how-do-i-apply-filters-to-the-charts"></a>Hoe ik filters toepassen op de grafieken?

U kunt filters toepassen op de grafieken met metrische gegevens met dimensies. Bijvoorbeeld, als de metriek "Aantal transacties" een dimensie heeft, "reactietype', waarmee wordt aangegeven of het antwoord van transacties is geslaagd of mislukt vervolgens filteren op deze dimensie wilt tekenen regel in een grafiek voor alleen geslaagde (of alleen mislukt) transacties. 

### <a name="to-add-a-filter"></a>Een filter toevoegen:

1. Klik op het pictogram Filter toevoegen ![filterpictogram](./media/monitoring-metric-charts/icon002.png) boven de grafiek

2. Selecteer welke dimensie (eigenschap) die u wilt filteren

   ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/0006.png)

3. Selecteer welke dimensiewaarden die u opnemen wilt, wanneer het uitzetten van de grafiek (dit voorbeeld wordt gefilterd op de opslagaccount-transacties):

   ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/0007.png)

4. Na het selecteren van de filterwaarden, klikt u op weg van de Selector Filter om deze te sluiten. Het diagram toont nu het aantal opslagtransacties zijn mislukt:

   ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/0008.png)

5. U kunt de stappen 1-4 meerdere filters toepassen op de dezelfde grafieken herhalen.

## <a name="how-do-i-segment-a-chart"></a>Hoe ik een grafiek segment?

U kunt een metrische waarde splitsen op dimensie om te visualiseren hoe verschillende segmenten van de metrische gegevens vergelijken ten opzichte van elkaar en identificeren van de afgelegen segmenten van een dimensie. 

### <a name="to-segment-a-chart"></a>Een grafiek in segmenten:

1. Klik op het pictogram groepering toevoegen  ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/icon003.png) boven de grafiek.
 
   > [!NOTE]
   > U kunt meerdere filters, maar slechts één groepering op één grafiek hebben.

2. Kies een dimensie waarop u wilt indelen van de grafiek: 

   ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/0010.png)

   Het diagram toont nu nu meerdere regels, één voor elk segment van dimensie:

   ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/0012.png)

3. Klikt u op weg van de **groepering Selector** om deze te sluiten.

   > [!NOTE]
   > Gebruik zowel filteren en groeperen op dezelfde dimensie verborgen segmenten die zijn niet relevant voor uw scenario en grafieken gemakkelijker te lezen.

## <a name="how-do-i-pin-charts-to-dashboards"></a>Hoe ik grafieken aan dashboards vastmaken?

Na het configureren van de grafieken, kunt u deze toevoegen aan de dashboards, zodat u kunt opnieuw mogelijk in de context van andere bewaking telemetrie bekijken of met uw team delen. 

Voor een geconfigureerde grafiek vastmaken aan een dashboard:

Na het configureren van de grafiek, klik op de **grafiek acties** menu in de rechter hoek van de grafiek belangrijkste, en klikt u op **vastmaken aan dashboard**.

   ![de installatiekopie van de metrische gegevens](./media/monitoring-metric-charts/0013.png)

## <a name="next-steps"></a>Volgende stappen

  Lezen [het maken van aangepaste KPI-dashboards](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) voor meer informatie over de aanbevolen procedures voor het maken van bruikbare dashboards met metrische gegevens.