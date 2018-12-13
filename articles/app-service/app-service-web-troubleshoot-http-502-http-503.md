---
title: 502 Ongeldige gateway oplossen, 503 service niet beschikbaar fouten - Azure App Service | Microsoft Docs
description: Problemen oplossen met 502 Ongeldige gateway- en 503 service niet beschikbaar fouten in uw web-app die wordt gehost in Azure App Service.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: top-support-issue
keywords: 502 Ongeldige gateway, 503 service niet beschikbaar, 503-fout, fout 502
ms.assetid: 51cd331a-a3fa-438f-90ef-385e755e50d5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 71842f9dbc8d0454da1847c956dea3b063208836
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53259287"
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>HTTP-fouten '502 Ongeldige gateway' en '503 service niet beschikbaar' in uw Azure-web-apps oplossen
'502 Ongeldige gateway' en '503 service niet beschikbaar' zijn veelvoorkomende fouten in uw web-app die wordt gehost in [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). Dit artikel helpt bij het oplossen van deze fouten.

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning voor Azure site](https://azure.microsoft.com/support/options/) en klikt u op **ontvang ondersteuning**.

## <a name="symptom"></a>Symptoom
Wanneer u naar de web-app bladert, wordt een HTTP '502 Ongeldige Gateway' fout of een HTTP-fout '503 Service niet beschikbaar'.

## <a name="cause"></a>Oorzaak
Dit probleem wordt vaak veroorzaakt door problemen met toepassingen niveau, zoals:

* aanvragen duurt lang
* toepassing met behulp van hoge geheugen/CPU
* de toepassing is gecrasht vanwege een uitzondering.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>Stappen voor probleemoplossing '502 Ongeldige gateway' en '503 service niet beschikbaar'-fouten oplossen
Oplossen van problemen kan worden onderverdeeld in drie verschillende taken, in opeenvolgende volgorde:

1. [Bekijk en werking van de toepassing controleren](#observe)
2. [Gegevens verzamelen](#collect)
3. [Het probleem](#mitigate)

[App Service Web Apps](app-service-web-overview.md) biedt verschillende opties bij elke stap.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Bekijk en werking van de toepassing controleren
#### <a name="track-service-health"></a>Servicestatus bijhouden
Microsoft Azure publicizes telkens wanneer er een onderbreking of prestaties verslechtering van de service is. U kunt de status van de service volgen op de [Azure Portal](https://portal.azure.com/). Zie voor meer informatie, [servicestatus bijhouden](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Uw web-app bewaken
Deze optie kunt u achterhalen als uw toepassing problemen ondervindt. Klik op de blade van uw web-app, op de **aanvragen en fouten** tegel. De **Metric** blade ziet u alle metrische gegevens die u kunt toevoegen.

Sommige van de metrische gegevens die u wilt mogelijk om te controleren voor uw web-app

* Gemiddeld geheugenwerkset
* Gemiddelde reactietijd
* CPU-tijd
* Geheugenwerkset
* Aanvragen

![Monitor voor web-app voor het oplossen van HTTP-fouten van 502 Ongeldige gateway- en 503 service niet beschikbaar](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Zie voor meer informatie:

* [Web-App in Azure App Service bewaken](web-sites-monitor.md)
* [Waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a>2. Gegevens verzamelen
#### <a name="use-the-diagnostics-tool"></a>Het hulpprogramma diagnostics gebruiken
App Service biedt een intelligente en interactieve ervaring om u te helpen bij het oplossen van uw web-app zonder configuratie vereist. Als u problemen met uw web-app uitvoert, wordt het hulpprogramma diagnostics wijs wat er mis om u te helpen de juiste informatie snel en eenvoudig kunt oplossen en los het probleem is.

Voor toegang tot App Service-diagnostics, gaat u naar uw App Service-app of een App Service-omgeving in de [Azure-portal](https://portal.azure.com). Klik in het linkernavigatievenster op **vaststellen en oplossen van problemen met**.

#### <a name="use-the-kudu-debug-console"></a>Gebruik de Kudu-Console voor foutopsporing
Web Apps wordt geleverd met een console voor foutopsporing die u gebruiken kunt voor foutopsporing, verkennen, het uploaden van bestanden, evenals de JSON-eindpunten voor het ophalen van gegevens over uw omgeving. Dit heet de *Kudu-Console* of de *SCM-Dashboard* voor uw web-app.

U kunt dit dashboard openen door te gaan naar de koppeling **https://&lt;uw app-naam >.scm.azurewebsites.net/**.

Enkele van de dingen die Kudu biedt, zijn:

* omgevingsinstellingen voor uw toepassing
* logboekstream
* diagnostische dump
* fouten opsporen in een console waarin u Powershell-cmdlets en basic DOS-opdrachten kunt uitvoeren.

Een andere handige functie van Kudu is dat, als uw toepassing de eerste kans uitzonderingen genereren is, kunt u Kudu en dumpbestanden voor de SysInternals-hulpprogramma Procdump geheugen maken. Deze geheugendumps zijn momentopnamen van het proces en vaak kunt u meer gecompliceerde problemen oplossen met uw web-app.

Zie voor meer informatie over functies die beschikbaar zijn in Kudu [Azure Websites online tools die u moet weten over](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Het probleem
#### <a name="scale-the-web-app"></a>De web-app schalen
In Azure App Service, kunt voor betere prestaties en doorvoer, u de schaal waarmee u uw toepassing uitvoert aanpassen. Omhoog schalen van een web-app bestaat uit twee gerelateerde acties: uw App Service-plan in een hogere prijscategorie te wijzigen, en bepaalde instellingen te configureren nadat u hebt overgeschakeld naar de hogere prijscategorie.

Zie voor meer informatie over het omhoog schalen [een web-app schalen in Azure App Service](web-sites-scale.md).

Bovendien kunt u uw toepassing uitvoeren op meer dan één exemplaar. Dit niet alleen biedt u meer mogelijkheden voor verwerking, maar biedt ook een zekere mate van fouttolerantie. Als het proces zich op één instantie uitvalt, wordt in het andere exemplaar nog steeds blijven uitvoeren van aanvragen.

U kunt instellen om het handmatig of automatisch worden geschaald.

#### <a name="use-autoheal"></a>Gebruikmaken van AutoHeal
AutoHeal wordt gerecycled het werkproces voor uw app op basis van de instellingen die u (zoals wijzigingen in de configuratie, aanvragen, de limieten voor geheugen of de tijd die nodig is kiest voor het uitvoeren van een aanvraag). De meeste van de tijd is recyclen van het proces de snelste manier om te herstellen van een probleem. Hoewel u altijd opnieuw de web-app rechtstreeks vanuit de Azure Portal starten kunt, wordt AutoHeal het automatisch voor u doen. U hoeft alleen is bepaalde triggers toevoegen in de hoofdmap web.config voor uw web-app. Houd er rekening mee dat deze instellingen op dezelfde manier als werken zou, zelfs als uw toepassing niet een .net een is.

Zie voor meer informatie, [automatisch herstel Azure Web Sites](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-web-app"></a>De WebApp opnieuw starten
Dit is vaak de eenvoudigste manier om eenmalige problemen te verhelpen. Op de [Azure Portal](https://portal.azure.com/), op de blade van uw web-app, hebt u de opties om te stoppen of opnieuw starten van uw app.

 ![opnieuw opstarten van de app om op te lossen HTTP-fouten van 502 Ongeldige gateway- en 503 service niet beschikbaar](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

U kunt ook uw web-app met behulp van Azure Powershell beheren. Zie [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md) voor meer informatie.

