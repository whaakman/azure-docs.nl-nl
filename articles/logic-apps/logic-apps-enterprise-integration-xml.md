---
title: XML-berichten voor B2B-bedrijfsintegratie - Azure Logic Apps | Microsoft Docs
description: Verwerken, valideren, transformeren en verrijken van XML-berichten voor B2B-oplossingen in Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 47672dc4-1caa-44e5-b8cb-68ec3a76b7dc
ms.date: 02/27/2017
ms.openlocfilehash: a75ac9773072423c13eef85ecad29c632c13d024
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56873250"
---
# <a name="xml-messages-and-flat-files-in-azure-logic-apps-with-enterprise-integration-pack"></a>XML-berichten en platte bestanden in Azure Logic Apps met Enterprise Integration Pack

Met logic apps, hebt u de mogelijkheid om te verwerken van XML-berichten verzenden en ontvangen. Deze functie is opgenomen in de Enterprise Integration Pack. Voor gebruikers met een achtergrond BizTalk Server biedt de Enterprise Integration Pack vergelijkbare mogelijkheden te transformeren en valideren van berichten, werken met platte bestanden en zelfs XPath gebruiken om te verrijken of specifieke eigenschappen van een bericht ophalen. 

Voor gebruikers die niet bekend bent met deze ruimte, vouw deze functies uit hoe u berichten verwerken binnen de werkstroom. Bijvoorbeeld, als u zich in een scenario voor het business-to-business en met specifieke XML-schema's, werken kunt u het Enterprise Integration Pack gebruiken voor het verbeteren van hoe uw bedrijf deze berichten verwerkt. 

Het Enterprise Integration Pack bevat: 

* [XML-validatie](logic-apps-enterprise-integration-xml-validation.md "meer informatie over XML-bericht validatie") -valideren van een binnenkomende of uitgaande XML-bericht op basis van een specifieke schema.
* [XML-transformatie](../logic-apps/logic-apps-enterprise-integration-transform.md "meer informatie over XML-bericht transformaties en maps") - converteren of aanpassen van een XML-bericht op basis van uw vereisten of de vereisten van een partner.
* [Bestandscodering en ontsleuteling van plat bestand met de platte](logic-apps-enterprise-integration-flatfile.md "meer informatie over de platte bestandsindeling codering/decodering") - codering of een plat bestand decoderen. Bijvoorbeeld, SAP accepteert en verzendt IDOC-bestanden in de platte bestandsindeling. Veel platforms met integratie van maken XML-berichten, met inbegrip van logische Apps. U kunt dus een logische app die gebruikmaakt van het coderingsprogramma plat bestand "" XML-bestanden converteren naar platte bestanden maken. 
* [XPath](https://msdn.microsoft.com/library/mt643789.aspx) - verrijken van een bericht en specifieke eigenschappen onttrekken aan het bericht. U kunt vervolgens de uitgepakte eigenschappen gebruiken voor het routeren van het bericht naar een locatie, of een tussenliggende eindpunt.

## <a name="try-it-out"></a>Uitproberen
[Een volledig operationeel logische app implementeren](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) (voorbeeld van GitHub) met behulp van de XML-functies in Azure Logic Apps.

## <a name="learn-more"></a>Meer informatie
[Meer informatie over het Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")
