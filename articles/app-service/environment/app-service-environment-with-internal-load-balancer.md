---
title: Het maken en gebruiken van een interne Load Balancer met een App Service Environment | Microsoft Docs
description: Het maken en gebruiken van een as-omgeving met een ILB
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: ad9a1e00-d5e5-413e-be47-e21e5b285dbf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: fe447a655f579441be667d7555aa83faf2b9385d
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52957007"
---
# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Met behulp van een interne Load Balancer met een App Service Environment

> [!NOTE] 
> In dit artikel gaat over de App Service Environment v1. Er is een nieuwere versie van de App Service Environment die gebruiksvriendelijker en wordt uitgevoerd op een krachtigere infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot App Service Environment](intro.md).
>

De functie App Service Environment (ASE) is een Premium-service van Azure App Service die zorgt voor een verbeterde mogelijkheden die niet beschikbaar in de stempels voor meerdere tenants. De functie voor ASE implementeert in feite de Azure App Service in uw Azure virtuele netwerk (vnet). Om te krijgen een beter inzicht in de mogelijkheden die door App Service-omgevingen lezen de [wat is er een App Service Environment] [ WhatisASE] documentatie. Als u niet weet de voordelen wat van het besturingssysteem in een VNet lezen de [virtuele netwerk Veelgestelde vragen over Azure][virtualnetwork]. 

## <a name="overview"></a>Overzicht
Een as-omgeving kan worden geïmplementeerd met een eindpunt dat internet toegankelijk is of met een IP-adres in uw VNet. Om in te stellen het IP-adres aan een VNet-adres moet u uw as-omgeving met een interne Load Balancer(ILB) implementeren. Wanneer de as-omgeving is geconfigureerd met een ILB, moet u het volgende opgeven:

* uw eigen domein of subdomein. Om het eenvoudig maken, in dit document wordt ervan uitgegaan dat subdomein, maar kunt u deze configureren in beide gevallen. 
* het certificaat wordt gebruikt voor HTTPS
* DNS-beheer voor het subdomein. 

Hierna kunt u dingen doen zoals:

* host intranettoepassingen, zoals line-of-business-toepassingen, veilig in de cloud die toegankelijk zijn via een Site naar Site- of ExpressRoute-VPN
* host-apps in de cloud die niet worden vermeld in de openbare DNS-servers
* internet (geïsoleerd) back-end-apps waarmee front-end-apps veilig kunnen worden geïntegreerd maken

#### <a name="disabled-functionality"></a>Uitgeschakelde functionaliteit
Er zijn enkele dingen die u niet kunt doen wanneer u een ILB as-omgeving. Die dingen zijn onder andere:

* met behulp van IPSSL
* IP-adressen toewijzen aan specifieke apps
* kopen en met behulp van een certificaat met een app via de portal. U kunt natuurlijk nog steeds certificaten rechtstreeks bij een certificeringsinstantie verkrijgen en gebruiken met uw apps, maar niet via de Azure-portal.

## <a name="creating-an-ilb-ase"></a>Het maken van een ILB as-omgeving
Het maken van een ILB as-omgeving verschilt niet veel van normaal gesproken een as-omgeving maken. Zie voor meer gedetailleerde informatie over het maken van een as-omgeving, [over het maken van een App Service Environment][HowtoCreateASE]. Het proces voor het maken van een ILB as-omgeving is hetzelfde tussen een VNet maken tijdens het maken van ASE of een bestaande VNet selecteren. Ga als volgt te werk om een ILB AS-omgeving te maken: 

1. Selecteer in de Azure portal, **maken een resource -> Web en mobiel -> App Service-omgeving**.
2. Selecteer uw abonnement.
3. Selecteer of maak een resourcegroep.
4. Selecteer of maak een VNet.
5. Maak een subnet als het selecteren van een VNet.
6. Selecteer **virtueel netwerk/locatie-VNet-configuratie >** en stel de VIP-Type op interne.
7. Geef de subdomeinnaam van een (deze naam is het subdomein dat wordt gebruikt voor de apps die in deze as-omgeving gemaakt).
8. Selecteer **OK** en vervolgens **maken**.

![][1]

In het Virtueelnetwerk, moet u er een optie waarmee u kunt kiezen tussen een extern VIP-adres of een intern VIP VNet-configuratie is. De standaardwaarde is extern. Als u ingesteld op externe hebt, de as-omgeving maakt gebruik van een internet toegankelijk VIP-adres. Als u interne selecteert, wordt de as-omgeving is geconfigureerd met een ILB op een IP-adres binnen uw VNet. 

Na het selecteren van interne, de mogelijkheid meer IP-adressen toevoegen aan de as-omgeving is verwijderd en in plaats daarvan moet u het subdomein van de as-omgeving. In een as-omgeving met een extern VIP-adres, de naam van de as-omgeving gebruikt in het subdomein voor apps die zijn gemaakt in deze as-omgeving. Als de naam van de as-omgeving ***contosotest*** en uw app in deze as-omgeving met de naam ***mytest***, het subdomein van de indeling is ***contosotest.p.azurewebsites.net*** en de URL voor die app is ***mytest.contosotest.p.azurewebsites.net***. Als u de VIP-Type op interne instelt, wordt de naam van uw as-omgeving niet gebruikt in het subdomein voor de as-omgeving. U opgeven het subdomein expliciet. Als uw subdomein ***contoso.corp.net*** en u een app gemaakt in deze as-omgeving met de naam ***timereporting***, is de URL voor die app ***timereporting.contoso.corp.net***.

## <a name="apps-in-an-ilb-ase"></a>Apps in een ILB as-omgeving
Het maken van een app in een ILB as-omgeving, is hetzelfde als voor het maken van een app in een as-omgeving is het normaal. 

1. Selecteer in de Azure portal, **maken een resource -> Web en mobiel -> Web** of **Mobile** of **API-App**.
2. Voer de naam van de app in.
3. Selecteer uw abonnement.
4. Selecteer of maak een resourcegroep.
5. Selecteer of maak een App Service-Plan(ASP). Als het maken van een nieuwe ASP, selecteert u uw as-omgeving als de locatie en selecteer de worker-groep die u wilt dat uw ASP moet worden gemaakt. Wanneer u de ASP maakt, selecteert u de as-omgeving als de locatie en de groep met werkrollen. Wanneer u de naam van de app opgeeft, ziet u dat het subdomein onder de appnaam van uw is vervangen door het subdomein van uw as-omgeving. 
6. Selecteer **Maken**. Zorg ervoor dat u selecteert de **vastmaken aan dashboard** selectievakje in als u wilt dat de app op uw dashboard wordt weergegeven. 

![][2]

Naam van het subdomein wordt onder de naam van de app bijgewerkt om het subdomein van de as-omgeving weer te geven. 

## <a name="post-ilb-ase-creation-validation"></a>Validatie van post ILB as-omgeving maken
Een ILB AS-omgeving verschilt iets van de niet-ILB AS-omgeving. Als al vermeld, moet u uw eigen DNS beheren en u moet ook uw eigen certificaat voor HTTPS-verbindingen opgeven. 

Nadat u uw as-omgeving maakt, zult u merken dat het subdomein ziet u het subdomein u hebt opgegeven en er is een nieuw item in de **instelling** menu met de naam **ILB-certificaat**. De as-omgeving wordt gemaakt met een zelfondertekend certificaat, waardoor het gemakkelijker voor het testen van HTTPS. De portal ziet u die u nodig hebt voor uw eigen certificaat voor HTTPS, maar dit is om aan te moedigen u aan een certificaat hebben dat met het subdomein gaat. 

![][3]

Als u dingen gewoon uit te proberen zijn en u weet niet hoe u een certificaat maakt, kunt u de IIS MMC-console-toepassing te maken van een zelfondertekend certificaat. Zodra deze is gemaakt, kunt u dit exporteren als een pfx-bestand en upload het in de gebruikersinterface van de ILB. Wanneer u toegang een site die is beveiligd met een zelf-ondertekend certificaat tot, kunt u in uw browser een waarschuwing dat de site die u wilt openen is niet beveiligd vanwege de modeldatabase groter om het certificaat te valideren. Als u voorkomen dat de waarschuwing wilt, moet u een correct ondertekende certificaat die overeenkomt met het subdomein en bevat een keten van vertrouwen dat wordt herkend door uw browser.

![][6]

Als u wilt proberen de stroom met uw eigen certificaten en testen van HTTP en HTTPS-toegang tot de as-omgeving:

1. Ga naar de gebruikersinterface van de as-omgeving nadat de as-omgeving is gemaakt **ASE -> Instellingen -> ILB certificaten**.
2. ILB-certificaat ingesteld door het pfx-certificaatbestand selecteren en wachtwoord opgeven. Deze stap neemt enige tijd om te verwerken en het bericht dat een bewerking voor vergroten/verkleinen uitgevoerd wordt wordt weergegeven.
3. Ophalen van het ILB-adres voor de as-omgeving (**ASE -> Eigenschappen van virtuele IP-adres ->**).
4. Een web-app maken in as-omgeving nadat is gemaakt. 
5. Een virtuele machine maken als u geen in dit VNET (niet in hetzelfde subnet bevindt als het einde van de ASE of things).
6. DNS instellen voor het subdomein. U kunt een jokerteken gebruiken bij uw subdomein in DNS of als u wilt doen enkele eenvoudige tests uit, bewerkt u het hosts-bestand op de virtuele machine de naam van de web-app instellen op IP-adres. Als de as-omgeving heeft de naam van het subdomein. ilbase.com en u de web-app mytestapp gemaakt zodat deze kan worden opgelost op mytestapp.ilbase.com, stelt u die in het hosts-bestand. (Op Windows, het hosts-bestand is op C:\Windows\System32\drivers\etc\)
7. Gebruik een browser op deze VM en Ga naar https://mytestapp.ilbase.com (of wat de naam van uw web-app met een subdomein is).
8. Gebruik een browser op die virtuele machine en ga naar https://mytestapp.ilbase.com. Als een zelfondertekend certificaat gebruikt, moet u het gebrek aan beveiliging accepteren. 

Het IP-adres voor uw ILB wordt vermeld in de eigenschappen van uw als het virtuele IP-adres.

![][4]

## <a name="using-an-ilb-ase"></a>Een App Service Environment met interne Load Balancer gebruiken
#### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen
Een ILB as-omgeving kunt netwerkisolatie voor uw apps. De apps zijn niet toegankelijk of zelfs bekend zijn met het internet. Deze aanpak is uitermate geschikt voor het hosten van intranetsites, zoals line-of-business-toepassingen. Wanneer u nodig hebt om toegang nog verder te beperken, kunt u Network Security Groups(NSGs) nog steeds gebruiken voor het beheren van toegang op het niveau van het netwerk. 

Als u gebruiken van nsg's wilt voor het verder beperken van toegang, moet u ervoor zorgen dat u de communicatie die de as-omgeving nodig heeft om te kunnen werken niet verbreken. Hoewel de HTTP/HTTPS-toegang alleen via de ILB die worden gebruikt door de as-omgeving is, wordt de as-omgeving nog steeds voor bronnen buiten het VNet. Als u wilt zien welke toegang tot het netwerk is nog steeds vereist, Zie [binnenkomende verkeer beheren in een App Service Environment] [ ControlInbound] en [Netwerkconfiguratiedetails voor App Service-omgevingen met ExpressRoute][ExpressRoute]. 

Als u wilt uw nsg's configureren, moet u de IP-adres dat wordt gebruikt door Azure voor het beheren van de as-omgeving weten. Dat IP-adres is ook het uitgaande IP-adres van de as-omgeving als het aanvragen van internet maakt. De uitgaande IP-adres voor de as-omgeving blijft statische voor de levensduur van de as-omgeving. Als u verwijderen en opnieuw maken van de as-omgeving, krijgt u een nieuw IP-adres. Als u het IP-adres zoekt, gaat u naar **instellingen -> eigenschappen** en zoek de **uitgaande IP-adres**. 

![][5]

#### <a name="general-ilb-ase-management"></a>Algemene ILB as-omgeving beheren
Een ILB as-omgeving te beheren is grotendeels hetzelfde als normaal gesproken een as-omgeving beheren. U moet uw werknemersgroepen meer ASP-instanties te hosten en schalen van uw front-endwebservers om af te handelen van grotere hoeveelheden HTTP/HTTPS-verkeer opschalen. Raadpleeg voor algemene informatie over het beheren van de configuratie van een as-omgeving [configureren van een App Service Environment][ASEConfig]. 

De items aanvullend beheer zijn Certificaatbeheer en DNS-beheer. U moet ophalen en upload het certificaat wordt gebruikt voor HTTPS nadat de ILB as-omgeving is gemaakt en vervangen voordat het verloopt. Omdat Azure eigenaar is van het basis-domein, kan deze levert certificaten voor as-omgevingen met een extern VIP-adres. Omdat het subdomein dat wordt gebruikt door een ILB as-omgeving van alles zijn kan, moet u uw eigen certificaat opgeven voor HTTPS. 

#### <a name="dns-configuration"></a>DNS-configuratie
Wanneer u een extern VIP-adres gebruikt, wordt de DNS-server wordt beheerd door Azure. Elke app die is gemaakt in de AS-omgeving wordt automatisch toegevoegd aan Azure DNS, wat een openbaar DNS is. In een ILB AS-omgeving moet u uw eigen DNS beheren. Voor een bepaalde subdomein, zoals contoso.corp.net, moet u DNS A-records die verwijzen naar uw ILB-adres voor maken:

    * 
    *.SCM ftp publiceren 


## <a name="getting-started"></a>Aan de slag
Als u wilt aan de slag met App Service-omgevingen, Zie [Inleiding tot App Service-omgevingen][WhatisASE]

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
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/
[ASEAutoscale]: app-service-environment-auto-scale.md
[ExpressRoute]: app-service-app-service-environment-network-configuration-expressroute.md
[vnetnsgs]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: app-service-web-configure-an-app-service-environment.md
