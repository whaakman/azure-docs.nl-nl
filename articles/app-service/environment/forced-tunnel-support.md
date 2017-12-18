---
title: De Azure App Service-omgeving configureren om geforceerde tunnels te gebruiken
description: Zorg ervoor dat de App Service-omgeving werkt wanneer uitgaand verkeer gebruikmaakt van geforceerde tunnels
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 384cf393-5c63-4ffb-9eb2-bfd990bc7af1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/10/2017
ms.author: ccompy
ms.custom: mvc
ms.openlocfilehash: 4caaf0df3f1dd4b2cb9b76283a6beed897531c1c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>De Azure App Service-omgeving configureren met geforceerde tunnels

De App Service-omgeving is een implementatie van Azure App Service in het klantenexemplaar van een virtueel Azure-netwerk. Veel klanten configureren hun virtuele netwerken zodat deze uitbreidingen zijn van hun on-premises netwerken met VPN’s of Azure ExpressRoute-verbindingen. Vanwege bedrijfsbeleid of andere beveiligingsbeperkingen configureren ze routes om al het uitgaande verkeer on-premises te verzenden, voordat het zichtbaar is op internet. Geforceerde tunnels: het wijzigen van de routering van het virtuele netwerk zodat uitgaand verkeer dat afkomstig is van dit virtuele netwerk, via een VPN- of ExpressRoute-verbinding naar on-premises wordt verzonden. 

Geforceerde tunnels kunnen problemen veroorzaken voor een App Service-omgeving. De App Service-omgeving heeft een aantal extra afhankelijkheden, die worden opgesomd in het document [App Service Environment network architecture][network] (Netwerkarchitectuur van App Service-omgeving). Voor de App Service-omgeving is standaard vereist dat alle uitgaande communicatie verloopt via het VIP-adres dat is ingericht met de App Service-omgeving.

Routes zijn een essentieel onderdeel van geforceerde tunnels en hoe deze werken. In een virtueel Azure-netwerk vindt routering plaats op basis van LPM (Longest Prefix Match). Als er meer dan één route met dezelfde overeenkomende LPM is, wordt een route geselecteerd op basis van de oorsprong in de volgende volgorde:

* UDR (door de gebruiker opgegeven route)
* BGP-route (wanneer u ExpressRoute gebruikt)
* Systeemroute

Lees [User-defined routes en doorsturen via IP][routes] (Door de gebruiker opgegeven routes en Doorsturen via IP) voor meer informatie over routering in een virtueel netwerk . 

Als u wilt dat de App Service-omgeving werkt met een virtueel netwerk met geforceerde tunnels, hebt u twee opties:

* Zorg ervoor dat de App Service-omgeving rechtstreeks toegang heeft tot internet.
* Wijzig het uitgaande eindpunt voor de App Service-omgeving.

## <a name="enable-your-app-service-environment-to-have-direct-internet-access"></a>Ervoor zorgen dat de App Service-omgeving rechtstreeks toegang heeft tot internet

Doe het volgende om ervoor te zorgen dat de App Service-omgeving werkt wanneer het virtuele netwerk wordt geconfigureerd met een ExpressRoute-verbinding:

* Configureer ExpressRoute om 0.0.0.0/0 te adverteren. Standaard maakt hierdoor al het uitgaande verkeer dat naar on-premises locaties wordt verzonden gebruik van geforceerde tunnels.
* Maak een UDR. Pas deze toe op het subnet dat de App Service-omgeving bevat met het adresvoorvoegsel 0.0.0.0/0 gevolgd door een hoptype van internet.

Als u deze twee wijzigingen aanbrengt, wordt internetverkeer dat afkomstig is van het subnet van de App Service-omgeving niet gedwongen om de ExpressRoute-verbinding te gebruiken, en werkt de App Service-omgeving naar behoren.

> [!IMPORTANT]
> De routes die zijn gedefinieerd in een UDR, moeten specifiek genoeg zijn om voorrang te krijgen boven alle routes die worden geadverteerd met de ExpressRoute-configuratie. In het voorgaande voorbeeld wordt het brede adresbereik 0.0.0.0/0 gebruikt. Dit bereik kan mogelijk per ongeluk worden overschreven door routeadvertenties die gebruikmaken van specifiekere adresbereiken.
>
> App Service-omgevingen worden niet ondersteund met ExpressRoute-configuraties die advertentieoverschrijdende routes gebruiken van het openbare-peeringpad naar het privépeeringpad. ExpressRoute-configuraties waarvoor openbare peering is geconfigureerd, ontvangen routeadvertenties van Microsoft. De advertenties bevatten een groot aantal IP-adresbereiken van Microsoft Azure. Als de adresbereiken advertentieoverschrijdend zijn op het privépeeringpad, wordt voor alle uitgaande netwerkpakketten gebruikgemaakt van geforceerde tunnels, wanneer ze van de App Service-omgeving worden verzonden naar een on-premises netwerkinfrastructuur van de klant. Deze netwerkstroom wordt momenteel niet standaard ondersteund met App Service-omgevingen. Een mogelijke oplossing voor dit probleem is om advertentieoverschrijdende routes van het openbare-peeringpad naar het privépeeringpad te stoppen. Een andere oplossing is om ervoor te zorgen dat de App Service-omgeving werkt in een geforceerde-tunnelconfiguratie.

## <a name="change-the-egress-endpoint-for-your-app-service-environment"></a>Het uitgaande eindpunt voor de App Service-omgeving wijzigen ##

In deze sectie wordt beschreven hoe u ervoor zorgt dat een App Service-omgeving werkt in een geforceerde-tunnelconfiguratie door het uitgaande eindpunt te wijzigen dat wordt gebruikt in de App Service-omgeving. Als het uitgaande verkeer van de AS-omgeving via geforceerde tunnels wordt verzonden naar een on-premises netwerk, moet u toestaan dat dit verkeer afkomstig is van andere IP-adressen dat het VIP-adres van de AS-omgeving.

Een App Service-omgeving heeft niet alleen externe afhankelijkheden, maar moet ook luisteren naar binnenkomend verkeer en op dergelijk verkeer reageren. De antwoorden kunnen niet worden geretourneerd vanaf een ander adres, omdat dit het TCP schendt. Er zijn drie stappen nodig om het uitgaande eindpunt voor de App Service-omgeving te wijzigen:

1. Stel een routetabel in om ervoor te zorgen dat inkomend beheerverkeer kan worden geretourneerd vanaf hetzelfde IP-adres.

2. Voeg de IP-adressen toe die moeten worden gebruikt voor uitgaand verkeer naar de firewall van de App Service-omgeving.

3. Stel in dat uitgaand verkeer van de AS-omgeving via een tunnel wordt verzonden.

   ![Netwerkstroom over geforceerde tunnels][1]

U kunt de App Service-omgeving configureren met andere uitgaande adressen als de App Service-omgeving al operationeel is, of u kunt deze adressen instellen tijdens de implementatie van de App Service-omgeving.

### <a name="change-the-egress-address-after-the-app-service-environment-is-operational"></a>Het adres van uitgaand verkeer wijzigen als de App Service-omgeving al operationeel is ###
1. Haal de IP-adressen op die u wilt gebruiken als uitgaande IP-adressen voor de App Service-omgeving. Als u gebruikmaakt van geforceerde tunnels, zijn deze adressen afkomstig van de NAT’s of van de IP-adressen van de gateway. Als u het uitgaande verkeer van de App Service-omgeving wilt routeren via een NVA, is het uitgaande adres het openbare IP-adres van de NVA.

2. Stel de uitgaande adressen in bij de configuratiegegevens van de App Service-omgeving. Ga naar resource.azure.com en ga naar Subscription/<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name>. Vervolgens ziet u de JSON die de App Service-omgeving beschrijft. Controleer of bovenaan **lezen/schrijven** staat. Selecteer **Bewerken**. Schuif naar beneden en wijzig de waarde **userWhitelistedIpRanges** van **null** in een waarde die lijkt op het volgende. Gebruik de adressen die u wilt instellen als het bereik met uitgaande adressen. 

        "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

   Selecteer bovenaan **PUT**. Met deze optie wordt een schaalbewerking in de App Service-omgeving geactiveerd en de firewall aangepast.
 
3. Maak of bewerk een routetabel, en vul de regels in om toegang tot/van de beheeradressen toe te staan die zijn toegewezen aan de locatie van de App Service-omgeving. Zie [App Service Environment management addresses][management] (Beheeradressen van de App Service-omgeving) om de beheeradressen te zoeken.

4. Pas de routes die zijn toegepast op het subnet van de App Service-omgeving, aan met een routetabel of BGP-routes. 

Als de App Service-omgeving niet meer reageert vanuit de portal, is er een probleem opgetreden met de wijzigingen. Het probleem kan zijn dat uw lijst met uitgaande adressen onvolledig is, of dat het verkeer verloren is gegaan of is geblokkeerd. 

### <a name="create-a-new-app-service-environment-with-a-different-egress-address"></a>Een nieuwe App Service-omgeving maken met een ander uitgaand adres ###

Als uw virtuele netwerk al is geconfigureerd om geforceerde tunnels te gebruiken voor al het verkeer, moet u extra stappen nemen om de App Service-omgeving te maken, zodat alles naar behoren verloopt. U moet het gebruik van een ander uitgaand eindpunt inschakelen tijdens het maken van de App Service-omgeving. Hiervoor moet u de App Service-omgeving maken met een sjabloon die de toegestane uitgaande adressen specificeert.

1. Haal de IP-adressen op die moeten worden gebruikt als uitgaande adressen voor de App Service-omgeving.

2. Maak vooraf het subnet dat moet worden gebruikt voor de App Service-omgeving. U hebt dit subnet nodig om routes in te stellen, en ook omdat dit is vereist voor de sjabloon.

3. Maak een routetabel met de IP-beheeradressen die zijn toegewezen aan de locatie van de App Service-omgeving. Wijs deze toe aan de App Service-omgeving.

4. Volg de instructies in [Create an App Service Environment with a template][template] (Een App Service-omgeving maken met een sjabloon). Open de juiste sjabloon.

5. Bewerk de sectie Resources in het bestand azuredeploy.json. Neem een regel op voor **userWhitelistedIpRanges** met waarden zoals deze:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Als deze sectie juist is geconfigureerd, start de App Service-omgeving zonder problemen. 


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
