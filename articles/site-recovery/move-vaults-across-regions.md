---
title: Azure Site Recovery configuratie verplaatsen naar een andere Azure-regio | Microsoft Docs
description: Richt lijnen voor het verplaatsen van Site Recovery configuratie naar een andere Azure-regio
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/31/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: ba0e2577d6fb8bac66322917936fe06825af0d96
ms.sourcegitcommit: 6ad03fa28a0f60cb6dce6144f728c2ceb56ff6e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68708296"
---
# <a name="moving-recovery-services-vault-and--azure-site-recovery-configuration-to-another-azure-region"></a>Recovery Services kluis en Azure Site Recovery configuratie verplaatsen naar een andere Azure-regio

Er zijn verschillende scenario's waarin u uw bestaande Azure-resources uit de ene regio wilt verplaatsen naar een andere beheer baarheid, governance-redenen of als gevolg van bedrijfs samenvoegers/-acquisities, enzovoort. Een van de gerelateerde resources die u mogelijk wilt verplaatsen wanneer u Azure Vm's verplaatst, is de configuratie voor herstel na nood gevallen. Er is geen eersteklas manier om een bestaande configuratie voor herstel na nood gevallen van de ene regio naar de andere te verplaatsen. Dit is in wezen omdat u uw doel regio hebt geconfigureerd op basis van de regio van de bron-VM en als u besluit dat u de bestaande configuraties van de doel regio opnieuw wilt gebruiken en opnieuw wilt instellen. In dit artikel wordt het stapsgewijze proces beschreven voor het opnieuw configureren van de installatie van nood herstel en het verplaatsen naar een andere regio.

In dit document gaat u als volgt te werkt:

> [!div class="checklist"]
> * Controleer de vereisten voor de verplaatsing
> * De resources identificeren die zijn gebruikt door Azure Site Recovery 
> * Replicatie uitschakelen
> * De resources verwijderen 
> * Site Recovery opnieuw instellen op basis van de nieuwe bron regio voor de Vm's.

> [!IMPORTANT]
> Momenteel is er geen eerste manier om een Recovery Services kluis te verplaatsen en de DR-configuratie naar een andere regio. dit document richt zich op het proces van het uitschakelen van de replicatie en het opnieuw instellen van het in de nieuwe regio.

## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u de configuratie voor herstel na nood gevallen verwijdert en verwijdert voordat u probeert de virtuele machines van Azure naar een andere regio te verplaatsen. 

> [!NOTE]
> Als uw nieuwe doel regio voor de virtuele Azure-machine hetzelfde is als de doel regio voor herstel na nood gevallen, kunt u de bestaande replicatie Configuratie gebruiken en verplaatsen op basis van de stappen die [hier](azure-to-azure-tutorial-migrate.md) worden beschreven 

- Zorg ervoor dat u een weloverwogen beslissing neemt en dat de belanghebbenden op de hoogte worden gesteld als uw virtuele machine niet wordt beschermd tegen calamiteiten, terwijl de verplaatsing van de virtuele machine is voltooid. 

## <a name="identify-the-resources-that-were-used-by-azure-site-recovery"></a>De resources identificeren die zijn gebruikt door Azure Site Recovery
We raden u aan deze stap uit te voeren voordat u doorgaat naar de volgende, omdat het gemakkelijker is om de relevante resources te identificeren terwijl de Vm's nog worden gerepliceerd.

Voor elk van de virtuele machines van Azure die worden gerepliceerd, gaat u naar **beveiligde items** > **Eigenschappen** van**gerepliceerde items**>en identificeert u de volgende resources

- Doelresourcegroep
- Cacheopslagaccount
- Doel opslag account (in het geval van een niet-beheerde op schijven gebaseerde Azure-VM) 
- Doelnetwerk


## <a name="disable-existing-disaster-recovery-configuration"></a>Bestaande configuratie voor herstel na nood gevallen uitschakelen

1. Ga naar de **Recovery Services kluis** 
2.  Klik in **beveiligde items** > **gerepliceerde items**met de rechter muisknop op de machine > **Schakel replicatie uit**.
3. Herhaal dit voor alle virtuele machines die u wilt verplaatsen.
> [!NOTE]
> de Mobility-service wordt niet van de beveiligde servers verwijderd. u moet deze hand matig verwijderen. Als u van plan bent om de server opnieuw te beveiligen, kunt u het verwijderen van de Mobility-service overs Laan.

## <a name="delete-the-resources"></a>De resources verwijderen

1. Ga naar de **Recovery Services kluis**
2. Klik op **verwijderen**
3. Ga door met het verwijderen van alle andere resources die zijn geïdentificeerd in de[ vorige stap](#identify-the-resources-that-were-used-by-azure-site-recovery)
 
## <a name="move-azure-vms-to-the-new-target-region"></a>Virtuele Azure-machines verplaatsen naar de nieuwe doel regio

Volg de onderstaande stappen op basis van uw vereisten voor het verplaatsen van virtuele Azure-machines naar de doel regio.

- [Virtuele Azure-machines verplaatsen naar een andere regio](azure-to-azure-tutorial-migrate.md)
- [Virtuele Azure-machines verplaatsen naar beschikbaarheidszones](move-azure-VMs-AVset-Azone.md)

## <a name="re-set-up-site-recovery-based-on-the-new-source-region-for-the-vms"></a>Site Recovery opnieuw instellen op basis van de nieuwe bron regio voor de Vm's

Herstel na nood gevallen configureren voor de virtuele Azure-machines die zijn verplaatst naar de nieuwe regio met behulp van de [hier](azure-to-azure-tutorial-enable-replication.md) beschreven stappen
