---
title: 'Scenario: een klant insights dashboard maken met Azure zonder server | Microsoft Docs'
description: Een voorbeeld van hoe u een dashboard voor het beheren van feedback van klanten, sociale gegevens en meer met Azure Logic Apps en Azure Functions kunt samenstellen.
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2017
ms.author: jehollan
ms.openlocfilehash: 0b6e118cb13ab8185d8eeb42bec6147155967967
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-real-time-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Een realtime klant insights dashboard maken met Azure Logic Apps en Azure Functions

Azure zonder Server-hulpprogramma's bieden krachtige mogelijkheden om snel te bouwen en toepassingen in de cloud, zonder dat bij het nadenken over de infrastructuur worden gehost.  In dit scenario zullen we een dashboard voor het activeren van feedback van klanten, feedback met machine learning analyseren en inzichten publiceren een bron, zoals Power BI of Azure Data Lake maken.

## <a name="overview-of-the-scenario-and-tools-used"></a>Overzicht van het scenario en de hulpprogramma's gebruikt

Om deze oplossing te implementeren, zullen we gebruikmaken van de twee belangrijkste onderdelen van zonder server apps in Azure: [Azure Functions](https://azure.microsoft.com/services/functions/) en [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).

Logic Apps is een zonder server werkstroomengine in de cloud.  Deze orchestration biedt verschillende zonder Server-onderdelen en ook verbinding maakt met meer dan 100 API's en services.  We gaan een logische app activeren op feedback van klanten te maken voor dit scenario.  Enkele van de connectors die helpen kunnen bij het reageren op feedback van klanten Outlook.com, Office 365, enquête Monkey, Twitter en een HTTP-aanvraag zijn [van een webformulier](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).  Voor de werkstroom hieronder, wordt er een hashtag op Twitter bewaken.

Functies bieden zonder server compute in de cloud.  In dit scenario gebruiken we Azure Functions tweets van klanten op basis van een reeks vooraf gedefinieerde trefwoorden markeren.

De volledige oplossing kan worden [bouwen in Visual Studio](logic-apps-deploy-from-vs.md) en [geïmplementeerd als onderdeel van een sjabloon resource](logic-apps-create-deploy-template.md).  Er is ook video-overzicht van het scenario [op Channel 9](http://aka.ms/logicappsdemo).

## <a name="build-the-logic-app-to-trigger-on-customer-data"></a>De logische app activeren op gegevens van de klant opbouwen

Na [maken van een logische app](logic-apps-create-a-logic-app.md) in Visual Studio of de Azure-portal:

1. Toevoegen van een trigger voor **op nieuwe Tweets** van Twitter
2. Configureer de trigger om te luisteren naar tweets op een sleutelwoord of hashtag.

   > [!NOTE]
   > De eigenschap terugkeerpatroon in de trigger wordt bepaald hoe vaak de logische app controleert op nieuwe items op triggers op basis van polling

   ![Voorbeeld van een trigger Twitter][1]

Deze app wordt nu gestart op alle nieuwe tweets.  We kunnen vervolgens worden die gegevens tweet en begrijpen meer van de gevoel uitgedrukt.  We gebruiken hiervoor de [Azure cognitieve Service](https://azure.microsoft.com/services/cognitive-services/) voor het detecteren van gevoel van tekst.

1. Klik op **nieuwe stap**
1. Selecteer of zoek naar de **Tekstanalyse** connector
1. Selecteer de **detecteren gevoel** bewerking
1. Als u wordt gevraagd, geeft u een geldige sleutel cognitieve Services voor de service Tekstanalyse
1. Voeg de **Tweet tekst** als de tekst te analyseren.

Nu dat we de tweet gegevens en inzichten op de tweet hebt, kan een aantal andere connectors relevant zijn:
* Power BI - rijen toevoegen aan de gegevensset Streaming: weergave tweets realtime op een Power BI-dashboard.
* Azure Data Lake - bestand toevoegen: klantgegevens toevoegen aan een Azure Data Lake-gegevensset in analytics-taken op te nemen.
* SQL - rijen toevoegen: opslaan van gegevens in een database voor later gebruik.
* Vertraging - bericht verzenden: waarschuwing van een ongebruikt kanaal op negatieve feedback die acties vereist.

Een Azure-functie kan ook worden gebruikt om te doen meer aangepaste rekenservice boven op de gegevens.

## <a name="enriching-the-data-with-an-azure-function"></a>De gegevens met een Azure-functie uit te breiden

Voordat we een functie maken kunt, moeten we hebben een functie-app in ons Azure-abonnement.  Meer informatie over het maken van een Azure-functie in de portal kunnen [hier vinden](../azure-functions/functions-create-first-azure-function-azure-portal.md)

Voor een functie moet worden aangeroepen rechtstreeks vanuit een logische app, moet beschikken over een HTTP-binding activeren.  Wordt u aangeraden de **HttpTrigger** sjabloon.

In dit scenario zou de hoofdtekst van de aanvraag van de Azure-functie worden de tweet tekst.  In de functiecode gewoon definiëren logica op als de tekst tweet een sleutel woord of woordgroep bevat.  De functie zelf kan worden gehouden zo eenvoudig of complex zo nodig voor het scenario.

Aan het einde van de functie moet u gewoon een antwoord naar de logische app met enkele gegevens retourneren.  Dit kan zijn van een eenvoudige Booleaanse waarde (bijvoorbeeld `containsKeyword`), of een complex object.

![Geconfigureerde Azure-functie stap][2]

> [!TIP]
> Bij het openen van een complexe antwoord van een functie in een logische app, moet u de actie parseren JSON gebruiken.

Wanneer de functie is opgeslagen, kan worden toegevoegd in de logische app die eerder is gemaakt.  In de logische app:

1. Klik hier om een **nieuwe stap**
1. Selecteer de **Azure Functions** connector
1. Selecteer een bestaande functie kiezen en blader naar de functie gemaakt
1. Verzenden in de **Tweet tekst** voor de **aanvraagtekst**

## <a name="running-and-monitoring-the-solution"></a>Controleert de oplossing

Een van de voordelen van het ontwerpen van zonder Server integraties in Logic Apps is de uitgebreide foutopsporing en mogelijkheden voor bewaking.  Alle uitvoeren (huidige of historische) kan worden weergegeven uit in Visual Studio de Azure-portal of via de REST-API en SDK's.

Een van de eenvoudigste manieren voor het testen van een logische app gebruikt de **uitvoeren** knop in de ontwerpfunctie.  Te klikken op **uitvoeren** , blijven de trigger pollen om de vijf seconden totdat een gebeurtenis wordt gedetecteerd en de voortgang van de uitvoering van een actuele weergave zoveel.

Geschiedenis van vorige uitvoering kunnen worden weergegeven op de overzichtsblade in de Azure portal of met behulp van Visual Studio Cloud Explorer.

## <a name="creating-a-deployment-template-for-automated-deployments"></a>Maken van een implementatiesjabloon voor geautomatiseerde implementaties

Zodra een oplossing is ontwikkeld, kan deze worden vastgelegd en geïmplementeerd via een Azure-implementatie-sjabloon naar een Azure-regio in de hele wereld.  Dit is handig voor beide wijzigen parameters voor verschillende versies van deze werkstroom, maar ook voor het integreren van deze oplossing in een pijplijn build en release.  Informatie over het maken van een sjabloon voor de implementatie vindt [in dit artikel](logic-apps-create-deploy-template.md).

Azure Functions kunnen ook worden opgenomen in de sjabloon voor de implementatie - zodat de volledige oplossing met alle afhankelijkheden kan worden beheerd als één sjabloon.  Een voorbeeld van een functie-implementatiesjabloon vindt u in de [Azure quickstart sjabloon opslagplaats](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).

## <a name="next-steps"></a>Volgende stappen

* [Zie andere voorbeelden en scenario's voor Azure Logic Apps](logic-apps-examples-and-scenarios.md)
* [Bekijk een videowalkthrough over het maken van deze oplossing voor end-to-end](http://aka.ms/logicappsdemo)
* [Informatie over het verwerken en uitzonderingen in een logische app catch](logic-apps-exception-handling.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png