---
title: Instellen van de bron en doel voor Hyper-V-replicatie naar Azure (zonder de System Center VMM) met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van de stappen voor het instellen van de bron en doel-instellingen voor de replicatie van Hyper-V-machines naar Azure storage met Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d2010d85-77fd-4dea-84f3-1c960ed4c63f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: b38eb3a011d46f2239891ea1d1bcac2a4059a866
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="step-8-set-up-the-source-and-target-for-hyper-v-replication-to-azure"></a>Stap 8: Instellen van de bron en doel voor Hyper-V-replicatie naar Azure

Dit artikel wordt beschreven hoe u de bron en doel-instellingen configureren wanneer repliceren lokale Hyper-V virtuele machines (zonder de System Center VMM) naar Azure met behulp van de [Azure Site Recovery](site-recovery-overview.md) service in de Azure portal.

Opmerkingen en vragen plaatsen onder aan dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

De Hyper-V-site instellen, de Azure Site Recovery Provider en de Azure Recovery Services-agent installeren op Hyper-V-hosts en de site in de kluis registreren.

1. In **infrastructuur voorbereiden**, klikt u op **bron**. Een nieuwe Hyper-V-site toevoegen als een container voor uw Hyper-V-hosts of clusters: klik op **+ Hyper-V-Site**.

    ![Bron instellen](./media/hyper-v-site-walkthrough-source-target/set-source1.png)
2. In **maken van Hyper-V-site**, Geef een naam voor de site. Klik vervolgens op **OK**. Selecteer de site hebt gemaakt Klik op nu **+ Hyper-V Server** een server toevoegen aan de site.

    ![Bron instellen](./media/hyper-v-site-walkthrough-source-target/set-source2.png)

3. In **Server toevoegen** > **servertype**, controleert u of **Hyper-V-server** wordt weergegeven.

    - Zorg ervoor dat de Hyper-V-server die u wilt toevoegen, voldoet aan de [vereisten](#on-premises-prerequisites), en kan worden gebruikt voor toegang tot de opgegeven URL's.
    - Download het installatieprogramma voor de Azure Site Recovery-provider. U uitvoeren dit bestand voor het installeren van de Provider en de Recovery Services agent op elke Hyper-V-host.

    ![Bron instellen](./media/hyper-v-site-walkthrough-source-target/set-source3.png)


## <a name="install-the-provider-and-agent"></a>De Provider en de agent installeren

1. Voer de Provider-installatiebestand op elke host die u hebt toegevoegd aan de Hyper-V-site. Als u op een Hyper-V-cluster installeert, moet u setup uitvoeren op elk clusterknooppunt. Installeren en registreren van elk clusterknooppunt Hyper-V zorgt ervoor dat virtuele machines worden beschermd, zelfs als ze via knooppunten migreren.
2. In **Microsoft Update** kunt u updates inschakelen, zodat de providerupdates volgens uw Microsoft Update-beleid worden geïnstalleerd.
3. Accepteer of wijzig in **Installatie** de standaardinstallatielocatie van de provider en klik op **Installeren**.
4. In **Kluisinstellingen**, klikt u op **Bladeren** selecteren van het kluissleutelbestand dat u hebt gedownload. Geef het Azure Site Recovery-abonnement, de kluisnaam van de en de Hyper-V-site waarop de Hyper-V-server behoort.

    ![Serverregistratie](./media/hyper-v-site-walkthrough-source-target/provider3.png)

5. In **Proxy-instellingen**, opgeven hoe de Provider die wordt uitgevoerd op Hyper-V-hosts verbinding maakt met Azure Site Recovery via internet.

    * Als u wilt dat de provider rechtstreeks verbinding maakt, selecteert u **Rechtstreeks verbinding maken met Azure Site Recovery zonder proxyserver**.
    * Als uw bestaande proxy verificatie vereist, of u wilt een aangepaste proxy gebruikt voor de providerverbinding, selecteer **verbinding maken met Azure Site Recovery via een proxyserver**.
    * Als u een proxy gebruikt:
        - Het adres, poort en referenties opgeven
        - Zorg ervoor dat de URL's die worden beschreven de [vereisten](#prerequisites) zijn toegestaan via de proxy.

    ![internet](./media/hyper-v-site-walkthrough-source-target/provider7.png)

6. Nadat de installatie is voltooid, klikt u op **registreren** de server in de kluis registreren.

    ![Installatielocatie](./media/hyper-v-site-walkthrough-source-target/provider2.png)

7. Nadat de registratie is voltooid, metagegevens van de Hyper-V-server worden opgehaald door Azure Site Recovery en de server wordt weergegeven in **Site Recovery-infrastructuur** > **Hyper-V-Hosts**.


## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Geef de Azure storage-account voor replicatie en het Azure-netwerk waarmee virtuele Azure-machines verbinding maken na een failover.

1. Klik op **infrastructuur voorbereiden** > **doel**.
2. Selecteer het abonnement en de resourcegroep waarin u wilt maken van de Azure VM's na een failover. Kies het implementatiemodel dat u wilt gebruiken in Azure (klassiek of de resource management) voor de virtuele machines.

3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.

    - Als u geen storage-account hebt, klikt u op **en opslag** voor het maken van een inline-account op basis van een Resource Manager. 
    - Als u geen Azure-netwerk hebt, klikt u op **+ netwerk** voor het maken van een inline Resource Manager-netwerk.






## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 9: een replicatiebeleid instellen](hyper-v-site-walkthrough-replication.md)
