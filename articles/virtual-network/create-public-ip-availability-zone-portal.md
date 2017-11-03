---
title: Maken van een zoned openbare IP-adres met de Azure Portal | Microsoft Docs
description: Maak een openbaar IP-adres in een zone beschikbaarheid met de Azure-portal.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 2fcbed2f83d66a0b4336cd1c464bb02eff3ef229
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-the-azure-portal"></a>Maken van een openbaar IP-adres in een zone beschikbaarheid met de Azure-portal

U kunt een openbare IP-adres in een zone in Azure beschikbaarheid (preview) implementeren. Een zone beschikbaarheid is een fysiek afgescheiden zone in een Azure-regio. Leer hoe u het volgende doet:

> * Maken van een openbaar IP-adres in een zone beschikbaarheid
> * Verwante bronnen die zijn gemaakt in de zone beschikbaarheid identificeren

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

> [!NOTE]
> Beschikbaarheid zones zijn Preview-versie en zijn gereed voor het ontwikkelen en testen van scenario's. Ondersteuning is beschikbaar voor Selecteer Azure-resources en regio's en families voor VM-grootte. Zie voor meer informatie over het aan de slag en welke Azure-resources, regio's en VM-grootte families u beschikbaarheid zones met proberen kunt, [overzicht van de Zones van de beschikbaarheid](https://docs.microsoft.com/azure/availability-zones/az-overview). Voor ondersteuning kunt u vragen stellen op [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) of [een Azure-ondersteuningsticket openen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij de Azure portal op https://portal.azure.com. 

## <a name="create-a-zonal-public-ip-address"></a>Een zonal openbaar IP-adres maken

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Networking**, en selecteer vervolgens **openbaar IP-adres**.
3. Typ of Selecteer waarden voor de volgende instellingen, selecteer uw abonnement, accepteer de standaardwaarden voor de overige instellingen en klik vervolgens op **maken**:

    |Instelling|Waarde|
    |---|---|
    |SKU| **Basic**: met de toewijzingsmethode statisch of dynamisch toegewezen. Kunnen worden toegewezen aan een Azure-resource die openbare IP-adres netwerkinterfaces, VPN-Gateways, Toepassingsgateways en Internet gerichte load balancers kan worden toegewezen. U kunt het openbare IP-adres toewijzen aan een specifieke zone in de **beschikbaarheid zone** instelling. Er zijn geen zone-redundante. Zie [Overzicht van beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview) voor meer informatie over beschikbaarheidszones. **Standaard**: toegewezen met alleen de statische toewijzingsmethode. Kunnen worden toegewezen aan netwerkinterfaces of standaard internetgerichte netwerktaakverdelers. Als u het openbare IP-adres aan een standaard Internet gerichte load balancer toewijst, moet u standaard. Zie [Standaard-SKU's van Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) voor meer informatie over SKU's van Azure Load Balancer. Zijn zone-redundante, standaard. Kunnen zonegebonden worden gemaakt en gegarandeerd in een bepaalde beschikbaarheidszone. De standaard-SKU zit in de evaluatieversie. Voordat u maakt een standaard SKU openbare IP-adres, moet u eerst registreren voor de preview. Zie [Registreren voor de evaluatieversie van de standaard-SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#preview-sign-up) om u te registreren voor de evaluatieversie. De SKU van de standaard kunnen alleen worden gemaakt op een ondersteunde locatie.  Zie [Beschikbaarheid in regio's](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#region-availability) voor een lijst met ondersteunde locaties (regio's) en controleer de pagina [Azure Virtual Network-updates](https://azure.microsoft.com/updates/?product=virtual-network) voor aanvullende ondersteuning voor regio's.|   
    |Naam|De naam moet uniek zijn binnen de resourcegroep die u selecteert.|
    |Resourcegroep|Klik op Nieuw en voer myResourceGroup|
    |Locatie|West-Europa|
    |Beschikbaarheid zone|Als u hebt geselecteerd de **standaard** SKU, kunt u *Zoneredundant* als u wilt dat het IP-adres zijn robuuste via zones. Als u selecteert de **Basic** SKU, het IP-adres is niet robuuste diverse zones. Ongeacht de SKU die u kiest, kunt u het adres toewijzen aan een specifieke zone als u kiest. |

    De instellingen worden weergegeven in de portal, zoals wordt weergegeven in de volgende afbeelding:

    ![Zonal openbare IP-adres maken](./media/create-public-ip-availability-zone-portal/public-ip-address.png) 

> [!NOTE]
> Als u een openbaar IP-adres van een standaard-SKU toewijst aan een netwerkinterface van een virtuele machine, moet u het bedoelde verkeer expliciet toestaan met een [netwerkbeveiligingsgroep](security-overview.md#network-security-groups). Communicatie met de resource mislukt totdat u een netwerkbeveiligingsgroep maakt en koppelt en het gewenste verkeer expliciet toestaat.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [beschikbaarheid zones](https://docs.microsoft.com/azure/availability-zones/az-overview)
- Meer informatie over [openbare IP-adressen](virtual-network-public-ip-address.md?toc=%2fazure%2fvirtual-network%2ftoc.json)