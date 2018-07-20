---
title: Scenario's voor Azure DNS Private Zones
description: Overzicht van veelvoorkomende scenario's voor het gebruik van Azure DNS Private Zones.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 03/15/2018
ms.author: victorh
ms.openlocfilehash: d84da36ad6b1ef3e2a507a0944aac583861d5ccb
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39162164"
---
# <a name="azure-dns-private-zones-scenarios"></a>Scenario's voor Azure DNS Private Zones
Azure DNS Private Zones bieden naamomzetting binnen een virtueel netwerk evenals tussen virtuele netwerken. In dit artikel kijken we enkele algemene scenario's die kunnen worden gerealiseerd met behulp van deze functie. 

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="scenario-name-resolution-scoped-to-a-single-virtual-network"></a>Scenario: Naamomzetting binnen het bereik van één virtueel netwerk
In dit scenario hebt u een virtueel netwerk in Azure met een aantal Azure-resources, met inbegrip van virtuele machines (VM's). U wilt oplossen, de resources van binnen het virtuele netwerk via een specifiek domeinnaam (DNS-zone) en moet u het omzetten van de persoonlijke en niet toegankelijk is vanaf internet. Voor de virtuele machines binnen het VNET moet u bovendien Azure deze automatisch wordt geregistreerd in de DNS-zone. 

Dit scenario wordt hieronder beschreven. Virtueel netwerk met de naam 'A' bevat twee VM's (VNETA VM1 en VNETA VM2). Elk van deze zijn persoonlijke IP-adressen die zijn gekoppeld. Nadat u een privé-Zone met de naam contoso.com maken en koppelen van dit virtuele netwerk als een Registration virtual network, maakt Azure DNS automatisch twee A-records in de zone zoals u kunt zien. DNS-query's uit VNETA-VM1 om op te lossen VNETA VM2.contoso.com ontvangt nu een DNS-antwoord met het privé-IP van VNETA VM2. Bovendien ontvangt een Reverse-DNS-query (Pointer) voor de privé-IP van VNETA-VM1 (10.0.0.1) uitgegeven door VNETA VM2 een DNS-antwoord met de naam van VNETA-VM1, zoals verwacht. 

![Oplossing voor één virtueel netwerk](./media/private-dns-scenarios/single-vnet-resolution.png)

## <a name="scenario-name-resolution-across-virtual-networks"></a>Scenario: Naamomzetting tussen virtuele netwerken

In dit scenario is het meest voorkomende geval waarin u wilt een Privézone koppelen aan meerdere virtuele netwerken. In dit scenario past architecturen zoals de Hub en Spoke-model, waarbij er een centrale Hub-netwerk waarin meerdere andere knooppunt virtuele netwerken zijn verbonden. De centrale Hub-netwerk kan worden gekoppeld als de Registration virtual network naar een privé-zone en Spoke-netwerken kunnen worden gekoppeld als het probleem zou moeten virtuele netwerken. 

Het volgende diagram toont een eenvoudige versie van dit scenario waarin er slechts twee virtuele netwerken - A en B. A is aangewezen als een Registration virtual network en B is aangewezen als een resolutie van virtueel netwerk. De bedoeling is voor beide virtuele netwerken voor het delen van een algemene zone contoso.com. Wanneer de zone wordt gemaakt en de resolutie en registratie van virtuele netwerken zijn gekoppeld aan de zone, worden Azure DNS-records automatisch geregistreerd voor de virtuele machines (VNETA VM1 en VNETA VM2) van het virtuele netwerk A. U kunt ook handmatig toevoegen DNS-records in de zone voor virtuele machines in het virtuele resolutienetwerk B. Met deze instelling ziet u het volgende gedrag voor voorwaartse en achterwaartse DNS-query's:
* Een DNS-query uit als VNETB VM1 in het virtuele resolutienetwerk B, voor VNETA-VM1.contoso.com, ontvangt een DNS-antwoord met het privé-IP van VNETA VM1.
* Een omgekeerde DNS PTR-query als VNETB VM2 in het virtuele resolutienetwerk B, voor 10.1.0.1, ontvangt een DNS-antwoord met de FQDN-naam als VNETB-VM1.contoso.com. De reden is dat omgekeerde DNS-query's zijn gericht op hetzelfde virtuele netwerk. 
* Een omgekeerde DNS PTR-query als VNETB VM3 in het virtuele resolutienetwerk B, voor 10.0.0.1 ontvangt NXDOMAIN. De reden is dat omgekeerde DNS-query's zijn alleen binnen het bereik van hetzelfde virtuele netwerk. 


![Oplossingen voor meerdere virtuele netwerk](./media/private-dns-scenarios/multi-vnet-resolution.png)

## <a name="scenario-split-horizon-functionality"></a>Scenario: Split-Horizon-functionaliteit

In dit scenario hebt u een use-case waar u om te profiteren van verschillende DNS-omzetting gedrag, afhankelijk van waar de client zich bevindt (binnen Azure of uit op het internet), voor dezelfde DNS-zone. Bijvoorbeeld, u hebt een persoonlijke en openbare versie van uw toepassing met een andere functie of het gedrag, maar u wilt gebruiken voor beide versies van dezelfde domeinnaam. In dit scenario kan worden gerealiseerd met Azure DNS met het maken van een openbare DNS-zone, evenals een Privézone met dezelfde naam.

Het volgende diagram ziet u in dit scenario. U hebt een virtueel netwerk A met twee VM's (VNETA VM1 en VNETA VM2) waarvoor zowel privé-IP-adressen en openbare IP-adressen toegewezen. U maakt een openbare DNS-zone met de naam contoso.com en meld u aan het openbare IP-adressen voor deze virtuele machines als DNS-records in de zone. U wordt ook een privé-DNS-zone ook met de naam contoso.com A Als het Registration virtual network op te geven. Azure registreert automatisch de virtuele machines als de A-records in de Zone privé, die verwijst naar hun privé-IP-adressen.

Nu wanneer een client een DNS-query om te controleren of VNETA VM1.contoso.com uitgeeft, wordt Azure de openbare IP-adresrecord geretourneerd uit de openbare zone. Als dezelfde DNS-query is uitgegeven door een andere virtuele machine (bijvoorbeeld: VNETA VM2) in hetzelfde virtuele netwerk een, Azure de privé-IP-record wordt geretourneerd vanaf de privézone. 

![Gesplitste Brian resolutie](./media/private-dns-scenarios/split-brain-resolution.png)

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over privé-DNS-zones raadpleegt u [Using Azure DNS for private domains](private-dns-overview.md) (Azure DNS gebruiken voor privédomeinen).

Meer informatie over het [maken van een privé-DNS-zone](./private-dns-getstarted-powershell.md) in Azure DNS.

Meer informatie over DNS-zones en records, recentst: [DNS-zones en records overzicht](dns-zones-records.md).

Informatie over enkele van de andere belangrijke [netwerkmogelijkheden](../networking/networking-overview.md) van Azure.

