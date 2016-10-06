
<properties
   pageTitle="Peering op virtueel netwerk Azure | Microsoft Azure"
   description="Meer informatie over VNet-peering in Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/28/2016"
   ms.author="narayan" />


# VNet-peering

VNet-peering is een mechanisme dat twee virtuele netwerken in dezelfde regio via het backbone-netwerk van Azure met elkaar verbindt. Als de twee virtuele netwerken gekoppeld zijn, worden deze als één netwerk weergegeven voor alle verbindingsdoeleinden. Ze worden nog steeds beheerd als afzonderlijke resources, maar virtuele machines in deze virtuele netwerken kunnen met elkaar communiceren via privé-IP-adressen.

Het verkeer tussen virtuele machines in de gekoppelde virtuele netwerken wordt doorgestuurd via de Azure-infrastructuur zoals het verkeer dat wordt gerouteerd tussen virtuele machines in hetzelfde virtuele netwerk. De voordelen van VNet-peering zijn onder andere:

- Een verbinding met lage latentie en hoge bandbreedte tussen resources in verschillende virtuele netwerken.
- De mogelijkheid om resources zoals netwerkapparaten en VPN Gateways als doorvoerpunten te gebruiken in een gekoppeld VNet.
- De mogelijkheid om een virtueel netwerk dat gebruikmaakt van het Azure Resource Manager-model te verbinden met een virtueel netwerk dat gebruikmaakt van het klassieke implementatiemodel, en de mogelijkheid volledige verbindingen te hebben tussen resources in deze virtuele netwerken.

Vereisten en de belangrijkste aspecten van VNet-peering:

- De twee virtuele netwerken die worden gekoppeld, moeten zich in dezelfde Azure-regio bevinden.
- De virtuele netwerken die worden gekoppeld, moeten niet-overlappende IP-adresruimten hebben.
- VNet-peering vindt plaats tussen twee virtuele netwerken en er is geen afgeleide transitieve relatie. Als virtueel netwerk A bijvoorbeeld is gekoppeld aan virtueel netwerk B, en als virtueel netwerk B is gekoppeld aan virtueel netwerk C, betekent dit niet automatisch dat virtueel netwerk A is gekoppeld aan virtueel netwerk C.
- Peering kan tot stand worden gebracht tussen virtuele netwerken in twee verschillende abonnementen, mits een bevoegde gebruiker van beide abonnementen de peering toestaat en de abonnementen zijn gekoppeld aan dezelfde Active Directory-tenant. 
- Een virtueel netwerk dat gebruikmaakt van het Resource Manager-implementatiemodel kan worden gekoppeld aan een ander virtueel netwerk dat gebruikmaakt van dit model, of aan een virtueel netwerk dat gebruikmaakt van het klassieke implementatiemodel. Echter, virtuele netwerken die gebruikmaken van het klassieke implementatiemodel kunnen niet aan elkaar worden gekoppeld.
- Hoewel de communicatie tussen virtuele machines in gekoppelde virtuele netwerken geen extra bandbreedtebeperkingen heeft, is er nog steeds een bandbreedtelimiet op basis van VM-grootte van toepassing.


![Basic VNet-peering](./media/virtual-networks-peering-overview/figure01.png)

## Connectiviteit
Nadat twee virtuele netwerken aan elkaar zijn gekoppeld, kan een virtuele machine (web-/werkrol) in het virtuele netwerk rechtstreeks verbinding maken met andere virtuele machines in het gekoppelde virtuele netwerk. Deze twee netwerken hebben een volledige IP-verbinding.

De netwerklatentie voor een retour tussen twee virtuele machines in gelijkwaardige virtuele netwerken is dezelfde als die voor een retour binnen een lokaal virtueel netwerk. De netwerkdoorvoer is gebaseerd op de bandbreedte die is toegestaan voor de virtuele machine, evenredig aan de grootte. Er gelden verder geen extra beperkingen voor bandbreedte.

Het verkeer tussen de virtuele machines in gekoppelde virtuele netwerken wordt rechtstreeks doorgestuurd via de back-endinfrastructuur van Azure en niet via een gateway.

Virtuele machines in een virtueel netwerk hebben toegang tot de interne taakverdeling (ILB)-eindpunten in het gekoppelde virtuele netwerk. Netwerkbeveiligingsgroepen (NSG's) kunnen worden toegepast in elk virtueel netwerk om, indien gewenst, de toegang tot andere virtuele netwerken of subnetten te blokkeren.

Wanneer gebruikers peering configureren, kunnen ze de NSG-regels tussen de virtuele netwerken openzetten of afsluiten. Als de gebruiker ervoor kiest om de volledige connectiviteit tussen gelijkwaardige virtuele netwerken open te zetten (dit is de standaardoptie), kunnen zij vervolgens NSG's op specifieke subnetten of virtuele machines gebruiken voor het blokkeren of weigeren van specifieke toegang.

Door Azure verschafte interne DNS-naamomzetting voor virtuele machines werkt niet in gekoppelde virtuele netwerken. Virtuele machines hebben interne DNS-namen die alleen binnen het lokale virtuele netwerk oplosbaar zijn. Gebruikers kunnen echter virtuele machines in gekoppelde virtuele netwerken configureren als DNS-servers voor een virtueel netwerk.

## Servicechaining
Gebruikers kunnen door gebruikers gedefinieerde routetabellen configureren die naar virtuele machines in gekoppelde virtuele netwerken verwijzen als het 'volgende hop'-IP-adres, zoals verderop in dit artikel wordt weergegeven in het diagram. Hierdoor kunnen gebruikers servicechaining bereiken, waarmee ze verkeer van een virtueel netwerk naar een virtueel apparaat dat wordt uitgevoerd in een gekoppeld virtueel netwerk kunnen leiden, door middel van door gebruikers gedefinieerde routetabellen.

Gebruikers kunnen ook effectief hub en spoke-achtige omgevingen bouwen waarin de hub infrastructuuronderdelen kan hosten, zoals een virtueel netwerkapparaat. Alle virtuele spokenetwerken kunnen hier vervolgens aan worden gekoppeld, terwijl ook een subset verkeer wordt geleid naar apparaten die worden uitgevoerd in het virtuele hub-netwerk. Kort gezegd, met VNet-peering kan het 'volgende hop'-IP-adres op de door de gebruiker gedefinieerde routetabel het IP-adres zijn van een virtuele machine in het gekoppelde virtuele netwerk.

## Gateways en on-premises connectiviteit
Elk virtueel netwerk, ongeacht of het gekoppeld is aan een ander virtueel netwerk, kan nog steeds een eigen gateway hebben en gebruiken om te verbinden met on-premises. Gebruikers kunnen ook [VNet-naar-VNet-verbindingen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) configureren door gateways te gebruiken, ook al zijn de virtuele netwerken gekoppeld.

Wanneer beide opties voor interconnectiviteit tussen virtuele netwerken zijn geconfigureerd, loopt het verkeer tussen de virtuele netwerken via de peering-configuratie (dat wil zeggen, via de Azure-backbone).

Wanneer virtuele netwerken gelijkwaardig zijn gemaakt, kunnen gebruikers ook de gateway in het gelijkwaardige virtuele netwerk configureren als een doorvoerpunt naar on-premises. Het virtuele netwerk dat gebruikmaakt van een externe gateway kan in dit geval niet een eigen gateway hebben. Een virtueel netwerk kan slechts één gateway hebben. Het kan een lokale gateway of een externe gateway zijn (in het gekoppelde virtuele netwerk), zoals wordt weergegeven in de volgende afbeelding.

Gateway-overdracht wordt niet ondersteund in de peering-relatie tussen virtuele netwerken die gebruikmaken van het Resource Manager-model en netwerken die gebruikmaken van het klassieke implementatiemodel. Beide virtuele netwerken in de peering-relatie moeten het Resource Manager-implementatiemodel gebruiken voor een werkende gateway-overdracht.

Wanneer virtuele netwerken die één Azure ExpressRoute-verbinding delen, worden gekoppeld, gaat het verkeer tussen de twee netwerken via de peering-relatie (dat wil zeggen, via het backbone-netwerk van Azure). Gebruikers kunnen nog steeds in elk virtueel netwerk lokale gateways gebruiken om verbinding te maken met het on-premises circuit. Ze kunnen ook een gedeelde gateway gebruiken en de doorvoer voor on-premises connectiviteit configureren.

![Doorvoer VNet-peering](./media/virtual-networks-peering-overview/figure02.png)

## Inrichten
VNet-peering is een bevoegde bewerking. Het is een afzonderlijke functie onder de VirtualNetworks-naamruimte. Een gebruiker kan specifieke rechten krijgen voor het machtigen van peering. Een gebruiker met lees-/schrijftoegang tot het virtuele netwerk krijgt automatisch deze rechten.

Een gebruiker die ofwel een beheerder is of een bevoegde gebruiker van de peering-mogelijkheid kan een peering-bewerking op een ander VNet initiëren. Als er een overeenkomende aanvraag voor peering van de andere kant komt, en aan andere vereisten wordt voldaan, wordt de peering tot stand gebracht.

Raadpleeg de artikelen in de sectie 'Volgende stappen' voor meer informatie over het opzetten van VNet-peering tussen twee virtuele netwerken.

## Limieten
Er zijn limieten voor het aantal peerings dat is toegestaan voor één virtueel netwerk. Raadpleeg [Limieten voor Azure-netwerken](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.

## Prijzen
VNet-peering is tijdens de controleperiode gratis. Na de release wordt er een nominaal bedrag in rekening gebracht voor inkomend en uitgaand verkeer dat gebruikmaakt van de peering. Raadpleeg voor meer informatie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/virtual-network).


## Volgende stappen
- [Peering instellen tussen virtuele netwerken](virtual-networks-create-vnetpeering-arm-portal.md).
- Meer informatie over [NSG's](virtual-networks-nsg.md).
- Meer informatie over [door de gebruiker gedefinieerde routes en doorsturen via IP](virtual-networks-udr-overview.md).



<!--HONumber=Sep16_HO4-->


