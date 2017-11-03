---
title: Verbinding maken met Azure Data Lake Store vanaf VNETs | Microsoft Docs
description: Verbinding maken met Azure Data Lake Store vanaf Azure VNETs
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 683fcfdc-cf93-46c3-b2d2-5cb79f5e9ea5
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 029939eabc7283d7591576fa1b14731a9e626a88
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="access-azure-data-lake-store-from-vms-within-an-azure-vnet"></a>Toegang tot Azure Data Lake Store van VM's binnen een Azure VNET
Azure Data Lake Store is een PaaS-service die wordt uitgevoerd op het openbare Internet IP-adressen. Een server die verbinding met het openbare Internet maken kan kunt maken doorgaans verbinding met Azure Data Lake Store-eindpunten ook. Standaard alle VM's die in Azure VNETs kunnen toegang tot het Internet en daarmee toegang tot Azure Data Lake Store. Het is echter mogelijk voor het configureren van virtuele machines in een VNET-naar-geen toegang tot het Internet. Voor deze VM's is toegang tot Azure Data Lake Store beperkt ook. Openbare toegang tot Internet blokkeren voor virtuele machines in Azure VNETs kan worden gedaan met behulp van de volgende benadering.

* Door het configureren van Netwerkbeveiligingsgroep groepen (NSG)
* Door gebruiker gedefinieerde Routes (UDR) configureren
* Door het uitwisselen van routes via BGP (dynamische routering standaardprotocol) wanneer u ExpressRoute gebruikt dat blok toegang tot het Internet

In dit artikel leert u hoe u toegang inschakelen voor Azure Data Lake Store van de Azure VM's die zijn beperkt tot toegang tot bronnen met behulp van een van de drie methoden die hierboven worden genoemd.

## <a name="enabling-connectivity-to-azure-data-lake-store-from-vms-with-restricted-connectivity"></a>Connectiviteit met Azure Data Lake Store inschakelen van VM's met beperkte connectiviteit
Voor toegang tot Azure Data Lake Store van dergelijke virtuele machines, moet u configureren zodat ze toegang krijgen tot het IP-adres waar het Azure Data Lake Store-account beschikbaar is. U kunt de IP-adressen voor uw Data Lake Store-accounts identificeren door de DNS-namen van uw accounts op te lossen (`<account>.azuredatalakestore.net`). Hiervoor kunt u hulpprogramma's zoals **nslookup**. Open een opdrachtprompt op de computer en voer de volgende opdracht.

    nslookup mydatastore.azuredatalakestore.net

De uitvoer lijkt op de volgende. De waarde ten opzichte van **adres** eigenschap is het IP-adres dat is gekoppeld aan uw Data Lake Store-account.

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a>Verbinding van VM's met behulp van de NSG beperkt inschakelen
Wanneer u een NSG-regel wordt gebruikt voor het blokkeren van toegang tot het Internet, kunt u een andere NSG die toegang tot het Data Lake Store IP-adres verleent maken. Meer informatie over het NSG-regels is beschikbaar op [wat is er een Netwerkbeveiligingsgroep?](../virtual-network/virtual-networks-nsg.md). Zie voor instructies over het maken van de nsg's [het nsg's met de Azure portal beheren](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a>Verbinding van VM's met behulp van UDR of ExpressRoute beperkt inschakelen
Als routes, udr's of uitgewisseld BGP routes worden gebruikt voor het blokkeren van toegang tot het Internet, moet een speciale route worden geconfigureerd zodat virtuele machines in deze subnetten toegang Data Lake Store-eindpunten tot hebben. Zie voor meer informatie [wat de gebruiker gedefinieerde Routes zijn?](../virtual-network/virtual-networks-udr-overview.md). Zie voor instructies over het maken van udr's [maken udr's in Resource Manager](../virtual-network/virtual-network-create-udr-arm-ps.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a>Verbinding van VM's met behulp van ExpressRoute beperkt inschakelen
Wanneer een ExpressRoute-circuit is geconfigureerd, kunnen de on-premises servers toegang tot Data Lake Store via openbare peering. Meer informatie over het configureren van ExpressRoute voor openbare peering is beschikbaar op [Veelgestelde vragen over ExpressRoute](../expressroute/expressroute-faqs.md).

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Store](data-lake-store-overview.md)
* [De beveiliging van gegevens die zijn opgeslagen in Azure Data Lake Store](data-lake-store-security-overview.md)

