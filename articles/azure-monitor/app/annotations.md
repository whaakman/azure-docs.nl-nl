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
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mbullwin
ms.openlocfilehash: 6567d7f2ebaab5bd7b5bc8fb7b5a62970f169161
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66476165"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Aantekeningen op grafieken met metrische gegevens in Application Insights

Aantekeningen op [Metrics Explorer](../../azure-monitor/app/metrics-explorer.md) grafieken weergeven waar u een nieuwe build of andere belangrijke gebeurtenis hebt geïmplementeerd. Ze maken het gemakkelijk om te zien of uw wijzigingen geen effect op de prestaties van uw toepassing heeft. Ze automatisch kunnen worden gemaakt door de [systeem bouwen voor Azure DevOps Services](https://docs.microsoft.com/azure/devops/pipelines/tasks/). Ook kunt u aantekeningen voor het markeren van een gebeurtenis die u wilt met ze worden gemaakt vanuit PowerShell.

> [!NOTE]
> In dit artikel bevat de afgeschafte **klassieke metrische gegevens ervaring**. Aantekeningen zijn alleen beschikbaar in de klassieke ervaring en  **[werkmappen](../../azure-monitor/app/usage-workbooks.md)** . Voor meer informatie over de huidige metrics-ervaring, kunt u raadplegen [in dit artikel](../../azure-monitor/platform/metrics-charts.md).

![Voorbeeld van aantekeningen](./media/annotations/0-example.png)

## <a name="release-annotations-with-azure-devops-services-build"></a>Release-aantekeningen met Azure DevOps-Services-build

Release-aantekeningen zijn een functie van de Azure-pijplijnen cloud-gebaseerde service van Azure DevOps-Services.

### <a name="install-the-annotations-extension-one-time"></a>Installeer de extensie aantekeningen (één keer)
Als u om release-aantekeningen te maken, moet u een van de beschikbare veel Services van Azure DevOps-extensies installeren in de Visual Studio Marketplace.

1. Aanmelden bij uw [Azure DevOps Services](https://azure.microsoft.com/services/devops/) project.
2. In Visual Studio Marketplace, [ophalen van de extensie Versieaantekeningen](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), en voeg deze toe aan uw organisatie Azure DevOps-Services.

![Selecteer een organisatie die Azure DevOps en vervolgens installeren.](./media/annotations/1-install.png)

U hoeft dit maar eenmalig te doen voor uw organisatie Azure DevOps-Services. Release-aantekeningen kunnen nu worden geconfigureerd voor elk project in uw organisatie.

### <a name="configure-release-annotations"></a>Configureer versieaantekeningen

U moet een afzonderlijke API-sleutel voor elke release-sjabloon van Azure DevOps-Services ophalen.

1. Aanmelden bij de [Microsoft Azure portal](https://portal.azure.com) en open de Application Insights-resource die wordt uw toepassing bewaakt. (Of [Maak nu een](../../azure-monitor/app/app-insights-overview.md), als u dat nog niet hebt gedaan.)
2. Open de **API-toegang** tabblad en kopieer de **Application Insights-ID**.
   
    ![Open uw Application Insights-resource in portal.azure.com en kies instellingen. Open API-toegang. Kopieer de toepassings-ID](./media/annotations/2-app-id.png)

4. Open (of maak) in een nieuw browservenster op de releasesjabloon die u uw implementaties van Azure DevOps-Services beheert.
   
    Een taak toevoegen en selecteer de Application Insights-Release-aantekening-taak in het menu.

   ![Klik op het plusteken om een taak toevoegen en selecteer Application Insights-Release-aantekening. Plak de Application Insights-ID.](./media/annotations/3-add-task.png)

    Plak de **toepassings-ID** die u hebt gekopieerd uit het tabblad API-toegang.
   
    ![Plak de Application Insights-ID](./media/annotations/4-paste-app-id.png)

5. Maak een nieuwe API-sleutel en een kopie van deze terug in het venster Azure.
   
    ![Klik op het tabblad API-toegang in het venster van de Azure API-sleutel maken.](./media/annotations/5-create-api-key.png)

    ![In het tabblad van maken API-sleutel een opmerking, controleert u aantekeningen schrijven, en klik op sleutel genereren. Kopieer de nieuwe sleutel.](./media/annotations/6-create-api-key.png)

6. Open het tabblad configuratie van de releasesjabloon.
   
    Maken van de definitie van een variabele voor `ApiKey`.
   
    Plak uw API-sleutel voor de definitie van de ApiKey-variabele.
   
    ![Selecteer het tabblad variabele in het venster Azure DevOps-Services en klik op toevoegen. Klik op het vergrendelingspictogram stelt u de naam ApiKey en in de waarde en plak de sleutel die u hebt gegenereerd.](./media/annotations/7-paste-api-key.png)
1. Ten slotte **opslaan** de release-pijplijn.


## <a name="view-annotations"></a>Aantekeningen bekijken
Wanneer u de releasesjabloon gebruikt voor het implementeren van een nieuwe versie, worden er nu een aantekening worden verzonden naar Application Insights. De aantekeningen worden weergegeven op de grafieken in Metrics Explorer.

Klik op een aantekening-markering (licht grijze pijl) te openen van gegevens over de versie, met inbegrip van de aanvrager, besturingselement bronvertakking, release-pijplijn, milieu en meer.

![Klik op een markering release-aantekening.](./media/annotations/8-release.png)

## <a name="create-custom-annotations-from-powershell"></a>Aangepaste annotaties maken vanuit PowerShell
U kunt ook aantekeningen maken van een proces dat u wilt (zonder dat het met Azure DevOps-Services). 


1. Maak een lokale kopie van de [Powershell-script vanuit GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. De toepassings-ID ophalen en maken van een API-sleutel op het tabblad API-toegang.

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

* [Werkitems maken](../../azure-monitor/app/diagnostic-search.md#create-work-item)
* [Automatisering met PowerShell](../../azure-monitor/app/powershell.md)
