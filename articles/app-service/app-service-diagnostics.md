---
title: Overzicht van diagnostische gegevens - Azure App Service | Microsoft Docs
description: Meer informatie over hoe u problemen kunt oplossen met uw web-app met App Service-diagnostics.
keywords: appservice, azure appservice, diagnostische gegevens, ondersteuning, web-app, het oplossen van problemen zelfhulp
services: app-service
documentationcenter: ''
author: jen7714
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: jennile
ms.custom: seodec18
ms.openlocfilehash: a8b256f43d8e4103404ab4276431ceb06d9ed36a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53255139"
---
# <a name="azure-app-service-diagnostics-overview"></a>Overzicht van Azure App Service-diagnostics 

Wanneer u een web-App uitvoert, die u wilt worden voorbereid voor eventuele problemen die zich voordoen kunnen, van 500-fouten naar uw gebruikers om u aan te geven dat uw site niet actief is. App Service-diagnostics is een intelligente en interactieve ervaring om op te lossen van uw web-app zonder configuratie vereist. Als u problemen met uw web-app uitvoert, wordt App Service-diagnostics ontdekt u wat er mis om u te helpen de juiste informatie snel en eenvoudig kunt oplossen en los het probleem is. 
 
Hoewel deze ervaring handig is als u problemen met uw web-app in de afgelopen 24 uur ondervindt, wordt de diagnostische grafieken worden beschikbaar voor u te allen tijde kunt analyseren. Extra hulpprogramma's voor probleemoplossing en koppelingen naar nuttige documentatie en forums bevinden zich in de rechterkolom.

App Service-diagnostics werkt niet alleen uw app op Windows, maar ook apps op [Linux/containers](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro), [App Service-omgeving](https://docs.microsoft.com/azure/app-service/environment/intro), en [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview). 

## <a name="open-app-service-diagnostics"></a>Open App Service-diagnostics

Voor toegang tot App Service-diagnostics, gaat u naar uw App Service-app of een App Service-omgeving in de [Azure-portal](https://portal.azure.com). Klik in het linkernavigatievenster op **vaststellen en oplossen van problemen met**. 

Voor Azure Functions, gaat u naar uw functie-app en in de bovenste navigatiebalk, klikt u op **platformfuncties** en selecteer **vaststellen en oplossen van problemen met** uit de **bewaking**sectie. 

![Startpagina](./media/app-service-diagnostics/Homepage1.png)

## <a name="health-checkup"></a>Gezondheidscontrole

Als u niet weet wat er mis is met uw web-app is of niet weet waar u uw problemen met het starten, is de gezondheidscontrole een goede plaats om te starten. De gezondheidscontrole analyseert uw webtoepassingen zodat u een snelle, interactieve overzicht die op wat in orde is en wat is het probleem aangegeven waar u wijst wilt zoeken voor het onderzoeken van het probleem. De intelligente en interactieve interface biedt u instructies voor het proces voor het oplossen van problemen.  

![Gezondheidscontrole](./media/app-service-diagnostics/HealthCheckup2.png)

Als er een probleem is gedetecteerd met een specifiek probleemcategorie in de afgelopen 24 uur, kunt u de volledige diagnostische rapport bekijken en App Service-diagnostics wordt u om weer te geven van advies voor meer probleemoplossing en de volgende stappen voor een meer begeleide ervaring mogelijk gevraagd.

![Probleemoplossing en de volgende stappen](./media/app-service-diagnostics/Troubleshooting3.png)

## <a name="tile-shortcuts"></a>Tegel snelkoppelingen

Als u precies wat voor soort informatie die u zoekt voor probleemoplossing weet, gaat de tegel snelkoppelingen u rechtstreeks naar de volledige diagnostisch rapport van de probleemcategorie waarin u geïnteresseerd bent. Vergeleken met de gezondheidscontrole, de tegel snelkoppelingen zijn de meer rechtstreeks, maar minder begeleide wijze van toegang tot uw metrische gegevens voor diagnose. Als onderdeel van de tegel snelkoppelingen, dit is ook waar u kunt vinden **diagnostische hulpprogramma's voor** die zijn meer geavanceerde hulpprogramma's waarmee u problemen met betrekking tot problemen met de code van toepassingen, traagheid, verbindingsreeksen en onderzoeken. 

![Tegel snelkoppelingen](./media/app-service-diagnostics/TileShortcuts4.png)

## <a name="diagnostic-report"></a>Diagnostisch rapport

Of u wilt u meer informatie nadat u een [gezondheidscontrole](#health-checkup) of u hebt geklikt op een van de [tegel snelkoppelingen](#tile-shortcuts), de volledige diagnostisch rapport ziet u relevante grafische metrische gegevens van de afgelopen 24 uur. Als uw app uitvaltijd ondervindt, wordt deze vertegenwoordigd door een oranje balk onder de tijdlijn. U kunt een van de oranje balken om te selecteren van de downtime voor opmerkingen over deze uitvaltijd en de voorgestelde stappen voor probleemoplossing. 

![Diagnostisch rapport](./media/app-service-diagnostics/DiagnosticReport5.png)


## <a name="investigating-application-code-issues"></a>Onderzoek van toepassingsproblemen code

Omdat veel app problemen zijn met betrekking tot problemen in uw toepassingscode, App Service-diagnostics kan worden geïntegreerd met [Application Insights](https://azure.microsoft.com/services/application-insights/) markeren van uitzonderingen en afhankelijkheidsproblemen in te correleren met de geselecteerde downtime. Application Insights beschikt over afzonderlijk worden ingeschakeld. 

Als u wilt weergeven in Application Insights-uitzonderingen en afhankelijkheden, selecteert u de **Web-App omlaag** of **Web-App langzaam** snelkoppelingen tegel. 

![Application insights](./media/app-service-diagnostics/AppInsights6.png)

