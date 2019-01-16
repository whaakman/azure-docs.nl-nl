---
title: Azure Monitor metrics explorer
description: Meer informatie over nieuwe functies in Azure Monitor Metrics Explorer
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: vitalyg
ms.component: metrics
ms.openlocfilehash: 457c7e8904797955854c4c3e16a631cf6537e2b8
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54330072"
---
# <a name="azure-monitor-metrics-explorer"></a>Azure Monitor Metrics Explorer

Azure Monitor Metrics Explorer is een onderdeel van de Microsoft Azure-portal die u kunt grafieken te tekenen, visueel correleren van trends en onderzoeken van pieken en dalen in de metrische waarden. Metrics Explorer is een essentiële beginpunt voor het onderzoeken van verschillende prestaties en van beschikbaarheidsproblemen met uw toepassingen en infrastructuur die wordt gehost in Azure of bewaakt door Azure Monitor-services.

## <a name="metrics-in-azure"></a>Metrische gegevens in Azure

Metrische gegevens in Microsoft Azure, zijn de reeks meetwaarden en aantallen die worden verzameld en opgeslagen na verloop van tijd. Er zijn metrische gegevens voor standard (of 'platform') en aangepaste metrische gegevens. De standaard metrische gegevens worden aan u vertrekt door de Azure-platform zelf. Standaard metrische gegevens weer de status en gebruik statistische gegevens van uw Azure-resources. Terwijl aangepaste metrische gegevens naar Azure worden verzonden door uw toepassingen met behulp van de [Application Insights-API voor aangepaste gebeurtenissen](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics). Aangepaste metrische gegevens worden opgeslagen in de Application Insights-resources samen met andere specifieke metrische gegevens voor een toepassing.

## <a name="create-a-new-chart"></a>Maak een nieuwe grafiek

1. De Azure-portal openen
2. Navigeer naar de nieuwe **Monitor** tabblad, en selecteer vervolgens **metrische gegevens**.

   ![Afbeelding van de metrische gegevens](./media/metrics-charts/00001.png)

3. De **metrische selector** automatisch worden geopend voor u. Kies een resource in de lijst om de bijbehorende metrische gegevens weer te geven. Alleen bronnen met metrische gegevens worden weergegeven in de lijst.

   ![Afbeelding van de metrische gegevens](./media/metrics-charts/00002.png)

   > [!NOTE]
   >Als u meer dan één Azure-abonnement hebt, Metrics Explorer worden de resources in alle abonnementen die zijn geselecteerd in de Portal-instellingen -> Filter door de abonnementenlijst met. Als u wilt wijzigen, klikt u op het tandwielpictogram voor instellingen van Portal boven op het scherm en selecteer welke abonnementen die u wilt gebruiken.

4. Voor sommige typen resources (Storage-Accounts en virtuele Machines), voordat u selecteert een metrische waarde moet u een **Namespace**. Elke naamruimte heeft een eigen set metrische gegevens die relevant zijn voor alleen deze naamruimte, en niet voor andere naamruimten.

   Elke Azure-Storage heeft bijvoorbeeld metrische gegevens voor subservices 'Blobs', 'Files', 'Wachtrijen' en 'Tabellen', alle onderdelen van het storage-account zijn. De metriek "aantal Wachtrijberichten" is echter op een natuurlijke manier van toepassing op de subservice 'Wachtrij' en niet op een andere opslag account subservices.

   ![Afbeelding van de metrische gegevens](./media/metrics-charts/00003.png)

5. Selecteer een metrische waarde in de lijst. Als u een gedeeltelijke naam van de metrische gegevens die u wilt weten, kunt u beginnen te typen in een gefilterde lijst met beschikbare metrische gegevens zien:

   ![Afbeelding van de metrische gegevens](./media/metrics-charts/00004.png)

6. Na het selecteren van een metrische waarde, worden de grafiek weergegeven met de standaardaggregatie voor de geselecteerde metrische gegevens. Op dit punt hoeft u alleen op weg van de **metrische gegevens selector** om deze te sluiten. U kunt eventueel ook de grafiek overschakelen naar een andere aggregatie. Voor sommige metrische gegevens kunt schakelen tussen aggregatie u kiezen welke waarde u wilt zien op de grafiek. U kunt bijvoorbeeld schakelen tussen de gemiddelde, minimale en maximale waarden. 

7. Door te klikken op **metrische waarde toevoegen** en herhalende stap 3-6, kunt u meer metrische gegevens op de dezelfde grafiek toevoegen.

   > [!NOTE]
   > Normaal gesproken u niet metrische gegevens met verschillende eenheden (dat wil zeggen "milliseconden" en 'kilobytes') of met aanmerkelijk verschillende schalen in een grafiek hebt. In plaats daarvan kunt u overwegen meerdere diagrammen. Klik op de knop grafiek toevoegen aan meerdere diagrammen maken in Metrics Explorer.

## <a name="apply-filters-to-charts"></a>Filters toepassen op grafieken

U kunt filters toepassen op de grafieken met metrische gegevens met dimensies. Bijvoorbeeld, als de metriek "Aantal transacties" een dimensie heeft, "reactietype', waarmee wordt aangegeven of het antwoord van transacties is geslaagd of mislukt vervolgens filteren op deze dimensie wilt tekenen regel in een grafiek voor alleen geslaagde (of alleen mislukt) transacties. 

### <a name="to-add-a-filter"></a>Een filter toevoegen

1. Selecteer **filter toevoegen** boven de grafiek

2. Selecteer welke dimensie (eigenschap) die u wilt filteren

   ![de installatiekopie van de metrische gegevens](./media/metrics-charts/00006.png)

3. Selecteer welke dimensiewaarden die u opnemen wilt, wanneer het uitzetten van de grafiek (dit voorbeeld wordt gefilterd op de opslagaccount-transacties):

   ![de installatiekopie van de metrische gegevens](./media/metrics-charts/00007.png)

4. Na het selecteren van de filterwaarden, klikt u op weg van de Selector Filter om deze te sluiten. Het diagram toont nu het aantal opslagtransacties zijn mislukt:

   ![de installatiekopie van de metrische gegevens](./media/metrics-charts/00008.png)

5. U kunt de stappen 1-4 meerdere filters toepassen op de dezelfde grafieken herhalen.

## <a name="segment-a-chart"></a>Een grafiek in segmenten

U kunt een metrische waarde splitsen op dimensie om te visualiseren hoe verschillende segmenten van de metrische gegevens vergelijken ten opzichte van elkaar en identificeren van de afgelegen segmenten van een dimensie. 

### <a name="to-segment-a-chart"></a>Een grafiek in segmenten

1. Klik op **toepassen splitsen** boven de grafiek.
 
   > [!NOTE]
   > U kunt meerdere filters, maar slechts één splitsen/segmentering waarde kan hebben op één grafiek.

2. Kies een dimensie waarop u wilt indelen van de grafiek:

   ![de installatiekopie van de metrische gegevens](./media/metrics-charts/00010.png)

   Het diagram toont nu nu meerdere regels, één voor elk segment van dimensie:

   ![de installatiekopie van de metrische gegevens](./media/metrics-charts/00012.png)

3. Klikt u op weg van de **groepering Selector** om deze te sluiten.

   > [!NOTE]
   > Filteren en splitsen in dezelfde dimensie gebruiken om te verbergen van de segmenten zijn niet relevant voor uw scenario en grafieken gemakkelijker te lezen.

### <a name="new-alert-rule"></a>Nieuwe waarschuwingsregel

Ook kunt u de criteria die u hebt ingesteld voor het visualiseren van uw metrische gegevens zoals de basis voor de onderliggende logica van een metrische waarde op basis van de waarschuwingsregel. 

Als u klikt op **nieuwe waarschuwingsregel**

![Knop nieuwe waarschuwingsregel rood gemarkeerd](./media/metrics-charts/015.png)

U gaat naar het deelvenster voor het maken van waarschuwingsregel met de grootte van het onderliggende metrische gegevens van de grafiek die vooraf is ingevuld voor het genereren van regels voor aangepaste waarschuwingen te vereenvoudigen.

![Waarschuwingsregel maken](./media/metrics-charts/016.png)

Bekijk dit [artikel](alerts-metric.md) voor meer informatie over het instellen van metrische waarschuwingen.

## <a name="lock-boundaries-of-chart-y-axis"></a>Grenzen van de vergrendeling van grafiek y-as

Vergrendeling van het bereik van de y-as wordt belangrijk wanneer de grafiek kleinere fluctuaties van hogere waarden toont. 

Wanneer het aantal geslaagde aanvragen wordt weergegeven van 99,99% bij minder dan 99,5%, kan er bijvoorbeeld een significante vermindering van de kwaliteit van de service vertegenwoordigen. Echter, dat u een kleine numerieke waarde boven is moeilijk of zelfs onmogelijk van de standaardinstellingen van de grafiek. In dit geval kunt u de laagste grens van de grafiek voor 99%, waardoor deze kleine vervolgkeuzelijst duidelijker vergrendelen. 

Een ander voorbeeld is een schommelingen in het beschikbare geheugen, waarbij de waarde zal technisch nooit contact met u 0. Oplossen van het bereik tot een hogere waarde kan eenvoudiger de val in het beschikbare geheugen te zien. 

Gebruiken voor het beheren van de y-as-bereik, de '...' grafiek van menu en selecteer **grafiek bewerken** voor toegang tot geavanceerde grafiekinstellingen. Wijzig de waarden in de sectie y-as-bereik, of gebruik **automatisch** knop om terug te keren naar de standaardinstellingen.

![de installatiekopie van de metrische gegevens](./media/metrics-charts/00014-manually-set-granularity.png)

> [!WARNING]
> Vergrendelen van de grenzen van de y-as voor de grafieken die verschillende bijhouden telt of opgeteld gedurende een bepaalde tijd (en dus gebruik count, som, minimum of maximum aggregaties) moet u meestal een vaste tijdgranulatie op te geven in plaats van afhankelijk zijn van de automatische standaardwaarden. Dit is nodig omdat de waarden in grafieken wijzigen wanneer de tijdgranulatie wordt automatisch gewijzigd door de gebruiker vergroten of verkleinen browservenster of als een schermresolutie gaan. De resulterende wijzigen in tijd granulariteit effecten het uiterlijk van de grafiek, ongeldig huidige selectie van y-as-bereik.

## <a name="pin-charts-to-dashboards"></a>Grafieken vastmaken aan dashboards

Na het configureren van de grafieken, kunt u deze toevoegen aan de dashboards, zodat u kunt opnieuw mogelijk in de context van andere bewaking telemetrie bekijken of met uw team delen.

Voor een geconfigureerde grafiek vastmaken aan een dashboard:

Na het configureren van de grafiek, klik op de **grafiek acties** menu in de rechter hoek van de grafiek belangrijkste, en klikt u op **vastmaken aan dashboard**.

![de installatiekopie van de metrische gegevens](./media/metrics-charts/00013.png)

## <a name="next-steps"></a>Volgende stappen

  Lezen [het maken van aangepaste KPI-dashboards](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) voor meer informatie over de aanbevolen procedures voor het maken van bruikbare dashboards met metrische gegevens.
