---
title: Verbinding maken met Azure Data Lake Storage Gen1 vanuit VNETs | Microsoft Docs
description: Verbinding maken met Azure Data Lake Storage Gen1 vanuit Azure VNETs
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
ms.openlocfilehash: 130d0154fc0558ae7284e8407ba88fda3a2a53d5
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391297"
---
# <a name="access-azure-data-lake-storage-gen1-from-vms-within-an-azure-vnet"></a>Toegang tot Azure Data Lake Storage Gen1 van virtuele machines binnen een Azure VNET
Azure Data Lake Storage Gen1 is een PaaS-service die wordt uitgevoerd op het openbare Internet-IP-adressen. Een server die u verbinding met het openbare Internet maken kunt kunt maken doorgaans verbinding met Azure Data Lake Storage Gen1 eindpunten ook. Standaard worden alle virtuele machines die zich in Azure vnet's hebben toegang tot het Internet en daarom hebben toegang tot Azure Data Lake Storage Gen1. Het is echter mogelijk om te configureren van virtuele machines in een VNET-naar-geen toegang tot het Internet. Voor dergelijke VM's wordt toegang tot Azure Data Lake Storage Gen1 beperkt ook. Openbare toegang via Internet blokkeren voor virtuele machines in Azure vnet's kan worden gedaan met behulp van een van de volgende methoden:

* Door het configureren van de Netwerkbeveiligingsgroep groepen (NSG)
* Door gebruiker gedefinieerde Routes (UDR) configureren
* Door het uitwisselen van routes via BGP (dynamische routering standaardprotocol), wanneer u ExpressRoute gebruikt, die toegang blokkeren tot met Internet

In dit artikel leert u hoe u kunt toegang inschakelen voor Azure Data Lake Storage Gen1 van Azure VM's, die zijn beperkt tot toegang tot resources met behulp van een van de drie methoden die eerder is vermeld.

## <a name="enabling-connectivity-to-azure-data-lake-storage-gen1-from-vms-with-restricted-connectivity"></a>Om verbinding met Azure Data Lake Storage Gen1 van virtuele machines met beperkte connectiviteit
Voor toegang tot Azure Data Lake Storage Gen1 van dergelijke virtuele machines, moet u configureren zodat ze toegang krijgen tot het IP-adres waar het Gen1 van Azure Data Lake Storage-account beschikbaar is. U kunt de IP-adressen voor de accounts van uw Data Lake Storage Gen1 identificeren door het omzetten van de DNS-namen van uw accounts (`<account>.azuredatalakestore.net`). U kunt oplossen door een DNS-namen van uw accounts, kunt u hulpprogramma's zoals **nslookup**. Open een opdrachtprompt op uw computer en voer de volgende opdracht uit:

    nslookup mydatastore.azuredatalakestore.net

De uitvoer lijkt op het volgende. De waarde op basis van **adres** eigenschap is het IP-adres dat is gekoppeld aan uw Data Lake Storage Gen1-account.

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a>Inschakelen van de connectiviteit van VM's die zijn beperkt met behulp van NSG
Wanneer een NSG-regel wordt gebruikt om toegang tot het Internet te blokkeren, kunt u een andere NSG waarmee toegang tot het Data Lake Storage Gen1 IP-adres maken. Zie voor meer informatie over NSG-regels, [overzicht van netwerkbeveiligingsgroepen](../virtual-network/security-overview.md). Zie voor instructies over het maken van nsg's [over het maken van een netwerkbeveiligingsgroep](../virtual-network/tutorial-filter-network-traffic.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a>Inschakelen van de connectiviteit van VM's die zijn beperkt met behulp van UDR of ExpressRoute
Wanneer, routes, udr's of uitgewisseld BGP routes worden gebruikt om toegang tot het Internet te blokkeren, moet een speciale route worden geconfigureerd zodat virtuele machines in deze subnetten toegang Data Lake Storage Gen1 eindpunten tot hebben. Zie voor meer informatie, [overzicht van de gebruiker gedefinieerde routes](../virtual-network/virtual-networks-udr-overview.md). Zie voor instructies over het maken van udr's [udr's van maken in Resource Manager](../virtual-network/tutorial-create-route-table-powershell.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a>Inschakelen van de connectiviteit van VM's die zijn beperkt met behulp van ExpressRoute
Wanneer een ExpressRoute-circuit is geconfigureerd, kunnen de on-premises servers toegang tot Data Lake Storage Gen1 via openbare peering. Meer informatie over het configureren van ExpressRoute voor openbare peering is beschikbaar op [Veelgestelde vragen over ExpressRoute](../expressroute/expressroute-faqs.md).

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Storage Gen1](data-lake-store-overview.md)
* [Het beveiligen van gegevens die zijn opgeslagen in Azure Data Lake Storage Gen1](data-lake-store-security-overview.md)

