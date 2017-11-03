---
title: Een web-app maken in een App Service-omgeving v1
description: Informatie over het maken van web-apps en app-service-abonnementen in een App Service-omgeving v1
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 983ba055-e9e4-495a-9342-fd3708dcc9ac
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: b031807073313e9e093dbc7576ecfd3d2a970abe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-web-app-in-an-app-service-environment-v1"></a>Een web-app maken in een App Service-omgeving v1

> [!NOTE]
> In dit artikel gaat over de v1 App Service-omgeving.  Er is een nieuwere versie van de App Service-omgeving die eenvoudiger te gebruiken en wordt uitgevoerd op krachtiger infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot de App-serviceomgeving](intro.md).
> 

## <a name="overview"></a>Overzicht
Deze zelfstudie laat zien hoe u maakt de web-apps en App Service-plannen een [App Service-omgeving v1](app-service-app-service-environment-intro.md) (as-omgeving). 

> [!NOTE]
> Als u informatie over het maken van een web-app wilt, maar hoeft niet te doen in een App Service-omgeving, Zie [een .NET-web-app maken](../app-service-web-get-started-dotnet.md) of een van de gerelateerde zelfstudies voor andere talen en frameworks.
> 
> 

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie wordt ervan uitgegaan dat u een App Service-omgeving hebt gemaakt. Als u die nog niet hebt gedaan, Zie [maken van een App-serviceomgeving](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Een webtoepassing maken
1. In de [Azure Portal](https://portal.azure.com/), klikt u op **Nieuw > Web + mobiel > Web-App**. 
   
    ![][1]
2. Selecteer uw abonnement.  
   
    Als u meerdere abonnementen, houd er rekening mee dat als u wilt een app maken in uw App Service-omgeving, u hetzelfde abonnement die u hebt gebruikt moet bij het maken van de omgeving gebruiken. 
3. Selecteer of maak een resourcegroep.
   
    *Resourcegroepen* kunt u Azure gerelateerde resources beheren als één eenheid en zijn handig bij het maken van *toegangsbeheer op basis van rollen* regels (RBAC) voor uw apps. Zie voor meer informatie [overzicht van Azure Resource Manager][ResourceGroups]. 
4. Selecteer of maak een App Service-abonnement.
   
    *App Service-plannen* beheerde sets met web-apps.  Normaal gesproken wanneer u prijzen selecteert, wordt de prijs toegepast op het App Service-abonnement in plaats van de afzonderlijke apps. In een as-omgeving betaalt u voor de compute-exemplaren die is toegewezen aan de as-omgeving in plaats van de lijst met uw ASP.  Als u wilt schalen van het aantal exemplaren van een web-app die u de exemplaren van uw App Service opschalen plan en deze van invloed op alle van de web-apps in plan.  Sommige functies zoals site sleuven of VNET integratie hebben ook hoeveelheidsbeperkingen binnen het plan.  Zie voor meer informatie [overzicht van Azure App Service-plannen](../azure-web-sites-web-hosting-plans-in-depth-overview.md)
   
    De App Service-plannen kunt in uw as-omgeving u identificeren door te kijken naar de locatie die wordt vermeld onder de naam van het abonnement.  
   
    ![][5]
   
    Als u een App Service-abonnement dat al in uw App Service-omgeving voorkomt gebruiken wilt, selecteert u plan. Als u maken van een nieuw App Service-plan wilt, raadpleegt u de volgende sectie van deze zelfstudie [maken van een App Service-abonnement in een App-serviceomgeving](#createplan).
5. Geef de naam voor uw web-app en klik vervolgens op **maken**. 
   
    Als uw as-omgeving gebruikmaakt van een externe VIP de URL van een app in een as-omgeving is: [*sitename*]. [ *naam van uw App Service-omgeving*]. p.azurewebsites.net in plaats van [*sitename*]. azurewebsites.net
   
    Als uw as-omgeving gebruikmaakt van een interne VIP en vervolgens de URL van een app in dat as-omgeving is: [*sitename*]. [ *subdomein opgegeven tijdens het maken van de as-omgeving*]   
    Nadat u uw ASP tijdens het maken van de as-omgeving selecteren, ziet u het subdomein bijwerken hieronder **naam**

## <a name="createplan"></a>Een App Service-abonnement maken
Wanneer u een App Service-abonnement in een App Service-omgeving maakt, zijn uw keuzes worker verschillend omdat er geen gedeelde werknemers zich in een as-omgeving.  De werknemers die u moet gebruiken zijn degene die zijn toegewezen aan de as-omgeving door de beheerder.  Dit betekent dat voor het maken van een nieuw plan hoeft te hebben meer werknemers die zijn toegewezen aan uw as-omgeving werknemersgroep dan het totale aantal exemplaren voor alle uw plannen al uit de betreffende worker-groep.  Als u geen voldoende werknemers in uw werknemersgroep as-omgeving om uw plan te maken, moet u samen met uw beheerder as-omgeving aan om ze toegevoegd.

Een ander verschil met App Service-abonnementen die worden gehost door een App Service-omgeving is het ontbreken van de prijzen voor selectie.  Wanneer u een App Service-omgeving hebt u betaalt voor de rekenresources die door het systeem gebruikt en bevatten geen extra kosten voor de plannen in deze omgeving.  Normaal gesproken wanneer u een App Service-abonnement maakt u een prijscategorie selecteren waarmee wordt bepaald van de facturering.  Een App Service-omgeving is in wezen een persoonlijke locatie waarin u inhoud kunt maken.  U betaalt voor de omgeving en niet voor het hosten van uw inhoud.

De volgende instructies hoe App Service-abonnement maken tijdens het maken van een web-app, zoals wordt beschreven in de vorige sectie van de zelfstudie.

1. Klik op **nieuw** in de selectie plan gebruikersinterface en geef een naam voor uw abonnement net zoals u dat gewend buiten een as-omgeving bent.
2. Selecteer de as-omgeving die u wilt gebruiken van uw locatie kiezen.
   
    Omdat een App Service-omgeving in wezen een locatie van de persoonlijke implementatie is, ziet u onder locatie. 
   
    ![][2]
   
    Na de selectie van een as in de kiezer voor locatie-omgeving, het maken van de App Service-plan gebruikersinterface is bijgewerkt.  De locatie ziet nu de naam van het systeem as-omgeving en de regio is in en de prijscategorie plan objectkiezer is vervangen door een objectkiezer worker-groep.  
   
    ![][3]

### <a name="selecting-a-worker-pool"></a>Een werknemersgroep selecteren
Normaal gesproken zijn in Azure App Service en buiten een App Service-omgeving, er 3 compute-grootten die beschikbaar met de selectie van een speciale prijs-plan zijn.  Op soortgelijke wijze voor een as-omgeving definiëren maximaal 3 pools van werknemers en geef de compute-grootte die wordt gebruikt voor die worker-groep.  Wat dit betekent dat voor tenants van de as-omgeving is dat in plaats daarvan van het selecteren van een prijsstelling met compute-grootte voor uw App Service-abonnement kunt u selecteren wat heet een *werknemersgroep*.  

De selectie van de groep worker UI toont de compute-grootte gebruikt voor die werknemersgroep onder de naam.  De beschikbare hoeveelheid verwijst naar hoeveel compute exemplaren zijn beschikbaar voor gebruik in die groep.  De totale groep mogelijk meer exemplaren dan het aantal daadwerkelijk hebben, maar deze waarde verwijst naar gewoon hoeveel zich niet in gebruik.  Als u wilt aanpassen raadpleegt u uw App Service-omgeving om toe te voegen meer bronnen berekenen [configureren van uw App Service-omgeving](app-service-web-configure-an-app-service-environment.md).

![][4]

In dit voorbeeld ziet u slechts twee werknemersgroepen die beschikbaar is. Dat komt doordat de beheerder van de as-omgeving toegewezen alleen hosts in deze twee werknemersgroepen.  Het derde zou weergegeven wanneer er virtuele machines die zijn toegewezen in de App.  

## <a name="after-web-app-creation"></a>Na het maken van web-app
Er zijn enkele overwegingen voor het uitvoeren van de web-apps en het beheren van App Service-abonnementen in een as-omgeving die moet worden gehouden.  

Zoals eerder opgemerkt, de eigenaar van de as-omgeving is verantwoordelijk voor de grootte van het systeem en als gevolg hiervan zijn ook verantwoordelijk voor het garanderen dat er voldoende capaciteit voor het hosten van de gewenste App Service-abonnementen is. Als er geen beschikbare werknemers, wordt het niet mogelijk om uw App Service-plan te maken.  Dit is ook waar het schalen van uw web-app.  Als u meer exemplaren nodig zou hebt u uw App Service-omgeving-beheerder om toe te voegen meer werknemers ophalen.

Na het maken van uw web-app en App Service-abonnement is het een goed idee om omhoog te schalen.  In een as-omgeving moet u altijd ten minste 2 exemplaren van uw App Service-plan voor fouttolerantie voor uw apps.  Schalen van een App Service-abonnement in een as-omgeving, is hetzelfde als normale via de gebruikersinterface van de App Service-abonnement.  Voor meer informatie over het schalen [schalen van een web-app in een App-serviceomgeving](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[Appserviceplans]: ../azure-web-sites-web-hosting-plans-in-depth-overview.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[HowtoScale]: app-service-web-scale-a-web-app-in-an-app-service-environment.md
[HowtoConfigureASE]: app-service-web-configure-an-app-service-environment.md
[ResourceGroups]: ../../azure-resource-manager/resource-group-overview.md
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
