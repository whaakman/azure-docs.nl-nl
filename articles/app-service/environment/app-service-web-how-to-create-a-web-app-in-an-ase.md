---
title: Een web-app maken in een App Service Environment v1 - Azure
description: Informatie over het maken van web-apps en app-service-plannen in een App Service Environment v1
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: 983ba055-e9e4-495a-9342-fd3708dcc9ac
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 84a8a0687a1da0eaf7842a9bf6115c1d0f5d3fd1
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338138"
---
# <a name="create-a-web-app-in-an-app-service-environment-v1"></a>Een web-app maken in een App Service Environment v1

> [!NOTE]
> In dit artikel gaat over de App Service Environment v1.  Er is een nieuwere versie van de App Service Environment die gebruiksvriendelijker en wordt uitgevoerd op een krachtigere infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot App Service Environment](intro.md).
> 

## <a name="overview"></a>Overzicht
In deze zelfstudie leert u over het maken van web-apps en App Service-plannen een [App Service Environment v1](app-service-app-service-environment-intro.md) (ASE). 

> [!NOTE]
> Als u wilt meer informatie over het maken van een web-app, maar hoeft te doen in een App Service-omgeving, Zie [een .NET-web-app maken](../app-service-web-get-started-dotnet.md) of een van de gerelateerde zelfstudies voor andere talen en frameworks.
> 
> 

## <a name="prerequisites"></a>Vereisten
In deze zelfstudie wordt ervan uitgegaan dat u een App Service-omgeving hebt gemaakt. Als u dat nog niet hebt gedaan, Zie [maken van een App Service Environment](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Een webtoepassing maken
1. In de [Azure Portal](https://portal.azure.com/), klikt u op **een resource maken > Web en mobiel > Web-App**. 
   
    ![][1]
2. Selecteer uw abonnement.  
   
    Als u meerdere abonnementen hebt er rekening mee dat voor het maken van een app in uw App Service Environment, u hetzelfde abonnement die u hebt gebruikt moet bij het maken van de omgeving worden gebruikt. 
3. Selecteer of maak een resourcegroep.
   
    *Resourcegroepen* kunt u gerelateerde Azure-resources beheren als een eenheid en zijn nuttig bij het maken van *op rollen gebaseerd toegangsbeheer* regels (RBAC) voor uw apps. Zie voor meer informatie, [overzicht van Azure Resource Manager][ResourceGroups]. 
4. Selecteer of maak een App Service-plan.
   
    *App Service-plannen* beheerde sets met web-apps.  Normaal gesproken wanneer u prijzen selecteert, wordt de prijs toegepast op de App Service-plan in plaats van de afzonderlijke apps. In een as-omgeving betaalt u voor de compute-exemplaren die is toegewezen aan de as-omgeving in plaats van wat u hebt opgenomen met uw ASP.  Als u wilt opschalen van het aantal exemplaren van een web-app opschalen van de exemplaren van uw App Service-plan en heeft invloed op alle van de web-apps in het plan.  Sommige functies, zoals sitesleuven of VNET-integratie hebben ook hoeveelheidsbeperkingen binnen het abonnement.  Zie voor meer informatie, [overzicht van Azure App Service-plannen](../azure-web-sites-web-hosting-plans-in-depth-overview.md)
   
    U kunt de App Service-plannen in uw as-omgeving identificeren door te kijken op de locatie die wordt vermeld onder de naam van het.  
   
    ![][5]
   
    Als u gebruiken van een App Service-plan dat al in uw App Service-omgeving wilt bestaat, selecteert u dat abonnement. Als u maken van een nieuwe App Service-plan wilt, Zie de volgende sectie van deze zelfstudie, [een App Service-plan maken in een App Service Environment](#createplan).
5. Voer de naam in voor uw web-app en klik vervolgens op **maken**. 
   
    Als de as-omgeving gebruikmaakt van een extern VIP-adres de URL van een app in een as-omgeving is: [*sitename*]. [ *naam van uw App Service Environment*]. p.azurewebsites.net in plaats van [*sitename*]. azurewebsites.net
   
    Als de as-omgeving gebruikmaakt van een intern VIP-adres en de URL van een app in deze as-omgeving is: [*sitename*]. [ *subdomein opgegeven tijdens het maken van ASE*]   
    Nadat u uw ASP tijdens het maken van ASE selecteren ziet u het subdomein bijwerken hieronder **naam**

## <a name="createplan"></a> Maak een App Service-plan
Wanneer u een App Service-plan in een App Service-omgeving maakt, zijn uw keuzes worker anders als er geen gedeelde werknemers in een as-omgeving zijn.  De werknemers die u moet gebruiken, zijn de velden die zijn toegewezen aan de as-omgeving door de beheerder.  Dit betekent dat als u wilt een nieuw abonnement maakt, moet u hebt meer werknemers die zijn toegewezen aan uw groep met werkrollen as-omgeving dan het totale aantal exemplaren in al uw abonnementen al in die groep met werkrollen.  Als u niet voldoende werknemers in uw groep met werkrollen as-omgeving te maken van uw abonnement hebt, moet u contact op met de beheerder van uw as-omgeving aan om ze toegevoegd.

Een ander verschil met App Service-plannen die wordt gehost door een App Service Environment is het ontbreken van een keuze uit de prijscategorie.  Wanneer u een App Service-omgeving hebt u betaalt voor de rekenresources die worden gebruikt door het systeem en geen extra kosten in rekening gebracht voor de abonnementen in die omgeving.  Normaal gesproken wanneer u een App Service-plan maakt u een prijscategorie selecteren waarmee wordt bepaald hoe de facturering.  Een App Service Environment is in feite een persoonlijke locatie waar u inhoud kunt maken.  U betaalt voor de omgeving en niet voor het hosten van uw inhoud.

De volgende instructies laten zien hoe een App Service-plan maken tijdens het maken van een web-app, zoals wordt beschreven in de vorige sectie van de zelfstudie.

1. Klik op **nieuw** in de selectie plan gebruikersinterface en geef een naam voor uw abonnement net zoals u gewend buiten een as-omgeving.
2. Selecteer de as-omgeving die u wilt gebruiken van uw locatie kiezen.
   
    Omdat een App Service Environment in wezen de locatie van een persoonlijke implementatie is, ziet u onder locatie. 
   
    ![][2]
   
    Nadat de selectie van een as-omgeving in de kiezer voor locatie, het maken van de App Service-plan gebruikersinterface is bijgewerkt.  De locatie bevat nu de naam van het ASE-systeem en de regio deel uitmaakt van en de kiezer voor prijscategorie plan is vervangen door een worker-groep kiezen.  
   
    ![][3]

### <a name="selecting-a-worker-pool"></a>Een groep met werkrollen selecteren
Normaal zijn in Azure App Service en buiten een App Service-omgeving, er 3 compute-grootten die beschikbaar met de selectie van het prijsplan is een toegewezen zijn.  Op dezelfde manier, voor een as-omgeving kunt u maximaal 3 pools van werknemers te definiëren en geeft u de compute-grootte die wordt gebruikt voor deze groep met werkrollen.  Dit betekent dat voor tenants van de as-omgeving is dat in plaats van het selecteren van een abonnement met een compute-grootte voor uw App Service-plan kunt u selecteren wat heet een *groep met werkrollen*.  

De selectie van de pool worker-gebruikersinterface wordt gebruikt voor deze groep met werkrollen onder de naam van de grootte van de compute.  De beschikbare hoeveelheid verwijst naar het aantal compute-instanties zijn beschikbaar voor gebruik in die groep.  De totale groep mogelijk meer exemplaren dan dit aantal daadwerkelijk, maar deze waarde verwijst naar gewoon hoeveel zich niet in gebruik.  Als u nodig hebt om aan te passen raadpleegt u uw App Service-omgeving om toe te voegen meer rekenbronnen [configureren van uw App Service Environment](app-service-web-configure-an-app-service-environment.md).

![][4]

In dit voorbeeld ziet u slechts twee werknemersgroepen beschikbaar. Dat komt doordat de beheerder van de as-omgeving alleen hosts toegewezen in deze twee worker-groepen.  De derde zou worden weergegeven wanneer er virtuele machines erin is toegewezen.  

## <a name="after-web-app-creation"></a>Na het maken van web-app
Er zijn enkele overwegingen voor het uitvoeren van webtoepassingen en beheren van App Service-plannen in een as-omgeving die moet worden gehouden.  

Zoals eerder aangegeven, de eigenaar van de as-omgeving is verantwoordelijk voor de grootte van het systeem en als gevolg hiervan ze zijn ook verantwoordelijk om ervoor te zorgen dat er onvoldoende capaciteit is voor het hosten van de gewenste App Service-plannen. Als er geen beschikbare werkrollen, wordt het niet mogelijk om te maken van uw App Service-plan.  Dit is ook waar het omhoog schalen van uw web-app.  Als u meer exemplaren nodig hebt u wilt ophalen van uw App Service Environment-beheerder om toe te voegen meer werknemers.

Na het maken van uw web-app en App Service-plan is het een goed idee om omhoog te schalen.  In een as-omgeving moet u altijd ten minste 2 exemplaren van uw App Service-plan voor fouttolerantie voor uw apps.  Schalen van een App Service-plan in een as-omgeving is hetzelfde als normale via de gebruikersinterface van de App Service-plan.  Voor meer informatie over het schalen, [een web-app schalen in een App Service Environment](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

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
[AzurePowershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
