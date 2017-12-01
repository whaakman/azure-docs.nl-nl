---
title: Uw Azure App Service-omgeving te configureren via een tunnel forceren
description: Uw App Service-omgeving werken als uitgaand verkeer via een tunnel force wordt inschakelen
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
ms.openlocfilehash: f5f099042cefe666e22a9d561afeb4584db3d92c
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Uw App Service-omgeving met geforceerde tunneling configureren

De App Service-omgeving is een implementatie van Azure App Service in een klant-exemplaar van Azure Virtual Network. Veel klanten hun virtuele netwerken om te worden van extensies van hun on-premises netwerken met VPN-verbindingen of Azure ExpressRoute configureren verbindingen. Vanwege bedrijfsbeleid of andere beveiligingsbeperkingen configureren ze routes voor het verzenden van alle uitgaande verkeer lokale voordat het met internet kunt gaat. Het wijzigen van de routering van het virtuele netwerk, zodat het uitgaande verkeer van het virtuele netwerk, via de VPN- of ExpressRoute-verbinding met on-premises loopt wordt geforceerde tunneling genoemd. 

Geforceerde tunneling kan problemen veroorzaken voor een App Service-omgeving. De App Service-omgeving heeft een aantal externe afhankelijkheden die zijn geïnventariseerd in de [App Service-omgeving netwerkarchitectuur] [ network] document. De App-serviceomgeving moet standaard alle uitgaande communicatie, doorloopt aan de VIP is ingericht met de App Service-omgeving.

Routes zijn een essentieel aspect van welke geforceerde tunneling is en het omgaan met het. In een Azure-netwerk routering plaats op basis van het langste voorvoegsel overeen (LPM). Als er meer dan één route met dezelfde overeenkomende LPM is, wordt er een route geselecteerd op basis van de oorsprong in de volgende volgorde:

* Gebruiker gedefinieerde route (UDR)
* BGP-route (wanneer u ExpressRoute gebruikt)
* Systeemroute

Lees voor meer informatie over routering in een virtueel netwerk [gebruiker gedefinieerde routes en doorsturen via IP][routes]. 

Als u wilt dat uw App Service-omgeving werkt met een virtueel netwerk geforceerde tunnel, hebt u twee opties:

* Schakel uw App Service-omgeving directe toegang tot internet hebben.
* Het eindpunt uitgaande wijzigen voor uw App Service-omgeving.

## <a name="enable-your-app-service-environment-to-have-direct-internet-access"></a>Uw App Service-omgeving directe toegang tot internet hebben inschakelen

U kunt voor uw App Service-omgeving werken terwijl het virtuele netwerk is geconfigureerd met een ExpressRoute-verbinding:

* ExpressRoute voor het adverteren 0.0.0.0/0 configureren. Standaard deze geforceerd tunnels alle uitgaande verkeer van de lokale.
* Maak een UDR. Toepassen op het subnet waarin de App Service-omgeving met een adresvoorvoegsel van 0.0.0.0/0 en een volgend hoptype van Internet.

Als u deze twee wijzigingen aanbrengt, internet bestemd verkeer dat afkomstig van het subnet van de App Service-omgeving is wordt niet uitgevoerd op de ExpressRoute-verbinding en de App-serviceomgeving werkt.

> [!IMPORTANT]
> De routes die zijn gedefinieerd in een UDR moet specifieke voorrang op alle routes die worden geadverteerd door de ExpressRoute-configuratie. Het adresbereik brede 0.0.0.0/0 maakt gebruik van het vorige voorbeeld. Mogelijk kan worden per ongeluk overschreven door de route-advertisements met meer specifieke adresbereiken.
>
> App Service-omgevingen worden niet ondersteund met ExpressRoute-configuraties die cross routes van het openbare peering pad naar het pad voor persoonlijke peering adverteert. ExpressRoute-configuraties met openbare peering geconfigureerd ontvangen route-advertisements van Microsoft. De aankondigingen bevatten een groot aantal Microsoft Azure-IP-adresbereiken. Als de adresbereiken cross aangekondigd op het pad voor persoonlijke peering, zijn alle uitgaande pakketten van de App Service-omgeving subnet force tunneled aan een klant on-premises netwerkinfrastructuur. Deze stroom netwerk is momenteel niet ondersteund met App Service-omgevingen standaard. Er is een oplossing voor dit probleem cross-adverteert routes van het pad voor openbare peering naar het pad voor persoonlijke peering stoppen. Er is een andere oplossing zodat uw App Service-omgeving om te werken in een configuratie geforceerde tunnel.

## <a name="change-the-egress-endpoint-for-your-app-service-environment"></a>Het eindpunt uitgaande wijzigen voor uw App Service-omgeving ##

Deze sectie beschrijft het inschakelen van een App-serviceomgeving functioneren in een configuratie geforceerde tunnel door het wijzigen van de uitgaande eindpunt dat wordt gebruikt door de App Service-omgeving. Als het uitgaande verkeer van de App-serviceomgeving geforceerd naar een on-premises netwerk via een tunnel is, moet u dat verkeer naar de bron van de IP-adressen dan het App Service-omgeving VIP-adres toestaan.

Een App Service-omgeving niet alleen heeft externe afhankelijkheden, maar ook moet luisteren voor binnenkomend verkeer en reageren op dergelijke verkeer. De antwoorden kunnen niet worden verzonden door een ander adres, omdat dat TCP wordt verbroken. Er zijn drie stappen nodig voor het wijzigen van het uitgaande-eindpunt voor de App Service-omgeving:

1. Stel een routetabel om ervoor te zorgen dat inkomende beheer van verkeer kan gaan van hetzelfde IP-adres.

2. Toevoegen van uw IP-adressen die moeten worden gebruikt voor uitgaande aan de firewall App Service-omgeving.

3. De routes instellen op uitgaand verkeer van de App Service-omgeving worden via een tunnel.

   ![Gedwongen tunnel netwerk stroom][1]

U kunt de App Service-omgeving met andere uitgaande adressen configureren nadat de App Service-omgeving al omhoog en operationele is of ze kunnen worden ingesteld tijdens de implementatie van App Service-omgeving.

### <a name="change-the-egress-address-after-the-app-service-environment-is-operational"></a>Het adres van de uitgaande wijzigen nadat de App-serviceomgeving operationeel is ###
1. Ophalen van de IP-adressen die u wilt gebruiken als uitgaande IP-adressen voor uw App Service-omgeving. Als u geforceerde tunneling alleen, is deze adressen afkomstig zijn van de NAT- of gateway IP-adressen. Als u wilt dat het uitgaande verkeer van de App Service-omgeving door middel van een NVA sturen, is het adres van de uitgaande het openbare IP-adres van de NVA.

2. De uitgaande adressen in de configuratie-informatie van uw App Service-omgeving instellen. Ga naar resource.azure.com en gaat u naar abonnement /<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name>. Vervolgens ziet u de JSON die uw App Service-omgeving beschrijft. Controleer of **lezen/schrijven** aan de bovenkant. Selecteer **bewerken**. Schuif helemaal naar beneden en wijzig de **userWhitelistedIpRanges** waarde uit de **null** naar ongeveer als volgt. De adressen die u wilt instellen als het adresbereik uitgaande gebruiken. 

        "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

   Selecteer **plaatsen** aan de bovenkant. Deze optie wordt een schaalaanpassing van uw App Service-omgeving wordt geactiveerd en past u de firewall.
 
3. Maken of bewerken van een routetabel en de regels voor toegang van de adressen van de beheerserver die zijn toegewezen aan de locatie van uw App Service-omgeving te vullen. De adressen van de beheerserver Zie [adressen van de App Service-omgeving beheerserver][management].

4. Corrigeer de routes die worden toegepast op het subnet van de App Service-omgeving met een routetabel of BGP-routes. 

Als de App Service-omgeving niet meer via de portal wordt, is er een probleem met uw wijzigingen. Het probleem kan zijn dat uw lijst met uitgaande adressen onvolledig is, het verkeer verbroken is of het verkeer is geblokkeerd. 

### <a name="create-a-new-app-service-environment-with-a-different-egress-address"></a>Een nieuwe App Service-omgeving met een andere uitgaande-adres maken ###

Als het virtuele netwerk al is geconfigureerd om af te dwingen tunnel alle verkeer, moet u extra stappen uitvoeren om te maken van uw App Service-omgeving, zodat deze afkomstig van is zijn kan. U moet het gebruik van een ander uitgaande eindpunt inschakelen tijdens het maken van App Service-omgeving. Om dit te doen, moet u de App Service-omgeving maken met een sjabloon die de toegestane uitgaande adressen bevat.

1. Ophalen van de IP-adressen worden gebruikt als de uitgaande adressen voor uw App Service-omgeving.

2. Het subnet moet worden gebruikt door de App-serviceomgeving vooraf maken. U moet deze zodat u dat routes instellen kunt, en ook omdat de sjabloon is vereist.

3. Een routetabel maken met het beheer van IP-adressen die zijn toegewezen aan de locatie van uw App Service-omgeving. Wijs het toe aan uw App Service-omgeving.

4. Volg de aanwijzingen in [een App Service-omgeving maken met een sjabloon][template]. Open de geschikte sjabloon.

5. Bewerk de bronnensectie '' in het bestand azuredeploy.json. Een regel voor opnemen **userWhitelistedIpRanges** door de waarden als volgt:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Als deze sectie correct is geconfigureerd, moet de App Service-omgeving zonder problemen start. 


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
