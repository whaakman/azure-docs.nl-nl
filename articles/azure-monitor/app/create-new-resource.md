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
ms.date: 06/10/2019
ms.author: mbullwin
ms.openlocfilehash: 9da52e5a9dfa3b55431d66ed3162172226f71a40
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073318"
---
# <a name="create-an-application-insights-resource"></a>Een Application Insights-resource maken

Azure Application Insights geeft gegevens over uw toepassing in een Microsoft Azure *resource*. Het maken van een nieuwe resource maakt daarom deel uit van [Application Insights instellen voor het bewaken van een nieuwe toepassing][start]. Nadat u uw nieuwe resourcegroep hebt gemaakt, kunt u de instrumentatiesleutel ophalen en gebruiken die voor het configureren van de Application Insights-SDK. De instrumentatiesleutel wordt uw telemetrie gekoppeld aan de resource.

## <a name="sign-in-to-microsoft-azure"></a>Aanmelden bij Microsoft Azure

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="create-an-application-insights-resource"></a>Een Application Insights-resource maken

Aanmelden bij de [Azure-portal](https://portal.azure.com), en maak een Application Insights-resource:

![Klik op het teken '+' in de linkerbovenhoek. Ontwikkelhulpprogramma's, gevolgd door Application Insights selecteren](./media/create-new-resource/new-app-insights.png)

   | Instellingen        |  Waarde           | Beschrijving  |
   | ------------- |:-------------|:-----|
   | **Naam**      | Globaal unieke waarde | Naam ter identificatie van de app die u bewaakt. |
   | **Resourcegroep**     | myResourceGroup      | Naam voor de nieuwe of bestaande resourcegroep waarin App Insights-gegevens voor host. |
   | **Location** | US - oost | Kies een locatie bij u in de buurt of in de buurt van waar uw app wordt gehost. |

Voer de juiste waarden in de vereiste velden en selecteer vervolgens **revisie + maken**.

![Voer waarden in de vereiste velden en selecteer vervolgens 'beoordelen en maken'.](./media/create-new-resource/review-create.png)

Wanneer uw app is gemaakt, wordt er een nieuw deelvenster geopend. Dit deelvenster is ziet u prestatie- en gebruiksgegevens over uw bewaakte toepassing. 

## <a name="copy-the-instrumentation-key"></a>De instrumentatiesleutel kopiëren

De instrumentatiesleutel identificeert de bron die u wilt de telemetriegegevens van uw met koppelen. U moet kopiëren naar de instrumentatiesleutel toevoegen aan de code van uw toepassing.

![Klik op en de instrumentatiesleutel kopiëren](./media/create-new-resource/instrumentation-key.png)

## <a name="install-the-sdk-in-your-app"></a>De SDK installeren in uw app

Installeer de Application Insights-SDK in uw app. Deze stap, hangt sterk af van het type van uw toepassing.

Gebruik de instrumentatiesleutel configureren [de SDK die u in uw toepassing installeert][start].

De SDK bevat standaard modules die telemetrievolume verzenden zonder dat u hoeft extra code te schrijven. Acties van de gebruiker bijhouden of problemen in meer detail [gebruikmaken van de API] [ api] om uw eigen telemetrie te verzenden.

## <a name="creating-a-resource-automatically"></a>Het automatisch maken van een resource
U kunt schrijven een [PowerShell-script](../../azure-monitor/app/powershell.md) voor het automatisch maken van een resource.

## <a name="next-steps"></a>Volgende stappen
* [Diagnostische gegevens doorzoeken](../../azure-monitor/app/diagnostic-search.md)
* [Metrische gegevens verkennen](../../azure-monitor/app/metrics-explorer.md)
* [Analytics-query's schrijven](../../azure-monitor/app/analytics.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[start]: ../../azure-monitor/app/app-insights-overview.md