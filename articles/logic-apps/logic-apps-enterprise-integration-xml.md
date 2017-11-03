---
title: Werken met XML-berichten in uw werkstromen - Azure Logic Apps | Microsoft Docs
description: Verwerken, valideren, transformeren en XML-berichten in logic apps en zakelijke verrijken-scenario's met behulp van het Enterprise-integratiepakket
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 47672dc4-1caa-44e5-b8cb-68ec3a76b7dc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 02/27/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 3fec4935f5317be4bf8c9e05f1c24a7c05381b1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="validate-and-transform-xml-encode-and-decode-flat-files-and-enrich-messages-features-in-logic-apps"></a>Valideren en transformeren XML, coderen en decoderen platte bestanden en verrijken berichten functies in logic apps

Met logic apps, hebt u de mogelijkheid om te verwerken van XML-berichten die u verzendt en ontvangt. Deze functie is opgenomen in de Enterprise-integratiepakket. Voor gebruikers met een achtergrond BizTalk Server biedt de Enterprise Integration Pack vergelijkbare mogelijkheden te transformeren en berichten valideren, werken met platte bestanden en zelfs XPath te sitmuleren of specifieke eigenschappen op te halen uit een bericht te gebruiken. 

Voor gebruikers die nieuw in deze ruimte zijn, vouw deze functies uit hoe u berichten verwerken binnen uw werkstroom. Bijvoorbeeld, als u zich in een scenario voor business-to-business en met specifieke XML-schema's, werken kunt u de Enterprise-integratiepakket voor het verbeteren van hoe uw bedrijf deze berichten verwerkt. 

De Enterprise Integration Pack omvat: 

* [XML-validatie](logic-apps-enterprise-integration-xml-validation.md "meer informatie over XML-bericht validatie") -valideren van een XML-binnenkomende of uitgaande bericht ten opzichte van een specifieke schema.
* [XML-transformatie](../logic-apps/logic-apps-enterprise-integration-transform.md "meer informatie over XML-bericht transformaties en maps") - converteren of aanpassen van een XML-bericht op basis van uw vereisten of de vereisten van een partner.
* [Bestand coderen en decoderen van plat bestand platte](logic-apps-enterprise-integration-flatfile.md "meer informatie over een plat bestand-codering/decodering") - coderen of een plat bestand decoderen. Bijvoorbeeld, SAP accepteert en IDOC bestanden in een plat bestand-indeling verzendt. Veel integratie platforms maken XML-berichten, inclusief Logic Apps. U kunt dus een logische app die gebruikmaakt van het coderingsprogramma plat bestand "" XML-bestanden converteren naar platte bestanden maken. 
* [XPath](https://msdn.microsoft.com/library/mt643789.aspx) - een bericht Sitmuleren en specifieke eigenschappen ophalen uit het bericht. Vervolgens kunt u de uitgepakte eigenschappen voor het routeren van het bericht naar een bestemming, of een tussenliggende eindpunt.

## <a name="try-it-out"></a>Probeer het
[Implementeren van een volledig operationeel logische app ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (GitHub voorbeeld) met behulp van de XML-functies in Azure Logic Apps.

## <a name="learn-more"></a>Meer informatie
[Meer informatie over het Enterprise-integratiepakket](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")
