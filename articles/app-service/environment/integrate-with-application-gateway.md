---
title: Uw as ILB-omgeving integreren met een Azure Application Gateway
description: Overzicht over het integreren van een app in uw as ILB-omgeving met uw Azure-toepassingsgateway
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
ms.date: 10/17/2017
ms.author: ccompy
ms.openlocfilehash: eedad8824add7fe425d34975dab640fbee82c2bc
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2017
---
# <a name="integrating-your-ilb-ase-with-an-application-gateway"></a>Uw as ILB-omgeving integreren met een Application Gateway #

De [Azure App Service omgeving (as-omgeving)](./intro.md) is een implementatie van de Azure App Service in het subnet van een klant Azure Virtual Network. Het kan worden geïmplementeerd met een openbare of particuliere eindpunt voor toegang tot Apps. De implementatie van de as-omgeving met een persoonlijke eindpunt heet een ILB-as-omgeving.  
De Azure Application Gateway is een virtueel apparaat zorgt laag 7 taakverdeling, SSL-offloading en WAF beveiliging. Wordt geluisterd op een openbaar IP-adres en route verkeer naar het toepassingseindpunt van uw. De volgende informatie beschrijft het integreren van een WAF Application Gateway geconfigureerd met een app op een as ILB-omgeving.  

De integratie van de toepassingsgateway met de as ILB-omgeving is op het niveau van een app.  Als u de toepassingsgateway met de as ILB-omgeving configureert, doet u dit voor specifieke toepassingen in uw as ILB-omgeving. Hierdoor kunnen die als host fungeert voor beveiligde multitenant-toepassingen in een enkel ILB as-omgeving.  

![Toepassingsgateway die verwijst naar de app op een as ILB-omgeving][1]

In deze procedure gaat u het volgende doen:

* Een toepassingsgateway maken
* de toepassingsgateway om te verwijzen naar een app op uw as ILB-omgeving configureren
* uw app om te voldoen aan de aangepaste domeinnaam configureren
* uw openbare DNS-hostnaam die naar de Gateway van uw toepassing verwijst bewerken

Als u wilt uw toepassingsgateway integreren in uw as ILB-omgeving, hebt u het volgende nodig:

* een as ILB-omgeving
* een app die wordt uitgevoerd in de as ILB-omgeving
* een routeerbaar internetdomeinnaam moet worden gebruikt met uw app in de as ILB-omgeving
* de ILB adres dat wordt gebruikt door de as ILB-omgeving (dit is in de portal as-omgeving onder **instellingen -> IP-adressen**)

    ![IP-adressen die worden gebruikt door de as ILB-omgeving][9]
    
* een openbare DNS-naam die later wordt gebruikt om te verwijzen naar uw toepassingsgateway 

Voor meer informatie over het maken van een as-omgeving voor de ILB lezen van het document [maken en gebruiken van een as ILB-omgeving][ilbase]

Deze handleiding wordt ervan uitgegaan dat u wilt dat een Application Gateway in hetzelfde Azure-netwerk dat is geïmplementeerd in de as-omgeving. Voordat u start het maken van de toepassingsgateway, kiezen of maken van een subnet die u gebruiken wilt voor het hosten van de toepassingsgateway. U moet een subnet plaatsen dat is niet de één met de naam GatewaySubnet of het subnet dat wordt gebruikt door de as ILB-omgeving.
Als u de toepassingsgateway in het GatewaySubnet geplaatst vervolgens kunt u zich kan later een virtuele netwerkgateway maken. U niet kunt ook plaatsen in het subnet als de as-omgeving is het enige dat die in een subnet kan worden gebruikt door uw as ILB-omgeving.

## <a name="steps-to-configure"></a>Stappen voor het configureren ##

1. Uit binnen de Azure-portal, gaat u naar **Nieuw > netwerk > Application Gateway** 
    1. Bieden:
        1. Naam van de toepassingsgateway
        1. Selecteer WAF
        1. Selecteer de hetzelfde abonnement dat u gebruikt voor het VNet as-omgeving
        1. Maak of Selecteer de resourcegroep
        1. Selecteer de locatie met het VNet as-omgeving

    ![Basisprincipes van nieuwe application gateway maken][2]   
    1. In de set van het gedeelte instellingen:
        1. Het VNet as-omgeving
        1. Het subnet van de toepassingsgateway moet worden geïmplementeerd in. Doet u er geen gebruik van het GatewaySubnet omdat voorkomen het maken van VPN-gateways dat wordt
        1. Openbare selecteren
        1. Selecteer een openbaar IP-adres. Als u geen een maakt u een op dit moment
        1. Configureer voor HTTP of HTTPS. Als voor HTTPS, moet u een PFX-certificaat configureren
        1. Selecteer de instellingen voor de webtoepassing fireway. Hier kunt u de firewall inschakelen en ook ingesteld voor de detectie of te voorkomen, zoals u ziet past.

    ![Het maken van instellingen voor de nieuwe toepassingsgateway][3]
    
    1. Selecteer in het overzicht van de samenvatting, **Ok**. Het kan iets meer dan 30 minuten duren voordat uw toepassingsgateway om setup te voltooien.  

2. Nadat uw toepassingsgateway setup voltooit, gaat u in uw portal Application Gateway. Selecteer **back-endpool**.  Het adres ILB toevoegen voor uw as ILB-omgeving.

    ![Back endpool configureren][4]

3. Nadat de verwerking is voltooid voor het configureren van uw back endpool, selecteert u **statuscontroles**. Maak een health test voor de domeinnaam die u wilt gebruiken voor uw app. 

    ![Statuscontroles configureren][5]
    
4. Nadat de verwerking is voltooid voor het configureren van uw statuscontroles, selecteert u **HTTP-instellingen**.  Bewerk de bestaande instelling er, selecteert u **aangepaste logboekregistratie gebruiken test**, en kies de test die u hebt geconfigureerd.

    ![HTTP-instellingen configureren][6]
    
5. Ga naar de toepassingsgateway **overzicht**, en kopieer de openbare IP-adres voor uw toepassingsgateway.  Stel in dat IP-adres een A-record voor de domeinnaam van uw app of de DNS-naam voor dit adres gebruiken in een CNAME-record.  Het is gemakkelijker te selecteert u het openbare IP-adres en kopiëren vanuit de gebruikersinterface van openbare IP-adres in plaats van de koppeling in de sectie overzicht van Application Gateway kopiëren. 

    ![Application Gateway-portal][7]

6. De aangepaste domeinnaam voor uw app instellen in uw as ILB-omgeving.  Ga naar uw app in de portal en selecteer instellingen onder **aangepaste domeinen**

![De aangepaste domeinnaam instellen op de app.][8]

Er is informatie over het instellen van aangepaste domeinnamen voor uw web-apps hier [aangepaste domeinnamen voor uw web-app instellen][custom-domain]. Het verschil met een app in een ILB-as-omgeving en dat document is dat er validatie van de domeinnaam niet.  Omdat u beschikt over de DNS-server die de app-eindpunten beheert, kunt u elke gewenste daar kunt plaatsen. De aangepaste domeinnaam die u in dit geval toevoegt hoeft niet in uw DNS, maar nog steeds hoeft worden geconfigureerd met uw app. 

Nadat setup is voltooid en u hebt een korte tijdsduur voor de DNS-wijzigingen te kunnen doorvoeren, toegestaan en vervolgens kunt u uw app openen door de aangepaste domeinnaam die u hebt gemaakt. 


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
