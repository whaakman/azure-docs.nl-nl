---
title: De ILB App Service-omgeving integreren met Azure Application Gateway
description: Overzicht over het integreren van een app in uw ILB App Service Environment met een Application Gateway
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a6a74f17-bb57-40dd-8113-a20b50ba3050
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/03/2018
ms.author: ccompy
ms.openlocfilehash: f1e527918086fb003696c09828969e371ff9ca96
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968814"
---
# <a name="integrate-your-ilb-app-service-environment-with-the-azure-application-gateway"></a>De ILB App Service-omgeving integreren met Azure Application Gateway #

De [App Service-omgeving](./intro.md) is een implementatie van Azure App Service in het subnet van de Azure-netwerk van een klant. Deze kan worden geïmplementeerd met een openbaar of privé-eindpunt voor toegang tot Apps. De implementatie van de App Service-omgeving met een persoonlijke eindpunt (dat wil zeggen, een interne load balancer) is een ILB App Service-omgeving genoemd.  

Web application firewalls helpen met het beveiligen uw webtoepassingen door inkomend webverkeer te inspecteren om te blokkeren van de SQL-injecties, Cross-Site Scripting, malware-uploads en DDoS-toepassingen en andere aanvallen. Ook inspecteert deze de antwoorden van de back-end-webservers voor gegevensverlies te voorkomen (DLP). Krijgt u een WAF-apparaat in Azure marketplace of kunt u de [Azure Application Gateway][appgw].

De Azure Application Gateway is een virtueel apparaat waarmee layer 7 taakverdeling, SSL-offloading en bescherming van web application firewall (WAF). Het kan luisteren naar een openbare IP-adres en route-verkeer naar het toepassingseindpunt van uw. De volgende informatie wordt beschreven hoe u een toepassingsgateway met WAF geconfigureerd integreren met een app in een ILB App Service-omgeving.  

De integratie van application gateway met de ILB App Service-omgeving is op het niveau van een app. Wanneer u de toepassingsgateway met de ILB App Service-omgeving configureert, doet u dit voor specifieke apps in uw ILB App Service-omgeving. Deze techniek kan hosten van veilige multitenant-toepassingen in een enkele ILB App Service-omgeving.  

![Toepassingsgateway die verwijst naar de app op een ILB App Service Environment][1]

In deze procedure gaat u het volgende doen:

* Maak een Azure Application Gateway.
* Configureer de toepassingsgateway om te verwijzen naar een app in uw ILB App Service-omgeving.
* Uw app om te voldoen aan de aangepaste domeinnaam configureren.
* Bewerk de openbare DNS-hostnaam die naar uw application gateway verwijst.

## <a name="prerequisites"></a>Vereisten

Als u wilt uw Application Gateway integreren met uw ILB App Service-omgeving, hebt u het volgende nodig:

* Een ILB App Service-omgeving.
* Een app die wordt uitgevoerd in de ILB App Service-omgeving.
* Een internet routeerbare domeinnaam moet worden gebruikt met uw app in de ILB App Service-omgeving.
* Het ILB-adres dat gebruikmaakt van de ILB App Service-omgeving. Deze informatie is in de portal voor App Service-omgeving onder **instellingen** > **IP-adressen**:

    ![Voorbeeld van de lijst met IP-adressen die worden gebruikt door de ILB App Service-omgeving][9]
    
* Een openbare DNS-naam die later gebruikt wordt om te verwijzen naar uw Application Gateway. 

Zie voor meer informatie over het maken van een ILB App Service Environment [maken en gebruiken van een ILB App Service Environment][ilbase].

In dit artikel wordt ervan uitgegaan dat u wilt een Application Gateway in de dezelfde Azure-netwerk waarop de App Service Environment wordt geïmplementeerd. Voordat u begint met het maken van de Application Gateway, kiezen of maken van een subnet die u gebruiken wilt voor het hosten van de gateway. 

U moet een subnet plaatsen dat is niet één met de naam GatewaySubnet gebruiken. Als u de toepassingsgateway op GatewaySubnet opslaat, kunt u zich niet aan een virtuele netwerkgateway later maken. 

U kunt ook de gateway in het subnet dat gebruikmaakt van de ILB App Service-omgeving opnemen. De App Service-omgeving is het enige wat die in dit subnet worden kan.

## <a name="configuration-steps"></a>Configuratiestappen ##

1. In de Azure-portal, gaat u naar **nieuw** > **netwerk** > **Application Gateway**.

1. In de **basisbeginselen** gebied:

   a. Voor **naam**, voer de naam van de toepassingsgateway.

   b. Voor **laag**, selecteer **WAF**.

   c. Voor **abonnement**, selecteert u hetzelfde abonnement als dat het virtuele netwerk van App Service-omgeving wordt gebruikt.

   d. Voor **resourcegroep**, maak of Selecteer de resourcegroep.

   e. Voor **locatie**, selecteert u de locatie van het virtuele netwerk van App Service-omgeving.

   ![Nieuwe Application Gateway maken-basisbeginselen][2]

1. In de **instellingen** gebied:

   a. Voor **virtueel netwerk**, selecteer het virtuele netwerk van App Service-omgeving.

   b. Voor **Subnet**, selecteert u het subnet waarin de toepassingsgateway moet worden geïmplementeerd. Gebruik geen GatewaySubnet, omdat voorkomen het maken van VPN-gateways dat wordt.

   c. Voor **type IP-adres**, selecteer **openbare**.

   d. Voor **openbaar IP-adres**, selecteert u een openbaar IP-adres. Als u niet hebt, moet u een nu maken.

   e. Voor **Protocol**, selecteer **HTTP** of **HTTPS**. Als u voor HTTPS configureert, moet u een PFX-certificaat op te geven.

   f. Voor **Web application firewall**, kunt u de firewall inschakelen en ook instellen voor een **detectie** of **preventie** wens naar.

   ![Nieuwe instellingen voor het maken van Application Gateway][3]
    
1. In de **samenvatting** sectie, Controleer de instellingen en selecteer **OK**. Uw Application Gateway kan duren iets meer dan 30 minuten om installatie te voltooien.  

1. Nadat uw Application Gateway is setup voltooid, gaat u naar uw Application Gateway-portal. Selecteer **back-endpool**. Het ILB-adres voor uw ILB App Service Environment toevoegen.

   ![Back-endpool configureren][4]

1. Nadat het proces van het configureren van uw back-end-pool is voltooid, selecteert u **statuscontroles**. Maak een statustest voor de domeinnaam die u wilt gebruiken voor uw app. 

   ![Statuscontroles configureren][5]
    
1. Nadat het proces van het configureren van uw statuscontroles is voltooid, selecteert u **HTTP-instellingen**. De bestaande instellingen bewerken, selecteert u **gebruik aangepaste test**, en kies de test die u hebt geconfigureerd.

   ![HTTP-instellingen configureren][6]
    
1. Ga naar de Application Gateway **overzicht** sectie en kopieer de openbare IP-adres dat gebruikmaakt van uw Application Gateway. Dat IP-adres instellen als een A-record voor de naam van uw app-domein of de DNS-naam voor dit adres gebruiken in een CNAME-record. Het is gemakkelijker om het openbare IP-adres selecteren en kopiëren vanuit de gebruikersinterface van de openbare IP-adres in plaats van te kopiëren uit de koppeling in de Application Gateway **overzicht** sectie. 

   ![Application Gateway-portal][7]

1. De aangepaste domeinnaam voor uw app in uw ILB App Service-omgeving instellen. Ga naar uw app in de portal en onder **instellingen**, selecteer **aangepaste domeinen**.

   ![De aangepaste domeinnaam instellen op de app.][8]

Er is informatie over het instellen van aangepaste domeinnamen voor uw web-apps in het artikel [aangepaste domeinnamen voor uw web-app instellen][custom-domain]. Maar voor een app in een ILB App Service-omgeving, niet een validatie van de domeinnaam. Omdat u eigenaar van de DNS-server waarmee de eindpunten van de app wordt beheerd, kunt u alles wat u wilt dat er kunt plaatsen. De aangepaste domeinnaam die u in dit geval toevoegt hoeft niet te worden in het DNS, maar hoeft nog steeds kan worden geconfigureerd met uw app. 

Nadat setup is voltooid en u hebt een korte periode voor uw DNS-wijzigingen zijn doorgegeven toegestaan, kunt u uw app openen met behulp van de aangepaste domeinnaam die u hebt gemaakt. 


<!--IMAGES-->
[1]: ./media/integrate-with-application-gateway/appgw-highlevel.png
[2]: ./media/integrate-with-application-gateway/appgw-createbasics.png
[3]: ./media/integrate-with-application-gateway/appgw-createsettings.png
[4]: ./media/integrate-with-application-gateway/appgw-backendpool.png
[5]: ./media/integrate-with-application-gateway/appgw-healthprobe.png
[6]: ./media/integrate-with-application-gateway/appgw-httpsettings.png
[7]: ./media/integrate-with-application-gateway/appgw-publicip.png
[8]: ./media/integrate-with-application-gateway/appgw-customdomainname.png
[9]: ./media/integrate-with-application-gateway/appgw-iplist.png

<!--LINKS-->
[appgw]: https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[custom-domain]: ../app-service-web-tutorial-custom-domain.md
[ilbase]: ./create-ilb-ase.md
