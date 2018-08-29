---
title: Apps van BizTalk Services verplaatsen naar Azure Logic Apps | Microsoft Docs
description: Migreren van Azure BizTalk Services (MABS) naar Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: jonfancey
ms.author: jonfan
ms.reviewer: estfan, LADocs
ms.topic: article
ms.date: 05/30/2017
ms.openlocfilehash: f27e82e780917e00625ef6a14ab8317d1f5b8ae8
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43124796"
---
# <a name="migrate-from-biztalk-services-to-azure-logic-apps"></a>Migreren van BizTalk Services naar Azure Logic Apps

Microsoft Azure BizTalk Services (MABS) wordt buiten gebruik gesteld. Uw MABS-integratie-oplossingen verplaatsen [Azure Logic Apps](../logic-apps/logic-apps-overview.md), volg de instructies in dit artikel. 

## <a name="introduction"></a>Inleiding

BizTalk Services bestaat uit twee subservices:

* Microsoft BizTalk Services-hybride verbindingen
* EAI en EDI bruggebaseerde integratie

[Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) vervangt Hybrid Connections van BizTalk Services. Azure Hybrid Connections is beschikbaar met Azure App Service via de Azure-portal. Deze service biedt een Hybrid Connection Manager zodat u kunt bestaande hybride verbindingen van BizTalk Services en ook nieuwe hybride verbindingen die u in de portal maakt beheren. 

[Logic Apps](../logic-apps/logic-apps-overview.md) EAI en EDI bruggebaseerde integratie vervangt door de dezelfde mogelijkheden in BizTalk Services en meer. Deze service biedt schaalbare verbruik-functies op basis van werkstroom en indeling voor u snel en eenvoudig complexe integratie om oplossingen te bouwen via een browser of met Visual Studio.

Deze tabel wijst functionaliteit van BizTalk Services naar Logic Apps.

| BizTalk Services   | Logic Apps            | Doel                      |
| ------------------ | --------------------- | ---------------------------- |
| Connector          | Connector             | Gegevens verzenden en ontvangen   |
| Brug             | Logische apps             | Pipeline-processor           |
| Fase valideren     | XML-validatie-actie | Valideren van een XML-document op basis van een schema | 
| Fase verrijken       | Gegevenstokens           | Eigenschappen in berichten of voor Routeringsbeslissingen promoten |
| Fase transformeren    | Actie transformeren      | XML-berichten van de ene indeling geconverteerd naar een andere |
| Fase-decodering       | Plat bestand decoderen actie | Plat bestand converteren naar XML |
| Fase coderen       | Plat bestand coderen actie | Converteren van XML naar plat bestand |
| Bericht-Inspector  | Azure Functions of API-Apps | Aangepaste code wordt uitgevoerd in uw integraties |
| Route actie       | Voorwaarde of Switch | Routeren van berichten naar een van de opgegeven connectors |
|||| 

## <a name="biztalk-services-artifacts"></a>BizTalk Services artefacten

BizTalk Services heeft verschillende soorten artefacten. 

## <a name="connectors"></a>Connectors

BizTalk Services connectors helpen bruggen verzenden en ontvangen van gegevens, inclusief wederzijdse bruggen waarmee op basis van HTTP-aanvraag/antwoord interacties. Logic Apps maakt gebruik van de dezelfde terminologie en 180 connectors die hetzelfde doel dienen via een verbinding met een breed scala van technologieën en services heeft. Bijvoorbeeld: connectors zijn beschikbaar voor cloud SaaS en PaaS-services, zoals OneDrive, Office 365, Dynamics CRM en nog veel meer, plus on-premises systemen via de On-Premises Data Gateway, die wordt vervangen door de BizTalk Adapter Service voor BizTalk Services. Bronnen in BizTalk Services zijn beperkt tot de FTP-, SFTP, en Service Bus-wachtrij of onderwerp.

![](media/logic-apps-move-from-mabs/sources.png)

Elke brug heeft standaard een HTTP-eindpunt is geconfigureerd met de Runtime-adres en de relatieve adres-eigenschappen voor de brug. Voor het bereiken van de dezelfde resultaten met Logic Apps, gebruikt u de [aanvraag en respons](../connectors/connectors-native-reqres.md) acties.

## <a name="xml-processing-and-bridges"></a>XML-verwerking en bruggen

Een brug is in BizTalk Services, vergelijkbaar met een pijplijn voor verwerking. Een brug kan duren voordat gegevens ontvangen van een connector, doen enkele werken met de gegevens en de resultaten verzenden naar een ander systeem. Logic Apps biedt dezelfde doordat ondersteunen dezelfde interactie op basis van een pijplijn patronen als BizTalk Services en ook andere integratiepatronen. De [XML-aanvraag / antwoord-brug](https://msdn.microsoft.com/library/azure/hh689781.aspx) in BizTalk Services wordt ook wel een VETER-pijplijn die bestaat uit fasen die taken uitvoeren:

* (V) valideren
* (E) verrijken
* (T) transformeren
* (E) verrijken
* (R) route

Deze afbeelding ziet u hoe verwerking wordt verdeeld tussen tot locatieaanvragen en antwoorden, waarmee u controle over de aanvraag en de antwoordpaden afzonderlijk, bijvoorbeeld met behulp van andere kaarten voor elk pad:

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Een XML-eenzijdige bridge wordt decoderen en coderen fasen ook toegevoegd aan het begin en einde van de verwerking van. De brug Pass Through-query bevat een eenmalige toegang bieden-fase.

### <a name="message-processing-decoding-and-encoding"></a>Verwerking van berichten, decoderen en codering

In BizTalk Services, kunt u verschillende soorten XML-berichten ontvangen en bepalen het overeenkomende schema voor het ontvangen bericht. Deze taak wordt uitgevoerd in de *berichttypen* fase van de pijplijn van de verwerking van ontvangen. De fase decoderen gebruikt vervolgens de gedetecteerde berichttype om het decoderen van het bericht met het opgegeven schema. Als het schema een schema van platte bestanden is, wordt deze fase de binnenkomende plat bestand geconverteerd naar XML. 

Logic Apps biedt vergelijkbare mogelijkheden. U ontvangt een plat bestand via verschillende protocollen met behulp van de andere connector triggers (File System, FTP, HTTP, enzovoort) en gebruik de [plat bestand decoderen](../logic-apps/logic-apps-enterprise-integration-flatfile.md) actie die moet worden de binnenkomende gegevens converteren naar XML. U kunt uw bestaande schema's van plat bestand rechtstreeks naar Logic Apps zonder wijzigingen aan te gaan en schema's vervolgens uploaden naar uw integratie-Account.

### <a name="validation"></a>Validatie

Nadat de binnenkomende gegevens wordt geconverteerd naar XML (of als XML de indeling van het bericht ontvangen is), validatie wordt uitgevoerd om te bepalen als het bericht aan uw XSD-schema voldoet. Om deze taak uitvoeren in Logic Apps, gebruikt u de [XML-validatie](../logic-apps/logic-apps-enterprise-integration-xml-validation.md) actie. U kunt hetzelfde schema's van BizTalk Services zonder wijzigingen aan te gebruiken.

### <a name="transform-messages"></a>Berichten transformeren

In BizTalk Services, wordt de transformatie-fase een op basis van een XML-berichtindeling geconverteerd naar een andere. Deze taak wordt uitgevoerd door het toepassen van een kaart, met behulp van het toewijzen van de TRFM gebaseerde. In Logic Apps is het proces vergelijkbaar. De transformatie-actie wordt uitgevoerd een overzicht van uw integratie-Account. Het belangrijkste verschil is dat kaarten in Logic Apps in de XSLT-indeling. XSLT biedt de mogelijkheid om bestaande XSLT die u al hebt, met inbegrip van maps voor BizTalk Server gemaakt met functoids opnieuw te gebruiken. 

### <a name="routing-rules"></a>Regels voor doorsturen

BizTalk Services kunt u een routering beslissing over welke eindpunt of de connector om binnenkomende berichten of gegevens te verzenden. De mogelijkheid om te selecteren in de vooraf geconfigureerde eindpunten is mogelijk met behulp van de routering filteroptie:

![](media/logic-apps-move-from-mabs/route-filter.png)

In de BizTalk Services, als er slechts twee opties, met behulp van een *voorwaarde* is de beste manier voor het converteren van routeringsfilters in BizTalk Services. Als er meer dan twee zijn, gebruikt u een **overschakelen**.

Logic Apps biedt geavanceerde logica mogelijkheden plus geavanceerde Controlestroom en routering met [voorwaardelijke instructies](../logic-apps/logic-apps-control-flow-conditional-statement.md) en [switch-instructies](../logic-apps/logic-apps-control-flow-switch-statement.md).

### <a name="enrich"></a>Verrijken

In de verwerking van BizTalk Services, wordt in de fase toegang bieden eigenschappen toevoegt aan de Berichtcontext dat is gekoppeld aan de ontvangen gegevens. Bijvoorbeeld, bevordering van een eigenschap te gebruiken voor de routering van zoeken in de database, of door een waarde met behulp van een XPath-expressie te extraheren. Logic Apps biedt toegang tot alle contextuele gegevens uitvoer van de voorgaande acties, waardoor het eenvoudig om de hetzelfde gedrag te repliceren. Bijvoorbeeld, met behulp van de `Get Row` verbindingsactie SQL, u als resultaat de gegevens van een SQL Server-database en de gegevens in een actie besluit gebruiken voor de routering. Evenzo, eigenschappen op binnenkomende Service Bus berichten in de wachtrij door een trigger worden opgevraagd, evenals XPath met behulp van de xpath-expressie voor taal werkstroom-definitie.

### <a name="run-custom-code"></a>Aangepaste code wordt uitgevoerd

BizTalk Services kunt u [aangepaste code wordt uitgevoerd](https://msdn.microsoft.com/library/azure/dn232389.aspx) die is geüpload in uw eigen assembly's. Deze functionaliteit is geïmplementeerd door de [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector) interface. Elke fase in de brug omvat twee eigenschappen (op Inspector invoeren en op afsluiten Inspector) waarmee het .NET-type die u hebt gemaakt dat deze interface wordt geïmplementeerd. Aangepaste code kunt u meer complexe verwerking voor gegevens uitvoeren en kunt u bestaande code in assembly's die uitvoeren van algemene bedrijfslogica hergebruiken. 

Logic Apps biedt twee primaire manieren voor het uitvoeren van aangepaste code: Azure Functions en API-Apps. Azure Functions kunnen worden gemaakt en worden aangeroepen vanuit logische apps. Zie [toevoegen en aangepaste code uitvoeren voor logische apps via Azure Functions](../logic-apps/logic-apps-azure-functions.md). Gebruik API-Apps, onderdeel van Azure App Service te maken van uw eigen triggers en acties. Meer informatie over [het maken van een aangepaste API te gebruiken met Logic Apps](../logic-apps/logic-apps-create-api-app.md). 

Als u aangepaste code in assembly's die u vanuit BizTalk Services aanroept hebt, kunt u deze code verplaatsen naar Azure Functions of aangepaste API's maken met API Apps, afhankelijk van wat u implementeert. Bijvoorbeeld, hebt u code die een andere service waarvoor Logic Apps een connector beschikt niet over de wrap vormt, vervolgens een API-App maken en gebruiken van de acties die uw API-app binnen uw logische app biedt. Als u ondersteunende functies of bibliotheken hebt, is Azure Functions waarschijnlijk het meest geschikt is.

### <a name="edi-processing-and-trading-partner-management"></a>EDI-verwerking en het beheer van handelspartners

BizTalk Services en Logic Apps zijn onder andere EDI en B2B-verwerking met ondersteuning voor AS2 (Applicability Statement 2), X12 en EDIFACT. In de BizTalk Services, het maken van EDI-bruggen en maken of beheren van handelspartners en -overeenkomsten in de toegewezen bijhouden en beheren-portal.
In Logic Apps kunt u deze functionaliteit kunt ophalen via de [Enterprise Integration Pack (EIP)](../logic-apps/logic-apps-enterprise-integration-overview.md). De EIP biedt [Integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) en B2B-acties voor EDI en B2B-verwerking. Ook gebruik van een Integratieaccount maakt en beheert [handelspartners](../logic-apps/logic-apps-enterprise-integration-partners.md) en [overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md). Nadat u een Integratieaccount maakt, kunt u een of meer logische apps kunt koppelen aan het account. U kunt vervolgens B2B-acties gebruiken voor toegang tot trading Partnerinformatie van uw logische app. De volgende acties zijn beschikbaar:

* AS2-codering
* AS2-decodering
* X12 coderen
* X12 decoderen
* EDIFACT-codering
* EDIFACT-decodering

In tegenstelling tot BizTalk Services, worden deze acties zijn ontkoppeld van de transportprotocollen. Dus wanneer u uw logische apps maakt, u meer flexibiliteit in welke connectors hebt waarmee u kunt gegevens verzenden en ontvangen. Bijvoorbeeld, kunt u ontvangen X12-bestanden als bijlagen van e-mail en vervolgens verwerken deze bestanden in een logische app. 

## <a name="manage-and-monitor"></a>Beheren en controleren

Een speciale portal opgegeven in BizTalk Services, bijhouden mogelijkheden om te controleren en oplossen van problemen. Logic Apps biedt uitgebreidere bijhouden en bewakingsmogelijkheden via de [Azure-portal](../logic-apps/logic-apps-monitor-your-logic-apps.md), en is voorzien van een mobiele app voor het bewaren van gaten op zaken wanneer u onderweg bent.

## <a name="high-availability"></a>Hoge beschikbaarheid

Voor hoge beschikbaarheid (HA) in BizTalk Services, kunt u de verwerkingsbelasting kunt delen met behulp van meer dan één exemplaar in een bepaalde regio. Logic Apps biedt regionale HA zonder extra kosten. 

Buiten de regio noodherstel voor B2B-verwerking vereist in BizTalk Services, een proces back-up en herstel. Logic Apps biedt voor bedrijfscontinuïteit, interregionale actief/passief [DR-mogelijkheid](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md), waarmee u B2B-gegevens synchroniseren tussen integratieaccounts in verschillende regio's.

## <a name="next-steps"></a>Volgende stappen

* [Wat is de Logic Apps?](../logic-apps/logic-apps-overview.md)
* [Uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md), of snel aan de slag met behulp van een [vooraf gemaakte sjabloon](../logic-apps/logic-apps-create-logic-apps-from-templates.md)  
* [Weergeven van alle beschikbare connectors](../connectors/apis-list.md) die u kunt gebruiken in logische apps