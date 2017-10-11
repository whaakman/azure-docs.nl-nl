---
title: Aantekeningen voor Application Insights release | Microsoft Docs
description: Implementatie toevoegen of markeringen om uw metrics explorer-grafieken in Application Insights te bouwen.
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: bwren
ms.openlocfilehash: f7eb2f3cba535eb64db5544c498289c9e895987a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Aantekeningen voor metrische grafieken in Application Insights
Aantekeningen op [Metrics Explorer](app-insights-metrics-explorer.md) grafieken weergegeven waarin u een nieuwe build, of een andere belangrijke gebeurtenis geïmplementeerd. Ze maakt het eenvoudig om te zien of uw wijzigingen invloed zijn op de prestaties van de toepassing hadden. Ze kunnen automatisch gemaakt door de [Visual Studio Team Services bouwen system](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs). U kunt ook aantekeningen voor een gebeurtenis die u met wilt vlag maken [ze worden gemaakt vanuit PowerShell](#create-annotations-from-powershell).

![Voorbeeld van aantekeningen met zichtbaar correlatie met serverreactietijd](./media/app-insights-annotations/00.png)



## <a name="release-annotations-with-vsts-build"></a>Release aantekeningen met VSTS build

Release aantekeningen zijn een functie van de cloud-gebaseerde build en release van service van het Visual Studio Team Services. 

### <a name="install-the-annotations-extension-one-time"></a>De uitbreiding voor de aantekeningen (eenmaal) installeren
Om te kunnen maken van aantekeningen release, moet u een van de vele Team Service-extensies in de Visual Studio Marketplace te installeren.

1. Aanmelden bij uw [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) project.
2. In Visual Studio Marketplace, [de uitbreiding Release aantekeningen](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), en voeg deze toe aan uw Team Services-account.

![AT top rechts van de webpagina Team Services, open Marketplace. Selecteer Visual Team Services en onder Build en Release, kies meer.](./media/app-insights-annotations/10.png)

U hoeft hiervoor eenmaal voor uw Visual Studio Team Services-account. Release aantekeningen kunnen nu worden geconfigureerd voor elk project in uw account. 

### <a name="configure-release-annotations"></a>Release aantekeningen configureren

U moet een afzonderlijke API-sleutel voor elke release VSTS sjabloon.

1. Aanmelden bij de [Microsoft Azure Portal](https://portal.azure.com) en open de Application Insights-resource die uw toepassing wordt bewaakt. (Of [er nu een maken](app-insights-overview.md), als u dit nog niet hebt gedaan.)
2. Open **API-toegang**, **Application Insights-Id**.
   
    ![Open uw Application Insights-resource in portal.azure.com en instellingen kiezen. Open API-toegang. Kopieer de toepassings-ID](./media/app-insights-annotations/20.png)

4. Open (of maak) op de releasesjabloon die u uw implementaties van Visual Studio Team Services beheert in een apart browservenster. 
   
    Een taak toevoegen en selecteer de Application Insights Release aantekening-taak in het menu.
   
    Plak de **toepassings-Id** die u hebt gekopieerd uit de blade API-toegang.
   
    ![Release openen in Visual Studio Team Services, selecteert u de definitie van een release en kies bewerken. Klik op taak toevoegen en selecteer Application Insights Release aantekening. Plak de Application Insights-id.](./media/app-insights-annotations/30.png)
4. Stel de **APIKey** veld aan een variabele `$(ApiKey)`.

5. Maak een nieuwe API-sleutel en een kopie van deze weer in het venster Azure.
   
    ![Klik in de blade API-toegang in het venster Azure API-sleutel maken. Geef een opmerking, schrijven aantekeningen, en klik op sleutel genereren. Kopieer de nieuwe sleutel.](./media/app-insights-annotations/40.png)

6. Open het tabblad configuratie van de releasesjabloon.
   
    Maken van een variabele definitie voor `ApiKey`.
   
    Uw API-sleutel voor de definitie van de ApiKey plakken.
   
    ![In het venster Team Services, selecteer het tabblad configuratie en klik variabele toevoegen. De naam ingesteld op ApiKey en naar de waarde, plak de sleutel die u zojuist hebt gegenereerd en klik op het vergrendelingspictogram.](./media/app-insights-annotations/50.png)
7. Ten slotte **opslaan** de release-definitie.


## <a name="view-annotations"></a>Weergave aantekeningen
Wanneer u de sjabloon voor het implementeren van een nieuwe release hebt gebruikt, wordt nu een aantekening verzonden naar Application Insights. De aantekeningen worden weergegeven op de grafieken in Metrics Explorer.

Klik op de markering van een aantekening details over de release, met inbegrip van de aanvrager, bron besturingselement vertakking openen, vrijgeven definitie en omgeving.

![Klik op een markering van de aantekening release.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a>Aangepaste aantekeningen maken vanuit PowerShell
U kunt ook aantekeningen maken van een proces gewenste (zonder tegenover Team System). 


1. Maak een lokale kopie van de [Powershell-script uit GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. De toepassings-ID ophalen en maak een API-sleutel vanuit de blade API-toegang.

3. Roept het script als volgt:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

Het is gemakkelijk om het script, bijvoorbeeld om aantekeningen te maken voor het verleden.

## <a name="next-steps"></a>Volgende stappen

* [Werkitems maken](app-insights-diagnostic-search.md#create-work-item)
* [Automatisering met PowerShell](app-insights-powershell.md)
