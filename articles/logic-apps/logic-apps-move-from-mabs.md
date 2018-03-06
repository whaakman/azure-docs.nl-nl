---
title: Apps verplaatsen van BizTalk Services naar Azure Logic Apps | Microsoft Docs
description: Verplaatsen of Azure BizTalk Services (MABS) migreert naar Azure Logic Apps
services: logic-apps
documentationcenter: 
author: jonfancey
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: jonfan; LADocs
ms.openlocfilehash: 6e00e62e60c059a16731a77e529b4b93f50802e9
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
# <a name="move-from-biztalk-services-to-azure-logic-apps"></a>Verplaatsen van BizTalk Services naar Azure Logic Apps

Microsoft Azure BizTalk Services (MABS) is buiten gebruik stellen. Uw MABS integratieoplossingen te verplaatsen [Azure Logic Apps](../logic-apps/logic-apps-overview.md), volg de instructies in dit artikel. 

## <a name="introduction"></a>Inleiding

BizTalk Services bestaat uit twee subservices:

* Microsoft BizTalk Services hybride verbindingen
* EAI- en EDI-brug-integratie

[Azure App Service hybride verbindingen](../app-service/app-service-hybrid-connections.md) vervangt BizTalk Services hybride verbindingen. Azure hybride verbindingen is beschikbaar met Azure App Service via de Azure-portal. Deze service biedt een hybride Verbindingsbeheer zodat u kunt bestaande BizTalk Services hybride verbindingen en ook nieuwe hybride verbindingen die u in de portal maakt beheren. 

[Logic Apps](../logic-apps/logic-apps-overview.md) EAI- en EDI-brug-integratie vervangt door de dezelfde mogelijkheden in BizTalk Services en meer. Deze service biedt cloud schalen op basis van verbruik werkstroom en orchestration functies waarmee u snel en eenvoudig complexe integratie om oplossingen te maken via een browser of met Visual Studio.

Deze tabel wijst mogelijkheden van BizTalk Services toe aan Logic Apps.

| BizTalk Services   | Logic Apps            | Doel                      |
| ------------------ | --------------------- | ---------------------------- |
| Connector          | Connector             | Gegevens verzenden en ontvangen   |
| Over de sitekoppelingsbrug             | Logische apps             | Pipeline-processor           |
| Fase valideren     | XML-validatie actie | Valideren van een XML-document tegen een schema | 
| Sitmuleren fase       | Gegevens Tokens           | Eigenschappen promoveren naar berichten of voor de besluitvorming voor routering |
| Fase transformeren    | Actie transformeren      | XML-berichten van de ene indeling geconverteerd naar een andere |
| Fase worden gedecodeerd       | Plat bestand decoderen actie | Plat bestand converteren naar XML |
| Fase coderen       | Plat bestand coderen actie | Converteren van XML naar platte bestanden |
| Bericht-Inspector  | Azure Functions of API-Apps | Aangepaste code uitvoeren in uw integraties |
| Route actie       | Voorwaarde of Switch | Routeren van berichten naar een van de opgegeven connectors |
|||| 

## <a name="biztalk-services-artifacts"></a>BizTalk Services artefacten

BizTalk Services heeft verschillende soorten artefacten. 

## <a name="connectors"></a>Connectors

BizTalk Services connectors helpen bruggen verzenden en ontvangen van gegevens, inclusief wederzijdse bruggen waarmee op basis van HTTP-aanvraag/antwoord-interactie. Logic Apps maakt gebruik van de dezelfde terminologie en heeft 180 + connectors die hetzelfde doel dienen door verbinding te maken met een breed scala aan technologieën en services. Bijvoorbeeld, connectors zijn beschikbaar voor cloud SaaS en PaaS-services, zoals OneDrive, Office365, Dynamics CRM en meer, plus lokale systemen via de On-Premises Data Gateway, die de BizTalk Adapter Service voor BizTalk Services vervangt. Bronnen in BizTalk Services zijn beperkt tot de FTP-, SFTP, en Service Bus-wachtrij of onderwerp abonnement.

![](media/logic-apps-move-from-mabs/sources.png)

Elke brug heeft standaard een HTTP-eindpunt is geconfigureerd met het adres van de Runtime en de eigenschappen relatief adres voor de brug. Om te behalen dezelfde resultaten met Logic Apps, de [aanvraag en -antwoord](../connectors/connectors-native-reqres.md) acties.

## <a name="xml-processing-and-bridges"></a>XML-verwerking en bruggen

In BizTalk Services is een brug vergelijkbaar met een pipeline verwerkt. Een brug gegevens ontvangen van een connector kan duren, maar sommige werken met de gegevens en verzenden van de resultaten naar een ander systeem. Logic Apps heeft hetzelfde effect door dezelfde interactie op basis van een pijplijn patronen als BizTalk Services en ondersteunen ook andere integratie patronen. De [XML-Request-Reply Bridge](https://msdn.microsoft.com/library/azure/hh689781.aspx) in BizTalk Services wordt ook wel een VETER-pijplijn die bestaat uit fasen die deze taken uitvoeren:

* V valideren
* (E) verrijken
* (T) transformatie
* (E) verrijken
* (R) route

Deze afbeelding ziet u hoe verwerking verdeeld is over locatieaanvragen en antwoorden die controle over de aanvraag biedt en de antwoordpaden afzonderlijk, bijvoorbeeld met behulp van verschillende kaarten voor elk pad:

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Een XML-eenzijdige bridge wordt decoderen en coderen fasen ook toegevoegd aan het begin en einde van de verwerking. De brug Pass-Through bevat één fase toegang bieden.

### <a name="message-processing-decoding-and-encoding"></a>Verwerking van berichten, decoderen en codering

In BizTalk Services kunt u verschillende soorten XML-berichten ontvangen en bepalen het overeenkomende schema voor het ontvangen bericht. Deze taak wordt uitgevoerd in de *berichttypen* fase van de pipeline ontvangen verwerkt. De fase decoderen gebruikt vervolgens de gedetecteerde berichttype decoderen van het bericht met het opgegeven schema. Als het schema een plat bestand-schema is, worden in deze fase de binnenkomende plat bestand converteert naar XML. 

Logic Apps biedt vergelijkbare mogelijkheden. U ontvangt een plat bestand via verschillende protocollen met behulp van de andere connector triggers (File System, FTP-, HTTP-, enzovoort) en gebruiken de [plat bestand decoderen](../logic-apps/logic-apps-enterprise-integration-flatfile.md) actie binnenkomende gegevens converteren naar XML. U kunt uw bestaande plat bestand-schema's rechtstreeks naar Logic Apps zonder wijzigingen aan te gaan en vervolgens schema's uploaden naar uw integratie-Account.

### <a name="validation"></a>Validatie

Nadat de inkomende gegevens wordt geconverteerd naar XML (of als XML de berichtindeling ontvangen is), validatie wordt uitgevoerd om te bepalen of het bericht aan uw XSD-schema voldoet. Voor het uitvoeren van deze taak in Logic Apps gebruiken de [XML-validatie](../logic-apps/logic-apps-enterprise-integration-xml-validation.md) in te grijpen. U kunt hetzelfde schema's van BizTalk Services zonder wijzigingen aan te gebruiken.

### <a name="transform-messages"></a>Berichten transformeren

In BizTalk Services converteert de transformatie-fase één op XML gebaseerde berichtindeling naar een andere. Deze taak wordt uitgevoerd door het toepassen van een kaart met behulp van de toewijzing op basis van TRFM. Het proces is vergelijkbaar in Logic Apps. De transformatie-actie wordt uitgevoerd een kaart uit uw Account-integratie. Het belangrijkste verschil is dat toewijzingen in Logic Apps in XSLT-indeling. XSLT omvat de mogelijkheid om bestaande XSLT die u al hebt, zoals voor BizTalk Server met functoids opnieuw te gebruiken. 

### <a name="routing-rules"></a>Regels voor het doorsturen

BizTalk Services maakt een routering beslissing over welke eindpunt of de connector om binnenkomende berichten of gegevens te verzenden. De mogelijkheid te kiezen uit vooraf geconfigureerde eindpunten is het mogelijk de routering filter gebruiken:

![](media/logic-apps-move-from-mabs/route-filter.png)

In BizTalk Services, als er slechts twee opties, met behulp van een *voorwaarde* is de beste manier voor het converteren van de routing-filters in BizTalk Services. Als er meer dan twee, gebruikt een **overschakelen**.

Logic Apps biedt geavanceerde logica mogelijkheden plus geavanceerde Controlestroom en routering met [voorwaardelijke instructies](../logic-apps/logic-apps-control-flow-conditional-statement.md) en [overschakelen instructies](../logic-apps/logic-apps-control-flow-switch-statement.md).

### <a name="enrich"></a>Verrijken

Bij de verwerking van BizTalk Services, worden de fase toegang bieden eigenschappen toegevoegd aan de Berichtcontext die is gekoppeld aan de ontvangen gegevens. Bijvoorbeeld: een eigenschap te gebruiken voor het doorsturen van zoeken in de database, of door het uitpakken van een waarde met behulp van een XPath-expressie te promoveren. Logic Apps biedt toegang tot alle contextgegevens uitvoerwaarden van de voorgaande acties, waardoor het eenvoudig om hetzelfde gedrag te repliceren. Bijvoorbeeld, met behulp van de `Get Row` actie SQL u retourneren gegevens uit een SQL Server-database en de gegevens in een actie besluit gebruiken voor het doorsturen van. Evenzo eigenschappen op binnenkomende Service Bus berichten in de wachtrij door een trigger worden adresseerbare, evenals met behulp van de xpath-expressie voor taal werkstroom definitie XPath.

### <a name="run-custom-code"></a>Aangepaste code uitvoeren

BizTalk Services kunt u [uitvoeren van aangepaste code](https://msdn.microsoft.com/library/azure/dn232389.aspx) die in uw eigen assembly's worden geüpload. Deze functionaliteit wordt geïmplementeerd door de [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector) interface. Elk stadium in de brug bevat twee eigenschappen (op Inspector invoeren en op afsluiten Inspector) waarmee de .NET-type die u hebt gemaakt die deze interface implementeert. Aangepaste code kunt u complexere verwerking uitvoeren voor gegevens en kunt u bestaande code in assembly's die uitvoeren van algemene zakelijke logica hergebruiken. 

Logic Apps biedt twee methoden voor het uitvoeren van aangepaste code: Azure Functions en API Apps. Azure Functions kunnen worden gemaakt en worden aangeroepen vanuit logic apps. Zie [toevoegen en aangepaste code uitvoeren voor logic apps via Azure Functions](../logic-apps/logic-apps-azure-functions.md). API Apps, onderdeel van Azure App Service gebruiken om uw eigen triggers en acties te maken. Meer informatie over [maken van een aangepaste API gebruiken met Logic Apps](../logic-apps/logic-apps-create-api-app.md). 

Als u aangepaste code in assembly's die u vanuit BizTalk Services aanroept hebt, kunt u deze code verplaatsen naar Azure Functions of aangepaste API's maken met API-Apps, afhankelijk van wat u implementeren. Bijvoorbeeld, hebt u code die een andere service waarvoor geen Logic Apps een connector verpakt, vervolgens een API-App maken en gebruiken van de acties die uw API-app in uw logische app biedt. Als u Help-functies of bibliotheken hebt, klikt u vervolgens is Azure Functions waarschijnlijk het meest geschikt is.

### <a name="edi-processing-and-trading-partner-management"></a>EDI verwerking en handelspartnerbeheer

BizTalk Services en Logic Apps zijn onder andere EDI- en B2B verwerking met ondersteuning voor AS2 (toepasselijkheid instructie 2), X12 en EDIFACT. In BizTalk Services uw EDI-bruggen maken en maken of beheren van handelspartners partners en overeenkomsten in de toegewezen bijhouden en beheer-portal.
In Logic Apps u deze functionaliteit kunt ophalen via de [Enterprise Integration Pack (EIP)](../logic-apps/logic-apps-enterprise-integration-overview.md). De EIP biedt [integratie Account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) en B2B-acties voor EDI- en B2B-verwerking. U ook een integratie-Account gebruiken voor het maken en beheren [handelspartners](../logic-apps/logic-apps-enterprise-integration-partners.md) en [overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md). Nadat u een Account integratie maakt, kunt u een of meer logische apps koppelen aan het account. U kunt vervolgens B2B acties gebruiken voor toegang tot trading Partnerinformatie van uw logische app. De volgende acties zijn beschikbaar:

* AS2 coderen
* AS2 Decode
* X12 Encode
* X12 Decode
* EDIFACT coderen
* EDIFACT Decode

In tegenstelling tot BizTalk Services, worden deze acties losgekoppeld van de transportprotocollen. Dus als u uw logische apps maakt, hebt u meer flexibiliteit in welke connectors waarmee u kunt gegevens verzenden en ontvangen. U kunt bijvoorbeeld ontvangen X12 bestanden als bijlagen van e-mail en vervolgens proces deze bestanden in een logische app. 

## <a name="manage-and-monitor"></a>Beheren en controleren

In BizTalk Services biedt een speciale portal mechanismen voor het bijhouden om te controleren en oplossen van problemen. Logic Apps biedt uitgebreidere bijhouden en bewakingsmogelijkheden via de [Azure-portal](../logic-apps/logic-apps-monitor-your-logic-apps.md), en met de [B2B Operations Management Suite-oplossing](../logic-apps/logic-apps-monitor-b2b-message.md), bevat een mobiele app voor het bewaren van gaten op dingen Wanneer u bent op de verplaatsing.

## <a name="high-availability"></a>Hoge beschikbaarheid

Voor hoge beschikbaarheid (HA) in BizTalk Services, kunt u de verwerkingsbelasting delen met behulp van meer dan één exemplaar in een specifieke regio. Logic Apps biedt regio-HA zonder extra kosten. 

BizTalk Services vereist out van de regio noodherstel voor B2B-verwerking is dat een proces voor back-up en herstel. Voor zakelijke continuïteit Logic Apps kunt u regio-overschrijdende actief/passief- [DR mogelijkheid](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md), waarmee u B2B gegevens synchroniseren integratie accounts in verschillende regio's.

## <a name="next-steps"></a>Volgende stappen

* [Wat is Logic Apps?](../logic-apps/logic-apps-overview.md)
* [Uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md), of snel aan de slag met behulp van een [vooraf gemaakte sjabloon](../logic-apps/logic-apps-create-logic-apps-from-templates.md)  
* [Bekijk de beschikbare connectors](../connectors/apis-list.md) die u kunt gebruiken in logic apps