---
title: Overzicht van Azure zonder server | Microsoft Docs
description: Meer informatie over het maken van krachtige oplossingen in de cloud zonder dat u over de infrastructuur
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: jeffhollan
ms.author: jehollan
ms.reviewer: klam, estfan, LADocs
ms.custom: vs-azure
ms.topic: article
ms.date: 03/30/2017
ms.openlocfilehash: 9cd1946d8fa670764bcc95b82298d07cc70417bd
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57191320"
---
# <a name="overview-azure-serverless-with-azure-logic-apps-and-azure-functions"></a>Overzicht: Azure Serverloos met Azure Logic Apps en Azure Functions

[Serverloze](https://azure.microsoft.com/solutions/serverless/) toepassingen bieden voordelen van een toename van de snelheid van de ontwikkeling, verlaging van de code vereist en eenvoud schaal.  In dit artikel gaat in de verschillende kenmerken van serverloze oplossingen en serverloze oplossingen van Azure.

## <a name="what-is-serverless"></a>Wat is serverloze?

Zonder server betekent niet dat er zijn geen servers - alleen dat de ontwikkelaar heeft geen zorgen te hoeven maken over servers.  Een groot deel van de ontwikkeling van traditionele toepassingen met het beantwoorden van vragen over schalen en bewakingsoplossingen om te voldoen aan de eisen van de toepassing die als host fungeert.  Met zonder server, worden deze vragen afgehandeld als onderdeel van de oplossing.  Bovendien Serverloze toepassingen worden in rekening gebracht op een abonnement op basis van gebruik.  Als de toepassing nooit gebruikt wordt, is nooit een toeslag berekend.  Deze functies kunnen ontwikkelaars zich kunnen richten uitsluitend op de bedrijfslogica van de oplossing.

De core-services in Azure om zonder server zijn [Azure Functions](https://azure.microsoft.com/services/functions/) en [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).  Beide van deze oplossingen bovenstaande principes en kunnen ontwikkelaars krachtige cloudtoepassingen met minimale code te bouwen.

## <a name="what-are-azure-functions"></a>Wat zijn Azure Functions?

Azure Functions is een oplossing voor het eenvoudig uitvoeren van kleine stukjes code, ofwel ‘functies’, in de cloud. U hoeft alleen de code te schrijven die u op dat moment nodig hebt, zonder dat u een complete toepassing of de bijbehorende infrastructuur nodig hebt. Functions kunnen het ontwikkeltraject nog efficiënter en kunt u uw ontwikkeling taal naar keuze, zoals C#, F#, Node.js, Python of PHP. Betaal alleen voor de tijd dat uw code wordt uitgevoerd en Azure kan worden geschaald naar behoefte.

Als u meteen aan de slag wilt met Azure Functions, kunt u beginnen met [Uw eerste Azure-functie maken](../azure-functions/functions-create-first-azure-function.md). Als u behoefte hebt aan meer technische informatie over Functions, raadpleegt u de [naslaginformatie voor ontwikkelaars](../azure-functions/functions-reference.md).

## <a name="what-are-azure-logic-apps"></a>Wat zijn Azure Logic Apps?

Met Azure Logic Apps biedt een manier om te vereenvoudigen en implementeren van schaalbare integraties en werkstromen in de cloud. Het biedt een visuele ontwerper om te modelleren en automatiseren als een reeks stappen een werkstroom met de naam.  Er zijn [veel connectors](../connectors/apis-list.md) tussen cloud en on-premises services snel een serverloze app verbinden met andere API's.  Een logic app begint met een trigger (zoals “Wanneer is een account toegevoegd aan Dynamics CRM”) en na het starten kunnen vele combinaties, acties, conversies en voorwaardelijke logica beginnen.  Logic Apps is een goede keuze wanneer verschillende Azure-functies in een proces - organiseren met name wanneer het proces vereist interactie met een extern systeem of de API.

Als u wilt aan de slag met Logic Apps, beginnen met [het maken van uw eerste logische app](quickstart-create-first-logic-app-workflow.md).  Als u meer technische informatie over Logic Apps zoekt, raadpleegt u de [referentie voor ontwikkelaars](logic-apps-workflow-actions-triggers.md).

## <a name="how-can-i-build-and-deploy-serverless-applications-in-azure"></a>Hoe kan ik bouw en implementeer Serverloze toepassingen in Azure?

Azure biedt een uitgebreide set hulpprogramma's voor ontwikkeling, implementatie en het beheer van Serverloze apps.  Apps kunnen worden gebouwd, rechtstreeks in Azure portal of met [vanuit Visual Studio-hulpmiddelen](logic-apps-serverless-get-started-vs.md).  Wanneer een toepassing is ontwikkeld kan zijn [onmiddellijk geïmplementeerd](logic-apps-create-deploy-template.md).  Azure biedt ook de bewaking voor serverloze apps.  Deze bewaking zijn toegankelijk vanuit Azure portal, via de API of SDK's of met geïntegreerde hulpprogramma's voor Azure Monitor-logboeken en Application Insights.

## <a name="next-steps"></a>Volgende stappen

* [Begin met het bouwen van een Serverloze app in Visual Studio](logic-apps-serverless-get-started-vs.md)
* [Een customer insights-dashboard maken met de zonder servers](logic-apps-scenario-social-serverless.md)
* [Een sjabloon voor de implementatie van een logische app bouwen](logic-apps-create-deploy-template.md)
