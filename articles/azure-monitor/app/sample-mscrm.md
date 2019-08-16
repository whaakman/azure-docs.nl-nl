---
title: Micro soft Dynamics CRM en Azure-toepassing Insights | Microsoft Docs
description: Ontvang telemetrie van micro soft Dynamics CRM Online met behulp van Application Insights. Overzicht van Setup, gegevens ophalen, visualisatie en exporteren.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/16/2018
ms.reviewer: mazhar
ms.author: mbullwin
ms.openlocfilehash: 470f723782ca29409549e0df8e900edf86cd446e
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69534296"
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Walkthrough: Telemetrie inschakelen voor micro soft Dynamics CRM Online met behulp van Application Insights
Dit artikel laat u zien hoe u telemetriegegevens van [micro soft Dynamics CRM Online](https://www.dynamics.com/) kunt ophalen met behulp van [Azure-toepassing Insights](https://azure.microsoft.com/services/application-insights/). Het volledige proces voor het toevoegen van Application Insights script aan uw toepassing, het vastleggen van gegevens en gegevens visualisatie wordt door lopen.

> [!NOTE]
> [Blader door de voorbeeld oplossing](https://dynamicsandappinsights.codeplex.com/).
> 
> 

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Application Insights toevoegen aan nieuwe of bestaande CRM Online-instantie
Als u uw toepassing wilt bewaken, voegt u een Application Insights SDK toe aan uw toepassing. De SDK verzendt telemetrie naar de [Application Insights-Portal](https://portal.azure.com), waar u onze krachtige analyse-en diagnostische hulpprogram ma's kunt gebruiken of de gegevens exporteert naar opslag.

### <a name="create-an-application-insights-resource-in-azure"></a>Een Application Insights resource maken in azure
1. [Een account in Microsoft Azure](https://azure.com/pricing)ophalen. 
2. Meld u aan bij de [Azure Portal](https://portal.azure.com) en voeg een nieuwe Application Insights resource toe. Hier worden uw gegevens verwerkt en weer gegeven.

    ![Klik op +, ontwikkelaars Services Application Insights.](./media/sample-mscrm/01.png)

    Kies ASP.NET als het toepassingstype.
3. Volg de instructies voor [het downloaden van het Java script SDK-script voor uw app](../../azure-monitor/app/javascript.md), kopieer het Java script-fragment en zorg ervoor dat u de instrumentatie sleutel vervangt door de juiste waarde voor uw Application Insights resource.

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Een Java script-WebResource maken in micro soft Dynamics CRM
1. Open uw CRM Online-exemplaar en meld u aan met beheerders bevoegdheden.
2. Open micro soft Dynamics CRM-instellingen, aanpassingen, het systeem aanpassen

    ![Micro soft Dynamics CRM-instellingen](./media/sample-mscrm/00001.png)

    ![Instellingen > aanpassingen](./media/sample-mscrm/00002.png)

1. Maak een Java script-resource.

    ![Dialoog venster nieuwe WebResource](./media/sample-mscrm/07.png)

    Geef een naam op, selecteer **script (JScript)** en open de tekst editor.

    ![Open de tekst editor](./media/sample-mscrm/00004.png)
2. Kopieer de code uit de Application Insights java script-SDK waarin u de instrumentatie sleutel eerder hebt geconfigureerd. Zorg ervoor dat u script tags negeert tijdens het kopiëren. Raadpleeg de onderstaande scherm afbeelding:

    ![De instrumentatie sleutel instellen](./media/sample-mscrm/000005.png)

    De code bevat de instrumentatie sleutel die uw Application Insights-resource identificeert.
3. Opslaan en publiceren.

    ![Opslaan en publiceren](./media/sample-mscrm/00006.png)

### <a name="instrument-forms"></a>Instrument formulieren
1. Open in micro soft CRM Online het account formulier

    ![Formulier account](./media/sample-mscrm/00007.png)
2. Open de eigenschappen van het formulier

    ![Formulier eigenschappen](./media/sample-mscrm/00008.png)
3. De Java script-WebResource toevoegen die u hebt gemaakt

    ![Menu Toevoegen](./media/sample-mscrm/13.png)

4. Sla de formulier aanpassingen op en publiceer deze.

## <a name="metrics-captured"></a>Vastgelegde metrische gegevens
U hebt nu telemetrie-vastleggen ingesteld voor het formulier. Wanneer het wordt gebruikt, worden er gegevens naar uw Application Insights-bron verzonden.

Hier vindt u voor beelden van de gegevens die u ziet.

#### <a name="application-health"></a>Toepassings status
![Voorbeeld pagina laad tijd](./media/sample-mscrm/15.png)

![Grafiek met voorbeeld pagina weergaven](./media/sample-mscrm/16.png)

Browser uitzonderingen:

![Diagram van browser uitzonderingen](./media/sample-mscrm/17.png)

Klik op de grafiek voor meer details:

![Lijst met uitzonde ringen](./media/sample-mscrm/18.png)

#### <a name="usage"></a>Gebruik
![Gebruikers, sessies en pagina weergaven](./media/sample-mscrm/19.png)

![Sessie grafieken](./media/sample-mscrm/20.png)

![Browser versies](./media/sample-mscrm/21.png)

#### <a name="browsers"></a>Browsers
![Uitsplitsing van pagina laad tijd](./media/sample-mscrm/22.png)

![Aantal sessies per browser versie](./media/sample-mscrm/23.png)

#### <a name="geolocation"></a>Geolocatie
![Aantal sessies per land](./media/sample-mscrm/24.png)

![Sessies en gebruikers per land](./media/sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Weergave aanvraag binnen pagina
![Samen vatting van pagina weergave](./media/sample-mscrm/26.png)

![Zoeken in pagina weergave gebeurtenissen](./media/sample-mscrm/27.png)

![Soortgelijke paginaweergaven](./media/sample-mscrm/28.png)

![Eigenschappen van paginaweergaven](./media/sample-mscrm/29.png)

![Pagina's per sessie](./media/sample-mscrm/30.png)

## <a name="sample-code"></a>Voorbeeldcode
[Blader door de voorbeeld code](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI
U kunt zelfs een diepere analyse uitvoeren als u [de gegevens naar micro soft power bi exporteert](../../azure-monitor/app/export-power-bi.md ).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Voor beeld van micro soft Dynamics CRM-oplossing
[Hier volgt de voorbeeld oplossing die is geïmplementeerd in micro soft Dynamics CRM](https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Meer informatie
* [Wat is Application Insights?](../../azure-monitor/app/app-insights-overview.md)
* [Application Insights voor webpagina's](../../azure-monitor/app/javascript.md)
* [Meer voor beelden en scenario's](../../azure-monitor/app/app-insights-overview.md)
