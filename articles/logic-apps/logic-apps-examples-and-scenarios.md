---
title: Voorbeelden & algemene scenario's - Azure Logic Apps | Microsoft Docs
description: Meer informatie over logische apps met voorbeelden, scenario's, zelfstudies en scenario 's
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: e06311bc-29eb-49df-9273-1f05bbb2395c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 09/13/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: b88d0c1ccb7a729c95299bcdc3cba5fd73fcdeac
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="common-scenarios-examples-tutorials-and-walkthroughs-for-azure-logic-apps"></a>Algemene scenario's, voorbeelden, zelfstudies en scenario's voor Azure Logic Apps

[Logische Apps van Azure](../logic-apps/logic-apps-overview.md) helpt u bij het indelen en het integreren van verschillende services door te geven [100 + kant-en-klare connectors](../connectors/apis-list.md), bereikbepaling van lokale SQL Server of SAP met cognitieve Microsoft-Services. De service Logic Apps is 'zonder server', zodat u niet hoeft te hoeven maken over scale of exemplaren. U hoeft te doen is definiëren van de werkstroom met een trigger en de acties die de werkstroom uitvoert. Het onderliggende platform verwerkt schaal, beschikbaarheid en prestaties. Logic Apps is vooral nuttig voor gebruiksvoorbeelden en scenario's waarin u wilt meerdere acties coördineren in meerdere systemen.

Kunt u meer informatie over de vele patronen en mogelijkheden die [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ondersteunt, Hier vindt u algemene voorbeelden en scenario's.

## <a name="popular-starting-points-for-logic-app-workflows"></a>Populair beginpunt voor logic app-werkstromen

Elke logische app begint met een [ *trigger*](../logic-apps/logic-apps-overview.md#logic-app-concepts), en slechts één worden geactiveerd, waardoor uw logische app werkstroom wordt gestart en wordt in alle gegevens worden doorgegeven als onderdeel van deze trigger. Sommige connectors bieden triggers, die worden geleverd in de volgende typen:

* *Polling-triggers*: controleert regelmatig een service-eindpunt voor nieuwe gegevens. Als er nieuwe gegevens bestaat, wordt de trigger wordt gemaakt en wordt een nieuw werkstroomexemplaar wordt uitgevoerd met de gegevens als invoer.

* *Push-triggers*: luistert naar gegevens op een service-eindpunt en wacht totdat een bepaalde gebeurtenis gebeurt. Wanneer de gebeurtenis plaatsvindt, de trigger wordt geactiveerd onmiddellijk, maken en uitvoeren van een nieuw workflowexemplaar die gebruikmaakt van alle beschikbare gegevens als invoer.

Hier volgen enkele voorbeelden van populaire trigger:

* Polling: 

  * [**Schema - terugkeerpatroon** trigger](../connectors/connectors-native-recurrence.md) kunt u de datum en tijd plus de terugkeer voor het activeren van uw logische app instellen. 
  U kunt bijvoorbeeld de dagen van de week en de tijdstippen voor activering van uw logische app selecteren.

  * De trigger 'Wanneer u een e-mailbericht ontvangen' kiest, kunnen uw logische app controleren op nieuwe e-mail via een e-mailprovider die wordt ondersteund door Logic Apps, bijvoorbeeld [Outlook van Office 365](../connectors/connectors-create-api-office365-outlook.md), [Gmail](https://docs.microsoft.com/connectors/gmail/), [ Outlook.com](https://docs.microsoft.com/connectors/outlook/), enzovoort.

  * De [ **HTTP** trigger](../connectors/connectors-native-http.md) kunt u uw logische app een opgegeven service-eindpunt controleren door te communiceren via HTTP.
  
* Push:

  * De [ **vraag / antwoord - aanvraag** trigger](../connectors/connectors-native-reqres.md) kunt u uw logische app ontvangen van HTTP-aanvragen en hierop reageren in realtime gebeurtenissen in een bepaalde manier.

  * De [ **HTTP-Webhook** trigger](../connectors/connectors-native-webhook.md) is lid van een service-eindpunt via het registreren van een *retouraanroep URL* met die service. 
  Op die manier de service kan alleen melden de trigger als de opgegeven gebeurtenis gebeurt, zodat de trigger niet nodig voor het pollen van de service.

Maakt een nieuw exemplaar van logic app werkstroom en de acties in de werkstroom wordt uitgevoerd na de ontvangst van een melding over nieuwe gegevens of een gebeurtenis, de trigger wordt geactiveerd. U kunt toegang tot alle gegevens van de trigger in de hele werkstroom. De trigger 'aan een nieuwe tweet"wordt de inhoud tweet doorgegeven in de logische app uitgevoerd. 

## <a name="respond-to-triggers-and-extend-actions"></a>Reageren op triggers en acties uit te breiden

Voor systemen en services die mogelijk niet connectors hebt gepubliceerd, kunt u ook logische apps uitbreiden.

* [Maken van aangepaste triggers of acties](../logic-apps/logic-apps-create-api-app.md)
* [Instellen van langlopende acties voor de werkstroom wordt uitgevoerd](../logic-apps/logic-apps-create-api-app.md)
* [Reageren op externe gebeurtenissen en acties met webhooks.](../logic-apps/logic-apps-create-api-app.md)
* [Bel, trigger of nesten van werkstromen met synchrone reacties op HTTP-aanvragen](../logic-apps/logic-apps-http-endpoint.md)
* [Zelfstudie: Bouw een AI ingeschakeld sociale dashboard in minuten met Logic Apps en Power BI](http://aka.ms/logicappsdemo)
* [Zelfstudie: Reageren op Twilio SMS webhooks en verzendt een tekstantwoord](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="control-flow-error-handling-and-logging-capabilities"></a>Controlestroom foutafhandeling en mogelijkheden voor logboekregistratie

Logische apps bevatten uitgebreide mogelijkheden voor geavanceerde Controlestroom, zoals voorwaarden, switches, lussen en -scopes. Om ervoor te zorgen flexibele oplossingen, kunt u ook fout en afhandeling van uitzonderingen in uw werkstromen implementeren. Meldingen en logboeken met diagnostische gegevens voor werkstroom uitvoeringsstatus biedt Azure Logic Apps ook controle en waarschuwingen.

* [Proces-items in de arrays en verzamelingen met lussen en batches in logic apps](../logic-apps/logic-apps-loops-and-scopes.md)
* [Andere acties uitvoeren in het switch-instructies](../logic-apps/logic-apps-switch-case.md)
* [Fout van de auteur en afhandeling van uitzonderingen in een werkstroom](../logic-apps/logic-apps-exception-handling.md)
* [Gebruiksvoorbeeld: hoe een bedrijf gezondheidszorg logic app uitzonderingsverwerking voor HL7 FHIR werkstromen gebruikt](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Bewaking, logboekregistratie en waarschuwingen voor bestaande logische apps inschakelen](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Controle en logboekregistratie van diagnostische gegevens inschakelen bij het maken van logische apps](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md)

## <a name="deploy-and-manage-logic-apps"></a>Logic apps implementeren en beheren

U kunt volledig ontwikkelen en logic apps implementeren met Visual Studio, Visual Studio Team Services, of een andere broncodebeheer en geautomatiseerde build tools. Logische apps gebruiken ter ondersteuning van implementatie voor werkstromen en afhankelijke verbindingen in de sjabloon van een resource, implementatiesjablonen Azure-resource. Deze sjablonen die u broncodebeheer voor versiebeheer controleren kunt automatisch worden gegenereerd in Visual Studio-hulpprogramma's.

* [Bouwen en implementeren van logische apps vanuit Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md)
* [Bewaking, logboekregistratie en waarschuwingen voor bestaande logische apps inschakelen](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Een sjabloon voor automatische implementatie maken](../logic-apps/logic-apps-create-deploy-template.md)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>Typen inhoud, conversies en transformaties binnen een uitvoering

Toegang tot, converteren en transformatie van verschillende typen inhoud met behulp van de vele functies in de Azure Logic Apps [werkstroom definition language](http://aka.ms/logicappsdocs). Bijvoorbeeld, u kunt converteren tussen een tekenreeks, JSON en XML met de `@json()` en `@xml()` expressies. De Logic Apps-engine van bewaart inhoudstypen ter ondersteuning van inhoudsoverdracht op een manier zonder verlies tussen services.

* [De werking van expressies in logic apps](../logic-apps/logic-apps-author-definitions.md)
* [Typen niet JSON-inhoud verwerken](../logic-apps/logic-apps-content-type.md), bijvoorbeeld `application/xml`, `application/octet-stream`, en`multipart/formdata`
* [Referentie: Definitietaal van de werkstroom met logische Apps van Azure](http://aka.ms/logicappsdocs)

## <a name="other-integrations-and-capabilities"></a>Andere integraties en mogelijkheden

Logische apps bieden ook integratie met veel services, zoals Azure Functions, Azure API Management, Azure App Services en aangepaste HTTP-eindpunten, bijvoorbeeld REST en SOAP.

* [Maak een realtime sociale dashboard met Azure zonder server](../logic-apps/logic-apps-scenario-social-serverless.md)
* [Azure-functies aanroepen vanuit logic apps](../logic-apps/logic-apps-azure-functions.md)
* [Scenario: Trigger logic apps met Azure Functions](../logic-apps/logic-apps-scenario-function-sb-trigger.md)
* [Blog: Call SOAP-eindpunten vanuit logic apps](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="end-to-end-scenarios"></a>End-to-end scenario 's

* [Technisch document: Enterprise integration end-to-end aanvraagbeheer met Azure-services, zoals Logic Apps](https://aka.ms/enterprise-integration-e2e-case-management-utilities-logic-apps)

## <a name="next-steps"></a>Volgende stappen

* [De workflowdefinitie auteur met de werkstroom definition language](../logic-apps/logic-apps-author-definitions.md)
* [Voor het afhandelen van fouten en uitzonderingen in logic apps](../logic-apps/logic-apps-exception-handling.md)
* [Verzenden van uw opmerkingen, vragen, feedback of suggesties voor hoe kunnen we Azure Logic Apps verbeteren](https://feedback.azure.com/forums/287593-logic-apps)