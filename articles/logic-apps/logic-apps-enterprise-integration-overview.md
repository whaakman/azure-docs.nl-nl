---
title: Enterprise-integratie voor B2B - Azure Logic Apps | Microsoft Docs
description: B2B-werkstromen bouwen en ondersteuning voor enterprise integration-scenario's voor logische apps met de Enterprise-integratiepakket
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: dd517c4d-1701-4247-b83c-183c4d8d8aae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 9462707db03ecfcc3d5186ce7ded8655ad3bdcc9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="overview-b2b-scenarios-and-communication-with-the-enterprise-integration-pack"></a>Overzicht: B2B-scenario's en communicatie met de Enterprise-integratiepakket

U kunt enterprise integratiescenario's met Microsoft cloud-gebaseerde oplossing voor het integratiepakket Enterprise inschakelen voor business-to-business (B2B) werkstromen en naadloze communicatie met Azure Logic Apps. Organisaties kunnen uitwisselen berichten elektronisch, zelfs als ze verschillende protocollen en indelingen gebruiken. Het pack transformeert verschillende indelingen in een indeling die organisaties systemen kunnen interpreteren en verwerken. Organisaties kunnen berichten via industriestandaard-protocollen, zoals uitwisselen [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), en [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md). U kunt ook berichten met zowel versleuteling als digitale handtekeningen beveiligen.

Als u bekend met BizTalk Server of Microsoft Azure BizTalk Services bent, zijn de functies voor Enterprise Integration eenvoudig te gebruiken omdat de meeste concepten vergelijkbaar zijn. Een belangrijk verschil is dat Enterprise Integration integratieaccounts gebruikt voor het vereenvoudigen van de opslag en het beheer van artefacten in B2B-communicatie gebruikt. 

Het integratiepakket Enterprise is qua architectuur is gebaseerd op 'integratieaccounts'. Deze accounts zijn cloud-gebaseerde containers waarin alle uw artefacten, zoals schema's, partners, certificaten, maps en overeenkomsten. U kunt deze artefacten ontwerpen, implementeren en onderhouden van uw B2B-apps en om B2B-werkstromen voor logic apps te bouwen. Maar voordat u deze artefacten gebruiken kunt, moet u eerst uw integratie-account koppelen aan uw logische app. Daarna uw logische app toegang tot uw integratie-account artefacten.

## <a name="why-should-you-use-enterprise-integration"></a>Waarom moet u de enterprise-integratie gebruiken?

* Met enterprise-integratie kunt u alle uw artefacten opslaan op één plek--uw integratie-account.
* U kunt B2B werkstromen bouwen en integreren met software-as-service (SaaS)-apps van derden, lokale apps en aangepaste apps met behulp van de Azure Logic Apps-engine en alle bijbehorende connectors.
* U kunt aangepaste code maken voor uw logische apps met Azure functions.

## <a name="how-to-get-started-with-enterprise-integration"></a>Hoe kan ik aan de slag met enterprise-integratie?

U kunt bouwen en B2B apps beheren met de Enterprise Integration Pack via de App-ontwerper van de logica in de **Azure-portal**. U kunt ook uw logische apps met beheren [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logic apps PowerShell-onderwerpen").

Hier volgen de hoofdstappen die u nemen moet voordat u apps in de Azure portal maken kunt:

![Overzicht van de installatiekopie](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Wat zijn enkele algemene scenario's?

Enterprise Integration ondersteunt deze industrienormen:

* EDI - Electronic Data Interchange
* EAI - Enterprise toepassingsintegratie

## <a name="heres-what-you-need-to-get-started"></a>Dit is wat u moet aan de slag

* Een Azure-abonnement met een account voor de integratie
* Visual Studio 2015 maps en schema's maken
* [Microsoft Azure Logic Apps Enterprise Integration-Tools voor Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it-now"></a>Probeer het nu

[Implementeren van een volledig operationeel voorbeeld AS2 verzenden en ontvangen van de logische app](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) die gebruikmaakt van de B2B-functies voor Azure Logic Apps.

## <a name="learn-more"></a>Meer informatie
* [Overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md "meer informatie over enterprise integration-overeenkomsten")
* [Scenario's voor Business-to-Business (B2B)](../logic-apps/logic-apps-enterprise-integration-b2b.md "informatie over het maken van logische apps met B2B-functies")  
* [Certificaten](logic-apps-enterprise-integration-certificates.md "meer informatie over certificaten voor ondernemingen-integratie")
* [Platte bestand codering/decodering](logic-apps-enterprise-integration-flatfile.md "informatie over het coderen en decoderen plat bestand-inhoud")  
* [Integratieaccounts](../logic-apps/logic-apps-enterprise-integration-accounts.md "meer informatie over integratieaccounts")
* [Toegewezen](../logic-apps/logic-apps-enterprise-integration-maps.md "meer informatie over enterprise integration maps")
* [Partners](logic-apps-enterprise-integration-partners.md "meer informatie over enterprise integration-partners")
* [Schema's](logic-apps-enterprise-integration-schemas.md "meer informatie over enterprise integration-schema's")
* [XML-bericht validatie](logic-apps-enterprise-integration-xml.md "informatie over het valideren van XML-berichten met Logic apps")
* [XML-transformatie](logic-apps-enterprise-integration-transform.md "meer informatie over enterprise integration maps")
* [Enterprise Integration-Connectors](../connectors/apis-list.md "meer informatie over enterprise integration pack-connectors")
* [Integratie Account metagegevens](../logic-apps/logic-apps-enterprise-integration-metadata.md "meer informatie over de metagegevens van de integratie-account")
* [B2B-berichten](logic-apps-monitor-b2b-message.md "meer informatie over het bewaken van B2B-berichten")
* [B2B-berichten in de OMS-portal bijhouden](logic-apps-track-b2b-messages-omsportal.md "meer informatie over het bijhouden van B2B-berichten in de OMS-portal")

