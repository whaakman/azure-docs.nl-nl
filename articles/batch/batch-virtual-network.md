---
title: Azure Batch-pool in een virtueel netwerk inrichten | Microsoft Docs
description: U kunt een Batch-pool maken in een virtueel netwerk, zodat de rekenknooppunten veilig kunnen communiceren met andere virtuele machines in het netwerk, zoals een bestandsserver.
services: batch
author: v-dotren
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 10/16/2017
ms.author: v-dotren
ms.openlocfilehash: f34647afc600b72704859952d0a40edad4a3b40f
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="create-an-azure-batch-pool-in-a-virtual-network"></a>Een Azure Batch-pool in een virtueel netwerk maken


Wanneer u een Azure Batch-pool maakt, kunt u de pool in een subnet van inrichten een [virtuele Azure-netwerk](../virtual-network/virtual-networks-overview.md) (VNet) die u opgeeft. Dit artikel wordt uitgelegd hoe u een Batch-pool in een VNet instelt. 



## <a name="why-use-a-vnet"></a>Waarom zou ik een VNet gebruiken?


Een Azure Batch-pool heeft instellingen om toe te staan van rekenknooppunten om te communiceren met elkaar - bijvoorbeeld, om uit te voeren taken met meerdere instanties. Deze instellingen hoeven niet een afzonderlijke VNet. Standaard de knooppunten kunnen niet communiceren met virtuele machines die geen deel uitmaken van de Batch-pool, zoals een licentieserver of een bestandsserver. U kunt de toepassingen in een subnet van een Azure-VNet inrichten zodat pool van rekenknooppunten om veilig te communiceren met andere virtuele machines of met een on-premises netwerk. 



## <a name="prerequisites"></a>Vereisten

* **Authentication** (Verificatie). Het gebruik van een Azure VNet vereist dat de Batch-client-API gebruikmaakt van Azure Active Directory-verificatie (AD). Azure Batch-ondersteuning voor Azure AD wordt beschreven in [Oplossingen van Batch-service verifiëren met Active Directory](batch-aad-auth.md). 

* **Een Azure VNet**. Als u van tevoren een VNet met een of meer subnetten, kunt u de Azure-portal, Azure PowerShell, de Azure-opdrachtregelinterface (CLI) of andere methoden. Zie het maken van een Azure Resource Manager gebaseerde VNet [een virtueel netwerk maken met meerdere subnetten](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Zie het maken van een klassiek VNet [een virtueel netwerk (klassiek) maken met meerdere subnetten](../virtual-network/create-virtual-network-classic.md).

### <a name="vnet-requirements"></a>Vereisten voor VNet
[!INCLUDE [batch-virtual-network-ports](../../includes/batch-virtual-network-ports.md)]
    
## <a name="create-a-pool-with-a-vnet-in-the-portal"></a>Een pool maken met een VNet in de portal

Nadat u uw VNet hebt gemaakt en toegewezen een subnet, kunt u een Batch-pool maken met dit VNet. Volg deze stappen voor het maken van een pool van de Azure-portal: 



1. Ga in Azure Portal naar uw Batch-account. Dit account moet zich in hetzelfde abonnement en dezelfde regio als de resourcegroep met de VNet die u wilt gebruiken. 
2. In de **instellingen** venster aan de linkerkant, selecteer de **Pools** menu-item.
3. In de **Pools** Selecteer de **toevoegen** opdracht.
4. Op de **van toepassingen toevoegen** venster, selecteert u de optie die u gebruiken wilt vanaf de **afbeeldingstype** vervolgkeuzelijst. 
5. Selecteer de juiste **aanbieding-Publisher-Sku** voor uw aangepaste installatiekopie.
6. Geef de overige instellingen, inclusief vereist de **knooppuntgrootte**, **gericht op specifieke knooppunten**, en **knooppunten prioriteit laag**, maar ook een optionele instellingen gewenst.
7. In **virtueel netwerk**, selecteert u het virtuele netwerk en subnet dat u wilt gebruiken.
  
  ![Toepassingen met het virtuele netwerk toevoegen](./media/batch-virtual-network/add-vnet-pool.png)

## <a name="user-defined-routes-for-forced-tunneling"></a>Gebruiker gedefinieerde routes voor geforceerde tunneling

U wellicht vereisten in uw organisatie internetverkeer omleiden (forceren) van het subnet terug naar uw on-premises locatie voor controle en logboekregistratie. U kunt hebt ingeschakeld geforceerde tunneling van de subnetten in uw VNet. 

Om ervoor te zorgen dat de rekenknooppunten van uw Azure Batch-pool werken in een VNet dat zich tunneling is ingeschakeld geforceerde heeft, moet u het volgende toevoegen [gebruiker gedefinieerde routes](../virtual-network/virtual-networks-udr-overview.md) voor dat subnet:

* De Batch-service moet communiceren met een pool van rekenknooppunten voor het plannen van taken. Toevoegen zodat deze communicatie een door de gebruiker gedefinieerde route voor elk IP-adres gebruikt door de Batch-service in de regio waar uw Batch-account bestaat. De lijst met IP-adressen van de Batch-service, neem contact op met ondersteuning van Azure.

* Zorg ervoor dat uitgaand verkeer naar Azure Storage (in het bijzonder URL's van het formulier `<account>.table.core.windows.net`, `<account>.queue.core.windows.net`, en `<account>.blob.core.windows.net`) niet is geblokkeerd via uw lokale netwerkapparaat.

Wanneer u een gebruiker gedefinieerde route toevoegt, de route voor elk gerelateerde Batch IP-adresvoorvoegsel definiëren en instellen **volgende hop type** naar **Internet**. Zie het volgende voorbeeld:

![Gebruiker gedefinieerde route](./media/batch-virtual-network/user-defined-route.png)

## <a name="next-steps"></a>Volgende stappen

- Zie voor een gedetailleerd overzicht van Batch [ontwikkelen grootschalige parallelle compute-oplossingen met Batch](batch-api-basics.md).
- Zie voor meer informatie over het maken van een gebruiker gedefinieerde route [maken van een gebruiker gedefinieerde route - Azure-portal](../virtual-network/create-user-defined-route-portal.md).
