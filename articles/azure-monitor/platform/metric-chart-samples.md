---
title: Voorbeelden van Azure Monitor-grafiek met metrische gegevens
description: Meer informatie over het visualiseren van uw gegevens met Azure Monitor.
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: bbfeb428d38c23955df4497242184499349aecf9
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55303077"
---
# <a name="metric-chart-samples"></a>Voorbeelden van de grafiek met metrische gegevens

De Azure-platform biedt [duizend meeteenheden](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported)veel van dat dimensies hebben. Met behulp van [dimensie filters](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts), toepassen [splitsen](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts), grafiektype beheren en aanpassen van de grafiekinstellingen kunt u krachtige diagnostische weergaven en dashboards die inzicht geven in de status van maken uw infrastructuur en toepassingen. In dit artikel ziet u enkele voorbeelden van de grafieken die u kunt bouwen met behulp van [Metrics Explorer](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts) en worden de vereiste stappen beschreven voor het configureren van elk van deze grafieken.

Wilt u uw voorbeelden grote diagrammen met de wereld delen? Bijdragen aan deze pagina op GitHub en deel uw eigen voorbeelden van grafiek hier!

## <a name="website-cpu-utilization-by-server-instances"></a>Website CPU-gebruik per server-exemplaren

In deze grafiek worden als CPU voor een App Service binnen het toegestane bereik is en ze splitst op per exemplaar om te bepalen of de belasting correct is gedistribueerd. U kunt zien in de grafiek die de app werd uitgevoerd op een exemplaar van één server vóór 06: 00 en klikt u vervolgens omhoog geschaald door een ander exemplaar toe te voegen.

![Lijndiagram van gemiddelde cpu-percentage van de server-exemplaar](./media/metric-chart-samples/cpu-by-instance.png)

### <a name="how-to-configure-this-chart"></a>Het configureren van deze grafiek?

Selecteer uw App Service-resource en zoek de **CPU-Percentage** metrische gegevens. Klik vervolgens op **toepassen splitsen** en selecteer de **exemplaar** dimensie.

## <a name="application-availability-by-region"></a>Beschikbaarheid per regio

Weergeven van uw toepassing beschikbaarheid per regio om te identificeren welke geografische locaties problemen ondervindt. In deze grafiek toont de beschikbaarheid van Application Insights-metriek. U kunt zien dat de bewaakte toepassing geen probleem met de beschikbaarheid van het datacenter VS-Oost heeft, maar het is een probleem gedeeltelijke beschikbaarheid van VS-West en Oost-Azië.

![Grafiek van gemiddelde beschikbaarheid per locaties](./media/metric-chart-samples/availability-run-location.png)

### <a name="how-to-configure-this-chart"></a>Het configureren van deze grafiek?

U moet eerst in te schakelen [Application Insights-beschikbaarheid](https://docs.microsoft.com/azure/azure-monitor/app/monitor-web-app-availability) bewaking voor uw website. Hierna uw Application Insights-resource kiezen en selecteert u de beschikbaarheid van metrische gegevens. Van toepassing op te splitsen de **uitvoeringslocatie** dimensie.

## <a name="volume-of-storage-account-transactions-by-api-name"></a>Volume van de account opslagtransacties door API-naam

De resource van uw opslagaccount is een met een extra volume van transacties. U kunt de metrische gegevens van transacties om te identificeren die verantwoordelijk is voor de overmatige belasting API gebruiken. U ziet dat het volgende diagram wordt geconfigureerd met dezelfde dimensie (API-naam) in filteren en te splitsen als u alleen de API-aanroepen van het belang van de weergave wilt beperken:

![Staafdiagram van API-transacties](./media/metric-chart-samples/transactions-by-api.png)

### <a name="how-to-configure-this-chart"></a>Het configureren van deze grafiek?

Selecteer in de kiezer voor metrische gegevens, uw opslagaccount en de **transacties** metrische gegevens. Grafiektype overschakelen **staafdiagram**. Klik op **toepassen splitsen** en selecteer dimensie **API-naam**. Klik vervolgens op de **filter toevoegen** en kies de **API-naam** dimensie opnieuw. Selecteer de API's die u wilt tekenen op de grafiek in het filterdialoogvenster.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over Azure Monitor [werkmappen](../../azure-monitor/app/usage-workbooks.md)
* Meer informatie over [Metric Explorer](metrics-charts.md)