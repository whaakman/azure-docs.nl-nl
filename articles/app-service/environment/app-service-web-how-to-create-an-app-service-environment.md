---
title: Het maken van een App Service omgeving v1
description: Beschrijving van de stroom maken voor een app service-omgeving v1
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 81bd32cf-7ae5-454b-a0d2-23b57b51af47
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: ef0dc1b820f42b73af3af3882085729ecc21230c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-an-app-service-environment-v1"></a>Het maken van een App Service omgeving v1 

> [!NOTE]
> In dit artikel gaat over de v1 App Service-omgeving. Er is een nieuwere versie van de App Service-omgeving die eenvoudiger te gebruiken en wordt uitgevoerd op krachtiger infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot de App-serviceomgeving](intro.md).
> 

### <a name="overview"></a>Overzicht
De as-omgeving (App Service omgeving) is de optie voor een Premium-service van Azure App Service, die zorgt voor een verbeterde functie die is niet beschikbaar in de multitenant stempels. De functie as-omgeving wordt in wezen de Azure App Service geïmplementeerd in virtueel netwerk van een klant. Een beter inzicht in de mogelijkheden die worden aangeboden door App Service-omgevingen lezen krijgen de [wat is er een App-serviceomgeving] [ WhatisASE] documentatie.

### <a name="before-you-create-your-ase"></a>Voordat u uw as-omgeving maken
Het is belangrijk te weten van de dingen die u niet wijzigen. Deze aspecten die u over uw as-omgeving niet wijzigen nadat deze is gemaakt, zijn:

* Locatie
* Abonnement
* Resourcegroep
* VNet gebruikt
* Subnet dat wordt gebruikt 
* De subnetgrootte van het

Wanneer het verzamelen van een VNet en het opgeven van een subnet, Controleer of is het groot genoeg voor een eventuele toekomstige groei. 

### <a name="creating-an-app-service-environment-v1"></a>Maken van een App Service-omgeving v1
Maken van een App Service-omgeving v1 die u wilt zoeken van de Azure Marketplace voor ***App Service-omgeving v1***, of door te gaan via nieuw -> Web en mobiel -> App Service-omgeving. Een ASEv1 maken:

1. Geef de naam van uw as-omgeving. De naam die is opgegeven voor de as-omgeving wordt gebruikt voor de apps die zijn gemaakt in de as-omgeving. Naam van het subdomein zou zijn als de naam van de as-omgeving appsvcenvdemo. *appsvcenvdemo.p.azurewebsites.net*. Als u een app met de naam dus gemaakt *mytestapp* en vervolgens het normaal zou op adresseerbare zijn *mytestapp.appsvcenvdemo.p.azurewebsites.net*. U kunt witruimte niet gebruiken in de naam van uw as-omgeving. Als u hoofdletters in de naam gebruikt, worden de domeinnaam de totale kleine versie met die naam. Als u een ILB gebruikt vervolgens de naam van uw as-omgeving wordt niet gebruikt in een subdomein maar in plaats daarvan expliciet is opgegeven tijdens het maken van de as-omgeving
   
    ![][1]
2. Selecteer uw abonnement. Het abonnement dat u gebruikt voor uw as-omgeving is ook een die alle apps in die as-omgeving met worden gemaakt. U kunt uw as-omgeving in een VNet dat in een ander abonnement plaatsen
3. Selecteer of geef een nieuwe resourcegroep. De resourcegroep die wordt gebruikt voor uw as-omgeving moet dezelfde dat wordt gebruikt voor uw VNet. Als u een bestaande VNet selecteert wordt vervolgens de selectie van de resourcegroep voor uw as-omgeving bijgewerkt naar aanleiding van uw VNet.
   
    ![][2]
4. Controleer uw selecties virtueel netwerk en de locatie. U kunt een nieuw VNet maken of selecteren van een bestaande VNet. Als u een nieuw VNet daarna kunt u een naam en locatie opgeven. De nieuwe VNet heeft de adres-bereik 192.168.250.0/23 en een subnet met de naam **standaard** die is gedefinieerd als 192.168.250.0/24. U kunt ook gewoon een reeds bestaande klassiek of Resource Manager VNet selecteren. De selectie van het VIP bepaalt of de as-omgeving rechtstreeks toegankelijk zijn vanaf internet (extern) of als een interne Load Balancer (ILB) wordt gebruikt. Voor meer informatie over deze lezen [met behulp van een interne Load Balancer met een App-serviceomgeving][ILBASE]. Als u een VIP-type van extern selecteert kunt u het aantal externe IP-adressen het systeem is gemaakt met voor IPSSL doeleinden selecteren. Als u interne selecteren moet u opgeven van het subdomein die door uw as-omgeving wordt gebruikt. ASEs kan worden geïmplementeerd in virtuele netwerken die gebruikmaken van *beide* openbare-adresbereiken, *of* RFC1918 adresruimten (dat wil zeggen particuliere adressen). U moet het VNet tevoren maken om een virtueel netwerk gebruiken met een openbare-adresbereik. Wanneer u een bestaande VNet selecteren moet u een nieuw subnet maken tijdens het maken van de as-omgeving. **U kunt een vooraf gemaakte subnet niet gebruiken in de portal. Als u uw as-omgeving met een resource manager-sjabloon maakt, kunt u een as-omgeving met een bestaande subnet.** Een as-omgeving maken van een sjabloon gebruiken de informatie hier [maken van een App-serviceomgeving uit sjabloon] [ ILBAseTemplate] en hier [een ILB-App Service-omgeving te maken van sjabloon][ASEfromTemplate].

### <a name="details"></a>Details
Een as-omgeving wordt gemaakt met 2-Front-Ends en twee 2 werkers beschikken. De Front-Ends fungeren als de HTTP/HTTPS-eindpunten en verkeer worden verzonden naar de werknemers die zijn de functies die voor het hosten van uw apps. U kunt het aantal na het maken van de as-omgeving aanpassen en zelfs instellen regels voor automatisch schalen op deze resourcegroepen. Ga voor meer informatie over het handmatig schalen beheren en controleren van een App Service-omgeving naar: [een App-serviceomgeving configureren][ASEConfig] 

Alleen de één as-omgeving kan bestaan in het subnet dat wordt gebruikt door de as-omgeving. Het subnet kan niet worden gebruikt voor iets anders dan de as-omgeving

### <a name="after-app-service-environment-v1-creation"></a>Na het maken van App Service-omgeving v1
U kunt na het maken van de as-omgeving aanpassen:

* Aantal van de Front-Ends (minimum: 2)
* Aantal werknemers (minimum: 2)
* Het aantal IP-adressen beschikbaar voor IP-SSL
* COMPUTE resource grootten gebruikt door de Front-Ends of werknemers (front-end minimumgrootte is P2)

Er zijn meer details omtrent handmatige schaal, management en hier bewaking van App Service-omgevingen: [een App-serviceomgeving configureren][ASEConfig] 

Voor informatie over automatisch schalen is er een handleiding hier: [automatisch schalen voor een App-serviceomgeving configureren][ASEAutoscale]

Er zijn extra afhankelijkheden die niet beschikbaar om aan te passen, zoals de database en de opslag. Deze zijn verwerkt door Azure en worden geleverd met het systeem. De opslagruimte op het systeem ondersteunt maximaal 500 GB voor de hele App Service-omgeving en de database wordt aangepast door Azure naar behoefte van de schaal van het systeem.

## <a name="getting-started"></a>Aan de slag
Om aan de slag met App Service-omgeving v1, Zie [Inleiding tot de App Service-omgeving v1][WhatisASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[ASEConfig]: app-service-web-configure-an-app-service-environment.md
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[ASEAutoscale]: app-service-environment-auto-scale.md
[ILBASE]: app-service-environment-with-internal-load-balancer.md
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: app-service-app-service-environment-create-ilb-ase-resourcemanager.md
