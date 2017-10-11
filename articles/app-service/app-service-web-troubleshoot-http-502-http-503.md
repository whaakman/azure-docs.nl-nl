---
title: 502 Slechte gateway oplossen, 503 service niet beschikbaar fouten | Microsoft Docs
description: Problemen oplossen met 502 Slechte gateway en 503 service niet beschikbaar fouten in uw web-app in Azure App Service wordt gehost.
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: top-support-issue
keywords: 502 Slechte gateway 503 service niet beschikbaar, 503-fout, fout 502
ms.assetid: 51cd331a-a3fa-438f-90ef-385e755e50d5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 397a6aaf7dc27adfa0fc0e722b8a2be5cc1d75f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>HTTP-fouten van '502 Slechte gateway' en '503 service niet beschikbaar' in uw Azure-web-apps
'502 Slechte gateway' en '503 service niet beschikbaar' zijn veelvoorkomende fouten in uw web-app gehost in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). In dit artikel helpt u bij deze fouten oplossen.

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op [de Azure MSDN en de Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook kunt u ook een incident voor ondersteuning van Azure bestand. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en klik op **Get Support**.

## <a name="symptom"></a>Symptoom
Wanneer u de web-app bladert, wordt een HTTP '502 Slechte Gateway' fout of een HTTP-fout '503 Service niet beschikbaar'.

## <a name="cause"></a>Oorzaak
Dit probleem wordt vaak veroorzaakt door problemen met toepassingen niveau, zoals:

* aanvragen duurt lang
* toepassing met hoge geheugen/CPU
* de toepassing is gecrasht vanwege een uitzondering.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>Stappen voor probleemoplossing om op te lossen '502 Slechte gateway' en '503 service niet beschikbaar'-fouten
Het oplossen van problemen kan worden onderverdeeld in drie verschillende taken in opeenvolgende volgorde:

1. [Bekijken en controleren van gedrag van toepassingen](#observe)
2. [Gegevens verzamelen](#collect)
3. [Het probleem te verhelpen](#mitigate)

[App Service Web Apps](/services/app-service/web/) biedt verschillende opties bij elke stap.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Bekijken en controleren van gedrag van toepassingen
#### <a name="track-service-health"></a>Servicestatus bijhouden
Microsoft Azure publicizes telkens wanneer er een service wordt onderbroken of de prestaties nadelig beïnvloeden is. U kunt de status van de service bijhouden op het [Azure Portal](https://portal.azure.com/). Zie voor meer informatie [bijhouden servicestatus](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Uw web-app bewaken
Deze optie kunt u achterhalen als uw toepassing problemen ondervindt. Klik op de blade van uw web-app de **aanvragen en fouten** tegel. De **metriek** blade ziet u alle metrische gegevens die u kunt toevoegen.

Sommige van de metrische gegevens die u voor uw web-app kunt bewaken

* Gemiddelde geheugen werkset
* Gemiddelde reactietijd
* CPU-tijd
* Geheugen-werkset
* Aanvragen

![Monitor voor web-app voor het oplossen van HTTP-fouten van 502 Slechte gateway en 503 service niet beschikbaar](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Zie voor meer informatie:

* [Web-Apps bewaken in Azure App Service](web-sites-monitor.md)
* [Waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a>2. Gegevens verzamelen
#### <a name="use-the-azure-app-service-support-portal"></a>Gebruik de Portal-Azure App Service-ondersteuning
Web Apps biedt u de mogelijkheid om op te lossen problemen met betrekking tot uw web-app door te kijken HTTP Logboeken, gebeurtenislogboeken proces dumpbestanden en meer. U toegang hebt tot deze informatie met onze portal ondersteuning op **http://&lt;uw app-naam >.scm.azurewebsites.net/Support**

De portal voor ondersteuning van Azure App Service biedt drie afzonderlijke tabbladen ter ondersteuning van de drie stappen van een algemeen scenario voor het oplossen van problemen:

1. Huidige gedrag observeren
2. Door het verzamelen van diagnostische gegevens en uitvoeren van de ingebouwde analyzers analyseren
3. Beperken

Als het probleem nu voordoet zich, klikt u op **analyseren** > **Diagnostics** > **onderzoeken nu** een diagnostische sessie voor u te maken, die HTTP-Logboeken, Logboeken, geheugen dumpbestanden, PHP-foutenlogboek en PHP-proces rapport verzamelt.

Nadat de gegevens worden verzameld, wordt ook een analyse uitgevoerd op de gegevens en bieden u een HTML-rapport.

Als u downloaden van de gegevens standaard wilt, zou deze opgeslagen in de map D:\home\data\DaaS.

Zie voor meer informatie over de ondersteuning van Azure App Service-portal [nieuwe Updates naar de Site-extensie ondersteuning voor Azure Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Gebruik de Kudu-Console voor foutopsporing
Web-Apps wordt geleverd met een Foutopsporingsconsole die u gebruiken kunt voor foutopsporing, verkennen, het uploaden van bestanden, evenals de JSON-eindpunten voor het ophalen van informatie over uw omgeving. Dit heet de *Kudu-Console* of de *SCM Dashboard* voor uw web-app.

U kunt dit dashboard openen door op de koppeling **https://&lt;uw app-naam >.scm.azurewebsites.net/**.

Enkele van de Kudu verschaft zaken zijn:

* omgevingsinstellingen voor uw toepassing
* logboekstream
* diagnostische dump
* fouten opsporen console waarin u Powershell-cmdlets en basic DOS-opdrachten kunt uitvoeren.

Een andere handige functie van Kudu is dat, als uw toepassing die eerste kans uitzonderingen, kunt u Kudu en dumpbestanden voor het SysInternals-hulpprogramma Procdump geheugen maken. Deze geheugendumps momentopnamen van het proces zijn en vaak kunt u meer gecompliceerde problemen oplossen met uw web-app.

Zie voor meer informatie over functies die beschikbaar zijn in Kudu [Azure Websites on line hulpmiddelen die u moet weten over](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Het probleem te verhelpen
#### <a name="scale-the-web-app"></a>De web-app schalen
In Azure App Service kunt voor betere prestaties en doorvoer, u de schaal waarmee u uw toepassing uitvoert aanpassen. Schalen van een web-app omvat twee gerelateerde acties: uw App Service-abonnement wijzigen in een hogere prijscategorie en bepaalde instellingen configureren nadat u hebt overgeschakeld naar de hogere prijscategorie.

Zie voor meer informatie over het schalen [schalen van een web-app in Azure App Service](web-sites-scale.md).

U kunt bovendien uw toepassing uitvoeren in meer dan één exemplaar. Dit niet alleen biedt u meer mogelijkheden voor verwerking, maar biedt u eveneens bepaalde hoeveelheid fouttolerantie. Als het proces uitgeschakeld op één exemplaar wordt, wordt in het andere exemplaar echter blijven voldoen aan aanvragen.

U kunt instellen om de schaal worden handmatig of automatisch.

#### <a name="use-autoheal"></a>Gebruik AutoHeal
AutoHeal wordt gerecycled het werkproces voor uw app op basis van de instellingen die u (zoals wijzigingen in de configuratie, aanvragen, limieten op basis van geheugen of de benodigde tijd kiest voor het uitvoeren van een aanvraag). De meeste gevallen, is recyclen van het proces de snelste manier om te herstellen van een probleem. Hoewel u altijd opnieuw van de web-app rechtstreeks in de Azure Portal opstarten kunt, wordt AutoHeal het automatisch voor u doen. Hoeft u sommige triggers toevoegen in web.config voor de hoofdmap voor uw web-app is. Houd er rekening mee dat deze instellingen op dezelfde manier werken zou, zelfs als uw toepassing niet een .net een is.

Zie voor meer informatie [Azure websites automatisch herstel](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-web-app"></a>Opnieuw opstarten van de web-app
Dit is vaak de eenvoudigste manier om eenmalige problemen herstellen. Op de [Azure Portal](https://portal.azure.com/), op de blade van uw web-app, hebt u de opties te stoppen of opnieuw opstarten van uw app.

 ![opnieuw opstarten van de app om op te lossen HTTP-fouten van 502 Slechte gateway en 503 service niet beschikbaar](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

U kunt ook uw web-app met Azure Powershell beheren. Zie [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md) voor meer informatie.

