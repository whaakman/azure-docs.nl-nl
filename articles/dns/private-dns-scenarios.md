---
title: Scenario's voor Azure DNS-Zones die persoonlijke | Microsoft Docs
description: Overzicht van algemene scenario's voor het gebruik van Azure persoonlijke DNS-Zones.
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
ms.openlocfilehash: de543913d4f8264fa8e5b3bca0c510c99c479cae
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32771868"
---
# <a name="azure-dns-private-zones-scenarios"></a>Scenario's Azure persoonlijke DNS-Zones
Azure persoonlijke DNS-Zones naamomzetting binnen een virtueel netwerk ook tussen virtuele netwerken. In dit artikel kijken we enkele algemene scenario's die kunnen worden gerealiseerd met behulp van deze functie. 

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="scenario-name-resolution-scoped-to-a-single-virtual-network"></a>Scenario: Naamomzetting binnen het bereik van één virtueel netwerk
In dit scenario hebt u een virtueel netwerk in Azure met een aantal Azure-resources, met inbegrip van virtuele machines (VM's). U wilt oplossen, de bronnen van binnen het virtuele netwerk via een specifiek domein-naam (DNS-zone) en moet u de naamomzetting persoonlijke en niet toegankelijk is vanaf internet. Voor de virtuele machines binnen het VNET moet u bovendien Azure deze automatisch wordt geregistreerd in de DNS-zone. 

Dit scenario wordt hieronder beschreven. Virtueel netwerk met de naam "A" bevat twee VMs (VNETA VM1 en VNETA VM2). Elk van deze persoonlijke IP-adressen die zijn gekoppeld hebben. Nadat u een privé-Zone met de naam contoso.com maken en koppelen van dit virtuele netwerk als een virtueel netwerk voor registratie, maakt Azure DNS automatisch twee A-records in de zone zoals afgebeeld. DNS-query's uit VNETA-VM1 VNETA VM2.contoso.com omzetten ontvangt nu een DNS-antwoord dat de privé-IP van VNETA VM2 bevat. Een omgekeerde DNS-query (Pointer) voor de privé-IP van VNETA-VM1 (10.0.0.1) uitgegeven door VNETA VM2 ontvangt bovendien een DNS-antwoord dat de naam van VNETA-VM1, bevat zoals verwacht. 

![Oplossing voor één virtueel netwerk](./media/private-dns-scenarios/single-vnet-resolution.png)

## <a name="scenario-name-resolution-across-virtual-networks"></a>Scenario: Naamomzetting tussen virtuele netwerken

Dit scenario is de meeste gevallen waarin u wilt koppelen aan een Zone persoonlijke meerdere virtuele netwerken. Dit scenario past architecturen zoals de Hub en Spoke-model wanneer er een centrale Hub virtueel netwerk waarin meerdere andere Spoke virtuele netwerken zijn verbonden. Het virtuele netwerk van centrale Hub kan worden gekoppeld als het virtuele netwerk van de registratie van een zone voor persoonlijke en de Spoke virtuele netwerken kunnen worden gekoppeld als de virtuele netwerken resolutie. 

Het volgende diagram toont een eenvoudig versie van dit scenario met slechts twee virtuele netwerken - A en B. A is aangewezen als een virtueel netwerk voor registratie en B is aangewezen als een virtueel netwerk resolutie. De bedoeling is voor beide virtuele netwerken voor het delen van een algemene zone contoso.com. Als de zone wordt gemaakt en de resolutie en registratie van virtuele netwerken zijn gekoppeld aan de zone, wordt Azure DNS-records automatisch geregistreerd voor de virtuele machines (VNETA VM1 en VNETA VM2) van het virtuele netwerk A. U kunt ook handmatig toevoegen DNS-records in de zone voor virtuele machines in het virtuele netwerk van resolutie B. Met deze instelling ziet u het volgende gedrag voor voorwaartse en achterwaartse DNS-query's:
* Een DNS-query uit VNETB VM1 in de omzetting van virtueel netwerk B, voor VNETA-VM1.contoso.com, ontvangt een DNS-antwoord met de privé-IP van VNETA VM1.
* Een omgekeerde DNS PTR-query uit VNETB VM2 in de omzetting van virtueel netwerk B, voor 10.1.0.1, ontvangt een DNS-antwoord met de FQDN-naam VNETB-VM1.contoso.com. De reden is dat de Reverse DNS-query's zijn gericht op hetzelfde virtuele netwerk. 
* Een omgekeerde DNS PTR-query van VNETB VM3 in de omzetting van virtueel netwerk B, voor 10.0.0.1 ontvangt NXDOMAIN. De reden is dat Reverse DNS-query's zijn alleen binnen het bereik van hetzelfde virtuele netwerk. 


![Oplossingen voor meerdere virtuele netwerk](./media/private-dns-scenarios/multi-vnet-resolution.png)

## <a name="scenario-split-horizon-functionality"></a>Scenario: Split-Horizon functionaliteit

In dit scenario hebt u een gebruiksvoorbeeld waarvoor mee ander gedrag, afhankelijk van waar de client zich bevindt (binnen Azure of af op het internet), DNS-naamomzetting voor dezelfde DNS-zone. Bijvoorbeeld, u misschien een persoonlijke en openbare versie van uw toepassing met een andere functie of het gedrag, maar u wilt gebruiken voor beide versies van dezelfde domeinnaam. Dit scenario kan worden gerealiseerd met Azure DNS door een openbare DNS-zone, evenals een privé-Zone maken met dezelfde naam.

Het volgende diagram illustreert dit scenario. U hebt een virtueel netwerk A met twee VMs (VNETA VM1 en VNETA VM2) waarvoor zowel privé-IP-adressen en openbare IP-adressen toegewezen. U maakt een openbare DNS-zone contoso.com aangeroepen en registreren van het openbare IP-adressen voor deze virtuele machines als DNS-records in de zone. U ook maken een persoonlijke DNS-zone contoso.com A opgeven als het virtuele netwerk van de registratie een afkorting. De virtuele machines Azure automatisch geregistreerd als een records in de eigen Zone, hun privé-IP-adressen aan te wijzen.

Nu wanneer een client een DNS-query om te zoeken VNETA VM1.contoso.com uitgeeft, wordt Azure de openbare IP-adresrecord geretourneerd uit de openbare zone. Als dezelfde DNS-query is uitgegeven door een andere virtuele machine (bijvoorbeeld: VNETA VM2) in hetzelfde virtuele netwerk A, Azure de privé IP-adresrecord retourneert uit de zone persoonlijke. 

![Gesplitste Brian resolutie](./media/private-dns-scenarios/split-brain-resolution.png)

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over privé-DNS-zones raadpleegt u [Using Azure DNS for private domains](private-dns-overview.md) (Azure DNS gebruiken voor privédomeinen).

Meer informatie over hoe [een persoonlijke DNS-zone maken](./private-dns-getstarted-powershell.md) in Azure DNS.

Meer informatie over DNS-zones en records in via: [DNS-zones en registreert overzicht](dns-zones-records.md).

Informatie over enkele van de andere belangrijke [netwerkmogelijkheden](../networking/networking-overview.md) van Azure.

