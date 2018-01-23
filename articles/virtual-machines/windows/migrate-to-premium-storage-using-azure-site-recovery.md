---
title: Uw Windows-virtuele machines migreren naar Azure Premium-opslag met Azure Site Recovery | Microsoft Docs
description: Uw bestaande virtuele machines migreren naar Azure Premium-opslag met behulp van Site Recovery. Premium-opslag biedt ondersteuning voor schijven voor hoge prestaties, lage latentie voor I/O-intensieve werkbelastingen die worden uitgevoerd op Azure Virtual Machines.
services: virtual-machines-windows
cloud: Azure
documentationcenter: na
author: luywang
manager: jeconnoc
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: luywang
ms.openlocfilehash: 325ddb640e0cc10ae242e0a4ce2cc0d042dad320
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="migrate-to-premium-storage-by-using-azure-site-recovery"></a>Migreren naar Premium-opslag met behulp van Azure Site Recovery

[Azure Premium-opslag](premium-storage.md) biedt ondersteuning voor hoge prestaties, lage latentie schijven voor virtuele machines (VM's) die I/O-intensieve werkbelastingen worden uitgevoerd. Deze handleiding helpt u uw VM-schijven van de account van een standard-opslag migreren naar een premium storage-account met behulp van [Azure Site Recovery](../../site-recovery/site-recovery-overview.md).

Site Recovery is een Azure-service die aan uw strategie voor bedrijfscontinuïteit en herstel na noodgevallen bijdraagt door de replicatie van fysieke on-premises servers en virtuele machines in de cloud (Azure) of naar een secundair datacenter te organiseren. Wanneer er storingen optreden op uw primaire locatie, schakelt u over naar de secundaire locatie om toepassingen en workloads beschikbaar te houden. U failback naar uw primaire locatie wanneer deze weer normaal functioneren. 

Site Recovery biedt testfailovers ter ondersteuning van noodhersteloefeningen zonder productieomgevingen. U kunt failovers met minimaal gegevensverlies (afhankelijk van de replicatiefrequentie) bij onverwachte noodsituaties uitvoeren. In het scenario van de migratie naar de Premium-opslag, kunt u de [-failover in Site Recovery](../../site-recovery/site-recovery-failover.md) doel schijven migreren naar een premium storage-account.

Het is raadzaam om de migratie naar de Premium-opslag met behulp van Site Recovery omdat deze optie biedt een minimale downtime. Deze optie voorkomt ook de handmatige uitvoering van het kopiëren van schijven en het maken van nieuwe virtuele machines. Site Recovery wordt systematischer kopiëren van de schijven en nieuwe virtuele machines maken tijdens de failover. 

Site Recovery biedt ondersteuning voor verschillende soorten failovers met minimaal of er geen uitvaltijd. Voor het plannen van uw uitvaltijd en verlies van gegevens schatten, Zie de [soorten failovers in Site Recovery](../../site-recovery/site-recovery-failover.md). Als u [voorbereiden verbinding maken met virtuele Azure-machines na een failover](../../site-recovery/vmware-walkthrough-overview.md), moet u kunnen verbinding maken met de Azure VM via RDP na een failover.

![Diagram van herstel na noodgevallen][1]

## <a name="azure-site-recovery-components"></a>Azure Site Recovery-onderdelen

Deze Site Recovery-onderdelen die relevant zijn voor dit migratiescenario:

* **Configuratieserver** is van een virtuele machine van Azure die coördineert de communicatie en processen voor replicatie en herstel van gegevens beheert. Op deze virtuele machine uitvoeren van een enkele setup-bestand voor het installeren van de configuratieserver en een extra onderdeel aangeroepen processerver, als replicatiegateway. Meer informatie over [server configuratievereisten](../../site-recovery/vmware-walkthrough-overview.md). Instellen van de configuratieserver slechts één keer en u kunt deze gebruiken voor alle migraties om dezelfde regio.

* **Processerver** is een replicatiegateway die: 

  1. Ontvangt gegevens van replicatie van de virtuele bronmachines.
  2. Optimaliseert de gegevens met caching, compressie en codering.
  3. Verzendt de gegevens naar een opslagaccount. 

  Ook push-installatie van de mobility-service naar de virtuele bronmachines worden verwerkt en wordt automatische detectie van de virtuele bronmachines uitgevoerd. De processerver standaard is geïnstalleerd op de configuratieserver. U kunt aanvullende zelfstandige processervers om te schalen van uw implementatie kunt implementeren. Meer informatie over [best practices voor processerverimplementatie](https://azure.microsoft.com/blog/best-practices-for-process-server-deployment-when-protecting-vmware-and-physical-workloads-with-azure-site-recovery/) en [implementatie extra processervers](../../site-recovery/site-recovery-plan-capacity-vmware.md#deploy-additional-process-servers). Instellen van de processerver slechts één keer en u kunt deze gebruiken voor alle migraties om dezelfde regio.

* **Mobility-service** is een onderdeel dat is geïmplementeerd op elke standard VM die u wilt repliceren. Deze gegevens schrijfbewerkingen op de standaard virtuele machine vastgelegd en stuurt deze door naar de processerver. Meer informatie over [gerepliceerde machine vereisten](../../site-recovery/vmware-walkthrough-overview.md).

Deze afbeelding toont de wisselwerking tussen deze onderdelen:

![Interactie van onderdelen van Site Recovery][15]

> [!NOTE]
> Site Recovery biedt geen ondersteuning voor de migratie van schijven met opslagruimten.

Zie voor extra onderdelen voor andere scenario's [scenarioarchitectuur](../../site-recovery/vmware-walkthrough-overview.md).

## <a name="azure-essentials"></a>Azure essentials

Dit zijn de Azure-vereisten voor dit migratiescenario:

* Een Azure-abonnement.
* Een Azure premium storage-account voor het opslaan van gerepliceerde gegevens.
* Een Azure-netwerk waarmee virtuele machines verbinding maken wanneer ze worden gemaakt bij een failover. De virtuele Azure-netwerk moet in dezelfde regio bevinden als de waarin de Site Recovery wordt uitgevoerd.
* Een Azure standard-opslag-account voor het opslaan van replicatielogboeken. Dit is hetzelfde opslagaccount voor de VM-schijven die worden gemigreerd.

## <a name="prerequisites"></a>Vereisten

* Inzicht in de relevante migratie scenario-onderdelen in de vorige sectie.
* De uitvaltijd plannen door informatie over [-failover in Site Recovery](../../site-recovery/site-recovery-failover.md).

## <a name="setup-and-migration-steps"></a>Stappen voor installatie en -migratie

U kunt Site Recovery kunt gebruiken voor het migreren van Azure IaaS VM's tussen regio's of binnen dezelfde regio. De volgende instructies zijn geschikt voor dit migratiescenario van het artikel [VMware-machines repliceren of fysieke servers naar Azure](../../site-recovery/vmware-walkthrough-overview.md). Volg de koppelingen voor gedetailleerde stappen naast de instructies in dit artikel.

### <a name="step-1-create-a-recovery-services-vault"></a>Stap 1: Een Recovery Services-kluis maken

1. Open de [Azure Portal](https://portal.azure.com).
2. Selecteer **nieuwe** > **Management** > **back-up** en **Site Recovery (OMS)**. U kunt ook selecteren **Bladeren** > **Recovery Services-kluis** > **toevoegen**. 
3. Geef een regio die virtuele machines worden gerepliceerd naar. Selecteer de regio waar uw Bronmachines en bron storage-accounts zijn omwille van de migratie in dezelfde regio. 

### <a name="step-2-choose-your-protection-goals"></a>Stap 2: Uw beveiligingsdoelstellingen kiezen 

1. Open op de virtuele machine waarop u wilt installeren van de configuratieserver, de [Azure-portal](https://portal.azure.com).
2. Ga naar **Recovery Services-kluizen** > **instellingen** > **siteherstel** > **stap 1: voorbereiden Infrastructuur** > **beveiligingsdoel**.

   ![Bladeren naar de doel-deelvenster beveiliging][2]

3. Onder **beveiligingsdoel**, selecteer in de vervolgkeuzelijst eerste **naar Azure**. Selecteer in de vervolgkeuzelijst tweede **niet gevirtualiseerde / andere**, en selecteer vervolgens **OK**.

   ![Doel deelvenster beveiliging met ingevuld vakken][3]

### <a name="step-3-set-up-the-source-environment-configuration-server"></a>Stap 3: Instellen van de bronomgeving (configuratieserver)

1. Download **Azure Site Recovery Unified Setup** en de kluisregistratiesleutel door te gaan naar de **infrastructuur voorbereiden** > **bron voorbereiden**  >  **Server toevoegen** deelvensters. 
 
   U moet de kluisregistratiesleutel de uniforme setup uit te voeren. De sleutel blijft vijf dagen na het genereren ervan geldig.

   ![Bladeren naar het deelvenster Server toevoegen][4]

2. In de **Server toevoegen** deelvenster een configuratieserver toevoegen.

   ![Deelvenster van de Server toevoegen met de configuratieserver geselecteerd][5]

3. Op de virtuele machine die u als de configuratieserver gebruikt Unified Setup uitvoeren om te installeren van de configuratieserver en de processerver. U kunt [helpt bij de schermafbeeldingen](../../site-recovery/vmware-walkthrough-overview.md) om de installatie te voltooien. U kunt verwijzen naar de volgende schermafbeeldingen voor stappen die zijn opgegeven voor dit migratiescenario.

   1. In **voordat u begint**, selecteer **installeren van de configuratieserver en de processerver**.

      ![Voordat u begint met pagina][6]

   2. In **registratie**, bladeren en selecteer de registratiesleutel die u hebt gedownload van de kluis.

      ![Pagina voor registratie][7]

   3. Selecteer bij **Details van de omgeving** of u virtuele VMware-machines wilt repliceren. Voor dit migratiescenario kiezen **Nee**.

      ![De pagina Details omgeving][8]

4. Nadat de installatie voltooid is, doet u het volgende de **Microsoft Azure Site Recovery-configuratieserver** venster:
 
   1. Gebruik de **Accounts beheren** tabblad account wilt maken die Site Recovery kunt gebruiken voor automatische detectie. (In het scenario over het beveiligen van fysieke machines, instellen van het account niet relevant, maar u moet ten minste één account om in te schakelen op een van de volgende stappen uit. In dit geval kunt u naam-account en wachtwoord als willekeurig.) 
   2. Gebruik de **kluis registratie** tabblad voor het uploaden van het kluisreferentiebestand.

      ![Kluis registratie tabblad][9]

### <a name="step-4-set-up-the-target-environment"></a>Stap 4: De doelomgeving instellen

Selecteer **infrastructuur voorbereiden** > **doel**, en geef het implementatiemodel dat u wilt gebruiken voor virtuele machines na een failover. U kunt kiezen **klassieke** of **Resource Manager**, afhankelijk van uw scenario.

![Doel deelvenster][10]

Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt. 

> [!NOTE]
> Als u een premium storage-account voor gerepliceerde gegevens gebruikt, moet u een extra standard-opslagaccount instellen voor het opslaan van replicatielogboeken.

### <a name="step-5-set-up-replication-settings"></a>Stap 5: Replicatie-instellingen instellen

Om te bevestigen dat de configuratieserver is gekoppeld aan het beleid voor wachtwoordreplicatie die u maakt, volg [replicatie-instellingen instellen](../../site-recovery/vmware-walkthrough-overview.md).

### <a name="step-6-plan-capacity"></a>Stap 6: Plan capaciteit

1. Gebruik de [Capaciteitsplanner](../../site-recovery/site-recovery-capacity-planner.md) moet netwerkbandbreedte, opslag en andere vereisten om te voldoen aan uw replicatie nauwkeurig te schatten. 
2. Wanneer u bent klaar, selecteert u **Ja, ik dit hebt gedaan** in **hebt u capaciteitsplanning?**.

   ![Vak voor het bevestigen dat u capaciteitsplanning voltooid][11]

### <a name="step-7-install-the-mobility-service-and-enable-replication"></a>Stap 7: Installeren van de mobility-service en -replicatie inschakelen

1. U kunt [push-installatie](../../site-recovery/vmware-walkthrough-overview.md) aan uw Bronmachines of aan [handmatig installeren van de mobility-service](../../site-recovery/site-recovery-vmware-to-azure-install-mob-svc.md) op uw virtuele bronmachines. U vindt de vereiste van de installatie en het pad van het installatieprogramma voor het handmatig pushen in de onderstaande koppeling. Als u een handmatige installatie uitvoert, moet u mogelijk een interne IP-adres gebruiken om te bepalen van de configuratieserver.

   ![Configuratiepagina voor Server-gegevens][12]

   De failover-VM heeft twee tijdelijke schijven: één van de primaire virtuele machine en de andere gemaakt tijdens het inrichten van de virtuele machine in de regio van het herstel. Als u wilt uitsluiten van de tijdelijke schijf voordat de replicatie, installeert u de mobility-service voordat u replicatie inschakelt. Zie voor meer informatie over het uitsluiten van de tijdelijke schijf, [schijven uitsluiten van replicatie](../../site-recovery/vmware-walkthrough-overview.md).

2. Replicatie als volgt inschakelen:
   1. Selecteer **toepassing repliceren** > **bron**. Nadat u replicatie voor het eerst hebt ingeschakeld, selecteert u **+ repliceren** in de kluis aanvullende machines replicatie in te schakelen.
   2. In stap 1 voert instellen **bron** als de processerver.
   3. Geef het implementatiemodel na een failover, een premium storage-account om te migreren naar, een standaard opslagaccount logboeken en een virtueel netwerk niet opslaan in stap 2.
   4. In stap 3 toevoegen beveiligde virtuele machines op IP-adres. (Mogelijk moet u een interne IP-adres te zoeken.)
   5. In stap 4, door de eigenschappen te configureren met behulp van de accounts die u eerder hebt ingesteld op de processerver.
   6. Kies het replicatiebeleid dat u eerder hebt gemaakt in stap 5, ' stap 5: replicatie-instellingen instellen. "
   7. Selecteer **OK**.

   > [!NOTE]
   > Wanneer een Azure VM is de toewijzing ongedaan gemaakt en opnieuw hebt gestart, is er geen garantie dat hetzelfde IP-adres worden opgehaald. Als het IP-adres van de server-proces configuratieserver of de beveiligde virtuele machines van Azure wordt gewijzigd, werken de replicatie in dit scenario mogelijk niet correct.

   ![Deelvenster replicatie inschakelen met bron geselecteerd][13]

Wanneer u uw Azure Storage-omgeving ontwerpt, wordt u aangeraden dat u afzonderlijke storage-accounts voor elke virtuele machine in een beschikbaarheidsset gebruiken. We raden u aan de aanbevolen procedure in de opslaglaag te [meerdere storage-accounts gebruiken voor elke beschikbaarheidsset](../linux/manage-availability.md). Distributie van VM-schijven naar meerdere accounts voor opslag voor het verbeteren van de beschikbaarheid van opslag en verdeelt de i/o over de Azure-opslag-infrastructuur.

Als uw virtuele machines in een beschikbaarheidsset, in plaats van de schijven van alle virtuele machines repliceren naar één storage-account, sterk aangeraden meerdere keren voor meerdere virtuele machines migreren. Op die manier de virtuele machines in dezelfde beschikbaarheidsset delen niet één storage-account. Gebruik de **replicatie inschakelen** deelvenster kunt u een doelopslagaccount instellen voor elke virtuele machine, één voor één.
 
U kunt een implementatiemodel na een failover volgens uw behoeften. Als u Azure Resource Manager als uw na een failover-implementatiemodel kiezen, kunt u een virtuele machine (Resource Manager) voor een virtuele machine (Resource Manager) failover of kunt u failover van een VM (klassiek) voor een virtuele machine (Resource Manager).

### <a name="step-8-run-a-test-failover"></a>Stap 8: Een testfailover uitvoeren

Als u wilt controleren of de replicatie voltooid is, selecteer uw Site Recovery-exemplaar en selecteer vervolgens **instellingen** > **gerepliceerde Items**. U ziet de status en het percentage van het replicatieproces. 

Na de initiële replicatie is voltooid, een testfailover voor het valideren van uw replicatiestrategie voor uitvoeren. Zie voor gedetailleerde stappen van een testfailover [een testfailover uitvoeren in Site Recovery](../../site-recovery/vmware-walkthrough-overview.md). 

> [!NOTE]
> Voordat u een failover uitvoert, zorg ervoor dat uw virtuele machines en replicatiestrategie voldoen aan de vereisten. Zie voor meer informatie over het uitvoeren van een testfailover [testen van failover naar Azure in Site Recovery](../../site-recovery/site-recovery-test-failover-to-azure.md).

U ziet de status van de testfailover in **instellingen** > **taken** > *YOUR_FAILOVER_PLAN_NAME*. Klik in het deelvenster ziet u een overzicht van de stappen en de resultaten van de geslaagde/mislukte. Als de testfailover is mislukt tijdens elke stap, selecteert u de stap voor het controleren van het foutbericht. 

### <a name="step-9-run-a-failover"></a>Stap 9: Een failover uitvoeren

Nadat de test is failover voltooid, wordt een failover voor het migreren van uw schijven naar Premium-opslag en repliceren van de VM-instanties worden uitgevoerd. De gedetailleerde stappen in [een failover uitvoeren](../../site-recovery/site-recovery-failover.md#run-a-failover). 

Selecteer **virtuele machines afsluiten en de meest recente gegevens synchroniseren**. Deze optie geeft aan dat de Site Recovery proberen moet de beveiligde virtuele machines afsluiten en de gegevens te synchroniseren, zodat de meest recente versie van de gegevens failover. Als u deze optie niet selecteert, of de poging niet lukt, worden de failover van de meest recente beschikbare herstelpunt voor de virtuele machine. 

Site Recovery maakt een VM-exemplaar waarvan het type hetzelfde als of vergelijkbare voor een virtuele machine voor Premium-opslag-ondersteuning is. U kunt de prestaties en de prijs van verschillende VM-instanties controleren door te gaan naar [prijzen van virtuele Machines in Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) of [prijzen van Linux virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="post-migration-steps"></a>Stappen na de migratie

1. **Configureren van de gerepliceerde virtuele machines voor de beschikbaarheidsset, indien van toepassing**. Site Recovery biedt geen ondersteuning voor migratie VM's samen met de beschikbaarheidsset. Afhankelijk van de implementatie van de gerepliceerde virtuele machine, gaat u een van de volgende:
   * Voor een virtuele machine wordt gemaakt via het klassieke implementatiemodel: de virtuele machine toevoegen aan de beschikbaarheidsset voor de Azure-portal. Ga voor gedetailleerde stappen naar [een bestaande virtuele machine toevoegen aan een beschikbaarheidsset](../linux/classic/configure-availability-classic.md).
   * Voor een virtuele machine wordt gemaakt via het implementatiemodel van Resource Manager: opslaan van uw configuratie van de virtuele machine en vervolgens verwijderen en opnieuw maken van de virtuele machines in de beschikbaarheidsset. Om dit te doen, gebruikt u het script op [ingesteld Azure Resource Manager VM Beschikbaarheidsset](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4). De beperkingen controleren voordat u dit script uitvoert, en de uitvaltijd plannen.

2. **Verwijderen van oude VM's en schijven**. Zorg ervoor dat de Premium-schijven consistent met de schijven van de bron zijn en dat de nieuwe virtuele machines uit te dezelfde functie uit als de bron-VM's voeren. Verwijder de virtuele machine en de schijven verwijderen uit uw gegevensbron storage-accounts in de Azure portal. Als er is een probleem dat de schijf is niet verwijderd, ook al verwijderd van de virtuele machine, raadpleegt u [fouten bij het verwijderen van VHD's oplossen](../../storage/common/storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

3. **Opschonen van de Azure Site Recovery-infrastructuur**. Wanneer u Site Recovery niet langer nodig hebt, kunt u de infrastructuur opschonen. Gerepliceerde items, de configuratieserver en het herstelbeleid verwijderd en verwijder vervolgens de Azure Site Recovery-kluis.

## <a name="troubleshooting"></a>Problemen oplossen

* [Controleren en problemen oplossen van beveiliging voor virtuele machines en fysieke servers](../../site-recovery/site-recovery-monitoring-and-troubleshooting.md)
* [Microsoft Azure Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)

## <a name="next-steps"></a>Volgende stappen

Zie voor specifieke scenario's voor het migreren van virtuele machines, de volgende bronnen:

* [Azure virtuele Machines tussen Opslagaccounts migreren](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Maken en een Windows Server-VHD uploaden naar Azure](upload-generalized-managed.md)
* [Migreren van virtuele Machines van Amazon AWS naar Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Zie ook de volgende bronnen voor meer informatie over Azure Storage en Azure Virtual Machines:

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Premium Storage: opslag met hoge prestaties voor de workloads van virtuele Azure-machines](premium-storage.md)

[1]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-1.png
[2]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-2.png
[3]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-3.png
[4]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-4.png
[5]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-5.png
[6]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-6.PNG
[7]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-7.PNG
[8]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-8.PNG
[9]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-9.PNG
[10]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-10.png
[11]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-11.PNG
[12]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-12.PNG
[13]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-13.png
[14]:../site-recovery/media/site-recovery-vmware-to-azure/v2a-architecture-henry.png
[15]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-14.png
