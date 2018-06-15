---
title: Een bestandsserver beveiligen met behulp van Azure Site Recovery
description: Dit artikel wordt beschreven hoe u een bestandsserver beveiligen met behulp van Azure Site Recovery
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: 830f9c76d9d1bf11692fa9f2f5c49cbecdb69f25
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
ms.locfileid: "30266186"
---
# <a name="protect-a-file-server-by-using-azure-site-recovery"></a>Een bestandsserver beveiligen met behulp van Azure Site Recovery 

[Azure Site Recovery](site-recovery-overview.md) draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel (BCDR) door te zorgen dat uw zakelijke apps actief blijven tijdens geplande en ongeplande uitval. Site Recovery beheert en herstel na noodgevallen van on-premises machines en virtuele Azure-machines (VM's) ingedeeld. Herstel na noodgevallen omvat replicatie, failovers en herstel van verschillende workloads.

In dit artikel wordt beschreven hoe u een bestandsserver beveiligen met behulp van Site Recovery en andere aanbevelingen op basis van verschillende omgevingen maakt. 

- [Azure IaaS-bestand server-machines repliceren](#disaster-recovery-recommendation-for-azure-iaas-virtual-machines)
- [Een on-premises bestand-server repliceren met behulp van Site Recovery](#replicate-an-on-premises-file-server-by-using-site-recovery)

## <a name="file-server-architecture"></a>File server-architectuur
Het doel van een open gedistribueerde systemen die delen van bestanden is voor een omgeving waar een groep geografisch verspreide gebruikers samenwerken kan efficiënt werken op bestanden en worden gegarandeerd dat de vereisten voor integriteit worden afgedwongen. Een typische lokale server ecosysteem die ondersteuning biedt voor een groot aantal gelijktijdige gebruikers en een groot aantal inhoudsitems maakt gebruik van Distributed File System Replication (DFSR) voor replicatie planning en bandbreedtebeperking. 

DFSR maakt gebruik van een compressiealgoritme, ook bekend als Remote Differential Compression (RDC) die kan worden gebruikt om efficiënt updatebestanden via een netwerk met beperkte bandbreedte. Deze detecteert invoegingen, verwijderingen en of van gegevens in bestanden. DFSR is ingeschakeld voor replicatie van alleen de gewijzigde bestandsblokken wanneer bestanden worden bijgewerkt. Er zijn ook serveromgevingen van het bestand waar dagelijkse back-ups worden uitgevoerd in niet-piekuren tijdsinstellingen die geschikt zijn voor noodherstel behoeften. DFSR is niet geïmplementeerd.

Het volgende diagram illustreert de file server-omgeving met DFSR geïmplementeerd.
                
![DFSR-architectuur](media/site-recovery-file-server/dfsr-architecture.JPG)

In het vorige diagram deelnemen meerdere bestandsservers leden actief genoemd aan het repliceren van bestanden via een replicatiegroep. De inhoud in de gerepliceerde map zijn beschikbaar voor alle clients die aanvragen naar een van de leden verzenden, zelfs als een lid offline gaat.

## <a name="disaster-recovery-recommendations-for-file-servers"></a>Disaster recovery aanbevelingen voor bestandsservers

* **Een bestandsserver repliceren met behulp van Site Recovery**: bestandsservers kunnen worden gerepliceerd naar Azure met behulp van Site Recovery. Wanneer een of meer lokale bestandsservers niet toegankelijk zijn, kan het herstel van virtuele machines in Azure up worden gebracht. De virtuele machines kunnen aanvragen van clients fungeren, on-premises opgegeven er site-naar-site VPN-verbinding en Active Directory is geconfigureerd in Azure. U kunt deze methode in het geval van een DFSR geconfigureerd of een eenvoudige bestand server-omgeving gebruiken met geen DFSR. 

* **DFSR uitbreiden naar een virtuele machine van Azure IaaS**: In een geclusterde bestandsserver-serveromgeving met DFSR geïmplementeerd, kunt u de lokale DFSR uitbreiden naar Azure. Een Azure-virtuele machine wordt ingeschakeld om uit te voeren van de bestandsserverrol. 

    * Nadat de afhankelijkheden van de site-naar-site VPN-verbinding en Active Directory worden verwerkt en DFSR geïmplementeerd, is wanneer een of meer lokale bestandsservers niet toegankelijk zijn, kunnen clients verbinding met de Azure VM, die de aanvragen fungeert.

    * U kunt deze methode als uw virtuele machines hebt configuraties die worden niet ondersteund door Site Recovery. Een voorbeeld is een gedeelde clusterschijf waarop soms meestal wordt gebruikt in serveromgevingen bestand. DFSR werkt ook goed in omgevingen met gemiddeld verloop lage bandbreedte. U moet rekening houden met de extra kosten van een virtuele machine van Azure en voortdurend. 

* **Azure bestand Sync gebruiken voor replicatie van uw bestanden**: als u wilt gebruikmaken van de cloud of al een virtuele machine in Azure, kunt u Azure File-synchronisatie. Azure File-synchronisatie biedt synchroniseren van volledig beheerde bestandsshares in de cloud die toegankelijk via de industriestandaard zijn [Server Message Block](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) protocol (SMB). Azure-bestandsshares kunnen vervolgens gelijktijdig worden geladen in de cloud of on-premises implementaties van Windows, Linux en Mac OS. 

Het volgende diagram kunt u bepalen welke strategie moet worden gebruikt voor uw serveromgeving bestand.

![beslissingsstructuur](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-to-consider-in-your-decisions-about-disaster-recovery-to-azure"></a>Factoren in uw besluiten over noodherstel naar Azure

|Omgeving  |Aanbeveling  |Aandachtspunten |
|---------|---------|---------|
|Bestand-serveromgeving met of zonder DFSR|   [Site Recovery gebruiken voor replicatie](#replicate-an-on-premises-file-server-by-using-site-recovery)   |    Site Recovery biedt geen ondersteuning voor gedeelde schijfclusters of NAS-opslag (NAS). Als uw omgeving deze configuraties gebruikt, gebruikt u een van de andere methoden naar gelang van toepassing. <br> Site Recovery biedt geen ondersteuning voor SMB 3.0. De gerepliceerde virtuele machine bevat wijzigingen alleen als er wijzigingen aangebracht in de bestanden in de oorspronkelijke locatie van de bestanden zijn bijgewerkt.
|Bestand-serveromgeving met DFSR     |  [DFSR uitbreiden naar een virtuele machine van Azure IaaS](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |      DFSR werkt goed in extreem bandbreedte crunched omgevingen. Deze aanpak is vereist voor een virtuele machine in Azure of actief is en voortdurend. U moet het account voor de kosten van de virtuele machine in de planning.         |
|Azure IaaS VM     |     [Bestand-synchronisatie ](#use-azure-file-sync-service-to-replicate-your-files)   |     Als u synchronisatie-bestand in een noodherstelscenario gebruikt, tijdens de failover moet u uitvoeren handmatige acties om ervoor te zorgen dat de bestandsshares toegankelijk voor de clientcomputer op transparante wijze zijn. Bestand synchronisatie vereist poort 445 geopend vanaf de clientcomputer.     |


### <a name="site-recovery-support"></a>Site Recovery-ondersteuning
Omdat replicatie van Site Recovery networkdirect van toepassing is, worden deze aanbevelingen voor het opslaan van de volgende scenario's waar verwacht.
| Bron    |Een secundaire site    |Naar Azure
|---------|---------|---------|
|Azure| -|Ja|
|Hyper-V|   Ja |Ja
|VMware |Ja|   Ja
|Fysieke server|   Ja |Ja
 

> [!IMPORTANT]
> Voordat u met een van de volgende drie methoden doorgaat, ervoor zorgen dat deze afhankelijkheden worden afgehandeld.

**Site-naar-site-connectiviteit**: een rechtstreekse verbinding tussen de lokale site en de Azure-netwerk moet tot stand worden gebracht voor de communicatie tussen servers. Gebruik een beveiligde site-naar-site VPN-verbinding met een Azure-netwerk dat wordt gebruikt als de site van de herstel na noodgevallen. Zie voor meer informatie [een site-naar-site VPN-verbinding tussen een on-premises site en een Azure-netwerk](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal).

**Active Directory**: DFSR is afhankelijk van Active Directory. Dit betekent dat de Active Directory-forest met lokale domeincontrollers wordt uitgebreid naar de site van het herstel na noodgevallen in Azure. Zelfs als u niet DFSR gebruikt als de beoogde gebruikers moeten worden toegang verleend of geverifieerd om toegang te krijgen, moet u deze stappen uitvoeren. Zie voor meer informatie [uitbreiden op lokale Active Directory naar Azure](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendation-for-azure-iaas-virtual-machines"></a>Aanbeveling van herstel na noodgevallen voor Azure IaaS virtuele machines

Als u configureert en herstel na noodgevallen van bestandsservers gehost op Azure IaaS VM's beheren, u uit twee opties kiezen kunt, op basis van of u verplaatsen wilt naar [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction):

* [Bestand Sync gebruiken](#use-file-sync-to-replicate-files-hosted-on-an-iaas-virtual-machine)
* [Site Recovery gebruiken](#replicate-an-iaas-file-server-virtual-machine-by-using-site-recovery)

## <a name="use-file-sync-to-replicate-files-hosted-on-an-iaas-virtual-machine"></a>Bestand Sync gebruiken voor replicatie van bestanden die worden gehost op een virtuele machine voor IaaS

Azure Files kan worden gebruikt om bestandsshares op traditionele on-premises bestandsservers of NAS-apparaten volledig te vervangen of aan te vullen. Azure-bestandsshares kunnen ook worden gerepliceerd met het synchroniseren van het bestand naar de Windows-servers, on-premises of in de cloud, prestaties en gedistribueerd cachegebruik van de gegevens waar deze wordt gebruikt. De volgende stappen beschrijven de aanbeveling van het herstel na noodgevallen voor Azure VM's die dezelfde functionaliteit als traditionele bestandsservers uitvoeren:
* Computers beveiligen met behulp van Site Recovery. Volg de stappen in [een Azure VM gerepliceerd naar een andere Azure-regio](azure-to-azure-quickstart.md).
* Bestand Sync gebruiken om te repliceren van bestanden van de virtuele machine die als de bestandsserver naar de cloud fungeert.
* Gebruik de Site Recovery [herstelplan](site-recovery-create-recovery-plans.md) functie scripts toevoegen [koppelen van de Azure-bestandsshare](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) en toegang tot de share in de virtuele machine.

De volgende stappen wordt kort beschreven hoe bestand Sync gebruiken:

1. [Een opslagaccount maken in Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Als u voor uw opslagaccounts geografisch redundante opslag met leestoegang hebt gekozen, krijgt u leestoegang tot uw gegevens van de secundaire regio als er een ramp optreedt. Zie voor meer informatie [Azure file share disaster recovery strategieën](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
2. [Een bestandsshare maken](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Bestand synchronisatie starten](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) op uw Azure-bestandsserver.
4. Maak een groep voor synchronisatie. Eindpunten in een groep voor synchronisatie zijn gesynchroniseerd met elkaar. Een groep voor synchronisatie moet ten minste één cloudeindpunt, waarmee een Azure-bestandsshare bevatten. Een groep voor synchronisatie moet ook bevatten één servereindpunt waarmee een pad op een WindowsServer wordt aangeduid.
5. Uw bestanden worden nu gesynchroniseerd tussen uw Azure-bestandsshare en de lokale server.
6. In het geval van een ramp in uw on-premises-omgeving, kunt u een failover uitvoeren met behulp van een [herstelplan](site-recovery-create-recovery-plans.md). Het script toevoegen [koppelen van de Azure-bestandsshare](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) en toegang tot de share in de virtuele machine.

### <a name="replicate-an-iaas-file-server-virtual-machine-by-using-site-recovery"></a>Virtuele machine voor IaaS-bestand server repliceren met behulp van Site Recovery

Als u lokale clients die toegang hebben tot de virtuele machine voor IaaS-bestand server hebt, alle volgende stappen uitvoeren. Ga anders verder met stap 3.

1. Een site-naar-site VPN-verbinding tussen de lokale site en de Azure-netwerk.
2. On-premises Active Directory uitbreiden.
3. [Instellen van herstel na noodgevallen](azure-to-azure-tutorial-enable-replication.md) voor de servercomputer van IaaS-bestand naar een secundaire regio.


Zie voor meer informatie over herstel na noodgevallen in een secundaire regio [in dit artikel](concepts-azure-to-azure-architecture.md).


## <a name="replicate-an-on-premises-file-server-by-using-site-recovery"></a>Een on-premises bestand-server repliceren met behulp van Site Recovery

De volgende stappen beschrijven replicatie voor een VMware-VM. Zie voor stappen voor het repliceren van een virtuele Hyper-V-machine, [in deze zelfstudie](tutorial-hyper-v-to-azure.md).

1. [Voorbereiden van de Azure-resources](tutorial-prepare-azure.md) voor replicatie van machines lokale.
2. Een site-naar-site VPN-verbinding tussen de lokale site en de Azure-netwerk. 
3. On-premises Active Directory uitbreiden.
4. [On-premises VMware-servers voorbereiden](tutorial-prepare-on-premises-vmware.md).
5. [Instellen van herstel na noodgevallen](tutorial-vmware-to-azure.md) naar Azure voor lokale virtuele machines.

## <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>DFSR uitbreiden naar een virtuele machine van Azure IaaS

1. Een site-naar-site VPN-verbinding tussen de lokale site en de Azure-netwerk. 
2. On-premises Active Directory uitbreiden.
3. [Maken en inrichten van een bestandsserver VM](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) op de virtuele Azure-netwerk.
Zorg ervoor dat de virtuele machine is toegevoegd aan het dezelfde Azure-netwerk met cross-connectiviteit met de on-premises omgeving. 
4. Installeren en [DFSR configureren](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) op Windows Server.
5. [Implementeren van een DFS-naamruimte](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).
6. Met de DFS-naamruimte is geïmplementeerd, kan de failover van gedeelde mappen van de productie noodherstelsites worden gedaan door het bijwerken van de mapdoelen DFS-naamruimte. Na deze DFS namespace wijzigingen worden gerepliceerd via Active Directory gebruikers zijn verbonden met de juiste mapdoelen transparant.

## <a name="use-file-sync-to-replicate-your-on-premises-files"></a>Synchronisatie-bestand gebruiken om te repliceren van uw lokale bestanden
U kunt bestand Sync kunt gebruiken voor het repliceren van bestanden naar de cloud. In het geval van een ramp optreedt en het ontbreken van de lokale bestandsserver, kunt u de gewenste bestandslocaties vanuit de cloud koppelen en doorgaan serviceaanvragen van clientcomputers.
Bestand Sync integreren met Site Recovery:

* De file server machines beveiligen met behulp van Site Recovery. Volg de stappen in [in deze zelfstudie](tutorial-vmware-to-azure.md).
* Bestand Sync gebruiken om te repliceren van bestanden van de computer die als een bestandsserver met de cloud fungeert.
* Gebruik de functie herstel plan in Site Recovery om toe te voegen scripts voor het koppelen van de Azure-bestandsshare op de bestandsserver failover virtuele machine in Azure.

Volg deze stappen voor het bestand Sync gebruiken:

1. [Een opslagaccount maken in Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Als u geografisch redundante opslag met leestoegang (aanbevolen) voor uw storage-accounts hebt gekozen, hebt u leestoegang tot uw gegevens van de secundaire regio als er een ramp optreedt. Zie voor meer informatie [Azure file share disaster recovery strategieën](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
2. [Een bestandsshare maken](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Bestand Sync implementeren](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) in uw lokale bestandsserver.
4. Maak een groep voor synchronisatie. Eindpunten in een groep voor synchronisatie zijn gesynchroniseerd met elkaar. Een groep voor synchronisatie moet ten minste één cloudeindpunt, waarmee een Azure-bestandsshare bevatten. De groep voor synchronisatie moet ook een servereindpunt waarmee een pad op de lokale Windows-server bevatten.
5. Uw bestanden worden nu gesynchroniseerd tussen uw Azure-bestandsshare en de lokale server.
6. In het geval van een ramp in uw on-premises-omgeving, kunt u een failover uitvoeren met behulp van een [herstelplan](site-recovery-create-recovery-plans.md). Het script voor het koppelen van de Azure-bestandsshare en toegang tot de share in de virtuele machine toevoegen.

> [!NOTE]
> Zorg ervoor dat poort 445 is geopend. Het SMB-protocol maakt gebruik van Azure Files. SMB communiceert via TCP-poort 445. Controleer als TCP-poort 445 vanaf een clientcomputer wordt niet geblokkeerd door uw firewall.


## <a name="do-a-test-failover"></a>Voer een testfailover

1. Ga naar de Azure-portal en selecteer uw kluis Recovery-Service.
2. Selecteer het herstelplan gemaakt voor de server-omgeving van het bestand.
3. Selecteer **Testfailover**.
4. Selecteer het herstelpunt en het Azure-netwerk om de test failover-proces te starten.
5. Nadat de secundaire-omgeving is, voert u uw validaties.
6. Nadat de validaties zijn voltooid, selecteert u **opschonen testfailover** van het herstelplan en de testfailover omgeving is opgeschoond.

Zie voor meer informatie over het uitvoeren van een testfailover [Testfailover Site Recovery](site-recovery-test-failover-to-azure.md).

Zie voor instructies over het tijdens het doorzoeken van de testfailover voor Active Directory en DNS, [Testfailover-overwegingen voor Active Directory en DNS](site-recovery-active-directory.md).

## <a name="do-a-failover"></a>Voer een failover

1. Ga naar de Azure-portal en selecteer de Recovery Services-kluis.
2. Selecteer het herstelplan gemaakt voor de server-omgeving van het bestand.
3. Selecteer **Failover**.
4. Selecteer het herstelpunt dat om de failoverproces te starten.

Zie voor meer informatie over het uitvoeren van een failover [-Failover in Site Recovery](site-recovery-failover.md).
