---
title: Release-aantekeningen voor Application Insights | Microsoft Docs
description: Implementatie toevoegen of maken van markeringen naar uw explorer grafieken met metrische gegevens in Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 11/16/2016
ms.author: mbullwin
ms.openlocfilehash: f943f0e371b3092717a62a2e83a98211723e5302
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44304404"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Aantekeningen op grafieken met metrische gegevens in Application Insights
Aantekeningen op [Metrics Explorer](app-insights-metrics-explorer.md) grafieken weergeven waar u een nieuwe build of andere belangrijke gebeurtenis hebt geïmplementeerd. Ze maken het gemakkelijk om te zien of uw wijzigingen geen effect op de prestaties van uw toepassing heeft. Ze automatisch kunnen worden gemaakt door de [systeem bouwen voor Azure DevOps Services](https://docs.microsoft.com/azure/devops/pipelines/tasks/). U kunt ook maken met aantekeningen voor het markeren van een gebeurtenis die u met wilt [ze worden gemaakt vanuit PowerShell](#create-annotations-from-powershell).

![Voorbeeld van aantekeningen met zichtbaar correlatie met serverreactietijd](./media/app-insights-annotations/00.png)



## <a name="release-annotations-with-azure-devops-services-build"></a>Release-aantekeningen met Azure DevOps-Services-build

Release-aantekeningen zijn een functie van de Azure-pijplijnen cloud-gebaseerde service van Azure DevOps-Services. 

### <a name="install-the-annotations-extension-one-time"></a>Installeer de extensie aantekeningen (één keer)
Als u om release-aantekeningen te maken, moet u een van de beschikbare veel Services van Azure DevOps-extensies installeren in de Visual Studio Marketplace.

1. Aanmelden bij uw [Azure DevOps Services](https://visualstudio.microsoft.com/vso/) project.
2. In Visual Studio Marketplace, [ophalen van de extensie Versieaantekeningen](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), en voeg deze toe aan uw organisatie Azure DevOps-Services.

![AT rechtsboven in de webpagina van Azure DevOps-Services, open Marketplace. Selecteer Azure DevOps-Services en onder Azure pijplijnen, en kies vervolgens meer.](./media/app-insights-annotations/10.png)

U hoeft dit maar eenmalig te doen voor uw organisatie Azure DevOps-Services. Release-aantekeningen kunnen nu worden geconfigureerd voor elk project in uw organisatie. 

### <a name="configure-release-annotations"></a>Configureer versieaantekeningen

U moet een afzonderlijke API-sleutel voor elke release-sjabloon van Azure DevOps-Services ophalen.

1. Aanmelden bij de [Microsoft Azure Portal](https://portal.azure.com) en open de Application Insights-resource die wordt uw toepassing bewaakt. (Of [Maak nu een](app-insights-overview.md), als u dat nog niet hebt gedaan.)
2. Open **API-toegang**, **Application Insights-Id**.
   
    ![Open uw Application Insights-resource in portal.azure.com en kies instellingen. Open API-toegang. Kopieer de toepassings-ID](./media/app-insights-annotations/20.png)

4. Open (of maak) in een nieuw browservenster op de releasesjabloon die u uw implementaties van Azure DevOps-Services beheert. 
   
    Een taak toevoegen en selecteer de Application Insights-Release-aantekening-taak in het menu.
   
    Plak de **toepassings-Id** die u hebt gekopieerd uit de blade API-toegang.
   
    ![In Azure DevOps-Services, versie openen, selecteert u een release-pijplijn en kiest u bewerken. Klik op taak toevoegen en selecteer Application Insights-Release-aantekening. Plak de Application Insights-id.](./media/app-insights-annotations/30.png)
4. Stel de **APIKey** veld aan een variabele `$(ApiKey)`.

5. Maak een nieuwe API-sleutel en een kopie van deze terug in het venster Azure.
   
    ![Klik in de blade API-toegang in het venster van de Azure API-sleutel maken. Een opmerking, controleert u aantekeningen schrijven, en klik op sleutel genereren. Kopieer de nieuwe sleutel.](./media/app-insights-annotations/40.png)

6. Open het tabblad configuratie van de releasesjabloon.
   
    Maken van de definitie van een variabele voor `ApiKey`.
   
    Plak uw API-sleutel voor de definitie van de ApiKey-variabele.
   
    ![Selecteer het tabblad configuratie en klikt u op een variabele toevoegen in het venster Azure DevOps-Services. Stel de naam ApiKey en in de waarde, plak de sleutel die u zojuist hebt gemaakt en klik op het vergrendelingspictogram.](./media/app-insights-annotations/50.png)
7. Ten slotte **opslaan** de release-pijplijn.


## <a name="view-annotations"></a>Aantekeningen bekijken
Wanneer u de releasesjabloon gebruikt voor het implementeren van een nieuwe versie, worden er nu een aantekening worden verzonden naar Application Insights. De aantekeningen worden weergegeven op de grafieken in Metrics Explorer.

Klik op een markering aantekening details over de release, met inbegrip van de aanvrager, besturingselement bronvertakking openen, release-pijplijn, milieu en meer.

![Klik op een markering release-aantekening.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a>Aangepaste annotaties maken vanuit PowerShell
U kunt ook aantekeningen maken van een proces dat u wilt (zonder dat het met Azure DevOps-Services). 


1. Maak een lokale kopie van de [Powershell-script vanuit GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. De toepassings-ID ophalen en maken van een API-sleutel op de blade API-toegang.

3. Aanroepen van het script als volgt:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

Het is eenvoudig om het script, bijvoorbeeld om aantekeningen te maken voor het verleden.

## <a name="next-steps"></a>Volgende stappen

* [Werkitems maken](app-insights-diagnostic-search.md#create-work-item)
* [Automatisering met PowerShell](app-insights-powershell.md)