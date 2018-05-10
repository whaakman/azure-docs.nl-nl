---
title: Overzicht van de diagnostische gegevens van Azure App Service | Microsoft Docs
description: Meer informatie over hoe u problemen kunt oplossen met uw web-app met diagnostische gegevens van App Service.
keywords: App service, azure app service, diagnostische gegevens, ondersteuning, web-app, het oplossen van problemen zelfhulp
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
ms.openlocfilehash: 50e0e9f5edc18aac42ee80e232f70e09736124bc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="azure-app-service-diagnostics-overview"></a>Overzicht van de diagnostische gegevens van Azure App Service 

Wanneer u een webtoepassing uitvoert, die u wilt worden voorbereid voor eventuele problemen die zich voordoen kunnen, 500 fouten aan uw gebruikers om u aan te geven die uw site is niet beschikbaar. App Service diagnostische gegevens is een intelligente en interactieve ervaring om u te helpen bij het oplossen van uw web-app zonder configuratie vereist. Wanneer u in problemen met uw web-app uitvoert, wijst u informatie over wat er verkeerde om u te helpen de juiste informatie gemakkelijker en sneller oplossen en los het probleem diagnostische gegevens van App Service. 
 
Hoewel deze ervaring handig is als je problemen met uw web-app in de afgelopen 24 uur hebt, wordt de diagnostische grafieken beschikbaar voor u te allen tijde analyseren zijn. Extra hulpprogramma's voor probleemoplossing en koppelingen naar nuttige documentatie en forums bevinden zich in de rechterkolom.

Diagnostische gegevens van App Service werkt niet alleen uw app in Windows, maar ook apps op [Linux/containers](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro), [App Service-omgeving](https://docs.microsoft.com/en-us/azure/app-service/environment/intro), en [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview). 

## <a name="open-app-service-diagnostics"></a>Open diagnostische gegevens van App Service

Als u diagnostische gegevens van App Service, gaat u naar uw App Service-app of een App Service-omgeving in de [Azure-portal](https://portal.azure.com). Klik in het linkernavigatievenster op **diagnosticeren en oplossen van problemen**. 

Voor Azure Functions, gaat u naar uw app functie en in de bovenste navigatiebalk, klikt u op **platformfuncties** en selecteer **diagnosticeren en oplossen van problemen** van de **bewaking**sectie. 

![Startpagina](./media/app-service-diagnostics/Homepage1.png)

## <a name="health-checkup"></a>Health checkup

Als u niet weet wat is er iets mis met uw web-app of niet weet waar u de problemen met het starten, is de status checkup een goede plaats om te starten. De checkup health analyseert uw webtoepassingen om u te bieden een snelle, interactieve overzicht die op wat in orde is en wat is het probleem dat u waar u wijst wilt zoeken voor het onderzoeken van het probleem. De interface intelligente en interactieve biedt richtlijnen door het proces voor het oplossen van problemen.  

![Health checkup](./media/app-service-diagnostics/HealthCheckup2.png)

Als er een probleem is gedetecteerd met een specifiek probleemcategorie in de afgelopen 24 uur, kunt u de volledige diagnostisch rapport bekijken en diagnostische gegevens van App Service u mogelijk gevraagd te advies meer over probleemoplossing en de volgende stappen voor een meer begeleide ervaring weergeven.

![Probleemoplossing en de volgende stappen](./media/app-service-diagnostics/Troubleshooting3.png)

## <a name="tile-shortcuts"></a>Tegel snelkoppelingen

Als u precies wat voor soort informatie die u zoekt voor probleemoplossing, gaat de tegel snelkoppelingen u rechtstreeks naar de volledige diagnostisch rapport van de probleemcategorie dat u geïnteresseerd bent in. Vergeleken met de health-checkup, de tegel snelkoppelingen meer rechtstreekse zijn, maar kleiner manier van de toegang tot metrische gegevens over uw diagnostische geleid. Als onderdeel van de tegel snelkoppelingen, dit is ook waar vindt u **diagnostische hulpprogramma's voor** die zijn meer geavanceerde hulpmiddelen waarmee u problemen met betrekking tot problemen met toepassingen, traagheid en verbindingsreeksen onderzoeken. 

![Tegel snelkoppelingen](./media/app-service-diagnostics/TileShortcuts4.png)

## <a name="diagnostic-report"></a>Diagnostisch rapport

Hiermee wordt aangegeven of u meer wilt weten nadat een [health checkup](#health-checkup) of u hebt geklikt op een van de [tegel snelkoppelingen](#tile-shortcuts), de volledige diagnostisch rapport leert u relevante grafische metrische gegevens van de afgelopen 24 uur. Als uw app-uitvaltijd ervaringen, wordt vertegenwoordigd door een oranje balk onder de tijdlijn. U kunt een van de oranje balken selecteren de uitvaltijd voor Zie Opmerkingen over deze uitvaltijd en de voorgestelde stappen voor probleemoplossing. 

![Diagnostisch rapport](./media/app-service-diagnostics/DiagnosticReport5.png)


## <a name="investigating-application-code-issues"></a>Onderzoek van toepassingsproblemen code

Omdat veel app zijn problemen met problemen in uw toepassingscode, diagnostische gegevens van App Service kan worden geïntegreerd met [Application Insights](https://azure.microsoft.com/services/application-insights/) markeren uitzonderingen en afhankelijkheidsproblemen met elkaar correleren met de geselecteerde uitvaltijd. Application Insights heeft afzonderlijk worden ingeschakeld. 

Als u wilt weergeven in Application Insights-uitzonderingen en afhankelijkheden, selecteer de **Web-App niet actief** of **Web App trage** snelkoppelingen tegel. 

![Application Insights](./media/app-service-diagnostics/AppInsights6.png)

