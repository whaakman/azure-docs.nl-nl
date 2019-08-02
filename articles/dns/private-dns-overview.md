---
title: Wat is privé-DNS in Azure?
description: Een overzicht van de privé-DNS-hosting service op Microsoft Azure.
services: dns
author: vhorne
ms.service: dns
ms.topic: overview
ms.date: 6/12/2019
ms.author: victorh
ms.openlocfilehash: 35a4c7410b36e9716d2ee9625ed98fcc34043e58
ms.sourcegitcommit: d060947aae93728169b035fd54beef044dbe9480
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2019
ms.locfileid: "68742466"
---
# <a name="what-is-azure-private-dns"></a>Wat is privé-DNS in Azure?

> [!IMPORTANT]
> Azure Privé-DNS is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

De Domain Name System, of DNS, is verantwoordelijk voor het vertalen van een service naam naar het IP-adres.  Azure DNS is een hosting service voor DNS-domeinen die naam omzetting biedt met behulp van de Microsoft Azure-infra structuur. Naast de ondersteuning van Internet gerichte DNS-domeinen, ondersteunt Azure DNS ook persoonlijke DNS-zones.

Azure Privé-DNS biedt een betrouw bare, veilige DNS-service voor het beheren en omzetten van domein namen in een virtueel netwerk zonder dat u een aangepaste DNS-oplossing hoeft toe te voegen. Met behulp van privé-DNS-zones kunt u uw eigen aangepaste domein namen gebruiken in plaats van de door Azure meegeleverde namen die vandaag beschikbaar zijn. Met aangepaste domein namen kunt u uw virtuele netwerk architectuur aanpassen aan de behoeften van uw organisatie. Het biedt naam omzetting voor virtuele machines (Vm's) in een virtueel netwerk en tussen virtuele netwerken. Daarnaast kunt u zone namen configureren met een split-horizon-weer gave, waardoor een persoonlijke en een open bare DNS-zone de naam kunnen delen.

Als u de records van een privé-DNS-zone wilt omzetten vanuit uw virtuele netwerk, moet u het virtuele netwerk koppelen aan de zone. Gekoppelde virtuele netwerken hebben volledige toegang en kunnen alle DNS-records die in de privé zone worden gepubliceerd, omzetten. Daarnaast kunt u ook de optie voor het inschakelen van de registratie op een virtueel netwerk. Als u registratie op een virtueel netwerk inschakelt, worden de DNS-records voor de virtuele machines in het virtuele netwerk geregistreerd in de privé zone. Als automatische registratie is ingeschakeld, werkt Azure DNS ook de zone records bij wanneer een virtuele machine wordt gemaakt, wordt het IP-adres gewijzigd of wordt dit verwijderd.

![Overzicht van DNS](./media/private-dns-overview/scenario.png)

> [!NOTE]
> Gebruik als best practice geen *. lokaal* domein voor uw privé-DNS-zone. Niet alle besturings systemen ondersteunen dit.

## <a name="benefits"></a>Voordelen

Azure Privé-DNS biedt de volgende voor delen:

* **Hiermee verwijdert u de nood zaak van aangepaste DNS-oplossingen**. Voorheen hebben veel klanten aangepaste DNS-oplossingen gemaakt voor het beheren van DNS-zones in hun virtuele netwerk. U kunt nu DNS-zones beheren met behulp van de systeem eigen Azure-infra structuur, waardoor de belasting van het maken en beheren van aangepaste DNS-oplossingen wordt verwijderd.

* **Alle algemene DNS-record typen gebruiken**. Azure DNS ondersteunt A-, AAAA-, CNAME-en MX-, PTR-, SOA-, SRV-en TXT-records.

* **Automatische hostname-record beheer**. Naast het hosten van uw aangepaste DNS-records, onderhoudt Azure automatisch hostname-records voor de virtuele machines in de opgegeven virtuele netwerken. In dit scenario kunt u de domein namen die u gebruikt, optimaliseren zonder dat u aangepaste DNS-oplossingen hoeft te maken of toepassingen hoeft te wijzigen.

* **Omzetting van hostname tussen virtuele netwerken**. In tegens telling tot door Azure verschafte hostnamen kunnen persoonlijke DNS-zones worden gedeeld tussen virtuele netwerken. Deze mogelijkheid vereenvoudigt scenario's voor meerdere netwerken en service detectie, zoals peering op het virtuele netwerk.

* **Vertrouwde hulpprogram ma's en gebruikers ervaring**. Voor een kortere leer curve gebruikt deze service goed gedefinieerde Azure DNS-hulpprogram ma's (Azure Portal, Azure PowerShell, Azure CLI, Azure Resource Manager sjablonen en de REST API).

* **Split-horizon-ondersteuning voor DNS**. Met Azure DNS kunt u zones maken met dezelfde naam die worden omgezet in verschillende antwoorden vanuit een virtueel netwerk en via het open bare Internet. Een typisch scenario voor Split-horizon DNS is het leveren van een speciale versie van een service voor gebruik in uw virtuele netwerk.

* **Beschikbaar in alle Azure-regio's**. De functie Azure DNS persoonlijke zones is beschikbaar in alle Azure-regio's in de open bare Azure-Cloud.

## <a name="capabilities"></a>Mogelijkheden

Azure DNS biedt de volgende mogelijkheden:

* **Automatische registratie van virtuele machines van een virtueel netwerk dat is gekoppeld aan een privé zone waarvoor automatische registratie is ingeschakeld**. De virtuele machines worden geregistreerd (toegevoegd) aan de privé zone als een record die verwijst naar hun privé-IP-adressen. Wanneer een virtuele machine in een virtuele netwerk koppeling waarvoor automatische registratie is ingeschakeld, wordt verwijderd, verwijdert Azure DNS ook automatisch de bijbehorende DNS-record uit de gekoppelde persoonlijke zone.

* **Forward DNS-omzetting wordt ondersteund voor virtuele netwerken die zijn gekoppeld aan de privé zone**. Voor de DNS-omzetting tussen virtuele netwerken is er geen expliciete afhankelijkheid, zodat de virtuele netwerken met elkaar worden gekoppeld. Het is echter mogelijk dat u virtuele netwerken wilt peeren voor andere scenario's (bijvoorbeeld HTTP-verkeer).

* **Achterwaartse DNS-Zoek opdrachten worden ondersteund binnen het bereik van het virtuele netwerk**. Omgekeerde DNS-zoek opdracht voor een persoonlijk IP-adres in het virtuele netwerk dat is toegewezen aan een privé zone, wordt de FQDN-naam van de host/recordnaam en de zone naam als achtervoegsel geretourneerd.

## <a name="known-issues"></a>Bekende problemen
De volgende items zijn bekende bugs en problemen in de preview-versie:
* Als u een virtueel netwerk verwijdert dat is gekoppeld aan een privé-DNS-zone, worden de koppelingen naar de privé-DNS-zone niet verwijderd. De koppeling mislukt als u het virtuele netwerk met dezelfde naam en resource groep opnieuw maakt en het opnieuw probeert te koppelen aan een privé-DNS-zone. U kunt dit probleem omzeilen door het virtuele netwerk te maken in een andere resource groep of met een andere naam in dezelfde resource groep.
* Als u een virtueel netwerk naar een andere resource groep of een ander abonnement verplaatst, worden de koppelingen naar de privé-DNS-zone niet bijgewerkt. De naam omzetting voor het verplaatste virtuele netwerk blijft werken. er worden echter oude ARM-Id's van het virtuele netwerk weer gegeven wanneer u de koppelingen van het virtuele netwerk van de privé-DNS-zone bekijkt.
* Momenteel kunnen gekoppelde virtuele netwerken die worden gehost in UAE-noord, UAE-centraal, Zuid-Afrika-west, Zuid-Afrika-noord, Canada-oost, Frankrijk-zuid mislukken en kunnen er onregelmatige problemen met de DNS-omzetting optreden. 


## <a name="other-considerations"></a>Andere overwegingen

Azure DNS heeft de volgende beperkingen:

* Een specifiek virtueel netwerk kan aan slechts één privé zone worden gekoppeld alsof automatische registratie van VM-DNS-records is ingeschakeld. U kunt echter meerdere virtuele netwerken koppelen aan één DNS-zone.
* Omgekeerde DNS werkt alleen voor particuliere IP-ruimte in het gekoppelde virtuele netwerk
* Omgekeerde DNS voor een persoonlijk IP-adres voor een gekoppeld virtueel netwerk retourneert "internal.cloudapp.net" als het standaard achtervoegsel voor de virtuele machine. Voor virtuele netwerken die zijn gekoppeld aan een privé zone waarvoor registratie is ingeschakeld, retourneert reverse DNS voor een privé-IP 2 FQDN-namen, een met het standaard achtervoegsel *Internal.cloudapp.net* en een andere met het achtervoegsel van de privé zone.
* Voorwaardelijk door sturen wordt op het moment niet standaard ondersteund. Om omzetting tussen Azure-en on-premises netwerken mogelijk te maken. Zie [naam omzetting voor vm's en rolinstanties](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)
 
## <a name="pricing"></a>Prijzen

Zie [Azure DNS prijzen](https://azure.microsoft.com/pricing/details/dns/)voor prijs informatie.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het maken van een privé zone in Azure DNS met behulp van [Azure PowerShell](./private-dns-getstarted-powershell.md) of [Azure cli](./private-dns-getstarted-cli.md).

* Meer informatie over enkele veelvoorkomende [scenario's voor particuliere zones](./private-dns-scenarios.md) die kunnen worden gerealiseerd met persoonlijke zones in azure DNS.

* Voor veelgestelde vragen en antwoorden over persoonlijke zones in Azure DNS, met inbegrip van specifieke gedrag dat u kunt verwachten voor bepaalde soorten bewerkingen, raadpleegt u [privé-DNS Veelgestelde vragen](./dns-faq-private.md).

* Ga voor meer informatie over DNS-zones en-records naar [overzicht van DNS-zones en-records](dns-zones-records.md).

* Informatie over enkele van de andere belangrijke [netwerkmogelijkheden](../networking/networking-overview.md) van Azure.
