---
title: Schalen van een App in App Service-omgeving
description: Schalen van een app in App Service-omgeving
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: jimbe
ms.assetid: 78eb1e49-4fcd-49e7-b3c7-f1906f0f22e3
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: ccompy
ms.openlocfilehash: d04a5fce920dae25507cdf2f64832574e24c51dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="scaling-apps-in-an-app-service-environment"></a>Apps schalen in een App Service-omgeving
In de Azure App Service zijn er normaal gesproken drie dingen die kunt schalen:

* plan prijzen
* de grootte van de werknemer 
* aantal exemplaren.

In een as-omgeving hoeft niet te selecteren of de prijsstelling wijzigen.  Wat betreft mogelijkheden is het al op een Premium-prijscategorie mogelijkheid niveau.  

Met betrekking tot de grootte van de werknemer, kan de beheerder van de as-omgeving de grootte van de compute-bron moet worden gebruikt voor elke worker-groep toewijzen.  Dit betekent dat u kunt Worker-groep 1 hebben met P4 rekenresources en 2 voor Worker-groep met P1 rekenresources indien gewenst.  Ze hoeft niet te worden in volgorde van de grootte.  Zie voor meer informatie om de grootte en de prijscategorie het document hier [prijzen van Azure App Service][AppServicePricing].  Hierbij blijft de schalingsopties voor web-apps en App Service-abonnementen in een App-serviceomgeving moet:

* selectie van Worker-groep
* aantal exemplaren

Het wijzigen van een item wordt gedaan door de juiste gebruikersinterface weergegeven voor de as-omgeving App Service-abonnementen gehoste.  

![][1]

U kunt geen uw ASP dan het aantal beschikbare rekenresources in de worker-groep die de ASP is in opschalen.  Als u resources in die groep worker moet berekenen die u wilt ophalen van de as-omgeving-beheerder om toe te voegen.  Voor informatie over uw as-omgeving opnieuw te configureren, lees de informatie hier: [het configureren van een App Service-omgeving][HowtoConfigureASE].  U kunt ook om te profiteren van de as-omgeving voor automatisch schalen functies toevoegen capaciteit op basis van schema of metrische gegevens.  Voor meer informatie over het configureren van automatisch schalen voor de as-omgeving omgeving zelf Zie [automatisch schalen configureren voor een App-serviceomgeving][ASEAutoscale].

Kunt u meerdere app service-abonnementen met behulp van rekenresources vanaf verschillende werknemersgroepen maken of kunt u dezelfde worker-groep.  Bijvoorbeeld als er (10) beschikbare rekenresources in Worker-groep 1, kunt u een app service-abonnement met behulp van rekenresources (6) maken en een tweede app service-plan dat wordt gebruikt (4) de bronnen te berekenen.

### <a name="scaling-the-number-of-instances"></a>Het aantal exemplaren van schaal
Wanneer u eerst uw web-app in App Service-omgeving maken wordt gestart 1 exemplaar.  U kunt vervolgens uitschalen naar extra exemplaren voor extra rekenresources voor uw app.   

Als u uw as-omgeving heeft onvoldoende capaciteit vervolgens is dit heel eenvoudig.  Gaat u naar uw App Service-Plan waarin de sites die u wilt opschalen en selecteer schaal.  Hiermee opent u de kunt u handmatig de schaal voor de ASP instellen of regels voor automatisch schalen configureren voor de ASP-gebruikersinterface.  Voor het handmatig schalen van uw app gewoon ingesteld ***schalen door*** naar ***aantal instanties dat ik handmatig invoeren***.  Sleep de schuifregelaar naar de gewenste hoeveelheid of voer deze in het vak naast de schuifregelaar hiervandaan.  

![][2] 

De regels voor automatisch schalen voor een ASP in een as-omgeving werken op dezelfde manier als normaal.  U kunt selecteren ***CPU-Percentage*** onder ***schalen door*** en automatisch schalen regels te maken voor de ASP op basis van CPU-Percentage, of u complexere regels die gebruikmaken van kunt maken ***regels voor planning en prestaties*** .  Voor meer informatie over het configureren van automatisch schalen gebruikt u de handleiding hier [schalen van een app in Azure App Service][AppScale]. 

### <a name="worker-pool-selection"></a>Selectie van Worker-groep
Zoals eerder opgemerkt, wordt de selectie worker-groep is toegankelijk vanuit de gebruikersinterface voor ASP.  Open de blade voor de ASP-code die u wilt schalen en selecteer worker-groep.  U ziet alle worker-groepen die u hebt geconfigureerd in uw App Service-omgeving.  Als er slechts één werknemersgroep vervolgens ziet alleen u de één groep weergegeven.  Als u welke worker-groep die de ASP is in, selecteert u gewoon de worker-groep die u wilt dat uw App Service-Plan om naar te verplaatsen.  

![][3]

Voordat u uw ASP in een werknemersgroep verplaatst naar een andere is het belangrijk om ervoor te zorgen dat er voldoende capaciteit voor de ASP.  In de lijst met werknemersgroepen, niet alleen wordt de naam van de werknemer toepassingen vermeld maar u kunt ook zien hoeveel werknemers beschikbaar zijn in die worker-groep.  Zorg ervoor dat er voldoende exemplaren beschikbaar zijn voor uw App Service-Plan bevatten.  Als u meer bronnen in de worker-groep die u verplaatsen wilt naar berekenen moet, krijgt u vervolgens de beheerder as-omgeving om toe te voegen.  

> [!NOTE]
> Het verplaatsen van dat een ASP in een werknemersgroep zullen koude start van de ASP-apps.  Hierdoor kan aanvragen langzamer uitgevoerd als uw app koude gestart op de nieuwe rekenresources is.  De koude start kan worden voorkomen met behulp van de [toepassing warme up mogelijkheid] [ AppWarmup] in Azure App Service.  De initialisatie van de toepassing module beschreven in het artikel werkt ook voor koude startprocedures omdat het initialisatieproces ook aangeroepen wordt wanneer apps koude gestart op de nieuwe rekenresources zijn. 
> 
> 

## <a name="getting-started"></a>Aan de slag
Om aan de slag met App Service-omgevingen, Zie [hoe te maken van een App Service-omgeving][HowtoCreateASE]

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[ScaleWebapp]: ../web-sites-scale.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[HowtoConfigureASE]: app-service-web-configure-an-app-service-environment.md
[CreateWebappinASE]: app-service-web-how-to-create-a-web-app-in-an-ase.md
[Appserviceplans]: ../azure-web-sites-web-hosting-plans-in-depth-overview.md
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[ASEAutoscale]: app-service-environment-auto-scale.md
[AppScale]: ../web-sites-scale.md
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
