---
title: Azure DNS gebruiken voor persoonlijke domeinen | Microsoft Docs
description: Overzicht van persoonlijke DNS hostingservice op Microsoft Azure.
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
ms.openlocfilehash: 677fc66b66d6c17806a313f2fac3a15e8e1775ba
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="using-azure-dns-for-private-domains"></a>Azure DNS gebruiken voor persoonlijke domeinen
Het Domain Name System en DNS, is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een service het IP-adres. Azure DNS is een hosting service voor DNS-domeinen omzetten van namen met behulp van de Microsoft Azure-infrastructuur.  Naast internet gerichte DNS-domeinen ondersteunt Azure DNS ook nu persoonlijke DNS-domeinen als preview-functie.  
 
Azure DNS biedt een betrouwbare, veilige DNS-service wilt beheren en oplossen van domeinnamen in een virtueel netwerk zonder te hoeven toevoegen van een aangepaste DNS-oplossing. Persoonlijke DNS-zones kunnen u uw eigen aangepaste domeinnamen in plaats van de beschikbare vandaag nog Azure verschafte namen gebruiken.  Aangepaste domeinnamen helpt u bij het aanpassen van de virtuele netwerkarchitectuur zelf behoeften van uw organisatie. Het biedt naamomzetting voor virtuele machines binnen een virtueel netwerk en tussen virtuele netwerken. U kunt bovendien de namen van de zones configureren met een weergave split-horizon - zodat een persoonlijke en een openbare DNS-zone voor het delen van dezelfde naam.

![DNS-overzicht](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="benefits"></a>Voordelen

* **Verwijdert de behoefte voor aangepaste DNS-oplossingen.** Eerder gemaakt veel klanten aangepaste DNS-oplossingen voor het beheren van DNS-zones in het virtuele netwerk.  DNS-zone management kan nu worden gedaan met behulp van de systeemeigen infrastructuur van Azure, waardoor de last van maken en beheren van aangepaste DNS-oplossingen.

* **Alle algemene DNS-records typen gebruiken.**  Azure DNS ondersteunt A, AAAA, CNAME, MX, NS, PTR, SOA, SRV en TXT-records.

* **Beheer van automatische hostname-record.** Samen met de aangepaste DNS-records hosten, onderhoudt Azure automatisch hostnaam records voor de virtuele machines in de opgegeven virtuele netwerken.  Hiermee kunt u voor het optimaliseren van de domeinnamen gebruiken zonder te maken van aangepaste DNS-oplossingen of toepassing wijzigen.

* **Hostnaamomzetting tussen virtuele netwerken.** In tegenstelling tot Azure verschafte hostnamen kunnen persoonlijke DNS-zones worden gedeeld tussen virtuele netwerken.  Deze mogelijkheid vereenvoudigt de detectie van cross-netwerk en service-scenario's zoals het virtuele netwerk peering.

* **Vertrouwde hulpprogramma's en gebruikerservaring.** Deze nieuwe aanbieding gebruikt om het leerproces verminderen, de al goed tot stand gebrachte Azure DNS-hulpprogramma's (PowerShell Resource Manager-sjablonen, REST-API).

* **Ondersteuning van split-horizon DNS.** Azure DNS kunt u zones maken met dezelfde naam omzetten in verschillende antwoorden van vanuit een virtueel netwerk en het openbare Internet.  Er is een typisch scenario voor split-horizon DNS voor een specifieke versie van een service voor gebruik in uw virtuele netwerk.

* **Beschikbaar in alle Azure-regio's.** Azure persoonlijke DNS-Zones is beschikbaar in alle Azure-regio's in de Azure openbare cloud. 


## <a name="capabilities"></a>Functionaliteit 
* Automatische registratie van virtuele machines van één virtueel netwerk is gekoppeld aan een zone voor persoonlijke als een virtueel netwerk voor registratie. De virtuele machines worden geregistreerd (toegevoegd) aan de zone met persoonlijke als A-records die verwijst naar hun privé-IP-adressen. Bovendien wanneer een virtuele machine in de registratie van een virtueel netwerk wordt verwijderd, wordt Azure ook automatisch verwijderd de bijbehorende DNS-record van de gekoppelde persoonlijke zone. Houd er rekening mee dat virtuele netwerken van de registratie ook standaard als omzetting van virtuele netwerken fungeren in dat DNS-omzetting op basis van de zone vanuit een van de virtuele machines binnen het virtuele netwerk van de registratie werken. 
* Doorsturen van DNS-omzetting ondersteund tussen virtuele netwerken die zijn gekoppeld aan de zone met persoonlijke als omzetting van virtuele netwerken. Voor meerdere virtuele netwerk DNS-omzetting is geen expliciete afhankelijkheid die de virtuele netwerken worden ingesteld als peer met elkaar. Klanten kunt echter als peer virtuele netwerken voor andere scenario's (bijvoorbeeld: HTTP-verkeer).
* Omgekeerde DNS-zoekopdracht binnen het bereik van de VNET wordt ondersteund. Omgekeerde DNS-zoekopdracht voor een privé-IP binnen het virtuele netwerk toegewezen aan een Zone persoonlijke wordt de FQDN-naam met de naam van de host/record, evenals de naam van de zone als het achtervoegsel geretourneerd. 


## <a name="limitations"></a>Beperkingen
* 1 virtueel netwerk van de registratie per persoonlijke Zone
* Maximaal 10 virtuele netwerken van de oplossing per persoonlijke Zone
* Een bepaald virtueel netwerk kan slechts worden gekoppeld aan een Zone voor persoonlijke als een virtueel netwerk voor registratie
* Een bepaald virtueel netwerk kan worden gekoppeld aan maximaal 10 persoonlijke Zones als een virtueel netwerk resolutie
* Als een virtueel netwerk van de registratie is opgegeven, de DNS-records voor de virtuele machines van dit virtuele netwerk die zijn geregistreerd op de Zone persoonlijke niet worden bekeken of worden opgehaald van de Powershell/CLI/API's, maar de VM-records inderdaad zijn geregistreerd en wordt opgelost is
* Omgekeerde DNS werkt alleen voor privé-IP-adresruimte in het virtuele netwerk voor registratie
* Omgekeerde DNS voor een particuliere IP die niet is geregistreerd in de privé-Zone (bijvoorbeeld: privé-IP voor een virtuele machine in een virtueel netwerk dat is gekoppeld als een virtueel netwerk omzetten naar een persoonlijke zone) retourneert 'internal.cloudapp.net' als de DNS-achtervoegsel, maar dit achtervoegsel kan niet worden omgezet.   
* Virtueel netwerk moet leeg zijn (eenledige Er zijn geen VM-records) wanneer in eerste instantie (eenledige voor de eerste keer) koppelen aan een Zone persoonlijke als registratie of omzetting van virtueel netwerk. Echter, het virtuele netwerk niet-lege vervolgens kan worden voor toekomstige koppelen als een virtueel netwerk van registratie of omzetting naar andere persoonlijke zones. 
* Op dit moment voorwaardelijk doorsturen niet wordt ondersteund, bijvoorbeeld voor het inschakelen van de omzetting tussen Azure en OnPrem netwerken. Zie voor documentatie op hoe klanten in dit scenario via andere methoden kunnen realiseren [naamomzetting voor VM's en Rolinstanties](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)

We raden ook je ook Lees informatie over de [Veelgestelde vragen over](./dns-faq.md#private-dns) voor enkele veelgestelde vragen en antwoorden op persoonlijke Zones in Azure DNS, met inbegrip van specifieke DNS-registratie en omzetting gedrag kunt u verwachten voor bepaalde soorten bewerkingen. 


## <a name="pricing"></a>Prijzen

Persoonlijke DNS-zones is gratis tijdens de openbare preview. Deze functie wordt een op gebruik gebaseerde prijsmodel vergelijkbaar met de bestaande Azure DNS-server biedt gebruik tijdens de algemene beschikbaarheid. 


## <a name="next-steps"></a>Volgende stappen

Informatie over het maken van een persoonlijke zone in Azure DNS met behulp van de [PowerShell](./private-dns-getstarted-powershell.md) of [CLI](./private-dns-getstarted-cli.md).

Lees meer over een aantal veelvoorkomende [Privézone-scenario's](./private-dns-scenarios.md) die u kunt realiseren met Privézones in Azure DNS.

Lees informatie over de [Veelgestelde vragen over](./dns-faq.md#private-dns) voor enkele veelgestelde vragen en antwoorden op persoonlijke Zones in Azure DNS, inclusief specifiek gedrag kunt u verwachten voor bepaalde soorten bewerkingen. 

Meer informatie over DNS-zones en records in via: [DNS-zones en registreert overzicht](dns-zones-records.md).

Informatie over enkele van de andere belangrijke [netwerkmogelijkheden](../networking/networking-overview.md) van Azure.

