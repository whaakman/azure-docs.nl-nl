---
title: Hyper-V-machines in VMM met SAN repliceren met behulp van Azure Site Recovery | Microsoft Docs
description: Dit artikel wordt beschreven hoe u Hyper-V virtuele machines repliceren tussen twee sites met Azure Site Recovery SAN-replicatie.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: eb480459-04d0-4c57-96c6-9b0829e96d65
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: 3df38802fcdc86e4553253d38c49faff455f873e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-vms-in-vmm-clouds-to-a-secondary-site-with-azure-site-recovery-by-using-san"></a>Hyper-V-machines in VMM-clouds repliceren naar een secundaire site met Azure Site Recovery met behulp van SAN


Gebruik dit artikel als u wilt implementeren [Azure Site Recovery](site-recovery-overview.md) voor het beheren van replicatie van Hyper-V-machines (beheerd in System Center Virtual Machine Manager-clouds) naar een secundaire VMM-site met Azure Site Recovery in de klassieke portal. Dit scenario is niet beschikbaar in de nieuwe Azure portal.



Na de eventuele opmerkingen aan het einde van dit artikel. Vind antwoorden op technische vragen in de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="why-replicate-with-san-and-site-recovery"></a>Waarom repliceren met SAN- en Site Recovery?

* SAN biedt een replicatieoplossing op bedrijfsniveau, schaalbare, zodat een primaire site met Hyper-V met SAN LUN's naar een secundaire site met SAN repliceren kunt. Opslag wordt beheerd door VMM en replicatie en failover wordt beheerd met Site Recovery.
* Site Recovery heeft samengewerkt met verschillende [SAN-opslag partners](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) om te voorzien in replicatie Fibre Channel en iSCSI-opslag.  
* Gebruik uw bestaande SAN-infrastructuur ter bescherming van essentiële apps die worden geïmplementeerd in Hyper-V-clusters. Virtuele machines kunnen worden gerepliceerd als een groep, zodat u apps met N-aantal lagen failover mogelijk consistent.
* SAN-replicatie zorgt voor replicatieconsistentie in verschillende lagen van een toepassing met gesynchroniseerde replicatie voor de laag RTO en RPO en asynchrone uitvoering replicatie voor hoge flexibiliteit (afhankelijk van de mogelijkheden van uw opslagmatrix).  
* U kunt SAN-opslag in de VMM-fabric beheren en gebruiken van SMI-S in VMM om bestaande opslag te detecteren.  

Opmerking:
- Replicatie van site-naar-site met SAN is niet beschikbaar in de Azure-portal. De optie is alleen beschikbaar in de klassieke portal. Nieuwe kluizen kunnen niet worden gemaakt in de klassieke portal. Bestaande kluizen kunnen worden beheerd.
- Replicatie van SAN naar Azure wordt niet ondersteund.
- U kunt geen gedeelde vhdx-bestanden of logische eenheden (LUN's) die rechtstreeks zijn verbonden met virtuele machines via iSCSI of Fibre Channel repliceren. Gastclusters kunnen worden gerepliceerd.


## <a name="architecture"></a>Architectuur

![SAN-architectuur](./media/site-recovery-vmm-san/architecture.png)

- **Azure**: instellen van een Site Recovery-kluis in de Azure portal.
- **SAN-opslag**: SAN-opslag in de VMM-fabric wordt beheerd. U voegt u de opslagprovider toe, opslagcategorieën aanmaken en instellen van de opslaggroepen.
- **VMM en Hyper-V**: We raden aan een VMM-server op elke site. Instellen van VMM-privéclouds en Hyper-V-clusters in die clouds plaatsen. Tijdens de implementatie van de Azure Site Recovery Provider is geïnstalleerd op elke VMM-server en de server is geregistreerd in de kluis. De Provider communiceert met de Site Recovery-service voor het beheren van replicatie, failover en failback.
- **Replicatie**: nadat u opslag in VMM installeren en configureren van replicatie in Site Recovery, replicatie tussen de primaire en secundaire SAN-opslag. Er zijn geen replicatiegegevens verzonden naar Site Recovery.
- **Failover**: schakelen van failover in de Site Recovery-portal. Er is geen gegevens verloren gaan tijdens de failover omdat replicatie synchroon is.
- **Failback**: als u wilt een failback uit, schakelt u de omgekeerde replicatie deltawijzigingen overzetten van de secundaire site naar de primaire site. Als omgekeerde replicatie voltooid is, kunt u een geplande failover uitvoeren vanaf de secundaire op primaire. Deze geplande failover wordt de replica virtuele machines op de secundaire site gestopt en start ze op de primaire site.


## <a name="before-you-start"></a>Voordat u begint


**Vereisten** | **Details**
--- | ---
**Azure** |U hebt een [Microsoft Azure](https://azure.microsoft.com/)-account nodig. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over prijzen voor Site Recovery. Maak een Azure Site Recovery-kluis configureren en beheren van replicatie en failover.
**VMM** | U kunt gebruiken één VMM-server en andere clouds repliceren, maar we raden aan één VMM in de primaire site en één in de secundaire site. VMM kan worden geïmplementeerd als een fysieke of virtuele zelfstandige server of als een cluster. <br/><br/>De VMM-server moet worden uitgevoerd op System Center 2012 R2 of hoger met de meest recente cumulatieve updates.<br/><br/> U moet ten minste één cloud zijn geconfigureerd op de primaire VMM-server die u wilt beveiligen en één cloud zijn geconfigureerd op de secundaire VMM-server die u wilt gebruiken voor failover.<br/><br/> De broncloud moet een of meer VMM-hostgroepen bevatten.<br/><br/> Alle VMM-clouds moeten het Hyper-V-Capaciteitsprofiel ingesteld hebben.<br/><br/> Zie voor meer informatie over het instellen van VMM-clouds [implementeren van een privécloud VM](https://technet.microsoft.com/en-us/system-center-docs/vmm/scenario/cloud-overview).
**Hyper-V** | U moet een of meer Hyper-V-clusters in primaire en secundaire VMM-clouds.<br/><br/> Het bron-Hyper-V-cluster moet een of meer virtuele machines bevatten.<br/><br/> De VMM-hostgroepen in de primaire en secundaire sites moeten ten minste één van de Hyper-V-clusters bevatten.<br/><br/>De servers van de Hyper-V-host en het doel moeten worden uitgevoerd WindowsServer 2012 of hoger met de Hyper-V-rol en de meest recente updates zijn geïnstalleerd.<br/><br/> Als u Hyper-V in een cluster uitvoert en een statische IP-adressen gebaseerde cluster hebt, zijn de clusterbroker niet automatisch gemaakt. U moet deze handmatig configureren. Zie voor meer informatie [hostclusters voor Hyper-V replica voorbereiden](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters).
**SAN-opslag** | U kunt guest geclusterde virtuele machines met iSCSI of channel-opslag of met behulp van gedeelde virtuele harde schijven (vhdx) repliceren.<br/><br/> U moet twee SAN-matrices: één in de primaire site en één in de secundaire site.<br/><br/> Een netwerkinfrastructuur moet worden ingesteld tussen de matrices. Peering en replicatie moeten worden geconfigureerd. Replicatie-licenties moeten worden ingesteld in overeenstemming met de opslagvereisten voor de matrix.<br/><br/>Instellen van het netwerk tussen de Hyper-V-host-servers en de opslagmatrix zodat hosts met opslag-LUN's communiceren kunnen met behulp van iSCSI of Fibre Channel.<br/><br/> Controleer [ondersteunde opslagmatrices](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx).<br/><br/> SMI-S-providers van fabrikanten van opslag-matrix moeten worden geïnstalleerd en de SAN-matrices moeten worden beheerd door de provider. Instellen van de Provider volgens de instructies van de fabrikant.<br/><br/>Zorg ervoor dat de SMI-S-provider van de matrix is op een server waarop de VMM-server toegankelijk via het netwerk met een IP-adres of FQDN-naam.<br/><br/> Elke SAN-matrix moet een of meer beschikbare opslaggroepen hebben.<br/><br/> De primaire VMM-server de primaire matrix moet beheren en de secundaire VMM-server moet de secundaire matrix beheren.
**Netwerktoewijzing** | Stel netwerktoewijzing zodat gerepliceerde virtuele machines optimaal op secundaire Hyper-V-hostservers zijn geplaatst na een failover en zodat ze zijn verbonden met de juiste VM-netwerken. Als u geen netwerktoewijzing configureert, replica VMs niet verbonden met een netwerk na een failover.<br/><br/> Zorg ervoor dat VMM netwerken correct zijn geconfigureerd zodat u netwerktoewijzing tijdens de implementatie van Site Recovery kunt instellen. In VMM, moeten de virtuele machines op de bronhost Hyper-V zijn verbonden met een VMM VM-netwerk. Dit netwerk moet zijn gekoppeld aan een logisch netwerk dat is verbonden met de cloud.<br/><br/> De doelcloud moet een bijbehorende VM-netwerk hebt en deze op zijn beurt moet zijn gekoppeld aan een corresponderende logisch netwerk dat is gekoppeld aan de doelcloud.<br/><br/>.

## <a name="step-1-prepare-the-vmm-infrastructure"></a>Stap 1: De VMM-infrastructuur voorbereiden
Als u met het voorbereiden van uw VMM-infrastructuur, moet u:

1. Controleer of de VMM-clouds.
2. Integreren en classificeren van SAN-opslag in VMM.
3. LUN's maken en toewijzen van opslag.
4. Replicatiegroepen maken.
5. Stel VM-netwerken.

### <a name="verify-vmm-clouds"></a>Controleer of de VMM-clouds

Zorg ervoor dat de VMM-clouds correct zijn ingesteld voordat u begint met implementatie van Site Recovery.

### <a name="integrate-and-classify-san-storage-in-the-vmm-fabric"></a>Integreren en classificeren van SAN-opslag in de VMM-fabric

1. Ga in de VMM-console naar **Fabric** > **opslag** > **Resources toevoegen** > **opslagapparaten**.
2. In de **opslagapparaten toevoegen** wizard selecteert u **Selecteer een type opslagprovider** en selecteer **SAN- en NAS-apparaten gedetecteerd en beheerd door een SMI-S-provider**.

    ![Type provider](./media/site-recovery-vmm-san/provider-type.png)

3. Op de **Geef Protocol en adres op van de SMI-S-opslagprovider** pagina **SMI-S CIMXML** en geef de instellingen voor het verbinden met de provider.
4. In **Provider IP-adres of FQDN-naam** en **TCP/IP-poort**, geef de instellingen voor het verbinden met de provider. U kunt alleen een SSL-verbinding voor de SMI-S CIMXML gebruiken.

    ![Provider verbinding maken](./media/site-recovery-vmm-san/connect-settings.png)
5. In **Run as-account**, VMM uitvoeren als-account die u kunt toegang tot de provider of maak een account opgeven.
6. Op de **informatie verzamelen** pagina VMM probeert automatisch te detecteren en importeren van de informatie over opslagapparaten. Om te proberen detectie, klikt u op **Provider scannen**. Als het detectieproces is geslaagd, de gedetecteerde opslagmatrices, opslaggroepen, de fabrikant, model en capaciteit staan op de pagina.

    ![Opslag detecteren](./media/site-recovery-vmm-san/discover.png)
7. In **Selecteer opslaggroepen onder beheer te plaatsen en het toewijzen van een classificatie**, selecteer de opslaggroepen VMM beheerd en ze een classificatie toewijzen. LUN-gegevens worden geïmporteerd vanaf de opslaggroepen. LUN's op basis van de toepassingen die u wilt beveiligen, hun capaciteitsvereisten en uw vereisten voor wat er moet repliceren samen maken.

    ![Opslag classificeren](./media/site-recovery-vmm-san/classify.png)

### <a name="create-luns-and-allocate-storage"></a>LUN's maken en toewijzen van opslag

LUN's op basis van de toepassingen die u wilt beveiligen, capaciteitsvereisten en uw vereisten voor wat er moet repliceren samen maken.

1. Nadat de opslag wordt weergegeven in de VMM-fabric, kunt u [inrichten van LUN's](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-storage-host-groups#create-a-lun-in-vmm).

     > [!NOTE]
     > Voeg de virtuele harde schijven voor de virtuele machine die zijn ingeschakeld voor replicatie met LUN's niet. Als deze LUN's niet in een replicatiegroep voor de Site Recovery, won't ze worden gedetecteerd door Site Recovery.
     >

2. Opslagcapaciteit voor het Hyper-V-hostcluster toewijzen zodat VMM kan de gegevens van virtuele machines naar de ingerichte opslag implementeren kunt:

   * Voordat het toewijzen van opslag aan het cluster, moet u deze toewijzen aan de VMM-hostgroep waarin het cluster zich bevindt. Zie voor meer informatie [hoe logische eenheden voor opslag toewijzen aan een hostgroep in VMM](https://technet.microsoft.com/library/gg610686.aspx) en [hoe opslaggroepen toewijzen aan een hostgroep in VMM](https://technet.microsoft.com/library/gg610635.aspx).
   * Toewijzen van opslagcapaciteit aan het cluster, zoals beschreven in [opslag configureren op een Hyper-V-hostcluster in VMM](https://technet.microsoft.com/library/gg610692.aspx).

    ![Type provider](./media/site-recovery-vmm-san/provider-type.png)
3. In **Geef Protocol en adres op van de SMI-S-opslagprovider**, selecteer **SMI-S CIMXML**. Geef de instellingen voor het verbinden met de provider. Alleen voor de SMI-S CIMXML kunt u een SSL-verbinding.

    ![Provider verbinding maken](./media/site-recovery-vmm-san/connect-settings.png)
4. In **Run as-account**, VMM uitvoeren als-account die u kunt toegang tot de provider of maak een account opgeven.
5. In **informatie verzamelen**, VMM probeert automatisch te detecteren en importeren van de informatie over opslagapparaten. Als u opnieuw proberen moet, klikt u op **Provider scannen**. Als het detectieproces is geslaagd, de opslagmatrices, opslaggroepen, worden de fabrikant, model en capaciteit weergegeven op de pagina.

    ![Opslag detecteren](./media/site-recovery-vmm-san/discover.png)
7. In **Selecteer opslaggroepen onder beheer te plaatsen en het toewijzen van een classificatie**, selecteer de opslaggroepen dat VMM wordt beheren en hieraan een classificatie. LUN-gegevens worden geïmporteerd vanaf de opslaggroepen.

    ![Opslag classificeren](./media/site-recovery-vmm-san/classify.png)


### <a name="create-replication-groups"></a>Replicatiegroepen maken

Maak een replicatiegroep met alle LUN's die moet worden gerepliceerd samen.

1. Open in de VMM-console de **replicatiegroepen** tabblad van de eigenschappen van de opslagmatrix en klik vervolgens op **nieuw**.
2. Maken van de replicatiegroep.

    ![Groep voor SAN-replicatie](./media/site-recovery-vmm-san/rep-group.png)

### <a name="set-up-networks"></a>Stel netwerken

Als u netwerktoewijzing configureren wilt, het volgende doen:

1. Site Recovery netwerktoewijzing zien.
2. Bereid VM-netwerken in VMM voor:

   * [Stel logische netwerken in](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-logical-networks).
   * [Stel VM-netwerken in](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-vm-networks).


## <a name="step-2-create-a-vault"></a>Stap 2: Een kluis maken

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) van de VMM-server die u wilt registreren in de kluis.
2. Vouw **Data Services** > **Recovery Services**, en klik vervolgens op **Site Recovery-kluis**.
3. Klik op **Nieuw maken** > **Snelle invoer**.
4. Voer in **Naam** een beschrijvende naam in om de kluis aan te duiden.
5. Selecteer in **Regio** de geografische regio voor de kluis. Ondersteunde regio's, Zie [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klik op **Kluis maken**.

    ![Nieuwe kluis](./media/site-recovery-vmm-san/create-vault.png)

Controleer de statusbalk om te bevestigen dat de kluis is gemaakt. De kluis wordt weergegeven als **Active** op de hoofdblade **Recovery Services** pagina.

### <a name="register-the-vmm-servers"></a>De VMM-servers registreren

1. Open de **Quick Start** pagina van de **Recovery Services** pagina. Snel starten kan ook op elk gewenst moment worden geopend door het kiezen van het pictogram.

    ![Pictogram Snel starten](./media/site-recovery-vmm-san/quick-start-icon.png)
2. Selecteer in de vervolgkeuzelijst **tussen Hyper-V lokale site met behulp van replicatie van de matrix**.

    ![Registratiesleutel](./media/site-recovery-vmm-san/select-san.png)
3. In **VMM-servers voorbereiden**, download de nieuwste versie van het Azure Site Recovery Provider-installatiebestand.
4. Voer dit bestand op de bron-VMM-server uit. Als VMM in een cluster is geïmplementeerd en u de Provider voor het eerst installeert, de Provider installeren op een actief knooppunt- en eindtijd van de installatie om de VMM-server in de kluis registreren. Installeer Provider vervolgens op de andere knooppunten. Als u Provider bijwerkt, moet u de upgrade op alle knooppunten, zodat ze dezelfde versie van Provider hebben.
5. Het installatieprogramma controleert de vereisten en aanvragen machtiging voor het stoppen van de VMM-service om te beginnen met de installatie van de Provider. De service wordt opnieuw opgestart nadat setup is voltooid. Op een cluster VMM u gevraagd om te stoppen van de Cluster-rol.
6. In **Microsoft Update**, kunt u zich aanmelden voor updates en updates van de Provider worden geïnstalleerd volgens uw beleid voor Microsoft Update.

    ![Microsoft-updates](./media/site-recovery-vmm-san/ms-update.png)

7. De locatie is geïnstalleerd voor de Provider is standaard <SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin. Klik op **installeren** om te beginnen.

    ![Installatielocatie](./media/site-recovery-vmm-san/install-location.png)

8. Nadat de Provider is geïnstalleerd, klikt u op **registreren** de VMM-server registreren in de kluis.

    ![De installatie is voltooid](./media/site-recovery-vmm-san/install-complete.png)

9. In **internetverbinding**, opgeven hoe de Provider verbinding maakt met Internet. Selecteer **standaardsysteemproxyinstellingen gebruiken** als u wilt de standaardinstellingen van de Internet-verbindingen op de server gebruiken.

    ![Instellingen voor internet](./media/site-recovery-vmm-san/proxy-details.png)

   * Als u een aangepaste proxy gebruikt wilt, deze instellen voordat u de Provider installeren. Wanneer u aangepaste proxyinstellingen configureert, wordt een test uitgevoerd om te controleren van de proxyverbinding.
   * Als u een aangepaste proxy gebruikt, of als uw standaardproxy verificatie vereist, moet u de proxy-gegevens, inclusief het adres en poort.
   * De vereiste URL's moeten toegankelijk is vanaf de VMM-server.
   * Als u een aangepaste proxy gebruikt, wordt automatisch een VMM uitvoeren als-account (DRAProxyAccount) gemaakt met behulp van de opgegeven proxyreferenties. De proxyserver zodanig configureren dat dit account kan worden geverifieerd. U kunt de instellingen van het Run As-account in de VMM-console wijzigen (**instellingen** > **beveiliging** > **Run As-Accounts**  >   **DRAProxyAccount**). De VMM-service voor de wijziging door te voeren, moet u opnieuw.
10. In **registratiesleutel**, selecteer de sleutel die u hebt gedownload vanuit de portal en gekopieerd naar de VMM-server.
11. Controleer in **Kluisnaam** de naam van de kluis waarin de server wordt geregistreerd.

    ![Serverregistratie](./media/site-recovery-vmm-san/vault-creds.png)
12. De versleutelingsinstelling wordt alleen gebruikt voor VMM naar Azure replicatie. U kunt deze negeren.

    ![Serverregistratie](./media/site-recovery-vmm-san/encrypt.png)
13. Geef in **Servernaam** een beschrijvende naam op om de VMM-server in de kluis te identificeren. Geef in een clusterconfiguratie de VMM-clusterrolnaam op.
14. In **initiële synchronisatie van metagegevens van cloud**Selecteer of u metagegevens wilt synchroniseren voor alle clouds op de VMM-server. Deze actie hoeft op elke server slechts één keer te worden uitgevoerd. Als u niet alle clouds wilt synchroniseren, laat u deze instelling uitgeschakeld en synchroniseert u elke cloud afzonderlijk in de cloudeigenschappen in de VMM-console.

    ![Serverregistratie](./media/site-recovery-vmm-san/friendly-name.png)

15. Klik op **Volgende** om het proces te voltooien. Na de registratie worden door Azure Site Recovery metagegevens van de VMM-server opgehaald. De server wordt weergegeven in **Servers** > **VMM-Servers** in de kluis.

### <a name="command-line-installation"></a>Installatie vanaf de opdrachtregel

De Azure Site Recovery Provider kan ook worden geïnstalleerd met behulp van de volgende opdrachtregel. Deze methode kan worden gebruikt voor het installeren van de provider in Server Core voor Windows Server 2012 R2.

1. Download het providerinstallatiebestand en de registratiesleutel naar een map. Bijvoorbeeld: C:\ASR.
2. Stop de VMM-service.
3. Pak het Provider-installatieprogramma. Deze opdrachten uitvoeren als beheerder:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Installeer de Provider:

        C:\ASR> setupdr.exe /i
5. Registreer de Provider:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>         

Parameters:

* **/ Credentials**: vereiste parameter voor de locatie waarin het registratiesleutelbestand zich bevindt.  
* **/ FriendlyName**: vereiste parameter voor de naam van de Hyper-V-hostserver die wordt weergegeven in de Azure Site Recovery-portal.
* **/ EncryptionEnabled**: een optionele parameter alleen gebruikt wanneer van VMM naar Azure repliceren.
* **/proxyAddress**: een optionele parameter waarmee het adres van de proxyserver wordt opgegeven.
* **/proxyport**: een optionele parameter waarmee de poort van de proxyserver wordt opgegeven.
* **proxyusername**: optionele parameter waarmee de proxygebruikersnaam (als de proxy verificatie is vereist).
* **/ proxypassword**: optionele parameter waarmee het wachtwoord voor verificatie met de proxyserver (als de proxy verificatie is vereist).

## <a name="step-3-map-storage-arrays-and-pools"></a>Stap 3: Opslagmatrices en groepen toewijzen

Primaire en secundaire matrices om op te geven welke secundaire opslaggroep replicatiegegevens ontvangt van de primaire groep worden toegewezen. Opslag toewijzen voordat het configureren van replicatie, omdat de informatie over de Identiteitstoewijzing wordt gebruikt wanneer u de beveiliging voor replicatiegroepen inschakelen.

Voordat u begint, controleert u of de VMM-clouds worden weergegeven in de kluis. Clouds worden gedetecteerd bij het synchroniseren van alle clouds tijdens de installatie van de Provider of wanneer u een specifieke cloud in de VMM-console synchroniseren.

1. Klik op **Resources** > **serveropslag** > **kaart bron- en Doelmatrices**.
    ![Serverregistratie](./media/site-recovery-vmm-san/storage-map.png)

2. Selecteer de opslagmatrices op de primaire site en toe te wijzen aan opslagmatrices op de secundaire site. In **opslaggroepen**, selecteert u een bron en doel van de opslaggroep om toe te wijzen.

    ![Serverregistratie](./media/site-recovery-vmm-san/storage-map-pool.png)

## <a name="step-4-configure-replication-settings"></a>Stap 4: Replicatie-instellingen configureren

Nadat de VMM-servers zijn geregistreerd, kunt u cloudbeveiligingsinstellingen configureren.

1. Op de **Quick Start** pagina, klikt u op **beveiliging instellen voor VMM-clouds**.
2. Op de **beveiligde Items** tabblad, selecteer de cloud **configuratie**.
3. In **doel**, selecteer **VMM**.
4. In **doellocatie**, selecteert u de VMM-server die de cloud die u wilt gebruiken voor herstel beheert.
5. In **doel cloud**, selecteer de doelcloud die u wilt gebruiken voor VM-failover.
   * Het is raadzaam dat u selecteert een doelcloud op die voldoet aan de herstelvereisten voor de virtuele machines die u beveiligt.
   * Een cloud kan slechts aan één cloudpaar--als een primaire of een doelcloud op.
6. Site Recovery controleert of dat wolken hebben toegang tot SAN-opslag, en dat de opslagmatrices zijn toegewezen.
7. Als controle is geslaagd, in **replicatietype**, selecteer **SAN**.

Nadat u de instellingen hebt opgeslagen, wordt er een taak gemaakt die kan worden gecontroleerd op de **taken** tabblad. Instellingen kunnen worden gewijzigd op de **configureren** tabblad. Als u de doellocatie of doelcloud wijzigen wilt, moet u de cloudconfiguratie verwijderen en vervolgens opnieuw configureren van de cloud.

## <a name="step-5-enable-network-mapping"></a>Stap 5: Netwerktoewijzing inschakelen

1. Op de **Quick Start** pagina, klikt u op **netwerken toewijzen**.
2. Selecteer de bron-VMM-server en selecteer vervolgens de doel-VMM-server waarop de netwerken worden toegewezen. De lijst met de bron en hun bijbehorende doelnetwerken worden weergegeven. Een lege waarde weergegeven voor de netwerken die niet zijn toegewezen. Klik op het informatiepictogram naast de bron en doel netwerknamen om weer te geven van de subnetten voor elk netwerk.
3. Selecteer een netwerk in **netwerk op de bron**, en klik vervolgens op **kaart**. De service detecteert de VM-netwerken op de doelserver en geeft deze weer.

    ![SAN-architectuur](./media/site-recovery-vmm-san/network-map1.png)
4. Selecteer een van de VM-netwerken in de doel-VMM-server.

    ![SAN-architectuur](./media/site-recovery-vmm-san/network-map2.png)

5. Wanneer u een doelnetwerk selecteert, worden de beveiligde clouds die gebruikmaken van het Bronnetwerk weergegeven. Netwerken met beschikbare doelservers worden ook weergegeven. Het is raadzaam dat u selecteert een doelnetwerk dat beschikbaar is voor alle clouds die u voor replicatie gebruikt.
6. Klik op het vinkje om het toewijzingsproces te voltooien. Een taak gestart die uitgevoerd wordt bijgehouden. U kunt weergeven op de **taken** tabblad.

## <a name="step-6-enable-replication-for-replication-groups"></a>Stap 6: Replicatie inschakelen voor replicatiegroepen

Voordat u beveiliging voor virtuele machines inschakelen kunt, moet u replicatie inschakelen voor replicatie van opslaggroepen.

1. Op de **eigenschappen** pagina van de primaire cloud in de Site Recovery-portal, open de **virtuele Machines** tabblad en klik op **replicatiegroep toevoegen**.
2. Selecteer een of meer VMM replicatiegroepen die zijn gekoppeld aan de cloud, Controleer of de bron en doel-matrices en geef de replicatiefrequentie.

Site Recovery, VMM en de SMI-S-providers inrichten van de site doelopslag LUN's en storage-replicatie inschakelen. Als de replicatiegroep al is gerepliceerd, wordt met Site Recovery hergebruikt de bestaande replicatierelatie en de informatie bijgewerkt.

## <a name="step-7-enable-protection-for-virtual-machines"></a>Stap 7: Beveiliging voor virtuele machines inschakelen


Wanneer u een opslaggroep wordt gerepliceerd, schakel de beveiliging voor virtuele machines in de VMM-console met een van de volgende methoden:

* **Nieuwe virtuele machine**: wanneer u een virtuele machine maakt, u replicatie inschakelt en de virtuele machine koppelen aan de replicatiegroep.
  Met deze optie gebruikt VMM intelligente plaatsing om optimaal plaats van de VM-opslag op de LUN's van de replicatiegroep. Site Recovery ingedeeld van het maken van een schaduwkopie VM op de secundaire site en capaciteit toegewezen zodat replica-VM kunnen worden gestart na een failover.
* **Bestaande virtuele machine**: als een virtuele machine in VMM al is geïmplementeerd, kunt u replicatie inschakelt en uitvoeren van een opslagmigratie aan een replicatiegroep. Na voltooiing wordt VMM en de Site Recovery de nieuwe virtuele machine te detecteren en start deze beheren in Site Recovery. Een schaduw VM wordt gemaakt op de secundaire site en capaciteit is toegewezen, zodat de replica virtuele machine kan worden gestart na een failover.

![Beveiliging inschakelen](./media/site-recovery-vmm-san/enable-protect.png)

Nadat de virtuele machines zijn ingeschakeld voor replicatie, worden deze weergegeven in de Site Recovery-console. U kunt zien VM-eigenschappen, status volgen en bijhouden van failover-replicatiegroepen die meerdere virtuele machines bevatten. In SAN-replicatie moeten alle virtuele machines die zijn gekoppeld aan een replicatiegroep failover samen. Dit is omdat failover op de opslaglaag eerst plaatsvindt. Het is belangrijk dat uw replicatiegroepen goed gegroepeerd en alleen bijbehorende virtuele machines samen plaats.

> [!NOTE]
> Nadat u replicatie voor een virtuele machine hebt ingeschakeld, niet toevoegen de VHD's met LUN's tenzij ze zich in een replicatiegroep voor de Site Recovery bevinden. Virtuele harde schijven wordt alleen door Site Recovery worden gedetecteerd als ze zich in een replicatiegroep voor de Site Recovery bevinden.
>
>

U kunt de voortgang, inclusief de initiële replicatie op volgen de **taken** tabblad. Nadat de taak beveiliging voltooien wordt uitgevoerd, is de virtuele machine gereed voor failover.

![Beveiligingstaak voor virtuele machines](./media/site-recovery-vmm-san/job-props.png)

## <a name="step-8-test-the-deployment"></a>Stap 8: De implementatie testen

Test uw implementatie om ervoor te zorgen dat virtuele machines mislukken dan verwacht. U doet dit door een herstelplan maken en een testfailover uitvoeren.

1. Op de **herstelplannen** tabblad **herstelplan maken**.
2. Geef een naam voor het herstelplan en selecteer de bron en doel-VMM-servers. De bronserver moet virtuele machines die zijn ingeschakeld voor failover en herstel. Selecteer **SAN** om weer te geven alleen clouds die zijn geconfigureerd voor SAN-replicatie.

    ![Herstelplan maken](./media/site-recovery-vmm-san/r-plan.png)

3. In **virtuele Machines selecteren**, replicatiegroepen selecteren. Alle virtuele machines die zijn gekoppeld aan de groep worden toegevoegd aan het herstelplan. Deze virtuele machines worden toegevoegd aan de standaardgroep van recovery plan (groep 1). U kunt meer groepen toevoegen, indien nodig. Virtuele machines worden na de replicatie genummerd volgens de volgorde van de recovery planningsgroepen.

    ![Selecteer de virtuele machines](./media/site-recovery-vmm-san/r-plan-vm.png)
4. Nadat het herstelplan is gemaakt, is deze weergegeven in de lijst op de **herstelplannen** tabblad. Selecteer het plan en kies **Testfailover**.
5. Op de **Testfailover bevestigen** pagina **geen**. Met deze optie is ingeschakeld, failover replica-VM niet verbonden met een netwerk. Hiermee wordt gecontroleerd dat de virtuele machines mislukken dan verwacht, maar niet het testen van de netwerkomgeving. Zie voor meer informatie over andere netwerkopties [siteherstel failover](site-recovery-failover.md).

    ![Selecteer testnetwerk](./media/site-recovery-vmm-san/test-fail1.png)

6. De test virtuele machine wordt gemaakt op dezelfde host als de host waarop de replica virtuele machine bestaat. Dit is niet toegevoegd aan de cloud waarin de replica-VM zich bevindt.
2. De replica virtuele machine heeft na de replicatie, een ander IP-adres dan de primaire virtuele machine. Als u adressen van DHCP uitgeeft, wordt deze automatisch worden bijgewerkt. Als u geen DHCP en u dezelfde adressen wilt, moet u een aantal scripts worden uitgevoerd.
3. Voer dit script voor het ophalen van het IP-adres:

       $vm = Get-SCVirtualMachine -Name <VM_NAME>
       $na = $vm[0].VirtualNetworkAdapters>
       $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
       $ip.address  

4. Voer dit voorbeeldscript voor het bijwerken van DNS. Geef het IP-adres dat u hebt opgehaald.

       [string]$Zone,
       [string]$name,
       [string]$IP
       )
       $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
       $newrecord = $record.clone()
       $newrecord.RecordData[0].IPv4Address  =  $IP
       Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


## <a name="next-steps"></a>Volgende stappen

Na het uitvoeren van een testfailover kan controleren of uw omgeving werkt zoals verwacht, Zie [siteherstel failover](site-recovery-failover.md) voor meer informatie over de verschillende soorten failovers.
