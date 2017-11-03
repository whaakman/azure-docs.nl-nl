---
title: Bewaken van Apps in Azure App Service | Microsoft Docs
description: Informatie over het bewaken van Apps in Azure App Service met behulp van de Azure-Portal.
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: byvinyal
ms.openlocfilehash: 25d3776920d683fffedcd8ac6ed0e84dfe875974
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a>How to: in Azure App Service-Apps bewaken
[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) biedt ingebouwde bewaking functionaliteit in de [Azure Portal](https://portal.azure.com).
Dit omvat de mogelijkheid om te controleren **quota** en **metrische gegevens** voor een app, evenals de App Service-abonnement in te stellen **waarschuwingen** en zelfs **schalen**automatisch op basis van deze metrische gegevens.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Understanding quota's en metrische gegevens
### <a name="quotas"></a>Quota
Toepassingen die worden gehost in App Service zijn onderworpen aan bepaalde *limieten* op de bronnen die ze kunnen gebruiken. De limieten zijn gedefinieerd door de **App Service-abonnement** die zijn gekoppeld aan de app.

Als de toepassing wordt gehost een **vrije** of **gedeelde** plant, en vervolgens de limieten op de resources die de app kunt gebruiken, zijn gedefinieerd door **quota**.

Als de toepassing wordt gehost een **Basic**, **standaard** of **Premium** plant, en vervolgens de limieten op de bronnen die ze kunnen gebruiken die zijn ingesteld door de **grootte**(Klein, normaal, groot) en **aantal exemplaar** (1, 2, 3,...) van de **App Service-abonnement**.

**Quota's** voor **vrije** of **gedeelde** apps zijn:

* **CPU(Short)**
  * De hoeveelheid CPU toegestaan voor deze toepassing in een interval van 5 minuten. Dit quotum wordt opnieuw ingesteld om de 5 minuten.
* **CPU(Day)**
  * Totale hoeveelheid CPU toegestaan voor deze toepassing in een dag. Dit quotum wordt opnieuw ingesteld voor elke 24 uur om middernacht UTC.
* **Geheugen**
  * Totale hoeveelheid geheugen die is toegestaan voor deze toepassing.
* **Bandbreedte**
  * Totale hoeveelheid bandbreedte voor uitgaande toegestaan voor deze toepassing in een dag.
    Dit quotum wordt opnieuw ingesteld voor elke 24 uur om middernacht UTC.
* **Bestandssysteem**
  * Totale hoeveelheid opslag dat is toegestaan.

Het alleen van toepassing op apps die worden gehost op quotum **Basic**, **standaard** en **Premium** plannen is **bestandssysteem**.

Meer informatie over de specifieke quota en limieten functies beschikbaar zijn voor de verschillende App Service-SKU's vindt u hier: [Servicelimieten voor Azure-abonnement](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Het afdwingen van quota
Als een toepassing in het gebruik ervan hoger is dan de **CPU (korte)**, **CPU (dag)**, of **bandbreedte** quota en vervolgens de toepassing wordt gestopt totdat het quotum is opnieuw ingesteld. Tijdens deze periode kunnen alle inkomende aanvragen resulteert in een **HTTP 403**.
![][http403]

Als de toepassing **geheugen** quotum wordt overschreden, en vervolgens de toepassing wordt niet opnieuw worden gestart.

Als de **bestandssysteem** quotum wordt overschreden, en vervolgens een bewerking mislukt, dit omvat het schrijven naar Logboeken geschreven.

Quota's kunnen worden verhoogd of verwijderd uit uw app door het upgraden van uw App Service-abonnement.

### <a name="metrics"></a>Metrische gegevens
**Metrische gegevens** bevatten informatie over de app, of het gedrag van de App Service-abonnement.

Voor een **toepassing**, zijn de beschikbare metrische gegevens:

* **Gemiddelde reactietijd**
  * De gemiddelde tijd voor de app voor aanvragen in ms.
* **Gemiddelde geheugen werkset**
  * De gemiddelde hoeveelheid geheugen in MIB's die worden gebruikt door de app.
* **CPU-tijd**
  * De hoeveelheid CPU in seconden verbruikt door de app. Voor meer informatie over deze metrische Zie: [tegenover CPU-percentage van CPU-tijd](#cpu-time-vs-cpu-percentage)
* **Gegevens In**
  * De hoeveelheid binnenkomende bandbreedte door de app in MIB's.
* **Gegevens uit**
  * De hoeveelheid bandbreedte voor uitgaande gebruikt door de app in MIB's.
* **HTTP-2xx**
  * Aantal aanvragen, wat resulteert in een http-statuscode > = 200 maar < 300.
* **HTTP-3xx**
  * Aantal aanvragen, wat resulteert in een http-statuscode > 300 maar 400 < =.
* **401 HTTP**
  * Het aantal aanvragen, wat resulteert in een 401 HTTP-statuscode.
* **HTTP-fout 403**
  * Het aantal aanvragen, wat resulteert in 403 HTTP-statuscode.
* **HTTP 404**
  * Het aantal aanvragen, wat resulteert in een HTTP 404-statuscode.
* **HTTP 406**
  * Het aantal aanvragen, wat resulteert in 406 HTTP-statuscode.
* **HTTP-4xx**
  * Aantal aanvragen, wat resulteert in een http-statuscode > = 400 maar < 500.
* **HTTP-Server-fouten**
  * Aantal aanvragen, wat resulteert in een http-statuscode > = 500 maar < 600.
* **Geheugen-werkset**
  * Huidige hoeveelheid geheugen die wordt gebruikt door de app in MIB's.
* **Aanvragen**
  * Totaal aantal aanvragen ongeacht hun resulterende HTTP-statuscode.

Voor een **App Service-abonnement**, zijn de beschikbare metrische gegevens:

> [!NOTE]
> App Service plan metrische gegevens zijn alleen beschikbaar voor abonnementen in **Basic**, **standaard** en **Premium** SKU.
> 
> 

* **CPU-Percentage**
  * Het gemiddelde CPU-gebruik over alle exemplaren van het plan.
* **Geheugenpercentage**
  * Het gemiddelde geheugengebruik in alle exemplaren van het plan.
* **Gegevens In**
  * De gemiddelde binnenkomende bandbreedte die wordt gebruikt in alle exemplaren van het plan.
* **Gegevens uit**
  * Het gemiddelde voor uitgaande bandbreedte die wordt gebruikt in alle exemplaren van het plan.
* **Wachtrijlengte voor schijf**
  * Het gemiddelde aantal zowel lezen als schrijven aanvragen in de wachtrij zijn geplaatst in de opslag. Een hoge wachtrijlengte is een indicatie van een toepassing die mogelijk als gevolg van overmatige schijf-i/o vertragen.
* **HTTP-wachtrijlengte**
  * Het gemiddelde aantal HTTP-aanvragen die zitten in de wachtrij moest voordat het wordt voldaan. Een hoog of toenemende HTTP-wachtrijlengte is een symptoom van een plan zwaar wordt belast.

### <a name="cpu-time-vs-cpu-percentage"></a>Vs CPU-percentage van CPU-tijd
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Er zijn 2 metrische gegevens die overeenkomen met het CPU-gebruik. **CPU-tijd** en **CPU-percentage**

**CPU-tijd** is nuttig voor apps die worden gehost **vrije** of **gedeelde** plannen omdat een van hun quota is gedefinieerd in CPU minuten gebruikt door de app.

**CPU-percentage** aan de andere kant is handig voor apps die worden gehost **basic**, **standaard** en **premium** plannen omdat ze kunnen worden uitgebreid en met deze metriek is een goede indicatie van het totale gebruik voor alle exemplaren.

## <a name="metrics-granularity-and-retention-policy"></a>Samenvattingen van de metrische gegevens en bewaarbeleid
Metrische gegevens voor een toepassing en de app service-abonnement zijn geregistreerd en samenvoegen met de service met de volgende granulaties en bewaarbeleidsregels:

* **Minuut** granulatie metrische gegevens worden bewaard voor **48 uur**
* **Uur** granulatie metrische gegevens worden bewaard voor **30 dagen**
* **Dag** granulatie metrische gegevens worden bewaard voor **90 dagen**

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Controle-quota's en metrische gegevens in de Azure-Portal.
U kunt de status van de verschillende bekijken **quota** en **metrische gegevens** die invloed hebben op een toepassing in de [Azure Portal](https://portal.azure.com).

![][quotas]
**Quota's** vindt u onder Instellingen >**quota**. De UX kunt u bekijken: (1) de naam van de quota, (2) het interval opnieuw instellen, (3) de huidige limiet en (4) de huidige waarde.

![][metrics]
**Metrische gegevens** toegang rechtstreeks vanaf de resourceblade kan zijn. U kunt ook de grafiek door aanpassen: (1) **klikt u op** op en selecteert u (2) **grafiek bewerken**.
Hier kunt u de (3) **tijdsbereik**, (4) **grafiektype**, en 5, **metrische gegevens** om weer te geven.  

U kunt meer informatie over metrische gegevens hier: [service metrische gegevens controleren](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Waarschuwingen en voor automatisch schalen
Metrische gegevens voor een App of een App Service-plan kan worden aangesloten tot waarschuwingen, voor meer informatie over deze, Zie [ontvangen van meldingen van waarschuwingen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

App Service-apps die zijn gehost in basic, standard of premium App Service-abonnementen ondersteunen **automatisch schalen**. Zodoende kunt u regels te configureren die de App Service plan metrische gegevens controleren en kunnen vergroten of verkleinen het aantal exemplaren verstrekken aanvullende bronnen die nodig is of opslaan geld wanneer de toepassing te veel inrichten is. U kunt meer informatie over automatisch schalen hier: [How to Scale](../monitoring-and-diagnostics/insights-how-to-scale.md) en hier [Best practices voor het bewaken van de Azure-automatisch schalen](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](https://azure.microsoft.com/try/app-service/). Hier kunt u direct een tijdelijke web-app maken in App Service. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.
> 
> 

## <a name="whats-changed"></a>Wat is er gewijzigd
* Als u van Websites wilt overstappen op App Service, raadpleegt u de volgende handleiding: [Azure App Service en de invloed ervan op bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
