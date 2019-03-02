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
ms.date: 01/23/2019
ms.author: mbullwin
ms.openlocfilehash: c1165fcebf8382d30b1be86f102da78ef0a4ac9a
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57244363"
---
# <a name="set-alerts-in-application-insights"></a>Waarschuwingen instellen in Application Insights
[Azure Application Insights] [ start] kunt u te waarschuwen voor wijzigingen in de prestaties of gebruik metrische gegevens in uw web-app. 

Application Insights bewaakt uw live app op een [groot aantal verschillende platformen] [ platforms] voor het vaststellen van prestatieproblemen en inzicht in gebruikspatronen.

Er zijn meerdere typen waarschuwingen:

* [**Metrische waarschuwingen** ](../../azure-monitor/platform/alerts-metric-overview.md) laat u weten wanneer een metriek een drempelwaarde voor een bepaalde - zoals reactietijd, aantal uitzonderingen, CPU-gebruik of paginaweergaven.
* [**Waarschuwingen voor activiteitenlogboeken** ](../../azure-monitor/platform/alerts-unified-log.md) wordt gebruikt om te beschrijven van waarschuwingen, waarbij de waarschuwing signaal is gebaseerd op een aangepaste Kusto-query.
* [**Webtests** ] [ availability] zien wanneer uw site beschikbaar is op het internet of reageert traag is. [Meer informatie][availability].
* [**Proactieve diagnostische gegevens** ](../../azure-monitor/app/proactive-diagnostics.md) worden automatisch geconfigureerd om u te waarschuwen over ongebruikelijke prestatiepatronen.

## <a name="set-a-metric-alert"></a>Instellen van een waarschuwing voor metrische gegevens
Open het tabblad regels voor waarschuwingen en gebruik vervolgens de knop toevoegen.

![Kies waarschuwing toevoegen op het tabblad regels voor waarschuwingen. Uw app als de bron om te meten, Geef een naam op voor de waarschuwing en kies een metrische waarde ingesteld.](./media/alerts/01-set-metric.png)

* De resource voordat de andere eigenschappen instellen. **Kies de resource "(onderdelen)"** als u wilt instellen van waarschuwingen over metrische gegevens voor prestaties of het gebruik.
* De naam die u aan de waarschuwing toewijst moet uniek zijn binnen de resourcegroep (niet alleen uw toepassing).
* Wees voorzichtig om te weten de eenheden waarin u wordt gevraagd om in te voeren van de drempelwaarde.
* Als u het selectievakje '... e-mail sturen naar eigenaars', worden waarschuwingen verzonden via e-mail naar iedereen die toegang tot deze resourcegroep heeft. Als u wilt deze set personen wilt uitbreiden, voeg deze toe aan de [resourcegroep of abonnement](../../azure-monitor/app/resources-roles-access-control.md) (niet de bron).
* Als u 'Extra e-mailadressen' opgeeft, worden meldingen verzonden naar deze personen of groepen (of u dit selectievakje inschakelt het vak 'e-eigenaren...'). 
* Stel een [webhook adres](../../azure-monitor/platform/alerts-webhooks.md) als u een web-app die op waarschuwingen reageert hebt ingesteld. Deze wordt aangeroepen wanneer de waarschuwing is geactiveerd en wanneer het probleem is opgelost. (Maar houd er rekening mee dat momenteel, queryparameters niet worden doorgegeven als webhookeigenschappen).
* U kunt uitschakelen of inschakelen van de waarschuwing: de knoppen aan de bovenkant.

*Ik zie niet de knop Waarschuwing toevoegen.*

* Gebruikt u een organisatie-account? Als u eigenaar of bijdrager toegang tot de bron van deze toepassing hebt, kunt u waarschuwingen instellen. Bekijk de Access Control-tabblad. [Meer informatie over toegangsbeheer][roles].

> [!NOTE]
> In de blade waarschuwingen ziet u dat er al een waarschuwing set up is: [Proactive Diagnostics](../../azure-monitor/app/proactive-failure-diagnostics.md). De automatische waarschuwing controleert een bepaalde metrische gegevens, fout snelheid van aanvragen. Tenzij u besluit de proactieve waarschuwing uitschakelen, moet u niet uw eigen melding instellen op de snelheid van aanvragen voor fout.
> 
> 

## <a name="see-your-alerts"></a>Uw waarschuwingen weergegeven
U krijgt een e-mail wanneer de status van een waarschuwing wijzigt tussen niet-actieve en actieve. 

De huidige status van elke waarschuwing wordt weergegeven in het tabblad regels voor waarschuwingen.

Er is een overzicht van recente activiteiten in de waarschuwingen vervolgkeuzelijst:

![Vervolgkeuzelijst voor waarschuwingen](./media/alerts/010-alert-drop.png)

De geschiedenis van statuswijzigingen is in het activiteitenlogboek:

![Klik op instellingen, auditlogboeken op het tabblad Overzicht.](./media/alerts/09-alerts.png)

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

## <a name="how-to-set-an-exception-alert-using-custom-log-search"></a>Over het instellen van een uitzonderingswaarschuwing met behulp van aangepaste zoekopdrachten in Logboeken

In deze sectie doorlopen we over het instellen van een waarschuwing voor een query op basis van uitzondering. Stel dat we willen een waarschuwing als de mislukte frequentie hoger is dan 10% in de afgelopen 24 uur voor dit voorbeeld.

1. Ga naar uw Application Insights-resource in Azure portal.
2. Aan de linkerkant, onder Klik op configureren **waarschuwing**.

    ![Aan de linkerkant onder configureren klikt u op waarschuwing](./media/alerts/1appinsightalert.png)

3. Selecteer aan de bovenkant van het tabblad waarschuwing **nieuwe waarschuwingsregel**.

     ![Aan de bovenkant van het tabblad waarschuwing klikt u op nieuwe waarschuwingsregel](./media/alerts/2createalert.png)

4. De resource moet worden automatisch geselecteerd. Als u wilt een voorwaarde instellen, klikt u op **voorwaarde toevoegen**.

    ![Klik op een voorwaarde toevoegen](./media/alerts/3addcondition.png)

5. Selecteer in het tabblad configureren signaal logische **aangepast zoeken in Logboeken**

    ![Klik op het aangepaste logboek zoeken](./media/alerts/4customlogsearch.png)

6. Voer de query in het vak 'Zoekquery' op het tabblad Zoeken met het aangepaste logboek. Voor dit voorbeeld gebruiken we de onderstaande Kusto-query.
    ```kusto
    let percentthreshold = 10;
    let period = 24h;
    requests
    | where timestamp >ago(period)
    | summarize requestsCount = sum(itemCount)
    | project requestsCount, exceptionsCount = toscalar(exceptions | where timestamp >ago(period) | summarize sum(itemCount))
    | extend exceptionsRate = toreal(exceptionsCount)/toreal(requestsCount) * 100
    | where exceptionsRate > percentthreshold

    ```

    ![Query van het type in het zoekvak van de query](./media/alerts/5searchquery.png)
    
    > [!NOTE]
    > U kunt deze stappen ook toepassen op andere typen waarschuwingen op basis van een query. U kunt meer informatie over de Kusto-query-language uit deze [Kusto-introductie doc](https://docs.microsoft.com/azure/kusto/concepts/) of [overzichtskaart van SQL tot Kusto](https://docs.microsoft.com/azure/kusto/query/sqlcheatsheet)

7. Kies onder 'Alert logic', of deze gebaseerd op het aantal resultaten of meting van metrische gegevens. Selecteer vervolgens de voorwaarde (groter is dan, gelijk aan, kleiner dan) en een drempelwaarde. Terwijl u deze waarden wijzigen wilt, merkt u wellicht de voorwaarde voorbeeld zin van wijzigingen. In dit voorbeeld gebruiken we 'gelijk aan'.

    ![Onder Alert logic kiezen uit de opties voor op basis van opgegeven en de voorwaarde, en typ vervolgens een drempelwaarde](./media/alerts/6alertlogic.png)

8. Onder 'Evaluated op basis van', stelt u de periode en de frequentie. De periode hier moet overeenkomen met de waarde die we voor periode in de bovenstaande query. Klik vervolgens op **gedaan**.

    ![Periode en frequentie instellen aan de onderkant en klik vervolgens op gereed](./media/alerts/7evaluate.png)

9. Nu ziet u de voorwaarde die we hebben gemaakt met de geschatte maandelijkse kosten. Hieronder onder ["Actiegroepen"](../platform/action-groups.md) kunt u een nieuwe groep maken of een bestaande set selecteren. Als u wilt, kunt u de acties kunt aanpassen.

    ![Klik op de selecteren of knoppen onder actiegroep maken](./media/alerts/8actiongroup.png)

10. Ten slotte de gegevens van uw waarschuwing toevoegen (naam, beschrijving, ernst melding). Wanneer u klaar bent, klikt u op **waarschuwingsregel maken** aan de onderkant.

    ![Onder Waarschuwingsdetails Typ uw naam waarschuwingsregel, Geef een beschrijving en kies een ernst ](./media/alerts/9alertdetails.png)

## <a name="who-receives-the-classic-alert-notifications"></a>Wie de (klassiek) waarschuwingsmeldingen ontvangen?

In deze sectie is alleen van toepassing op klassieke waarschuwingen en helpt u optimaliseren van uw meldingen van waarschuwingen om ervoor te zorgen dat alleen de gewenste geadresseerden meldingen ontvangen. Meer informatie geven over het verschil tussen [klassieke waarschuwingen](../platform/alerts-classic.overview.md) en de nieuwe waarschuwingen optreden, raadpleeg dan de [waarschuwingen overzichtsartikel](../platform/alerts-overview.md). Gebruiken voor het beheren van waarschuwingsmeldingen in de nieuwe ervaring voor waarschuwingen, [actiegroepen](../platform/action-groups.md).

* We raden het gebruik van specifieke ontvangers voor klassieke waarschuwingen.

* Voor waarschuwingen over een Application Insights-metrische gegevens (inclusief metrische gegevens over beschikbaarheid), de **bulksgewijs/groep** selectievakje, indien ingeschakeld, verzendt naar gebruikers met de rol van eigenaar, bijdrager of lezer in het abonnement. In feite _alle_ gebruikers met toegang tot het abonnement de Application Insights-resource in het bereik en meldingen ontvangt.

> [!NOTE]
> Als u momenteel gebruikmaakt van de **bulksgewijs/groep** selectievakje, en uitschakelen, kunt u zich niet meer herstellen van de wijziging.

Gebruik de nieuwe waarschuwing ervaring/bijna realtime waarschuwingen als u meldingen naar gebruikers op basis van hun rol nodig hebt. Met [actiegroepen](../platform/action-groups.md), kunt u e-mailmeldingen voor gebruikers configureren met een van de rollen Inzender of eigenaar/lezer is (niet gecombineerd samen als één optie).

## <a name="automation"></a>Automation
* [PowerShell gebruiken voor het instellen van waarschuwingen automatiseren](../../azure-monitor/app/powershell-alerts.md)
* [Webhooks gebruiken om te reageren op waarschuwingen automatiseren](../../azure-monitor/platform/alerts-webhooks.md)

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

