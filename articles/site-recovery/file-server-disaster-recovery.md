---
title: Een bestandsserver met Azure Site Recovery beveiligen
description: Dit artikel wordt beschreven hoe u een bestandsserver met Azure Site Recovery beveiligen
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2017
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: ac734ffc6cb57188b7b0959cbe7655949b2853de
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="protect-a-file-server-using-azure-site-recovery"></a>Een bestandsserver met Azure Site Recovery beveiligen 

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel herstel (BCDR) door uw business-apps te houden beschikbaar tijdens geplande en ongeplande uitval. Site Recovery beheert en noodherstel van on-premises machines en virtuele Azure-machines (VM's), met inbegrip van replicatie, failovers en herstel van verschillende workloads ingedeeld.

In dit artikel wordt beschreven hoe een bestandsserver die aan de behoeften van verschillende omgevingen met behulp van Azure Site Recovery en andere aanbevelingen beveiligen.     

- [Azure IaaS-bestand server-machines repliceren](#disaster-recovery-recommendation-for-azure-iaas-virtual-machines)
- [Replicatie van de lokale bestandsserver met Azure Site Recovery](#replicate-an-onpremises-file-server-using-azure-site-recovery)

## <a name="file-server-architecture"></a>File Server-architectuur
Het doel van een open gedistribueerde bestandsdeling system is voor een omgeving waar een groep geografisch verspreide gebruikers samenwerken kan om efficiënt te werken aan bestanden en worden gegarandeerd dat de vereisten voor integriteit worden afgedwongen. Een typische lokale bestandsserver ecosysteem die ondersteuning biedt voor een groot aantal gelijktijdige gebruikers en een groot aantal inhoudsitems Distributed File System Replication (DFSR) voor replicatie planning en bandbreedtebeperking gebruiken DFSR maakt gebruik van een compressiealgoritme, ook bekend als Remote Differential Compression (RDC), die kunnen worden gebruikt voor het efficiënt updatebestanden via een netwerk met beperkte bandbreedte. Er wordt gedetecteerd invoegingen, verwijderingen en of van gegevens in bestanden inschakelen DFSR gerepliceerd alleen de gewijzigde bestandsblokken wanneer bestanden worden bijgewerkt. Er zijn ook bestandsserver omgevingen, waarbij de dagelijkse back-ups worden uitgevoerd in niet-piekuren tijdsinstellingen die geschikt zijn voor noodherstel behoeften en er is geen implementatie van DFSR.

De volgende topologie ziet u de bestandsserver-omgeving met DFSR geïmplementeerd.
                
![DFSR architexture](media/site-recovery-file-server/dfsr-architecture.JPG)

In de bovenstaande verwijzing meerdere bestandsservers aangeduid als leden, actief deelnemen aan het repliceren van bestanden via een replicatiegroep. De inhoud in de gerepliceerde map zijn beschikbaar op alle clients die aanvragen verzenden naar een van de leden, zelfs in geval van een van de leden gaat offline.

## <a name="disaster-recovery-recommendation-for-file-servers"></a>Aanbeveling van herstel na noodgevallen voor bestandsservers:

1.  Een bestandsserver met Azure Site Recovery worden gerepliceerd: de bestandsservers kunnen worden gerepliceerd naar Azure met Azure Site Recovery. Wanneer u een of meer van de bestand servers on-premises niet toegankelijk is, het herstel van virtuele machines kan worden gebracht van in Azure, dit kan vervolgens dienst aanvragen van clients, de on-premises opgegeven er Site-naar-Site VPN-verbinding en Active directory geconfigureerd in Azure. Dit kan worden gedaan in geval van een omgeving DFSR geconfigureerd of een eenvoudige bestand serveromgeving met geen DFSR. 

2.  DFSR uitbreiden naar een virtuele machine van Azure Iaas:-In een geclusterde bestandsserver-serveromgeving met DFSR geïmplementeerd een voorgestelde benadering bestaat uit de lokale DFSR uitbreiden naar Azure. Azure een virtuele machine wordt ingeschakeld om uit te voeren van de bestandsserverrol. 

    Zodra de afhankelijkheden van de Site-naar-Site VPN-verbinding en Active directory worden verwerkt en DFSR geïmplementeerd, is wanneer een of meer van de bestand servers on-premises niet toegankelijk is, de clients kunnen nog steeds verbinding maken met de Azure VM die zal de aanvragen verwerken.

    Deze aanpak wordt voorgesteld in geval uw virtuele machines hebben configuraties die niet worden ondersteund door Azure Site Recovery, zoals bijvoorbeeld: gedeelde clusterschijf waarop soms meestal wordt gebruikt in omgevingen van de bestandsserver.  DFSR werkt ook goed in omgevingen met gemiddeld verloop lage bandbreedte. De extra kosten van een virtuele machine van Azure en voortdurend moet ook worden aangepast met deze.  

3.  Azure File-Sync-service gebruiken om te repliceren van uw bestanden: als u voorbereidt voor uw reis naar de cloud of een Azure VM al gebruikt, moet we raden het gebruik van Azure File sync-service biedt synchroniseren van een volledig beheerde bestandsshares in de cloud die toegankelijk v IA de industriestandaard [Server Message Block ](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)protocol (SMB). Azure File shares vervolgens worden gekoppeld, gelijktijdig door cloud of on-premises implementaties van Windows, Linux en Mac OS. 

Volgende diagram biedt een grafisch weergegeven die gericht is op versnelling van de beslissing over welke strategie moet worden gebruikt voor uw serveromgeving bestand.

![decisiontree](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-to-consider-while-making-decision-of-disaster-recovery-to-azure"></a>Factoren tijdens het maken van de beslissing over noodherstel naar Azure

|Omgeving  |Aanbeveling  |Aandachtspunten |
|---------|---------|---------|
|Server-omgeving met/zonder DFSR bestand|   [Azure Site Recovery gebruiken voor replicatie](#replicate-an-onpremises-file-servers-using-azure-site-recovery)   |    Site Recovery biedt geen ondersteuning voor gedeelde schijfcluster kan NAS. Als een van deze configuraties wordt gebruikt in uw omgeving, kunt u een van de andere methoden zo nodig gebruiken. <br> Azure Site Recovery biedt geen ondersteuning voor SMB 3.0, wat betekent dat alleen wanneer de wijzigingen in de bestanden worden bijgewerkt in de oorspronkelijke locatie van het bestand de gerepliceerde virtuele machine de wijzigingen zodat wordt.
|Bestand-serveromgeving met DFSR     |  [DFSR uitbreiden naar een virtuele machine van Azure IaaS:](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |     DFSR werkt goed in extreem bandbreedte crunched omgevingen, maar hiervoor zijn een Azure VM up en voortdurend. De kosten van de virtuele machine moet worden gehouden in de planning.         |
|Azure Iaas VM     |     [Azure File-synchronisatie](#use-azure-file-sync-service-to-replicate-your-files)   |     In een herstel na noodgevallen als u van Azure File-Sync gebruikmaakt, moeten tijdens de failover handmatige acties worden ondernomen om ervoor te zorgen dat de bestandsshares als toegankelijk is op een transparante manier naar de clientcomputer. AFS poort 445 te openen vanaf de clientcomputer zijn vereist.     |


### <a name="site-recovery-support"></a>Site Recovery-ondersteuning
Als de replicatie van Site Recovery is networkdirect toepassing, de aanbevelingen die naar verwachting geldt voor de volgende scenario's:
| Bron    |Een secundaire site    |Naar Azure
|---------|---------|---------|
|Azure| -|Ja|
|Hyper-V|   Ja |Ja
|VMware |Ja|   Ja
|Fysieke server|   Ja |Ja
 

> [!IMPORTANT]
> Voordat u met een van verdergaat de onderstaande drie methoden ervoor te zorgen dat de volgende afhankelijkheden worden afgehandeld:

**Site-naar-Site-connectiviteit**: directe verbinding tussen de lokale site en de Azure-netwerk moet worden gemaakt voor de communicatie tussen servers.  Dit kan worden gegarandeerd door een veilige Site-naar-Site VPN-verbinding met een Microsoft Azure-netwerk dat wordt gebruikt als de DR-site.  
Raadpleeg: [maken Site-naar-Site VPN-verbinding tussen on-premises site en Azure-netwerk](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

**Active Directory**: DFSR is afhankelijk van Active Directory.  Dit betekent dat de Active Directory-forest met lokale domeincontrollers wordt uitgebreid naar de DR-site in Azure. Zelfs als u DFSR niet gebruikt als de beoogde gebruikers moeten worden verleend of geverifieerd voor toegang zoals in de meeste organisaties, moeten deze stappen worden uitgevoerd.
Raadpleeg: [on-premises Active Directory naar Azure uitbreiden](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendation-for-azure-iaas-virtual-machines"></a>Aanbeveling van herstel na noodgevallen voor Azure IaaS virtuele machines

Als u configureren wilt en beheren van herstel na noodgevallen van bestandsservers gehost op Azure IaaS VM's u uit twee opties kiezen kunt, op basis van of u verplaatsen wilt naar [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).

1. [Azure File Sync gebruiken](#use-azure-file-sync-service-to-replicate-files-hosted-on-iaas-virtual-machine)
2. [Azure Site Recovery gebruiken](#replicate-an-iaas-file-server-virtual-machine-using-azure-site-recovery)

## <a name="use-azure-file-sync-service-to-replicate-files-hosted-on-iaas-virtual-machine"></a>Gebruik Azure bestand Sync-service voor replicatie van bestanden die worden gehost op IaaS virtuele machine

**Azure Files** kan worden gebruikt om volledig vervangen of vormen een aanvulling op de bestandsservers traditionele on-premises of NAS-apparaten. Azure-bestandsshares kunnen ook met Azure-bestandssynchronisatie worden gerepliceerd naar Windows-Servers, on-premises of in de cloud, voor een krachtige en gedistribueerde opslag in de cache van gegevens waar deze worden gebruikt. Volgende stappen wordt beschreven de aanbeveling Noodherstel voor de Azure VM's die dezelfde functionaliteit als traditionele bestandsservers uitvoeren:
1.  Beveiligen met Azure Site Recovery met behulp van stappen genoemde machines [hier](azure-to-azure-quickstart.md).
2.  Azure bestand Sync gebruiken om te repliceren van bestanden van de virtuele machine die als de bestandsserver, naar de cloud fungeert.
3.  Gebruik van Azure Site Recovery [herstelplan](site-recovery-create-recovery-plans.md) functie scripts toevoegen [de Azure-bestandsshare koppelen](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) en toegang tot de share in de virtuele machine.

De onderstaande stappen beschreven kort hoe u Azure File-Sync-service:

1. [Een opslagaccount maken in Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Als u geografisch redundante opslag met leestoegang (RA-GRS) voor uw storage-accounts hebt gekozen, krijgt u leestoegang tot uw gegevens van de secundaire regio als er een ramp optreedt. Raadpleeg de [Azure File share disaster recovery strategieën](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor meer informatie.
2. [Een bestandsshare maken](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Azure File-synchronisatie implementeren](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) op uw Azure-bestandsserver.
4. Maken van een groep voor synchronisatie: eindpunten in een groep voor synchronisatie synchroon met elkaar worden behouden. Een groep voor synchronisatie moet ten minste één Cloudeindpunt dat vertegenwoordigt een Azure-bestandsshare, en één servereindpunt waarmee een pad op een Windows-Server bevatten.
5.  Uw bestanden wordt nu gesynchroniseerd blijven in uw Azure-bestandsshare en de lokale server.
6.  Uitvoeren in het geval van een ramp in uw on-premises-omgeving, als het gebruik van de failover een [herstelplan](site-recovery-create-recovery-plans.md) en voeg het script [de Azure-bestandsshare koppelen](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) en toegang tot de share in de virtuele machine.

### <a name="replicate-an-iaas-file-server-virtual-machine-using-azure-site-recovery"></a>Een IaaS-bestand server virtuele machine met Azure Site Recovery repliceren

Als u lokale clients toegang tot het uitvoeren van de virtuele machine van bestand IaaS hebt u de eerste twee stappen, anders verder met stap 3.

1. Site-naar-Site VPN-verbinding tussen on-premises site en Azure-netwerk maken.
1. On-premises Active Directory uitbreiden.
1. [Instellen van herstel na noodgevallen](azure-to-azure-tutorial-enable-replication.md) voor de servercomputer van IaaS-bestand naar een secundaire regio.


Raadpleeg voor meer informatie over herstel na noodgevallen in een secundaire regio [hier](concepts-azure-to-azure-architecture.md).


## <a name="replicate-an-on-premises-file-server-using-azure-site-recovery"></a>Een lokale bestandsserver met Azure Site Recovery repliceren

De onderstaande stappen detail replicatie voor een VM VMware voor stappen voor het repliceren van een Hyper-V-machine verwijzen [hier](tutorial-hyper-v-to-azure.md).

1.  [Voorbereiden van de Azure-resources](tutorial-prepare-azure.md) voor replicatie van machines lokale.
2.  Site-naar-Site VPN-verbinding tussen on-premises site en Azure-netwerk maken.  
3. On-premises Active Directory uitbreiden.
4.  [On-premises VMware-servers voorbereiden](tutorial-prepare-on-premises-vmware.md).
5.  [Instellen van herstel na noodgevallen](tutorial-vmware-to-azure.md) naar Azure voor lokale virtuele machines.

## <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>DFSR uitbreiden naar een virtuele machine van Azure IaaS:

1.  Site-naar-Site VPN-verbinding tussen on-premises site en Azure-netwerk maken. 
2.  On-premises Active Directory uitbreiden.
3.  [Maken en inrichten van een bestandsserver VM](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) op de Windows Azure Virtual Network.
Zorg ervoor dat de virtuele machine is toegevoegd aan het dezelfde Windows Azure virtuele netwerk, waardoor de cross verbinding met de on-premises omgeving heeft. 
4.  Installeren en [DFS-replicatie configureren](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) op Windows Server.
5.  [Implementeren van een DFS-Namespace](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).
6.  Met de DFS Namespace geïmplementeerd, kan de failover van gedeelde mappen van de productie DR sites worden gedaan door de DFS Namespace-mapdoelen bij te werken.  Zodra deze wijzigingen van de DFS Namespace via Active Directory repliceren, zijn gebruikers verbonden met de juiste mapdoelen transparant.

## <a name="use-azure-file-sync-service-to-replicate-your-on-premises-files"></a>Gebruik Azure bestand Sync-service voor replicatie van uw lokale bestanden:
Met de Azure-bestand Sync-service, kunt u repliceren de gewenste bestanden in de cloud, zodat in het geval van een noodherstel en het ontbreken van de lokale bestandsserver, kunt u de gewenste bestandslocaties vanuit de cloud te koppelen en serviceaanvragen van blijven de clientcomputers.
De voorgestelde benadering van de integratie van Azure File-synchronisatie met Azure Site Recovery is
1.  De file server-machines met Azure Site Recovery met behulp van stappen vermeld beveiligen [hier](tutorial-vmware-to-azure.md).
2.  Azure bestand Sync gebruiken om te repliceren van bestanden van de computer die als een bestandsserver, naar de cloud fungeert.
3.  Gebruik van Azure Site Recovery herstel plan functie toevoegen scripts voor het koppelen van de Azure-bestandsshare op de mislukte via FileServer VM in Azure.

De onderstaande stappen detail controleren met behulp van Azure bestand Sync-service:

1. [Een opslagaccount maken in Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Als u hebt gekozen geografisch redundante opslag met leestoegang (RA-GRS) voor uw storage-accounts (aanbevolen), hebt u leestoegang tot uw gegevens van de secundaire regio als er een ramp optreedt. Raadpleeg de [Azure File share disaster recovery strategieën](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor meer informatie.
2. [Een bestandsshare maken](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Azure File-synchronisatie implementeren](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) in uw lokale bestandsserver.
4. Maken van een groep voor synchronisatie: eindpunten in een groep voor synchronisatie synchroon met elkaar worden behouden. Een groep voor synchronisatie moet ten minste één Cloudeindpunt dat vertegenwoordigt een Azure-bestandsshare, en één servereindpunt waarmee een pad op de lokale Windows-Server bevatten.
1. Uw bestanden wordt nu gesynchroniseerd blijven in uw Azure-bestandsshare en de lokale server.
6.  In het geval van een noodgeval in uw on-premises omgeving uitvoeren als het gebruik van de failover een [herstelplan](site-recovery-create-recovery-plans.md) en het script voor het koppelen van de Azure-bestandsshare en toegang tot de share in de virtuele machine toe te voegen.

> [!NOTE]
> Zorg ervoor dat poort 445 is geopend: Azure Files SMB-protocol gebruikt. SMB communiceert via TCP-poort 445 - controleer of de TCP-poort 445 van de clientcomputer niet door uw firewall wordt geblokkeerd.


## <a name="doing-a-test-failover"></a>Een testfailover uitvoeren

1.  Ga naar de Azure-portal en selecteer de Recovery Services-kluis.
2.  Klik op het herstelplan gemaakt voor de bestandsserver-omgeving.
3.  Klik op 'Testfailover'.
4.  Selecteer het herstelpunt en virtuele Azure-netwerk om de test failover-proces te starten.
5.  Als de secundaire-omgeving is, kunt u uw validaties kunt uitvoeren.
6.  Als de validaties voltooid zijn, kunt u 'Opschonen testfailover' op het herstelplan en de testfailoveromgeving is opgeschoond.

Raadpleeg voor meer informatie over het uitvoeren van de testfailover [hier](site-recovery-test-failover-to-azure.md).

Voor hulp bij de testfailover uitvoeren voor AD en DNS, verwijzen naar [testfailover-overwegingen voor AD- en DNS-](site-recovery-active-directory.md).

## <a name="doing-a-failover"></a>U een failover uitvoert

1.  Ga naar de Azure-portal en selecteer de Recovery Services-kluis.
2.  Klik op het herstelplan gemaakt voor de bestandsserver-omgeving.
3.  Klik op 'Failover'.
4.  Selecteer het herstelpunt om de failoverproces te starten.

Raadpleeg voor meer informatie over het uitvoeren van een failover [hier](site-recovery-failover.md).
