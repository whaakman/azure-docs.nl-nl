---
title: Uw Windows-VM's migreren naar Azure Premium Storage met Azure Site Recovery | Microsoft Docs
description: Uw bestaande virtuele machines migreren naar Azure Premium Storage met behulp van Site Recovery. Premium Storage biedt ondersteuning voor schijven voor hoge prestaties en lage latentie voor I/O-intensieve werkbelastingen op Azure Virtual Machines.
services: virtual-machines-windows,storage
cloud: Azure
author: luywang
ms.service: virtual-machines-windows
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/15/2017
ms.author: luywang
ms.subservice: disks
ms.openlocfilehash: 5744ee76d36b1cea256cd2594bcbc07c954f38f7
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57993289"
---
# <a name="migrate-to-premium-storage-by-using-azure-site-recovery"></a>Migreren naar Premium Storage met behulp van Azure Site Recovery

[Azure premium SSD's](disks-types.md) bieden ondersteuning voor hoge prestaties en lage latentie schijven voor virtuele machines (VM's) die I/O-intensieve workloads worden uitgevoerd. Deze handleiding helpt u de VM-schijven van een standaardopslagaccount migreren naar premium storage-account met behulp van [Azure Site Recovery](../../site-recovery/site-recovery-overview.md).

Site Recovery is een Azure-service die aan uw strategie voor bedrijfscontinuïteit en herstel na noodgevallen bijdraagt door de replicatie van fysieke on-premises servers en virtuele machines in de cloud (Azure) of naar een secundair datacenter te organiseren. Als er uitval optreedt op uw primaire locatie, schakelt u over naar de secundaire locatie om toepassingen en workloads beschikbaar te houden. U een failback naar de primaire locatie wanneer deze weer normaal functioneren. 

Site Recovery biedt testfailovers ter ondersteuning van noodhersteloefeningen zonder gevolgen voor productie-omgevingen. U kunt failovers uitvoeren met minimaal gegevensverlies (afhankelijk van de replicatiefrequentie) bij onverwachte noodsituaties. In het scenario van de migratie naar Premium Storage, kunt u de [failover in Site Recovery](../../site-recovery/site-recovery-failover.md) doel schijven migreren naar premium storage-account.

Het is raadzaam om eerst te migreren naar Premium Storage met behulp van Site Recovery omdat deze optie minimale downtime biedt. Deze optie voorkomt ook de handmatige uitvoering van kopiëren van schijven en het maken van nieuwe virtuele machines. Site Recovery wordt systematisch uw schijven kopiëren en nieuwe virtuele machines maken tijdens de failover. 

Site Recovery ondersteunt een aantal soorten failovers met minimale of geen downtime. Als u wilt uw uitvaltijd plannen en een schatting maken van gegevensverlies, Zie de [typen failover in Site Recovery](../../site-recovery/site-recovery-failover.md). Als u [voorbereidingen voor het verbinding maken met virtuele Azure-machines na een failover](../../site-recovery/vmware-walkthrough-overview.md), moet u kunnen verbinding maken met de Azure-VM met behulp van RDP na een failover.

![Diagram van herstel na noodgevallen][1]

## <a name="azure-site-recovery-components"></a>Azure Site Recovery-onderdelen

Deze Site Recovery-onderdelen zijn relevant voor dit scenario voor migratie:

* **Configuratieserver** is een Azure-VM die coördineert de communicatie en processen voor replicatie en het herstel van gegevens beheert. Op deze virtuele machine, uitvoeren van een enkele setup-bestand voor het installeren van de configuratieserver en een extra onderdeel met de naam een processerver als replicatiegateway. Meer informatie over [vereisten voor configuratieserver](../../site-recovery/vmware-walkthrough-overview.md). Instellen van de configuratieserver slechts één keer, en u kunt deze gebruiken voor alle migraties om dezelfde regio bevinden.

* **Processerver** replicatiegateway is die: 

  1. Ontvangt replicatiegegevens van bron-VM's.
  2. Optimaliseert de gegevens met caching, compressie en codering.
  3. Verzendt de gegevens naar een opslagaccount. 

  Ook push-installatie van de mobility-service op de bron-VM's worden verwerkt en wordt automatische detectie van de bron-VM's uitgevoerd. De standaard-processerver is geïnstalleerd op de configuratieserver. U kunt aanvullende zelfstandige processervers als u de schaal van uw implementatie wilt implementeren. Meer informatie over [aanbevolen procedures voor processerverimplementatie](https://azure.microsoft.com/blog/best-practices-for-process-server-deployment-when-protecting-vmware-and-physical-workloads-with-azure-site-recovery/) en [implementeren extra processervers](../../site-recovery/site-recovery-plan-capacity-vmware.md#deploy-additional-process-servers). Instellen van de processerver slechts één keer, en u kunt deze gebruiken voor alle migraties om dezelfde regio bevinden.

* **Mobility-service** is een onderdeel dat is geïmplementeerd op elke standard Virtuele machine die u wilt repliceren. Het schrijven van gegevens op de standaard virtuele machine worden vastgelegd en ze doorstuurt naar de processerver. Meer informatie over [vereisten voor de machines gerepliceerd](../../site-recovery/vmware-walkthrough-overview.md).

Deze afbeelding ziet u hoe deze onderdelen werken:

![Interactie van Site Recovery-onderdelen][15]

> [!NOTE]
> Site Recovery biedt geen ondersteuning voor de migratie van schijven met opslagruimten.

Zie voor extra onderdelen voor andere scenario's, [scenarioarchitectuur](../../site-recovery/vmware-walkthrough-overview.md).

## <a name="azure-essentials"></a>Azure essentials

Dit zijn de vereisten voor Azure voor dit scenario voor migratie:

* Een Azure-abonnement.
* Een Azure premium storage-account voor het opslaan van gerepliceerde gegevens.
* Een Azure-netwerk waarmee virtuele machines verbinding maken wanneer ze na een failover worden gemaakt. Het Azure-netwerk moet zich in dezelfde regio bevinden als het account dat in die Site Recovery wordt uitgevoerd.
* Azure standard storage-account voor het opslaan van replicatielogboeken. Dit kan hetzelfde opslagaccount voor de VM-schijven die worden gemigreerd zijn.

## <a name="prerequisites"></a>Vereisten

* Krijg inzicht in de relevante migratie scenario-onderdelen in de vorige sectie.
* Plannen van uw uitvaltijd door te leren over [failover in Site Recovery](../../site-recovery/site-recovery-failover.md).

## <a name="setup-and-migration-steps"></a>Stappen voor installatie en migratie

U kunt Site Recovery gebruiken voor het migreren van Azure IaaS VM's tussen regio's of binnen dezelfde regio bevinden. De volgende instructies zijn geschikt voor dit scenario voor migratie van het artikel [repliceren virtuele VMware-machines of fysieke servers naar Azure](../../site-recovery/vmware-walkthrough-overview.md). Volg de koppelingen voor gedetailleerde stappen naast de instructies in dit artikel.

### <a name="step-1-create-a-recovery-services-vault"></a>Stap 1: Een Recovery Services-kluis maken

1. Open de [Azure Portal](https://portal.azure.com).
2. Selecteer **een resource maken** > **Management** > **back-up en Site Recovery (OMS)**. U kunt ook selecteren **Bladeren** > **Recovery Services-kluis** > **toevoegen**.
   >[!NOTE]
   >Back-up en Site Recovery was voorheen deel uitmaakt van de ![OMS suite](https://github.com/MicrosoftDocs/azure-docs-pr/pull/azure-monitor/azure-monitor-rebrand.md#retirement-of-operations-management-suite-brand).
1. Een regio die virtuele machines worden gerepliceerd naar opgeven. Selecteer de regio waar uw bron-VM's en de bron-opslagaccounts zijn voor migratie in dezelfde regio. 

### <a name="step-2-choose-your-protection-goals"></a>Stap 2: Uw beveiligingsdoelstellingen kiezen 

1. Open op de virtuele machine waarop u wilt installeren van de configuratieserver, de [Azure-portal](https://portal.azure.com).
2. Ga naar **Recovery Services-kluizen** > **instellingen** > **siteherstel** > **stap 1: infrastructuur voorbereiden** > **Beveiligingsdoel.**

   ![Bladeren naar het deelvenster beveiliging doel][2]

3. Onder **beveiligingsdoel**, selecteer in de eerste vervolgkeuzelijst met **naar Azure**. Selecteer in de tweede vervolgkeuzelijst met **niet gevirtualiseerd / Overig**, en selecteer vervolgens **OK**.

   ![Doel-deelvenster beveiliging met ingevuld vakken][3]

### <a name="step-3-set-up-the-source-environment-configuration-server"></a>Stap 3: Instellen van de bronomgeving (configuratieserver)

1. Download **geïntegreerde Setup van Azure Site Recovery** en de kluisregistratiesleutel door te gaan naar de **infrastructuur voorbereiden** > **bron voorbereiden**  >  **-Server toevoegen** deelvensters. 
 
   U moet de kluisregistratiesleutel de geïntegreerde setup uit te voeren. De sleutel blijft vijf dagen na het genereren ervan geldig.

   ![Bladeren naar het deelvenster Server toevoegen][4]

2. In de **-Server toevoegen** in het deelvenster een configuratieserver toevoegen.

   ![Deelvenster van de Server toevoegen met de configuratieserver geselecteerd][5]

3. Voer op de virtuele machine die u als de configuratieserver gebruikt, geïntegreerde Setup voor het installeren van de configuratieserver en de processerver. U kunt [stapsgewijs door de schermafbeeldingen](../../site-recovery/vmware-walkthrough-overview.md) om de installatie te voltooien. U kunt verwijzen naar de volgende schermafbeeldingen voor stappen die zijn opgegeven voor dit scenario voor migratie.

   1. In **voordat u begint**, selecteer **de configuratieserver en processerver installeren**.

      ![Voordat u begint met pagina][6]

   2. In **registratie**, bladeren en selecteer de registratiesleutel die u hebt gedownload van de kluis.

      ![Pagina voor de registratie][7]

   3. Selecteer bij **Details van de omgeving** of u virtuele VMware-machines wilt repliceren. Kies voor dit migratiescenario **Nee**.

      ![Pagina met Details van omgeving][8]

4. Nadat de installatie voltooid is, doet u het volgende de **Microsoft Azure Site Recovery-configuratieserver** venster:
 
   1. Gebruik de **Accounts beheren** tabblad om te maken van het account dat Site Recovery kunt gebruiken voor automatische detectie. (In het scenario over het beveiligen van fysieke computers, het instellen van het account is niet relevant, maar u hebt ten minste één account om in te schakelen op een van de volgende stappen nodig. In dit geval kunt u naam het account en wachtwoord als willekeurig.) 
   2. Gebruik de **kluis registratie** tabblad voor het uploaden van het kluisreferentiebestand.

      ![Tabblad Kluisregistratie][9]

### <a name="step-4-set-up-the-target-environment"></a>Stap 4: De doelomgeving instellen

Selecteer **infrastructuur voorbereiden** > **doel**, en geef het implementatiemodel dat u wilt gebruiken voor virtuele machines na een failover. U kunt ervoor kiezen **klassieke** of **Resource Manager**, afhankelijk van uw scenario.

![Doel-deelvenster][10]

Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt. 

> [!NOTE]
> Als u een premium storage-account voor gerepliceerde gegevens gebruikt, moet u een extra standard-opslagaccount instellen voor het opslaan van replicatielogboeken.

### <a name="step-5-set-up-replication-settings"></a>Stap 5: Replicatie-instellingen instellen

Om te bevestigen dat uw configuratieserver is gekoppeld aan het replicatiebeleid dat u hebt gemaakt, gaat u als volgt [replicatie-instellingen instellen](../../site-recovery/vmware-walkthrough-overview.md).

### <a name="step-6-plan-capacity"></a>Stap 6: Capaciteit plannen

1. Gebruik de [Capaciteitsplanner](../../site-recovery/site-recovery-capacity-planner.md) nauwkeurig te schatten netwerkbandbreedte, opslag en andere vereisten voor uw behoeften. 
2. Wanneer u klaar bent, selecteert u **Ja, heb ik gedaan** in **hebt u plannen van capaciteit?**.

   ![Vak voor het bevestigen dat u voltooid capaciteitsplanning][11]

### <a name="step-7-install-the-mobility-service-and-enable-replication"></a>Stap 7: De mobility-service installeren en inschakelen van replicatie

1. U kunt kiezen om [push-installatie](../../site-recovery/vmware-walkthrough-overview.md) aan uw bron-VM's of aan [de mobiliteitsservice handmatig installeren](../../site-recovery/site-recovery-vmware-to-azure-install-mob-svc.md) op uw bron-VM's. Hier vindt u de vereiste van de installatie en het pad van het installatieprogramma voor het handmatig pushen in de onderstaande koppeling. Als u een handmatige installatie uitvoert, moet u mogelijk een interne IP-adres gebruiken om te bepalen van de configuratieserver.

   ![Pagina met Configuration Server Details][12]

   De failover-VM heeft twee tijdelijke schijven: één van de primaire virtuele machine en de andere gemaakt tijdens het inrichten van de virtuele machine in de herstelregio voor. Als u wilt uitsluiten van de tijdelijke schijf voordat u replicatie, de mobility-service te installeren voordat u replicatie inschakelt. Zie voor meer informatie over het uitsluiten van de tijdelijke schijf, [schijven uitsluiten van replicatie](../../site-recovery/vmware-walkthrough-overview.md).

2. Schakel als volgt replicatie in:
   1. Selecteer **toepassing repliceren** > **bron**. Wanneer u replicatie voor de eerste keer inschakelt, selecteer **+ repliceren** in de kluis replicatie voor aanvullende machines in te schakelen.
   2. Instellen in stap 1, **bron** als de processerver.
   3. Geef in stap 2, het implementatiemodel na failover, een premium storage-account om naar te migreren, een standaardopslagaccount voor het opslaan van Logboeken en een virtueel netwerk niet kunnen worden.
   4. In stap 3 toevoegen beveiligde virtuele machines door IP-adres. (Mogelijk moet u een interne IP-adres te zoeken.)
   5. In stap 4, door de eigenschappen te configureren door het selecteren van de accounts die u eerder hebt ingesteld op de processerver.
   6. In stap 5, kies het replicatiebeleid dat u eerder hebt gemaakt in ' stap 5: Instellen van replicatie-instellingen."
   7. Selecteer **OK**.

   > [!NOTE]
   > Wanneer een Azure-VM is de toewijzing ongedaan gemaakt en opnieuw hebt gestart, is er geen garantie dat het hetzelfde IP-adres krijgt. Als het IP-adres van de server-/ processerver voor configuratie of de beveiligde Azure-VM's wordt gewijzigd, werkt de replicatie in dit scenario mogelijk niet correct.

   ![Inschakelen van replicatie deelvenster met de bron geselecteerd][13]

Bij het ontwerpen van uw Azure Storage-omgeving, wordt u aangeraden dat u afzonderlijke storage-accounts voor elke virtuele machine in een beschikbaarheidsset gebruiken. We raden u aan de aanbevolen procedure in de storage-laag naar [gebruik meerdere opslagaccounts voor elke beschikbaarheidsset](../linux/manage-availability.md). Distributie van VM-schijven naar meerdere opslagaccounts voor het verbeteren van de beschikbaarheid en de i/o verdeeld over de infrastructuur van Azure storage.

Als uw VM's in een beschikbaarheidsset, in plaats van de schijven van alle virtuele machines repliceren naar een opslagaccount, sterk aangeraden meerdere keren voor meerdere virtuele machines migreren. Op die manier kunnen de virtuele machines in dezelfde beschikbaarheidsset deel niet één opslagaccount. Gebruik de **inschakelen replicatie** deelvenster kunt u een doel-opslagaccount instellen voor elke virtuele machine, één voor één.
 
U kunt een implementatiemodel na failover op basis van uw behoeften. Als u Azure Resource Manager als uw na een failover-implementatiemodel, kunt u een failover uitvoert voor een virtuele machine (Resource Manager) voor een VM (Resource Manager) of kunt u een failover uitvoert voor een virtuele machine (klassiek) met een virtuele machine (Resource Manager).

### <a name="step-8-run-a-test-failover"></a>Stap 8: Een testfailover uitvoeren

Als u wilt controleren of de replicatie voltooid is, selecteer uw exemplaar van Site Recovery en selecteer vervolgens **instellingen** > **gerepliceerde Items**. U ziet de status en het percentage van het replicatieproces. 

Na de initiële replicatie is voltooid, voert een testfailover uit voor het valideren van uw replicatiestrategie voor. Zie voor gedetailleerde stappen van een testfailover [een testfailover uitvoeren in Site Recovery](../../site-recovery/vmware-walkthrough-overview.md). 

> [!NOTE]
> Voordat u een failover uitvoert, controleert u uw VM's en replicatiestrategie voldoen aan de vereisten. Zie voor meer informatie over het uitvoeren van een testfailover [Testfailover naar Azure in Site Recovery](../../site-recovery/site-recovery-test-failover-to-azure.md).

U ziet de status van uw test-failover in **instellingen** > **taken** > *YOUR_FAILOVER_PLAN_NAME*. In het deelvenster ziet u een uitsplitsing van de stappen en geslaagde/mislukte resultaten. Als de testfailover is mislukt bij elke stap, selecteert u de stap om te controleren of het foutbericht. 

### <a name="step-9-run-a-failover"></a>Stap 9: Een failover uitvoeren

Nadat de test is failover voltooid, wordt een failover uitvoeren om te migreren van uw schijven naar Premium Storage en de VM-exemplaren repliceren. Volg de gedetailleerde stappen in [een failover uitvoeren](../../site-recovery/site-recovery-failover.md#run-a-failover). 

Zorg ervoor dat u selecteert **VM's af en de nieuwste gegevens synchroniseren**. Deze optie geeft aan dat Site Recovery beveiligde virtuele machines afsluiten en de gegevens te synchroniseren, zodat de meest recente versie van de gegevens wordt een failover moet proberen. Als u deze optie niet selecteert of als de poging mislukt, worden de failover van de meest recente beschikbare herstelpunt voor de virtuele machine. 

Site Recovery maakt u een VM-exemplaar waarvan het type hetzelfde als of vergelijkbaar met een Premium Storage-compatibele VM is. U kunt de prestaties en de prijs van verschillende VM-exemplaren controleren door te gaan naar [prijzen van virtuele Machines in Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) of [prijzen van Linux virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="post-migration-steps"></a>Stappen na de migratie

1. **Gerepliceerde VM's aan de beschikbaarheidsset indien van toepassing configureren**. Site Recovery biedt geen ondersteuning voor migratie van VM's, samen met de beschikbaarheidsset. Afhankelijk van de implementatie van uw gerepliceerde virtuele machine, moet u een van de volgende handelingen:
   * Voor een virtuele machine die zijn gemaakt via het klassieke implementatiemodel: De virtuele machine toevoegen aan de beschikbaarheidsset in de Azure-portal. Ga voor gedetailleerde stappen naar [een bestaande virtuele machine toevoegen aan een beschikbaarheidsset](../linux/classic/configure-availability-classic.md).
   * Voor een virtuele machine die via het Resource Manager-implementatiemodel zijn gemaakt: De configuratie van de virtuele machine op te slaan en vervolgens verwijderen en opnieuw maken van de virtuele machines in de beschikbaarheidsset. Om dit te doen, gebruikt u het script op [ingesteld Azure Resource Manager VM Availability Set](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4). Voordat u dit script uitvoert, controleert u de beperkingen en plan uw downtime.

2. **Verwijderen van oude VM's en schijven**. Zorg ervoor dat de Premium-schijven consistent met bronschijven zijn en dat de nieuwe VM's uit te dezelfde functie als de bron-VM's voeren. Verwijderen van de virtuele machine en verwijder de schijven van de bron-storage-accounts in Azure portal. Als er een probleem is in dat de schijf is niet verwijderd, zelfs als u de virtuele machine verwijderd, raadpleegt u [oplossen van fouten bij het verwijderen van het storage-resource](storage-resource-deletion-errors.md).

3. **Opschonen van de Azure Site Recovery-infrastructuur**. Als u Site Recovery is niet meer nodig hebt, kunt u de infrastructuur opschonen. Gerepliceerde items, de configuratieserver en het beleid voor herstel verwijderen en verwijder vervolgens de Azure Site Recovery-kluis.

## <a name="troubleshooting"></a>Problemen oplossen

* [Bewaken en problemen oplossen van beveiliging voor virtuele machines en fysieke servers](../../site-recovery/site-recovery-monitoring-and-troubleshooting.md)
* [Microsoft Azure Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)

## <a name="next-steps"></a>Volgende stappen

Zie voor specifieke scenario's voor migratie van virtuele machines, de volgende bronnen:

* [Azure virtuele Machines migreren tussen Opslagaccounts](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Maken en een Windows Server-VHD uploaden naar Azure](upload-generalized-managed.md)
* [Migreren van virtuele Machines van Amazon AWS naar Microsoft Azure](https://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Zie ook de volgende bronnen voor meer informatie over Azure Storage en Azure Virtual Machines:

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)

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
