---
title: Voorbeelden en algemene scenario's - Azure Logic Apps | Microsoft Docs
description: Voorbeelden, scenario's, zelfstudies en -scenario's voor Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: e06311bc-29eb-49df-9273-1f05bbb2395c
ms.date: 01/31/2018
ms.openlocfilehash: 89e0294db3178cedd3b14aada0b505787b17c75e
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56671791"
---
# <a name="common-scenarios-examples-tutorials-and-walkthroughs-for-azure-logic-apps"></a>Algemene scenario's, voorbeelden, zelfstudies en -scenario's voor Azure Logic Apps

[Met Azure Logic Apps](../logic-apps/logic-apps-overview.md) helpt u bij het organiseren en andere services integreren door te geven [100 + kant-en-klare connectors](../connectors/apis-list.md), die ligt tussen on-premises SQL Server of SAP op Microsoft Cognitive Services. De Logic Apps-service is 'serverloze', zodat u niet hoeft te hoeven maken over schalen of exemplaren. U moet doen, is definiëren van de werkstroom met een trigger en de acties die de werkstroom wordt uitgevoerd. Het onderliggende platform verwerkt schaal, beschikbaarheid en prestaties. Logic Apps is vooral handig voor use cases en scenario's waar u nodig hebt voor de coördinatie van meerdere acties in verschillende systemen.

Kunt u meer informatie over de vele patronen en mogelijkheden die [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ondersteunt, Hier vindt u algemene voorbeelden en scenario's.

## <a name="popular-starting-points-for-logic-app-workflows"></a>Populair startpunt voor werkstromen voor logische app

Elke logische app begint met een [ *trigger*](../logic-apps/logic-apps-overview.md#logic-app-concepts), en slechts één trigger, die de werkstroom van uw logische app wordt gestart en alle gegevens doorgegeven als onderdeel van deze trigger. Sommige connectors bieden triggers, die worden geleverd in de volgende typen:

* *Poll-triggers*: Een service-eindpunt voor nieuwe gegevens wordt regelmatig gecontroleerd. Als er nieuwe gegevens bestaat, wordt de trigger wordt gemaakt en wordt een nieuw werkstroomexemplaar wordt uitgevoerd met de gegevens als invoer.

* *Push-triggers*: Luistert naar gegevens op een service-eindpunt en wordt er gewacht tot een bepaalde gebeurtenis plaatsvindt. Wanneer de gebeurtenis plaatsvindt, de trigger wordt geactiveerd direct, het maken en uitvoeren van een nieuw werkstroomexemplaar die gebruikmaakt van alle beschikbare gegevens als invoer.

Hier volgen slechts enkele voorbeelden van populaire trigger:

* Polling: 

  * [**Planning - terugkeerpatroon** trigger](../connectors/connectors-native-recurrence.md) kunt u de datum en tijd plus het terugkeerpatroon voor het starten van uw logische app instellen. 
  U kunt bijvoorbeeld de dagen van de week en tijden van de dag voor het activeren van uw logische app selecteren.

  * De trigger 'Wanneer een e-mailbericht wordt ontvangen' kunt u uw logische app controleren op nieuwe e-mailbericht van een e-mailprovider die wordt ondersteund door Logic Apps, bijvoorbeeld [Office 365 Outlook](../connectors/connectors-create-api-office365-outlook.md), [Gmail](https://docs.microsoft.com/connectors/gmail/), [ Outlook.com](https://docs.microsoft.com/connectors/outlook/), enzovoort.

  * De [ **HTTP** trigger](../connectors/connectors-native-http.md) kunt u uw logische app een opgegeven service-eindpunt controleren door te communiceren via HTTP.
  
* Pushen:

  * De [ **aanvraag / antwoord - aanvraag** trigger](../connectors/connectors-native-reqres.md) kunt u uw logische app ontvangen van HTTP-aanvragen en in realtime reageren op gebeurtenissen op een bepaalde manier.

  * De [ **HTTP-Webhook** trigger](../connectors/connectors-native-webhook.md) zich abonneert op een service-eindpunt met het registreren van een *URL voor terugbellen* met die service. 
  Op die manier kunnen de service kan alleen op de hoogte stellen de trigger wanneer de opgegeven gebeurtenis plaatsvindt, zodat de trigger niet hoeft op te vragen voor de service.

Hiermee maakt u een nieuwe werkstroomexemplaar van de logische app en de acties in de werkstroom wordt uitgevoerd na de ontvangst van een melding over nieuwe gegevens of een gebeurtenis, de trigger wordt geactiveerd. U kunt toegang tot alle gegevens van de trigger in de hele werkstroom. De trigger "op een nieuwe tweet' geeft bijvoorbeeld de tweetinhoud in de logische app uitvoeren. 

## <a name="respond-to-triggers-and-extend-actions"></a>Reageren op triggers en acties uitbreiden

Voor systemen en services die mogelijk niet connectors hebt gepubliceerd, kunt u ook logische apps uitbreiden.

* [Maken van aangepaste triggers of acties](../logic-apps/logic-apps-create-api-app.md)
* [Instellen van langdurige acties voor de werkstroom wordt uitgevoerd](../logic-apps/logic-apps-create-api-app.md)
* [Reageren op externe gebeurtenissen en acties met webhooks](../logic-apps/logic-apps-create-api-app.md)
* [Aanroepen, trigger of nesten van werkstromen met synchrone antwoorden op HTTP-aanvragen](../logic-apps/logic-apps-http-endpoint.md)
* [Zelfstudie: Bouw een AI aangestuurde sociale dashboard binnen enkele minuten met Logic Apps en Power BI](https://aka.ms/logicappsdemo)
* [Video: Reageren op Twilio-SMS-webhooks en verzenden van een tekstantwoord](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="control-flow-error-handling-and-logging-capabilities"></a>Controlestroom, foutafhandeling en mogelijkheden voor logboekregistratie

Logische apps bevatten de uitgebreide mogelijkheden voor geavanceerde Controlestroom, zoals voorwaarden, switches, lussen en bereiken. Om ervoor te zorgen veerkrachtige oplossingen, kunt u ook fout en de afhandeling van uitzonderingen in uw werkstromen implementeren. Meldingen en logboeken met diagnostische gegevens voor de status van de werkstroomuitvoering, biedt Azure Logic Apps ook controle en meldingen.

* Andere acties uitvoeren op basis [voorwaardelijke instructies](../logic-apps/logic-apps-control-flow-conditional-statement.md) en [switch-instructies](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Herhaal de stappen of proces items in matrices en verzamelingen met lussen](../logic-apps/logic-apps-control-flow-loops.md)
* [Groepacties samen met scopes](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* [De auteur van fout- en afhandeling van uitzonderingen in een werkstroom](../logic-apps/logic-apps-exception-handling.md)
* [Gebruiksvoorbeeld: Hoe een gezondheidszorg bedrijf maakt gebruik van logische app uitzonderingsverwerking voor HL7 FHIR-werkstromen](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Schakel bewaking, logboekregistratie en waarschuwingen voor logische apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Bewaking en logboekregistratie van diagnostische gegevens inschakelen bij het maken van logische apps](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md)

## <a name="deploy-and-manage-logic-apps"></a>Logische apps implementeren en beheren

U kunt volledig ontwikkelen en implementeren van logische apps met Visual Studio, Azure DevOps, of een andere broncodebeheer en geautomatiseerde build-hulpprogramma's. Logische apps gebruiken ter ondersteuning van implementatie voor werkstromen en afhankelijke verbindingen in de resourcesjabloon van een, sjablonen voor Azure-resource-implementatie. Visual Studio-hulpprogramma's automatisch genereren van deze sjablonen die u broncodebeheer voor versiebeheer controleren kunt.

* [Maken en implementeren van logische apps met Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)
* [Schakel bewaking, logboekregistratie en waarschuwingen voor logische apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Een sjabloon voor automatische implementatie maken](../logic-apps/logic-apps-create-deploy-template.md)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>Typen inhoud, conversies en transformaties in een uitvoering

U kunt toegang krijgen tot, converteren en verschillende typen inhoud transformeren met behulp van de vele functies in de Azure Logic Apps [werkstroomdefinitietaal](https://aka.ms/logicappsdocs). Bijvoorbeeld, u kunt converteren tussen een tekenreeks, JSON en XML met de `@json()` en `@xml()` werkstroom expressies. De Logic Apps-engine bewaart inhoudstypen die u ter ondersteuning van inhoudsoverdracht op een manier zonder verlies tussen services.

* [De werking van werkstroom expressies in logische apps](../logic-apps/logic-apps-author-definitions.md)
* [Niet-JSON-inhoudstypen verwerken](../logic-apps/logic-apps-content-type.md), bijvoorbeeld `application/xml`, `application/octet-stream`, en `multipart/formdata`
* [Werkstroom van werkstroomschema voor Azure Logic Apps](https://aka.ms/logicappsdocs)

## <a name="other-integrations-and-capabilities"></a>Andere integraties en mogelijkheden

Logische apps bieden ook integratie met veel services, zoals Azure Functions, Azure API Management, Azure App Services en aangepaste HTTP-eindpunten, bijvoorbeeld REST en SOAP.

* [Een realtime sociale dashboard maken met de Serverloze Azure](../logic-apps/logic-apps-scenario-social-serverless.md)
* [Azure Functions aanroepen vanuit logic apps](../logic-apps/logic-apps-azure-functions.md)
* [Zelfstudie: Trigger logische apps met Azure Functions](../logic-apps/logic-apps-scenario-function-sb-trigger.md)
* [Zelfstudie: Wijzigingen van de virtuele machine met Azure Event Grid en Logic Apps bewaken](../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md)
* [Zelfstudie: Een functie maken die kan worden geïntegreerd met Azure Logic Apps en Microsoft Cognitive Services naar het Twitter-bericht stemming analyseren](../azure-functions/functions-twitter-email.md)
* [Zelfstudie: Externe controle IoT en meldingen met Azure Logic Apps die gebruikmaken van uw IoT-hub en Postvak](../iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps.md)
* [Blog: SOAP-eindpunten worden aangeroepen vanuit logische apps](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="end-to-end-scenarios"></a>End-to-end scenario 's

* [Whitepaper: Case management van end-to-end-integratie met Azure-services, zoals Logic Apps](https://aka.ms/enterprise-integration-e2e-case-management-utilities-logic-apps)

## <a name="customer-stories"></a>Verhalen van klanten

Informatie over hoe Azure Logic Apps, samen met andere Azure-services en Microsoft-producten, geholpen [deze bedrijven](https://aka.ms/logic-apps-customer-stories) hun flexibiliteit verbeteren en zich richten op hun kernactiviteiten door door te vereenvoudigen, organisatie, automatiseren en organiseren complexe processen.

## <a name="next-steps"></a>Volgende stappen

* [Bouwen op definities voor logische Apps met JSON](../logic-apps/logic-apps-author-definitions.md)
* [Fouten en uitzonderingen in logische apps verwerken](../logic-apps/logic-apps-exception-handling.md)
* [Uw opmerkingen, vragen, feedback of suggesties voor het verbeteren van Azure Logic Apps](https://feedback.azure.com/forums/287593-logic-apps)
