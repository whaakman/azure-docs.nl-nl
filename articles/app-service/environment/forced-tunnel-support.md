---
title: Uw Azure App Service-omgeving te configureren via een tunnel forceren
description: Uw as-omgeving werken als uitgaand verkeer via een tunnel force wordt inschakelen
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 384cf393-5c63-4ffb-9eb2-bfd990bc7af1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: ccompy
ms.openlocfilehash: cd89bd23074dec1de6fa0e8784d42a5c539938b1
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Uw App Service-omgeving met geforceerde tunneling configureren

De as-omgeving (App Service omgeving) is een implementatie van de Azure App Service in een klant Azure Virtual Network (VNet). Veel klanten hun VNets als extensies van hun on-premises netwerken met VPN-verbindingen of ExpressRoute configureren verbindingen. Vanwege bedrijfsbeleid of andere beveiligingsbeperkingen configureren ze routes voor het verzenden van alle uitgaande verkeer lokale voordat het met internet kunt gaat. Het wijzigen van de routering van de VNet, zodat het uitgaande verkeer van het VNet via de VPN- of ExpressRoute-verbinding met on-premises loopt wordt geforceerde tunneling genoemd.  

Geforceerde tunneling kan problemen veroorzaken voor een as-omgeving. De as-omgeving heeft een aantal externe afhankelijkheden die zijn geïnventariseerd op dit [as-omgeving netwerkarchitectuur] [ network] document. De as-omgeving, vereist standaard dat alle uitgaande communicatie doorloopt de VIP is ingericht met de as-omgeving.

Routes zijn een essentieel aspect wat geforceerde tunneling is en het omgaan met het. In een virtueel netwerk in Azure routering gebeurt op basis van langste voorvoegsel overeen (LPM).  Als er meer dan één route met dezelfde overeenkomende LPM is, klikt u vervolgens een route geselecteerd op basis van de oorsprong in de volgende volgorde:

1. Door de gebruiker gedefinieerde route
1. BGP-route (wanneer u ExpressRoute gebruikt)
1. Systeemroute

Lees voor meer informatie over routering in een VNet [gebruiker gedefinieerde routes en doorsturen via IP][routes]. 

Als u uw as-omgeving wilt werkt met een geforceerde tunnel VNet, hebt u twee opties:

1. Uw as-omgeving directe toegang tot internet hebben inschakelen
1. Het eindpunt uitgaande wijzigen voor uw as-omgeving

## <a name="enable-your-ase-to-have-direct-internet-access"></a>Uw as-omgeving directe toegang tot internet hebben inschakelen

Voor uw as werken terwijl uw VNet is geconfigureerd met een ExpressRoute-omgeving, kunt u het volgende doen:

* ExpressRoute voor het adverteren 0.0.0.0/0 configureren. Standaard deze geforceerd tunnels alle uitgaande verkeer van de lokale.
* Maak een UDR. Toepassen op het subnet waarin de as-omgeving met een adresvoorvoegsel van 0.0.0.0/0 en een volgend hoptype van Internet.

Als u deze twee wijzigingen aanbrengt, wordt niet internet bestemd verkeer dat afkomstig van het subnet van de as-omgeving is uitgevoerd op de ExpressRoute- en de as-omgeving werkt.

> [!IMPORTANT]
> De routes die zijn gedefinieerd in een UDR moet specifieke voorrang op alle routes die worden geadverteerd door de ExpressRoute-configuratie. Het adresbereik brede 0.0.0.0/0 maakt gebruik van het vorige voorbeeld. Mogelijk kan worden per ongeluk overschreven door de route-advertisements met meer specifieke adresbereiken.
>
> ASEs worden niet ondersteund met ExpressRoute-configuraties die cross routes van het openbare peering pad naar het pad voor persoonlijke peering adverteert. ExpressRoute-configuraties met openbare peering geconfigureerd ontvangen route-advertisements van Microsoft. De aankondigingen bevatten een groot aantal Microsoft Azure-IP-adresbereiken. Als de adresbereiken cross aangekondigd op het pad voor persoonlijke peering, zijn alle uitgaande pakketten van subnet van de as-omgeving force tunneled aan een klant on-premises netwerkinfrastructuur. Deze stroom netwerk is momenteel niet ondersteund met ASEs standaard. Er is een oplossing voor dit probleem cross-adverteert routes van het pad voor openbare peering naar het pad voor persoonlijke peering stoppen.  De andere oplossing is om in te schakelen uw as-omgeving te laten werken in een geforceerde tunnel te configureren.

## <a name="change-the-egress-endpoint-for-your-ase"></a>Het eindpunt uitgaande wijzigen voor uw as-omgeving ##

Deze sectie wordt beschreven hoe een as-omgeving in een configuratie geforceerde tunnel functioneren door het wijzigen van de uitgaande eindpunt dat wordt gebruikt door de as-omgeving inschakelen. Als het uitgaande verkeer van de as-omgeving wordt afgedwongen via een tunnel naar een on-premises netwerk, moet u die verkeer naar de bron van de IP-adressen dan het as-omgeving VIP-adres toestaan.

Een as-omgeving is niet alleen externe afhankelijkheden, maar deze ook moet luisteren voor binnenkomend verkeer voor het beheren van de as-omgeving. De as-omgeving moet kunnen reageren op dergelijke verkeer en de antwoorden kunnen niet worden verzonden door een ander adres zoals dat TCP wordt verbroken.  Er zijn dus drie stappen nodig voor het wijzigen van het uitgaande-eindpunt voor de as-omgeving.

1. Een routetabel om ervoor te zorgen dat inkomende beheer van verkeer kan gaan van hetzelfde IP-adres instellen
1. Voeg dat uw IP-adressen die moet worden gebruikt voor uitgaande aan de firewall as-omgeving
1. De routes instellen op uitgaand verkeer van de as-omgeving om te worden via een tunnel

![Gedwongen tunnel netwerk stroom][1]

U kunt de as-omgeving met andere uitgaande adressen configureren nadat de as-omgeving al omhoog en operationele is of ze kunnen worden ingesteld tijdens de implementatie van de as-omgeving.  

### <a name="changing-the-egress-address-after-the-ase-is-operational"></a>Het adres van de uitgaande wijzigen nadat de as-omgeving operationeel is ###
1. Ophalen van de IP-adressen die u wilt gebruiken als uitgaande IP-adressen voor uw as-omgeving. Als u geforceerde tunneling, en vervolgens uw NAT's of de gateway IP-adressen dit zou.  Als u wilt dat het uitgaande verkeer van de as-omgeving door middel van een NVA sturen, zou het adres van de uitgaande het openbare IP-adres van de NVA zijn.
2. De uitgaande adressen in de configuratiegegevens van de as-omgeving instellen. Ga naar resource.azure.com en gaat u naar: abonnement /<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name> kunt u de json die uw as-omgeving beschrijft zien.  Zorg ervoor dat het lezen/schrijven bovenaan staat.  Klik op bewerken Schuif helemaal naar beneden en userWhitelistedIpRanges van wijzigen  

       "userWhitelistedIpRanges": null 
      
  op ongeveer als volgt. De adressen die u wilt instellen als het adresbereik uitgaande gebruiken. 

      'userWhitelistedIpRanges': ['11.22.33.44/32', '55.66.77.0/24'] 

  Klik op opslag aan de bovenkant. Dit activeert een scale-bewerking op de as-omgeving en de firewall aanpassen.
   
3. Maken of bewerken van een routetabel en de regels voor toegang van de adressen van de beheerserver die zijn toegewezen aan uw locatie as-omgeving te vullen.  De adressen van de beheerserver zijn hier [adressen van de beheerserver App Service-omgeving][management] 

4. Corrigeer de routes die worden toegepast op het subnet van de as-omgeving met een routetabel of BGP-routes.  

Als de as-omgeving niet meer via de portal wordt, is er een probleem met uw wijzigingen.  Het kan zijn dat uw lijst met uitgaande adressen onvolledig is, het verkeer verbroken is of het verkeer is geblokkeerd.  

### <a name="create-a-new-ase-with-a-different-egress-address"></a>Een nieuwe as-omgeving met een andere uitgaande-adres maken  ###

In het geval dat uw VNet is al geconfigureerd voor het afdwingen van de tunnel alle verkeer, moet u een aantal extra stappen voor het maken van uw as-omgeving zodanig dat het met succes kunt actief houden. Dit betekent dat u moet het gebruik van een ander uitgaande eindpunt inschakelen tijdens het maken van de as-omgeving.  Om dit te doen, moet u het as-omgeving maken met een sjabloon die de toegestane uitgaande adressen bevat.

1. Ophalen van de IP-adressen moet worden gebruikt als de uitgaande adressen voor uw as-omgeving.
1. Vooraf maken het subnet moet worden gebruikt door de as-omgeving. Dit is nodig waarmee u kunt routes instellen en ook omdat de sjabloon is vereist.  
1. Een routetabel maken met het beheer van IP-adressen toewijzen aan de locatie van uw as-omgeving en deze toewijzen aan uw as-omgeving
1. Volg de aanwijzingen hier [een as-omgeving maken met een sjabloon][template], en het meest geschikte sjabloon
1. Bewerk de sectie azuredeploy.json bestand 'resources'. Een regel voor opnemen **userWhitelistedIpRanges** door de waarden zoals:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Als dit correct is geconfigureerd, moet de as-omgeving start zonder problemen.  


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
