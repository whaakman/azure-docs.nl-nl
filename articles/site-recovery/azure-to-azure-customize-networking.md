---
title: Netwerk configuraties aanpassen voor failover-VM | Microsoft Docs
description: Biedt een overzicht van het aanpassen van netwerk configuraties voor failover-VM in de replicatie van virtuele Azure-machines met behulp van Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 08/07/2019
ms.author: rajanaki
ms.openlocfilehash: 907a698a675a039dfdc852210adecb94c7bfab25
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68886895"
---
# <a name="customize-networking-configurations-of-the-target-azure-vm"></a>Netwerk configuraties van de doel-Azure-VM aanpassen

Dit artikel bevat richt lijnen voor het aanpassen van netwerk configuraties op de doel-Azure-VM wanneer u virtuele machines van Azure repliceert en herstelt van de ene regio naar de andere, met behulp van [Azure site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Voordat u begint

Meer informatie over hoe Site Recovery herstel na nood gevallen biedt voor [dit scenario](azure-to-azure-architecture.md).

## <a name="support-networking-resources"></a>Ondersteunings netwerk bronnen

De volgende belang rijke bron configuraties kunnen worden gegeven voor de failover-VM tijdens het repliceren van virtuele Azure-machines.

- [Interne load balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#what-is-standard-load-balancer)
- [Openbare IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses)
- [Netwerk beveiligings groep](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group) zowel voor het subnet als voor de NIC

## <a name="pre-requisites"></a>Vereisten

- Zorg ervoor dat u de configuraties van de herstel kant van tevoren plant.
- U moet de netwerk bronnen vooraf maken. Geef het op als invoer zodat Azure Site Recovery service deze instellingen kan naleven en ervoor moet zorgen dat de failover-VM aan deze instellingen voldoet.

## <a name="steps-to-customize-failover-networking-configurations"></a>Stappen voor het aanpassen van failover-netwerk configuraties

1. Navigeer naar **gerepliceerde items**. 
2. Klik op de gewenste Azure-VM.
3. Klik op **Compute en netwerk**en vervolgens op **bewerken**. U zult zien dat de configuratie-instellingen van de NIC de bijbehorende resources op de bron bevatten. 

     ![Aanpassen](media/azure-to-azure-customize-networking/edit-networking-properties.png)

4. Klik op **bewerken** in de buurt van de NIC die u wilt configureren. Selecteer in de volgende Blade die wordt geopend de bijbehorende vooraf gemaakte resources in het doel.

    ![NIC-DrillDown](media/azure-to-azure-customize-networking/nic-drilldown.png) 

5. Klik op **OK**.

Site Recovery gaat nu door met deze instellingen en zorg ervoor dat de virtuele machine op failover is verbonden met de geselecteerde resource via de bijbehorende NIC.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="unable-to-view-or-select-a-resource"></a>Een resource kan niet worden weer gegeven of geselecteerd

Als u geen netwerk bron kunt selecteren of weer geven, gaat u door de volgende controles & omstandigheden:

- Het doel veld voor een netwerk bron wordt alleen ingeschakeld als de bron-VM een overeenkomende invoer heeft. Dit is gebaseerd op het principe dat voor een nood herstel scenario u een exacte of een geschaalde versie van de bron moet hebben.
- Voor elk van de netwerk bronnen in kwestie worden sommige filters toegepast in de vervolg keuzelijst om ervoor te zorgen dat de virtuele machine voor failover kan worden gekoppeld aan de geselecteerde resource en dat de betrouw baarheid van de failover wordt gehandhaafd. Deze filters zijn gebaseerd op de netwerk omstandigheden die tijdens de configuratie van de bron-VM zouden zijn gecontroleerd.

Interne load balancer validaties:

1. Het abonnement en de regio van LB en de doel-VM moeten hetzelfde zijn.
2. Het virtuele netwerk dat is gekoppeld aan de interne Load Balancer en de VMshould van het doel, moeten hetzelfde zijn.
3. De open bare IP-SKU van de doel-VM en de SKU van de interne Load Balancer moeten hetzelfde zijn.
4. Als de doel-VM is geconfigureerd om in een beschikbaarheids zone te worden geplaatst, controleert u of de load balancer zone redundant is of deel uitmaakt van een beschikbaarheids zone. (Basic SKU load balancers bieden geen ondersteuning voor zones en worden niet weer gegeven in de vervolg keuzelijst in dit geval.)
5. Zorg ervoor dat de interne Load Balancer een vooraf gemaakte back-end-pool en front-end-configuratie heeft.


Openbaar IP-adres:
    
1. Het abonnement en de regio van het open bare IP-adres en de doel-VM moeten hetzelfde zijn.
2. De open bare IP-SKU van de doel-VM en de SKU van de interne Load Balancer moeten hetzelfde zijn.

Netwerk beveiligings groep:
1. Het abonnement en de regio van de netwerk beveiligings groep en de doel-VM moeten hetzelfde zijn.


> [!WARNING]
> Als de doel-VM is gekoppeld aan een Beschikbaarheidsset, moet u de open bare IP/interne Load Balancer koppelen van dezelfde SKU als de open bare IP/interne Load Balancer van de andere VM in de Beschikbaarheidsset. Als u dit niet doet, kan dit leiden tot een failover-fout.
