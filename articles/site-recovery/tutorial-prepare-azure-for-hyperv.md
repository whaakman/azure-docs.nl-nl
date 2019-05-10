---
title: Azure-resources voorbereiden op herstel na noodgevallen van on-premises machines
description: Meer informatie over het voorbereiden van Azure voor herstel na noodgevallen van on-premises Hyper-V-machines met behulp van Azure Site Recovery
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 29189a5919a01fcb897758fb64ca9e84b9381fb6
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410907"
---
# <a name="prepare-azure-resources-for-disaster-recovery-of-on-premises-machines"></a>Azure-resources voorbereiden op herstel na noodgevallen van on-premises machines

 [Azure Site Recovery](site-recovery-overview.md) helpt zakelijke continuïteit en herstel na noodgevallen (BCDR) door te houden tijdens geplande en ongeplande uitval business-apps. Site Recovery beheert en orkestreert noodherstel van on-premises machines en virtuele Azure-machines (VM's), met inbegrip van replicatie, failover en herstel.

In deze zelfstudie is de eerste in een serie die wordt beschreven hoe u herstel na noodgevallen instellen voor on-premises Hyper-V-machines.

> [!NOTE]
> Zelfstudies om weer te geven van de meest eenvoudige implementatie pad voor een scenario voor het ontwerp. Deze zelfstudies Gebruik standaardopties indien mogelijk, en alle mogelijke instellingen en paden niet weergeven. Zie de sectie 'How To' voor elke bijbehorende scenario voor meer informatie.

Deze zelfstudie leert u hoe u Azure-onderdelen voorbereidt wanneer u wilt repliceren van on-premises virtuele machines (Hyper-V) naar Azure. U leert het volgende:

> [!div class="checklist"]
> * Controleren of uw Azure-account replicatiemachtigingen heeft.
> * Maak een Azure storage-account, die installatiekopieën van gerepliceerde machines worden opgeslagen.
> * Maak een Recovery Services-kluis waarin metagegevens en configuratie-informatie voor VM's en andere replicatieonderdelen worden opgeslagen.
> * Stel een Azure-netwerk in. Wanneer Azure-VM's zijn gemaakt na een failover, worden ze gekoppeld aan dit netwerk.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.

## <a name="sign-in"></a>Aanmelden

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="verify-account-permissions"></a>Accountmachtigingen controleren

Als u een gratis Azure-account hebt gemaakt, u kunt de beheerder voor dat abonnement. Als u niet de beheerder bent, werkt u met de beheerder om toe te wijzen de machtigingen die u nodig hebt. Om replicatie in te kunnen schakelen voor een nieuwe virtuele machine, moet u de volgende machtigingen hebben:

- Het maken van een VM in de geselecteerde resourcegroep.
- Het maken van een VM in het geselecteerde virtuele netwerk.
- Schrijven naar het geselecteerde opslagaccount.

Als u wilt deze taken hebt voltooid, moet uw account de ingebouwde rol van inzender voor virtuele machines worden toegewezen. Voor het beheren van Site Recovery-bewerkingen in een kluis, moet uw account de ingebouwde rol van Site Recovery-Inzender worden toegewezen.

## <a name="create-a-storage-account"></a>Create a storage account

Installatiekopieën van gerepliceerde machines worden bewaard in Azure Storage. Azure VM's worden gemaakt vanuit de opslag wanneer u een failover van on-premises naar Azure uitvoert. Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis.

1. In de [Azure-portal](https://portal.azure.com) in het menu **een resource maken** > **opslag** > **opslagaccount - blob, bestand, tabel, wachtrij** .
2. Voer in **Opslagaccount maken** een naam voor de account in.  De naam die u kiest moet uniek zijn binnen Azure, worden van 3 tot 24 tekens lang zijn, en alleen kleine letters en cijfers gebruiken. Voor deze zelfstudie gebruiken we **contosovmsacct1910171607**.
3. Selecteer bij **implementatiemodel** **Resource Manager**.
4. In **soort Account**, selecteer **Storage (algemeen gebruik v1)**. Selecteer niet blob-opslag.
5. Selecteer bij **Replicatie** de standaardoptie **Geografisch redundante opslag met leestoegang** voor opslagredundantie. Laat de veilige overdracht vereist instellen als uitgeschakeld.
6. Selecteer bij **Prestaties** **Standard**. Vervolgens in **toegangslaag**, selecteert u de standaardoptie **warm**.
7. In **abonnement**, kiest u het abonnement waarin u wilt maken van het nieuwe opslagaccount.
8. Geef bij **Resourcegroep** een nieuwe resourcegroep op. Een Azure-resourcegroep is een logische container waarin Azure resources worden geïmplementeerd en beheerd. Voor deze zelfstudie gebruiken we **ContosoRG**.
9. In **locatie**, kiest u de geografische locatie voor uw storage-account. Voor deze zelfstudie gebruiken we **West-Europa**.
10. Selecteer **Maken** om het opslagaccount te maken.

   ![Create a storage account](media/tutorial-prepare-azure/create-storageacct.png)

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

1. Selecteer in de Azure portal, **+ een resource maken**, en zoek vervolgens de Azure Marketplace naar Recovery Services.
2. Selecteer **Backup en Site Recovery (OMS)**. Volgende op de **back-up en siteherstel** weergeeft, schakelt **maken**.
1. In **Recovery services-kluis > naam**, voer een beschrijvende naam voor het identificeren van de kluis. Gebruik voor deze zelfstudie **ContosoVMVault**.
2. Selecteer bij **Resourcegroep** een bestaande resourcegroep of maak een nieuwe. Voor deze zelfstudie gebruiken we **contosoRG**.
3. In **locatie**, selecteer de regio waarin de kluis moet zich bevinden. Voor deze zelfstudie gebruiken we **West-Europa**.
4. Voor snelle toegang tot de kluis vanuit het dashboard, selecteert u **Aan dashboard vastmaken** > **Maken**.

![Een nieuwe kluis maken](./media/tutorial-prepare-azure/new-vault-settings.png)

De nieuwe kluis wordt weergegeven in **Dashboard** > **Alle resources** en op de hoofdpagina **Recovery Services-kluizen**.

## <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

Wanneer de Azure VM's zijn gemaakt vanuit de opslag na de failover, worden ze gekoppeld aan dit netwerk.

1. Selecteer in [Azure Portal](https://portal.azure.com) **Een resource maken** > **Netwerken** > **Virtueel netwerk**. Laat u Resource Manager is geselecteerd als het implementatiemodel.
2. Voer bij **Naam** een netwerknaam in. De naam moet uniek zijn binnen de Azure-resourcegroep. Voor deze zelfstudie gebruiken we **ContosoASRnet**.
3. Geef de resourcegroep waarin u wilt maken van het netwerk. Voor deze zelfstudie gebruikt u de bestaande resourcegroep **contosoRG**.
4. In **adresbereik**, voer **10.0.0.0/24** als het bereik voor het netwerk. Er is geen subnet voor dit netwerk.
5. Selecteer bij **Abonnement** het abonnement waarin u het netwerk wilt maken.
6. In **locatie**, kiest u **West-Europa**. Het netwerk moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
7. Laat de standaardopties van basic DDoS protection, met geen service-eindpunt op het netwerk.
8. Selecteer **Maken**.

![Een virtueel netwerk maken](media/tutorial-prepare-azure/create-network.png)

Het duurt een paar seconden voordat het virtuele netwerk is gemaakt. Nadat deze gemaakt, ziet u deze in de Azure portal-dashboard.

## <a name="useful-links"></a>Handige koppelingen

Meer informatie over:
- [Azure-netwerken](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)
- [Beheerde schijven](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)



## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De on-premises Hyper-V-infrastructuur voorbereiden op herstel na noodgevallen naar Azure](hyper-v-prepare-on-premises-tutorial.md)
