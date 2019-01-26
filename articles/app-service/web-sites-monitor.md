---
title: Apps - Azure App Service bewaken | Microsoft Docs
description: Informatie over het bewaken van apps in Azure App Service met behulp van de Azure-portal.
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
ms.date: 01/11/2019
ms.author: byvinyal
ms.custom: seodec18
ms.openlocfilehash: a5d4d13d8e60cd7f273363a9bc385098e15cbb71
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54913154"
---
# <a name="monitor-apps-in-azure-app-service"></a>Apps bewaken in Azure App Service
[Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) biedt ingebouwde bewaking functionaliteit voor web-apps, mobiele back-ends en API-apps in de [Azure-portal](https://portal.azure.com).

In de Azure-portal, kunt u bekijken *quota* en *metrische gegevens* voor een app, Controleer de App Service-plan en automatisch instellen *waarschuwingen* en *schalen* die zijn gebaseerd op de metrische gegevens.

## <a name="understand-quotas"></a>Quota's begrijpen

Apps die worden gehost in App Service zijn gebonden aan bepaalde limieten op de resources die kan worden gebruikt. De limieten worden gedefinieerd door de App Service-plan dat is gekoppeld aan de app.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Als de app wordt gehost een *gratis* of *gedeelde* -abonnement, de limieten voor de resources die de app kunt gebruiken door quota worden gedefinieerd.

Als de app wordt gehost een *Basic*, *Standard*, of *Premium* -abonnement, de limieten voor de resources die ze gebruiken kunnen worden ingesteld door de *grootte* () Klein, normaal, groot) en *aantal instanties* (1, 2, 3, enzovoort) van de App Service-plan.

Quota's voor gratis of gedeeld apps zijn:

| Quota | Description |
| --- | --- |
| **CPU (kort)** | De hoeveelheid CPU toegestaan voor deze app in een interval van 5 minuten. Dit quotum stelt om de vijf minuten. |
| **CPU (dag)** | De totale hoeveelheid CPU toegestaan voor deze app in een dag. Dit quotum stelt elke 24 uur plaats om middernacht UTC. |
| **Geheugen** | De totale hoeveelheid geheugen die is toegestaan voor deze app. |
| **Bandwidth** | De totale hoeveelheid uitgaande bandbreedte toegestaan voor deze app in een dag. Dit quotum stelt elke 24 uur plaats om middernacht UTC. |
| **Bestandssysteem** | De totale hoeveelheid opslag die zijn toegestaan. |

Het alleen van toepassing op apps die worden gehost in quotum *Basic*, *Standard*, en *Premium* plannen bestandssysteem is.

Zie voor meer informatie over de specifieke quota's, limieten en functies die beschikbaar zijn voor de verschillende App Service-SKU's [Servicelimieten van Azure-abonnement](../azure-subscription-service-limits.md#app-service-limits).

### <a name="quota-enforcement"></a>Het afdwingen van quota

Als een app is groter dan de *CPU (kort)*, *CPU (dag)*, of *bandbreedte* quotum, de app is gestopt, totdat het quotum wordt opnieuw ingesteld. Gedurende deze periode worden resulteert alle binnenkomende aanvragen in een HTTP 403-fout.

![bericht 403-fout][http403]

Als de app-geheugenquotum wordt overschreden, kan de app opnieuw wordt opgestart.

Als het bestandssysteem quotum wordt overschreden, schrijven een bewerking mislukt. Bewerkingsfouten bevatten alle schrijfbewerkingen naar de logboeken worden geschreven.

U kunt verhogen of quota verwijderen uit uw app door te upgraden van uw App Service-plan.

## <a name="understand-metrics"></a>Informatie over metrische gegevens

Metrische gegevens bevatten informatie over de app of de App-Service van het plan gedrag.

Voor een app zijn de beschikbare metrische gegevens:

| Gegevens | Description |
| --- | --- |
| **Gemiddelde reactietijd** | De gemiddelde tijd voor de app te verzenden van aanvragen, in milliseconden. |
| **Gemiddeld geheugenwerkset** | De gemiddelde hoeveelheid geheugen die wordt gebruikt door de app, in megabytes (MiB). |
| **Verbindingen** | Het aantal gebonden sockets bestaan in de sandbox (w3wp.exe en onderliggende processen).  Een afhankelijke socket wordt gemaakt door het aanroepen van bind()/connect() API's en blijft totdat deze socket wordt gesloten met CloseHandle()/closesocket(). |
| **CPU-tijd** | De hoeveelheid CPU verbruikt door de app in seconden. Zie voor meer informatie over deze metrische gegevens, [vs CPU-percentage van CPU-tijd](#cpu-time-vs-cpu-percentage). |
| **Huidige assembly 's** | Het huidige aantal assembly's geladen via alle AppDomains in deze toepassing. |
| **Data In** | De hoeveelheid binnenkomende bandbreedte verbruikt door de app, in MiB. |
| **Uitgaande gegevens** | De hoeveelheid uitgaande bandbreedte verbruikt door de app, in MiB. |
| **Garbagecollections van generatie 0** | Het aantal keren dat die de objecten generatie 0 garbagecollection zijn verzameld sinds de start van het app-proces. Hogere generatie GS bevatten alle GC voor lagere generatie.|
| **Garbagecollections van 1** | Het aantal keren dat die de generatie 1-objecten garbagecollection zijn verzameld sinds de start van het app-proces. Hogere generatie GS bevatten alle GC voor lagere generatie.|
| **Garbagecollections van generatie 2** | Het aantal keren dat die de objecten van de 2e generatie garbagecollection zijn verzameld sinds de start van het app-proces.|
| **Aantal ingangen** | Het totale aantal ingangen geopend door het app-proces.|
| **Http 2xx** | Het aantal aanvragen die HTTP-statuscode ≥ leidt tot 200 maar < 300. |
| **Http 3xx** | Het aantal aanvragen, wat resulteert in een HTTP-statuscode ≥ 300 maar < 400. |
| **HTTP 401** | Het aantal aanvragen, wat resulteert in een 401 HTTP-statuscode. |
| **Http 403** | Het aantal aanvragen dat leidt tot de statuscode HTTP 403. |
| **Http 404** | Het aantal aanvragen dat leidt tot HTTP 404-statuscode. |
| **Http 406** | Het aantal aanvragen dat leidt tot HTTP 406-statuscode. |
| **Http 4xx** | Het aantal aanvragen, wat resulteert in een HTTP-statuscode ≥ 400 maar < 500. |
| **HTTP-serverfouten** | Het aantal aanvragen die HTTP-statuscode ≥ leidt tot 500, maar < 600. |
| **I/o-overige Bytes Per seconde** | De snelheid waarmee de app-proces bytes uitgeeft aan i/o-bewerkingen die geen betrekking op gegevens, zoals beheerbewerkingen.|
| **I/o-overige bewerkingen Per seconde** | De snelheid waarmee de app-proces i/o-bewerkingen die niet worden gelezen of geschreven bewerkingen verleent.|
| **I/o-gelezen Bytes Per seconde** | De snelheid waarmee de app-proces bytes van i/o-bewerkingen leest.|
| **I/o-leesbewerkingen Per seconde** | De snelheid waarmee de app-proces lezen i/o-bewerkingen.|
| **I/O geschreven Bytes Per seconde** | De snelheid waarmee de app-proces bytes voor i/o-bewerkingen schrijven.|
| **I/o-schrijfbewerkingen Per seconde** | De snelheid waarmee de app-proces is i/o-bewerkingen voor schrijven.|
| **Geheugenwerkset** | De huidige hoeveelheid geheugen die wordt gebruikt door de app, in MiB. |
| **Private Bytes** | Eigen Bytes is de huidige grootte, in bytes, van het geheugen dat de app-proces heeft toegewezen en die niet kan worden gedeeld met andere processen.|
| **Aanvragen** | Het totale aantal aanvragen, ongeacht hun resulterende HTTP-statuscode. |
| **Aanvragen In de toepassingswachtrij** | Het aantal aanvragen in de wachtrij voor toepassingsaanvragen.|
| **Threadaantal** | Het aantal threads dat momenteel actief zijn in het app-proces.|
| **Totaal aantal Appdomeinen** | Het huidige aantal AppDomains geladen in deze toepassing.|
| **Totaal aantal Appdomeinen** | Het totale aantal AppDomains is verwijderd sinds de start van de toepassing.|


Voor een App Service-plan zijn de beschikbare metrische gegevens:

> [!NOTE]
> App Service-plan metrische gegevens zijn alleen beschikbaar voor abonnementen in *Basic*, *Standard*, en *Premium* lagen.
> 

| Gegevens | Description |
| --- | --- |
| **CPU Percentage** | De gemiddelde CPU-gebruik over alle exemplaren van het abonnement. |
| **Geheugenpercentage** | De gemiddelde hoeveelheid geheugen die wordt gebruikt voor alle instanties van het plan. |
| **Data In** | De gemiddelde binnenkomende bandbreedte die wordt gebruikt voor alle instanties van het plan. |
| **Uitgaande gegevens** | De gemiddelde uitgaande bandbreedte die wordt gebruikt voor alle instanties van het plan. |
| **Wachtrijlengte voor schijf** | Het gemiddelde aantal zowel lezen en schrijven van aanvragen die in de wachtrij zijn geplaatst op opslag. Een hoge wachtrijlengte is een indicatie van een app die mogelijk vanwege overmatige schijf-i/o vertragen. |
| **Lengte van de HTTP-wachtrij** | Het gemiddelde aantal HTTP-aanvragen die zitten in de wachtrij staan moesten voordat het wordt voldaan. Een hoog of toenemende HTTP-wachtrijlengte is een symptoom van een plan onder zware belasting. |

### <a name="cpu-time-vs-cpu-percentage"></a>Vs CPU-percentage van CPU-tijd
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Er zijn twee metrische gegevens die overeenkomen met het CPU-gebruik:

**CPU-tijd**: Dit is handig voor apps die worden gehost in gratis of abonnementen, gedeelde omdat een van de quota in CPU-minuten die worden gebruikt door de app is gedefinieerd.

**CPU-percentage**: Dit is handig voor apps die worden gehost in Basic, Standard en Premium-abonnementen, omdat ze kunnen worden uitgebreid. CPU-percentage is een goede indicatie van het totale gebruik voor alle instanties.

## <a name="metrics-granularity-and-retention-policy"></a>Beleid voor het granulariteit en retentie van metrische gegevens
Metrische gegevens voor een app en app service-plan worden vastgelegd en samengevoegd door de service, met de volgende granulaties en voor het bewaren van:

* **Minuut** granulariteit metrische gegevens worden bewaard gedurende 30 uur.
* **Uur** granulariteit metrische gegevens worden gedurende 30 dagen bewaard.
* **Dag** granulariteit metrische gegevens worden gedurende 30 dagen bewaard.

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Quota's en metrische gegevens in Azure portal controleren
Als u wilt controleren van de status van de verschillende quota's en metrische gegevens die invloed hebben op een app, gaat u naar de [Azure-portal](https://portal.azure.com).

![Diagram van de quota in Azure portal][quotas]

Selecteer quota vindt **instellingen** > **quota**. Op de grafiek, kunt u het volgende bekijken: 
1. De naam van het quotum.
1. Het interval voor het opnieuw.
1. De huidige limiet.
1. De huidige waarde.

![Grafiek met metrische gegevens in Azure portal][metrics] u hebt toegang tot metrische gegevens rechtstreeks vanuit de **Resource** pagina. De grafiek aanpassen: 
1. Selecteer de grafiek.
1. Selecteer **grafiek bewerken**.
1. Bewerk de **tijdsbereik**.
1. Bewerk de **grafiektype**.
1. Bewerk de metrische gegevens die u wilt weergeven.  

Zie voor meer informatie over metrische gegevens, [metrische servicegegevens bewaken](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Waarschuwingen en voor automatisch schalen
Metrische gegevens voor een app of een App Service-plan kan worden aangesloten op waarschuwingen. Zie [Meldingen van waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-alerts-portal.md) voor meer informatie.

App Service-apps die worden gehost in Basic, Standard of Premium App Service-abonnementen ondersteuning voor automatisch schalen. Met automatisch schalen, kunt u configureert regels waarmee de App Service-plan metrische gegevens controleren. Regels kunnen vergroten of verkleinen van het aantal instanties, die aanvullende resources bieden kan naar behoefte. Regels kunt u geld besparen wanneer de app te veel is ingericht.

Zie voor meer informatie over automatisch schalen, [schalen](../monitoring-and-diagnostics/insights-how-to-scale.md) en [aanbevolen procedures voor automatisch schalen van Azure Monitor](../azure-monitor/platform/autoscale-best-practices.md).

[fzilla]:https://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:https://go.microsoft.com/fwlink/?LinkID=309169

<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png