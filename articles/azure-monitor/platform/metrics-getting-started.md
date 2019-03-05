---
title: Aan de slag met Azure metrics explorer
description: Informatie over het maken van uw eerste grafiek met metrische gegevens met metrische gegevens van Azure explorer.
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: a94e53644b2032af7be1575cd13807e0484eb182
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57344777"
---
# <a name="getting-started-with-azure-metrics-explorer"></a>Aan de slag met Azure Metrics Explorer

## <a name="where-do-i-start"></a>Waar moet ik beginnen

> [!NOTE]
> In dit artikel behandelt hoofdconcepten om nieuwe gebruikers aan de slag met de Azure Monitor metrics explorer. Zie voor meer documentatie en informatie over geavanceerde instellingen en metrische gegevens gedetailleerde, [geavanceerde functies van Azure Monitor metrics explorer](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts).

U kunt de metrics explorer gebruiken als u voor het onderzoeken van de status en het gebruik van uw resources. Start in de volgende volgorde:

1. Begin met [verzamelen van een resource en een metrische waarde](#creating-your-first-metric-chart) en ziet u een eenvoudige grafiek. Vervolgens [Selecteer een tijdsbereik](#picking-time-range) die relevant is voor uw onderzoek.

1. Nadat u meer over eenvoudige grafieken, kunt u proberen [dimensiefilters zijn toegepast en splitsen](#applying-dimension-filters-and-splitting). De filters en splitsen kunt u voor het analyseren van welke segmenten van de metrische gegevens van bijdragen aan de algemene metrische waarde en mogelijke uitbijters te identificeren.

1. Gebruik [geavanceerde instellingen](#advanced-chart-settings-and-next-steps) om aan te passen van de grafiek voordat u kunt vastmaken aan dashboards. [Waarschuwingen configureren](alerts-metric-overview.md) meldingen ontvangen wanneer de metrische waarde groter is dan of onder een drempelwaarde komt.

## <a name="create-your-first-metric-chart"></a>Uw eerste grafiek met metrische gegevens maken

Voor het maken van een grafiek met metrische gegevens van uw resource, resourcegroep, abonnement of Azure Monitor weergeven, opent u de **metrische gegevens** tabblad en volg deze stappen:

1. Met behulp van de objectkiezer, selecteer de resource waarvoor u wenst te metrische gegevens te verzamelen. (De resource is vooraf geselecteerd als u hebt geopend **metrische gegevens** in de context van een specifieke resource).

    > ![Een resource selecteren](./media/metrics-getting-started/resource-picker.png)

2. Voor sommige resources, moet u een naamruimte kiezen. De naamruimte is een manier om te organiseren van metrische gegevens, zodat u ze eenvoudig kunt terugvinden. Storage-accounts hebben bijvoorbeeld afzonderlijke naamruimten voor het opslaan van bestanden, tabellen, Blobs en wachtrijen metrische gegevens. Veel resourcetypen slechts hebben één naamruimte.

3. Selecteer een metrische waarde uit een lijst met beschikbare metrische gegevens.

    > ![Een metrische waarde selecteren](./media/metrics-getting-started/metric-picker.png)

4. U kunt eventueel de metrische aggregatie wijzigen. Bijvoorbeeld, kunt u de grafiek om minimale, maximale en gemiddelde waarden van de metrische gegevens weer te geven.

> [!NOTE]
> Gebruik de **metrische waarde toevoegen** knop en Herhaal deze stappen als u wilt zien van meerdere metrische gegevens in dezelfde grafiek getekend. Voor meerdere diagrammen in één weergave, selecteert u de **toevoegen grafiek** bovenaan op de knop.

## <a name="pick-a-time-range"></a>Kies een tijdsbereik

De grafiek toont standaard de meest recente 24 uur van metrische gegevens. Gebruik de **tijdkiezer** Configuratiescherm om het tijdsbereik wijzigen, Inzoomen of uitzoomen op uw grafiek. 

![Paneel voor waardenbereik wijzigen-tijd](./media/metrics-getting-started/time-picker.png)

## <a name="apply-dimension-filters-and-splitting"></a>Dimensiefilters en splitsing van toepassing

[Filteren](metrics-charts.md#apply-filters-to-charts) en [splitsen](metrics-charts.md#apply-splitting-to-a-chart) zijn krachtige diagnostische hulpprogramma's voor de metrische gegevens die dimensies hebben. Deze functie laten zien hoe verschillende metrische segmenten ("dimensiewaarden') van invloed zijn op de totale waarde van de metrische gegevens en kunnen u mogelijke uitbijters te identificeren.

- **Filteren** Hiermee kunt u opgeven welke dimensiewaarden zijn opgenomen in de grafiek. U wilt bijvoorbeeld weergeven van geslaagde aanvragen bij het uitzetten van de *serverreactietijd* metrische gegevens. U moet de filer toepassen op de *succes van aanvraag* dimensie. 

- **Splitsen** besturingselementen of het diagram toont de afzonderlijke regels voor elke waarde van een dimensie of waarden samen tot één regel. U kunt bijvoorbeeld zien van één regel voor een gemiddelde reactietijd voor alle serverinstanties of Zie afzonderlijke regels voor elke server. U moet toepassen splitsen op de *server-exemplaar* dimensie om te zien van afzonderlijke regels.

Zie [voorbeelden van de grafieken](metric-chart-samples.md) dat hebt filteren en te splitsen toegepast. Het artikel bevat dat de stappen zijn gebruikt voor het configureren van de grafieken.

## <a name="advanced-chart-settings-and-next-steps"></a>Geavanceerde instellingen en de volgende stappen

U kunt de grafiekstijl van de, titel, aanpassen en geavanceerde instellingen te wijzigen. Wanneer u klaar bent met aanpassingen, deze vastmaken aan een dashboard aan uw werk op te slaan. U kunt ook waarschuwingen voor metrische gegevens configureren. Ga als volgt [productdocumentatie](metrics-charts.md) voor meer informatie over deze en andere geavanceerde functies van Azure Monitor metrics explorer.

## <a name="next-steps"></a>Volgende stappen

* [Een lijst met beschikbare metrische gegevens voor Azure-services](metrics-supported.md)
* [Meer informatie over geavanceerde functies van Metric Explorer](metrics-charts.md)
* [Zie de voorbeelden van geconfigureerde grafieken](metric-chart-samples.md)