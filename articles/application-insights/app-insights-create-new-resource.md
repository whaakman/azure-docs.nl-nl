---
title: Maak een nieuwe Azure Application Insights-resource | Microsoft Docs
description: Stel handmatig bewaking van de Application Insights voor een nieuwe live-toepassing.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: mbullwin
ms.openlocfilehash: 9023f3d9ae3ddd4d75b5853a08177cba7718cec1
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="create-an-application-insights-resource"></a>Een Application Insights-resource maken
Azure Application Insights gegevens over uw toepassing worden weergegeven in een Microsoft Azure *resource*. Een nieuwe resource daarom deel uitmaakt van [Application Insights instellen voor het bewaken van een nieuwe toepassing][start]. In veel gevallen kan maken van een resource automatisch worden gedaan door de IDE. Maar in sommige gevallen u Maak handmatig een resource - bijvoorbeeld, als u wilt dat afzonderlijke resources voor ontwikkeling en productie builds van uw toepassing.

Nadat u de resource hebt gemaakt, kunt u de instrumentatiesleutel ophalen en gebruiken die voor het configureren van de SDK in de toepassing. De bronsleutel koppelingen de telemetrie naar de resource.

## <a name="sign-up-to-microsoft-azure"></a>Meld u aan Microsoft Azure
Als u dit nog niet hebt verkregen een [Microsoft-account, nu](http://live.com). (Als u gebruikmaakt van services zoals Outlook.com, OneDrive, Windows Phone of XBox Live, u hebt al een Microsoft-account.)

U moet ook een abonnement op [Microsoft Azure](http://azure.com). Als uw team of organisatie een Azure-abonnement heeft, de eigenaar kunt u toevoegen, met behulp van uw Windows Live ID. U bent alleen kosten in rekening gebracht voor wat u gebruikt. Het basic standaardplan kunt u een bepaalde hoeveelheid experimenteel gebruik gratis.

Wanneer u toegang tot een abonnement hebt, meld u aan bij Application Insights op [http://portal.azure.com](https://portal.azure.com), en gebruik van uw Live-ID om aan te melden.

## <a name="create-an-application-insights-resource"></a>Een Application Insights-resource maken
In de [portal.azure.com](https://portal.azure.com), een Application Insights-resource toevoegen:

![Klik op Nieuw > Application Insights](./media/app-insights-create-new-resource/01-new.png)

* **Toepassingstype** is van invloed op wat er op de overzichtsblade en de eigenschappen die beschikbaar zijn in [metrische explorer][metrics]. Als u uw app-type niet ziet, kiest u algemene.
* **Abonnement** is uw betaling-account in Azure.
* **Resourcegroep** is nuttig voor het beheren van eigenschappen zoals toegangsbeheer. Als u andere Azure-resources al hebt gemaakt, kunt u deze nieuwe bron plaatsen in dezelfde groep.
* **Locatie** is waar we uw gegevens wilt bewaren.
* **Vastmaken aan dashboard** plaatst u een snelle toegang tegel voor uw resource op de startpagina van Azure. Aanbevolen.

Wanneer uw app is gemaakt, wordt er een nieuwe blade geopend. Deze blade is waar u prestatie- en gebruiksgegevens over uw app bekijken. 

U kunt teruggaan naar het volgende keer dat u zich bij Azure aanmelden, zoekt u naar uw app snel starten-tegel in de start board (startscherm). Of klik op Bladeren om te zoeken.

## <a name="copy-the-instrumentation-key"></a>De instrumentatiesleutel kopiëren
De instrumentatiesleutel identificeert de resource die u hebt gemaakt. U moet deze aan de SDK.

![Klik op Essentials, klik op de Instrumentatiesleutel, CTRL + C](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-the-sdk-in-your-app"></a>De SDK installeren in uw app
De Application Insights-SDK installeren in uw app. Deze stap is sterk afhankelijk van het type van uw toepassing. 

De instrumentatiesleutel gebruiken voor het configureren van [de SDK die u in uw toepassing installeert][start].

De SDK bevat standaard modules weer die het verzenden van telemetrie zonder dat u code hoeft te schrijven. Voor het bijhouden van acties van de gebruiker of het analyseren van problemen in meer detail [gebruikmaken van de API] [ api] uw eigen telemetrie verzenden.

## <a name="monitor"></a>Zie telemetriegegevens
Sluit de blade snel starten om terug te keren naar de blade van uw toepassing in de Azure portal.

Klik op de tegel Search om te zien [diagnostische gegevens doorzoeken][diagnostic], waarbij de eerste gebeurtenissen worden weergegeven. 

Als u meer gegevens verwacht, klikt u op **vernieuwen** na enkele seconden.

## <a name="creating-a-resource-automatically"></a>Het automatisch maken van een resource
U kunt schrijven een [PowerShell-script](app-insights-powershell.md) automatisch maken van een resource.

## <a name="next-steps"></a>Volgende stappen
* [Een dashboard maken](app-insights-dashboards.md)
* [Diagnostische gegevens doorzoeken](app-insights-diagnostic-search.md)
* [Metrische gegevens verkennen](app-insights-metrics-explorer.md)
* [Analytics-query's schrijven](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md

