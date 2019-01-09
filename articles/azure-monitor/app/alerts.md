---
title: Stel waarschuwingen in Azure Application Insights | Microsoft Docs
description: Ontvang een melding over trage reactietijden, uitzonderingen en andere prestaties of gebruik wijzigingen in uw web-app.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.reviewer: lagayhar
ms.assetid: f8ebde72-f819-4ba5-afa2-31dbd49509a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: fe63813220d526536a31efb48b6ec29b6ae383e8
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54121138"
---
# <a name="set-alerts-in-application-insights"></a>Waarschuwingen instellen in Application Insights
[Azure Application Insights] [ start] kunt u te waarschuwen voor wijzigingen in de prestaties of gebruik metrische gegevens in uw web-app. 

Application Insights bewaakt uw live app op een [groot aantal verschillende platformen] [ platforms] voor het vaststellen van prestatieproblemen en inzicht in gebruikspatronen.

Er zijn drie soorten waarschuwingen:

* **Metrische waarschuwingen** laat u weten wanneer een metriek een drempelwaarde voor een bepaalde - zoals reactietijd, aantal uitzonderingen, CPU-gebruik of paginaweergaven. 
* [**Webtests** ] [ availability] zien wanneer uw site beschikbaar is op het internet of reageert traag is. [Meer informatie][availability].
* [**Proactieve diagnostische gegevens** ](../../azure-monitor/app/proactive-diagnostics.md) worden automatisch geconfigureerd om u te waarschuwen over ongebruikelijke prestatiepatronen.

We richten ons op metrische waarschuwingen in dit artikel.

## <a name="set-a-metric-alert"></a>Instellen van een waarschuwing voor metrische gegevens
Open de blade waarschuwingsregels en gebruik vervolgens de knop toevoegen. 

![In de blade waarschuwingsregels Kies waarschuwing toevoegen. Uw app als de bron om te meten, Geef een naam op voor de waarschuwing en kies een metrische waarde ingesteld.](./media/alerts/01-set-metric.png)

* De resource voordat de andere eigenschappen instellen. **Kies de resource "(onderdelen)"** als u wilt instellen van waarschuwingen over metrische gegevens voor prestaties of het gebruik.
* De naam die u aan de waarschuwing toewijst moet uniek zijn binnen de resourcegroep (niet alleen uw toepassing).
* Wees voorzichtig om te weten de eenheden waarin u wordt gevraagd om in te voeren van de drempelwaarde.
* Als u het selectievakje '... e-mail sturen naar eigenaars', worden waarschuwingen verzonden via e-mail naar iedereen die toegang tot deze resourcegroep heeft. Als u wilt deze set personen wilt uitbreiden, voeg deze toe aan de [resourcegroep of abonnement](../../azure-monitor/app/resources-roles-access-control.md) (niet de bron).
* Als u 'Extra e-mailadressen' opgeeft, worden meldingen verzonden naar deze personen of groepen (of u dit selectievakje inschakelt het vak 'e-eigenaren...'). 
* Stel een [webhook adres](../../azure-monitor/platform/alerts-webhooks.md) als u een web-app die op waarschuwingen reageert hebt ingesteld. Deze wordt aangeroepen wanneer de waarschuwing is geactiveerd en wanneer het probleem is opgelost. (Maar houd er rekening mee dat momenteel, queryparameters niet worden doorgegeven als webhookeigenschappen).
* U kunt uitschakelen of inschakelen van de waarschuwing: de knoppen aan de bovenkant van de blade zien.

*Ik zie niet de knop Waarschuwing toevoegen.* 

* Gebruikt u een organisatie-account? Als u eigenaar of bijdrager toegang tot de bron van deze toepassing hebt, kunt u waarschuwingen instellen. Bekijk de blade toegangsbeheer. [Meer informatie over toegangsbeheer][roles].

> [!NOTE]
> In de blade waarschuwingen ziet u dat er al een waarschuwing set up is: [Proactive Diagnostics](../../azure-monitor/app/proactive-failure-diagnostics.md). De automatische waarschuwing controleert een bepaalde metrische gegevens, fout snelheid van aanvragen. Tenzij u besluit de proactieve waarschuwing uitschakelen, moet u niet uw eigen melding instellen op de snelheid van aanvragen voor fout. 
> 
> 

## <a name="see-your-alerts"></a>Uw waarschuwingen weergegeven
U krijgt een e-mail wanneer de status van een waarschuwing wijzigt tussen niet-actieve en actieve. 

De huidige status van elke waarschuwing wordt weergegeven in de regels voor waarschuwingen-blade.

Er is een overzicht van recente activiteiten in de waarschuwingen vervolgkeuzelijst:

![Vervolgkeuzelijst voor waarschuwingen](./media/alerts/010-alert-drop.png)

De geschiedenis van statuswijzigingen is in het activiteitenlogboek:

![Op de blade overzicht klikt u op de logboeken voor controle-instellingen](./media/alerts/09-alerts.png)

## <a name="how-alerts-work"></a>Hoe waarschuwingen werken
* Een waarschuwing heeft drie statussen: 'Nooit geactiveerd', "Geactiveerd" en "Opgelost." Geactiveerde betekent dat de voorwaarde die u hebt opgegeven is waar, wanneer deze het laatst is geëvalueerd.
* Een melding wordt gegenereerd wanneer de status van een waarschuwing wordt gewijzigd. (Als de voorwaarde voor de waarschuwing is al waar bij het maken van de waarschuwing, mogelijk niet ontvangt u een melding totdat de voorwaarde onwaar is.)
* Elke melding genereert een e-mailbericht als u dit selectievakje is ingeschakeld in het vak e-mailberichten of e-mailadressen verstrekt. U kunt ook zoeken op de vervolgkeuzelijst met meldingen.
* Een waarschuwing wordt telkens wanneer een metrische waarde, maar niet verder bereikt worden geëvalueerd.
* De evaluatie van de metrische gegevens aggregeert in de voorafgaande periode en vergelijkt deze aan de drempelwaarde om de nieuwe status te bepalen.
* De periode die u kiest, geeft het interval op waarover de metrische gegevens worden samengevoegd. Dit heeft geen invloed op hoe vaak de waarschuwing wordt geëvalueerd: die afhankelijk is van de frequentie van binnenkomst van metrische gegevens.
* Als er worden geen gegevens voor een bepaalde metrische gegevens gedurende een bepaalde periode ontvangt, heeft de kloof verschillende gevolgen voor de evaluatie van de waarschuwing en op de grafieken in metric explorer. Wanneer er geen gegevens worden gedetecteerd voor langer zijn dan de controle-interval van de grafiek, wordt de grafiek in metric explorer een waarde van 0. Maar een waarschuwing op basis van de dezelfde meetwaarde is niet opnieuw worden geëvalueerd, en de status van de waarschuwing blijft ongewijzigd. 
  
    Wanneer gegevens uiteindelijk aankomen, wordt de grafiek gaat terug naar een andere waarde dan nul. De waarschuwing wordt geëvalueerd op basis van de gegevens beschikbaar zijn voor de periode die u hebt opgegeven. Als het nieuwe gegevenspunt beschikbaar in de periode is, is de statistische functie alleen gebaseerd op dat van gegevenspunt.
* Een waarschuwing kan vaak knipperen tussen de statussen van waarschuwingen en in orde is, zelfs als u een lange periode instelt. Dit kan gebeuren als de metrische waarde wordt bewogen over de drempelwaarde. Er is geen hysteresis drempelwaarde: de overgang naar waarschuwing vindt plaats op dezelfde waarde als de overgang op in orde.

## <a name="what-are-good-alerts-to-set"></a>Wat zijn goede waarschuwingen om in te stellen?
Dat hangt ervan af op uw toepassing. Beginnen met, is het beste niet te veel metrische gegevens instellen. Laten we zien hoe uw grafieken met metrische gegevens bekijkt terwijl uw app wordt uitgevoerd, als u wilt een idee krijgen van hoe deze gedraagt zich weer normaal. Met deze procedure kunt u vinden manieren om de prestaties te verbeteren. Stel waarschuwingen in om aan te geven wanneer de metrische gegevens buiten de normale zone gaat. 

Populaire waarschuwingen zijn onder andere:

* [Metrische gegevens over browser][client], met name Browser **pagina laadtijden**, zijn geschikt voor webtoepassingen. Als uw pagina veel scripts heeft, moet u controleren of **browseruitzonderingen**. Om deze metrische gegevens en waarschuwingen, die u hebt voor het instellen van [webpagina bewaking][client].
* **Serverreactietijd** voor de serverkant van webtoepassingen. En het instellen van waarschuwingen, te controleren op deze metrische gegevens om te zien als dit niet goed met hoge aanvraagsnelheden varieert: variant kan erop wijzen dat uw app wordt uitgevoerd uit middelen te halen. 
* **Serveruitzonderingen** : als u wilt zien, hebt u enkele [aanvullende instellingen](../../azure-monitor/app/asp-net-exceptions.md).

Vergeet niet dat [snelheid van proactieve foutdiagnoses](../../azure-monitor/app/proactive-failure-diagnostics.md) automatisch de snelheid waarmee uw app op aanvragen met foutcodes reageert bewaken. 

## <a name="automation"></a>Automation
* [PowerShell gebruiken voor het instellen van waarschuwingen automatiseren](../../azure-monitor/app/powershell-alerts.md)
* [Webhooks gebruiken om te reageren op waarschuwingen automatiseren](../../azure-monitor/platform/alerts-webhooks.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="see-also"></a>Zie ook
* [Webtests voor beschikbaarheid](../../azure-monitor/app/monitor-web-app-availability.md)
* [Instellen van waarschuwingen automatiseren](../../azure-monitor/app/powershell-alerts.md)
* [Proactieve diagnostische gegevens](../../azure-monitor/app/proactive-diagnostics.md) 

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[client]: ../../azure-monitor/app/javascript.md
[platforms]: ../../azure-monitor/app/platforms.md
[roles]: ../../azure-monitor/app/resources-roles-access-control.md
[start]: ../../azure-monitor/app/app-insights-overview.md

