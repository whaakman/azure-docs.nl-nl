---
title: Maak een nieuwe Azure Application Insights-resource | Microsoft Docs
description: Handmatig instellen van Application Insights-bewaking voor een nieuwe live-toepassing.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: mbullwin
ms.openlocfilehash: 3e642e5954a8c1154ca5b07f06f9450e4e9ee39a
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/28/2018
ms.locfileid: "53807921"
---
# <a name="create-an-application-insights-resource"></a>Een Application Insights-resource maken
Azure Application Insights geeft gegevens over uw toepassing in een Microsoft Azure *resource*. Het maken van een nieuwe resource maakt daarom deel uit van [Application Insights instellen voor het bewaken van een nieuwe toepassing][start]. In veel gevallen, kan het maken van een resource automatisch worden uitgevoerd door de IDE. Maar in sommige gevallen, maakt u een resource handmatig - bijvoorbeeld als afzonderlijke resources voor de ontwikkeling en productie builds van uw toepassing.

Nadat u de resource hebt gemaakt, kunt u de instrumentatiesleutel ophalen en gebruiken die voor het configureren van de SDK in de toepassing. De resource-sleutel wordt de telemetrie gekoppeld aan de resource.

## <a name="sign-up-to-microsoft-azure"></a>Aanmelden voor Microsoft Azure
Als u nog niet hebt u een [Microsoft account, nu](https://live.com). (Als u gebruikmaakt van services zoals Outlook.com, OneDrive, Windows Phone of XBox Live, u hebt al een Microsoft-account.)

U moet ook een abonnement op [Microsoft Azure](https://azure.com). Als uw team of organisatie een Azure-abonnement, de eigenaar kunt u toevoegen, met behulp van uw Windows Live ID. U betaalt alleen voor wat u gebruikt. Het standaard basic-abonnement kunt u een bepaalde hoeveelheid experimentele gratis gebruiken.

Wanneer u toegang tot een abonnement hebt, meld u aan bij Application Insights op [ https://portal.azure.com ](https://portal.azure.com), en gebruik van uw Live-ID om aan te melden.

## <a name="create-an-application-insights-resource"></a>Een Application Insights-resource maken
In de [portal.azure.com](https://portal.azure.com), een Application Insights-resource toevoegen:

![Klik op Nieuw > Application Insights](./media/app-insights-create-new-resource/01-new.png)

* **Toepassingstype** is van invloed op wat u ziet op de overzichtsblade en de eigenschappen die beschikbaar zijn in [metric explorer][metrics]. Als u het type van de app niet ziet, kiest u algemene.
* **Abonnement** is uw account betaling in Azure.
* **Resourcegroep** is nuttig voor het beheren van eigenschappen zoals toegangsbeheer. Als u andere Azure-resources al hebt gemaakt, kunt u deze nieuwe resource in dezelfde groep geplaatst.
* **Locatie** is waar we uw gegevens bewaren.
* **Vastmaken aan dashboard** hebt u een snelle toegang tegel voor uw resource op de startpagina van Azure. Aanbevolen.

Wanneer uw app is gemaakt, wordt er een nieuwe blade geopend. Deze blade is ziet u prestatie- en gebruiksgegevens over uw app. 

Als u wilt teruggaan naar het volgende keer dat u zich aanmeldt bij Azure, zoek naar Snel starten-tegel van uw app op het prikbord van start (startscherm). Of klik op Bladeren om te zoeken.

## <a name="copy-the-instrumentation-key"></a>De instrumentatiesleutel kopiëren
De instrumentatiesleutel identificeert de bron die u hebt gemaakt. U hebt deze om te geven tot de SDK nodig.

![Klik op Essentials, klik op de Instrumentatiesleutel, CTRL + C](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-the-sdk-in-your-app"></a>De SDK installeren in uw app
Installeer de Application Insights-SDK in uw app. Deze stap, hangt sterk af van het type van uw toepassing. 

Gebruik de instrumentatiesleutel configureren [de SDK die u in uw toepassing installeert][start].

De SDK bevat standaard modules die telemetrievolume verzenden zonder dat u hoeft code te schrijven. Acties van de gebruiker bijhouden of problemen in meer detail [gebruikmaken van de API] [ api] om uw eigen telemetrie te verzenden.

## <a name="monitor"></a>Zie telemetrische gegevens
Sluit de blade snel starten om terug te keren naar uw toepassingsblade in Azure portal.

Klik op de tegel Search om te zien [diagnostische gegevens doorzoeken][diagnostic], waarbij de eerste gebeurtenissen worden weergegeven. 

Als u meer gegevens verwacht, klikt u op **vernieuwen** na een paar seconden.

## <a name="creating-a-resource-automatically"></a>Het automatisch maken van een resource
U kunt schrijven een [PowerShell-script](app-insights-powershell.md) voor het automatisch maken van een resource.

## <a name="next-steps"></a>Volgende stappen
* [Een dashboard maken](../azure-monitor/app/app-insights-dashboards.md)
* [Diagnostische gegevens doorzoeken](../azure-monitor/app/diagnostic-search.md)
* [Metrische gegevens verkennen](app-insights-metrics-explorer.md)
* [Analytics-query's schrijven](../azure-monitor/app/analytics.md)

<!--Link references-->

[api]: ../azure-monitor/app/api-custom-events-metrics.md
[diagnostic]: ../azure-monitor/app/diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md

