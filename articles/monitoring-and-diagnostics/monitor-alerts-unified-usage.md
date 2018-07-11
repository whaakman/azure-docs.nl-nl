---
title: Maken, weergeven en beheren met behulp van Azure Monitor een waarschuwing
description: Gebruik de nieuwe waarschuwingen van geïntegreerde Azure-ervaring te ontwerpen, weergeven en beheren van metrische gegevens en meld u waarschuwingsregels vanaf één locatie.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/05/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: a913075c051c6b784495917b7edbd7340254a212
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952627"
---
# <a name="create-view-and-manage-alerts-using-azure-monitor"></a>Maken, weergeven en beheren van waarschuwingen via Azure Monitor  

## <a name="overview"></a>Overzicht
Dit artikel leest u hoe het instellen van waarschuwingen via de nieuwe interface voor waarschuwingen in Azure portal. Er is een definitie van een waarschuwingsregel in drie delen:
- Doel: Specifieke Azure-resource, die moet worden bewaakt
- De criteria: Bepaalde voorwaarde of logische dat wanneer in signaal zien, moet actie activeren
- Actie: Specifieke aanroep verzonden naar een ontvanger van een melding - e-mail, SMS, webhook, enzovoort.

Waarschuwingen van Azure biedt ook een centrale weergave zodat u al uw waarschuwingsregels en de mogelijkheid voor het beheren van één plek; inclusief het weergeven van alle niet-opgeloste waarschuwingen. Meer informatie over de functionaliteit van [waarschuwingen van Azure - overzicht](monitoring-overview-unified-alerts.md).

Waarschuwing wordt de term gebruikt **Logboekwaarschuwingen** om te beschrijven van waarschuwingen met waar signaal aangepaste query op basis van [Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) of [Application Insights](../application-insights/app-insights-analytics.md). De [nieuwe metrische waarschuwing mogelijkheid](monitoring-near-real-time-metric-alerts.md) biedt de mogelijkheid op waarschuwing op [multidimensionale metrische gegevens](monitoring-metric-charts.md) voor specifieke Azure-resources. De waarschuwingen voor deze resource meer filters kunnen gebruiken voor het maken van dimensies **multidimensionale metrische waarschuwingen**.


> [!NOTE]
> Bij waarschuwingen van Azure biedt een nieuwe en verbeterde ervaring voor het maken van waarschuwingen in Azure. De bestaande [waarschuwingen (klassiek)](monitoring-overview-alerts.md) ervaring bruikbaar blijft.
>

Gedetailleerde volgende is een stapsgewijze handleiding voor het gebruik van Azure-waarschuwingen.

## <a name="create-an-alert-rule-with-the-azure-portal"></a>Een waarschuwingsregel maken met de Azure-Portal
1. In de [portal](https://portal.azure.com/), selecteer **Monitor** en kies onder de sectie MONITOR - **waarschuwingen**.  
    ![Controle](./media/monitor-alerts-unified/AlertsPreviewMenu.png)

2. Selecteer de **nieuwe waarschuwingsregel** knop een nieuwe waarschuwing maken in Azure.
    ![Waarschuwing toevoegen](./media/monitor-alerts-unified/AlertsPreviewOption.png)

3. De sectie waarschuwingen maken wordt weergegeven met de drie onderdelen die bestaan uit: *waarschuwingsvoorwaarde definiëren*, *Waarschuwingsdetails definiëren*, en *actiegroep definiëren*.

    ![Regel maken](./media/monitor-alerts-unified/AlertsPreviewAdd.png)

4.  De waarschuwingsvoorwaarde definiëren met behulp van de **Resource selecteren** koppeling en het doel, door het selecteren van een resource op te geven. Filteren op het kiezen van de *-abonnement, * resourcetype en ten slotte selecteren vereist *Resource*.

    >[!NOTE]

    > Controleer of de signalen die beschikbaar zijn voor de geselecteerde resource voordat u doorgaat.

    ![Resource selecteren](./media/monitor-alerts-unified/Alert-SelectResource.png)

 De gebruikersinterface kunt u verschillende typen waarschuwingen maken vanaf één locatie. Op basis van het type waarschuwing gewenst kiest u de volgende stap als:

    - Voor **metrische waarschuwingen**: Volg de stappen 5 tot en met 7; Ga rechtstreeks naar stap 11
    - Voor **Logboekwaarschuwingen**, gaat u naar stap 8.

    > [!NOTE]

    > Waarschuwingen voor activiteitenlogboeken worden ook ondersteund, maar zijn beschikbaar als preview. [Meer informatie](monitoring-activity-log-alerts-new-experience.md).

5. *Metrische waarschuwingen*: Zorg ervoor dat **resourcetype** is ingeschakeld met signaaltype als **metrische gegevens**, klikt u vervolgens één keer juiste **resource** wordt gekozen klikt u op  *Gedaan* terug te keren naar het maken van waarschuwingen. Vervolgens gebruikt de **criteria toevoegen** knop om te kiezen om het specifieke signaal uit lijst met opties signaal, hun monitoring-service en het type vermeld - die beschikbaar zijn voor de resource die eerder hebt geselecteerd.

    ![Een resource selecteren](./media/monitor-alerts-unified/AlertsPreviewResourceSelection.png)

    > [!NOTE]

    >  Alle [bijna in realtime waarschuwingen](monitoring-near-real-time-metric-alerts.md) kunnen resources worden weergegeven met monitor-service **Platform** en aan te geven als **metrische gegevens**

6. *Metrische waarschuwingen*: nadat signaal is geselecteerd, logica voor waarschuwingen kan worden vermeld. Ter referentie, historische gegevens van het signaal wordt weergegeven met de optie voor het aanpassen van het venster met behulp **geschiedenis weergeven**, variërende van afgelopen zes uur voor de afgelopen week. Met de visualisatie aanwezig is, **Alert Logic** kunnen worden geselecteerd in de weergegeven opties van de voorwaarde, aggregatie en ten slotte drempelwaarde. Als Preview-versie van de logica die is opgegeven, wordt de voorwaarde in de visualisatie, samen met signaal geschiedenis weergegeven om aan te geven wanneer de waarschuwing zou hebben geactiveerd. Ten slotte opgeven gedurende welke tijd, waarschuwing moet worden gezocht naar de opgegeven voorwaarde door te kiezen uit de **periode** optie samen met de hoe vaak waarschuwing moet worden uitgevoerd door het selecteren van **frequentie**.

    ![Signaallogica voor metrische gegevens configureren](./media/monitor-alerts-unified/AlertsPreviewCriteria.png)

7. *Metrische waarschuwingen*: als signaal is een metrische waarde, wordt de waarschuwing venster kan worden gefilterd met behulp van meerdere gegevenspunten of dimensies voor de genoemde Azure-resource. 

    a. Kies een duur van de **geschiedenis weergeven** vervolgkeuzelijst voor het visualiseren van een andere periode. U kunt ervoor kiezen de afmetingen van de ondersteunde metrische gegevens om te filteren op een tijdreeks; het kiezen van dimensies is optioneel en maximaal vijf dimensies kunnen worden gebruikt. 

    b. **Waarschuwing logische** kunnen worden geselecteerd in de weergegeven opties van *voorwaarde*, * aggregatie, en *drempelwaarde*. Als Preview-versie van de logica die is opgegeven, wordt de voorwaarde in de visualisatie, samen met signaal geschiedenis weergegeven om aan te geven wanneer de waarschuwing zou worden geactiveerd in het verleden. 

    c. Als u wilt opgeven hoe lang, kies **periode** hoe vaak samen met waarschuwing moet worden uitgevoerd door het selecteren van **frequentie**.

    ![Signaallogica voor multi-dimensionale metrische gegevens configureren](./media/monitor-alerts-unified/AlertsPreviewCriteriaMultiDim.png)

8. *Waarschuwingen voor activiteitenlogboeken*: Zorg ervoor dat **resourcetype** is een analytics-bron, zoals *Log Analytics* of *Application Insights* en aan te geven als **Log** , klikt u vervolgens één keer juiste **resource** is gekozen, klikt u op *gedaan*. Vervolgens gebruikt de **criteria toevoegen** om de lijst weergeven van signaal opties die beschikbaar zijn voor de resource en uit de lijst signaal **zoeken in Logboeken aangepaste** optie voor de gekozen monitor-service, zoals melden *Log Analytics* of *Application Insights*.

   ![Selecteer een resource - aangepaste zoekopdrachten in Logboeken](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]

   > Waarschuwingen lijsten kunnen importeren analytics-query als signaaltype - **logboek (opgeslagen Query)**, zoals weergegeven in bovenstaande afbeelding. Gebruikers kunnen verbeteren van de query in Analytics en deze vervolgens opslaan voor toekomstig gebruik in waarschuwingen: meer informatie over het gebruik van opgeslagen query die beschikbaar zijn op [met zoeken in Logboeken in log analytics](../log-analytics/log-analytics-log-searches.md) of [gedeelde query in application insights Analytics](../log-analytics/log-analytics-overview.md). 

9.  *Waarschuwingen voor activiteitenlogboeken*: eenmaal hebt geselecteerd, query voor waarschuwingen kan worden vermeld in de **zoekquery** veld; als de query-syntaxis is onjuist fout wordt in het veld in het rood weergegeven. Als de querysyntaxis juist - ter referentie wordt historische gegevens van de opgegeven query weergegeven als een grafiek met de optie voor het aanpassen van het tijdvenster van afgelopen zes uur voor de afgelopen week.

 ![Waarschuwingsregel configureren](./media/monitor-alerts-unified/AlertsPreviewAlertLog.png)

 > [!NOTE]

    > Historische gegevensvisualisatie kan alleen worden weergegeven als de queryresultaten details van de tijd hebt. Als uw query in waarden van de specifieke kolom- of samengevatte gegevens resulteert is hetzelfde als een enkelvoudige diagram weergegeven.

    >  Het type van de meting van metrische gegevens van waarschuwingen met Application insights, kunt u opgeven welke specifieke variabele als u wilt de gegevens groeperen met behulp van de **cumulatieve op** optie; als de hieronder weergegeven:

    ![statistische optie](./media/monitor-alerts-unified/aggregate-on.png)

10.  *Waarschuwingen voor activiteitenlogboeken*: met de visualisatie aanwezig is, **Alert Logic** kunnen worden geselecteerd in de weergegeven opties van de voorwaarde, aggregatie en ten slotte drempelwaarde. Ten slotte opgeven in de logica, de tijd om te beoordelen voor de opgegeven voorwaarde, met behulp van **periode** optie. Samen met hoe vaak waarschuwing moet worden uitgevoerd door het selecteren van **frequentie**.
Voor **Logboekwaarschuwingen** waarschuwingen kunnen worden gebaseerd op:
   - *Aantal Records*: een waarschuwing gemaakt als het aantal records dat wordt geretourneerd door de query is groter dan of kleiner is dan de waarde die is opgegeven.
   - *Meting van metrische gegevens*: een waarschuwing gemaakt als elke *statistische waarde* overschrijdt de drempelwaarde die is opgegeven in de resultaten en is *gegroepeerd op* gekozen waarde. Het aantal schendingen voor een waarschuwing is het aantal keren dat die de drempelwaarde is overschreden in de geselecteerde tijdsperiode. Totaal aantal schendingen voor elke combinatie van schendingen kunt u opgeven via de resultatenset of achtereenvolgende schendingen om te vereisen dat de schendingen in opeenvolgende steekproeven plaatsvinden moeten. Meer informatie over [waarschuwingen en het bijhorende type](monitor-alerts-unified-log.md).

    > [!TIP]
    > Op dit moment in waarschuwingen - waarschuwingen voor zoeken in Logboeken kunnen duren voordat aangepaste *periode* en *frequentie* waarde in minuten. Waarden kunnen variëren van vijf minuten en 1440 minuten (dat wil zeggen) 24 uur. Dus als u wilt dat waarschuwingen periode bijvoorbeeld drie uur, zet u het in minuten: 180 minuten, voor gebruik

11. Als de tweede stap definieert u een naam voor de waarschuwing in de **naam waarschuwingsregel** veld samen met een **beschrijving** met gedetailleerde informatie over specificaties voor de waarschuwing en **ernst** waarde uit de opties opgegeven. Deze gegevens opnieuw worden gebruikt in alle e-mails met waarschuwingen, meldingen of push uitgevoerd door Azure Monitor. Bovendien gebruiker kan kiezen bij het maken van waarschuwingsregel onmiddellijk geactiveerd door het omschakelen van op de juiste wijze **regel inschakelen bij het maken van** optie.

    Voor **Logboekwaarschuwingen** alleen aanvullende functionaliteit is beschikbaar in de details van waarschuwing:

    - **Waarschuwingen onderdrukken**: wanneer u onderdrukking voor de waarschuwingsregel inschakelen, acties voor de regel zijn uitgeschakeld voor een gedefinieerde tijdsduur na het maken van een nieuwe waarschuwing. De regel wordt nog uitgevoerd en waarschuwing records gemaakt, mits de criteria wordt voldaan. Zodat u tijd kunt dit probleem oplossen zonder dat dubbele acties worden uitgevoerd.

        ![Waarschuwingen onderdrukken voor Logboekwaarschuwingen](./media/monitor-alerts-unified/AlertsPreviewSuppress.png)

        > [!TIP]
        > Geef een onderdrukken waarschuwing die groter is dan de frequentie van de waarschuwing om ervoor te zorgen meldingen zonder overlap worden gestopt

12. Als de derde en laatste stap, geef eventuele **actiegroep** moet voor de waarschuwingsregel wordt geactiveerd wanneer waarschuwingen voorwaarde wordt voldaan. U kunt kiezen eventuele bestaande actiegroep met waarschuwing of een nieuwe actiegroep maken. Geselecteerd volgens actiegroep, wanneer de waarschuwing is wordt Azure trigger: email(s) verzenden, SMS(s) verzenden, aanroepen van SMS-bericht(en), herstellen met behulp van Azure-Runbooks, pushen naar uw ITSM-hulpprogramma, enzovoort. Meer informatie over [actiegroepen](monitoring-action-groups.md).

    Voor **Logboekwaarschuwingen** aanvullende functionaliteit is beschikbaar voor de standaardacties onderdrukken:

    - **E-mailmelding**: onderdrukt *e-mailonderwerp* in het e-mailbericht verzonden via actiegroep; als een of meer e-mailacties in de groep met deze actie bestaat. U kunt de hoofdtekst van het e-mailbericht niet wijzigen en dit veld is **niet** voor e-mailadres.
    - **Aangepaste Json-nettolading opnemen**: heeft voorrang op de JSON die wordt gebruikt door actiegroepen; van de webhook als een of meer webhookacties aanwezig zijn in de groep met deze actie. Gebruiker kan de indeling van JSON moet worden gebruikt voor alle webhooks die zijn geconfigureerd in de bijbehorende actie groep; opgeven Zie voor meer informatie over de opmaak van de webhook [webhookactie voor Logboekwaarschuwingen](monitor-alerts-unified-log-webhook.md). Test Webhook-optie om te controleren of de indeling en verwerking van bestemming met behulp van de voorbeeld-JSON die wordt geleverd en deze optie als u met het label is bedoeld voor **testen** doeleinden.

        ![Actie onderdrukkingen voor Logboekwaarschuwingen](./media/monitor-alerts-unified/AlertsPreviewOverrideLog.png)

        > [!NOTE]
        > Voor **Webhook testen** optie om te werken, moet ondersteuning bieden voor het eindpunt [Cross-Origin Resource Sharing (CORS)](https://www.w3.org/TR/cors/) en van de gebruiker kunt CORS-proxy gebruiken om op te halen om problemen met 'Geen Access-Control-Allow-Origin-header'

13. Als alle velden geldig zijn en met groene maatstreepjes de **maken van waarschuwingsregel** knop kan worden geklikt en wordt er een waarschuwing gemaakt in Azure Monitor - waarschuwingen. Alle waarschuwingen kunnen worden weergegeven van de Dashboard-waarschuwingen.

    ![Het maken van regels](./media/monitor-alerts-unified/AlertsPreviewCreate.png)

    Binnen een paar minuten, wordt de waarschuwing is actief en wordt geactiveerd als eerder beschreven.

## <a name="view-your-alerts-in-azure-portal"></a>Uw waarschuwingen weergeven in Azure Portal

1. In de [portal](https://portal.azure.com/), selecteer **Monitor** en kies onder de sectie MONITOR - **waarschuwingen**.  

2. De **waarschuwingen Dashboard** wordt weergegeven - waarin alle Azure-waarschuwingen zijn unified en weergegeven in een enkel bord ![van een Waarschuwingendashboard](./media/monitoring-alerts-unified-usage/alerts-preview-overview.png)
3. Van boven links naar rechts ziet u het Dashboard in één oogopslag de volgende - die kan worden geklikt om te zien in de gedetailleerde lijst:
    - *Geactiveerde waarschuwingen*: het aantal waarschuwingen op dit moment die logica voldaan en geactiveerd in status hebben
    - *Totaal aantal waarschuwingsregels*: het aantal waarschuwingsregels die zijn gemaakt en in uitgehaald moet worden, het getal dat op dit moment zijn ingeschakeld 
    

        > [!NOTE]
        > Om ervoor te zorgen consistent dashboard met informatie over alle geactiveerde waarschuwingen, met inbegrip van waarschuwingen voor application insights en log analytics; [Uitgebreid geïntegreerde waarschuwen (preview)](monitoring-overview-unified-alerts.md#enhanced-unified-alerts-public-preview) moet worden gebruikt
  
  
4. Een lijst met alle geactiveerde waarschuwingen weergegeven die de gebruiker kan klikken om details weer te geven
5. Tot in zoeken naar specifieke waarschuwingen; de opties van de vervolgkeuzelijst bovenaan kunt gebruiken voor het filteren van specifieke *abonnement, resourcegroep en/of Resource*. Verdere voor een niet-omgezette waarschuwen, gebruikt de *Filter waarschuwing* optie om te zoeken over het opgegeven trefwoord - specifieke overeenkomende waarschuwingen met *, waarschuwing Criteria, resourcegroep, en Doelresource*

## <a name="managing-your-alerts-in-azure-portal"></a>Uw waarschuwingen in Azure Portal beheren
1. In de [portal](https://portal.azure.com/), selecteer **Monitor** en kies onder de sectie MONITOR - **waarschuwingen**.  
2. Selecteer de **regels beheren** knop op de bovenste balk, om te navigeren naar de beheersectie van regel - waar alle waarschuwingsregels gemaakt worden weergegeven, met inbegrip van waarschuwingen die zijn uitgeschakeld.
3. Als u wilt zoeken voor specifieke regels voor waarschuwingen, een kunt gebruiken de filters vervolgkeuzelijst bovenaan, waarmee lijst echtheidskenmerken waarschuwingsregels voor specifieke *abonnementen, resourcegroepen en/of Resource*. U kunt ook over het gebruik van de zoekopdracht deelvenster boven de lijst van de waarschuwingsregel gemarkeerd *waarschuwingen filteren*, een krijgt u een sleutelwoord, die wordt vergeleken met *naam van waarschuwing, de voorwaarde en de Doelresource*; alleen weergeven overeenkomende regels.
   ![Waarschuwing-regels beheren](./media/monitoring-alerts-unified-usage/alerts-preview-rules.png)
4. Als u wilt weergeven of wijzigen van specifieke waarschuwingsregel, klikt u op de naam die wordt weergegeven als een aanklikbare koppeling.
5. In de fasestructuur van de drie van de waarschuwing is gedefinieerd wordt weergegeven -: (1) waarschuwing voorwaarde 2) waarschuwing Detailgroep-3) de actie. **Gericht op Criteria** kan worden geklikt voor het wijzigen van de waarschuwing logica of een nieuwe criteria kunnen worden toegevoegd nadat u het pictogram verwijderen van de eerdere logica. Op deze manier in Waarschuwingsdetails sectie - **beschrijving** en **ernst** kan worden gewijzigd. En de actiegroep kan worden gewijzigd of een nieuwe kan worden ontworpen om te koppelen aan de waarschuwing met de **nieuwe actiegroep** knop.

   ![Waarschuwingsregel wijzigen](./media/monitor-alerts-unified/AlertsPreviewEdit.png)

6. Boven in het venster, wijzigingen in de waarschuwing kunnen zijn bij reflectieve inclusief: **opslaan** om door te voeren van eventuele wijzigingen worden aangebracht aan de waarschuwing, **negeren** om terug te gaan zonder het doorvoeren van wijzigingen in de waarschuwing, **uitschakelen**  voor het deactiveren van de waarschuwing - waarna deze niet meer wordt uitgevoerd of wordt een actie gestart. En ten slotte **verwijderen** permanent verwijderen van de gehele waarschuwingsregel van Azure.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de nieuwe [bijna realtime metrische waarschuwingen](monitoring-near-real-time-metric-alerts.md)
- Krijgen een [overzicht van metrische gegevens verzameling](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.
- Meer informatie over [waarschuwingen voor activiteitenlogboeken in Azure-waarschuwingen](monitor-alerts-unified-log.md)
- [Meer informatie over waarschuwingen voor activiteitenlogboeken in de ervaring voor waarschuwingen (Preview)](monitoring-activity-log-alerts-new-experience.md)
