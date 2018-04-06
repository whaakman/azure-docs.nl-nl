---
title: Uw ILB App Service-omgeving integreren met de Azure-toepassingsgateway
description: Overzicht over het integreren van een app in uw ILB App Service-omgeving met een Application Gateway
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
ms.openlocfilehash: 31aea1d19ed6da856bb5fc634a919819513cb6b2
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="integrate-your-ilb-app-service-environment-with-the-azure-application-gateway"></a>Uw ILB App Service-omgeving integreren met de Azure-toepassingsgateway #

De [App Service-omgeving](./intro.md) is een Azure App Service-implementatie in het subnet van een klant virtuele Azure-netwerk. Het kan worden geïmplementeerd met een openbare of particuliere eindpunt voor toegang tot Apps. De implementatie van de App Service-omgeving met een persoonlijke eindpunt (dat wil zeggen, een interne load balancer) heet een ILB-App Service-omgeving.  

Web application firewalls help beveiligd uw webtoepassingen door te inspecteren inkomend webverkeer blokkeren SQL-injectie, Cross-Site Scripting, malware uploads & DDoS-toepassing en andere aanvallen. Deze ook inspecteert de antwoorden van de back-end-webservers gegevens gegevensverlies te voorkomen (DLP). U kunt een apparaat WAF ophalen uit Azure marketplace of kunt u de [Azure Application Gateway][appgw].

De Azure Application Gateway is een virtueel apparaat zorgt laag 7 taakverdeling, SSL-offloading en beveiliging van web application firewall (WAF). Wordt geluisterd op een openbaar IP-adres en route verkeer naar het toepassingseindpunt van uw. De volgende informatie beschrijft hoe een WAF geconfigureerd toepassingsgateway integreren met een app in een ILB-App Service-omgeving.  

De integratie van de toepassingsgateway met een ILB App Service-omgeving is op het niveau van een app. Als u de toepassingsgateway met uw ILB App Service-omgeving configureert, staat u het alleen voor specifieke toepassingen in uw ILB App Service-omgeving. Deze techniek kan die als host fungeert voor beveiligde multitenant toepassingen in een enkele ILB App Service-omgeving.  

![Toepassingsgateway die verwijst naar de app op een ILB-App Service-omgeving][1]

In deze procedure gaat u het volgende doen:

* Maak een Azure-toepassing-Gateway.
* Configureer de toepassingsgateway om te verwijzen naar een app in uw ILB App Service-omgeving.
* Configureer uw app om te voldoen aan de aangepaste domeinnaam.
* De openbare DNS-hostnaam die naar de toepassingsgateway van uw verwijst bewerken.

## <a name="prerequisites"></a>Vereisten

Als u wilt uw toepassingsgateway integreren in uw ILB App Service-omgeving, hebt u het volgende nodig:

* Een ILB-App Service-omgeving.
* Een app die wordt uitgevoerd in de ILB App Service-omgeving.
* Een routeerbaar internetdomeinnaam moet worden gebruikt met uw app in de ILB App Service-omgeving.
* De ILB-adres dat uw ILB App Service-omgeving wordt gebruikt. Deze informatie wordt in de portal voor App Service-omgeving onder **instellingen** > **IP-adressen**:

    ![Voorbeeld van de lijst met IP-adressen die worden gebruikt door de ILB App-serviceomgeving][9]
    
* Een openbare DNS-naam die later wordt gebruikt om te verwijzen naar uw Application Gateway. 

Zie voor meer informatie over het maken van een ILB App-serviceomgeving [maken en gebruiken van een ILB App-serviceomgeving][ilbase].

In dit artikel wordt ervan uitgegaan dat u wilt dat een Application Gateway in hetzelfde virtuele netwerk van Azure waarop de App Service-omgeving wordt geïmplementeerd. Voordat u begint met het maken van de toepassingsgateway, kiezen of maken van een subnet die u gebruiken wilt voor het hosten van de gateway. 

U moet een subnet plaatsen dat is niet de één met de naam GatewaySubnet. Als u de toepassingsgateway in GatewaySubnet plaatst, kunt u zich niet aan een virtuele netwerkgateway later maken. 

U kunt geen ook de gateway in het subnet dat gebruikmaakt van uw App Service-omgeving ILB plaatsen. De App Service-omgeving is het enige dat die in dit subnet worden kan.

## <a name="configuration-steps"></a>Configuratiestappen ##

1. In de Azure portal, gaat u naar **nieuw** > **netwerk** > **Application Gateway**.

2. In de **basisbeginselen** gebied:

   a. Voor **naam**, voer de naam van de toepassingsgateway.

   b. Voor **laag**, selecteer **WAF**.

   c. Voor **abonnement**, selecteert u hetzelfde abonnement die gebruikmaakt van het virtuele netwerk van App Service-omgeving.

   d. Voor **resourcegroep**Maak of Selecteer de resourcegroep.

   e. Voor **locatie**, selecteer de locatie van het virtuele netwerk van App Service-omgeving.

   ![Basisprincipes van nieuwe toepassingsgateway maken][2]

3. In de **instellingen** gebied:

   a. Voor **virtueel netwerk**, selecteer het virtuele netwerk van App Service-omgeving.

   b. Voor **Subnet**, selecteert u het subnet waarin de toepassingsgateway moet worden geïmplementeerd. Gebruik geen GatewaySubnet, omdat voorkomen het maken van VPN-gateways dat wordt.

   c. Voor **type IP-adres**, selecteer **openbare**.

   d. Voor **openbaar IP-adres**, selecteert u een openbaar IP-adres. Als u niet hebt, moet u een nu maken.

   e. Voor **Protocol**, selecteer **HTTP** of **HTTPS**. Als u voor HTTPS configureren wilt, moet u een PFX-certificaat op te geven.

   f. Voor **Web application firewall**, kunt u de firewall inschakelen en ook ingesteld voor een **detectie** of **preventie** wens naar.

   ![Instellingen voor het nieuwe toepassingsgateway maken][3]
    
4. In de **samenvatting** sectie, Controleer de instellingen en selecteer **OK**. Uw toepassingsgateway kan duren iets meer dan 30 minuten om setup te voltooien.  

5. Nadat uw toepassingsgateway setup voltooit, gaat u naar uw Application Gateway-portal. Selecteer **back-endpool**. Het adres ILB toevoegen voor uw ILB App Service-omgeving.

   ![Back endpool configureren][4]

6. Nadat het proces van het configureren van uw back-end-pool is voltooid, selecteert u **statuscontroles**. Maak een health test voor de domeinnaam die u wilt gebruiken voor uw app. 

   ![Statuscontroles configureren][5]
    
7. Nadat het proces van het configureren van uw statuscontroles is voltooid, selecteert u **HTTP-instellingen**. De bestaande instellingen bewerken, selecteert u **aangepaste logboekregistratie gebruiken test**, en kies de test op die u hebt geconfigureerd.

   ![HTTP-instellingen configureren][6]
    
8. Ga naar de toepassingsgateway **overzicht** sectie en kopieer de openbare IP-adres dat gebruikmaakt van de Gateway van uw toepassing. Stel in dat IP-adres een A-record voor de domeinnaam van uw app of de DNS-naam voor dit adres gebruiken in een CNAME-record. Het is gemakkelijker om het openbare IP-adres selecteren en kopiëren vanuit de gebruikersinterface van het openbare IP-adres in plaats van kopiëren via de koppeling in de toepassingsgateway **overzicht** sectie. 

   ![Application Gateway-portal][7]

9. De aangepaste domeinnaam voor uw app in uw ILB App Service-omgeving instellen. Ga naar uw app in de portal en onder **instellingen**, selecteer **aangepaste domeinen**.

   ![De aangepaste domeinnaam instellen op de app.][8]

Er is informatie over het instellen van aangepaste domeinnamen voor uw web-apps in het artikel [aangepaste domeinnamen voor uw web-app instellen][custom-domain]. Maar voor een app in een ILB-App Service-omgeving, niet een validatie van de domeinnaam. Omdat u beschikt over de DNS-server die de app-eindpunten beheert, kunt u elke gewenste daar kunt plaatsen. De aangepaste domeinnaam die u in dit geval toevoegt hoeft niet in uw DNS, maar nog steeds hoeft worden geconfigureerd met uw app. 

Nadat setup is voltooid en u hebt een korte tijdsduur voor de DNS-wijzigingen te kunnen doorvoeren toegestaan, kunt u uw app openen met behulp van de aangepaste domeinnaam die u hebt gemaakt. 


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
[appgw]: http://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[custom-domain]: ../app-service-web-tutorial-custom-domain.md
[ilbase]: ./create-ilb-ase.md
