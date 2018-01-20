---
title: Web-API's & REST-API's maken voor Azure Logic Apps | Microsoft Docs
description: Web-API's & REST-API's voor het aanroepen van uw API's, services of systemen van logic app werkstromen van systeemintegraties maken
keywords: Web-API's, REST-API's, werkstromen, systeemintegraties
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: bd229179-7199-4aab-bae0-1baf072c7659
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/26/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: ec7fe2adfb89edd635adcf247eea0b98f7007b1b
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="create-custom-apis-that-you-can-call-from-logic-app-workflows"></a>Aangepaste API's die u vanuit logic app werkstromen aanroepen kunt maken

Hoewel Azure Logic Apps biedt [100 + ingebouwde verbindingslijnen](../connectors/apis-list.md) dat u kunt gebruiken in logic app werkstromen, kunt u aan te roepen API's, systemen en services die niet beschikbaar als connectors. U kunt uw eigen API's waarmee acties en triggers voor gebruik in logic apps kunt maken. Hier volgen andere redenen waarom u mogelijk wilt maken van uw eigen API's die u vanuit logic app werkstromen aanroepen kunt:

* Breid uw huidige systeem integratie en werkstromen voor integratie.
* Klanten die uw service gebruiken voor het beheren van professionele of persoonlijke taken helpen.
* Vouw de reach, detectie en gebruik voor uw service.

In principe connectors web-API's die gebruikmaken van de REST voor pluggable interfaces zijn [Swagger-metagegevensindeling](http://swagger.io/specification/) voor documentatie en JSON als hun exchange gegevensindeling. Omdat connectors REST-API's die via HTTP-eindpunten communiceren zijn, kunt u een andere taal, zoals .NET, Java of Node.js, voor het bouwen van connectors. U kunt ook uw API's hosten op [Azure App Service](../app-service/app-service-web-overview.md), een platform-as-a-service (PaaS) die een van de beste, eenvoudigste en meest schaalbare manier voor het hosten van de API biedt. 

Aangepaste API's om te werken met logic apps, uw API kan bieden [ *acties* ](./logic-apps-overview.md#logic-app-concepts) die specifieke taken uitvoeren in logic app werkstromen. Uw API kan ook fungeren als een [ *trigger* ](./logic-apps-overview.md#logic-app-concepts) die een werkstroom van logic app wordt gestart wanneer een opgegeven voorwaarde voldoet aan de nieuwe gegevens of een gebeurtenis. Dit onderwerp beschrijft algemene patronen die u uitvoeren kunt voor het bouwen van acties en triggers in uw API, op basis van het gedrag op dat u wilt dat uw API om te bieden.

U kunt uw API's hosten op [Azure App Service](../app-service/app-service-web-overview.md), een platform-as-a-service (PaaS) en biedt in hoge mate schaalbaar, eenvoudige API die als host fungeert van de aanbieding.

> [!TIP] 
> Hoewel u uw API's als web-apps implementeren kunt, houd rekening met uw API's implementeren als API apps, die kunnen efficiënter werken wanneer u, host bouwt, en API's in de cloud en on-premises gebruiken. U hoeft niet te wijzigen van de code in de API's, u hoeft uw code implementeren in een API-app. Bijvoorbeeld informatie over het bouwen van de API-apps die zijn gemaakt met deze talen: 
> 
> * [ASP.NET](../app-service/app-service-web-get-started-dotnet.md). 
> * [Java](../app-service/app-service-web-get-started-java.md)
> * [Node.js](../app-service/app-service-web-get-started-nodejs.md)
> * [PHP](../app-service/app-service-web-get-started-php.md)
> * [Python](../app-service/app-service-web-get-started-python.md)
>
> Voor voorbeelden van API-App voor logic apps zijn gebouwd, gaat u naar de [Azure Logic Apps GitHub-opslagplaats](http://github.com/logicappsio) of [blog](http://aka.ms/logicappsblog).

## <a name="how-do-custom-apis-differ-from-custom-connectors"></a>Hoe verschillen aangepaste API's van aangepaste connectors?

Aangepaste API's en [aangepaste connectors](../logic-apps/custom-connector-overview.md) web-API's die gebruikmaken van de REST voor pluggable interfaces zijn [Swagger-metagegevensindeling](http://swagger.io/specification/) voor documentatie en JSON als hun exchange gegevensindeling. En omdat deze API's en -connectors REST-API's die via HTTP-eindpunten communiceren zijn, kunt u een andere taal, zoals .NET, Java of Node.js, voor het bouwen van aangepaste API's en -connectors.

Aangepaste API's kunt u het aanroepen van API's die niet connectors zijn en HTTP- + Swagger-, Azure API Management- of App-Services bieden eindpunten die u kunt aanroepen. Aangepaste connectors werken als aangepaste API's, maar ook nog hebben deze kenmerken:

* Geregistreerd als bronnen in Azure Logic Apps-Connector.
* Met de pictogrammen naast Microsoft beheerde connectors in de ontwerpfunctie voor Logic Apps weergegeven.
* Alleen beschikbaar voor de verbindingslijnen auteurs en logic app-gebruikers die beschikken over dezelfde Azure Active Directory-tenant en Azure-abonnement in de regio waar de logic apps zijn geïmplementeerd.

U kunt ook geregistreerde connectors benoemen voor Microsoft-certificeringsinstantie. Dit proces wordt gecontroleerd of geregistreerde connectors voldoen aan de criteria voor openbaar gebruik en maakt deze connectors beschikbaar voor gebruikers in Microsoft Flow en Microsoft PowerApps.

Zie voor meer informatie over aangepaste connectors. 

* [Overzicht van aangepaste connectors](../logic-apps/custom-connector-overview.md)
* [Aangepaste connectors van Web-API's maken](../logic-apps/custom-connector-build-web-api-app-tutorial.md)
* [Aangepaste connectors registreren in Azure Logic Apps](../logic-apps/logic-apps-custom-connector-register.md)

## <a name="helpful-tools"></a>Nuttige hulpmiddelen

Een aangepaste API werkt het beste met logic apps als de API ook heeft een [Swagger-document](http://swagger.io/specification/) die beschrijft de bewerkingen van de API en de parameters.
Veel bibliotheken, zoals [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle), kan het Swagger-bestand automatisch genereren voor u. Maak het Swagger-bestand voor weergegeven namen en typen eigenschappen voor een aantekening ook kunt u [TRex](https://github.com/nihaue/TRex) zodat uw Swagger-bestand goed samen met logische apps werkt.

<a name="actions"></a>

## <a name="action-patterns"></a>Actie patronen

Voor logic apps taken uit te voeren, uw aangepaste API moet bieden [ *acties*](./logic-apps-overview.md#logic-app-concepts). Elke bewerking in uw API wordt toegewezen aan een actie. Een basic actie is een domeincontroller die accepteert HTTP-aanvragen en retourneert HTTP-antwoorden. Een logische app verzendt dus bijvoorbeeld een HTTP-aanvraag naar uw web-app of API-app. Uw app retourneert vervolgens een HTTP-antwoord, samen met de inhoud die kan worden verwerkt door de logische app.

U kunt een HTTP-aanvraagmethode schrijven in uw API en beschrijven die methode in een Swagger-bestand voor een standaard-actie. Vervolgens kunt u uw API rechtstreeks met aanroepen een [HTTP-actie](../connectors/connectors-native-http.md) of een [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) in te grijpen. Standaard antwoorden moeten worden geretourneerd binnen de [aanvraag time-outlimiet](./logic-apps-limits-and-config.md). 

![Standaard actie-patroon](./media/logic-apps-create-api-app/standard-action.png)

<a name="pattern-overview"></a>Als u een logische app geduld terwijl uw API langer actief taken zijn voltooid, uw API kan volgen de [asynchrone polling patroon](#async-pattern) of de [asynchrone webhook patroon](#webhook-actions) in dit onderwerp beschreven. Denk aan het proces voor het ordenen van een aangepaste taart van een bakkerij voor een vergelijking waarmee u verschillende gedrag van deze patronen visualiseren. Het polling-patroon komt overeen met het gedrag waar u de bakkerij elke 20 minuten aanroepen om te controleren of de cake gereed is. Het patroon webhook komt overeen met het gedrag waar de bakkerij u om uw telefoonnummer vraagt zodat deze u aanroepen kunnen wanneer de cake gereed is.

Voor voorbeelden, gaat u naar de [Logic Apps GitHub-opslagplaats](https://github.com/logicappsio). Ook meer informatie over [gebruiksmeting voor acties](logic-apps-pricing.md).

<a name="async-pattern"></a>

### <a name="perform-long-running-tasks-with-the-polling-action-pattern"></a>Uitvoeren van langlopende taken worden uitgevoerd met het patroon van de actie polling

Om uw API taken uitvoeren die kunnen worden uitgevoerd, langer is dan de [aanvraag time-outlimiet](./logic-apps-limits-and-config.md), kunt u het asynchrone polling-patroon. Dit patroon heeft uw API wilt werken in een afzonderlijke thread, maar blijft een actieve verbinding met de engine Logic Apps. Op die manier de logische app heeft geen time-out of Ga door met de volgende stap in de werkstroom voordat uw API werkende is voltooid.

Hier volgt het algemene patroon:

1. Zorg ervoor dat de engine weet dat uw API geaccepteerd van de aanvraag en is gestart.
2. Wanneer de engine voor het volgende aanvragen voor de taakstatus van de maakt, kunt u de engine weten wanneer uw API de taak is voltooid.
3. Terug naar de engine voor het relevante gegevens zodat de logic app-werkstroom kan worden voortgezet.

<a name="bakery-polling-action"></a>Nu toepassen van de vorige bakkerij vergelijking met het patroon polling en stel dat u een bakkerij en volgorde een aangepaste taart voor levering aanroepen. Het proces voor het maken van de cake kost tijd en u niet wilt wachten op de telefoon, terwijl de bakkerij op de taart werkt. De bakkerij wordt bevestigd dat uw bestelling en moet u elke 20 minuten voor de status van de cake aanroepen. Na 20 minuten doorgeven, aanroepen van de bakkerij, maar ze laat u weten dat uw taart wordt niet uitgevoerd en dat u in een andere 20 minuten aanroepen moet. Dit proces en weer schakelen wordt vervolgd totdat u aanroept en de bakkerij kunt u zien dat uw bestelling klaar is, en de taart levert. 

Dus laten we dit patroon polling weer worden toegewezen. De bakkerij geeft uw aangepaste API gebruiken, terwijl u de klant taart de engine voor Logic Apps vertegenwoordigen. Als de engine voor uw API met een aanvraag wordt aangeroepen, wordt uw API wordt bevestigd dat de aanvraag en reageert met het tijdsinterval wanneer de engine taakstatus kunt controleren. De engine blijft taakstatus controleren totdat uw API reageert dat de taak wordt uitgevoerd en retourneert gegevens naar uw logische app, die vervolgens werkstroom doorgaat. 

![Polling actie patroon](./media/logic-apps-create-api-app/custom-api-async-action-pattern.png)

Hier volgen de stappen voor uw API te volgen, vanuit het perspectief van de API van beschreven:

1. Wanneer uw API een HTTP-aanvraag wordt begonnen ontvangt, onmiddellijk retourneren van een HTTP `202 ACCEPTED` antwoord met de `location` header verderop in deze stap wordt beschreven. Dit antwoord kunt de Logic Apps-engine weten dat uw API heeft de aanvraag ontvangen, aanvaard de nettolading van de aanvraag (gegevens invoeren) en wordt nu verwerkt. 
   
   De `202 ACCEPTED` antwoord moet bestaan uit deze kopteksten:
   
   * *Vereist*: A `location` header die het absolute pad naar een URL waar de Logic Apps-engine de taakstatus van uw API kan controleren

   * *Optionele*: A `retry-after` -header het aantal seconden dat de engine moet worden gewacht bevat voordat het controleren van de `location` URL van taakstatus. 

     De engine controleert standaard elke 20 seconden. Als u een ander interval, omvatten de `retry-after` header en het aantal seconden totdat de volgende polling.

2. Als de opgegeven tijd is verstreken, de Logic Apps polls engine de `location` URL van taakstatus controleren. Uw API moet deze controles uitvoeren en deze antwoorden retourneren:
   
   * Als de taak wordt uitgevoerd, een HTTP retourneren `200 OK` antwoord, samen met de nettolading van de reactie (invoer voor de volgende stap).

   * Als de taak nog wordt verwerkt, geretourneerd met een andere HTTP `202 ACCEPTED` antwoord, maar met de dezelfde kopteksten, zoals het oorspronkelijke antwoord.

Als uw API na dit patroon, er geen iets doet in de definitie van logic Apps werkstroom om door te gaan taakstatus controleren. Wanneer de engine opgehaald met een HTTP `202 ACCEPTED` antwoord en een geldig `location` koptekst, de engine voor het asynchrone patroon respecteert en controleert de `location` header tot uw API een niet-202 antwoord retourneert.

> [!TIP]
> Bekijk dit voor een asynchrone patroon voorbeeld [asynchrone controller antwoord voorbeeld in GitHub](https://github.com/logicappsio/LogicAppsAsyncResponseSample).

<a name="webhook-actions"></a>

### <a name="perform-long-running-tasks-with-the-webhook-action-pattern"></a>Uitvoeren van langlopende taken worden uitgevoerd met het patroon van de actie webhook

Als alternatief kunt u de webhook-patroon voor langlopende taken en asynchrone verwerking. Dit patroon is de logische app wacht totdat een 'callback"van uw API verwerkt alvorens de werkstroom. Deze retouraanroep is een HTTP POST dat een bericht naar een URL verzendt wanneer een gebeurtenis plaatsvindt. 

<a name="bakery-webhook-action"></a>Nu de vorige bakkerij vergelijking met het patroon van de webhook van toepassing en stel dat u een bakkerij en volgorde een aangepaste taart voor levering aanroepen. Het proces voor het maken van de cake kost tijd en u niet wilt wachten op de telefoon, terwijl de bakkerij op de taart werkt. De bakkerij wordt bevestigd dat uw bestelling, maar dit moment kunt u ze geven uw telefoonnummer zodat deze u aanroepen kunnen wanneer de cake wordt uitgevoerd. Deze tijd ziet de bakkerij u wanneer uw bestelling gereed is en de taart te bezorgen.

Wanneer we dit patroon webhook opnieuw toewijst, vertegenwoordigt de bakkerij uw aangepaste API gebruiken, terwijl u de klant taart de engine voor Logic Apps vertegenwoordigen. De engine voor het aanroepen van uw API met een aanvraag en een URL "callback" bevat.
Wanneer de taak is voltooid, uw API maakt gebruikt van de URL de engine voor het melden en gegevens terugsturen naar uw logische app, die vervolgens werkstroom doorgaat. 

Instellen voor dit patroon worden twee eindpunten op uw domeincontroller: `subscribe` en`unsubscribe`

*  `subscribe`eindpunt: wanneer uitvoering van uw API-bewerking in de werkstroom bereikt, de Logic Apps aanroepen engine de `subscribe` eindpunt. Deze stap zorgt ervoor dat de logische app maken van een retouraanroep-URL die uw API worden opgeslagen en wacht u totdat de callback van uw API wanneer werk voltooid is. Uw API en vervolgens teruggebeld met een HTTP POST naar de URL en wordt elke geretourneerde inhoud en -koppen worden doorgegeven als invoer voor de logische app.

* `unsubscribe`eindpunt: als de logische app uitgevoerd wordt geannuleerd, de Logic Apps aanroepen engine de `unsubscribe` eindpunt. Uw API kan vervolgens Hef de registratie van de callback-URL en stop alle processen indien nodig.

![Webhook actie patroon](./media/logic-apps-create-api-app/custom-api-webhook-action-pattern.png)

> [!NOTE]
> Op dit moment ondersteunt de Logic App-ontwerper detecterende webhook eindpunten via Swagger niet. Dus voor dit patroon die u moet toevoegen een [ **Webhook** actie](../connectors/connectors-native-webhook.md) en geef de URL, de kopteksten en de hoofdtekst voor uw aanvraag. Zie ook [werkstroomacties en triggers](logic-apps-workflow-actions-triggers.md#apiconnection-webhook-action). Als u wilt doorgeven in de callback-URL, kunt u de `@listCallbackUrl()` Werkstroomfunctie in een van de vorige velden indien nodig.

> [!TIP]
> Bekijk dit voor een voorbeeld van de webhook-patroon [webhook trigger voorbeeld in GitHub](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

<a name="triggers"></a>

## <a name="trigger-patterns"></a>Trigger patronen

Uw aangepaste API kan fungeren als een [ *trigger* ](./logic-apps-overview.md#logic-app-concepts) die een logische app wordt gestart wanneer een opgegeven voorwaarde voldoet aan de nieuwe gegevens of een gebeurtenis. Deze trigger kunt regelmatig te controleren of wachten en luisteren voor nieuwe gegevens of gebeurtenissen op uw service-eindpunt. Als u nieuwe gegevens of een gebeurtenis voldoet aan de opgegeven voorwaarde, wordt de trigger wordt geactiveerd en begint met de logische app, die aan deze trigger luistert. U kunt logische apps op deze manier starten door uw API kan volgen de [ *polling trigger* ](#polling-triggers) of de [ *webhook trigger* ](#webhook-triggers) patroon. Deze patronen zijn vergelijkbaar aan hun collega's voor [acties polling](#async-pattern) en [webhookacties](#webhook-actions). Ook meer informatie over [gebruiksmeting voor triggers](logic-apps-pricing.md).

<a name="polling-triggers"></a>

### <a name="check-for-new-data-or-events-regularly-with-the-polling-trigger-pattern"></a>Controleren op nieuwe gegevens of gebeurtenissen regelmatig met het patroon van de trigger polling

Een *polling trigger* fungeert vergelijkbaar met de [polling actie](#async-pattern) eerder in dit onderwerp wordt beschreven. De Logic Apps-engine roept periodiek en controleert het eindpunt van de trigger voor nieuwe gegevens of gebeurtenissen. Als de engine voor het nieuwe gegevens of een gebeurtenis die voldoet aan de opgegeven voorwaarde wordt gevonden, wordt de trigger wordt geactiveerd. Vervolgens maakt de engine voor een logische app-exemplaar dat de gegevens als invoer verwerkt. 

![Polling-triggerpatroon](./media/logic-apps-create-api-app/custom-api-polling-trigger-pattern.png)

> [!NOTE]
> Elke aanvraag polling telt als een actie wordt uitgevoerd, zelfs wanneer er geen logic app-exemplaar wordt gemaakt. Om te voorkomen dat dezelfde gegevens meerdere keren verwerken, moet de trigger opschonen van de gegevens die al is gelezen en doorgegeven aan de logische app.

Hier vindt u specifieke stappen voor een trigger polling, vanuit het perspectief van de API van beschreven:

| Nieuwe gegevens of gebeurtenis gevonden?  | API-reactie | 
| ------------------------- | ------------ |
| Gevonden | Retourneren van een HTTP `200 OK` status met de nettolading van de reactie (invoer voor de volgende stap). <br/>Dit antwoord wordt gemaakt van een logische app-exemplaar en de werkstroom wordt gestart. | 
| Niet gevonden | Retourneren van een HTTP `202 ACCEPTED` status met een `location` header en een `retry-after` header. <br/>Voor triggers, de `location` header moet ook bevatten een `triggerState` query-parameter is meestal een 'tijdstempel'. Uw API kunt u deze id gebruiken voor het bijhouden van de laatste keer dat de logische app is geactiveerd. | 
||| 

U kunt bijvoorbeeld geregeld wordt gecontroleerd of uw service voor nieuwe bestanden, een polling-trigger die deze problemen heeft bouwen:

| Aanvraag bevat `triggerState`? | API-reactie | 
| -------------------------------- | -------------| 
| Nee | Retourneren van een HTTP `202 ACCEPTED` status plus een `location` header met `triggerState` ingesteld op de huidige tijd en de `retry-after` interval op 15 seconden. | 
| Ja | Controleer uw service voor bestanden die zijn toegevoegd na het `DateTime` voor `triggerState`. | 
||| 

| Aantal bestanden gevonden | API-reactie | 
| --------------------- | -------------| 
| Één bestand | Retourneren van een HTTP `200 OK` status en de nettolading van de inhoud bijwerken `triggerState` naar de `DateTime` voor de geretourneerde bestands- en set `retry-after` interval op 15 seconden. | 
| Meerdere bestanden | Retourneren van één bestand op een moment en een HTTP- `200 OK` status, update `triggerState`, en stel de `retry-after` interval 0 seconden. </br>Volgende stappen uit om de engine weten dat er meer gegevens beschikbaar is en dat de engine voor de gegevens direct moet aanvragen van de URL in de `location` header. | 
| Geen bestanden | Retourneren van een HTTP `202 ACCEPTED` status, worden niet gewijzigd `triggerState`, en stel de `retry-after` interval op 15 seconden. | 
||| 

> [!TIP]
> Voor een voorbeeld polling triggerpatroon, Bekijk dit [poll trigger controller voorbeeld in GitHub](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/PollTriggerController.cs).

<a name="webhook-triggers"></a>

### <a name="wait-and-listen-for-new-data-or-events-with-the-webhook-trigger-pattern"></a>Wacht en luistert naar nieuwe gegevens of gebeurtenissen met het patroon van de trigger webhook

Een trigger webhook is een *push-signaal* die wordt gewacht en luistert naar nieuwe gegevens of gebeurtenissen op uw service-eindpunt. Als u nieuwe gegevens of een gebeurtenis voldoet aan de opgegeven voorwaarde, wordt de trigger wordt geactiveerd en maakt u een logic app-exemplaar, die vervolgens de gegevens als invoer verwerkt.
Webhook-triggers fungeren vergelijkbaar met de [webhookacties](#webhook-actions) eerder in dit onderwerp beschreven en worden ingesteld met `subscribe` en `unsubscribe` eindpunten. 

* `subscribe`eindpunt: wanneer u toevoegen en de trigger van een webhook niet opslaan in uw logische app, de Logic Apps aanroepen engine de `subscribe` eindpunt. Deze stap zorgt ervoor dat de logische app maken van een retouraanroep-URL die uw API worden opgeslagen. Wanneer er nieuwe gegevens of een gebeurtenis die aan de opgegeven voorwaarde voldoet, wordt uw API teruggebeld met een HTTP POST naar de URL. De nettolading van de inhoud en -koppen doorgeven als invoer voor de logische app.

* `unsubscribe`eindpunt: als de webhook trigger of volledige logische app wordt verwijderd, de Logic Apps aanroepen engine de `unsubscribe` eindpunt. Uw API kan vervolgens Hef de registratie van de callback-URL en stop alle processen indien nodig.

![Webhook triggerpatroon](./media/logic-apps-create-api-app/custom-api-webhook-trigger-pattern.png)

> [!NOTE]
> Op dit moment ondersteunt de Logic App-ontwerper detecterende webhook eindpunten via Swagger niet. Dus voor dit patroon die u moet toevoegen een [ **Webhook** trigger](../connectors/connectors-native-webhook.md) en geef de URL, de kopteksten en de hoofdtekst voor uw aanvraag. Zie ook [HTTPWebhook trigger](logic-apps-workflow-actions-triggers.md#httpwebhook-trigger). Als u wilt doorgeven in de callback-URL, kunt u de `@listCallbackUrl()` Werkstroomfunctie in een van de vorige velden indien nodig.
>
> Om te voorkomen dat dezelfde gegevens meerdere keren verwerken, moet de trigger opschonen van de gegevens die al is gelezen en doorgegeven aan de logische app.

> [!TIP]
> Bekijk dit voor een voorbeeld van de webhook-patroon [webhook trigger controller voorbeeld in GitHub](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

## <a name="secure-calls-to-your-apis-from-logic-apps"></a>Beveiligde aanroepen naar uw API's vanuit logic apps

Na het maken van uw aangepaste API's, verificatie instellen voor uw API's zodat u ze veilig vanuit logic apps aanroepen kunt. Meer informatie over [het aanroepen van aangepaste API's vanuit logic apps beveiligen](../logic-apps/logic-apps-custom-api-authentication.md).

## <a name="deploy-and-call-your-apis"></a>Implementeren en roept de API 's

Na het instellen van verificatie, implementatie ingesteld voor uw API's. Meer informatie over [het implementeren en het aanroepen van aangepaste API's vanuit logic apps](../logic-apps/logic-apps-custom-api-host-deploy-call.md).

## <a name="publish-custom-apis-to-azure"></a>Aangepaste API's publiceren naar Azure

Als u uw aangepaste API's beschikbaar voor andere gebruikers Logic Apps in Azure, moet u beveiliging toevoegen en ze te registreren als Logic App-connectors. Zie voor meer informatie het [Overzicht van aangepaste connectors](../logic-apps/custom-connector-overview.md). 

Als u uw aangepaste API's beschikbaar voor alle gebruikers in Logic Apps, Microsoft Flow en Microsoft PowerApps, moet u beveiliging toevoegen, registreren van uw API's als Logic App-connectors en wijst de connectors voor de [Microsoft Azure-gecertificeerd programma](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/). 

## <a name="get-support"></a>Ondersteuning krijgen

* Voor specifieke hulp met aangepaste API's, neem contact op met [ customapishelp@microsoft.com ](mailto:customapishelp@microsoft.com).

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.

* Ter verbetering van Logic Apps kunt u stemmen op ideeën of ideeën indienen op de [site voor gebruikersfeedback van Logic Apps](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Volgende stappen

* [Fouten en uitzonderingen verwerken](../logic-apps/logic-apps-exception-handling.md)
* [Bel, trigger of nesten van logic apps met HTTP-eindpunten](../logic-apps/logic-apps-http-endpoint.md)
* [Meten van gebruik voor activiteiten en triggers](../logic-apps/logic-apps-pricing.md)
