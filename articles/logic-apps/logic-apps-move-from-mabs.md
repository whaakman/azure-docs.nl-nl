---
title: Apps verplaatsen van BizTalk Services naar Azure Logic Apps | Microsoft Docs
description: Verplaatsen of Azure BizTalk Services MABS migreren naar Logic Apps
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
ms.author: ladocs; jonfan; mandia
ms.openlocfilehash: 01c5376ac5ba9125eede9deb5ee0a7a006a91bb4
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="move-from-biztalk-services-to-logic-apps"></a>Verplaatsen van BizTalk Services naar Logic Apps

Microsoft Azure BizTalk Services (MABS) is buiten gebruik stellen. Gebruik dit onderwerp voor het verplaatsen van uw integratieoplossingen MABS naar Azure Logic Apps. 

## <a name="overview"></a>Overzicht

BizTalk Services bestaat uit twee onderdelen services:

1.  Microsoft BizTalk Services hybride verbindingen
2.  EAI- en EDI-brug-integratie

Als u zoekt hybride verbindingen vervolgens verplaatsen [Azure App Service hybride verbindingen](../app-service/app-service-hybrid-connections.md) hierin worden de wijzigingen en functies van deze service. Azure hybride verbindingen vervangt BizTalk Services hybride verbindingen. Azure hybride verbindingen beschikbaar met Azure App Service en wordt aangeboden in de Azure-portal. Azure hybride verbindingen biedt ook een nieuwe hybride Verbindingsbeheer voor het beheren van bestaande BizTalk Services hybride verbindingen en nieuwe hybride verbindingen die u in de portal maakt. Azure App Service hybride verbindingen is (GA) is algemeen beschikbaar.

EAI- en EDI-brug gebaseerde integratie is Logic Apps de vervanging. Logic Apps biedt dezelfde mogelijkheden als BizTalk Services en meer. Logic Apps biedt cloud schalen op basis van verbruik werkstroom en orchestration functies waarmee u snel en eenvoudig complexe integratieoplossingen bouwen met behulp van een browser of met de hulpprogramma's in Visual Studio.

De volgende tabel bevat een toewijzing van de mogelijkheden van BizTalk Services naar Logic Apps.

| BizTalk Services   | Logic Apps            | Doel                  |
| ------------------ | --------------------- | ---------------------------- |
| Connector          | Connector             | Verzenden en ontvangen van gegevens   |
| Over de sitekoppelingsbrug             | Logische apps             | Pipeline-processor           |
| Fase valideren     | XML-validatie actie      | Valideren van een XML-document tegen een schema             |
| Sitmuleren fase       | Gegevens Tokens      | Eigenschappen promoveren naar berichten of voor de besluitvorming voor routering             |
| Fase transformeren    | Actie transformeren      | XML-berichten van de ene indeling geconverteerd naar een andere             |
| Fase worden gedecodeerd       | Plat bestand decoderen actie      | Plat bestand converteren naar XML             |
| Fase coderen       |  Plat bestand coderen actie      | Converteren van XML naar platte bestanden             |
| Bericht-Inspector       |  Azure Functions of API-Apps      | Aangepaste code uitvoeren in uw integraties             |
| Route actie      |  Voorwaarde of Switch      | Routeren van berichten naar een van de opgegeven connectors             |

Er zijn een aantal verschillende soorten artefacten in BizTalk Services.

## <a name="connectors"></a>Connectors
Connectors in BizTalk Services toestaan bruggen verzenden en ontvangen van gegevens, inclusief wederzijdse bruggen die op basis van HTTP-aanvraag/antwoord-interactie ingeschakeld. In Logic Apps, wordt de dezelfde terminologie gebruikt. Connectors in logic apps kunnen voor dit doel, en tevens bevatten meer dan 140 die verbinding kunnen maken met een groot aantal technologieën en services, zowel on-premises met behulp van de lokale Data Gateway (als vervanging van de BizTalk Adapter Service die wordt gebruikt door BizTalk Services), en in de cloud SaaS en PaaS-services zoals OneDrive, Office365, Dynamics CRM en nog veel meer.

Bronnen in BizTalk Services zijn beperkt tot de FTP-, SFTP, en Service Bus-wachtrij of onderwerp abonnement.

![](media/logic-apps-move-from-mabs/sources.png)

Elke brug heeft een HTTP-eindpunt standaard die is geconfigureerd met de Runtime-adres en de eigenschappen relatief adres van de brug. Om te behalen hetzelfde met Logic Apps, de [aanvraag en -antwoord](../connectors/connectors-native-reqres.md) acties.

## <a name="xml-processing-and-bridges"></a>XML-verwerking en bruggen
Een brug in BizTalk Services is vergelijkbaar met een pipeline verwerkt. Een brug gegevens ontvangen van een connector kan duren, en sommige werken met de gegevens en verzend deze naar een ander systeem. Logische Apps doet hetzelfde door de ondersteuning van dezelfde interactie op basis van een pijplijn patronen als BizTalk Services en biedt ook een aantal andere patronen integratie. De [XML-Request-Reply Bridge](https://msdn.microsoft.com/library/azure/hh689781.aspx) in BizTalk Services wordt ook wel een VETER pijplijn die bestaat uit fasen die kunnen:

* V valideren
* (E) verrijken
* (T) transformatie
* (E) verrijken
* (R) route

Zoals u in de volgende afbeelding, wordt de verwerking verdeeld is over locatieaanvragen en antwoorden en hebt u controle over de aanvraag en de antwoordpaden afzonderlijk (bijvoorbeeld met behulp van verschillende kaarten voor elk):

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Bovendien een XML-eenzijdige bridge decoderen en coderen fasen toegevoegd aan het begin en einde van de verwerking en de Pass Through-brug bevat één fase toegang bieden.

### <a name="message-processing-and-decodingencoding"></a>Bericht te verwerken en te decoderen/codering
XML-berichten met verschillende typen ontvangen in BizTalk Services en bepalen het overeenkomende schema voor het ontvangen bericht. Dit wordt uitgevoerd in de **berichttypen** fase van de pipeline ontvangen verwerkt. De fase decoderen wordt het gedetecteerde berichttype decoderen met behulp van het opgegeven schema. Als het schema een schema PlatBestand is, converteert de binnenkomende PlatBestand naar XML. 

Logic Apps biedt vergelijkbare mogelijkheden. U ontvangt een PlatBestand via een groot aantal verschillende protocollen met behulp van de andere connector triggers (File System, FTP-, HTTP-, enzovoort) en gebruiken de [plat bestand decoderen](../logic-apps/logic-apps-enterprise-integration-flatfile.md) actie binnenkomende gegevens converteren naar XML. U kunt uw bestaande plat bestand-schema's rechtstreeks met logic apps verplaatsen zonder wijzigingen en vervolgens schema's uploaden naar uw integratie-Account.

### <a name="validation"></a>Validatie
Nadat de inkomende gegevens wordt geconverteerd naar XML (of als XML de berichtindeling ontvangen is), validatie wordt uitgevoerd om te bepalen of het bericht aan uw XSD-schema voldoet. Om dit te doen in Logic Apps, gebruikt u de [XML-validatie](../logic-apps/logic-apps-enterprise-integration-xml-validation.md) in te grijpen. Opnieuw, kunt u de dezelfde schema's van BizTalk Services zonder deze te wijzigen.

### <a name="transform-messages"></a>Berichten transformeren
In BizTalk Services converteert de transformatie-fase één op XML gebaseerde berichtindeling naar een andere. Dit wordt gedaan door het toepassen van een kaart met behulp van de toewijzing op basis van TRFM. Het proces is vergelijkbaar in Logic Apps. De transformatie-actie wordt uitgevoerd een kaart uit uw Account-integratie. Het belangrijkste verschil is dat toewijzingen in Logic Apps in XSLT-indeling. XSLT omvat de mogelijkheid om bestaande XSLT die u al hebt, zoals voor BizTalk Server met functoids opnieuw te gebruiken. 

### <a name="routing-rules"></a>Regels voor het doorsturen
BizTalk Services maakt een routering beslissing over welke eindpunt/connector om binnenkomende berichten/gegevens te verzenden. De mogelijkheid om een uit een aantal vooraf geconfigureerde eindpunten te selecteren is het mogelijk de routering filter gebruiken:

![](media/logic-apps-move-from-mabs/route-filter.png)

Logic Apps biedt geavanceerde mogelijkheden voor logica met [voorwaarde](../logic-apps/logic-apps-use-logic-app-features.md) en [Switch](../logic-apps/logic-apps-switch-case.md), geavanceerde Controlestroom inschakelen en routering. Converteren van de routing-filters in BizTalk Services het beste doen met behulp van een **voorwaarde** *als* er zijn slechts twee opties. Als er meer dan twee, gebruikt een **overschakelen**.

### <a name="enrich"></a>Verrijken
De fase toegang bieden in BizTalk Services verwerking worden de eigenschappen toegevoegd aan de Berichtcontext die is gekoppeld aan de ontvangen gegevens. Bijvoorbeeld: een eigenschap te gebruiken voor het doorsturen van (Zie hieronder) van zoeken in de database, of door het uitpakken van een waarde met behulp van een XPath-expressie te promoveren. Logic Apps biedt toegang tot alle contextgegevens uitvoerwaarden van de voorgaande acties, waardoor het eenvoudig om hetzelfde gedrag te repliceren. Bijvoorbeeld, met behulp van de `Get Row` actie SQL u retourneren gegevens uit een SQL Server-database en de gegevens in een actie besluit gebruiken voor het doorsturen van. Evenzo eigenschappen op binnenkomende Service Bus berichten in de wachtrij door een trigger worden adresseerbare, evenals met behulp van de xpath-expressie voor taal werkstroom definitie XPath.

### <a name="use-custom-code"></a>Aangepaste code gebruiken:
BizTalk Services biedt de mogelijkheid om [uitvoeren van aangepaste code](https://msdn.microsoft.com/library/azure/dn232389.aspx) geüpload in uw eigen assembly's. Dit is geïmplementeerd door de [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector.aspx) interface. Elk stadium in de brug bevat twee eigenschappen (op Inspector invoeren en op afsluiten Inspector) waarmee de .net-type die u hebt gemaakt die deze interface implementeert. Aangepaste code kunt u meer gecompliceerde bewerkingen uitvoeren op de gegevens, evenals opnieuw gebruiken van bestaande code in assembly's die algemene zakelijke logica uitvoeren. 

Logic Apps biedt twee methoden voor het uitvoeren van aangepaste code: Azure Functions en API Apps. Azure Functions kunnen worden gemaakt en worden aangeroepen vanuit logic apps. Zie [toevoegen en aangepaste code uitvoeren voor logic apps via Azure Functions](../logic-apps/logic-apps-azure-functions.md). API Apps, onderdeel van Azure App Service gebruiken om uw eigen triggers en acties te maken. Meer informatie over [maken van een aangepaste API gebruiken met Logic Apps](../logic-apps/logic-apps-create-api-app.md). 

Als u aangepaste code in assmeblies die u vanuit BizTalk Services aanroept hebt, kunt u deze code verplaatsen naar Azure Functions, of aangepaste API's maken met API-Apps; afhankelijk van wat u implementeren. Bijvoorbeeld, hebt u code die een andere service dat geen Logic Apps een connector verpakt, vervolgens een API-App maken en gebruiken van de acties die uw API-app in uw logische app biedt. Als u Help-functies of bibliotheken hebt, klikt u vervolgens is Azure Functions waarschijnlijk het meest geschikt is.

### <a name="edi-processing-and-trading-partner-management"></a>EDI verwerking en handelspartnerbeheer
BizTalk Services bevat EDI- en B2B verwerking met ondersteuning voor AS2 (toepasselijkheid instructie 2), X12 en EDIFACT. Dat geldt ook voor Logic Apps. In BizTalk Services uw EDI-bruggen maken en maken of beheren handelspartners partners en overeenkomsten in de toegewezen bijhouden en beheer-portal.

In Logic Apps deze functionaliteit is opgenomen in de [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md). Dit proces bestaat uit de Account van de integratie en B2B acties voor EDI- en B2B-verwerking. De [integratie Account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) wordt gebruikt om te maken en beheren van [handelspartners](../logic-apps/logic-apps-enterprise-integration-partners.md) en [overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md). Wanneer u een Account integratie maakt, kunt u een of meer logische apps aan het account koppelen. Eenmaal gekoppeld, kunt u de B2B-acties voor toegang tot trading Partnerinformatie in uw logische app. De volgende acties zijn beschikbaar:

* AS2 coderen
* AS2 Decode
* X12 Encode
* X12 Decode
* EDIFACT coderen
* EDIFACT Decode

In tegenstelling tot BizTalk Services, worden deze acties losgekoppeld van de transportprotocollen. Dus wanneer u uw logische apps maakt, hebt u meer flexibiliteit op welke connectors die u kunt gegevens verzenden en ontvangen. Bijvoorbeeld, bestanden mogelijk ontvangen X12 als bijlagen van e-mailbericht en klikt u vervolgens proces deze bestanden in een logische app. 

## <a name="manage-and-monitor"></a>Beheren en controleren
De speciale portal voor BizTalk Services opgegeven evenals handelspartnerbeheer, mechanismen voor het bijhouden om te controleren en oplossen van problemen. 

Logic Apps biedt uitgebreidere bijhouden en de bewakingsmogelijkheden in de [Azure-portal](../logic-apps/logic-apps-monitor-your-logic-apps.md), en met de [B2B Operations Management Suite-oplossing](../logic-apps/logic-apps-monitor-b2b-message.md), ook een mobiele app voor het bewaren van gaten op zaken wanneer u zich op de verplaatsing.

## <a name="high-availability"></a>Hoge beschikbaarheid
Om te zorgen voor hoge beschikbaarheid (HA) in BizTalk Services, kunt u meer dan één exemplaar in een bepaald gebied de verwerkingsbelasting delen. Met logic apps in de regio HA is ingebouwd en wordt geleverd zonder extra kosten. Een back-up en herstel proces is vereist voor out van de regio noodherstel voor de verwerking van de B2B van BizTalk Services. In Logic Apps, een actief/passief-regio-overschrijdende [DR mogelijkheid](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md) wordt geleverd; waarmee de synchronisatie van gegevens voor B2B-integratie accounts in verschillende regio's voor bedrijfscontinuïteit.

## <a name="next"></a>Volgende
* [Wat zijn logische apps?](logic-apps-overview.md)
* [Uw eerste logische app maken](quickstart-create-first-logic-app-workflow.md), of snel aan de slag met behulp van een [vooraf gemaakte sjabloon](logic-apps-create-logic-apps-from-templates.md)  
* [Bekijk alle beschikbare connectors](../connectors/apis-list.md) die u kunt gebruiken in een logische app
