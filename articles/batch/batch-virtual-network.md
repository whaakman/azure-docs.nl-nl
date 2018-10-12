---
title: Inrichten van Azure Batch-pool in een virtueel netwerk | Microsoft Docs
description: Het maken van een Batch-pool in een Azure-netwerk, zodat rekenknooppunten veilig met andere virtuele machines in het netwerk, zoals een bestandsserver communiceren kunnen.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 10/05/2018
ms.author: danlep
ms.openlocfilehash: ef37d482e86e4ae05d3f14c78404dc395792b236
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091949"
---
# <a name="create-an-azure-batch-pool-in-a-virtual-network"></a>Een Azure Batch-pool in een virtueel netwerk maken

Wanneer u een Azure Batch-pool maakt, kunt u inrichten van de pool in een subnet van een [virtueel Azure-netwerk](../virtual-network/virtual-networks-overview.md) (VNet) die u opgeeft. In dit artikel wordt uitgelegd hoe u een Batch-pool in een VNet instelt. 

## <a name="why-use-a-vnet"></a>Waarom een VNet gebruiken?

Een Azure Batch-pool heeft instellingen om toe te staan van rekenknooppunten om te communiceren met elkaar - voorbeeld, om uit te voeren taken met meerdere instanties. Deze instellingen hoeven niet een apart VNet. Standaard de knooppunten kunnen niet communiceren met virtuele machines die geen deel uitmaken van de Batch-pool, zoals een licentieserver of een bestandsserver. Als u wilt toestaan pool van rekenknooppunten om veilig te communiceren met andere virtuele machines, of met een on-premises netwerk, kunt u de pool in een subnet van een Azure VNet inrichten. 

## <a name="prerequisites"></a>Vereisten

* **Authentication** (Verificatie). Het gebruik van een Azure VNet vereist dat de Batch-client-API gebruikmaakt van Azure Active Directory-verificatie (AD). Azure Batch-ondersteuning voor Azure AD wordt beschreven in [Oplossingen van Batch-service verifiëren met Active Directory](batch-aad-auth.md). 

* **Een Azure VNet**. Zie de volgende sectie voor vereisten voor VNet- en configuratie. Als u wilt een VNet met een of meer subnetten van tevoren voorbereid, kunt u de Azure-portal, Azure PowerShell, de Azure-opdrachtregelinterface (CLI) of andere methoden.  
  * Zie voor het maken van een Azure Resource Manager gebaseerd VNet, [maken van een virtueel netwerk](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Een Resource Manager gebaseerd VNet wordt aanbevolen voor nieuwe implementaties en wordt alleen ondersteund op groepen in de configuratie van de virtuele Machine.
  * Zie voor het maken van een klassiek VNet [een virtueel netwerk (klassiek) maken met meerdere subnetten](../virtual-network/create-virtual-network-classic.md). Een klassieke VNet wordt alleen ondersteund op groepen in de Cloud Services-configuratie.

## <a name="vnet-requirements"></a>Vereisten voor VNet

[!INCLUDE [batch-virtual-network-ports](../../includes/batch-virtual-network-ports.md)]

## <a name="create-a-pool-with-a-vnet-in-the-portal"></a>Een pool maken met een VNet in de portal

Nadat u uw VNet hebt gemaakt en toegewezen subnet, kunt u een Batch-pool maken met dit VNet. Volg deze stappen voor het maken van een groep van toepassingen vanuit Azure portal: 

1. Ga in Azure Portal naar uw Batch-account. Dit account moet zich in hetzelfde abonnement en dezelfde regio als de resourcegroep met de VNet die u wilt gebruiken. 
2. In de **instellingen** venster aan de linkerkant, selecteer de **Pools** menu-item.
3. In de **Pools** venster de **toevoegen** opdracht.
4. Op de **groep toevoegen** venster, selecteert u de optie die u gebruiken wilt vanaf de **afbeeldingstype** vervolgkeuzelijst. 
5. Selecteer de juiste **uitgever/aanbieding/Sku** voor uw aangepaste installatiekopie.
6. Geef de overige vereiste instellingen, met inbegrip van de **knooppuntgrootte**, **doel toegewezen knooppunten**, en **lage prioriteit knooppunten**, evenals een optionele instellingen desgewenst.
7. In **Virtueelnetwerk**, selecteert u het virtuele netwerk en subnet dat u wilt gebruiken.
  
  ![Groep met een virtueel netwerk toevoegen](./media/batch-virtual-network/add-vnet-pool.png)

## <a name="user-defined-routes-for-forced-tunneling"></a>Gebruiker gedefinieerde routes voor geforceerde tunneling

Mogelijk hebt vereisten in uw organisatie voor omleiden (geforceerd) internetverkeer vanuit het subnet terug naar uw on-premises locatie voor controle en logboekregistratie. U mag is voorzien van geforceerde tunnels zijn voor de subnetten in uw VNet. 

Om ervoor te zorgen dat de rekenknooppunten van de Azure Batch-pool werken in een VNet dat is geforceerde tunnels zijn ingeschakeld, moet u het volgende toevoegen [gebruiker gedefinieerde routes](../virtual-network/virtual-networks-udr-overview.md) voor dat subnet:

* De Batch-service moet communiceren met een pool van rekenknooppunten voor het plannen van taken. Toevoegen zodat deze communicatie een door de gebruiker gedefinieerde route voor elk IP-adres gebruikt door de Batch-service in de regio waar uw Batch-account bestaat. Neem contact op met ondersteuning voor Azure om de lijst met IP-adressen van de Batch-service te verkrijgen.

* Zorg ervoor dat uitgaand verkeer naar Azure Storage (vooral URL's van het formulier `<account>.table.core.windows.net`, `<account>.queue.core.windows.net`, en `<account>.blob.core.windows.net`) niet via uw on-premises netwerk-apparaat is geblokkeerd.

Wanneer u een door de gebruiker gedefinieerde route toevoegt, de route voor elk gerelateerde Batch-IP-adresvoorvoegsel definiëren en stel **volgende hoptype** naar **Internet**. Zie het volgende voorbeeld:

![Door de gebruiker gedefinieerde route](./media/batch-virtual-network/user-defined-route.png)

## <a name="next-steps"></a>Volgende stappen

- Zie voor een gedetailleerd overzicht van Batch, [grootschalige parallelle rekenoplossingen ontwikkelen met Batch](batch-api-basics.md).
- Zie voor meer informatie over het maken van een gebruiker gedefinieerde route [maken van een gebruiker gedefinieerde route - Azure-portal](../virtual-network/tutorial-create-route-table-portal.md).
