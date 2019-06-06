---
title: Een interne load balancer maken met App Service Environment - Azure
description: Meer informatie over het maken en gebruiken van een Azure App Service-omgeving die geïsoleerd is van internet
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 05/28/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 5b05755502ad5836a21080a122d2e1721825f10c
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734692"
---
# <a name="create-and-use-an-internal-load-balancer-app-service-environment"></a>Maken en gebruiken van een interne Load Balancer App Service Environment 

De Azure App Service-omgeving is een implementatie van Azure App Service in een subnet in een Azure-netwerk (VNet). Er zijn twee manieren om een Azure App Service-omgeving (ASE) te implementeren: 

- Met een VIP-adres op een extern IP-adres, vaak aangeduid als Externe AS-omgeving.
- Met een VIP-adres op een intern IP-adres, vaak aangeduid als een ILB AS-omgeving omdat het interne eindpunt een ILB (Internal Load Balancer) is. 

In dit artikel wordt uitgelegd hoe u een ILB AS-omgeving maakt. Zie [Introduction to App Service Environments][Intro] voor een overzicht van de ASE. Zie [Create an External ASE][MakeExternalASE] (Een Externe AS-omgeving maken) voor informatie over het maken van een Externe AS-omgeving.

## <a name="overview"></a>Overzicht 

U kunt een AS-omgeving implementeren met een eindpunt dat toegankelijk is via internet of met een IP-adres in uw VNet. De AS-omgeving moet zijn geïmplementeerd met een ILB om het IP-adres in te stellen op een VNet-adres. Wanneer u uw ASE met een ILB implementeert, moet u de naam van de as-omgeving opgeven. De naam van de as-omgeving wordt gebruikt in het domeinachtervoegsel voor de apps in de as-omgeving.  Het domeinachtervoegsel voor uw ILB as-omgeving is &lt;as-omgeving de naam&gt;. appservicewebsites.net. Apps die zijn aangebracht in een ILB as-omgeving, worden niet in de openbare DNS-server geplaatst. 

Eerdere versies van de ILB as-omgeving vereist dat u voor het domeinachtervoegsel van een en een standaardcertificaat voor HTTPS-verbindingen. Het domeinachtervoegsel niet meer worden verzameld tijdens het maken van ILB as-omgeving en een standaardcertificaat worden ook niet meer verzameld. Wanneer u nu een ILB as-omgeving maakt, wordt het standaard-certificaat wordt geleverd door Microsoft en wordt vertrouwd door de browser. U bent nog steeds kunnen aangepaste domeinnamen voor apps in de as-omgeving instellen en instellen van certificaten voor deze aangepaste domeinnamen. 

Met een ILB as-omgeving, kunt u bijvoorbeeld dingen doen:

-   Host intranettoepassingen veilig op in de cloud, waartoe u toegang hebt via een site-naar-site- of ExpressRoute.
-   Apps beveiligen met een WAF-apparaat
-   Apps die niet worden vermeld op openbare DNS-servers, hosten in de cloud.
-   Back-end-apps met internetisolatie maken, waarmee front-end-apps veilig kunnen worden geïntegreerd.

### <a name="disabled-functionality"></a>Uitgeschakelde functionaliteit ###

Er is een aantal dingen dat u niet kunt doen wanneer u een ILB AS-omgeving gebruikt:

-   Op IP-gebaseerd SSL gebruiken.
-   IP-adressen toewijzen aan specifieke apps.
-   Een certificaat kopen en gebruiken met een app via Azure Portal. U kunt certificaten rechtstreeks bij een certificeringsinstantie verkrijgen en ze gebruiken met uw apps. U kunt ze niet verkrijgen via Azure Portal.

## <a name="create-an-ilb-ase"></a>Een ILB AS-omgeving maken ##

Ga als volgt te werk om een ILB AS-omgeving te maken:

1. Selecteer in de Azure-portal achtereenvolgens **Een resource maken** > **Web** > **App Service Environment**.

2. Selecteer uw abonnement.

3. Selecteer of maak een resourcegroep.

4. Voer de naam van uw App Service Environment.

5. Interne virtuele IP-type selecteren.

    ![ASE maken](media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase.png)

6. Netwerken selecteren

7. Selecteer of maak een Virtueelnetwerk. Als u hier een nieuw VNet maakt, wordt deze met een adresbereik van 192.168.250.0/23 worden gedefinieerd. Voor het maken van een VNet met een ander adresbereik of in een andere resourcegroep dan de as-omgeving, moet u de Azure Virtual Network maken-portal gebruiken. 

8. Selecteer of maak een lege een subnet. Als u wilt een subnet selecteren, is het moet leeg zijn en niet overgedragen. De grootte van het gatewaysubnet kan niet worden gewijzigd nadat de as-omgeving is gemaakt. We raden een grootte aan van `/24`. Dit formaat bevat 256 adressen en kan de grootst mogelijke AS-omgeving verwerken en voldoen aan alle schaalbehoeften. 

    ![ASE-netwerken][1]

7. Selecteer **bekijken en maken** Selecteer **maken**.

## <a name="create-an-app-in-an-ilb-ase"></a>Een app maken in een ILB AS-omgeving ##

Het maken van een app in een ILB AS-omgeving werkt hetzelfde als het maken van een app in een AS-omgeving.

1. Selecteer in de Azure portal, **een resource maken** > **Web** > **Web-App**.

1. Voer de naam van de app in.

1. Selecteer het abonnement.

1. Selecteer of maak een resourcegroep.

1. Selecteer uw publiceren, Runtime-Stack en besturingssysteem.

1. Selecteer een locatie waar de locatie een bestaande ILB as-omgeving is.  U kunt ook een nieuwe ASE maken tijdens het maken van apps door te selecteren van een geïsoleerde App Service-plan. Als u maken van een nieuwe ASE wilt, selecteert u de regio die u wilt dat de as-omgeving worden gemaakt.

1. Selecteer of maak een App Service-plan. 

1. Selecteer **bekijken en maken** Selecteer **maken** wanneer u klaar bent.

### <a name="web-jobs-functions-and-the-ilb-ase"></a>WebJobs, Functions en de ILB AS-omgeving 

Een ILB AS-omgeving biedt ondersteuning voor zowel Functions als WebJobs. Als u echter met deze wilt werken via de portal, hebt u netwerktoegang tot de SCM-site nodig.  Dit betekent dat de host van de browser zich in het virtuele netwerk moet bevinden of ermee moet zijn verbonden. Als uw ILB as-omgeving een domeinnaam die eindigt niet heeft op *appserviceenvironment.net*, moet u uw browser naar het HTTPS-certificaat wordt gebruikt door uw scm-site vertrouwt.

## <a name="dns-configuration"></a>DNS-configuratie 

Wanneer u een extern VIP-adres gebruikt, wordt de DNS-server beheerd met Azure. Elke app die is gemaakt in de AS-omgeving wordt automatisch toegevoegd aan Azure DNS, wat een openbaar DNS is. In een ILB AS-omgeving moet u uw eigen DNS beheren. Het domeinachtervoegsel gebruikt in combinatie met een ILB as-omgeving, is afhankelijk van de naam van de as-omgeving. Het domeinachtervoegsel is  *&lt;as-omgeving de naam&gt;. appserviceenvironment.net*. Het IP-adres voor uw ILB is in de portal onder **IP-adressen**. 

Het configureren van uw DNS:

- Maak een zone voor  *&lt;as-omgeving de naam&gt;. appserviceenvironment.net*
- een A-record maken in de zone die verwijst * naar de ILB IP-adres 
- Maak een zone in  *&lt;as-omgeving de naam&gt;. scm.appserviceenvironment.net* met de naam scm
- een A-record maken in de scm-zone die naar het ILB IP-adres verwijst

## <a name="publish-with-an-ilb-ase"></a>Publiceren met een ILB AS-omgeving

Elke app die wordt gemaakt, heeft twee eindpunten. In een ILB as-omgeving, hebt u *&lt;appnaam&gt;.&lt; Domein voor ILB as-omgeving&gt;* en  *&lt;appnaam&gt;.scm.&lt; Domein voor ILB as-omgeving&gt;* . 

De SCM-sitenaam leidt naar de Kudu-console, genaamd de **Geavanceerde portal**, binnen Azure Portal. Met behulp van de Kudu-console kunt u omgevingsvariabelen bekijken, de schijf verkennen, een console gebruiken, en nog veel meer. Zie [Kudu-console voor Azure App Service][Kudu] voor meer informatie. 

Op internet gebaseerde CI-systemen, zoals GitHub en Azure DevOps, werken nog steeds met een ILB AS-omgeving, als de buildagent toegankelijk is via internet en zich op hetzelfde netwerk bevindt als de ILB AS-omgeving. Als de buildagent dus, in het geval van Azure DevOps, is gemaakt in hetzelfde VNET als de ILB AS-omgeving (verschillende subnetten vormen geen probleem), kan met deze agent code worden opgehaald uit Azure DevOps-git en worden geïmplementeerd in de ILB AS-omgeving. Als u niet zelf een buildagent wilt maken, moet u een CI-systeem met een pull-model gebruiken, zoals Dropbox.

De publicatie-eindpunten voor apps in een ILB AS-omgeving maken gebruik van het domein waarmee de ILB AS-omgeving is gemaakt. Dit domein wordt weergegeven in het publicatieprofiel van de app en in de portalblade van de app (**Overzicht** > **Essentials** en ook **Eigenschappen**). Als u een ILB as-omgeving met het domeinachtervoegsel hebt  *&lt;as-omgeving de naam&gt;. appserviceenvironment.net*, en een app met de naam *mytest*, gebruikt u *mytest.&lt; De naam van de as-omgeving&gt;. appserviceenvironment.net* voor FTP en *mytest.scm.contoso.net* voor webimplementatie.

## <a name="configure-an-ilb-ase-with-a-waf-device"></a>Een ILB as-omgeving configureren met een WAF-apparaat ##

U kunt een web application firewall (WAF) apparaat combineren met uw ILB as-omgeving alleen blootstellen de apps dat u wilt dat met het internet en de overige alleen toegankelijk is vanaf in het VNet te houden. Hiermee kunt u aan het bouwen van veilige toepassingen met meerdere lagen onder andere.

Zie voor meer informatie over het configureren van uw ILB as-omgeving met een WAF-apparaat, [een web application firewall configureren met uw App Service environment][ASEWAF]. In dit artikel leest u hoe u een virtueel Barracuda-apparaat gebruikt met de AS-omgeving. Een andere optie is het gebruik van Azure Application Gateway. Application Gateway maakt gebruik van de OWASP-kernregels om alle toepassingen te beveiligen die erachter zijn geplaatst. Zie [Introduction to the Azure web application firewall][AppGW] (Inleiding tot de WAF (Web Application Firewall) in Azure) voor meer informatie over Application Gateway.

## <a name="ilb-ases-made-before-may-2019"></a>ILB as-omgevingen voor mei 2019 uitgevoerd

ILB as-omgevingen die zijn gemaakt voordat mei 2019 vereist dat u het domeinachtervoegsel instellen tijdens het maken van as-omgeving. Ze ook vereist dat u voor het uploaden van een standaardcertificaat dat is gebaseerd op die het domeinachtervoegsel. Ook met een oudere ILB as-omgeving uitvoeren u niet single sign-on bij de Kudu-console met apps in die ILB as-omgeving. Bij het configureren van DNS voor een oudere ILB as-omgeving, moet u het jokerteken A-record in een zone die overeenkomt met het achtervoegsel van uw domein instellen. 

## <a name="get-started"></a>Aan de slag ##

* Zie [Introduction to App Service environments][Intro] (Inleiding tot App Service-omgevingen) om aan de slag te gaan met AS-omgevingen. 

<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
[5]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-ipaddresses.png

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
[webapps]: ../overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[customdomain]: ../app-service-web-tutorial-custom-domain.md
[linuxapp]: ../containers/app-service-linux-intro.md
