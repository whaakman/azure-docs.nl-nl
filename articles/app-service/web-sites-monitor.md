---
title: Apps bewaken in Azure App Service | Microsoft Docs
description: Informatie over het bewaken van Apps in Azure App Service met behulp van de Azure-portal.
services: app-service
documentationcenter: ''
author: btardif
manager: erikre
editor: ''
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: byvinyal
ms.openlocfilehash: 6334b4cc50bfa6dca709fdc9d65938f0fec3ad1c
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956744"
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a>Hoe: Apps bewaken in Azure App Service
[App Service](https://go.microsoft.com/fwlink/?LinkId=529714) biedt ingebouwde functionaliteit voor bewaking in de [Azure-portal](https://portal.azure.com).
De Azure-portal biedt de mogelijkheid om te controleren **quota** en **metrische gegevens** voor een app, evenals de App Service-plan, instellen van **waarschuwingen** en zelfs **schalen**  automatisch op basis van deze metrische gegevens.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Informatie over quota's en metrische gegevens
### <a name="quotas"></a>Quota
Toepassingen die worden gehost in App Service zijn afhankelijk van bepaalde *limieten* op de resources die kan worden gebruikt. De limieten zijn gedefinieerd door de **App Service-plan** die zijn gekoppeld aan de app.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Als de toepassing wordt gehost een **gratis** of **gedeelde** van plan bent, en vervolgens de limieten voor de resources die de app kan gebruikt worden gedefinieerd door **quota**.

Als de toepassing wordt gehost een **Basic**, **Standard** of **Premium** van plan bent, en vervolgens de limieten voor de resources die kan worden gebruikt zijn ingesteld door de **grootte**(Klein, normaal, groot) en **aantal instanties** (1, 2, 3,...) van de **App Service-plan**.

**Quota** voor **gratis** of **gedeelde** apps zijn:

* **CPU(Short)**
  * De hoeveelheid CPU toegestaan voor deze toepassing in een interval van 5 minuten. Dit quotum stelt om de vijf minuten.
* **CPU(Day)**
  * Totale hoeveelheid CPU toegestaan voor deze toepassing in een dag. Dit quotum stelt elke 24 uur plaats om middernacht UTC.
* **Geheugen**
  * Totale hoeveelheid geheugen die is toegestaan voor deze toepassing.
* **Bandwidth**
  * Totale hoeveelheid uitgaande bandbreedte toegestaan voor deze toepassing in een dag.
    Dit quotum stelt elke 24 uur plaats om middernacht UTC.
* **Bestandssysteem**
  * Totale hoeveelheid opslag die zijn toegestaan.

Het alleen van toepassing op apps die worden gehost op quotum **Basic**, **Standard**, en **Premium** abonnementen is **bestandssysteem**.

Meer informatie over de specifieke quota's, limieten en functies die beschikbaar zijn voor de verschillende App Service-SKU's vindt u hier: [Servicelimieten van Azure-abonnement](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Het afdwingen van quota
Als een toepassing groter is dan de **CPU (kort)**, **CPU (dag)**, of **bandbreedte** quotum vervolgens de toepassing wordt gestopt, totdat het quotum wordt opnieuw ingesteld. Gedurende deze periode worden alle binnenkomende aanvragen resulteren in een **HTTP 403**.
![][http403]

Als de toepassing **geheugen** quotum wordt overschreden, en vervolgens de toepassing opnieuw wordt opgestart.

Als de **bestandssysteem** quotum wordt overschreden, en vervolgens een schrijven bewerking mislukt, waaronder alle schrijfbewerkingen naar de logboeken.

Quota kunnen worden vergroot of verwijderd uit uw app door te upgraden van uw App Service-plan.

### <a name="metrics"></a>Metrische gegevens
**Metrische gegevens** bevatten informatie over de app, of het gedrag van App Service-plan.

Voor een **toepassing**, de beschikbare metrische gegevens zijn:

* **Gemiddelde reactietijd**
  * De gemiddelde tijd voor de app-aanvragen verwerken in ms.
* **Gemiddeld geheugenwerkset**
  * De gemiddelde hoeveelheid geheugen in MIB's die worden gebruikt door de app.
* **CPU-tijd**
  * De hoeveelheid CPU in seconden die worden gebruikt door de app. Zie voor meer informatie over deze metrische gegevens: [vs CPU-percentage van CPU-tijd](#cpu-time-vs-cpu-percentage)
* **Gegevens In**
  * De hoeveelheid binnenkomende bandbreedte verbruikt door de app in MIB's.
* **Uitgaande gegevens**
  * De hoeveelheid uitgaande bandbreedte verbruikt door de app in MIB's.
* **HTTP 2xx**
  * Aantal aanvragen, wat resulteert in een HTTP-statuscode > = 200 maar < 300.
* **HTTP 3xx**
  * Aantal aanvragen, wat resulteert in een HTTP-statuscode > = 300 maar < 400.
* **HTTP 401**
  * Het aantal aanvragen, wat resulteert in een 401 HTTP-statuscode.
* **HTTP-fout 403**
  * Het aantal aanvragen dat leidt tot de statuscode HTTP 403.
* **Http 404**
  * Het aantal aanvragen dat leidt tot HTTP 404-statuscode.
* **HTTP 406**
  * Het aantal aanvragen dat leidt tot HTTP 406-statuscode.
* **HTTP 4xx**
  * Aantal aanvragen, wat resulteert in een HTTP-statuscode > = 400 maar < 500.
* **HTTP-serverfouten**
  * Aantal aanvragen, wat resulteert in een HTTP-statuscode > = 500, maar < 600.
* **Geheugenwerkset**
  * Huidige hoeveelheid geheugen die wordt gebruikt door de app in MIB's.
* **Aanvragen**
  * Totaal aantal aanvragen, ongeacht hun resulterende HTTP-statuscode.

Voor een **App Service-plan**, de beschikbare metrische gegevens zijn:

> [!NOTE]
> App Service-plan metrische gegevens zijn alleen beschikbaar voor abonnementen in **Basic**, **Standard**, en **Premium** lagen.
> 
> 

* **CPU-Percentage**
  * De gemiddelde CPU-gebruik over alle exemplaren van het abonnement.
* **Geheugenpercentage**
  * De gemiddelde hoeveelheid geheugen die wordt gebruikt voor alle instanties van het plan.
* **Gegevens In**
  * De gemiddelde binnenkomende bandbreedte die wordt gebruikt voor alle instanties van het plan.
* **Uitgaande gegevens**
  * De gemiddelde uitgaande bandbreedte die wordt gebruikt voor alle instanties van het plan.
* **Wachtrijlengte voor schijf**
  * Het gemiddelde aantal zowel lezen en schrijven van aanvragen die in de wachtrij zijn geplaatst op opslag. Een hoge wachtrijlengte is een indicatie van een toepassing die mogelijk vanwege overmatige schijf-i/o vertragen.
* **Lengte van de HTTP-wachtrij**
  * Het gemiddelde aantal HTTP-aanvragen die zitten in de wachtrij staan moesten voordat het wordt voldaan. Een hoog of toenemende HTTP-wachtrijlengte is een symptoom van een plan onder zware belasting.

### <a name="cpu-time-vs-cpu-percentage"></a>Vs CPU-percentage van CPU-tijd
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Er zijn twee metrische gegevens die overeenkomen met het CPU-gebruik. **CPU-tijd** en **CPU-percentage**

**CPU-tijd** is handig voor apps die worden gehost **gratis** of **gedeelde** omdat een van de quota is gedefinieerd in CPU-minuten die worden gebruikt door de app-abonnementen.

**CPU-percentage** is handig voor apps die worden gehost **basic**, **standard**, en **premium** plannen omdat ze kunnen worden uitgebreid. CPU-percentage is een goede indicatie van het totale gebruik voor alle instanties.

## <a name="metrics-granularity-and-retention-policy"></a>Metrische gegevens granulariteit en beleid voor Gegevensretentie
Metrische gegevens voor een toepassing en de app service-plan worden vastgelegd en samengevoegd door de service met de volgende granulaties en voor het bewaren van:

* **Minuut** granulariteit metrische gegevens worden bewaard voor **30 uur**
* **Uur** granulariteit metrische gegevens worden bewaard voor **30 dagen**
* **Dag** granulariteit metrische gegevens worden bewaard voor **30 dagen**

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Quota's en metrische gegevens bewaken in Azure portal.
U kunt de status van de verschillende bekijken **quota** en **metrische gegevens** die betrekking hebben op een toepassing in de [Azure-portal](https://portal.azure.com).

![][quotas]
**Quota** kunt u vinden onder Instellingen >**quota**. De UX kunt u bekijken: (1) de naam van de quota, (2) het interval opnieuw instellen, (3) de huidige limiet en (4) de huidige waarde.

![][metrics]
**Metrische gegevens** zijn toegankelijk vanaf de resourcepagina. U kunt ook de grafiek door aanpassen: (1) **klikt u op** op en selecteert u (2) **grafiek bewerken**.
Hier kunt u de (3) **tijdsbereik**, (4) **grafiektype**, en (5) **metrische gegevens** om weer te geven.  

U kunt meer informatie over deze metrische gegevens: [metrische servicegegevens bewaken](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Waarschuwingen en voor automatisch schalen
Metrische gegevens voor een App of een App Service-plan kan worden aangesloten op waarschuwingen. Zie voor meer informatie over het [meldingen van waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-alerts-portal.md).

App Service-apps die worden gehost in basic, standard of premium App Service-abonnementen ondersteuning **voor automatisch schalen**. Automatisch schalen kunt u regels configureren die de App Service-plan metrische gegevens controleren. Regels kunnen vergroten of verkleinen van het aantal instanties waarbij aanvullende resources naar behoefte. Regels kunt u geld besparen wanneer de toepassing te veel is ingericht. U kunt meer informatie over automatisch schalen hier: [voor schaling](../monitoring-and-diagnostics/insights-how-to-scale.md) en hier [aanbevolen procedures voor automatisch schalen van Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](https://azure.microsoft.com/try/app-service/). Hier kunt u direct een tijdelijke web-app maken in App Service. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.
> 
> 

[fzilla]:https://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:https://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
