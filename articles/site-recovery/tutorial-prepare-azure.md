---
title: Azure voorbereiden op herstel na noodgevallen van on-premises machines met Azure Site Recovery | Microsoft Docs
description: Informatie over het voorbereiden van Azure op herstel na noodgevallen van on-premises machines met Azure Site Recovery.
services: site-recovery
author: mayurigupta13
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/03/2019
ms.author: mayg
ms.custom: MVC
ms.openlocfilehash: 5168fc28952631f00c2415d6bc171a130dc85dfd
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57838558"
---
# <a name="prepare-azure-resources-for-disaster-recovery-of-on-premises-machines"></a>Azure-resources voorbereiden op herstel na noodgevallen van on-premises machines

 [Azure Site Recovery](site-recovery-overview.md) draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel (BCDR) door te zorgen dat uw zakelijke apps actief blijven tijdens geplande en ongeplande uitval. Site Recovery beheert en orkestreert noodherstel van on-premises machines en virtuele Azure-machines (VM's), met inbegrip van replicatie, failover en herstel.

Dit artikel is de eerste zelfstudie in een reeks waarin u leest hoe u noodherstel kunt instellen voor on-premises VM’s. Dit is van belang bij het beveiligen van on-premises VMware-VM’s, Hyper-V-VM’s of fysieke servers.

> [!NOTE]
> In deze zelfstudies ziet u steeds het eenvoudigste implementatiepad voor een scenario. Waar mogelijk wordt gebruikgemaakt van standaardopties en niet alle mogelijke instellingen en paden worden weergegeven. Raadpleeg de sectie **Instructies** voor het betreffende scenario voor gedetailleerde instructies.

Dit artikel laat zien hoe u Azure-onderdelen voorbereidt wanneer u on-premises VM’s (Hyper-V of VMware) of fysieke Windows- of Linux-servers wilt repliceren naar Azure. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleren of uw Azure-account replicatiemachtigingen heeft.
> * Maak een Recovery Services-kluis. Een kluis bevat metagegevens en configuratiegegevens voor VM’s, en andere replicatieonderdelen.
> * Stel een Azure-netwerk in. Wanneer de Azure VM's zijn gemaakt na de failover, worden ze gekoppeld aan dit Azure-netwerk.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="verify-account-permissions"></a>Accountmachtigingen controleren

Als u net pas uw gratis Azure-account hebt gemaakt, bent u de beheerder van uw abonnement. Als u niet de abonnementsbeheerder bent, neemt u contact op met de beheerder om de machtigingen te krijgen die u nodig hebt. Om replicatie in te kunnen schakelen voor een nieuwe virtuele machine, moet u de volgende machtigingen hebben:

- Het maken van een VM in de geselecteerde resourcegroep.
- Het maken van een VM in het geselecteerde virtuele netwerk.
- Schrijven naar storage-account.
- Schrijven naar een beheerde schijf.

U kunt deze taken alleen uitvoeren als aan uw account de ingebouwde rol van Inzender voor virtuele machines is toegewezen. En als u Site Recovery-bewerkingen wilt beheren in een kluis, moet aan uw account ook de ingebouwde rol van Site Recovery-inzender zijn toegewezen.


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

1. Klik in de Azure-portal op **+ Een resource maken** en zoek in de Marketplace naar **Recovery services**.
2. Klik op **Backup and Site Recovery (OMS)** en klik op de pagina Backup and Site Recovery op **Maken**. 
1. Ga naar **Recovery Services-kluis** > **Naam** en voer een beschrijvende naam in voor de kluis. Voor deze reeks zelfstudies gebruiken we **ContosoVMVault**.
2. Selecteer bij **Resourcegroep** een bestaande resourcegroep of maak een nieuwe. Voor deze zelfstudie gebruiken we **contosoRG**.
3. Selecteer bij **Locatie** de regio waarin de kluis zich moet bevinden. In dit voorbeeld gebruiken we **Europa - west**.
4. Voor snelle toegang tot de kluis vanuit het dashboard, selecteert u **Aan dashboard vastmaken** > **Maken**.

   ![Een nieuwe kluis maken](./media/tutorial-prepare-azure/new-vault-settings.png)

   De nieuwe kluis wordt weergegeven in **Dashboard** > **Alle resources** en op de hoofdpagina **Recovery Services-kluizen**.

## <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

Wanneer Azure-VM's zijn gemaakt op basis van beheerde schijven na een failover, worden ze gekoppeld aan dit netwerk.

1. Selecteer in [Azure Portal](https://portal.azure.com) **Een resource maken** > **Netwerken** > **Virtueel netwerk**.
2. Laat **Resource Manager** geselecteerd als het implementatiemodel.
3. Voer bij **Naam** een netwerknaam in. De naam moet uniek zijn binnen de Azure-resourcegroep. In deze zelfstudie gebruiken we **ContosoASRnet**.
4. Geef de resourcegroep op waarin het netwerk wordt gemaakt. We gebruiken de bestaande resourcegroep **contosoRG**.
5. Voer bij **Adresbereik** het bereik in voor het netwerk **10.0.0.0/24**. In dit netwerk gebruiken we geen subnet.
6. Selecteer bij **Abonnement** het abonnement waarin u het netwerk wilt maken.
7. Selecteer bij **Locatie** de optie **Europa - west**. Het netwerk moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
8. We laten de standaardopties staan voor DDoS-basisbescherming zonder service-eindpunt op het netwerk.
9. Klik op **Create**.

   ![Een virtueel netwerk maken](media/tutorial-prepare-azure/create-network.png)

   Het duurt een paar seconden voordat het virtuele netwerk is gemaakt. Nadat dit is gemaakt, wordt het in het Azure Portal-dashboard weergegeven.

## <a name="useful-links"></a>Handige koppelingen

- [Meer informatie](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) over Azure-netwerken.
- [Meer informatie over](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) beheerde schijven.



## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De on-premises VMware-infrastructuur voorbereiden op herstel naar Azure na een noodgeval](tutorial-prepare-on-premises-vmware.md)
