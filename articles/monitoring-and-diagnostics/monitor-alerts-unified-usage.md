---
title: Maken, weergeven en beheren met behulp van Azure-Monitor van waarschuwingen
description: Gebruik de nieuwe waarschuwingen van geïntegreerde Azure-ervaring aan auteur, weergeven en beheren van metrische gegevens en meld u waarschuwingsregels op één plek.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/05/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 9ade63c07003ead55e63093ae74e876589ec8d61
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287834"
---
# <a name="create-view-and-manage-alerts-using-azure-monitor"></a>Maken, weergeven en beheren van waarschuwingen aan met een Azure-Monitor  

## <a name="overview"></a>Overzicht
In dit artikel leest u hoe waarschuwingen met behulp van de nieuwe gebruikersinterface van de waarschuwingen in de Azure-portal instellen. Er is een definitie van een waarschuwingsregel in drie delen:
- Doel: Specifieke Azure-resource, die moeten worden bewaakt
- De criteria: Bepaalde voorwaarde of logica die wanneer gezien in signaal, moeten worden geactiveerd door actie
- Actie: Specifieke aanroep naar een ontvanger van een melding verzonden - e-mail, SMS, webhook enzovoort.

Waarschuwingen van Azure biedt ook een uniform overzicht op al uw regels voor waarschuwingen en de mogelijkheid voor het beheren van één plaats; inclusief eventuele niet-opgeloste waarschuwingen weergeven. Meer informatie over de functionaliteit van [waarschuwingen van Azure - overzicht](monitoring-overview-unified-alerts.md).

Waarschuwing wordt de term **logboek waarschuwingen** te beschrijven waar signaal aangepaste query op basis is van waarschuwingen [logboekanalyse](../log-analytics/log-analytics-tutorial-viewdata.md) of [Application Insights](../application-insights/app-insights-analytics.md). De [nieuwe metrische waarschuwing mogelijkheid](monitoring-near-real-time-metric-alerts.md) biedt de mogelijkheid op waarschuwing op [multidimensionale metrische gegevens](monitoring-metric-charts.md) voor specifieke Azure-resources. De waarschuwingen voor deze bron kunt meer filters op dimensies maken **multidimensionale metrische gegevens waarschuwingen**.


> [!NOTE]
> Tijdens het waarschuwingen van Azure biedt een nieuwe en verbeterde ervaring voor het maken van waarschuwingen in Azure. De bestaande [waarschuwingen (klassiek)](monitoring-overview-alerts.md) ervaring bruikbaar blijft.
>

Gedetailleerde volgende is stapsgewijze handleiding voor het gebruik van waarschuwingen van Azure.

## <a name="create-an-alert-rule-with-the-azure-portal"></a>Een waarschuwingsregel maken met de Azure Portal
1. In de [portal](https://portal.azure.com/), selecteer **Monitor** en kies onder de sectie MONITOR - **waarschuwingen**.  
    ![Controle](./media/monitor-alerts-unified/AlertsPreviewMenu.png)

2. Selecteer de **nieuwe waarschuwingsregel** knop een nieuwe waarschuwing maken in Azure.
    ![Waarschuwing toevoegen](./media/monitor-alerts-unified/AlertsPreviewOption.png)

3. De sectie maken waarschuwing wordt weergegeven met de drie onderdelen die bestaan uit: *definiëren meldingsvoorwaarde*, *definiëren Waarschuwingsdetails*, en *definiëren actiegroep*.

    ![Regel maken](./media/monitor-alerts-unified/AlertsPreviewAdd.png)

4.  De meldingsvoorwaarde definiëren met behulp van de **Resource selecteren** koppeling en het doel op te geven door het selecteren van een resource. Filteren op het kiezen van de *-abonnement, * brontype en ten slotte selecteren vereist *Resource*.

    >[!NOTE]

    > Controleer of de signalen die beschikbaar zijn voor de geselecteerde resource voordat u doorgaat.

    ![Resource selecteren](./media/monitor-alerts-unified/Alert-SelectResource.png)

 De gebruikersinterface, kunt u verschillende soorten waarschuwingen maken vanaf één locatie. Op basis van het type waarschuwing gewenst kiest u de volgende stap:

    - Voor **metriek waarschuwingen**: Volg de stappen 5 tot en met 7; gaat u verder met stap 11
    - Voor **logboek waarschuwingen**, Ga naar stap 8.

    > [!NOTE]

    > Activiteit logboek waarschuwingen worden ook ondersteund, maar zijn Preview-versie. [Meer informatie](monitoring-activity-log-alerts-new-experience.md).

5. * Metrische waarschuwingen: Zorg ervoor dat **brontype** is geselecteerd als signaaltype als **metriek**, klikt u vervolgens één keer juiste **resource** gekozen Klik *gedaan*terug te keren naar waarschuwing maken. Vervolgens gebruikt de **criteria toevoegen** knop kiezen die het signaal specifieke lijst met opties signaal, hun monitoring-service en het type vermeld - die beschikbaar zijn voor de resource die eerder hebt geselecteerd.

    ![Een resource selecteren](./media/monitor-alerts-unified/AlertsPreviewResourceSelection.png)

    > [!NOTE]

    >  Alle [near-realtime waarschuwingen](monitoring-near-real-time-metric-alerts.md) geschikte bronnen worden weergegeven met monitor-service **Platform** en aan te geven type als **metrische gegevens**

6. *Metrische waarschuwingen*: nadat u signaal hebt geselecteerd, logica voor waarschuwingen kan worden vermeld. Ter referentie: historische gegevens van signaal wordt weergegeven met de optie voor het aanpassen van de tijd venster via **geschiedenis weergeven**, variëren van afgelopen zes uur voor vorige week. Met de visualisatie aanwezig is, **waarschuwing logica** kan worden geselecteerd uit de weergegeven opties van de voorwaarde, aggregatie en ten slotte drempelwaarde. Als voorbeeld van de logica die is opgegeven, wordt de voorwaarde weergegeven in de visualisatie samen met de geschiedenis van een signaal om aan te geven wanneer de waarschuwing zou hebben geactiveerd. Ten slotte opgeven voor welke tijd, waarschuwing moet worden gezocht naar de opgegeven voorwaarde door te kiezen uit de **periode** optie samen met hoe vaak waarschuwing moet worden uitgevoerd door het selecteren van **frequentie**.

    ![Signaal logica voor metriek configureren](./media/monitor-alerts-unified/AlertsPreviewCriteria.png)

7. *Metrische waarschuwingen*: signaal als een metriek, wordt de waarschuwing venster kan worden gefilterd met meerdere gegevenspunten of dimensies voor de genoemde Azure-resource. 

    a. Kies een duur van de **geschiedenis weergeven** vervolgkeuzelijst voor het visualiseren van een andere periode. U kunt ervoor kiezen de afmetingen van de ondersteunde metrische gegevens te filteren op een tijdreeks; dimensies kiezen is optioneel en maximaal vijf dimensies kunnen worden gebruikt. 

    b. **Waarschuwing logica** kan worden geselecteerd uit de weergegeven opties van *voorwaarde*, * aggregatie en *drempelwaarde*. Als voorbeeld van de logica die is opgegeven, wordt de voorwaarde weergegeven in de visualisatie samen met de geschiedenis van een signaal om aan te geven wanneer de waarschuwing zou worden geactiveerd in het verleden. 

    c. Als de tijdsduur opgeven, kiest u **periode** hoe vaak samen met waarschuwing moet worden uitgevoerd door het selecteren van **frequentie**.

    ![Signaal logica voor multidimensionale metrische gegevens configureren](./media/monitor-alerts-unified/AlertsPreviewCriteriaMultiDim.png)

8. *Meld u waarschuwingen*: Zorg ervoor dat **brontype** is een bron analytics zoals *logboekanalyse* of *Application Insights* en aan te geven type als **logboek** , klikt u vervolgens één keer juiste **resource** is gekozen, klikt u op *gedaan*. Vervolgens gebruikt de **criteria toevoegen** om de lijst met signaal opties die beschikbaar zijn voor de bron- en uit de lijst signaal **aangepaste logboek zoekactie** optie voor het gekozen Meld monitor-service, zoals *logboek Analytics* of *Application Insights*.

   ![Selecteer een resource - het aangepaste logboek zoeken](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]

   > Waarschuwingen lijsten kunnen importeren analytics query als signaaltype - **logboek (opgeslagen Query)**, zoals in bovenstaande afbeelding. Zodat gebruikers kunnen de query in Analytics perfect en deze vervolgens opslaan voor toekomstig gebruik in waarschuwingen - meer informatie over het gebruik van opgeslagen query die beschikbaar zijn op [met logboek zoeken in logboekanalyse](../log-analytics/log-analytics-log-searches.md) of [gedeelde query's in application insights Analytics](../log-analytics/log-analytics-overview.md). 

9.  *Meld u waarschuwingen*: eenmaal hebt geselecteerd, query voor waarschuwingen kan worden vermeld in de **zoekquery** veld; als de querysyntaxis is onjuist het veld fout in rood weergegeven. Als de querysyntaxis juist - ter referentie wordt historische gegevens van de opgegeven query weergegeven als een grafiek met de optie voor het aanpassen van het tijdvenster van afgelopen zes uur voor vorige week.

 ![Waarschuwingsregel configureren](./media/monitor-alerts-unified/AlertsPreviewAlertLog.png)

 > [!NOTE]

    > Visualisatie van historische gegevens kan alleen worden weergegeven als de queryresultaten tijd details hebben. Als uw query in het samengevatte gegevens of specifieke kolomwaarden resulteert - is dezelfde als enkelvoud tekent weergegeven.

    >  Metriek maateenheid van logboek waarschuwingen met behulp van Application insights, kunt u opgeven welke specifieke variabele als u wilt de gegevens groeperen met behulp van de **cumulatieve op** optie; als de hieronder weergegeven:

    ![cumulatieve optie](./media/monitor-alerts-unified/aggregate-on.png)

10.  *Meld u waarschuwingen*: met de visualisatie aanwezig is, **waarschuwing logica** kan worden geselecteerd uit de weergegeven opties van de voorwaarde, aggregatie en ten slotte drempelwaarde. Ten slotte opgeven in de logica de tijd voor het evalueren van de opgegeven voorwaarde met **periode** optie. Samen met hoe vaak waarschuwing moet worden uitgevoerd door het selecteren van **frequentie**.
Voor **logboek waarschuwingen** waarschuwingen kunnen worden gebaseerd op:
   - *Aantal Records*: een waarschuwing wordt gemaakt als het aantal records dat wordt geretourneerd door de query is groter dan of kleiner zijn dan de opgegeven waarde.
   - *Metrische meting*: een waarschuwing wordt gemaakt als elke *cumulatieve waarde* overschrijdt de drempelwaarde die is opgegeven in de resultaten en is *gegroepeerd op* gekozen waarde. Het aantal schendingen van een waarschuwing is het aantal keren dat de drempelwaarde is overschreden in de opgegeven tijdsperiode. Totaal aantal schendingen voor elke combinatie van schendingen kunt u opgeven via de resultatenset of opeenvolgende schendingen om te vereisen dat de schendingen in opeenvolgende steekproeven plaatsvinden moeten. Meer informatie over [logboek waarschuwingen en de typen](monitor-alerts-unified-log.md).

    > [!TIP]
    > Op dit moment in waarschuwingen - logboek zoekwaarschuwingen kunnen nemen aangepaste *periode* en *frequentie* waarde in minuten. Waarden kunnen variëren van 5 minuten en 1440 minuten (dat wil) 24 uur. Dus als u zeggen drie uur-waarschuwing outperiode wilt, converteert u deze in minuten - 180 minuten vóór gebruik

11. Als de tweede stap definieert u een naam voor de waarschuwing in de **waarschuwingsregel naam** veld samen met een **beschrijving** met gedetailleerde informatie over specificaties voor de waarschuwing en **ernst** waarde uit de Opties voor het opgegeven. Deze gegevens worden in alle e-mails met waarschuwingen, meldingen of push gedaan door Azure Monitor hergebruikt. Bovendien kan de gebruiker kiezen onmiddellijk de waarschuwingsregel maken door met de knop op de juiste wijze geactiveerd **regel inschakelen tijdens het maken van** optie.

    Voor **logboek waarschuwingen** alleen aanvullende functionaliteit is beschikbaar in de details van waarschuwing:

    - **Waarschuwingen onderdrukken**: wanneer u de onderdrukking voor de waarschuwingsregel inschakelt, acties voor de regel voor een gedefinieerde tijdsduur na het maken van een nieuwe waarschuwing is uitgeschakeld. De regel is nog steeds uitgevoerd en waarschuwingen records maakt, mits de criteria wordt voldaan. Zodat u tijd het probleem te verhelpen zonder dubbele acties worden uitgevoerd.

        ![Waarschuwingen voor logboek waarschuwingen onderdrukken](./media/monitor-alerts-unified/AlertsPreviewSuppress.png)

        > [!TIP]
        > Geef een waarschuwing onderdrukken waarde die groter is dan de frequentie van de waarschuwing om ervoor te zorgen meldingen zijn gestopt zonder overlap

12. Als de derde stap, geef eventuele **actie groep** moet worden geactiveerd voor de waarschuwingsregel wanneer waarschuwing voorwaarde wordt voldaan. U kunt ervoor kiezen een bestaande groep actie met waarschuwing of maak een nieuwe actie-groep. Volgens geselecteerd actie groep wanneer de melding wordt trigger Azure: email(s) verzenden, SMS(s) verzenden, Webhook(s) aanroepen, herstellen met behulp van Runbooks in Azure, te pushen naar uw ITSM hulpprogramma, enzovoort. Meer informatie over [actiegroepen](monitoring-action-groups.md).

    Voor **logboek waarschuwingen** aanvullende functionaliteit is beschikbaar voor de standaardacties overschrijven:

    - **E-mailmeldingen**: overschrijft *e-mailonderwerp* in het e-mailbericht verzonden via de actie groep; als een of meer e-acties in de groep met deze actie bestaat. U kunt de hoofdtekst van het e-mailbericht niet wijzigen en dit veld is **niet** voor e-mailadres.
    - **Aangepaste Json-nettolading opnemen**: de webhook JSON die wordt gebruikt door de actiegroepen; onderdrukt als een of meer webhookacties aanwezig zijn in de groep met deze actie. Gebruiker kan de indeling van JSON moet worden gebruikt voor alle webhooks die zijn geconfigureerd in de bijbehorende actie groep; opgeven Zie voor meer informatie over webhook indelingen [webhook-actie voor het logboek waarschuwingen](monitor-alerts-unified-log-webhook.md). Test Webhook-optie is opgegeven voor de indeling en verwerking door bestemming JSON-voorbeeld controleren en deze optie als u met het label bedoeld alleen voor **testen** doeleinden.

        ![Actie overschrijvingen voor logboek-waarschuwingen](./media/monitor-alerts-unified/AlertsPreviewOverrideLog.png)

        > [!NOTE]
        > Voor **Test Webhook** optie om te werken, moet ondersteuning voor het eindpunt [Cross-Origin Resource delen (CORS)](https://www.w3.org/TR/cors/) en van de gebruiker kunt CORS proxy gebruiken om te omzeilen 'Er is geen header met de Access Control-toestaan-oorsprong' problemen

13. Als alle velden geldig zijn en met groen maatstreepjes de **waarschuwingsregel maken** knop kan worden geklikt en wordt een waarschuwing gemaakt in Azure Monitor - waarschuwingen. Alle waarschuwingen kunnen worden weergegeven van het Dashboard-waarschuwingen.

    ![Maken van regels](./media/monitor-alerts-unified/AlertsPreviewCreate.png)

    Binnen een paar minuten de waarschuwing is actief en wordt geactiveerd als eerder beschreven.

## <a name="view-your-alerts-in-azure-portal"></a>Uw waarschuwingen weergeven in Azure Portal

1. In de [portal](https://portal.azure.com/), selecteer **Monitor** en kies onder de sectie MONITOR - **waarschuwingen**.  

2. De **waarschuwingen Dashboard** wordt weergegeven - waarin alle Azure-waarschuwingen zijn unified en weergegeven in een enkelvoud board ![waarschuwing Dashboard](./media/monitoring-alerts-unified-usage/alerts-preview-overview.png)
3. Van links naar rechts boven ziet het Dashboard u in één oogopslag de volgende - protocollen voor een gedetailleerd overzicht kan worden geklikt:
    - *Deze gebeurtenis wordt gestart waarschuwingen*: het aantal waarschuwingen op dit moment hebt voldaan aan de logica en in staat wordt gestart
    - *Totaal aantal waarschuwingsregels*: het aantal waarschuwingsregels gemaakt en in subtekst, het getal dat zijn momenteel ingeschakeld 
    

        > [!NOTE]
        > Om ervoor te zorgen consistent dashboard met informatie over alle gestarte waarschuwingen, met inbegrip van logboek-waarschuwingen voor application insights en logboekanalyse; [Uitgebreid unified waarschuwing (preview)](monitoring-overview-unified-alerts.md#enhanced-unified-alerts-public-preview) moet worden gebruikt
  
  
4. Een lijst met alle gestarte waarschuwingen weergegeven die de gebruiker kan klikken om details te bekijken
5. Tot in zoeken naar specifieke waarschuwingen; een de dropdown-opties op de voorgrond kunt gebruiken voor het filteren van specifieke *abonnement, resourcegroep en/of Resource*. Verdere voor een niet-omgezette waarschuwing, gebruikt de *Filter waarschuwing* optie vinden voor het opgegeven trefwoord - specifieke overeenkomende waarschuwingen met *, waarschuwing Criteria, Resource-groep en doelbron*

## <a name="managing-your-alerts-in-azure-portal"></a>Het beheren van uw waarschuwingen in Azure Portal
1. In de [portal](https://portal.azure.com/), selecteer **Monitor** en kies onder de sectie MONITOR - **waarschuwingen**.  
2. Selecteer de **regels beheren** knop op de bovenste balk, om te navigeren naar de sectie regel beheer - waarin alle waarschuwingsregels gemaakt worden weergegeven, met inbegrip van waarschuwingen die zijn uitgeschakeld.
3. Als u wilt zoeken voor specifieke regels voor waarschuwingen, een kan gebruikmaken van de vervolgkeuzelijst filters op de voorgrond die lijst echtheidskenmerken waarschuwingsregels voor specifieke *abonnement, resourcegroepen en/of Resource*. U kunt ook op met de zoekfunctie deelvenster boven de waarschuwingsregel lijst gemarkeerd *waarschuwingen filteren*, een sleutelwoord waarmee wordt vergeleken met kan bieden *de naam van de waarschuwing, voorwaarde en doelbron*; alleen weergeven overeenkomende regels.
   ![Waarschuwing regels beheren](./media/monitoring-alerts-unified-usage/alerts-preview-rules.png)
4. Als u wilt bekijken of wijzigen van specifieke waarschuwingsregel, klik op de naam die wordt weergegeven als een link.
5. In de fasestructuur van de drie van gedefinieerd waarschuwing wordt weergegeven -: 1) waarschuwing voorwaarde 2) waarschuwing Detail 3) de actie groep. **Doel Criteria** kan worden geklikt om te wijzigen van de waarschuwing logica of een nieuwe criteria kunnen worden toegevoegd als u het pictogram met de eerdere logica te verwijderen. Op deze manier in Waarschuwingsdetails sectie - **beschrijving** en **ernst** kan worden gewijzigd. En de groep acties kunnen worden gewijzigd of een nieuwe kan worden ontworpen om te koppelen met de waarschuwing via de **nieuwe actiegroep** knop.

   ![Waarschuwingsregel wijzigen](./media/monitor-alerts-unified/AlertsPreviewEdit.png)

6. In het bovenste deelvenster wijzigingen in de waarschuwing kunnen worden met inbegrip van reflecteert: **opslaan** doorvoeren van wijzigingen gedaan op waarschuwing, **negeren** om terug te gaan zonder het doorvoeren van wijzigingen in de waarschuwing, **uitschakelen**  voor het deactiveren van de waarschuwing - waarna het niet meer wordt uitgevoerd of geen actie wordt geactiveerd. En tot slot, **verwijderen** de hele waarschuwingsregel permanent verwijderen uit Azure.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de nieuwe [near-realtime metrische waarschuwingen](monitoring-near-real-time-metric-alerts.md)
- Ophalen van een [overzicht van metrische gegevens verzameling](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.
- Meer informatie over [waarschuwingen melden waarschuwingen van Azure](monitor-alerts-unified-log.md)
- [Meer informatie over activiteit logboek waarschuwingen in de gebruikerservaring van waarschuwingen (Preview)](monitoring-activity-log-alerts-new-experience.md)
