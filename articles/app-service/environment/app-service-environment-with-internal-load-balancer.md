---
title: Maken en gebruiken van een interne Load Balancer met een App Service-omgeving | Microsoft Docs
description: Maken en gebruiken van een as-omgeving met een ILB
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: ad9a1e00-d5e5-413e-be47-e21e5b285dbf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: c0f942cb11edc6d6212914b3134e25c3fbc3d3ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Met behulp van een interne Load Balancer met een App Service-omgeving

> [!NOTE] 
> In dit artikel gaat over de v1 App Service-omgeving. Er is een nieuwere versie van de App Service-omgeving die eenvoudiger te gebruiken en wordt uitgevoerd op krachtiger infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot de App-serviceomgeving](intro.md).
>

De functie as-omgeving (App Service omgeving) is de optie voor een Premium-service van Azure App Service, die zorgt voor een verbeterde functie die is niet beschikbaar in de multitenant stempels. De functie as-omgeving implementeert in wezen de Azure App Service in uw virtuele Azure-Network(VNet). Een beter inzicht in de mogelijkheden die worden aangeboden door App Service-omgevingen lezen krijgen de [wat is er een App-serviceomgeving] [ WhatisASE] documentatie. Als u niet dat de voordelen weet van het besturingssysteem in een VNet-Lees de [virtuele netwerk Veelgestelde vragen over Azure][virtualnetwork]. 

## <a name="overview"></a>Overzicht
Een as-omgeving kan worden geïmplementeerd met een internet toegankelijk eindpunt of met een IP-adres in uw VNet. Om het IP-adres ingesteld op een VNet-adres moet u uw as-omgeving met een interne Load Balancer(ILB) implementeren. Wanneer uw as-omgeving is geconfigureerd met een ILB moet u het volgende opgeven:

* uw eigen domein of subdomein. Om het gemakkelijk maken, dit document wordt ervan uitgegaan dat subdomein maar kunt u deze configureren in beide gevallen. 
* het certificaat dat wordt gebruikt voor HTTPS
* DNS-beheer voor een subdomein. 

Tegenprestatie kunt u doen, zoals:

* host intranettoepassingen, zoals line-of-business-toepassingen, veilig in de cloud die u via een Site met Site of ExpressRoute VPN openen
* host-apps in de cloud die niet worden weergegeven in de openbare DNS-servers
* maken van internet geïsoleerd back-end voor apps die uw apps front-end kunnen veilig worden geïntegreerd met

#### <a name="disabled-functionality"></a>Uitgeschakelde functionaliteit
Er zijn een aantal zaken die u niet mogelijk wanneer u een ILB-as-omgeving. Die dingen zijn onder andere:

* met behulp van IPSSL
* IP-adressen toewijzen aan specifieke apps
* kopen en gebruik van een certificaat met een app via de portal. U kunt uiteraard nog steeds certificaten rechtstreeks met een certificeringsinstantie verkrijgen en deze gebruiken met uw apps, niet via de Azure portal.

## <a name="creating-an-ilb-ase"></a>Een as ILB-omgeving maken
Maken van een as ILB-omgeving is niet veel verschillende normaal gesproken een as-omgeving maakt. Lees voor meer gedetailleerde informatie over het maken van een as-omgeving [het maken van een App-serviceomgeving][HowtoCreateASE]. Het proces voor het maken van een as ILB-omgeving is hetzelfde tussen het maken van een VNet tijdens het maken van de as-omgeving of een bestaande VNet te selecteren. Een as ILB-omgeving maken: 

1. Selecteer in het Azure portal **Nieuw -> Web en mobiel -> App Service-omgeving**
2. Selecteer uw abonnement
3. Selecteer of maak een resourcegroep
4. Selecteer of maak een VNet
5. Een subnet maken als een VNet selecteren
6. Selecteer **virtuele netwerklocatie-VNet-configuratie >** en het VIP-Type op Internal instellen
7. Geef de subdomeinnaam (dit is het subdomein dat wordt gebruikt voor apps die zijn gemaakt in deze as-omgeving)
8. Klik op Ok en maak vervolgens

![][1]

Er is een VNet-configuratie-optie binnen de blade virtueel netwerk. Dit kunt u kiezen tussen een externe VIP of interne VIP. De standaardwaarde is extern. Als u ingesteld op extern hebt gebruik uw as-omgeving een internet toegankelijk VIP. Als u interne selecteert, wordt uw as-omgeving worden geconfigureerd met een ILB op een IP-adres binnen uw VNet. 

Na het selecteren van interne, de mogelijkheid meer IP-adressen toevoegen aan uw as-omgeving is verwijderd en in plaats daarvan moet u het subdomein van de as-omgeving. De naam van de as-omgeving wordt in een as met een externe VIP-omgeving gebruikt in het subdomein voor apps die zijn gemaakt in die as-omgeving. Als u uw as-omgeving is aangeroepen ***contosotest*** en de naam van uw app in die as-omgeving ***mytest*** en vervolgens het subdomein in de notatie worden zou ***contosotest.p.azurewebsites.net*** en de URL voor die app ***mytest.contosotest.p.azurewebsites.net***. Als u het VIP-Type op Internal instelt, wordt de naam van uw as-omgeving niet gebruikt in het subdomein voor de as-omgeving. U opgeven het subdomein expliciet. Als een subdomein is ***contoso.corp.net*** en u een app hebt aangebracht in de betreffende as-omgeving met de naam ***timereporting*** en vervolgens de URL voor die app ***timereporting.contoso.corp.net***.

## <a name="apps-in-an-ilb-ase"></a>Apps in een ILB-as-omgeving
Maken van een app in een ILB-as-omgeving, is hetzelfde als voor het maken van een app in een as-omgeving is het normaal. 

1. Selecteer in het Azure portal **Nieuw -> Web en mobiel -> Web** of **Mobile** of **API-App**
2. Voer de naam van app
3. Abonnement selecteren
4. Selecteer of resourcegroep maken
5. Selecteer of maak Plan(ASP) van App Service. Als u een nieuwe ASP maken selecteert u uw as-omgeving als de locatie en selecteer de worker-groep die u wilt dat uw ASP moet worden gemaakt. Bij het maken van de ASP selecteert u uw as-omgeving als de locatie en de worker-groep. Als u de naam van de app ziet u dat het subdomein onder de appnaam van uw is vervangen door het subdomein voor uw as-omgeving. 
6. Selecteer maken. Selecteer de **vastmaken aan dashboard** selectievakje in als u wilt dat de app worden weergegeven op uw dashboard. 

![][2]

Naam van het subdomein opgehaald onder de naam van de app bijgewerkt, zodat het subdomein van de as-omgeving. 

## <a name="post-ilb-ase-creation-validation"></a>Validatie van post ILB as-omgeving maken
Er is een ILB-as-omgeving iets anders dan de niet - ILB as-omgeving. Als al opgemerkt moet u uw eigen DNS beheren en u hebt ook uw eigen certificaat voor HTTPS-verbindingen opgeven. 

Nadat u uw as-omgeving maken ziet u dat het subdomein ziet u het subdomein dat u hebt opgegeven en er is een nieuw item in de **instelling** menu aangeroepen **ILB certificaat**. De as-omgeving wordt gemaakt met een zelfondertekend certificaat, waardoor het makkelijker voor het testen van HTTPS. De portal vertelt u die u nodig hebt om uw eigen certificaat voor HTTPS, maar dit is het station dat u hebt een certificaat dat bij een subdomein hoort. 

![][3]

Als u probeert gewoon things uit en het maken van een certificaat niet weet, kunt u de toepassing IIS MMC-console een zelfondertekend certificaat maken. U kunt dit exporteren als een .pfx-bestand en upload het in de gebruikersinterface van het certificaat ILB zodra deze is gemaakt. Wanneer u een site die is beveiligd met een zelfondertekend certificaat, uw browser, krijgt u een waarschuwing dat de site die u wilt openen is niet beveiligd vanwege het feit dat niet het certificaat te valideren. Als u wilt voorkomen dat deze waarschuwing moet u een correct ondertekende certificaat die overeenkomt met een subdomein en heeft een vertrouwensketen die door uw browser is herkend.

![][6]

Als u probeert de stroom met uw eigen certificaten en testen van HTTP en HTTPS-toegang tot uw as-omgeving wilt:

1. Ga naar de as-omgeving UI nadat as-omgeving is gemaakt **as-omgeving -> Instellingen -> ILB certificaten**
2. ILB certificaat ingesteld door het pfx-certificaatbestand selecteren en wachtwoord opgeven. Deze stap duurt even om te verwerken en het bericht dat een vergroten/verkleinen uitgevoerd wordt, worden weergegeven.
3. Het adres ILB ophalen voor uw as-omgeving (**as-omgeving -> Eigenschappen van virtuele IP-adres ->**)
4. Een WebApp maken in de as-omgeving na het maken 
5. Een virtuele machine maken als u niet in dit VNET (niet in hetzelfde subnet als het einde van de as-omgeving of dingen hebt)
6. DNS instellen voor een subdomein. U kunt een jokerteken worden gebruikt met een subdomein in uw DNS of als u wilt uitvoeren van enkele eenvoudige tests uit, bewerk het bestand hosts op de virtuele machine in te stellen van web-appnaam aan VIP-IP-adres. Als uw as-omgeving had de subdomeinnaam. ilbase.com en u de web-app mytestapp gemaakt zodat zou worden aangepakt op mytestapp.ilbase.com vervolgens ingesteld die in het hosts-bestand. (In Windows het hosts-bestand is op C:\Windows\System32\drivers\etc\)
7. Een browser gebruiken die op deze virtuele machine en Ga naar http://mytestapp.ilbase.com (of wat de naam van uw web-app met een subdomein is)
8. Een browser gebruiken die op deze virtuele machine en Ga naar https://mytestapp.ilbase.com die u het gebrek aan beveiliging accepteren moet als een zelfondertekend certificaat gebruikt. 

Het IP-adres voor de ILB wordt vermeld in de eigenschappen als het virtuele IP-adres

![][4]

## <a name="using-an-ilb-ase"></a>Met behulp van een as ILB-omgeving
#### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen
Een as ILB-omgeving kunnen netwerkisolatie voor uw apps als de apps niet toegankelijk of zelfs bekend zijn met het internet zijn. Dit is een uitstekende voor het hosten van websites op het intranet zoals line-of-business-toepassingen. Als u nodig hebt om toegang te beperken nog kunt verder u Network Security Groups(NSGs) toegangsbeheer op netwerkniveau. 

Als u wilt gebruiken nsg's verder beperkt toegang moet u om ervoor te zorgen verbreekt u niet de communicatie die de as-omgeving nodig heeft om te werken. Ondanks dat de HTTP/HTTPS-toegang beperken tot de ILB gebruikt door de as-omgeving is steeds het as-omgeving nog gebruik van bronnen buiten het VNet. Om te zien welke netwerktoegang is nog steeds vereist zoekt u naar de informatie in het document op [inkomend verkeer naar een App-serviceomgeving beheren] [ ControlInbound] en het document op [gegevens in de netwerkconfiguratie voor App Service-omgevingen met ExpressRoute][ExpressRoute]. 

U moet weten van het IP-adres dat door Azure wordt gebruikt voor het beheren van uw as-omgeving voor het configureren van uw nsg's. Dat IP-adres is ook het uitgaande IP-adres van uw as-omgeving, als het aanvragen van internet maakt. De uitgaande IP-adres voor uw as-omgeving blijft statische voor de duur van de as-omgeving. Als u verwijderen en opnieuw maken van uw as-omgeving, krijgt u een nieuw IP-adres. Om te zoeken naar dit IP-adres gaan **instellingen -> eigenschappen** en zoek de **uitgaande IP-adres**. 

![][5]

#### <a name="general-ilb-ase-management"></a>Beheer van algemene ILB as-omgeving
Het beheren van een as ILB-omgeving is grotendeels hetzelfde als normaal gesproken een as-omgeving beheren. U moet uw werknemersgroepen meer exemplaren van de ASP-host en schalen van de front-end-servers voor het afhandelen van toegenomen hoeveelheden HTTP/HTTPS-verkeer opschalen. Voor algemene informatie over het beheren van de configuratie van een as-omgeving, raadpleegt u het document op [configureren van een App-serviceomgeving][ASEConfig]. 

De items aanvullend beheer zijn Certificaatbeheer en DNS-beheer. U moet ophalen en upload het certificaat dat wordt gebruikt voor HTTPS na ILB as-omgeving maken en deze vervangen voordat het verloopt. Omdat Azure eigenaar is van het basisdomein bieden we certificaten voor ASEs met een externe VIP. Aangezien het subdomein dat wordt gebruikt door een ILB-as-omgeving van alles zijn kan, moet u uw eigen certificaat opgeven voor HTTPS. 

#### <a name="dns-configuration"></a>DNS-configuratie
Als u een externe VIP de DNS-server wordt beheerd door Azure. Elke app gemaakt in uw as-omgeving wordt automatisch toegevoegd aan Azure DNS die een openbare DNS-server. In een ILB-as-omgeving hebt u voor het beheren van uw eigen DNS. Voor een bepaalde subdomein zoals contoso.corp.net moet u DNS A-records die verwijzen naar uw adres ILB voor maken:

    * 
    *.SCM ftp publiceren 


## <a name="getting-started"></a>Aan de slag
Om aan de slag met App Service-omgevingen, Zie [Inleiding tot de App Service-omgevingen][WhatisASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[ControlInbound]: app-service-app-service-environment-control-inbound-traffic.md
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[ASEAutoscale]: app-service-environment-auto-scale.md
[ExpressRoute]: app-service-app-service-environment-network-configuration-expressroute.md
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: app-service-web-configure-an-app-service-environment.md
