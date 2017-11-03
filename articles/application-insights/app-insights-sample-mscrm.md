---
title: Microsoft Dynamics CRM en Azure Application Insights | Microsoft Docs
description: Telemetrie ophalen uit Microsoft Dynamics CRM Online met behulp van Application Insights. Overzicht van setup ophalen van gegevens, visualisatie en exporteren.
services: application-insights
documentationcenter: 
author: mazharmicrosoft
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: mbullwin
ms.openlocfilehash: 759dac681592d0e5951e09638533b93c6348d899
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Overzicht: Telemetrie voor Microsoft Dynamics CRM Online met behulp van Application Insights inschakelen
Dit artikel ziet u het ophalen van telemetriegegevens van [Microsoft Dynamics CRM Online](https://www.dynamics.com/) met [Azure Application Insights](https://azure.microsoft.com/services/application-insights/). We doorlopen het complete proces van het Application Insights-script toevoegen aan uw toepassing vastleggen van gegevens en gegevensvisualisatie.

> [!NOTE]
> [De Voorbeeldoplossing Bladeren](https://dynamicsandappinsights.codeplex.com/).
> 
> 

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Application Insights toevoegen aan nieuwe of bestaande exemplaar van CRM Online
Voor het bewaken van uw toepassing, kunt u een Application Insights-SDK toevoegen aan uw toepassing. De SDK telemetrie verzendt naar de [Application Insights-portal](https://portal.azure.com), kunt u gebruik van onze krachtige analyse en diagnostische hulpprogramma's, of de gegevens naar opslag exporteren.

### <a name="create-an-application-insights-resource-in-azure"></a>Een Application Insights-resource maken in Azure
1. Ophalen van [een account in Microsoft Azure](http://azure.com/pricing). 
2. Meld u aan bij de [Azure-portal](https://portal.azure.com) en een nieuwe Application Insights-resource toevoegen. Dit is waar uw gegevens worden verwerkt en weergegeven.
   
    ![Klik op +, Ontwikkelaarsservices, Application Insights.](./media/app-insights-sample-mscrm/01.png)
   
    Kies ASP.NET als het toepassingstype.
3. Open de pagina aan de slag en ' bewaken en onderzoeken van client-side '.
   
    ![Codefragment voor het invoegen in uw webpagina](./media/app-insights-sample-mscrm/03.png)

**Houd de codetabel open** terwijl u de volgende stap in een ander browservenster. U moet de code snel. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Een JavaScript-web-resource maken in Microsoft Dynamics CRM
1. Open uw CRM Online-instantie en aanmelding met administrator-bevoegdheden.
2. Open Microsoft Dynamics CRM-instellingen aanpassingen, past u het systeem
   
    ![Microsoft Dynamics CRM-instellingen](./media/app-insights-sample-mscrm/04.png)
   
    ![Instellingen > aanpassingen](./media/app-insights-sample-mscrm/05.png)

    ![De Systeemoptie aanpassen](./media/app-insights-sample-mscrm/06.png)

1. Maak een JavaScript-resource.
   
    ![Dialoogvenster nieuwe webbron](./media/app-insights-sample-mscrm/07.png)
   
    Geef het een naam, selecteer **Script (JScript)** en open de teksteditor.
   
    ![Open de teksteditor](./media/app-insights-sample-mscrm/08.png)
2. Kopieer de code uit de Application Insights. Zorg dat u scriptlabels negeren bij het kopiëren. Raadpleeg de onderstaande schermafbeelding:
   
    ![De instrumentatiesleutel instellen](./media/app-insights-sample-mscrm/09.png)
   
    De code bevat de instrumentatiesleutel die uw Application insights-resource identificeert.
3. Opslaan en publiceren.
   
    ![Opslaan en publiceren](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Instrument formulieren
1. Open het formulier Account in Microsoft CRM Online
   
    ![Accountformulier](./media/app-insights-sample-mscrm/11.png)
2. Open de eigenschappen van het formulier
   
    ![Eigenschappen van formulier](./media/app-insights-sample-mscrm/12.png)
3. De JavaScript-web-resource die u hebt gemaakt toevoegen
   
    ![Menu toevoegen](./media/app-insights-sample-mscrm/13.png)
   
    ![De webresource toevoegen](./media/app-insights-sample-mscrm/14.png)
4. Opslaan en publiceren van uw aanpassingen van formulieren.

## <a name="metrics-captured"></a>Metrische gegevens vastgelegd
U hebt nu het vastleggen van telemetriegegevens voor het formulier ingesteld. Wanneer deze wordt gebruikt, worden gegevens verzonden naar uw Application Insights-resource.

Hier vindt u voorbeelden van de gegevens die u ziet.

#### <a name="application-health"></a>Toepassingsstatus
![Voorbeeld van de paginalaadtijd](./media/app-insights-sample-mscrm/15.png)

![Voorbeeld pagina weergaven grafiek](./media/app-insights-sample-mscrm/16.png)

Browseruitzonderingen:

![Grafiek met serveruitzonderingen klikken](./media/app-insights-sample-mscrm/17.png)

Klik in de grafiek als u meer informatie:

![Lijst met uitzonderingen](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Gebruik
![Gebruikers, sessies en paginaweergaven](./media/app-insights-sample-mscrm/19.png)

![Sesion grafieken](./media/app-insights-sample-mscrm/20.png)

![Browserversies](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Browsers
![Overzicht van de laadtijd van pagina](./media/app-insights-sample-mscrm/22.png)

![Het aantal sessies per browserversie](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Geolocatie
![Het aantal sessies per land](./media/app-insights-sample-mscrm/24.png)

![Sessies en gebruikers per land](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Inside pagina-aanvraag voor weergave
![Overzicht van de pagina weergeven](./media/app-insights-sample-mscrm/26.png)

![Zoeken op de pagina-gebeurtenissen weergeven](./media/app-insights-sample-mscrm/27.png)

![Vergelijkbare paginaweergaven](./media/app-insights-sample-mscrm/28.png)

![Eigenschappen van paginaweergaven](./media/app-insights-sample-mscrm/29.png)

![Pagina's per sessie](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Voorbeeldcode
[De voorbeeldcode Bladeren](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI
U kunt zelfs grondigere analyse doen als u [de gegevens exporteren naar Microsoft Power BI](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Voorbeeld Microsoft Dynamics CRM-oplossing
[Hier volgt de Voorbeeldoplossing geïmplementeerd in Microsoft Dynamics CRM](https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Meer informatie
* [Wat is Application Insights?](app-insights-overview.md)
* [Application Insights voor webpagina 's](app-insights-javascript.md)
* [Meer voorbeelden en scenario 's](app-insights-code-samples.md)
