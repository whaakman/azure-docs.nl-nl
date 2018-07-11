---
title: Een bestandsserver beveiligen met behulp van Azure Site Recovery
description: In dit artikel wordt beschreven hoe u een bestandsserver beveiligen met behulp van Azure Site Recovery
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2018
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: 0b6d5dccbce30c55e259e4bb3f8ae4194a02b646
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916880"
---
# <a name="protect-a-file-server-by-using-azure-site-recovery"></a>Een bestandsserver beveiligen met behulp van Azure Site Recovery 

[Azure Site Recovery](site-recovery-overview.md) draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel (BCDR) door te zorgen dat uw zakelijke apps actief blijven tijdens geplande en ongeplande uitval. Site Recovery beheert en orkestreert noodherstel van on-premises machines en virtuele Azure-machines (VM's). Herstel na noodgevallen omvat replicatie, failover en herstel met verschillende workloads.

In dit artikel wordt beschreven hoe u een bestandsserver beveiligen met behulp van Site Recovery en andere aanbevelingen op basis van verschillende omgevingen maakt. 

- [Azure IaaS-bestand server-machines repliceren](#disaster-recovery-recommendation-for-azure-iaas-virtual-machines)
- [Een on-premises bestandsserver repliceren met behulp van Site Recovery](#replicate-an-on-premises-file-server-by-using-site-recovery)

## <a name="file-server-architecture"></a>Bestand server-architectuur
Het doel van een open gedistribueerd systeem die delen van bestanden is voor een omgeving waar een groep geografisch verspreide gebruikers samenwerken kan en efficiënt werken aan bestanden worden gegarandeerd dat de vereisten van de integriteit worden afgedwongen. Een typische on-premises server ecosysteem die ondersteuning biedt voor een groot aantal gelijktijdige gebruikers en een groot aantal inhoudsitems maakt gebruik van Distributed File System Replication (DFSR) voor replicatie planning en bandbreedtebeperking. 

DFS-replicatie maakt gebruik van een compressiealgoritme bekend als externe differentiële compressie (RDC) die kan worden gebruikt voor het efficiënt updatebestanden via een netwerk met beperkte bandbreedte. Deze invoegingen, verwijderingen en of van de gegevens in bestanden gedetecteerd. DFS-replicatie is ingeschakeld voor het repliceren van alleen de gewijzigde bestandsblokken wanneer bestanden worden bijgewerkt. Er zijn ook bestand server-omgevingen, waarbij dagelijkse back-ups in niet-piekuren tijdsinstellingen, die is gericht op na noodgevallen moet worden genomen. DFS-replicatie is niet geïmplementeerd.

Het volgende diagram illustreert de file server-omgeving met DFSR geïmplementeerd.
                
![Architectuur van DFS-replicatie](media/site-recovery-file-server/dfsr-architecture.JPG)

In het vorige diagram deelnemen meerdere bestandsservers leden actief genoemd aan het repliceren van bestanden in een replicatiegroep. De inhoud in de gerepliceerde map zijn beschikbaar voor alle clients die aanvragen naar een van de leden verzenden, zelfs als een lid offline gaat.

## <a name="disaster-recovery-recommendations-for-file-servers"></a>Aanbevelingen van herstel na noodgevallen voor bestandsservers

* **Een bestandsserver met behulp van Site Recovery repliceert**: bestandsservers kunnen worden gerepliceerd naar Azure met behulp van Site Recovery. Wanneer een of meer on-premises bestandsservers niet toegankelijk zijn, kan het herstel van virtuele machines worden gebracht van in Azure. De virtuele machines kunnen vervolgens verzenden van aanvragen van clients, on-premises opgegeven er is een site-naar-site VPN-connectiviteit en Active Directory is geconfigureerd in Azure. U kunt deze methode in het geval van een DFS-replicatie geconfigureerd of een eenvoudig bestand server-omgeving gebruiken met geen DFS-replicatie. 

* **DFS-replicatie uitbreiden naar een Azure IaaS VM**: In een geclusterde bestandsserver serveromgeving met DFSR geïmplementeerd, kunt u de on-premises DFSR uitbreiden naar Azure. Een Azure-VM wordt ingeschakeld om uit te voeren van de rol van bestandsserver. 

    * Nadat de afhankelijkheden van de site-naar-site VPN-connectiviteit en Active Directory worden verwerkt en DFS-replicatie ingesteld, is wanneer een of meer on-premises bestandsservers niet toegankelijk zijn, kunnen clients verbinding met de Azure-VM, dat de aanvragen.

    * U kunt deze aanpak gebruiken als uw virtuele machines hebt configuraties die niet worden ondersteund door siteherstel. Een voorbeeld is een gedeelde clusterschijf, soms doorgaans gebruikt in file server-omgevingen. DFS-replicatie werkt ook goed in omgevingen met een lage bandbreedte met gemiddeld verloop. U moet rekening houden met de extra kosten van een Azure-VM die u en voortdurend. 

* **Azure File Sync gebruiken voor het repliceren van uw bestanden**: als u van plan bent de cloud al gebruiken of een Azure-VM, kunt u Azure File Sync. Azure File Sync biedt synchronisatie van volledig beheerde bestandsshares in de cloud die toegankelijk via het industriestandaard zijn [Server Message Block](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (SMB)-protocol. Azure-bestandsshares kunnen vervolgens gelijktijdig worden gekoppeld door on-premises of cloudimplementaties van Windows, Linux en macOS. 

Het volgende diagram kunt u bepalen welke strategie moet worden gebruikt voor uw serveromgeving bestand.

![Beslissingsstructuur](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-to-consider-in-your-decisions-about-disaster-recovery-to-azure"></a>Rekening houden in uw beslissingen te nemen over herstel na noodgevallen naar Azure

|Omgeving  |Aanbeveling  |Aandachtspunten |
|---------|---------|---------|
|Bestand serveromgeving met of zonder DFS-replicatie|   [Gebruik Site Recovery voor replicatie](#replicate-an-on-premises-file-server-by-using-site-recovery)   |    Site Recovery biedt geen ondersteuning voor gedeelde schijfclusters of NAS-opslag (NAS). Als uw omgeving deze configuraties gebruikt, gebruikt u een van de andere methoden, waar nodig. <br> Site Recovery biedt geen ondersteuning voor SMB 3.0. De gerepliceerde virtuele machine bevat wijzigingen alleen wanneer wijzigingen in de bestanden in de oorspronkelijke locatie van de bestanden worden bijgewerkt.
|Bestand-serveromgeving met DFS-replicatie     |  [DFS-replicatie uitbreiden naar een virtuele machine van Azure IaaS](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |      DFS-replicatie werkt goed in extreem bandbreedte crunched omgevingen. Deze aanpak vereist een Azure-VM of actief is en voortdurend. U moet rekening voor de kosten van de virtuele machine in uw planning.         |
|Azure IaaS VM     |     [File Sync ](#use-azure-file-sync-service-to-replicate-your-files)   |     Als u File Sync in een noodherstelscenario gebruikt, tijdens de failover moet u uitvoeren handmatige acties om ervoor te zorgen dat de bestandsshares op een transparante manier toegankelijk voor de clientcomputer zijn. File Sync poort 445 te openen vanaf de clientcomputer zijn vereist.     |


### <a name="site-recovery-support"></a>Ondersteuning voor site Recovery
Omdat de Site Recovery-replicatie is neutraal van toepassing, worden deze aanbevelingen verwacht voor het opslaan van de waarde true voor de volgende scenario's.
| Bron    |Naar een secundaire site    |Naar Azure
|---------|---------|---------|
|Azure| -|Ja|
|Hyper-V|   Ja |Ja
|VMware |Ja|   Ja
|Fysieke server|   Ja |Ja
 

> [!IMPORTANT]
> Voordat u met een van de volgende drie methoden doorgaat, ervoor zorgen dat deze afhankelijkheden worden afgehandeld.

**Site-naar-site-connectiviteit**: een rechtstreekse verbinding tussen de on-premises site en het Azure-netwerk moet tot stand worden gebracht voor de communicatie tussen servers. Gebruik een beveiligde site-naar-site VPN-verbinding met een Azure-netwerk dat wordt gebruikt als de site voor noodherstel. Zie voor meer informatie, [een site-naar-site VPN-verbinding tussen een on-premises site en een Azure-netwerk](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal).

**Active Directory**: DFS-replicatie, is afhankelijk van Active Directory. Dit betekent dat de Active Directory-forest met lokale domeincontrollers wordt uitgebreid naar de site voor noodherstel in Azure. Zelfs als u niet DFSR, gebruikt als de beoogde gebruikers moeten worden gemachtigd of geverifieerd om toegang te krijgen, moet u deze stappen uitvoeren. Zie voor meer informatie, [uitbreiden on-premises Active Directory naar Azure](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendation-for-azure-iaas-virtual-machines"></a>Aanbeveling van herstel na noodgevallen voor Azure IaaS virtuele machines

Als u configureert en herstel na noodgevallen van bestandsservers die worden gehost op Azure IaaS VM's beheren, u tussen twee opties kiezen kunt, afhankelijk van of u verplaatsen wilt naar [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction):

* [File Sync gebruiken](#use-file-sync-to-replicate-files-hosted-on-an-iaas-virtual-machine)
* [Gebruik Site Recovery](#replicate-an-iaas-file-server-virtual-machine-by-using-site-recovery)

## <a name="use-file-sync-to-replicate-files-hosted-on-an-iaas-virtual-machine"></a>File Sync gebruiken voor het repliceren van bestanden die worden gehost op een IaaS-machine

Azure Files kan worden gebruikt om bestandsshares op traditionele on-premises bestandsservers of NAS-apparaten volledig te vervangen of aan te vullen. Azure-bestandsshares kunnen ook worden gerepliceerd met File Sync met Windows-servers, on-premises of in de cloud, prestaties en gedistribueerd cachegebruik van de gegevens waar deze wordt gebruikt. De volgende stappen beschrijven de aanbeveling van het herstel na noodgevallen voor Azure-VM's die dezelfde functionaliteit als traditionele bestandsservers uitvoeren:
* Machines beveiligen met behulp van Site Recovery. Volg de stappen in [een Azure-VM repliceren naar een andere Azure-regio](azure-to-azure-quickstart.md).
* File Sync gebruiken voor het repliceren van bestanden van de virtuele machine die als de server naar de cloud fungeert.
* Gebruik de Site Recovery [herstelplan](site-recovery-create-recovery-plans.md) functie voor het toevoegen van scripts [de Azure-bestandsshare koppelen](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) en de share openen in uw virtuele machine.

Korte beschrijving van de volgende stappen uit het gebruik van File Sync:

1. [Een storage-account maken in Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Als u geografisch redundante opslag met leestoegang voor uw storage-accounts hebt gekozen, ontvangt u leestoegang tot uw gegevens van de secundaire regio in geval van een ramp. Zie voor meer informatie, [strategieën voor noodherstel Azure file share](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
2. [Een bestandsshare maken](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [File Sync Start](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) op uw Azure-bestandsserver.
4. Maak een synchronisatiegroep. Eindpunten in een synchronisatiegroep worden synchroon met elkaar. Een groep voor synchronisatie moet ten minste één cloudeindpunt, waarmee een Azure-bestandsshare bevatten. Een groep voor synchronisatie moet ook een servereindpunt, waarmee een pad op een Windows-server bevatten.
5. Uw bestanden worden nu gesynchroniseerd gehouden in uw Azure-bestandsshare en uw on-premises server.
6. In het geval van een ramp in uw on-premises-omgeving, kunt u een failover uitvoeren met behulp van een [herstelplan](site-recovery-create-recovery-plans.md). Het script toevoegen [de Azure-bestandsshare koppelen](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) en de share openen in uw virtuele machine.

### <a name="replicate-an-iaas-file-server-virtual-machine-by-using-site-recovery"></a>Een virtuele machine voor IaaS-bestand server repliceren met behulp van Site Recovery

Als u on-premises clients die toegang hebben tot de server-machine voor IaaS-bestand hebt, kunt u alle van de volgende stappen uitvoeren. Anders gaat u naar stap 3.

1. Een site-naar-site VPN-verbinding tussen de on-premises site en het Azure-netwerk.
2. On-premises Active Directory uitbreiden.
3. [Herstel na noodgevallen instellen](azure-to-azure-tutorial-enable-replication.md) voor de servercomputer van de IaaS-bestand naar een secundaire regio.


Zie voor meer informatie over herstel na noodgevallen naar een secundaire regio [in dit artikel](concepts-azure-to-azure-architecture.md).


## <a name="replicate-an-on-premises-file-server-by-using-site-recovery"></a>Een on-premises bestandsserver repliceren met behulp van Site Recovery

De volgende stappen beschrijven de replicatie voor een VMware-VM. Zie voor stappen voor het repliceren van een Hyper-V-VM [in deze zelfstudie](tutorial-hyper-v-to-azure.md).

1. [Azure-resources voorbereiden](tutorial-prepare-azure.md) voor replicatie van on-premises machines.
2. Een site-naar-site VPN-verbinding tussen de on-premises site en het Azure-netwerk. 
3. On-premises Active Directory uitbreiden.
4. [On-premises VMware-servers voorbereiden](tutorial-prepare-on-premises-vmware.md).
5. [Herstel na noodgevallen instellen](tutorial-vmware-to-azure.md) naar Azure voor on-premises VM's.

## <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>DFS-replicatie uitbreiden naar een virtuele machine van Azure IaaS

1. Een site-naar-site VPN-verbinding tussen de on-premises site en het Azure-netwerk. 
2. On-premises Active Directory uitbreiden.
3. [Maken en inrichten van een bestandsserver VM](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) op Azure virtual network.
Zorg ervoor dat de virtuele machine met de dezelfde Azure-netwerk met cross-connectiviteit met de on-premises omgeving wordt toegevoegd. 
4. Installeren en [DFS-replicatie configureren](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) op Windows Server.
5. [Implementeren van een DFS-naamruimte](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).
6. Met de DFS-naamruimte wordt geïmplementeerd, kan de failover van gedeelde mappen van de productie disaster recovery sites worden gedaan door het bijwerken van de DFS-naamruimte-mapdoelen. Na deze DFS namespace-wijzigingen repliceren via Active Directory gebruikers zijn verbonden met de juiste mapdoelen transparant.

## <a name="use-file-sync-to-replicate-your-on-premises-files"></a>File Sync gebruiken voor het repliceren van uw on-premises-bestanden
File Sync kunt u bestanden repliceren naar de cloud. U kunt de gewenste bestandslocaties vanuit de cloud koppelen en doorgaan aanvragen van clientcomputers in het geval van een ramp en het ontbreken van uw on-premises bestandsserver.
File Sync integreren met Site Recovery:

* De file server-machines beveiligen met behulp van Site Recovery. Volg de stappen in [in deze zelfstudie](tutorial-vmware-to-azure.md).
* File Sync gebruiken voor het repliceren van bestanden van de computer die als een bestandsserver naar de cloud fungeert.
* Gebruik de functie voor herstel plan in Site Recovery scripts voor het koppelen van de Azure-bestandsshare op de bestandsserver voor failover-VM in Azure wilt toevoegen.

Volg deze stappen voor het gebruik van File Sync:

1. [Een storage-account maken in Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Als u geografisch redundante opslag met leestoegang (aanbevolen) voor uw storage-accounts hebt gekozen, hebt u leestoegang tot uw gegevens van de secundaire regio in geval van een ramp. Zie voor meer informatie, [strategieën voor noodherstel Azure file share](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
2. [Een bestandsshare maken](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [File Sync implementeren](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) in uw on-premises bestandsserver.
4. Maak een synchronisatiegroep. Eindpunten in een synchronisatiegroep worden synchroon met elkaar. Een groep voor synchronisatie moet ten minste één cloudeindpunt, waarmee een Azure-bestandsshare bevatten. De groep voor synchronisatie moet ook een servereindpunt, waarmee een pad op de on-premises Windows server bevatten.
5. Uw bestanden worden nu gesynchroniseerd gehouden in uw Azure-bestandsshare en uw on-premises server.
6. In het geval van een ramp in uw on-premises-omgeving, kunt u een failover uitvoeren met behulp van een [herstelplan](site-recovery-create-recovery-plans.md). Voeg het script voor het koppelen van de Azure-bestandsshare en de share openen in uw virtuele machine toe.

> [!NOTE]
> Zorg ervoor dat poort 445 geopend is. Azure Files maakt gebruik van het SMB-protocol. SMB communiceert via TCP-poort 445. Controleer als TCP-poort 445 vanaf een clientcomputer wordt niet geblokkeerd door uw firewall.


## <a name="do-a-test-failover"></a>Voer een failovertest uit

1. Ga naar de Azure-portal en selecteert u de Recovery Services-kluis.
2. Selecteer het herstelplan te gaan voor de server-omgeving van het bestand gemaakt.
3. Selecteer **Testfailover**.
4. Selecteer het herstelpunt en het Azure-netwerk om de test-failover-proces te starten.
5. Nadat de secundaire-omgeving is, voert u uw bevestigingen.
6. Nadat de controles zijn voltooid, selecteert u **failovertest** op het herstelplan te gaan en de testfailover omgeving is opgeschoond.

Zie voor meer informatie over het uitvoeren van een testfailover [failovertest met Site Recovery](site-recovery-test-failover-to-azure.md).

Zie voor instructies over het testfailover uitvoert voor Active Directory en DNS, [Testfailover-overwegingen voor Active Directory en DNS](site-recovery-active-directory.md).

## <a name="do-a-failover"></a>Voer een failover

1. Ga naar de Azure-portal en selecteert u de Recovery Services-kluis.
2. Selecteer het herstelplan te gaan voor de server-omgeving van het bestand gemaakt.
3. Selecteer **Failover**.
4. Selecteer het herstelpunt dat om de failoverproces te starten.

Zie voor meer informatie over het uitvoeren van een failover [Failover in Site Recovery](site-recovery-failover.md).
