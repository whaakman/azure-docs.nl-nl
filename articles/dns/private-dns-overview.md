---
title: Wat is Azure privé-DNS?
description: Een overzicht van de persoonlijke DNS-server die als host fungeert de service op Microsoft Azure.
services: dns
author: vhorne
ms.service: dns
ms.topic: overview
ms.date: 6/12/2019
ms.author: victorh
ms.openlocfilehash: a8548b4972d5853f09630ae3e9ded05ed6fee32b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076394"
---
# <a name="what-is-azure-private-dns"></a>Wat is Azure privé-DNS?

> [!IMPORTANT]
> Azure privé-DNS is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

De Domain Name System of DNS is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een service aan het bijbehorende IP-adres.  Azure DNS is een hostingservice voor DNS-domeinen die naamomzetting met behulp van de Microsoft Azure-infrastructuur biedt. Azure DNS ondersteunt naast ondersteuning voor internet gerichte DNS-domeinen, ook een persoonlijke DNS-zones.

Azure privé-DNS biedt een betrouwbare en veilige DNS-service voor het beheren en omzetten van domeinnamen in een virtueel netwerk zonder de noodzaak om toe te voegen een aangepaste oplossing voor de DNS. Met behulp van privé-DNS-zones, kunt u uw eigen aangepaste domeinnamen in plaats van de Azure geleverde namen die momenteel beschikbaar. Met behulp van aangepaste domeinnamen, helpt u bij het aanpassen van de architectuur van uw virtuele netwerk zelf behoeften van uw organisatie. Het biedt naamomzetting voor virtuele machines (VM's) binnen een virtueel netwerk en tussen virtuele netwerken. Bovendien kunt u zonenamen configureren met een split-horizon-weergave, waarmee een persoonlijke en openbare DNS-zone voor het delen van de naam.

U kunt oplossen door de records van een persoonlijke DNS-zone van uw virtuele netwerk, koppelt u het virtuele netwerk met de zone. Gekoppelde virtuele netwerken hebben volledige toegang en alle DNS-records die zijn gepubliceerd in de privézone kunnen oplossen. U kunt daarnaast ook automatische registratie op de koppeling van een virtueel netwerk inschakelen. Als u automatische registratie op de koppeling van een virtueel netwerk inschakelt, worden de DNS-records voor de virtuele machines op dit virtuele netwerk zijn geregistreerd in de privézone. Als automatische registratie is ingeschakeld, Azure DNS zonerecords ook bijgewerkt wanneer een virtuele machine wordt gemaakt, worden wijzigingen zijn ' IP-adres, of is verwijderd.

![DNS-overzicht](./media/private-dns-overview/scenario.png)

> [!NOTE]
> Als een best practice, gebruik niet een *.local* domein voor uw privé-DNS-zone. Niet alle besturingssystemen ondersteunen dit.

## <a name="benefits"></a>Voordelen

Azure privé-DNS bevat de volgende voordelen:

* **Hiermee verwijdert u de noodzaak van aangepaste DNS-oplossingen**. Veel klanten gemaakt eerder, aangepaste DNS-oplossingen voor het beheren van DNS-zones in het virtuele netwerk. U kunt nu de DNS-zones met behulp van de systeemeigen Azure-infrastructuur, waarmee wordt verwijderd van de belasting van het maken en beheren van aangepaste DNS-oplossingen beheren.

* **Gebruik van alle algemene DNS-records typen**. Azure DNS ondersteunt A, AAAA, CNAME, MX, PTR, SOA, SRV en TXT-records.

* **Automatische hostnaam recordbeheer**. Samen met uw aangepaste DNS-records hosten, bewaart Azure automatisch hostnaamrecords voor de virtuele machines in de opgegeven virtuele netwerken. In dit scenario kunt u de domeinnamen die u gebruikt zonder dat u hoeft te maken van aangepaste DNS-oplossingen of wijzig de toepassingen optimaliseren.

* **Hostnaamomzetting tussen virtuele netwerken**. In tegenstelling tot Azure verschafte hostnamen kunnen persoonlijke DNS-zones worden gedeeld tussen virtuele netwerken. Deze mogelijkheid vereenvoudigt cross-netwerk- en servicedetectie scenario's, zoals peering op virtueel netwerk.

* **Vertrouwde hulpprogramma's en gebruikerservaring**. Deze service gebruikt om te beperken het leerproces, gerenommeerd Azure DNS-hulpprogramma's (Azure portal, Azure PowerShell, Azure CLI, Azure Resource Manager-sjablonen en de REST-API).

* **Ondersteuning van DNS split-horizon**. U kunt zones met dezelfde naam die worden omgezet naar verschillende antwoorden uit vanuit een virtueel netwerk en het openbare internet kunt maken met Azure DNS. Een typisch scenario voor DNS-split-horizon is voor een specifieke versie van een service voor gebruik in uw virtuele netwerk.

* **Beschikbaar in alle Azure-regio's**. De functie voor Azure DNS private zones is beschikbaar in alle Azure-regio's in de openbare cloud van Azure.

## <a name="capabilities"></a>Functionaliteit

Azure DNS biedt de volgende mogelijkheden:

* **Automatische registratie van virtuele machines van een virtueel netwerk dat gekoppeld aan een privézone met automatische registratie ingeschakeld**. De virtuele machines zijn ingeschreven (toegevoegd) aan de persoonlijke zone als A-records die verwijst naar hun privé IP-adressen. Wanneer een virtuele machine in een virtueel netwerk koppelen met automatische registratie ingeschakeld wordt verwijderd, worden de bijbehorende DNS-record ook automatisch verwijderd met Azure DNS van de gekoppelde privézone.

* **Forward DNS-omzetting wordt ondersteund tussen virtuele netwerken die zijn gekoppeld aan de privézone**. Voor cross-virtueel netwerk DNS-omzetting is het geen expliciete afhankelijkheid zodanig dat de virtuele netwerken met elkaar zijn gekoppeld. Echter, als u wilt virtuele netwerken voor andere scenario's (bijvoorbeeld HTTP-verkeer).

* **Omgekeerde DNS-zoekopdracht wordt ondersteund binnen het bereik van het virtuele netwerk**. Omgekeerde DNS-zoekactie voor een privé IP-adres binnen het virtuele netwerk dat is toegewezen aan een privézone retourneert de FQDN-naam met de naam van de host/record en de naam van de zone als het achtervoegsel.

## <a name="other-considerations"></a>Andere overwegingen

Azure DNS heeft de volgende beperkingen:

* Registratie van slechts één virtueel netwerk is toegestaan per privézone.
* Virtuele netwerken mogen maximaal 10 oplossingen per privézone. Deze limiet wordt verwijderd wanneer deze functie algemeen beschikbaar is.
* Een specifiek virtueel netwerk kan worden gekoppeld aan de zone met slechts één persoonlijke als een registration virtual network.
* Een specifiek virtueel netwerk kan maximaal 10 private zones als een virtueel netwerk van het probleem zou moeten worden gekoppeld. Deze limiet wordt verwijderd wanneer deze functie algemeen beschikbaar is.
* Als u een virtueel netwerk registratie opgeeft, zijn de DNS-records voor de virtuele machines van dit virtuele netwerk die zijn geregistreerd bij de privézone niet kan worden weergegeven of worden opgehaald uit de Azure Powershell en Azure CLI-API's. De VM-records inderdaad worden geregistreerd en is de oplossing.
* Omgekeerde DNS-geldt alleen voor privé-IP-adresruimte in het registration virtual network.
* Reverse-DNS voor een privé IP-adres dat niet is geregistreerd bij de privézone (bijvoorbeeld een privé IP-adres voor een virtuele machine in een virtueel netwerk dat is gekoppeld als een resolutie van virtueel netwerk met een particulier zone) retourneert *internal.cloudapp.net* Als de DNS-achtervoegsel. Dit achtervoegsel wordt echter niet omgezet.
* Het virtuele netwerk moet volledig leeg zijn de eerste keer dat u deze aan een privézone als een registratie- of resolutienetwerken virtueel netwerk koppelen. Het virtuele netwerk kan echter vervolgens worden niet-lege voor toekomstige koppelen als een virtueel netwerk van registratie- of resolutienetwerken naar andere private zones.
* Voorwaardelijk doorsturen wordt op dit moment niet ondersteund (bijvoorbeeld voor het inschakelen van het probleem zou moeten tussen Azure en on-premises netwerken). Zie voor meer informatie over hoe klanten in dit scenario via andere methoden kunnen realiseren [naamomzetting voor VM's en rolexemplaren](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
* Omgekeerde DNS werkt alleen voor privé-IP-adresruimte in het gekoppelde virtuele netwerk
* Reverse-DNS voor een privé IP-adres voor een gekoppelde virtuele netwerk retourneert 'internal.cloudapp.net' als het standaard-achtervoegsel voor de virtuele machine. Voor virtuele netwerken die zijn gekoppeld aan een privézone met automatische registratie ingeschakeld, reverse-DNS voor een privé IP-adres 2 FQDN's, één met standaard het achtervoegsel retourneert *internal.cloudapp.net* en een andere met het achtervoegsel privézone.
* Voorwaardelijk doorsturen wordt niet ondersteund. Als u bijvoorbeeld om in te schakelen resolutie tussen Azure en on-premises netwerken. Meer informatie over hoe u dit scenario met andere methoden kan inschakelen. Zie [naamomzetting voor virtuele machines en rollen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)
 


## <a name="pricing"></a>Prijzen

Zie voor informatie over de prijzen [prijzen voor Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

## <a name="next-steps"></a>Volgende stappen

* Informatie over het maken van een privé-zone in Azure DNS met behulp van [Azure PowerShell](./private-dns-getstarted-powershell.md) of [Azure CLI](./private-dns-getstarted-cli.md).

* Meer informatie over sommige algemene [privézone-scenario's](./private-dns-scenarios.md) die u kunt realiseren met privézones in Azure DNS.

* Voor algemene vragen en antwoorden over privézones in Azure DNS, met inbegrip van specifiek gedrag kunt u verwachten voor bepaalde soorten bewerkingen, Zie [Veelgestelde vragen over privé-DNS](./dns-faq-private.md).

* Meer informatie over DNS-zones en records, recentst [DNS-zones en records overzicht](dns-zones-records.md).

* Informatie over enkele van de andere belangrijke [netwerkmogelijkheden](../networking/networking-overview.md) van Azure.
