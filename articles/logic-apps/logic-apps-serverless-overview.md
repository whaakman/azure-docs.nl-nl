---
title: Overzicht van Azure zonder server | Microsoft Docs
description: Krachtige oplossingen in de cloud maken zonder om na te denken over de infrastructuur.
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
ms.date: 03/30/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 5cc6837ed0b0f4467e48c736f5d596a51a799fae
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="overview-of-azure-serverless-with-functions-and-logic-apps"></a>Overzicht van Azure zonder server met functies en Logic Apps

Zonder server toepassingen bieden voordelen van een toename van de snelheid van de ontwikkeling van, vermindering van de vereiste code en eenvoud binnen de schaal.  In dit artikel gaat naar de andere kenmerken van zonder server oplossingen en Azure zonder server-producten.

## <a name="what-is-serverless"></a>Wat is zonder server?

Zonder server betekent niet dat er zijn geen servers: het betekent dat de ontwikkelaar heeft geen zorgen te hoeven maken over servers.  Een groot deel van de traditionele toepassingsontwikkeling is beantwoorden van vragen rond schalen, hosten en bewakingsoplossingen om te voldoen aan de vereisten van de toepassing.  Met zonder server, worden deze vragen afgehandeld als onderdeel van de oplossing.  Bovendien worden zonder server toepassingen gefactureerd op een plan op basis van verbruik.  Als de toepassing nooit gebruikt wordt, is nooit kosten met zich mee ontstaan.  Deze functies kunnen ontwikkelaars zich richten op het de zakelijke logica van de oplossing.

De core services in Azure rond zonder server zijn [Azure Functions](https://azure.microsoft.com/services/functions/) en [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).  Beide oplossingen Volg de bovenstaande beginselen en kunnen ontwikkelaars robuuste cloudtoepassingen met minimale code te bouwen.

## <a name="what-are-azure-functions"></a>Wat zijn de Azure Functions?

Azure Functions is een oplossing voor het eenvoudig uitvoeren van kleine stukjes code, ofwel ‘functies’, in de cloud. U hoeft alleen de code te schrijven die u op dat moment nodig hebt, zonder dat u een complete toepassing of de bijbehorende infrastructuur nodig hebt. Functies kunnen aanbrengen ontwikkelen nog efficiënter en kunt u de gewenste programmeertaal, zoals C#, F #, Node.js, Python of PHP. Betaalt alleen voor de tijd dat uw code wordt uitgevoerd en Azure schaalt naar behoefte.

Als u meteen aan de slag wilt met Azure Functions, kunt u beginnen met [Uw eerste Azure-functie maken](../azure-functions/functions-create-first-azure-function.md). Als u behoefte hebt aan meer technische informatie over Functions, raadpleegt u de [naslaginformatie voor ontwikkelaars](../azure-functions/functions-reference.md).

## <a name="what-are-azure-logic-apps"></a>Wat zijn Azure Logic Apps?

Logische Apps van Azure biedt een manier om te vereenvoudigen en implementeren van schaalbare integraties en werkstromen in de cloud. Het biedt een visuele ontwerper voor model en het automatiseren van uw proces als een reeks stappen een werkstroom wordt aangeroepen.  Er zijn [veel connectors](../connectors/apis-list.md) via cloud en on-premises services snel een app zonder server verbinding te maken met andere API's.  Een logic app begint met een trigger (zoals “Wanneer is een account toegevoegd aan Dynamics CRM”) en na het starten kunnen vele combinaties, acties, conversies en voorwaardelijke logica beginnen.  Logic Apps is een uitstekende keuze wanneer verschillende Azure-functies in een proces - organiseren, vooral wanneer het proces vereist interactie met een extern systeem of de API.

Om te beginnen met Logic Apps beginnen met [maken van uw eerste logische app](quickstart-create-first-logic-app-workflow.md).  Als u meer technische informatie over Logic Apps zoekt, raadpleegt u de [referentie voor ontwikkelaars](logic-apps-workflow-actions-triggers.md).

## <a name="how-can-i-build-and-deploy-serverless-applications-in-azure"></a>Hoe kan ik bouwen en implementeren zonder server-toepassingen in Azure?

Azure biedt een uitgebreide set hulpprogramma's voor ontwikkeling, implementatie en beheer van apps zonder server.  Apps kunnen worden gebouwd rechtstreeks in de Azure portal of met [tooling vanuit Visual Studio](logic-apps-serverless-get-started-vs.md).  Wanneer een toepassing ontwikkeld deze kan worden [onmiddellijk geïmplementeerd](logic-apps-create-deploy-template.md).  Azure biedt ook bewaking voor zonder server apps.  Deze bewaking toegankelijk zijn vanuit de Azure-portal via de API SDK's of met geïntegreerde tooling OMS en Application Insights.

## <a name="next-steps"></a>Volgende stappen

* [Het bouwen van een app zonder server in Visual Studio aan de slag](logic-apps-serverless-get-started-vs.md)
* [Maak een klant insights dashboard met zonder server](logic-apps-scenario-social-serverless.md)
* [Een implementatiesjabloon voor een logische app bouwen](logic-apps-create-deploy-template.md)