---
title: Over het maken van een App Service Environment v1
description: Beschrijving van het maken van de stroom voor een app service environment v1
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: 81bd32cf-7ae5-454b-a0d2-23b57b51af47
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: 289ff76e533497a731a4fc51b3e54101a9d34a68
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52958363"
---
# <a name="how-to-create-an-app-service-environment-v1"></a>Over het maken van een App Service Environment v1 

> [!NOTE]
> In dit artikel gaat over de App Service Environment v1. Er is een nieuwere versie van de App Service Environment die gebruiksvriendelijker en wordt uitgevoerd op een krachtigere infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot App Service Environment](intro.md).
> 

### <a name="overview"></a>Overzicht
De App Service Environment (ASE) is een Premium-service van Azure App Service die zorgt voor een verbeterde mogelijkheden die niet beschikbaar in de stempels voor meerdere tenants. De functie voor ASE implementeert in feite Azure App Service in het virtuele netwerk van een klant. Om te krijgen een beter inzicht in de mogelijkheden die door App Service-omgevingen lezen de [wat is er een App Service Environment] [ WhatisASE] documentatie.

### <a name="before-you-create-your-ase"></a>Voordat u uw ASE maken
Het is belangrijk om te worden op de hoogte van de dingen die u niet wijzigen. Deze aspecten die u niet over de as-omgeving wijzigen nadat deze is gemaakt, zijn:

* Locatie
* Abonnement
* Resourcegroep
* VNet gebruikt
* Subnet dat wordt gebruikt 
* Grootte van het gatewaysubnet

Bij het verzamelen van een VNet en het opgeven van een subnet, zorg ervoor dat is het groot genoeg is voor een eventuele toekomstige groei. 

### <a name="creating-an-app-service-environment-v1"></a>Het maken van een App Service Environment v1
Voor het maken van een App Service Environment v1, kunt u zoeken naar de Azure Marketplace voor ***App Service Environment v1***, of gaat u door **een resource maken** -> **Web en mobiel**  ->  **App Service-omgeving**. Een ASEv1 maken:

1. Geef de naam van de as-omgeving. De naam die u voor de as-omgeving opgeeft wordt gebruikt voor de apps die zijn gemaakt in de as-omgeving. Als de naam van de as-omgeving appsvcenvdemo is, de naam van het subdomein zou zijn: *appsvcenvdemo.p.azurewebsites.net*. Als u dus een app met de naam gemaakt *mytestapp*, het normaal zou zijn opgevraagd op *mytestapp.appsvcenvdemo.p.azurewebsites.net*. U kunt witruimte niet gebruiken in de naam van de as-omgeving. Als u hoofdletters in de naam gebruikt, worden de domeinnaam de totaal in kleine letters versie met die naam. Als u een ILB, wordt de naam van uw as-omgeving wordt niet gebruikt in een subdomein maar in plaats daarvan expliciet is opgegeven tijdens het maken van as-omgeving.
   
    ![][1]
2. Selecteer uw abonnement. Het abonnement dat u voor de as-omgeving gebruikt wordt ook toegepast op alle apps die u in deze as-omgeving maken. U kunt de as-omgeving in een VNet dat is in een ander abonnement plaatsen.
3. Selecteer of geef een nieuwe resourcegroep. De resourcegroep die is gebruikt voor de as-omgeving moet dezelfde die wordt gebruikt voor uw VNet. Als u een bestaand VNet selecteert, wordt de selectie van de resourcegroep voor de as-omgeving worden bijgewerkt met die van uw VNet.
   
    ![][2]
4. Controleer uw selecties Virtueelnetwerk en de locatie. U kunt een nieuw VNet maken of selecteren van een bestaande VNet. Als u een nieuw VNet selecteren en vervolgens kunt u een naam en locatie. De nieuwe VNet heeft de adresbereik 192.168.250.0/23 en een subnet met de naam **standaard** die is gedefinieerd als 192.168.250.0/24. U kunt ook gewoon een reeds bestaande klassieke of Resource Manager VNet selecteren. De VIP-Type-selectie bepaalt of de as-omgeving kan rechtstreeks worden benaderd vanaf het internet (extern) of als deze een interne Load Balancer (ILB) gebruikt. Voor meer informatie over deze lezen [met behulp van een interne Load Balancer met een App Service Environment][ILBASE]. Als u een VIP-type van externe kunt u hoeveel externe IP-adressen het systeem wordt gemaakt met IPSSL ter selecteren. Als u interne selecteert vervolgens moet u om op te geven van het subdomein dat door de as-omgeving wordt gebruikt. As-omgevingen kunnen worden geïmplementeerd in virtuele netwerken die gebruikmaken van *beide* openbare-adressen, *of* RFC1918 adresruimten (dat wil zeggen particuliere adressen). Als u wilt gebruiken een virtueel netwerk met een openbare-adresbereik, moet u het VNet vooraf maken. Wanneer u een bestaand VNet selecteert moet u een nieuw subnet maken tijdens het maken van as-omgeving. **U kunt een vooraf gemaakte subnet niet gebruiken in de portal. Als u uw ASE met een resource manager-sjabloon maakt, kunt u een as-omgeving met een bestaand subnet.** Een ASE maken van een voor het sjabloongebruik de informatie hier [het maken van een App Service Environment uit sjabloon] [ ILBAseTemplate] en hier [een ILB App Service Environment maken met sjabloon] [ASEfromTemplate].

### <a name="details"></a>Details
Een as-omgeving is gemaakt met 2 Front-Ends en twee 2 werkers beschikken. De Front-Ends fungeren als de HTTP/HTTPS-eindpunten en verkeer verzenden naar de werknemers die zijn de functies die uw apps hosten. U kunt het aantal aanpassen nadat de as-omgeving is gemaakt en kan zelfs stelt u de regels voor automatisch schalen op deze resourcegroepen. Ga voor meer informatie over het handmatig schalen, beheren en controleren van een App Service Environment naar: [een App Service Environment configureren][ASEConfig] 

Alleen de één as-omgeving kan bestaan in het subnet dat wordt gebruikt door de as-omgeving. Het subnet kan niet worden gebruikt voor iets anders dan de as-omgeving

### <a name="after-app-service-environment-v1-creation"></a>Na het maken van App Service Environment v1
Na het maken van de as-omgeving kunt u aanpassen:

* Aantal Front-Ends (minimum: 2)
* Aantal werknemers (minimum: 2)
* Aantal IP-adressen beschikbaar voor IP SSL
* COMPUTE resource-grootten die worden gebruikt door de Front-Ends of werkrollen (P2-Front-End minimale grootte is)

Er zijn meer informatie over handmatige schalen, beheer en bewaking van App Service-omgevingen hier: [een App Service Environment configureren][ASEConfig] 

Voor meer informatie over automatisch schalen is een handleiding hier: [voor automatisch schalen configureren voor een App Service Environment][ASEAutoscale]

Er zijn extra afhankelijkheden die niet beschikbaar voor aanpassing, zoals de database en opslag. Dit zijn verwerkt door Azure en worden geleverd met het systeem. De opslagruimte op het systeem ondersteunt maximaal 500 GB voor de hele App Service-omgeving en de database wordt aangepast aan de door Azure indien nodig door de schaal van het systeem.

## <a name="getting-started"></a>Aan de slag
Als u wilt aan de slag met App Service Environment v1, Zie [Inleiding tot de App Service Environment v1][WhatisASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[ASEConfig]: app-service-web-configure-an-app-service-environment.md
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/ 
[ASEAutoscale]: app-service-environment-auto-scale.md
[ILBASE]: app-service-environment-with-internal-load-balancer.md
[ILBAseTemplate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: app-service-app-service-environment-create-ilb-ase-resourcemanager.md
