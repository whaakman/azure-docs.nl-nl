---
title: Een externe Azure App Service-omgeving maken
description: Wordt uitgelegd hoe u een App Service-omgeving maken bij het maken van een app of een zelfstandige
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 94dd0222-b960-469c-85da-7fcb98654241
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 6df708c97750c89c850c993d0e1a43ded01934a2
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52959958"
---
# <a name="create-an-external-app-service-environment"></a>Een externe App Service-omgeving maken #

Azure App Service Environment is een implementatie van Azure App Service in een subnet in een virtueel Azure-netwerk (VNet). Er zijn twee manieren om een AS-omgeving te implementeren:

- Met een VIP-adres op een extern IP-adres, vaak aangeduid als Externe AS-omgeving.
- Met de VIP-adres op een intern IP-adres, vaak aangeduid als een ILB as-omgeving omdat het interne eindpunt een interne load balancer (ILB).

Dit artikel laat u het maken van een externe as-omgeving. Zie voor een overzicht van de as-omgeving, [een inleiding tot App Service Environment][Intro]. Zie voor meer informatie over het maken van een ILB as-omgeving [maken en gebruiken een ILB as-omgeving][MakeILBASE].

## <a name="before-you-create-your-ase"></a>Voordat u uw ASE maken ##

Nadat u uw as-omgeving hebt gemaakt, kunt u het volgende niet wijzigen:

- Locatie
- Abonnement
- Resourcegroep
- VNet gebruikt
- Subnet dat wordt gebruikt
- Grootte van het gatewaysubnet

> [!NOTE]
> Als u een VNet kiest en geef een subnet, zorgt u ervoor dat deze groot genoeg is voor toekomstige groei en schaalbehoeften. We raden een grootte van `/24` met 256-adressen.
>

## <a name="three-ways-to-create-an-ase"></a>Drie manieren om te maken van een as-omgeving ##

Er zijn drie manieren om te maken van een as-omgeving:

- **Tijdens het maken van een App Service-plan**. Deze methode maakt u de as-omgeving en de App Service-plan in één stap.
- **Als een zelfstandige actie**. Deze methode maakt u een zelfstandige as-omgeving, dit is een ASE met niets in deze. Deze methode is een meer geavanceerde proces voor het maken van een as-omgeving. U deze gebruiken om te maken van een as-omgeving met een ILB.
- **Een Azure Resource Manager-sjabloon**. Deze methode is geschikt voor geavanceerde gebruikers. Zie voor meer informatie, [een as-omgeving maken met een sjabloon][MakeASEfromTemplate].

Een externe as-omgeving heeft een openbaar VIP-adres, wat betekent dat alle HTTP/HTTPS-verkeer naar de apps in de as-omgeving komt binnen via internet toegankelijk is via het IP-adres. Een met een ILB as-omgeving heeft een IP-adres uit het subnet dat wordt gebruikt door de as-omgeving. De apps die worden gehost in een ILB as-omgeving worden niet rechtstreeks blootgesteld aan internet.

## <a name="create-an-ase-and-an-app-service-plan-together"></a>Samen een as-omgeving en een App Service-plan maken ##

De App Service-plan is een container van apps. Wanneer u een app in App Service maakt, kunt u kiezen of maken van een App Service-plan. App Service-omgevingen Houd App Service-plannen en App Service-plannen Houd apps.

Een as-omgeving maken bij het maken van een App Service-plan:

1. In de [Azure-portal](https://portal.azure.com/), selecteer **een resource maken** > **Web en mobiel** > **Web-App**.

    ![Web-Apps maken][1]

1. Selecteer uw abonnement. De app en de as-omgeving zijn gemaakt in dezelfde abonnementen.

1. Selecteer of maak een resourcegroep. Met resourcegroepen kunt u gerelateerde Azure-resources beheren als één eenheid. Resourcegroepen zijn ook nuttig wanneer u regels voor toegangsbeheer op basis van rollen voor uw apps instellen. Zie voor meer informatie de [overzicht van Azure Resource Manager][ARMOverview].

1. Selecteer uw besturingssysteem (Windows, Linux of Docker). 

1. Selecteer het App Service-plan, en selecteer vervolgens **nieuw**. Linux-web-apps en web-apps voor Windows kunnen niet in de dezelfde App Service-Plan, maar kunnen zich in hetzelfde App Service Environment. 

    ![Nieuw App Service-plan][2]

1. In de **locatie** vervolgkeuzelijst, selecteer de regio waar u wilt maken de as-omgeving. Als u een bestaande as-omgeving selecteert, wordt een nieuwe ASE is niet gemaakt. De App Service-plan is gemaakt in de as-omgeving die u hebt geselecteerd. 

1. Selecteer **prijscategorie**, en kies een van de **geïsoleerd** prijzen van voorraadeenheden. Als u ervoor kiest een **geïsoleerd** SKU-kaart en een locatie die niet een as-omgeving, een nieuwe as-omgeving is gemaakt op die locatie. Selecteer eerst het proces voor het maken van een as-omgeving **Selecteer**. De **geïsoleerd** SKU is alleen beschikbaar in combinatie met een as-omgeving. U ook niet gebruiken andere prijzen SKU's in een as-omgeving anders dan **geïsoleerd**. 

    ![Geselecteerde prijscategorie][3]

1. Voer de naam voor de as-omgeving. Deze naam wordt gebruikt in de adresseerbare naam voor uw apps. Als de naam van de ASE _appsvcenvdemo_, de domeinnaam is *. appsvcenvdemo.p.azurewebsites.net*. Als u een app met de naam maakt *mytestapp*, is het beschikbaar maken zonder op mytestapp.appsvcenvdemo.p.azurewebsites.net. U kunt witruimte in de naam van de gebruiken. Als u hoofdletters gebruikt, is de domeinnaam de totaal in kleine letters versie met die naam.

    ![Naam van een nieuwe App Service-plan][4]

1. Geef de details van Azure virtuele netwerken. Selecteer een **Maak een nieuwe** of **Selecteer een bestaande**. De optie voor het selecteren van een bestaand VNet is alleen beschikbaar als u een VNet in de geselecteerde regio hebt. Als u selecteert **nieuw**, voer een naam voor het VNet. Een nieuwe Resource Manager VNet met die naam is gemaakt. Hierbij de adresruimte `192.168.250.0/23` in de geselecteerde regio. Als u selecteert **bestaande selecteren**, moet u:

    a. Selecteer de VNet-Adresblok, als u meer dan één hebt.

    b. Voer een nieuwe naam van het subnet.

    c. Selecteer de grootte van het subnet. *Vergeet niet om een grootte die groot genoeg is voor toekomstige groei van uw as-omgeving selecteren.* Het is raadzaam `/25`, die bevat 128 adressen en een ASE grootst kunnen worden verwerkt. U kunt beter geen `/28`, bijvoorbeeld, omdat alleen 16-adressen beschikbaar zijn. Ten minste zeven adressen worden gebruikt voor infrastructuur en Azure Networking gebruikmaakt van een andere 5. In een `/28` subnet, bent u bent met een maximum van 4 App Service-plan-exemplaren voor een externe as-omgeving vergroten/verkleinen en slechts 3-exemplaren voor een ILB as-omgeving de App Service-plan.

    d. Selecteer het bereik van de IP-subnet.

1. Selecteer **maken** te maken van de as-omgeving. Dit proces maakt ook het App Service-plan en de app. De as-omgeving, App Service-plan en de app worden alle onder hetzelfde abonnement en dezelfde resourcegroep bevinden. Als de as-omgeving een afzonderlijke resourcegroep moet of als u een ILB as-omgeving nodig hebt, volgt u de stappen voor het maken van een as-omgeving op zichzelf.

## <a name="create-an-ase-and-a-linux-web-app-using-a-custom-docker-image-together"></a>Een as-omgeving en een Linux-web-app met een aangepaste Docker-installatiekopie samen maken

1. In de [Azure-portal](https://portal.azure.com/), **een Resource maken** > **Web en mobiel** > **Web App for Containers.** 

    ![Web-Apps maken][7]

1. Selecteer uw abonnement. De app en de as-omgeving zijn gemaakt in dezelfde abonnementen.

1. Selecteer of maak een resourcegroep. Met resourcegroepen kunt u gerelateerde Azure-resources beheren als één eenheid. Resourcegroepen zijn ook nuttig wanneer u regels voor toegangsbeheer op basis van rollen voor uw apps instellen. Zie voor meer informatie de [overzicht van Azure Resource Manager][ARMOverview].

1. Selecteer het App Service-plan, en selecteer vervolgens **nieuw**. Linux-web-apps en web-apps voor Windows kunnen niet in de dezelfde App Service-Plan, maar kunnen zich in hetzelfde App Service Environment. 

    ![Nieuw App Service-plan][8]

1. In de **locatie** vervolgkeuzelijst, selecteer de regio waar u wilt maken de as-omgeving. Als u een bestaande as-omgeving selecteert, wordt een nieuwe ASE is niet gemaakt. De App Service-plan is gemaakt in de as-omgeving die u hebt geselecteerd. 

1. Selecteer **prijscategorie**, en kies een van de **geïsoleerd** prijzen van voorraadeenheden. Als u ervoor kiest een **geïsoleerd** SKU-kaart en een locatie die niet een as-omgeving, een nieuwe as-omgeving is gemaakt op die locatie. Selecteer eerst het proces voor het maken van een as-omgeving **Selecteer**. De **geïsoleerd** SKU is alleen beschikbaar in combinatie met een as-omgeving. U ook niet gebruiken andere prijzen SKU's in een as-omgeving anders dan **geïsoleerd**. 

    ![Geselecteerde prijscategorie][3]

1. Voer de naam voor de as-omgeving. Deze naam wordt gebruikt in de adresseerbare naam voor uw apps. Als de naam van de ASE _appsvcenvdemo_, de domeinnaam is *. appsvcenvdemo.p.azurewebsites.net*. Als u een app met de naam maakt *mytestapp*, is het beschikbaar maken zonder op mytestapp.appsvcenvdemo.p.azurewebsites.net. U kunt witruimte in de naam van de gebruiken. Als u hoofdletters gebruikt, is de domeinnaam de totaal in kleine letters versie met die naam.

    ![Naam van een nieuwe App Service-plan][4]

1. Geef de details van Azure virtuele netwerken. Selecteer een **Maak een nieuwe** of **Selecteer een bestaande**. De optie voor het selecteren van een bestaand VNet is alleen beschikbaar als u een VNet in de geselecteerde regio hebt. Als u selecteert **nieuw**, voer een naam voor het VNet. Een nieuwe Resource Manager VNet met die naam is gemaakt. Hierbij de adresruimte `192.168.250.0/23` in de geselecteerde regio. Als u selecteert **bestaande selecteren**, moet u:

    a. Selecteer de VNet-Adresblok, als u meer dan één hebt.

    b. Voer een nieuwe naam van het subnet.

    c. Selecteer de grootte van het subnet. *Vergeet niet om een grootte die groot genoeg is voor toekomstige groei van uw as-omgeving selecteren.* Het is raadzaam `/25`, die bevat 128 adressen en een ASE grootst kunnen worden verwerkt. U kunt beter geen `/28`, bijvoorbeeld, omdat alleen 16-adressen beschikbaar zijn. Ten minste zeven adressen worden gebruikt voor infrastructuur en Azure Networking gebruikmaakt van een andere 5. In een `/28` subnet, bent u bent met een maximum van 4 App Service-plan-exemplaren voor een externe as-omgeving vergroten/verkleinen en slechts 3-exemplaren voor een ILB as-omgeving de App Service-plan.

    d. Selecteer het bereik van de IP-subnet.

1.  Selecteer 'Container configureren'.
    * Voer de naam van uw aangepaste installatiekopie (kunt u Azure Container Registry, Docker Hub en uw eigen persoonlijke register). Als u niet wilt gebruiken van uw eigen aangepaste container, kunt u alleen uw code en een ingebouwde installatiekopie gebruiken met App Service onder Linux, met behulp van de bovenstaande instructies. 

    ![Container configureren][9]

1. Selecteer **maken** te maken van de as-omgeving. Dit proces maakt ook het App Service-plan en de app. De as-omgeving, App Service-plan en de app worden alle onder hetzelfde abonnement en dezelfde resourcegroep bevinden. Als de as-omgeving een afzonderlijke resourcegroep moet of als u een ILB as-omgeving nodig hebt, volgt u de stappen voor het maken van een as-omgeving op zichzelf.


## <a name="create-an-ase-by-itself"></a>Zelf een as-omgeving maken ##

Als u een zelfstandige as-omgeving maakt, heeft niets in deze. Een lege as-omgeving worden nog steeds een maandelijkse kosten in rekening gebracht voor de infrastructuur. Volg deze stappen een ASE met een ILB maken of te maken van een as-omgeving in een eigen resourcegroep bevinden. Nadat u uw as-omgeving maakt, kunt u apps in deze maken met behulp van het normale proces. Selecteer de nieuwe as-omgeving als de locatie.

1. Zoek in de Azure Marketplace naar **App Service-omgeving**, of selecteer **een resource maken** > **Web mobiel** > **App Service-omgeving**. 

1. Voer de naam van de as-omgeving. Deze naam wordt gebruikt voor de apps die zijn gemaakt in de as-omgeving. Als de naam *mynewdemoase*, naam van het subdomein is *. mynewdemoase.p.azurewebsites.net*. Als u een app met de naam maakt *mytestapp*, is het beschikbaar maken zonder op mytestapp.mynewdemoase.p.azurewebsites.net. U kunt witruimte in de naam van de gebruiken. Als u hoofdletters gebruikt, is de domeinnaam naam van het totale kleine letters. Als u een ILB, wordt de naam van uw as-omgeving niet wordt gebruikt in een subdomein maar in plaats daarvan expliciet is opgegeven tijdens het maken van as-omgeving.

    ![Naamgeving van ASE][5]

1. Selecteer uw abonnement. Dit abonnement is ook een die gebruikmaken van alle apps in de as-omgeving. U kunt geen uw ASE plaatsen in een VNet dat is in een ander abonnement.

1. Selecteer of geef een nieuwe resourcegroep. De resourcegroep die is gebruikt voor de as-omgeving moet hetzelfde account dat wordt gebruikt voor uw VNet. Als u een bestaand VNet selecteert, wordt de selectie van de resourcegroep voor de as-omgeving bijgewerkt met die van uw VNet. *U kunt een as-omgeving maken met een resourcegroep die verschilt van de VNet-resourcegroep als u een Resource Manager-sjabloon gebruikt.* Zie voor informatie over het maken van een as-omgeving van een sjabloon [maken van een App Service environment met een sjabloon][MakeASEfromTemplate].

    ![Resourcegroep selecteren][6]

1. Selecteer uw VNet en de locatie. U kunt een nieuw VNet maken of een bestaand VNet selecteert: 

    * Als u een nieuw VNet selecteert, kunt u een naam en locatie opgeven. 
    
    * De nieuwe VNet heeft de adresbereik 192.168.250.0/23 en een subnet met de naam standaard. Het subnet wordt gedefinieerd als 192.168.250.0/24. U kunt alleen een Resource Manager VNet selecteren. De **VIP-Type** selectie bepaalt of de as-omgeving kan rechtstreeks worden benaderd vanaf het internet (extern) of als deze een ILB gebruikt. Zie voor meer informatie over deze opties, [maken en gebruiken van een interne load balancer met een App Service environment][MakeILBASE]. 

      * Als u selecteert **externe** voor de **VIP-Type**, kunt u het aantal externe IP-adressen het systeem wordt gemaakt met oog op IP gebaseerde SSL. 
    
      * Als u selecteert **intern** voor de **VIP-Type**, moet u het domein dat gebruikmaakt van de as-omgeving. U kunt een as-omgeving implementeren in een VNet dat gebruikmaakt van publieke of privé-adresbereiken. Voor het gebruik van een VNet met een openbare-adresbereik, moet u het VNet vooraf maken. 
    
    * Als u een bestaand VNet selecteert, wordt een nieuw subnet wordt gemaakt wanneer de as-omgeving is gemaakt. *U kunt een vooraf gemaakte subnet niet gebruiken in de portal. Als u een Resource Manager-sjabloon gebruikt, kunt u een as-omgeving maken met een bestaand subnet.* Zie voor informatie over het maken van een as-omgeving van een sjabloon [maken van een App Service Environment met een sjabloon][MakeASEfromTemplate].

## <a name="app-service-environment-v1"></a>App Service-omgeving v1 ##

U kunt nog steeds exemplaren van de eerste versie van App Service-omgeving (ASEv1) maken. U start dit proces, zoek in Marketplace naar **App Service Environment v1**. U maakt de as-omgeving op dezelfde manier die u de zelfstandige as-omgeving maakt. Wanneer deze voltooid, wordt uw ASEv1 heeft twee front-ends en twee werkrollen. Met ASEv1, moet u de front-ends en werkrollen beheren. Ze worden niet automatisch toegevoegd bij het maken van uw App Service-plannen. De front-ends fungeren als de HTTP/HTTPS-eindpunten en verkeer verzenden naar de werknemers. De werknemers zijn de functies die uw apps hosten. Nadat u uw as-omgeving hebt gemaakt, kunt u de hoeveelheid van front-ends en werkrollen aanpassen. 

Zie voor meer informatie over ASEv1 [Inleiding tot de App Service Environment v1][ASEv1Intro]. Zie voor meer informatie over het omhoog schalen, beheren en controleren van ASEv1 [het configureren van een App Service Environment][ConfigureASEv1].

<!--Image references-->
[1]: ./media/how_to_create_an_external_app_service_environment/createexternalase-create.png
[2]: ./media/how_to_create_an_external_app_service_environment/createexternalase-aspcreate.png
[3]: ./media/how_to_create_an_external_app_service_environment/createexternalase-pricing.png
[4]: ./media/how_to_create_an_external_app_service_environment/createexternalase-embeddedcreate.png
[5]: ./media/how_to_create_an_external_app_service_environment/createexternalase-standalonecreate.png
[6]: ./media/how_to_create_an_external_app_service_environment/createexternalase-network.png
[7]: ./media/how_to_create_an_external_app_service_environment/createexternalase-createwafc.png
[8]: ./media/how_to_create_an_external_app_service_environment/createexternalase-aspcreatewafc.png
[9]: ./media/how_to_create_an_external_app_service_environment/createexternalase-configurecontainer.png



<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
