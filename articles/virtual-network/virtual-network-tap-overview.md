---
title: Overzicht van Azure virtual network-Tik | Microsoft Docs
description: Meer informatie over virtueel netwerk-TAP. Virtueel netwerk Tik biedt u een grondige kopie van de virtuele machine-netwerkverkeer dat kan worden gestreamd naar een collector pakket.
services: virtual-network
documentationcenter: na
author: karthikananth
manager: ganesr
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/18/2019
ms.author: kaanan
ms.openlocfilehash: 45224b1b0ec4a4b3c93393c178f1f03baa58e10b
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58189135"
---
# <a name="virtual-network-tap"></a>Virtual Network TAP

Met Azure virtual network TAP (Terminal Access Point) kunt u continu stroom netwerkverkeer van uw virtuele machine naar een netwerk-pakket collector of analytics-hulpprogramma. De collector of analytics-hulpprogramma wordt geleverd door een [virtueel netwerkapparaat](https://azure.microsoft.com/solutions/network-appliances/) partner. Zie voor een lijst met oplossingen van partners die zijn gevalideerd om te werken met het virtuele netwerk-TAP's [partneroplossingen](#virtual-network-tap-partner-solutions).

> [!IMPORTANT]
> Virtual network TAP is momenteel in preview in alle Azure-regio's. Voor het gebruik van virtueel netwerk-TAP, moet u zich inschrijven in de Preview-versie door te sturen een e-mail naar <azurevnettap@microsoft.com> met uw abonnements-ID. U ontvangt een e-mailbevestiging zodra uw abonnement is geregistreerd. U niet de mogelijkheid gebruiken totdat u een e-mail ter bevestiging ontvangt. Deze preview wordt aangeboden zonder een service level agreement, en mag niet worden gebruikt voor werkbelastingen voor productie. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Zie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="virtual-network-tap-partner-solutions"></a>Virtueel netwerk-TAP partneroplossingen

### <a name="network-packet-brokers"></a>Netwerk-pakket brokers

- [Grote Switch Big Fabric controleren](https://www.bigswitch.com/products/big-monitoring-fabric/public-cloud/microsoft-azure)
- [Gigamon GigaSECURE](https://blog.gigamon.com/2018/09/13/why-microsofts-new-vtap-service-works-even-better-with-gigasecure-for-azure)
- [Ixia CloudLens](https://www.ixiacom.com/cloudlens/cloudlens-azure)

### <a name="security-analytics-networkapplication-performance-management"></a>Beveiligingsanalyses, netwerk-/ prestatiebeheer

- [Actief beveiliging](https://awakesecurity.com/technology-partners/microsoft-azure/)
- [Cisco Stealthwatch Cloud](https://blogs.cisco.com/security/cisco-stealthwatch-cloud-and-microsoft-azure-reliable-cloud-infrastructure-meets-comprehensive-cloud-security)
- [ExtraHop Reveal(x)](https://www.extrahop.com/company/tech-partners/microsoft/)
- [Fidelis Cyberbeveiliging](https://www.fidelissecurity.com/technology-partners/microsoft-azure )
- [Flowmon](https://www.flowmon.com/blog/azure-vtap)
- [NetFort LANGuardian](https://www.netfort.com/languardian/solutions/visibility-in-azure-network-tap/)
- [Netscout vSTREAM]( https://www.netscout.com/technology-partners/microsoft/azure-vtap)
- [Nubeva Prisms](https://www.nubeva.com/azurevtap)
- [RSA NetWitness® Platform](https://www.rsa.com/azure)
- [Vectra Cognito](https://vectra.ai/microsoftazure)

De volgende afbeelding ziet u hoe virtuele netwerk-TAP werkt. U kunt een Tik-configuratie toevoegen aan een [netwerkinterface](virtual-network-network-interface.md) die is gekoppeld aan een virtuele machine die wordt geïmplementeerd in uw virtuele netwerk. Het doel is een virtueel netwerk-IP-adres in hetzelfde virtuele netwerk bevinden als de bewaakte netwerkinterface of een [gekoppeld virtueel](virtual-network-peering-overview.md) netwerk. De collector-oplossing voor het virtuele netwerk-TAP kan worden geïmplementeerd achter een [Azure interne Load balancer](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#concepts) voor hoge beschikbaarheid. Als u wilt evalueren implementatieopties voor afzonderlijke oplossing, Zie [partneroplossingen](#virtual-network-tap-partner-solutions).

![Het virtuele netwerk-TAP werkt](./media/virtual-network-tap/architecture.png)

## <a name="prerequisites"></a>Vereisten

Voordat u een virtueel netwerk-TAP maakt, u moet hebben ontvangen een bevestigingsmail die u zijn geregistreerd bij de Preview-versie en een of meer virtuele machines die zijn gemaakt met [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) implementatiemodel en een partner oplossing voor het verzamelen van het verkeer te TIKKEN in WestCentralUS regio. Als u geen oplossing van een partner in uw virtuele netwerk hebt, Zie [partneroplossingen](#virtual-network-tap-partner-solutions) een te implementeren. U kunt hetzelfde virtuele netwerk resource te TIKKEN op het totale verkeer van meerdere netwerkinterfaces in dezelfde of verschillende abonnementen. Als de bewaakte netwerkinterfaces zich in verschillende abonnementen, is de abonnementen moeten worden gekoppeld aan dezelfde Azure Active Directory-tenant. Bovendien kunnen de bewaakte netwerkinterfaces en het eindpunt van de bestemming voor het verzamelen van het verkeer te TIKKEN in gekoppelde virtuele netwerken in dezelfde regio zijn. Als u dit implementatiemodel ervoor te zorgen dat de [peering op virtueel netwerk](virtual-network-peering-overview.md) is ingeschakeld voordat u een virtueel netwerk-TAP configureren.

## <a name="permissions"></a>Machtigingen

De accounts die u gebruiken om door te TIKKEN configuratie toepassen op netwerkinterfaces moeten worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die de noodzakelijke acties op basis van de volgende tabel is toegewezen:

| Bewerking | Name |
|---|---|
| Microsoft.Network/virtualNetworkTaps/* | Vereist voor het maken, bijwerken, lezen en verwijderen van een virtueel netwerk-TAP-resource |
| Microsoft.Network/networkInterfaces/read | Vereist voor het lezen van de network interface-resource waarop de Tik wordt geconfigureerd |
| Microsoft.Network/tapConfigurations/* | Vereist voor het maken, bijwerken, lezen en verwijderen van de configuratie te TIKKEN op een netwerkinterface |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [maken van een virtueel netwerk-TAP](tutorial-tap-virtual-network-cli.md).
