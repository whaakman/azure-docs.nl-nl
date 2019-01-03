---
title: Web-API's en REST API's maken voor Azure Logic Apps | Microsoft Docs
description: Web-API's en REST API's voor het aanroepen van uw API's, services of systemen van systeemintegraties in Azure Logic Apps maken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.topic: article
ms.assetid: bd229179-7199-4aab-bae0-1baf072c7659
ms.date: 05/26/2017
ms.openlocfilehash: 25b33242b9f7bddf0497067f111ca3fb4a1ea570
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53600712"
---
# <a name="create-custom-apis-you-can-call-from-azure-logic-apps"></a>Aangepaste API's die u vanuit Azure Logic Apps aanroepen kunt maken

Hoewel Azure Logic Apps biedt [meer dan 100 ingebouwde connectors](../connectors/apis-list.md) dat u kunt gebruiken in logic app workflows, wilt u mogelijk voor het aanroepen van API's, systemen en services die niet beschikbaar zijn als connectors. U kunt uw eigen API's waarmee u acties en triggers voor gebruik in logic apps kunt maken. Hier volgen andere redenen waarom u mogelijk wilt maken van uw eigen API's die u vanuit logic app workflows aanroepen kunt:

* Breid uw huidige systeem integratie en werkstromen voor gegevensintegratie.
* Help klanten uw service gebruiken voor het beheren van professionele of persoonlijke taken.
* Vouw in het bereik, zichtbaarheid en gebruik voor uw service.

In principe zijn connectors web-API's die gebruikmaken van REST voor pluggable interfaces [Swagger-metagegevensindeling](http://swagger.io/specification/) voor documentatie en JSON als de exchange-indeling van gegevens. Omdat de connectors zijn REST-API's die via HTTP-eindpunten communiceren, kunt u elke taal, zoals .NET, Java of Node.js, gebruiken voor het bouwen van connectors. U kunt ook uw API's hosten op [Azure App Service](../app-service/overview.md), een platform-as-a-service (PaaS) dat een van de beste, eenvoudigste en meest schaalbare manier biedt voor het hosten van de API. 

Aangepaste API's om te werken met logic apps, uw API kan bieden [ *acties* ](./logic-apps-overview.md#logic-app-concepts) die specifieke taken uitvoeren in logic app workflows. Uw API kan ook fungeren als een [ *trigger* ](./logic-apps-overview.md#logic-app-concepts) die een werkstroom voor logische Apps begint wanneer er nieuwe gegevens of een gebeurtenis aan een opgegeven voorwaarde voldoet. Dit onderwerp worden algemene patronen die u volgen kunt voor het bouwen van acties en triggers in uw API, op basis van het gedrag op dat u wilt dat uw API te geven.

U kunt uw API's hosten op [Azure App Service](../app-service/overview.md), een platform-as-a-serviceoplossing (PaaS) waarmee de uiterst schaalbare webhostingservice met eenvoudige API die als host fungeert.

> [!TIP] 
> Hoewel u uw API's als web-apps implementeren kunt, kunt u uw API's implementeren als API-apps die u van uw werk gemakkelijker maken kunnen bij het maken, hosten en gebruikmaken van API's in de cloud en on-premises. U hoeft niet te wijzigen van de code in uw API's--alleen implementeert uw code in een API-app. Bijvoorbeeld informatie over het bouwen van API-apps die zijn gemaakt met de volgende talen: 
> 
> * [ASP.NET](../app-service/app-service-web-get-started-dotnet.md). 
> * [Java](../app-service/app-service-web-get-started-java.md)
> * [Node.js](../app-service/app-service-web-get-started-nodejs.md)
> * [PHP](../app-service/app-service-web-get-started-php.md)
> * [Python](../app-service/containers/quickstart-python.md)
> * [Ruby](../app-service/containers/quickstart-ruby.md)
>
> Voor voorbeelden van API-App die is gebouwd voor logische apps, gaat u naar de [Azure Logic Apps-GitHub-opslagplaats](http://github.com/logicappsio) of [blog](https://aka.ms/logicappsblog).

## <a name="how-do-custom-apis-differ-from-custom-connectors"></a>Hoe verschillen aangepaste API's van aangepaste connectors?

Aangepaste API's en [aangepaste connectors](../logic-apps/custom-connector-overview.md) zijn web-API's die gebruikmaken van REST voor pluggable interfaces [Swagger-metagegevensindeling](http://swagger.io/specification/) voor documentatie en JSON als de exchange-indeling van gegevens. En omdat deze API's en connectors REST-API's die via HTTP-eindpunten communiceren zijn, kunt u elke taal, zoals .NET, Java of Node.js, voor het bouwen van aangepaste API's en connectors.

Aangepaste API's kunt u bij het aanroepen van API's die niet connectors zijn en eindpunten die u kunt aanroepen voorzien van HTTP + Swagger, Azure API Management of App-Services. Aangepaste connectors werken zoals aangepaste API's, maar hebt ook deze kenmerken:

* Als resources in Azure Logic Apps-Connector is geregistreerd.
* Met pictogrammen naast door Microsoft beheerde connectors in de ontwerper van logische Apps weergegeven.
* Alleen beschikbaar voor de connectors auteurs en gebruikers van de logische app met dezelfde Azure Active Directory-tenant en hetzelfde Azure-abonnement in de regio waar de logic apps zijn geïmplementeerd.

U kunt ook nomineer van geregistreerde connectors voor certificering door Microsoft. Dit proces wordt gecontroleerd of geregistreerde connectors voldoen aan de criteria voor openbaar gebruik en maakt deze connectors beschikbaar voor gebruikers in Microsoft Flow en Microsoft PowerApps.

Zie voor meer informatie over aangepaste connectors. 

* [Overzicht van aangepaste connectors](../logic-apps/custom-connector-overview.md)
* [Aangepaste connectors maken van Web-API 's](../logic-apps/custom-connector-build-web-api-app-tutorial.md)
* [Aangepaste connectors registreren in Azure Logic Apps](../logic-apps/logic-apps-custom-connector-register.md)

## <a name="helpful-tools"></a>Nuttige hulpmiddelen

Een aangepaste API werkt het beste met logic apps als de API ook is een [Swagger-document](http://swagger.io/specification/) met de beschrijving van de bewerkingen en parameters van de API.
Veel bibliotheken, zoals [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle), kan het Swagger-bestand automatisch genereren voor u. Als u wilt aantekeningen toevoegen aan het Swagger-bestand voor weergavenamen, eigenschaptypen, enzovoort, u kunt ook [TRex](https://github.com/nihaue/TRex) zodat uw Swagger-bestand goed met logic apps werkt.

<a name="actions"></a>

## <a name="action-patterns"></a>Actie-patronen

Voor logic apps taken uit te voeren, uw aangepaste API moet bieden [ *acties*](./logic-apps-overview.md#logic-app-concepts). Elke bewerking in uw API wordt toegewezen aan een actie. Een eenvoudige actie is een domeincontroller die accepteert HTTP-aanvragen en retourneert HTTP-antwoorden. Een logische app verzendt bijvoorbeeld een HTTP-aanvraag naar uw web-app of API-app. Uw app stuurt een HTTP-antwoord, samen met inhoud die de logische app kan worden verwerkt.

U kunt voor een standard actie, schrijft een HTTP-aanvraagmethode in uw API en beschrijven die methode in een Swagger-bestand. Vervolgens kunt u uw API rechtstreeks met aanroepen een [HTTP-actie](../connectors/connectors-native-http.md) of een [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) actie. Standaard antwoorden moeten worden teruggestuurd binnen de [aanvraag time-outlimiet](./logic-apps-limits-and-config.md). 

![Standaard actie-patroon](./media/logic-apps-create-api-app/standard-action.png)

<a name="pattern-overview"></a> Als u een logische app wacht terwijl uw API langer uitgevoerde taken zijn voltooid, uw API kunt volgen de [asynchrone polling patroon](#async-pattern) of de [asynchrone webhook patroon](#webhook-actions) in dit onderwerp beschreven. Voor een vergelijking die helpt u bij het visualiseren van deze patronen verschillend gedrag, stelt u zich het proces voor het bestellen van een aangepaste taart uit een bakkerij wordt gehost. Het polling-patroon komt overeen met het probleem waar u de bakkerij wordt gehost om de 20 minuten aanroepen om te controleren of de taart gereed is. De webhook-patroon komt overeen met het gedrag van waar de bakkerij u om uw telefoonnummer vraagt, zodat ze u bellen kunnen wanneer de taart klaar is.

Voor voorbeelden, gaat u naar de [Logic Apps-GitHub-opslagplaats](https://github.com/logicappsio). Ook meer informatie over [gebruiksmeting voor acties](logic-apps-pricing.md).

<a name="async-pattern"></a>

### <a name="perform-long-running-tasks-with-the-polling-action-pattern"></a>Uitvoeren van langlopende taken met het patroon van de actie polling

Om uw API taken uitvoeren die kunnen worden uitgevoerd langer zijn dan de [aanvraag time-outlimiet](./logic-apps-limits-and-config.md), kunt u het patroon asynchrone polling. Dit patroon heeft uw API wilt werken in een afzonderlijke thread, maar blijft een actieve verbinding met de Logic Apps-engine. Op die manier kunnen de logische app heeft geen time-out of doorgaan met de volgende stap in de werkstroom voordat uw API werken is voltooid.

Hier volgt het algemene patroon:

1. Zorg ervoor dat de engine weet dat uw API geaccepteerd van de aanvraag en werken aan de slag.
2. Wanneer de engine voor elke volgende keer dat de status van taak maakt, kunt u de engine weten wanneer de taak klaar is met uw API.
3. Terug naar de engine voor het relevante gegevens zodat de werkstroom voor logische Apps kunt blijven.

<a name="bakery-polling-action"></a> Nu de vorige overeenkomstige bakkerij wordt gehost in het polling-patroon van toepassing, en stel dat u een bakkerij wordt gehost en de bestelling een aangepaste taart voor de levering van aanroept. Het proces voor het maken van de taart kost tijd en u niet wilt wachten op de telefoon, terwijl de bakkerij wordt gehost op de taart werkt. De bakkerij wordt bevestigd dat uw order en heeft u elke 20 minuten voor de status van de taart aanroepen. Na 20 minuten worden gehaald, aanroepen van de bakkerij wordt gehost, maar ze u laten weten dat uw taart is niet uitgevoerd en dat u in een andere 20 minuten aanroepen moet. Dit proces en weer schakelen gaat door totdat u aanroept, en de bakkerij wordt aangegeven dat uw bestelling klaar is, en uw taart biedt. 

Laten we dit patroon polling terug worden toegewezen. De bakkerij geeft uw aangepaste API, terwijl u, de klant taart de Logic Apps-engine vertegenwoordigen. Wanneer de-engine de API met een aanvraag roept, wordt uw API wordt bevestigd dat de aanvraag en reageert met de tijdsinterval wanneer de engine taakstatus kunt controleren. De engine blijft de taakstatus van de te controleren totdat uw API reageert dat de taak is voltooid en retourneert gegevens naar uw logische app, die vervolgens werkstroom blijft. 

![Patroon voor polling-actie](./media/logic-apps-create-api-app/custom-api-async-action-pattern.png)

Dit zijn de specifieke stappen voor uw API te volgen, vanuit het perspectief van de API van beschreven:

1. Wanneer uw API een HTTP-aanvraag aan de slag haalt, onmiddellijk een HTTP terug `202 ACCEPTED` -antwoord met de `location` header verderop in deze stap wordt beschreven. Dit antwoord kunt de Logic Apps-engine weten dat uw API hebt u de aanvraag, aanvaard de nettolading van de aanvraag (gegevensinvoer), en nu wordt verwerkt. 
   
   De `202 ACCEPTED` antwoord moet deze headers bevatten:
   
   * *Vereiste*: Een `location` -header die Hiermee geeft u het absolute pad naar een URL waar de Logic Apps-engine de status van uw API's kan controleren

   * *Optionele*: Een `retry-after` -header het aantal seconden dat de engine wachten geeft voordat het controleren van moet de `location` URL voor de status van taak. 

     De engine controleert standaard elke 20 seconden. Als u een ander interval, bevatten de `retry-after` header en het aantal seconden tot de volgende poll.

2. Als de opgegeven tijd is verstreken, de Logic Apps-engine polls de `location` URL om te controleren of de status van taak. Uw API moet deze controles uitvoeren en deze antwoorden retourneren:
   
   * Als de taak is voltooid, retourneert een HTTP `200 OK` antwoord, samen met de nettolading van de reactie (invoer voor de volgende stap).

   * Als de taak nog steeds worden verwerkt, retourneren een andere HTTP `202 ACCEPTED` antwoord, maar met de dezelfde kopteksten, zoals het oorspronkelijke antwoord.

Als uw API dit patroon volgt, hebt u geen iets doet in de logische app werkstroomdefinitie om door te gaan met het controleren van de taakstatus. Wanneer de engine wordt een HTTP `202 ACCEPTED` antwoord en een geldig `location` koptekst, de engine rekening wordt gehouden met het asynchrone patroon en controleert de `location` header totdat uw API wordt een niet-202-antwoord geretourneerd.

> [!TIP]
> Voor een voorbeeld van de asynchrone patroon, bekijkt u deze [asynchrone controller antwoord voorbeeld in GitHub](https://github.com/logicappsio/LogicAppsAsyncResponseSample).

<a name="webhook-actions"></a>

### <a name="perform-long-running-tasks-with-the-webhook-action-pattern"></a>Uitvoeren van langlopende taken met het patroon van de actie webhook

Als alternatief kunt u de webhook-patroon voor langlopende taken en asynchrone verwerking. Dit patroon is de logische app onderbreken en wachten op een 'callback' van uw API om te verwerken voordat u doorgaat werkstroom voltooien. Deze aanroep is een HTTP POST waarmee een bericht naar een URL wordt verzonden wanneer een gebeurtenis plaatsvindt. 

<a name="bakery-webhook-action"></a> Nu toepassen van de vorige bakkerij wordt gehost vergelijking met het patroon webhook en stel dat u een bakkerij wordt gehost en de bestelling een aangepaste taart voor de levering van aanroept. Het proces voor het maken van de taart kost tijd en u niet wilt wachten op de telefoon, terwijl de bakkerij wordt gehost op de taart werkt. De bakkerij wordt bevestigd dat uw order, maar deze keer u ze geven uw telefoonnummer, zodat ze u bellen kunnen wanneer de taart is voltooid. Deze keer ziet de bakkerij u wanneer uw order is klaar en levert de taart.

Wanneer we dit patroon webhook opnieuw toewijst, vertegenwoordigt de bakkerij uw aangepaste API, terwijl u, de klant taart de Logic Apps-engine vertegenwoordigen. De engine voor uw API met een aanvraag wordt aangeroepen en een 'retouraanroep'-URL bevat.
Wanneer de taak is voltooid, de API maakt gebruik van de URL op de hoogte stellen de engine en stuur gegevens terug naar uw logische app, die vervolgens werkstroom blijft. 

Voor dit patroon worden twee eindpunten instellen op uw domeincontroller: `subscribe` en `unsubscribe`

*  `subscribe` Eindpunt: Wanneer de uitvoering van uw API-bewerking in de werkstroom is bereikt, de Logic Apps-engine aanroepen de `subscribe` eindpunt. Deze stap zorgt ervoor dat de logische app voor het maken van een URL voor terugbellen waarmee uw API worden opgeslagen en wacht totdat de callback van uw API wanneer werk voltooid is. Uw API en vervolgens weer-aanroepen met een HTTP POST naar de URL en wordt elke geretourneerde inhoud en -koppen doorgegeven als invoer voor de logische app.

* `unsubscribe` Eindpunt: Als de uitvoering van de logische app is geannuleerd, de Logic Apps-engine aanroepen de `unsubscribe` eindpunt. Uw API kan vervolgens de URL voor terugbellen voor de registratie ongedaan maken en beëindigen alle processen indien nodig.

![Webhook actie patroon](./media/logic-apps-create-api-app/custom-api-webhook-action-pattern.png)

> [!NOTE]
> Logic App Designer ondersteunt op dit moment niet detecteren webhook-eindpunten via Swagger. Dus voor dit patroon die u hebt om toe te voegen een [ **Webhook** actie](../connectors/connectors-native-webhook.md) en geef de URL, headers en hoofdtekst voor uw aanvraag. Zie ook [werkstroomacties en triggers](logic-apps-workflow-actions-triggers.md#apiconnection-webhook-action). Als u wilt doorgeven in de callback-URL, kunt u de `@listCallbackUrl()` Werkstroomfunctie in een van de vorige velden indien nodig.

> [!TIP]
> Voor een voorbeeld van de webhook-patroon, bekijkt u deze [webhook-trigger voorbeeld in GitHub](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

<a name="triggers"></a>

## <a name="trigger-patterns"></a>Trigger-patronen

Uw aangepaste API kan fungeren als een [ *trigger* ](./logic-apps-overview.md#logic-app-concepts) die een logische app wordt gestart wanneer nieuwe gegevens of een gebeurtenis aan een opgegeven voorwaarde voldoet. Deze trigger kunt regelmatig controleren of wacht en luisteren, nieuwe gegevens of gebeurtenissen op uw service-eindpunt. Als nieuwe gegevens of een gebeurtenis voldoet aan de opgegeven voorwaarde, wordt de trigger wordt geactiveerd en de logische app, die naar deze trigger luistert wordt gestart. Voor het starten van logische apps op deze manier, uw API kunt volgen de [ *polling trigger* ](#polling-triggers) of de [ *webhook-trigger* ](#webhook-triggers) patroon. Deze patronen zijn vergelijkbaar met hun tegenhangers voor [poll-acties](#async-pattern) en [webhookacties](#webhook-actions). Ook meer informatie over [gebruiksmeting voor triggers](logic-apps-pricing.md).

<a name="polling-triggers"></a>

### <a name="check-for-new-data-or-events-regularly-with-the-polling-trigger-pattern"></a>Controleren op nieuwe gegevens of gebeurtenissen regelmatig met het patroon van de trigger polling

Een *polling trigger* fungeert lijkt veel op de [polling-actie](#async-pattern) eerder in dit onderwerp wordt beschreven. De Logic Apps-engine roept regelmatig en controleert of het eindpunt van de trigger voor nieuwe gegevens of gebeurtenissen. Als de engine vindt de nieuwe gegevens of een gebeurtenis die aan de opgegeven voorwaarde voldoet, wordt de trigger wordt geactiveerd. Vervolgens maakt de engine een exemplaar van de logische app waarmee de gegevens als invoer wordt verwerkt. 

![Polling triggerpatroon](./media/logic-apps-create-api-app/custom-api-polling-trigger-pattern.png)

> [!NOTE]
> Elke aanvraag polling telt als een actie wordt uitgevoerd, zelfs als er geen logische app-exemplaar is gemaakt. Om te voorkomen dat dezelfde gegevens meerdere keren verwerken, moet de trigger opschonen van gegevens die al is gelezen en doorgegeven aan de logische app.

Hier vindt u specifieke stappen voor een polling-trigger, die vanuit het perspectief van de API worden beschreven:

| Nieuwe gegevens of gebeurtenis gevonden?  | API-reactie | 
| ------------------------- | ------------ |
| Gevonden | Retourneert een HTTP `200 OK` status met de nettolading van de reactie (invoer voor de volgende stap). <br/>Dit antwoord wordt gemaakt van een exemplaar van de logische app en de werkstroom wordt gestart. | 
| Niet gevonden | Retourneert een HTTP `202 ACCEPTED` status met een `location` koptekst en een `retry-after` header. <br/>Triggers, de `location` header mag ook een `triggerState` queryparameter, die meestal een "timestamp". Uw API kunt u deze id gebruiken voor het bijhouden van de laatste keer dat de logische app is geactiveerd. | 
||| 

Als u wilt regelmatig controleren op uw service voor nieuwe bestanden, kan u bijvoorbeeld een polling-trigger die deze problemen heeft bouwen:

| Verzoek bevat `triggerState`? | API-reactie | 
| -------------------------------- | -------------| 
| Nee | Retourneert een HTTP `202 ACCEPTED` status plus een `location` -header met `triggerState` ingesteld op de huidige tijd en de `retry-after` interval op 15 seconden. | 
| Ja | Controleer uw service voor bestanden die zijn toegevoegd na de `DateTime` voor `triggerState`. | 
||| 

| Aantal bestanden gevonden | API-reactie | 
| --------------------- | -------------| 
| Één bestand | Retourneert een HTTP `200 OK` status en de nettolading van de inhoud bijwerken `triggerState` naar de `DateTime` voor de geretourneerde bestands- en set `retry-after` interval op 15 seconden. | 
| Meerdere bestanden | Een bestand op een tijdstip en een HTTP-retourneren `200 OK` status, update `triggerState`, en stel de `retry-after` interval 0 seconden. </br>Volgende stappen uit om de engine weten dat er meer gegevens beschikbaar is en dat de engine onmiddellijk de gegevens moet aanvragen via de URL in de `location` header. | 
| Geen bestanden | Retourneert een HTTP `202 ACCEPTED` status, niet te wijzigen `triggerState`, en stel de `retry-after` interval op 15 seconden. | 
||| 

> [!TIP]
> Voor een voorbeeld polling-triggerpatroon, bekijkt u deze [poll trigger controller voorbeeld in GitHub](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/PollTriggerController.cs).

<a name="webhook-triggers"></a>

### <a name="wait-and-listen-for-new-data-or-events-with-the-webhook-trigger-pattern"></a>Wacht en luisteren naar nieuwe gegevens of gebeurtenissen met het patroon webhook-trigger

Een webhook-trigger is een *push-trigger* dat wordt gewacht en luistert naar nieuwe gegevens of gebeurtenissen op uw service-eindpunt. Als u voldoet aan de opgegeven voorwaarde nieuwe gegevens of een gebeurtenis, wordt de trigger wordt geactiveerd en maakt een logische app-exemplaar, die vervolgens de gegevens als invoer verwerkt.
Webhook-triggers fungeren lijkt veel op de [webhookacties](#webhook-actions) eerder in dit onderwerp wordt beschreven, en zijn ingesteld met `subscribe` en `unsubscribe` eindpunten. 

* `subscribe` Eindpunt: Wanneer u toevoegen en opslaan van een webhook-trigger in uw logische app, de Logic Apps-engine aanroepen de `subscribe` eindpunt. Deze stap zorgt ervoor dat de logische app te maken van een URL voor terugbellen waarmee uw API worden opgeslagen. Wanneer er nieuwe gegevens of een gebeurtenis die voldoet aan de opgegeven voorwaarde, uw API-aanroepen weer met een HTTP POST naar de URL. De nettolading van inhoud en -koppen doorgeven als invoer voor de logische app.

* `unsubscribe` Eindpunt: Als de webhook-trigger of een hele logische app wordt verwijderd, de Logic Apps-engine aanroepen de `unsubscribe` eindpunt. Uw API kan vervolgens de URL voor terugbellen voor de registratie ongedaan maken en beëindigen alle processen indien nodig.

![Webhook-trigger-patroon](./media/logic-apps-create-api-app/custom-api-webhook-trigger-pattern.png)

> [!NOTE]
> Logic App Designer ondersteunt op dit moment niet detecteren webhook-eindpunten via Swagger. Dus voor dit patroon die u hebt om toe te voegen een [ **Webhook** trigger](../connectors/connectors-native-webhook.md) en geef de URL, headers en hoofdtekst voor uw aanvraag. Zie ook [HTTPWebhook trigger](logic-apps-workflow-actions-triggers.md#httpwebhook-trigger). Als u wilt doorgeven in de callback-URL, kunt u de `@listCallbackUrl()` Werkstroomfunctie in een van de vorige velden indien nodig.
>
> Om te voorkomen dat dezelfde gegevens meerdere keren verwerken, moet de trigger opschonen van gegevens die al is gelezen en doorgegeven aan de logische app.

> [!TIP]
> Voor een voorbeeld van de webhook-patroon, bekijkt u deze [webhook-trigger controller voorbeeld in GitHub](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

## <a name="secure-calls-to-your-apis-from-logic-apps"></a>Beveiligde aanroepen naar uw API's vanuit logic apps

Na het maken van uw aangepaste API's, verificatie instellen voor uw API's zodat u ze veilig vanuit logic apps aanroepen kunt. Informatie over [over het beveiligen van aanroepen naar aangepaste API's vanuit logic apps](../logic-apps/logic-apps-custom-api-authentication.md).

## <a name="deploy-and-call-your-apis"></a>Implementeren en uw API's aanroepen

Na het instellen van verificatie, instellen van de implementatie voor uw API's. Informatie over [over het implementeren en aangepaste API's aanroepen vanuit logic apps](../logic-apps/logic-apps-custom-api-host-deploy-call.md).

## <a name="publish-custom-apis-to-azure"></a>Aangepaste API's publiceren naar Azure

Als u wilt uw aangepaste API's beschikbaar maken voor andere gebruikers van Logic Apps in Azure, moet u beveiliging toevoegen en deze als logische App-connectors te registreren. Zie voor meer informatie het [Overzicht van aangepaste connectors](../logic-apps/custom-connector-overview.md). 

Als u wilt uw aangepaste API's beschikbaar maken voor alle gebruikers in Logic Apps, Microsoft Flow en Microsoft PowerApps, moet u beveiliging toevoegen, meld u aan uw API's als Logic App-connectors en nomineer uw connectors voor de [Microsoft Azure Certified-programma](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/). 

## <a name="get-support"></a>Ondersteuning krijgen

* Voor specifieke hulp bij de aangepaste API's, neem contact op met [ customapishelp@microsoft.com ](mailto:customapishelp@microsoft.com).

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.

* Ter verbetering van Logic Apps kunt u stemmen op ideeën of ideeën indienen op de [site voor gebruikersfeedback van Logic Apps](https://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Volgende stappen

* [Fouten en uitzonderingen verwerken](../logic-apps/logic-apps-exception-handling.md)
* [Aanroepen, trigger of nesten van logische apps met HTTP-eindpunten](../logic-apps/logic-apps-http-endpoint.md)
* [Het meten van gebruik voor acties en triggers](../logic-apps/logic-apps-pricing.md)
