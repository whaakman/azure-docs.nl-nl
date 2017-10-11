---
title: Meldingen instellen in Azure Application Insights | Microsoft Docs
description: Blijf op de hoogte over trage responstijden, uitzonderingen, en andere prestaties of gebruik wijzigingen in uw web-app.
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: f8ebde72-f819-4ba5-afa2-31dbd49509a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: c8386ffc5d68260eeb75edf7efb77db1163dcef8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="set-alerts-in-application-insights"></a>Meldingen instellen in Application Insights
[Azure Application Insights] [ start] kunt u een waarschuwing op wijzigingen in de prestaties of gebruik metrische gegevens in uw web-app. 

Application Insights wordt uw live app bewaakt op een [groot aantal verschillende platformen] [ platforms] bij het vaststellen van prestatieproblemen en gebruikspatronen te begrijpen.

Er zijn drie soorten waarschuwingen:

* **Metrische waarschuwingen** laat u weten wanneer een metriek een drempelwaarde overschrijden gedurende een bepaalde - zoals reactietijden, aantal uitzonderingen is, CPU-gebruik of paginaweergaven. 
* [**Webtests** ] [ availability] zien wanneer de site niet beschikbaar op het internet of reageert traag is. [Meer informatie][availability].
* [**Proactieve diagnoses** ](app-insights-proactive-diagnostics.md) worden automatisch geconfigureerd om u te waarschuwen over ongebruikelijke prestatiepatronen.

We zich richten op metrische waarschuwingen in dit artikel.

## <a name="set-a-metric-alert"></a>Een metriek waarschuwing instellen
Open de blade met regels voor waarschuwingen en gebruik vervolgens de knop toevoegen. 

![In de blade waarschuwingsregels kiezen waarschuwing toevoegen. Uw app instellen als de bron wilt meten, Geef een naam op voor de waarschuwing en een waarde kiezen.](./media/app-insights-alerts/01-set-metric.png)

* De bron voor de overige eigenschappen instellen. **Kies de resource '(onderdelen)'** als u wilt waarschuwingen instellen voor prestaties of gebruik metrische gegevens.
* De naam die u aan de waarschuwing toewijst moet uniek zijn binnen de resourcegroep (niet alleen uw toepassing).
* Wees voorzichtig te weten de eenheden waarin u wordt gevraagd om in te voeren van de drempelwaarde.
* Als u het selectievakje '... e-eigenaars', worden waarschuwingen verzonden via e-mail wilt maken voor iedereen die toegang tot deze resourcegroep heeft. Als deze reeks mensen wilt uitbreiden, voeg deze toe aan de [resourcegroep of abonnement](app-insights-resources-roles-access-control.md) (niet de bron).
* Als u 'Extra e-mailberichten' opgeeft, worden waarschuwingen verzonden naar deze personen of groepen (of u het e-eigenaars...' selectievakje aangevinkt). 
* Stel een [webhook adres](../monitoring-and-diagnostics/insights-webhooks-alerts.md) als u een web-app die op waarschuwingen reageert hebt ingesteld. Deze wordt aangeroepen wanneer de waarschuwing is geactiveerd en wanneer het probleem is opgelost. (Maar houd er rekening mee dat op dit moment, query-parameters niet als webhookeigenschappen doorgegeven.)
* U kunt uitschakelen of de waarschuwing in te schakelen: Zie de knoppen aan de bovenkant van de blade.

*Ik ziet de waarschuwing toevoegen knop niet.* 

* Gebruikt u een organisatie-account? Als u de eigenaar of bijdrager toegang tot de bron van deze toepassing hebt, kunt u waarschuwingen instellen. Bekijk de Access Control-blade. [Meer informatie over toegangsbeheer][roles].

> [!NOTE]
> In de blade waarschuwingen u ziet dat er al een waarschuwing instellen: [proactieve diagnoses](app-insights-proactive-failure-diagnostics.md). De automatische waarschuwing controleert een bepaalde meetwaarde, percentage mislukte. Tenzij u besluit de proactieve waarschuwing uitschakelen, hoeft u niet instellen van uw eigen waarschuwing voor Faalpercentage van de aanvraag. 
> 
> 

## <a name="see-your-alerts"></a>Uw waarschuwingen weergegeven
Krijgt u een e-mailbericht wanneer de status van een waarschuwing wijzigt tussen inactieve en actief. 

De huidige status van elke waarschuwing wordt weergegeven in de regels voor waarschuwingen-blade.

Er is een overzicht van recente activiteit in de waarschuwingen vervolgkeuzelijst:

![Waarschuwingen-omlaag](./media/app-insights-alerts/010-alert-drop.png)

De geschiedenis van statuswijzigingen is in het logboek voor activiteit:

![Klik op instellingen, controlelogboeken op de overzichtsblade.](./media/app-insights-alerts/09-alerts.png)

## <a name="how-alerts-work"></a>Hoe waarschuwingen werken
* Een waarschuwing heeft drie statussen: 'Nooit geactiveerd', 'Geactiveerd' en "Opgelost." Geactiveerde betekent dat de voorwaarde die u hebt opgegeven is waar, wanneer deze het laatst is geëvalueerd.
* Een melding wordt gegenereerd wanneer de status van een waarschuwing wordt gewijzigd. (Als voorwaarde voor de waarschuwing al true is wanneer u de waarschuwing hebt gemaakt, mogelijk niet krijgt u een melding totdat de voorwaarde onwaar gaat.)
* Elke melding genereert een e-mailadres als u het selectievakje e-mails aangevinkt of e-mailadressen opgegeven. U kunt ook zoeken op de vervolgkeuzelijst meldingen.
* Een waarschuwing wordt elke keer dat een metriek, maar niet anders binnenkomt geëvalueerd.
* De evaluatie van de metrische gegevens aggregeert gedurende de afgelopen periode en vergelijkt deze aan de drempelwaarde om de nieuwe status te bepalen.
* De periode die u kiest, geeft het interval op gedurende welke de metrische gegevens worden geaggregeerd. Heeft geen invloed op hoe vaak de waarschuwing wordt geëvalueerd: dat is afhankelijk van de frequentie van de aankomst van metrische gegevens.
* Als er geen gegevens zijn voor enige tijd voor een bepaalde metriek binnenkomt, heeft de afstand van de verschillende gevolgen voor de evaluatie van de waarschuwing en op de grafieken in metrische explorer. Als er geen gegevens langer dan de grafiek steekproefinterval wordt gezien geeft de grafiek in metrische explorer een waarde van 0. Maar een waarschuwing op basis van dezelfde metric is niet opnieuw geëvalueerd, en de status van de waarschuwing blijft ongewijzigd. 
  
    Wanneer gegevens uiteindelijk aankomen, wordt de grafiek gaat terug naar een andere waarde dan nul. De waarschuwing wordt geëvalueerd op basis van de gegevens beschikbaar voor de periode die u hebt opgegeven. Als het nieuwe gegevenspunt beschikbaar in de periode is, de statistische functie gebaseerd alleen op dat van gegevenspunt.
* Een waarschuwing kunt vaak knipperen tussen waarschuwingen en de goede status, zelfs als u een lange periode instelt. Dit kan gebeuren als de metrische waarde wordt bewogen rond de drempelwaarde. Er is geen hysteresis drempelwaarde: de overgang naar waarschuwing gebeurt op dezelfde waarde als de overgang op in orde.

## <a name="what-are-good-alerts-to-set"></a>Wat zijn goede waarschuwingen instellen?
Dit is afhankelijk van uw toepassing. Beginnen met, is het beter niet te veel metrische gegevens instellen. Besteed voldoende tijd kijken naar uw metrische grafieken terwijl uw app wordt uitgevoerd, om een idee krijgen hoe het werkt normaal. Hierdoor kunt u de prestaties verbeteren vinden. Vervolgens waarschuwingen instellen om te zien wanneer de metrische gegevens buiten de normale zone gaat. 

Populaire waarschuwingen zijn onder andere:

* [Browser metrische gegevens][client], met name Browser **pagina laadtijden**, geschikt zijn voor webtoepassingen. Als uw pagina veel scripts heeft, moet u controleren of **browseruitzonderingen**. Om deze metrische gegevens en waarschuwingen, die u hebt voor het instellen van [webpagina bewaking][client].
* **Serverreactietijd** voor de serverkant van webtoepassingen. En het instellen van waarschuwingen, Let op deze metrische gegevens om te zien als deze niet goed afhankelijk van hoog tarieven: variatie kan erop wijzen dat uw app wordt uitgevoerd onvoldoende resources. 
* **Serveruitzonderingen** : als u wilt zien, kunnen sommige [aanvullende instellingen](app-insights-asp-net-exceptions.md).

Vergeet niet dat [proactieve fout snelheid diagnostics](app-insights-proactive-failure-diagnostics.md) automatisch de snelheid waarmee uw app op aanvragen met fout codes reageert te bewaken. 

## <a name="automation"></a>Automatisering
* [PowerShell gebruiken voor het instellen van waarschuwingen automatiseren](app-insights-powershell-alerts.md)
* [Webhooks gebruiken om te reageren op waarschuwingen automatiseren](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="see-also"></a>Zie ook
* [Webtests voor beschikbaarheid](app-insights-monitor-web-app-availability.md)
* [Instellen van waarschuwingen automatiseren](app-insights-powershell-alerts.md)
* [Proactieve diagnostische gegevens](app-insights-proactive-diagnostics.md) 

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[platforms]: app-insights-platforms.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md

