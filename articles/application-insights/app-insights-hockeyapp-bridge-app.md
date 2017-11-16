---
title: HockeyApp-gegevens in Azure Application Insights verkennen | Microsoft Docs
description: Analyseer gebruik en prestaties van uw app in Azure met Application Insights.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: mbullwin
ms.openlocfilehash: bec2b2dc8a29e5c94e3d3c7f2f0333e07bfe3a4f
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a>HockeyApp-gegevens in Application Insights verkennen

> [!NOTE]
> Visual Studio App Center is nu de aanbevolen service van Microsoft voor het bewaken van de nieuwe mobiele apps. [Meer informatie over het instellen van uw apps met App Center en Application Insights](app-insights-mobile-center-quickstart.md).
> 
> 

[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) is een service voor het bewaken van live desktop- en mobiele apps. U kunt van HockeyApp, aangepaste verzenden en traceren telemetrie in om te controleren van het gebruik en helpt bij de diagnose (naast het ophalen van crashgegevens). Deze stroom telemetrie kan worden opgevraagd met de krachtige [Analytics](app-insights-analytics.md) functie van [Azure Application Insights](app-insights-overview.md). Bovendien kunt u [exporteren van het aangepaste en telemetrie traceren](app-insights-export-telemetry.md). Als u wilt dat deze functies, moet u brug waarlangs HockeyApp aangepaste gegevens naar Application Insights doorstuurt instellen.

## <a name="the-hockeyapp-bridge-app"></a>De app HockeyApp brug
De HockeyApp brug App is de core-functie waarmee u toegang tot uw aangepaste HockeyApp en in Application Insights-tracetelemetrie via de analyses en continue Export-functies. Aangepaste en tracering verzamelde gebeurtenissen per HockeyApp na het maken van de HockeyApp Bridge App toegankelijk is vanaf deze functies. Kijken hoe u een van deze Apps Bridge instelt.

Open in de HockeyApp, accountinstellingen, [API Tokens](https://rink.hockeyapp.net/manage/auth_tokens). Een nieuw token maken of een bestaande hergebruiken. De minimale rechten vereist zijn 'alleen-lezen'. Een kopie van de API-token te nemen.

![Een API HockeyApp-token ophalen](./media/app-insights-hockeyapp-bridge-app/01.png)

Open de Microsoft Azure portal en [een Application Insights-resource maken](app-insights-create-new-resource.md). Toepassingstype ingesteld op 'HockeyApp bridge application':

![Nieuwe Application Insights-resource](./media/app-insights-hockeyapp-bridge-app/02.png)

U hoeft niet in te stellen van een naam - wordt deze automatisch worden ingesteld van de HockeyApp-naam.

De HockeyApp bridge velden worden weergegeven. 

![Geef de velden brug](./media/app-insights-hockeyapp-bridge-app/03.png)

Voer de HockeyApp-token dat u eerder hebt genoteerd. Deze actie wordt het vervolgkeuzemenu 'HockeyApp Application' met alle HockeyApp-toepassingen. Selecteer de gateway die u wilt gebruiken en voltooi de rest van de velden. 

Open de nieuwe resource. 

Houd er rekening mee dat de gegevens bij te starten waarbij de neemt.

![Application Insights-resource wachten op gegevens](./media/app-insights-hockeyapp-bridge-app/04.png)

Dat is alles. Aangepaste en tracering verzamelde gegevens in uw app HockeyApp geïnstrumenteerd vanaf dit punt is nu ook beschikbaar in de analyses en continue Export functies van Application Insights.

Laten we kort Bekijk elk van deze functies nu beschikbaar voor u.

## <a name="analytics"></a>Analytische gegevens
Analytics is een krachtig hulpprogramma ad hoc-query's van uw gegevens, zodat u kunt onderzoeken en analyseren van uw Telemetrie en snel hoofdoorzaken en patronen te detecteren.

![Analytische gegevens](./media/app-insights-hockeyapp-bridge-app/05.png)

* [Meer informatie over Analytics](app-insights-analytics-tour.md)

## <a name="continuous-export"></a>Continue export
Continue Export kunt u uw gegevens exporteren naar een Azure Blob Storage-container. Dit is handig als u nodig hebt om uw gegevens langer dan de bewaarperiode die momenteel worden aangeboden door Application Insights te houden. U kunt dat de gegevens in blob-opslag, in een SQL-Database of de oplossing van uw voorkeur gegevensopslag worden verwerkt.

[Meer informatie over de continue Export](app-insights-export-telemetry.md)

## <a name="next-steps"></a>Volgende stappen
* [Analytics toepassen op uw gegevens](app-insights-analytics-tour.md)

