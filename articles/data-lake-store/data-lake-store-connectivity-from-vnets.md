---
title: Verbinding maken met Azure Data Lake Store vanaf VNETs | Microsoft Docs
description: Verbinding maken met Azure Data Lake Store vanaf Azure VNETs
services: data-lake-store,data-catalog
documentationcenter: ''
author: esung22
manager: jhubbard
editor: cgronlun
ms.assetid: 683fcfdc-cf93-46c3-b2d2-5cb79f5e9ea5
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: elsung
ms.openlocfilehash: 4086ef6ce2a95e0467eda61116ac002cf53610b5
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300791"
---
# <a name="access-azure-data-lake-store-from-vms-within-an-azure-vnet"></a>Toegang tot Azure Data Lake Store van VM's binnen een Azure VNET
Azure Data Lake Store is een PaaS-service die wordt uitgevoerd op het openbare Internet IP-adressen. Een server die verbinding met het openbare Internet maken kan kunt maken doorgaans verbinding met Azure Data Lake Store-eindpunten ook. Standaard alle VM's die in Azure VNETs kunnen toegang tot het Internet en daarmee toegang tot Azure Data Lake Store. Het is echter mogelijk voor het configureren van virtuele machines in een VNET-naar-geen toegang tot het Internet. Voor deze VM's is toegang tot Azure Data Lake Store beperkt ook. Openbare toegang tot Internet blokkeren voor virtuele machines in Azure VNETs kan worden gedaan met behulp van een van de volgende methoden:

* Door het configureren van Netwerkbeveiligingsgroep groepen (NSG)
* Door gebruiker gedefinieerde Routes (UDR) configureren
* Door het uitwisselen van routes via BGP (dynamische routering standaardprotocol) wanneer u ExpressRoute gebruikt, die toegang blokkeren tot met het Internet

In dit artikel leert u hoe u toegang inschakelen voor Azure Data Lake Store van de Azure VM's, die zijn beperkt tot toegang tot bronnen met behulp van een van de drie methoden die eerder is vermeld.

## <a name="enabling-connectivity-to-azure-data-lake-store-from-vms-with-restricted-connectivity"></a>Connectiviteit met Azure Data Lake Store inschakelen van VM's met beperkte connectiviteit
Voor toegang tot Azure Data Lake Store van dergelijke virtuele machines, moet u configureren zodat ze toegang krijgen tot het IP-adres waar het Azure Data Lake Store-account beschikbaar is. U kunt de IP-adressen voor uw Data Lake Store-accounts identificeren door de DNS-namen van uw accounts op te lossen (`<account>.azuredatalakestore.net`). U kunt oplossen door DNS-namen van uw accounts, kunt u hulpprogramma's zoals **nslookup**. Open een opdrachtprompt op de computer en voer de volgende opdracht:

    nslookup mydatastore.azuredatalakestore.net

De uitvoer lijkt op de volgende. De waarde ten opzichte van **adres** eigenschap is het IP-adres dat is gekoppeld aan uw Data Lake Store-account.

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a>Verbinding van VM's met behulp van de NSG beperkt inschakelen
Wanneer een regel voor het NSG wordt gebruikt voor het blokkeren van toegang tot het Internet, kunt u een andere NSG die toegang tot het Data Lake Store IP-adres verleent maken. Zie voor meer informatie over het NSG-regels [netwerk beveiligingsgroepen overzicht](../virtual-network/security-overview.md). Zie voor instructies over het maken van de nsg's [het maken van een netwerkbeveiligingsgroep](../virtual-network/tutorial-filter-network-traffic.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a>Verbinding van VM's met behulp van UDR of ExpressRoute beperkt inschakelen
Als routes, udr's of uitgewisseld BGP routes worden gebruikt voor het blokkeren van toegang tot het Internet, moet een speciale route worden geconfigureerd zodat virtuele machines in deze subnetten toegang Data Lake Store-eindpunten tot hebben. Zie voor meer informatie [overzicht van de gebruiker gedefinieerde routes](../virtual-network/virtual-networks-udr-overview.md). Zie voor instructies over het maken van udr's [maken udr's in Resource Manager](../virtual-network/tutorial-create-route-table-powershell.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a>Verbinding van VM's met behulp van ExpressRoute beperkt inschakelen
Wanneer een ExpressRoute-circuit is geconfigureerd, kunnen de on-premises servers toegang tot Data Lake Store via openbare peering. Meer informatie over het configureren van ExpressRoute voor openbare peering is beschikbaar op [Veelgestelde vragen over ExpressRoute](../expressroute/expressroute-faqs.md).

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Store](data-lake-store-overview.md)
* [De beveiliging van gegevens die zijn opgeslagen in Azure Data Lake Store](data-lake-store-security-overview.md)

