---
title: Overzicht van B2B enterprise integration - Azure Logic Apps | Microsoft Docs
description: Bouw geautomatiseerde B2B-werkstromen om oplossingen voor integratie met Azure Logic Apps en Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: dd517c4d-1701-4247-b83c-183c4d8d8aae
ms.date: 09/08/2016
ms.openlocfilehash: 8c0e47f5bed0799b8536cecb38a85ed76185d0cd
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57194925"
---
# <a name="overview-b2b-enterprise-integration-scenarios-in-azure-logic-apps-with-enterprise-integration-pack"></a>Overzicht: B2B-integratie bedrijfsscenario's in Azure Logic Apps met Enterprise Integration Pack

U kunt voor business-to-business (B2B) werkstromen en naadloze communicatie met Azure Logic Apps enterprise integratiescenario's met Microsoft cloud-gebaseerde oplossing, het Enterprise Integration Pack inschakelen. Organisaties kunnen uitwisselen berichten elektronisch, zelfs als ze verschillende protocollen en indelingen gebruiken. Het pack transformeert verschillende indelingen in een indeling die organisaties systemen kunnen interpreteren en verwerken. Organisaties kunnen berichten via het industriestandaard-protocollen, zoals uitwisselen [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), en [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md). U kunt ook beveiligde berichten met zowel versleuteling en digitale handtekeningen.

Als u bekend met BizTalk Server of Microsoft Azure BizTalk Services bent, is het Enterprise Integration-functies zijn eenvoudig te gebruiken omdat de meeste concepten vergelijkbaar zijn. Een belangrijk verschil is dat in Enterprise Integration-integratieaccounts gebruikt voor het vereenvoudigen van de opslag en het beheer van artefacten in B2B-communicatie gebruikt. 

Het Enterprise Integration Pack is qua, gebaseerd op 'integratieaccounts'. Deze accounts zijn cloud-gebaseerde containers die opslaan van alle uw artefacten, zoals schema's, partners, certificaten, kaarten, en overeenkomsten. U kunt deze artefacten gebruiken om te ontwerpen, implementeren en onderhouden van uw B2B-apps en ook om B2B-werkstromen voor logische apps te bouwen. Maar voordat u deze artefacten gebruiken kunt, moet u eerst uw integratie-account koppelen aan uw logische app. Daarna uw logische app toegang tot uw integratie-account-artefacten.

## <a name="why-should-you-use-enterprise-integration"></a>Waarom zou u bedrijfsintegratie gebruiken?

* Met enterprise-integratie, kunt u alle uw artefacten opslaan op één plek--uw integratie-account.
* U kunt de B2B-werkstromen bouwen en integreren met software-as-service (SaaS)-apps van derden, on-premises toepassingen en aangepaste apps met behulp van de Azure Logic Apps-engine en alle bijbehorende connectors.
* U kunt aangepaste code maken voor uw logische apps met Azure functions.

## <a name="how-to-get-started-with-enterprise-integration"></a>Hoe kan ik aan de slag met bedrijfsintegratie?

U kunt bouwen en beheren van B2B-apps met het Enterprise Integration Pack via de ontwerper van logische App in de **Azure-portal**. U kunt ook uw logische apps beheren met [PowerShell](https://docs.microsoft.com/powershell/module/az.logicapp).

Hier volgen de hoofdstappen die u uitvoeren moet voordat u apps in Azure portal maken kunt:

![Overzichtsafbeelding](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Wat zijn enkele algemene scenario's?

Enterprise Integration ondersteunt deze industrienormen:

* EDI - Electronic Data Interchange
* EAI - Enterprise Application Integration

## <a name="heres-what-you-need-to-get-started"></a>Dit is wat u nodig hebt om aan de slag

* Een Azure-abonnement aan een integratieaccount
* Visual Studio 2015 om toewijzingen en schema's te maken
* [Microsoft Azure Logic Apps Enterprise Integration-hulpprogramma's voor Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it-now"></a>Probeer het nu

[Implementeren van een volledig operationeel voorbeeld AS2 verzenden en ontvangen van de logische app](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) die gebruikmaakt van de B2B-functies voor Azure Logic Apps.

## <a name="learn-more"></a>Meer informatie
* [Overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md "meer informatie over enterprise integration-overeenkomsten")
* [Business to Business (B2B)-scenario's](../logic-apps/logic-apps-enterprise-integration-b2b.md "informatie over het maken van logische apps met B2B-functies ")  
* [Certificaten](logic-apps-enterprise-integration-certificates.md "meer informatie over enterprise integration-certificaten")
* [Met de platte bestand codering/decodering](logic-apps-enterprise-integration-flatfile.md "meer informatie over het coderen en decoderen van de inhoud van een plat bestand")  
* [Integratieaccounts](../logic-apps/logic-apps-enterprise-integration-accounts.md "meer informatie over integratieaccounts")
* [Toegewezen](../logic-apps/logic-apps-enterprise-integration-maps.md "meer informatie over enterprise integration-kaarten")
* [Partners](logic-apps-enterprise-integration-partners.md "meer informatie over enterprise integration-partners")
* [Schema's](logic-apps-enterprise-integration-schemas.md "meer informatie over enterprise integration-schema's")
* [XML-bericht validatie](logic-apps-enterprise-integration-xml.md "meer informatie over het valideren van XML-berichten met Logic apps")
* [XML-transformatie](logic-apps-enterprise-integration-transform.md "meer informatie over enterprise integration-kaarten")
* [Enterprise Integration-Connectors](../connectors/apis-list.md "meer informatie over enterprise integration pack-connectors")
* [Integratie-Account metagegevens](../logic-apps/logic-apps-enterprise-integration-metadata.md "meer informatie over de metagegevens van de integratie-account")
* [B2B-berichten bewaken](logic-apps-monitor-b2b-message.md "meer informatie over het controleren van B2B-berichten")
* [B2B-berichten in Azure Monitor-logboeken bijhouden](logic-apps-track-b2b-messages-omsportal.md "meer informatie over het bijhouden van B2B-berichten in de logboeken van Azure Monitor")

