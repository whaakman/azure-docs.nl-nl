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
ms.date: 07/01/2019
ms.author: mbullwin
ms.openlocfilehash: e3ec202ba6126b150fb78c76591682f163018661
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604545"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Aantekeningen op grafieken met metrische gegevens in Application Insights

Aantekeningen op [Metrics Explorer](../../azure-monitor/app/metrics-explorer.md) grafieken weergeven waar u een nieuwe build of andere belangrijke gebeurtenissen hebt geïmplementeerd. Aantekeningen maken het gemakkelijk om te zien of uw wijzigingen geen effect op de prestaties van uw toepassing heeft. Ze automatisch kunnen worden gemaakt door de [Azure pijplijnen](https://docs.microsoft.com/azure/devops/pipelines/tasks/) bouwen-systeem. Ook kunt u aantekeningen voor het markeren van een gebeurtenis die u wilt met ze worden gemaakt vanuit PowerShell.

> [!NOTE]
> In dit artikel bevat de afgeschafte **klassieke metrische gegevens ervaring**. Aantekeningen zijn alleen beschikbaar in de klassieke ervaring en  **[werkmappen](../../azure-monitor/app/usage-workbooks.md)** . Zie voor meer informatie over de huidige ervaring voor metrische gegevens, [geavanceerde functies van Azure Metrics Explorer](../../azure-monitor/platform/metrics-charts.md).

![Voorbeeld van aantekeningen](./media/annotations/0-example.png)

## <a name="release-annotations-with-azure-pipelines-build"></a>Release-aantekeningen met Azure-pijplijnen bouwen

Release-aantekeningen zijn een functie van de Azure-pijplijnen cloud-gebaseerde service van Azure DevOps.

### <a name="install-the-annotations-extension-one-time"></a>Installeer de extensie aantekeningen (één keer)
Als u om release-aantekeningen te maken, moet u een van de vele beschikbare Azure DevOps-extensies installeren in de Visual Studio Marketplace.

1. Aanmelden bij uw [Azure DevOps](https://azure.microsoft.com/services/devops/) project.
   
1. Op de Visual Studio Marketplace [Versieaantekeningen extensie](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations) pagina en selecteer vervolgens uw organisatie Azure DevOps **installeren** de extensie toevoegen aan uw organisatie Azure DevOps.
   
   ![Selecteer een Azure DevOps-organisatie en selecteer vervolgens installeren.](./media/annotations/1-install.png)
   
U hoeft alleen voor het installeren van de extensie voor uw organisatie Azure DevOps. U kunt nu de release-aantekeningen voor een project configureren in uw organisatie.

### <a name="configure-release-annotations"></a>Configureer versieaantekeningen

Maak een afzonderlijke API-sleutel voor elk van uw Azure-pijplijnen release-sjablonen.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en open de Application Insights-resource die wordt uw toepassing bewaakt. Of als u nog geen hebt, [Maak een nieuwe Application Insights-resource](../../azure-monitor/app/app-insights-overview.md).
   
1. Open de **API-toegang** tabblad en kopieer de **Application Insights-ID**.
   
   ![Onder de API-toegang, kopieert u de toepassings-ID.](./media/annotations/2-app-id.png)

1. In een afzonderlijke browservenster openen of maken van de releasesjabloon die u uw pijplijnen voor Azure-implementaties beheert.
   
1. Selecteer **taak toevoegen**, en selecteer vervolgens de **Application Insights-Release-aantekening** taak in het menu.
   
   ![Selecteer de taak toevoegen en selecteer Application Insights-Release-aantekening.](./media/annotations/3-add-task.png)
   
1. Onder **toepassings-ID**, plakt u de Application Insights-ID die u hebt gekopieerd uit de **API-toegang** tabblad.
   
   ![Plak de Application Insights-ID](./media/annotations/4-paste-app-id.png)
   
1. Terug in de Application Insights **API-toegang** venster **API-sleutel maken**. 
   
   ![Selecteer de API-sleutel maken op het tabblad API-toegang.](./media/annotations/5-create-api-key.png)
   
1. In de **API-sleutel maken** venster, typ een beschrijving, selecteer **aantekeningen schrijven**, en selecteer vervolgens **sleutel genereren**. Kopieer de nieuwe sleutel.
   
   ![Typ een beschrijving in het venster van de API ' Create '-sleutel schrijven aantekeningen selecteren en selecteer vervolgens de sleutel genereren.](./media/annotations/6-create-api-key.png)
   
1. In het venster van de sjabloon versie op de **variabelen** tabblad **toevoegen** te maken van de definitie van een variabele voor de nieuwe API-sleutel.

1. Onder **naam**, voer `ApiKey`, en klikt u onder **waarde**, plak de API-sleutel die u hebt gekopieerd uit de **API-toegang** tabblad.
   
   ![Op het tabblad Azure DevOps variabelen toevoegen, de naam van de variabele ApiKey, selecteren en plak de API-sleutel onder waarde.](./media/annotations/7-paste-api-key.png)
   
1. Selecteer **opslaan** in het venster van de belangrijkste release-sjabloon op te slaan van de sjabloon.

## <a name="view-annotations"></a>Aantekeningen bekijken
Wanneer u de releasesjabloon gebruikt voor een nieuwe release implementeert, wordt nu een aantekening naar Application Insights verzonden. De aantekeningen worden weergegeven op de grafieken in **Metrics Explorer**.

Selecteer een aantekening-markering (licht grijze pijl) om te openen, details over de release, met inbegrip van de aanvrager, besturingselement bronvertakking, release-pijplijn en omgeving.

![Selecteer de markering van een release-aantekening.](./media/annotations/8-release.png)

## <a name="create-custom-annotations-from-powershell"></a>Aangepaste annotaties maken vanuit PowerShell
U kunt de [CreateReleaseAnnotation](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1) PowerShell-script vanuit GitHub om aantekeningen te maken van een proces dat u wilt, zonder dat met behulp van Azure DevOps. 

1. Maak een lokale kopie van [CreateReleaseAnnotation.ps1](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).
   
1. Gebruik de stappen in de vorige procedure om uw Application Insights-ID ophalen en maken van een API-sleutel van uw Application Insights **API-toegang** tabblad.
   
1. Het PowerShell-script met de volgende code, vervang de aanduidingen hoek tussen door de waarden in de aanroep. De `-releaseProperties` zijn optioneel. 
   
   ```powershell
   
        .\CreateReleaseAnnotation.ps1 `
         -applicationId "<applicationId>" `
         -apiKey "<apiKey>" `
         -releaseName "<releaseName>" `
         -releaseProperties @{
             "ReleaseDescription"="<a description>";
             "TriggerBy"="<Your name>" }
   ```

U kunt het script, bijvoorbeeld om aantekeningen te maken voor de laatste wijzigen.

## <a name="next-steps"></a>Volgende stappen

* [Werkitems maken](../../azure-monitor/app/diagnostic-search.md#create-work-item)
* [Automatisering met PowerShell](../../azure-monitor/app/powershell.md)
