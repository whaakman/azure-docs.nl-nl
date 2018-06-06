---
title: Azure DNS gebruiken voor persoonlijke domeinen | Microsoft Docs
description: Een overzicht van de persoonlijke DNS-server die als host service Microsoft Azure.
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: kumud
ms.openlocfilehash: 0ee3b18b7f874c4f6b7b2c9c559aa7e393ad7d8d
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34700601"
---
# <a name="use-azure-dns-for-private-domains"></a>Azure DNS gebruiken voor persoonlijke domeinen
Het Domain Name System en DNS, is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een service het IP-adres. Een hostingservice voor DNS-domeinen, biedt Azure DNS naamomzetting met de Microsoft Azure-infrastructuur. Naast de ondersteuning van internetgerichte DNS-domeinen, ondersteunt Azure DNS ook nu persoonlijke DNS-domeinen als preview-functie. 
 
Azure DNS biedt een betrouwbare, veilige DNS-service wilt beheren en oplossen van domeinnamen in een virtueel netwerk zonder uw toevoegen van een aangepaste DNS-oplossing. Met behulp van persoonlijke DNS-zones, kunt u uw eigen aangepaste domeinnamen in plaats van de Azure verschafte namen die beschikbaar zijn. Aangepaste domeinnamen helpt u bij het aanpassen van de virtuele netwerkarchitectuur zelf behoeften van uw organisatie. Het biedt naamomzetting voor virtuele machines (VM's) binnen een virtueel netwerk en tussen virtuele netwerken. U kunt bovendien de namen van de zones configureren met een weergave split-horizon, waarmee een persoonlijke en een openbare DNS-zone voor het delen van dezelfde naam.

![DNS-overzicht](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="benefits"></a>Voordelen

Azure DNS biedt de volgende voordelen:

* **Verwijdert de behoefte voor aangepaste DNS-oplossingen**. Eerder gemaakt veel klanten aangepaste DNS-oplossingen voor het beheren van DNS-zones in het virtuele netwerk. Nu kunt u DNS-zone-beheer uitvoeren met behulp van de systeemeigen Azure-infrastructuur, waardoor de last van maken en beheren van aangepaste DNS-oplossingen.

* **Gebruik van alle algemene DNS-records typen**. Azure DNS ondersteunt A, AAAA, CNAME, MX, NS, PTR, SOA, SRV en TXT-records.

* **Automatische hostnaam records beheren**. Samen met de aangepaste DNS-records hosten, onderhoudt Azure automatisch hostnaam records voor de virtuele machines in de opgegeven virtuele netwerken. In dit scenario kunt u de domeinnamen die te gebruiken zonder te maken van aangepaste DNS-oplossingen of wijzig de toepassingen te optimaliseren.

* **Hostnaamomzetting tussen virtuele netwerken**. In tegenstelling tot Azure verschafte hostnamen kunnen persoonlijke DNS-zones worden gedeeld tussen virtuele netwerken. Deze mogelijkheid vereenvoudigt cross-netwerk- en servicedetectie scenario's, zoals het virtuele netwerk peering.

* **Vertrouwde hulpprogramma's en gebruikerservaring**. Deze nieuwe aanbieding gebruikt om te beperken het leerproces, goed tot stand gebrachte Azure DNS-hulpprogramma's (PowerShell, Azure resourcemanager-sjablonen en de REST-API).

* **Ondersteuning van split-horizon DNS**. U kunt zones met dezelfde naam die worden omgezet in verschillende antwoorden van vanuit een virtueel netwerk en het openbare internet kunt maken met Azure DNS. Er is een typisch scenario voor split-horizon DNS voor een specifieke versie van een service voor gebruik in uw virtuele netwerk.

* **Beschikbaar in alle Azure-regio's**. De Azure DNS-zones voor persoonlijke-functie is beschikbaar in alle Azure-regio's in de Azure openbare cloud. 


## <a name="capabilities"></a>Functionaliteit

Azure DNS biedt de volgende mogelijkheden:
 
* **Automatische registratie van virtuele machines van één virtueel netwerk dat gekoppeld aan een persoonlijke zone als een virtueel netwerk registratie**. De virtuele machines zijn geregistreerd (toegevoegd) aan de zone met persoonlijke A-records die verwijst naar hun persoonlijke IP-adressen. Als een virtuele machine in de registratie van een virtueel netwerk wordt verwijderd, wordt Azure de bijbehorende DNS-server ook automatisch verwijderd van de gekoppelde persoonlijke zone. 

  > [!NOTE]
  > Standaard fungeren registratie virtuele netwerken ook als omzetting van virtuele netwerken in die zin dat DNS-omzetting op basis van de zone uit een van de virtuele machines binnen het virtuele netwerk registratie werkt. 

* **Forward DNS-omzetting is ondersteund tussen virtuele netwerken die zijn gekoppeld aan de zone met persoonlijke als virtuele netwerken resolutie**. Voor meerdere virtuele netwerk DNS-omzetting is het geen expliciete afhankelijkheid zodanig dat de virtuele netwerken met elkaar brengen. Klanten wilt echter peer virtuele netwerken voor andere scenario's (bijvoorbeeld HTTP-verkeer).

* **Omgekeerde DNS-zoekopdracht wordt ondersteund binnen het bereik van het virtuele netwerk**. Omgekeerde DNS-zoekopdracht voor een particuliere IP-adres in het virtuele netwerk toegewezen aan een persoonlijke zone wordt de FQDN-naam met de naam van de host/record, evenals de naam van de zone als het achtervoegsel geretourneerd. 


## <a name="limitations"></a>Beperkingen

Azure DNS is onderworpen aan de volgende beperkingen:

* Registratie van slechts één virtueel netwerk is toegestaan per persoonlijke zone.
* Virtuele netwerken mogen maximaal 10 oplossingen per persoonlijke zone.
* Een specifiek virtueel netwerk kan worden gekoppeld aan slechts één persoonlijke zone als een virtueel netwerk voor registratie.
* Een specifiek virtueel netwerk kan worden gekoppeld aan maximaal 10 persoonlijke zones als een virtueel netwerk resolutie.
* Als een virtueel netwerk van de registratie is opgegeven, de DNS-records voor de virtuele machines van dit virtuele netwerk die zijn geregistreerd op de persoonlijke zone zijn niet zichtbaar of worden opgehaald van de Azure Powershell en Azure CLI-API's, maar de VM-records inderdaad worden geregistreerd en wordt is opgelost.
* Omgekeerde DNS-werkt alleen voor persoonlijke IP-adresruimte in het virtuele netwerk van de registratie.
* Omgekeerde DNS voor een particuliere IP-adres dat niet is geregistreerd in de persoonlijke zone (bijvoorbeeld een particuliere IP-adres voor een virtuele machine in een virtueel netwerk dat is gekoppeld als een virtueel netwerk voor omzetting naar een privé zone) retourneert *internal.cloudapp.net* Als de DNS-achtervoegsel. Dit achtervoegsel kan echter niet worden omgezet. 
* Het virtuele netwerk moet leeg zijn (dat wil zeggen, er zijn geen records VM bestaat) wanneer deze in eerste instantie (dat wil zeggen, voor het eerst) koppelingen naar een persoonlijke zone als een registratie of omzetting van virtueel netwerk. Echter, het virtuele netwerk niet-lege vervolgens kan worden voor toekomstige koppelen als een virtueel netwerk van registratie of omzetting naar andere persoonlijke zones. 
* Voorwaardelijk doorsturen wordt op dit moment niet ondersteund (bijvoorbeeld voor het inschakelen van de omzetting tussen Azure en OnPrem netwerken). Zie voor meer informatie over hoe klanten in dit scenario via andere methoden kunnen realiseren [naamomzetting voor VM's en rolexemplaren](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Voor veelgestelde vragen en antwoorden over persoonlijke zones in Azure DNS, met inbegrip van specifieke DNS-registratie en omzetting gedrag kunt u verwachten voor bepaalde soorten bewerkingen, Zie [Veelgestelde vragen over](./dns-faq.md#private-dns).  


## <a name="pricing"></a>Prijzen

De functie Privé DNS-zones is gratis tijdens de openbare preview. Tijdens algemeen beschikbaar is, wordt de functie aangeboden door een op gebruik gebaseerde prijsmodel vergelijkbaar met die van de bestaande Azure DNS aanbieden. 


## <a name="next-steps"></a>Volgende stappen

Informatie over het maken van een persoonlijke zone in Azure DNS met behulp van [Azure PowerShell](./private-dns-getstarted-powershell.md) of [Azure CLI](./private-dns-getstarted-cli.md).

Meer informatie over een aantal gemeenschappelijke [persoonlijke zone scenario's](./private-dns-scenarios.md) die kunnen worden gerealiseerd met persoonlijke zones in Azure DNS.

Voor algemene vragen en antwoorden over persoonlijke in Azure DNS-zones, inclusief specifiek gedrag kunt u verwachten voor bepaalde soorten bewerkingen, Zie [Veelgestelde vragen over](./dns-faq.md#private-dns). 

Meer informatie over DNS-zones en records in via [DNS-zones en registreert overzicht](dns-zones-records.md).

Informatie over enkele van de andere belangrijke [netwerkmogelijkheden](../networking/networking-overview.md) van Azure. 

