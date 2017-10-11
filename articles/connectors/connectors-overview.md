---
title: Overzicht van Logic Apps Connectors | Microsoft Docs
description: Overzicht van connectors die kunnen worden gebruikt in een logische app
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: ca8dab2e-9b69-4b1e-865d-1facd9f0cdac
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan
ms.openlocfilehash: 9cbb258ae9e32549669623e6824dd9b18fa1f68f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="using-connectors-in-a-logic-app"></a>Met behulp van connectors in een logische app
Connectors bieden snel toegang tot de gebeurtenissen, gegevens en acties services, protocollen en-platforms.  De volledige lijst van connectors die ondersteuning biedt voor logische Apps kunt [hier vinden](apis-list.md).  Connectors kunnen worden gebruikt als een trigger of een actie in een logische app, en mogelijk een geconfigureerde *verbinding* te gebruiken (bijvoorbeeld: een Twitter-account voor toegang tot of post een bericht namens jou te autoriseren).

## <a name="basics"></a>Basisbeginselen
Connectors zijn gehoste services die u toegang hebt tot als onderdeel van een logische app integreren met andere services, zoals Dynamics, Azure, Salesforce, [en meer](apis-list.md).  Ze zijn geïmplementeerd en beheerd door Microsoft, zodat u kunt uw werkstromen integratie met de schaal, doorvoer en beveiliging gezorgd voor opbouwen.  U kunt een connector toevoegt aan een logische app zoeken en selecteert u de actie van een connector of trigger onder **beheerde API's van Microsoft weergeven**.

![Actiemenu voor het selecteren van de trigger][1]

Elke connector actie of trigger heeft een set eigenschappen om te configureren.  Klik op de knoppen info voor meer informatie over actie of verwijst naar de bijbehorende documentatie [voor meer informatie](apis-list.md).

Als u integreren met een service of API die niet nog een connector wilt, kunt u ook uitbreiden logic apps via een [aangepaste connector](../logic-apps/logic-apps-create-api-app.md) of roept u rechtstreeks naar de service via een protocol zoals HTTP.

## <a name="triggers"></a>Triggers
Sommige verbindingslijnen hebben een trigger, wat betekent dat een gebeurtenis op basis van die connector wordt gestart van een logische app en wordt doorgegeven in de gegevens als onderdeel van de trigger.  Een trigger is altijd de eerste stap in een logische app.  Populaire triggers zijn bewerkingen zoals:

* Terugkeerpatroon - elk uur uitgevoerd
* Wanneer een HTTP-aanvraag is ontvangen
* Wanneer een item wordt toegevoegd aan een wachtrij
* Wanneer u een e-mail ontvangen

Sommige triggers het moment dat een gebeurtenis wordt uitgevoerd door een melding aan de logische app geactiveerd en worden andere moet u een interval dat is geconfigureerd op de logische app hoe vaak de service voor een gebeurtenis (maximaal elke 15 seconden controleert).  

Zodra een gebeurtenis is ontvangen, wordt de logische app uitgevoerd wordt geactiveerd en de acties in de werkstroom wordt gestart.  Ook is mogelijk toegang tot alle gegevens van de trigger in de hele werkstroom (bijvoorbeeld de trigger 'op een nieuwe tweet' geeft de tweet in de uitvoeren).

## <a name="actions"></a>Acties
De meeste connectors zijn een of meer acties die kunnen worden uitgevoerd als onderdeel van de werkstroom.  Acties zijn de stappen die hebben plaatsgevonden nadat de uitvoering van een trigger is gestart.  Toevoegen van een actie klikt u op de **nieuwe stap** knop en zoek naar de connector die u wilt gebruiken.  Zodra geselecteerd (en na het configureren van eventuele [verbindingen](#connections) die mogelijk vereist) ziet u de actie kaart die u kunt configureren.  U kunt gegevens uit de vorige stappen door te klikken op een van de tokens voor uitvoer selecteren of invoeren in een andere configuratie zo nodig.

![Een actie connector configureren][2]

## <a name="connections"></a>Verbindingen
De meeste connectors vereisen dat u configureert een *verbinding* voordat u de connector kunt gebruiken.  Een *verbinding* is een aanmelding of verbinding configuratie nodig voor toegang tot de connector.  Betekent aanmeldt bij de service (zoals Office 365, Salesforce of GitHub) wanneer uw toegangstoken kan worden versleuteld en veilig opgeslagen in een Azure geheime archief voor connectors die gebruikmaken van OAuth, maakt u een verbinding.  Andere connectors (zoals FTP- en SQL) vereist een verbinding met de configuratie zoals serveradres, gebruikersnaam en wachtwoord.  Deze configuratie Verbindingsdetails zijn eveneens versleuteld en veilig opgeslagen.  Verbindingen zijn toegang kunnen krijgen tot de service voor als de service worden toegestaan.  Voor Azure Active Directory-OAuth-verbindingen (zoals Office 365 en Dynamics) kunt blijven het toegangstoken voor onbepaalde tijd te vernieuwen.  Andere services kunnen limieten plaats op hoe lang we een token kunnen gebruiken zonder deze wordt vernieuwd.  Bepaalde acties zoals het wijzigen van een wachtwoord worden in het algemeen alle toegangstokens ongeldig.  

Verbindingen worden weergegeven en beheerd in Azure door te klikken op **Bladeren** en het selecteren van **API verbindingen**.  Van de resource-API-verbindingen kunt u weergeven, bewerken, bijwerken of opnieuw autoriseren alle verbindingen die u hebt gemaakt.

## <a name="next-steps"></a>Volgende stappen
* [Een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md)
* [Meer informatie over algemene gebruikt en voorbeelden van logic apps](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Aan de slag met enterprise integration-triggers en acties](../logic-apps/logic-apps-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png
