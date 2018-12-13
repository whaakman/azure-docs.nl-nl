---
title: Een App schalen in een App Service Environment - Azure
description: Een app schalen in een App Service Environment
services: app-service
documentationcenter: ''
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
ms.custom: seodec18
ms.openlocfilehash: c94e7f761dcbc7e05965e359d7f9a864335c9c6b
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269342"
---
# <a name="scaling-apps-in-an-app-service-environment"></a>Apps schalen in een App Service-omgeving
Er zijn gewoonlijk drie dingen die u kunt schalen in Azure App Service:

* Het prijsplan
* grootte van de werknemer 
* het aantal exemplaren.

In een as-omgeving is er niet nodig om te selecteren of de prijsstelling wijzigen.  Wat betreft mogelijkheden is het al op een Premium capaciteit prijsniveau.  

De ASE-beheerder kan de grootte van de compute-resource moet worden gebruikt voor elke workergroep toewijzen met betrekking tot de worker-grootten.  Dat betekent dat u kunt de Worker-groep 1 met P4 hebben compute-resources en 2 voor Worker-groep met P1-rekenresources, indien gewenst.  Ze hoeven niet te zijn in volgorde van de grootte.  Zie voor meer informatie over de grootten en de prijzen het document hier [prijzen voor Azure App Service][AppServicePricing].  Dit blijft de schaalopties voor web-apps en App Service-abonnementen in een App Service-omgeving worden:

* selectie van Worker-groep
* aantal exemplaren

Wijzigen van een item wordt gedaan door de juiste gebruikersinterface die wordt weergegeven voor de as-omgeving die worden gehost App Service-abonnementen.  

![][1]

U kan niet schalen van uw ASP dan het aantal beschikbare compute-resources in de groep met werkrollen waarin uw ASP zich bevindt.  Als u resources in die groep met werkrollen moet berekenen die u wilt ophalen van de ASE-beheerder om toe te voegen.  Uw ASE Lees de informatie hier voor informatie om opnieuw te configureren: [Het configureren van een App Service environment][HowtoConfigureASE].  U kunt ook om te profiteren van de functies van de as-omgeving voor automatisch schalen om toe te voegen capaciteit op basis van planning of metrische gegevens.  Voor meer informatie over het configureren van automatisch schalen voor de as-omgeving omgeving zelf Zie [voor automatisch schalen configureren voor een App Service Environment][ASEAutoscale].

Kunt u meerdere app service-plannen met behulp van de compute-resources uit verschillende worker-groepen maken of kunt u de dezelfde groep met werkrollen.  Bijvoorbeeld als u hebt (10) beschikbare compute-resources in Worker-groep 1, kunt u een app service-plan met behulp van rekenresources (6) maken en een tweede appservice-plan dat wordt gebruikt (4) compute-resources.

### <a name="scaling-the-number-of-instances"></a>Het aantal instanties schalen
Wanneer u eerst uw web-app in een App Service Environment maken is het begint al met 1 exemplaar.  U kunt vervolgens uitschalen naar extra exemplaren voor aanvullende rekenbronnen voor uw app.   

Als de as-omgeving onvoldoende capaciteit heeft vervolgens is dit heel eenvoudig.  U gaat u naar uw App Service-Plan waarin de sites die u wilt opschalen en schaal selecteren.  Hiermee opent u de gebruikersinterface kunt u handmatig de schaal instellen voor de ASP of regels voor automatisch schalen configureren voor de ASP.  Instellen voor het handmatig schalen van uw app gewoon ***schalen door*** naar ***het aantal instanties dat ik handmatig heb***.  Sleep de schuifregelaar naar de gewenste hoeveelheid of voer deze in het vak naast de schuifregelaar hier.  

![][2] 

De regels voor automatisch schalen voor een ASP in een as-omgeving werken op dezelfde manier als normaal.  U kunt selecteren ***CPU-Percentage*** onder ***schalen door*** en regels voor automatisch schalen te maken voor de ASP op basis van CPU-Percentage of u complexere regels met behulp van kunt maken ***regels voor planning en prestaties*** .  Voor meer informatie over het configureren van gebruik voor automatisch schalen de handleiding hier [schalen van een app in Azure App Service][AppScale]. 

### <a name="worker-pool-selection"></a>Selectie van Worker-groep
Zoals eerder vermeld, wordt de selectie worker-groep is toegankelijk vanuit de gebruikersinterface voor ASP.  Open de blade voor de ASP-code die u wilt schalen en selecteer de groep met werkrollen.  U ziet alle van de worker-groepen die u hebt geconfigureerd in uw App Service Environment.  Hebt u slechts één werknemersgroep vervolgens ziet alleen u de één groep van toepassingen die worden vermeld.  Als u wilt wijzigen welke groep met werkrollen uw ASP is in, selecteer u gewoon de worker-groep die u wilt dat uw App Service-Plan om naar te verplaatsen.  

![][3]

Voordat u doorgaat uw ASP in een werkgroep naar een andere is het belangrijk om te controleren of er voldoende capaciteit voor de ASP.  In de lijst met werknemersgroepen, niet alleen wordt de naam van de werknemer toepassingen vermeld maar u kunt ook zien hoe veel werknemers zijn beschikbaar in deze groep met werkrollen.  Zorg ervoor dat er voldoende exemplaren beschikbaar zijn voor uw App Service-Plan bevatten.  Als u moet meer rekenresources in de worker-groep die u verplaatsen wilt naar, krijgt u vervolgens de ASE-beheerder om toe te voegen.  

> [!NOTE]
> Verplaatsen van dat een ASP in een werkgroep, zullen koude start van de ASP-apps.  Dit kan leiden tot aanvragen langzaam worden uitgevoerd als uw app koude gestart op de nieuwe compute-resources is.  De koude start kan worden voorkomen met behulp van de [toepassing warme van mogelijkheid] [ AppWarmup] in Azure App Service.  De initialisatie van toepassingen-module die is beschreven in het artikel werkt ook voor koude starts omdat het initialisatieproces ook aangeroepen wordt wanneer apps koude gestart op de nieuwe compute-resources zijn. 
> 
> 

## <a name="getting-started"></a>Aan de slag
Als u wilt aan de slag met App Service-omgevingen, Zie [hoe om te maken van een App Service-omgeving][HowtoCreateASE]

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
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/ 
[ASEAutoscale]: app-service-environment-auto-scale.md
[AppScale]: ../web-sites-scale.md
[AppWarmup]: https://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
