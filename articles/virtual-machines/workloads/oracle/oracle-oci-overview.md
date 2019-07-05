---
title: Integratie van Microsoft Azure met Oracle-Cloudinfrastructuur | Microsoft Docs
description: Meer informatie over de oplossingen die Oracle-apps die zijn uitgevoerd op Microsoft Azure met databases in Oracle Cloud Infrastructure (OCI) integreren.
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: jeconnoc
tags: ''
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/04/2019
ms.author: rogirdh
ms.custom: ''
ms.openlocfilehash: 5a60e41d3195c0f7d88fd3ba14336d693d2f528e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446685"
---
# <a name="oracle-application-solutions-integrating-microsoft-azure-and-oracle-cloud-infrastructure-preview"></a>Oplossingen voor Oracle-toepassingen integreren van Microsoft Azure en Oracle Cloud-infrastructuur (preview)

Microsoft en Oracle werken samen voor lage latentie en hoge doorvoer cross-cloud-connectiviteit, zodat u kunt om te profiteren van het beste van beide clouds. 

Met behulp van deze connectiviteit binnen de cloud, kunt u een toepassing met meerdere lagen om uit te voeren van uw database-laag op Oracle Cloud Infrastructure (OCI) en de toepassing en andere lagen op Microsoft Azure partitioneren. De ervaring is vergelijkbaar met die voor de gehele oplossing-stack in een enkele cloud. 

> [!IMPORTANT]
> Deze cross-cloud-functionaliteit is momenteel in preview en sommige [gelden beperkingen](#preview-limitations). Previews worden voor u beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt.

Als u geïnteresseerd bent in Oracle-oplossingen volledig op Azure-infrastructuur implementeren, Zie [Oracle VM-installatiekopieën en de implementatie ervan op Microsoft Azure](oracle-vm-solutions.md).

## <a name="scenario-overview"></a>Overzicht van scenario's

Cross-cloud-connectiviteit biedt een oplossing voor u van Oracle toonaangevende toepassingen en uw eigen aangepaste toepassingen uitvoeren op Azure virtual machines terwijl u tegelijkertijd profiteert van de voordelen van services in OCI gehoste database. 

U in een configuratie met meerdere cloud uitvoeren kunt-toepassingen zijn:

* E-Business Suite
* JD Edwards EnterpriseOne
* PeopleSoft
* Oracle Retail-toepassingen
* Oracle Hyperion financieel beheer

Het volgende diagram wordt een overzicht op hoog niveau van de oplossing voor verbonden. Voor het gemak toont het diagram alleen de toepassingslaag van een en een gegevenslaag. Afhankelijk van de toepassingsarchitectuur, kan uw oplossing meer lagen, zoals een weblaag opnemen in Azure. Zie de volgende secties voor meer informatie.

![Overzicht van de oplossing Azure OCI](media/oracle-oci-overview/crosscloud.png)

## <a name="preview-limitations"></a>Preview-beperkingen

* Cross-cloud-connectiviteit in preview is beperkt tot de regio VS-Azure-Oost (VS-Oost) en de regio van OCI Ashburn (VS-ashburn-1).

## <a name="networking"></a>Netwerken

Zakelijke klanten kiezen vaak beheersleutel en workloads via meerdere clouds voor verschillende zakelijke en operationele redenen implementeren. Als u wilt beheersleutel, interconnect klanten cloudnetwerken met behulp van het internet, IPSec VPN of met behulp van de cloudprovider directe connectiviteit oplossing via uw on-premises netwerk. Cloudnetwerken elkaar, kan aanzienlijke investeringen in tijd, geld, ontwerp, inkoop, installatie, testen en bewerkingen vereisen. 

Om deze problemen van klanten op te lossen, is Oracle en Microsoft voorzien van een geïntegreerde meerdere cloud-ervaring. Netwerken binnen de cloud tot stand is gebracht door verbinding te maken een [ExpressRoute](../../../expressroute/expressroute-introduction.md) -circuit in Microsoft Azure met een [FastConnect](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/fastconnectoverview.htm) -circuit in OCI. Deze connectiviteit is mogelijk, waarbij een peering Azure ExpressRoute-locatie in de nabijheid of in de dezelfde locatie als de FastConnect OCI is. Deze instelling kunt u veilig en snelle connectiviteit tussen de twee clouds zonder de noodzaak voor een tussenliggende-provider.

Met behulp van ExpressRoute en FastConnect, kunnen klanten een virtueel netwerk in Azure met een cloudnetwerk van virtuele in OCI, peer voorwaarde dat de persoonlijke IP-adresruimte elkaar niet overlappen. De twee netwerken kunt u een resource in het virtuele netwerk om te communiceren met een resource in het netwerk van de virtuele cloud OCI alsof ze beide in hetzelfde netwerk zijn.

## <a name="network-security"></a>Netwerkbeveiliging

Netwerkbeveiliging is een essentieel onderdeel van een bedrijfstoepassing, en centraal staat in deze meerdere cloud-oplossing. Verkeer gaat via ExpressRoute en FastConnect wordt doorgegeven via een particulier netwerk. Deze configuratie maakt het mogelijk voor veilige communicatie tussen een Azure-netwerk en een virtuele Oracle-cloudnetwerk. U hoeft niet te bieden van een openbaar IP-adres op virtuele machines in Azure. Op deze manier hoeft u niet een internet-gateway in OCI. Alle communicatie plaatsvindt via de privé IP-adres van de machines.

Bovendien kunt u instellen [beveiligingslijsten](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/securitylists.htm) op uw netwerk- en -regels voor de virtuele cloud voor OCI (die zijn gekoppeld aan Azure [netwerkbeveiligingsgroepen](../../../virtual-network/security-overview.md)). Deze regels gebruiken voor het beheren van het verkeer tussen de machines in de virtuele netwerken. Netwerkbeveiligingsregels kunnen worden toegevoegd op het computerniveau van een, op het subnetniveau van een, evenals op het niveau van het virtuele netwerk.
 
## <a name="identity"></a>Identiteit

Identiteit is een van de belangrijkste onderdelen van de samenwerking tussen Microsoft en Oracle. Aanzienlijke werk is verricht als u wilt integreren [Oracle Identiteitsservice in de Cloud](https://docs.oracle.com/en/cloud/paas/identity-cloud/index.html) (IDCS) met [Azure Active Directory](../../../active-directory/index.yml) (Azure AD). Azure AD is van Microsoft cloud-gebaseerde identiteits- en toegangsbeheer management-service. Het helpt uw gebruikers zich aanmelden en toegang tot verschillende bronnen. Azure AD kunt u uw gebruikers en hun machtigingen beheren.

Momenteel kunt deze integratie u beheren in één centrale locatie, die Azure Active Directory. Azure AD de wijzigingen in de directory synchroniseert met de bijbehorende Oracle-directory en wordt gebruikt voor eenmalige aanmelding voor cross-cloud Oracle-oplossingen.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met een [cross-cloudnetwerk](configure-azure-oci-networking.md) tussen Azure en OCI. 

Zie voor meer informatie en technische documenten over OCI de [Oracle Cloud](https://docs.cloud.oracle.com/iaas/Content/home.htm) documentatie.
