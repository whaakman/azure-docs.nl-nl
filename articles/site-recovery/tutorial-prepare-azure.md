---
title: Resources maken voor gebruik met Azure Site Recovery | Microsoft Docs
description: Informatie over het voorbereiden van Azure voor replicatie van on-premises machines met behulp van Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 10/10/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 4a92bfc9d682346c874ce91fc9835558402bae84
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078984"
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Azure-resources voorbereiden voor replicatie van lokale machines

 [Azure Site Recovery](site-recovery-overview.md) draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel (BCDR) door te zorgen dat uw zakelijke apps actief blijven tijdens geplande en ongeplande uitval. Site Recovery beheert en orkestreert noodherstel van on-premises machines en virtuele Azure-machines (VM's), met inbegrip van replicatie, failover en herstel.

Dit artikel is de eerste zelfstudie in een reeks waarin u leest hoe u noodherstel kunt instellen voor on-premises VM’s. Dit is van belang bij het beveiligen van on-premises VMware-VM’s, Hyper-V-VM’s of fysieke servers.

In deze zelfstudies ziet u steeds het eenvoudigste implementatiepad voor een scenario. Waar mogelijk wordt gebruikgemaakt van standaardopties en niet alle mogelijke instellingen en paden worden weergegeven. 

Dit artikel laat zien hoe u Azure-onderdelen voorbereidt wanneer u on-premises VM’s (Hyper-V of VMware) of fysieke Windows- of Linux-servers wilt repliceren naar Azure. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleren of uw Azure-account replicatiemachtigingen heeft.
> * Een Azure-opslagaccount maken. Installatiekopieën van gerepliceerde machines worden hierin opgeslagen.
> * Maak een Recovery Services-kluis. Een kluis bevat metagegevens en configuratiegegevens voor VM’s, en andere replicatieonderdelen.
> * Stel een Azure-netwerk in. Wanneer de Azure VM's zijn gemaakt na de failover, worden ze gekoppeld aan dit Azure-netwerk.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij [Azure Portal](http://portal.azure.com).

## <a name="verify-account-permissions"></a>Accountmachtigingen controleren

Als u net pas uw gratis Azure-account hebt gemaakt, bent u de beheerder van uw abonnement. Als u niet de abonnementsbeheerder bent, neemt u contact op met de beheerder om de machtigingen te krijgen die u nodig hebt. Om replicatie in te kunnen schakelen voor een nieuwe virtuele machine, moet u de volgende machtigingen hebben:

- Het maken van een VM in de geselecteerde resourcegroep.
- Het maken van een VM in het geselecteerde virtuele netwerk.
- Schrijven naar het geselecteerde opslagaccount.

U kunt deze taken alleen uitvoeren als aan uw account de ingebouwde rol van Inzender voor virtuele machines is toegewezen. En als u Site Recovery-bewerkingen wilt beheren in een kluis, moet aan uw account ook de ingebouwde rol van Site Recovery-inzender zijn toegewezen.

## <a name="create-a-storage-account"></a>Create a storage account

Installatiekopieën van gerepliceerde machines worden bewaard in Azure Storage. Azure VM's worden gemaakt vanuit de opslag wanneer u een failover van on-premises naar Azure uitvoert. Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis. In deze zelfstudie gebruiken we Europa - west.

1. Selecteer in het menu van [Azure Portal](https://portal.azure.com) achtereenvolgens **Een resource maken** > **Opslag** > **Opslagaccount - blob, bestand, tabel, wachtrij**.
2. Voer in **Opslagaccount maken** een naam voor het account in. Voor deze zelfstudies gebruiken we **contosovmsacct1910171607**. De naam die u selecteert moet uniek zijn in Azure, tussen de 3 en 24 tekens lang zijn en mag alleen cijfers en kleine letters bevatten.
3. Selecteer bij **implementatiemodel** **Resource Manager**.
4. Selecteer bij **Soort account** de optie **Opslag (algemeen gebruik v1)**. Selecteer niet blob-opslag.
5. Selecteer bij **Replicatie** de standaardoptie **Geografisch redundante opslag met leestoegang** voor opslagredundantie. **Veilige overdracht vereist** laten we **Uitgeschakeld**.
6. Selecteer in **Prestaties** **Standaard** en in **Toegangslaag** de standaardoptie **Dynamisch**.
7. Selecteer bij **Abonnement** het abonnement waarin u het nieuwe opslagaccount wilt maken.
8. Geef bij **Resourcegroep** een nieuwe resourcegroep op. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Voor deze zelfstudies maken gebruiken we **ContosoRG**.
9. Selecteer bij **Locatie** de geografische locatie voor het opslagaccount. 

   ![Create a storage account](media/tutorial-prepare-azure/create-storageacct.png)

9. Selecteer **Maken** om het opslagaccount te maken.

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

1. Selecteer in Azure Portal achtereenvolgens **Een resource maken** > **Opslag** > **Back-up en siteherstel (OMS)**.
2. Voer in **Naam** een beschrijvende naam in om de kluis aan te duiden. Voor deze reeks zelfstudies gebruiken we **ContosoVMVault**.
3. Voor **Resourcegroep** gebruiken we **contosoRG**.
4. Voor **Locatie**. gebruiken we **Europa - west**.
5. Voor snelle toegang tot de kluis vanuit het dashboard, selecteert u **Aan dashboard vastmaken** > **Maken**.

   ![Een nieuwe kluis maken](./media/tutorial-prepare-azure/new-vault-settings.png)

   De nieuwe kluis wordt weergegeven in **Dashboard** > **Alle resources** en op de hoofdpagina **Recovery Services-kluizen**.

## <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

Wanneer de Azure VM's zijn gemaakt vanuit de opslag na de failover, worden ze gekoppeld aan dit netwerk.

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
- [Meer informatie](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts) over typen Azure-opslag.
- [Meer informatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs#read-access-geo-redundant-storage) over opslagredundantie en [veilige overdracht](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) voor opslag.



## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De on-premises VMware-infrastructuur voorbereiden op herstel naar Azure na een noodgeval](tutorial-prepare-on-premises-vmware.md)
