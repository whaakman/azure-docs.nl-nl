---
title: Resources maken voor gebruik met Azure Site Recovery | Microsoft Docs
description: Informatie over het voorbereiden van Azure voor replicatie van lokale apparaten via de Azure Site Recovery-service.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 321e304f-b29e-49e4-aa64-453878490ea7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 2fa7e731a05e19697603058829f130074bb5b522
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Azure-resources voor de replicatie van machines lokale voorbereiden

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel herstel (BCDR) door uw business-apps te houden beschikbaar tijdens geplande en ongeplande uitval. Site Recovery beheert en noodherstel van on-premises machines en virtuele Azure-machines (VM's), met inbegrip van replicatie, failovers en herstel ingedeeld.

Deze zelfstudie laat zien hoe Azure-onderdelen voorbereiden wanneer u een lokale virtuele machines en fysieke servers repliceren naar Azure wilt maken. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleer of dat uw account replicatie machtigingen heeft
> * Een Azure-opslagaccount maken.
> * Een Azure-netwerk instellen. Wanneer virtuele Azure-machines na een failover worden gemaakt, zijn ze gekoppeld aan deze Azure-netwerk.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="verify-account-permissions"></a>Controleer de accountmachtigingen

Als u hebt zojuist hebt gemaakt met uw gratis Azure-account bent u de beheerder van uw abonnement. Als u niet de abonnementsbeheerder bent, samen met de beheerder u moet machtigingen toewijzen. Om replicatie inschakelen voor een nieuwe virtuele machine, moet u het volgende hebben:

- Machtiging voor het maken van een virtuele machine in de geselecteerde resourcegroep
- Machtiging voor het maken van een virtuele machine in de geselecteerde virtuele netwerk
- Machtigingen voor schrijven naar het geselecteerde opslagaccount

De ingebouwde rol 'Virtual Machine Contributor' heeft deze machtingen. U moet ook de machtiging voor het beheren van Azure Site Recovery-bewerkingen. De rol 'Inzender Site Recovery' heeft alle machtigingen nodig voor het beheren van Site Recovery-bewerkingen in een Recovery Services-kluis.

## <a name="create-a-storage-account"></a>Een opslagaccount maken

Installatiekopieën van gerepliceerde machines worden bewaard in Azure-opslag. Virtuele machines in Azure worden uit de opslag gemaakt wanneer u een failover van on-premises naar Azure.

1. In de [Azure-portal](https://portal.azure.com) menu, klikt u op **nieuw** -> **opslag** -> **opslagaccount**.
2. Voer een naam in voor het opslagaccount. Voor deze zelfstudie gebruiken we de naam van de **contosovmsacct1910171607**. De naam moet uniek zijn binnen Azure en tussen 3 en 24 tekens, witn alleen cijfers en kleine letters.
3. Gebruik de **Resource Manager** implementatiemodel.
4. Selecteer **algemeen** > **standaard**.
5. Selecteer de standaard **RA-GRS** voor redundantie van gegevensopslag.
6. Selecteer het abonnement waarin u het nieuwe opslagaccount wilt maken.
7. Geef een nieuwe resourcegroep. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Voor deze zelfstudie gebruiken we de naam van de **ContosoRG**.
8. Selecteer de geografische locatie voor het opslagaccount. Het opslagaccount moet in dezelfde regio bevinden als de Recovery Services-kluis. Voor deze zelfstudie gebruiken we de locatie **West-Europa**.

   ![Maak storageacct](media/tutorial-prepare-azure/create-storageacct.png)

9. Klik op **Maken** om het opslagaccount te maken.

## <a name="create-a-vault"></a>Een kluis maken

1. Klik in het menu van Azure portal, op **nieuw** > **voor bewaking en beheer** >
    **back-up en herstel van de Site**.
2. Geef in **Naam** een beschrijvende naam op om de kluis mee aan te duiden. Voor deze zelfstudie gebruiken we **ContosoVMVault**.
3. Selecteer de bestaande resourcegroep met de naam **contosoRG**.
4. Geef de Azure-regio **West-Europa**.
5. Voor snelle toegang tot de kluis vanuit het dashboard, klikt u op **vastmaken aan dashboard** > **maken**.

   ![Nieuwe kluis](./media/tutorial-prepare-azure/new-vault-settings.png)

   De nieuwe kluis wordt weergegeven in de **Dashboard** > **alle resources**, en klik op de hoofdblade **Recovery Services-kluizen** pagina.

## <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

Wanneer Azure VM's worden gemaakt vanuit opslag na een failover, zijn ze gekoppeld aan dit netwerk.

1. In de [Azure-portal](https://portal.azure.com) menu, klikt u op **nieuw** > **Networking** >
    **virtueel netwerk**
2. Laat **Resource Manager** geselecteerd als het implementatiemodel. Resource Manager is het aanbevolen implementatiemodel.
   - Geef een netwerknaam. De naam moet uniek zijn binnen de Azure-resourcegroep. We gebruiken de naam van de **ContosoASRnet**
   - Gebruik de bestaande resourcegroep **contosoRG**.
   - Geef het adresbereik 10.0.0.0/24 netwerk.
   - We hebben een subnet voor deze zelfstudie.
   - Selecteer het abonnement in te maken van het netwerk.
   - Selecteer de locatie **West-Europa**. Het netwerk moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
3. Klik op **Create**.

   ![netwerk maken](media/tutorial-prepare-azure/create-network.png)

   Het virtuele netwerk duurt een paar seconden te maken. Nadat deze gemaakt, kunt u deze bekijken in de Azure-portaldashboard.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De on-premises VMware-infrastructuur voorbereiden voor herstel na noodgevallen naar Azure](tutorial-prepare-on-premises-vmware.md)
