---
title: Azure voorbereiden op herstel na noodgevallen van on-premises machines met Azure Site Recovery | Microsoft Docs
description: Informatie over het voorbereiden van Azure op herstel na noodgevallen van on-premises machines met Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: d2de871176917dcc24d910b3742bdb2700c4f25d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64691759"
---
# <a name="prepare-azure-resources-for-disaster-recovery-of-on-premises-machines"></a>Azure-resources voorbereiden op herstel na noodgevallen van on-premises machines

Dit artikel wordt beschreven hoe u Azure-resources en -onderdelen voorbereidt zodat u herstel na noodgevallen van on-premises VMware-VM's, Hyper-V-machines of Windows/Linux fysieke servers naar Azure, kunt instellen met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.

Dit artikel is de eerste zelfstudie in een reeks waarin u leest hoe u noodherstel kunt instellen voor on-premises VM’s. 


In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleer of het Azure-account replicatiemachtigingen heeft.
> * Maak een Recovery Services-kluis. Een kluis bevat metagegevens en configuratiegegevens voor VM’s, en andere replicatieonderdelen.
> * Instellen van een Azure-netwerk (VNet). Wanneer Azure-VM's zijn gemaakt na een failover, worden ze gekoppeld aan dit netwerk.

> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario. Waar mogelijk wordt gebruikgemaakt van standaardopties en niet alle mogelijke instellingen en paden worden weergegeven. Lees het artikel in de sectie How To van de Site Recovery inhoudsopgave voor gedetailleerde instructies.

## <a name="before-you-start"></a>Voordat u begint

- Architectuur voor [VMware](vmware-azure-architecture.md), [Hyper-V](hyper-v-azure-architecture.md), en [fysieke server](physical-azure-architecture.md) herstel na noodgevallen.
- Lees de veelgestelde vragen voor [VMware](vmware-azure-common-questions.md) en [Hyper-V](hyper-v-azure-common-questions.md)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint. Meld u aan de [Azure-portal](https://portal.azure.com).


## <a name="verify-account-permissions"></a>Accountmachtigingen controleren

Als u uw gratis Azure-account hebt gemaakt, bent u de beheerder van uw abonnement en u de machtigingen die u nodig hebt. Als u niet de abonnementsbeheerder bent, neemt u contact op met de beheerder om de machtigingen te krijgen die u nodig hebt. Om replicatie in te kunnen schakelen voor een nieuwe virtuele machine, moet u de volgende machtigingen hebben:

- Het maken van een VM in de geselecteerde resourcegroep.
- Het maken van een VM in het geselecteerde virtuele netwerk.
- Schrijven naar een Azure storage-account.
- Schrijven naar een door Azure beheerde schijf.

U kunt deze taken alleen uitvoeren als aan uw account de ingebouwde rol van Inzender voor virtuele machines is toegewezen. En als u Site Recovery-bewerkingen wilt beheren in een kluis, moet aan uw account ook de ingebouwde rol van Site Recovery-inzender zijn toegewezen.


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

1. Klik in de Azure-portal op **+ een resource maken**, en zoek in Marketplace naar **Recovery**.
2. Klik op **Backup and Site Recovery (OMS)** en klik op de pagina Backup and Site Recovery op **Maken**. 
1. Ga naar **Recovery Services-kluis** > **Naam** en voer een beschrijvende naam in voor de kluis. Voor deze reeks zelfstudies gebruiken we **ContosoVMVault**.
2. Selecteer bij **Resourcegroep** een bestaande resourcegroep of maak een nieuwe. Voor deze zelfstudie gebruiken we **contosoRG**.
3. Selecteer bij **Locatie** de regio waarin de kluis zich moet bevinden. In dit voorbeeld gebruiken we **Europa - west**.
4. Voor snelle toegang tot de kluis vanuit het dashboard, selecteert u **Aan dashboard vastmaken** > **Maken**.

   ![Een nieuwe kluis maken](./media/tutorial-prepare-azure/new-vault-settings.png)

   De nieuwe kluis wordt weergegeven in **Dashboard** > **Alle resources** en op de hoofdpagina **Recovery Services-kluizen**.

## <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

On-premises machines worden gerepliceerd naar Azure beheerde schijven. Wanneer een failover optreedt, worden virtuele Azure-machines gemaakt op basis van deze beheerde schijven en toegevoegd aan het Azure-netwerk dat u in deze procedure opgeeft.

1. Selecteer in [Azure Portal](https://portal.azure.com) **Een resource maken** > **Netwerken** > **Virtueel netwerk**.
2. Houd **Resource Manager** geselecteerd als het implementatiemodel.
3. Voer bij **Naam** een netwerknaam in. De naam moet uniek zijn binnen de Azure-resourcegroep. In deze zelfstudie gebruiken we **ContosoASRnet**.
4. Geef de resourcegroep op waarin het netwerk wordt gemaakt. We gebruiken de bestaande resourcegroep **contosoRG**.
5. In **adresbereik**, Voer in het bereik voor het netwerk. We maken gebruik van **10.1.0.0/24**, en niet met behulp van een subnet.
6. Selecteer bij **Abonnement** het abonnement waarin u het netwerk wilt maken.
7. In **locatie**, selecteert u de dezelfde regio als die waarin de Recovery Services-kluis is gemaakt. In onze zelfstudie heeft **West-Europa**. Het netwerk moet zich in dezelfde regio als de kluis.
8. We laten de standaardopties staan voor DDoS-basisbescherming zonder service-eindpunt op het netwerk.
9. Klik op **Create**.

   ![Een virtueel netwerk maken](media/tutorial-prepare-azure/create-network.png)

Het duurt een paar seconden voordat het virtuele netwerk is gemaakt. Nadat dit is gemaakt, wordt het in het Azure Portal-dashboard weergegeven.




## <a name="next-steps"></a>Volgende stappen

- Voor herstel na noodgevallen VMware, [voorbereiden van de on-premises VMware-infrastructuur](tutorial-prepare-on-premises-vmware.md).
- Voor herstel na noodgevallen Hyper-V, [de on-premises Hyper-V-servers voorbereiden](hyper-v-prepare-on-premises-tutorial.md).
- Voor herstel na noodgevallen fysieke server, [instellen van de configuratie van server- en bron-omgeving](physical-azure-disaster-recovery.md)
- [Meer informatie](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) over Azure-netwerken.
- [Meer informatie over](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) beheerde schijven.
