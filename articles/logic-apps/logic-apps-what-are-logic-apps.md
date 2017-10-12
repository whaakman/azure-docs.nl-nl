---
title: Verbinding maken met apps en gegevens integreren met werkstromen - Azure Logic Apps | Microsoft Docs
description: Maak werkstromen en automatiseer processen door met apps verbinding te maken en gegevens te integreren met Azure Logic Apps.
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 07765c05-72a6-4169-a8ab-f6420bfbaf07
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/23/2017
ms.author: klam
ms.openlocfilehash: 59d35852d6c703f3c96089a8bf426b57660441a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-are-logic-apps"></a>Wat zijn Logic Apps?
Logic Apps biedt een manier om schaalbare integraties en workflows in de cloud te vereenvoudigen en te implementeren. Het biedt een visuele designer om uw proces te modelleren en te automatiseren als een reeks stappen bekend als een werkstroom.  Er zijn [vele connectoren](../connectors/apis-list.md) in de cloud en on-premises om snel te integreren over services en protocollen.  Een logic app begint met een trigger (zoals 'Wanneer een account is toegevoegd aan Dynamics CRM') en na het starten kunnen vele combinaties van acties, conversies en voorwaardelijke logica beginnen.

De voordelen van het gebruik van Logic Apps omvatten het volgende:  

* Tijd besparen door complexe processen met eenvoudig te begrijpen ontwerpfuncties ontwerpen
* Patronen en werkstromen naadloos implementeren, dat zou met code anders moeilijk zijn
* Snel aan de slag met voorbeelden
* Uw logic app aanpassen met uw eigen API’s, code en acties
* Verbind en synchroniseer ongelijksoortige systemen on-premises verspreid en in de cloud
* Opbouwen van BizTalk-server, API Management, Azure Functions en Azure Service Bus met eersteklas integratieondersteuning

Logic Apps is een volledig beheerd iPaaS (integration Platform as a Service) waarmee gebruikers zich geen zorgen hoeven te maken over het voorzien van hosting, schaalbaarheid, beschikbaarheid en beheer.  In Logic Apps wordt de automatisch geschaald op basis van vraag.

![App-ontwerper voor stromen](media/logic-apps-what-are-logic-apps/LogicAppCapture2.png)

Zoals eerder vermeld kunt u met Logic Apps bedrijfsprocessen automatiseren. Hieronder vindt u enkele voorbeelden:  

* Bestanden geüpload naar een FTP-server verplaatsen naar Azure Storage
* Proces- en routeorders verspreid over de cloud en on-premises systemen
* Bewaken van alle tweets over een bepaald onderwerp, het gevoel analyseren en meldingen en taken maken voor items die moeten worden opgevolgd.

Dergelijke scenario's kunnen worden geconfigureerd via de visuele ontwerper, zonder dat hiervoor ook maar één regel code hoeft te worden geschreven. Ga nu aan de slag met het [bouwen van een logische app][create].  Na het schrijven - kan een logische app[snel worden geïmplementeerd en opnieuw geconfigureerd](../logic-apps/logic-apps-create-deploy-template.md) over verschillende omgevingen en regio’s.

## <a name="why-logic-apps"></a>Waarom Logic Apps?
Logic App brengt snelheid en schaalbaarheid naar de integratieruimte van de onderneming.  Het gebruiksgemak van de ontwerper, de diversiteit van beschikbare triggers en acties en krachtige beheerstools maken het centraliseren van uw API’s eenvoudiger dan ooit.  Aangezien bedrijven steeds meer streven naar digitalisering, kunt u met Logic Apps oude en ultramoderne systemen met elkaar verbinden.

Bovendien kunt u met onze [Enterprise Integration Account][biztalk] schalen naar volgroeide integratiescenario's met de kracht van [XML-messaging][xml], [handelspartnerbeheer][tpm] en meer.

* **Eenvoudig te gebruiken ontwerptools** - Logic Apps kunnen end-to-end in de browser of met andere Visual Studio-tools worden ontworpen. Begin met een trigger - van een eenvoudig schema tot wanneer een GitHub-probleem ontstaat. Vervolgens kunt u een willekeurig aantal acties indelen met behulp van de uitgebreide galerie met connectors.
* **API’s gemakkelijk verbinden** - Zelfs compositietaken die gemakkelijk te beschrijven zijn, zijn moeilijk te integreren in code. Logic Apps maakt het gemakkelijk om ongelijksoortige systemen met elkaar te verbinden. Wilt u uw marketingoplossing in de cloud verbinden met uw on-premises factureringssysteem? Wilt u messaging centraliseren over API’s en systemen met een Enterprise Service Bus? De snelste en betrouwbaarste manier om deze problemen op te lossen, is door gebruik te maken van Logic Apps.
* **Snel aan de slag met behulp van sjablonen** - Om u op weg te helpen, is er een [galerie met sjablonen][templates] waarmee u snel enkele algemene oplossingen kunt maken. Van geavanceerde B2B-oplossingen zo eenvoudig als SaaS-connectiviteit en zelfs enkele gewoon “voor de lol” - de galerie is de snelste manier om an de slag te gaan met de kracht van Logic Apps.
* **Ingebakken uitbreidbaarheid** - Ziet u niet de connector die u nodig hebt? Logic Apps is ontworpen om te werken met uw eigen API’s en code; u kunt gemakkelijk uw eigen API-app maken om te gebruiken als aangepaste connector of beroep doen op een [Azure Function](https://functions.azure.com) om exclusieve snippets code on-demand uit te voeren. 
* **Echte paardenkracht bij integratie** - Start eenvoudig en groei wanneer dat nodig is. Met Logic Apps kunt u de kracht van BizTalk, de toonaangevende integratieoplossing van Microsoft, gebruiken om integratieprofessionals de oplossingen te laten bouwen die ze nodig hebben. Meer informatie over het [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Concept van Logic Apps
In het onderstaande overzicht ziet u enkele belangrijke onderdelen van Logic Apps. 

* **Werkstroom** - Met Logic Apps kunt u uw bedrijfsprocessen grafisch weergeven als een reeks stappen of als een werkstroom.
* **Beheerde connectoren** - Uw logic apps vragen toegang tot data en service. Beheerde connectoren zijn specifiek gemaakt om u te helpen bij het maken van verbinding en werken met uw data. Raadpleeg de lijst van connectoren die nu beschikbaar zijn in [beheerde connectoren][managedapis].
* **Triggers** - Sommige Beheerde Connectoren kunnen ook werken als trigger. Met een trigger wordt een nieuw exemplaar van een werkstroom gestart op basis van een bepaalde gebeurtenis, zoals de aankomst van een e-mail of een wijziging in uw Azure Storage-account.
* **Acties** - Elke stap na de trigger in een werkstroom wordt een actie genoemd. Elke actie verwijst normaal naar een bewerking op uw beheerde connector of aangepaste API-apps.
* **Enterprise Integration Pack** - Voor meer geavanceerde integratiescenario’s omvat Logic Apps functies van BizTalk. BizTalk is Microsoft’s toonaangevende integratieplatform. De Enterprise Integration Pack-connectoren zorgen ervoor dat u gemakkelijk validering, transpformatie en meer kunt opnemen in uw Logic App-workflows.

## <a name="getting-started"></a>Aan de slag
* Als u aan de slag wilt met Logic Apps, volgt u de zelfstudie [Een logische app maken][create].  
* [Algemene voorbeelden en scenario's weergeven](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Met Logic Apps kunt u bedrijfsprocessen automatiseren](http://channel9.msdn.com/Events/Build/2016/T694) 
* [Meer informatie over hoe u Logic Apps kunt integreren in uw systemen](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: logic-apps-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-web-overview.md
[create]: logic-apps-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: logic-apps-enterprise-integration-accounts.md
[xml]: logic-apps-enterprise-integration-b2b.md
[templates]: logic-apps-use-logic-app-templates.md
