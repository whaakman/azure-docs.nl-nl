---
title: Een bestandsserver beveiligen met behulp van Azure Site Recovery
description: Dit artikel wordt beschreven hoe u kunt een bestandsserver beveiligen met behulp van Azure Site Recovery
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
ms.openlocfilehash: cc585d6add9b9c5ce7f3379aeaf5ec79f5c61d51
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="protect-a-file-server-by-using-azure-site-recovery"></a>Een bestandsserver beveiligen met behulp van Azure Site Recovery 

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel herstel (BCDR) doordat uw business-apps die beschikbaar zijn tijdens geplande en ongeplande uitval. Site Recovery beheert en noodherstel van on-premises machines en virtuele Azure-machines (VM's), met inbegrip van replicatie, failovers en herstel van verschillende workloads ingedeeld.

Dit artikel wordt beschreven hoe u kunt een bestandsserver beveiligen met behulp van Azure Site Recovery en andere aanbevelingen op basis van verschillende omgevingen.

## <a name="file-server-architecture"></a>File server-architectuur
Een systeem van open, gedistribueerde delen van bestanden biedt een omgeving waar geografisch verspreide gebruikers op bestanden integriteit vereisten in gevaar kunnen samenwerken. 

Een typische lokale server ecosysteem die ondersteuning biedt voor een groot aantal gelijktijdige gebruikers en inhoudsitems maakt gebruik van Distributed File System Replication (DFSR) voor replicatie planning en bandbreedtebeperking. De Remote Differential Compression (RDC)-compressiealgoritme kan worden gebruikt om efficiënt updatebestanden via een netwerk met beperkte bandbreedte maakt gebruik van DFSR. RDC detecteert invoegingen, verwijderingen en of van gegevens in bestanden. Hiermee kunt DFSR gerepliceerd alleen de gewijzigde bestandsblokken wanneer bestanden worden bijgewerkt. 

In sommige omgevingen bestand server worden dagelijkse back-ups genomen op niet-piekuren tijdstippen voor herstel na noodgevallen. Deze omgevingen gebruik geen van DFSR.

De volgende topologie ziet u een bestand serveromgeving met DFSR geïmplementeerd:
                
![DFSR-architectuur](media/site-recovery-file-server/dfsr-architecture.JPG)

In de afbeelding deelnemen meerdere bestandsservers (aangeduid als leden) actief aan het repliceren van bestanden via een replicatiegroep. De inhoud in de gerepliceerde map zijn beschikbaar voor alle clients die aanvragen naar een van de leden verzendt, zelfs als een van de leden offline gaat.

## <a name="disaster-recovery-strategies-for-file-servers"></a>Strategieën van herstel na noodgevallen voor bestandsservers

- **Een bestandsserver repliceren naar Azure met behulp van Azure Site Recovery**: wanneer een of meer van de lokale bestandsservers niet toegankelijk zijn, het herstel van virtuele machines kan worden gebracht van in Azure. Het herstel van virtuele machines kan vervolgens aanvragen van clients, on-premises fungeren als er site-naar-site VPN-verbinding en Active Directory is geconfigureerd in Azure. U kunt dit doen in een omgeving met DFSR geconfigureerd of in een serveromgeving eenvoudig bestanden met geen DFSR. 

- **DFSR uitbreiden naar een virtuele machine van Azure IaaS**: In een geclusterde bestandsserver-serveromgeving met DFSR geïmplementeerd een voorgestelde benadering bestaat uit de lokale DFSR uitbreiden naar Azure. Een virtuele machine van Azure kan de bestandsserverrol uitvoeren. 

    Nadat de afhankelijkheden van de site-naar-site VPN-verbinding en Active Directory worden verwerkt en DFSR geïmplementeerd, is wanneer een of meer van de lokale bestandsservers niet toegankelijk zijn, de clients kunnen nog steeds verbinding maken met de Azure VM. De Azure VM fungeert de aanvragen.

    We raden aan deze methode als uw virtuele machines hebt configuraties die biedt geen ondersteuning voor Azure Site Recovery. Een voorbeeld van een dergelijke configuratie is een gedeelde clusterschijf dat vaak wordt gebruikt in serveromgevingen bestand. DFSR werkt ook goed in omgevingen met gemiddeld verloop lage bandbreedte. Met deze methode moet u ruimte voor de extra kosten van een virtuele machine van Azure en voortdurend.  

- **Azure bestand Sync gebruiken voor replicatie van uw bestanden**: als u voor uw reis naar de cloud voorbereiden wilt of al van een virtuele machine in Azure gebruikmaakt, we raden het gebruik van Azure File-synchronisatie. Deze service biedt synchroniseren van volledig beheerde bestandsshares in de cloud die toegankelijk via de industriestandaard zijn [Server Message Block ](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)protocol (SMB). Vervolgens kunt u bestandsshares koppelpunt Azure gelijktijdig met de cloud of on-premises implementaties van Windows, Linux en Mac OS. 

Het volgende diagram kunt u bepalen welke strategie moet worden gebruikt voor uw serveromgeving bestand:

![beslissingsstructuur](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-for-making-a-disaster-recovery-decision"></a>Factoren voor het maken van een herstel na noodgevallen beslissing

|Omgeving  |Aanbeveling  |Aandachtspunten |
|---------|---------|---------|
|Bestand-serveromgeving met of zonder DFSR|   [Azure Site Recovery gebruiken voor replicatie](#replicate-an-on-premises-file-server-by-using-azure-site-recovery)   |    Site Recovery biedt geen ondersteuning voor gedeelde schijfclusters of NAS. Als uw omgeving een van deze configuraties gebruikt, kunt u een van de andere methoden zo nodig gebruiken. <br> Azure Site Recovery biedt geen ondersteuning voor SMB 3.0. De gerepliceerde virtuele machine wordt bijgewerkt met wijzigingen aangebracht in bestanden alleen wanneer de wijzigingen zijn bijgewerkt op de oorspronkelijke locatie van het bestand. 
|Bestand-serveromgeving met DFSR     |  [DFSR uitbreiden naar een virtuele machine van Azure IaaS](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |      DFSR werkt goed in bandbreedte crunched omgevingen. Deze methode voert echter moet u een Azure VM is actief en werkend voortdurend. De planning moet de kosten van de virtuele machine.         |
|Azure IaaS VM     |     [Azure File Sync gebruiken](#use-azure-file-sync-to-replicate-your-on-premises-files)   |     Als u van Azure bestand synchronisatie tijdens de failover gebruikmaakt, moet u in een noodherstelscenario handmatige acties om ervoor te zorgen dat de bestandsshares toegankelijk in een transparante manier om de client-computer te zijn nemen. Azure File-synchronisatie vereist poort 445 geopend vanaf de clientcomputer.     |


### <a name="site-recovery-support"></a>Site Recovery-ondersteuning
Omdat replicatie van Site Recovery networkdirect toepassing is, de aanbevelingen die naar verwachting geldt voor de volgende scenario's:
| Bron    |Een secundaire site    |Naar Azure
|---------|---------|---------|
|Azure| -|Ja|
|Hyper-V|   Ja |Ja
|VMware |Ja|   Ja
|Fysieke server|   Ja |Ja
 

> [!IMPORTANT]
> Voordat u met een van de volgende methoden doorgaat, moet u adresafhankelijkheden.

**Site-naar-Site-connectiviteit**: een rechtstreekse verbinding tussen de lokale site en de Azure-netwerk moet tot stand worden gebracht voor de communicatie tussen servers. U kunt een beveiligde site-naar-site VPN-verbinding met een Azure-netwerk dat wordt gebruikt als het herstel na noodgevallen site gebruiken. Zie voor meer informatie [maken van een Site-naar-Site-verbinding in de Azure portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal).

**Active Directory**: DFSR is afhankelijk van Active Directory. Dit betekent dat de Active Directory-forest met lokale domeincontrollers wordt uitgebreid naar de site van het herstel na noodgevallen in Azure. Zelfs als u geen DFSR als de beoogde gebruikers moeten worden verleend/geverifieerd om toegang te krijgen, moet u deze stappen uitvoert.
Zie voor meer informatie [uitbreiden op lokale Active Directory naar Azure](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendations-for-azure-iaas-virtual-machines"></a>Disaster recovery aanbevelingen voor Azure IaaS virtuele machines

Als u configureren en beheren van herstel na noodgevallen van bestandsservers gehost op Azure IaaS VM's, kunt u kiezen uit twee opties: Azure File-synchronisatie en Azure Site Recovery. De beslissing is afhankelijk van of u verplaatsen wilt naar [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).

### <a name="use-azure-file-sync-to-replicate-files-hosted-on-iaas-virtual-machines"></a>Azure bestand Sync gebruiken voor replicatie van bestanden die worden gehost op IaaS virtuele machines

Azure-bestanden kunt u volledig vervangen of vormen een aanvulling op de bestandsservers traditionele on-premises of NAS-apparaten. Azure-bestandsshares kunnen ook worden gerepliceerd met het synchroniseren van Azure-bestand voor Windows-servers, on-premises of in de cloud, zodat en gedistribueerd cachegebruik van de gegevens waar deze wordt gebruikt. 

De volgende stappen worden de aanbeveling van het herstel na noodgevallen voor Azure virtuele machines die dezelfde functie als traditionele bestandsservers vervullen in detail beschreven:
1. Computers beveiligen met behulp van Azure Site Recovery door de stappen die hier worden genoemd.

2. Azure bestand Sync gebruiken om te repliceren van bestanden van de virtuele machine die als de bestandsserver, naar de cloud fungeert.

3. Gebruik de [herstelplan](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-create-recovery-plans) functie in Azure Site Recovery voor het toevoegen van scripts [koppelen van de Azure-bestandsshare](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) en toegang tot de share in de virtuele machine.

De volgende stappen is een korte beschrijving van het gebruik van de service Azure File-synchronisatie:

1. [Een opslagaccount maken in Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Als u voor uw opslagaccounts geografisch redundante opslag met leestoegang hebt gekozen, krijgt u leestoegang tot uw gegevens van de secundaire regio als er een ramp optreedt. Zie voor meer informatie [Azure File share disaster recovery strategieën](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

2. [Een bestandsshare maken](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).

3. [Azure File-synchronisatie implementeren](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) op uw Azure-bestandsserver.

4. Maak een groep voor synchronisatie. Eindpunten in een groep voor synchronisatie wordt gesynchroniseerd met elkaar. Een groep voor synchronisatie moet ten minste één cloudeindpunt dat vertegenwoordigt een Azure-bestandsshare, en één servereindpunt waarmee een pad op een Windows-server bevatten.  
    Uw bestanden wordt nu gesynchroniseerd blijven in uw Azure-bestandsshare en de lokale server.

5. Als er een ramp in uw on-premises-omgeving, moet u een failover uitvoeren met behulp van een herstelplan. Het script toevoegen [koppelen van de Azure-bestandsshare](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) en toegang tot de share in de virtuele machine.

### <a name="replicate-an-iaas-file-server-virtual-machine-by-using-azure-site-recovery"></a>Virtuele machine voor IaaS-bestand server repliceren met behulp van Azure Site Recovery

Als u lokale clients die toegang hebben tot de virtuele machine voor IaaS-bestand server hebt, voert u de volgende stappen uit. Anders alleen stap 3 uitvoeren.

1. Een site-naar-site VPN-verbinding tussen de lokale site en de Azure-netwerk.  

2. On-premises Active Directory uitbreiden.

3. [Instellen van herstel na noodgevallen](azure-to-azure-tutorial-enable-replication.md) voor de servercomputer van IaaS-bestand naar een secundaire regio.

Zie voor meer informatie over herstel na noodgevallen in een secundaire regio [Azure naar Azure replicatie architectuur](concepts-azure-to-azure-architecture.md).

## <a name="disaster-recovery-recommendations-for-on-premises-virtual-machines"></a>Aanbevelingen van herstel na noodgevallen voor on-premises virtuele machines 

### <a name="replicate-an-on-premises-file-server-by-using-azure-site-recovery"></a>Een on-premises bestand-server repliceren met behulp van Azure Site Recovery
De volgende stappen wordt beschreven replicatie voor een VMware-VM. Zie voor stappen voor het repliceren van een virtuele Hyper-V-machine, [herstel na noodgevallen van lokale Hyper-V-machines naar Azure instellen](https://docs.microsoft.com/en-us/azure/site-recovery/tutorial-hyper-v-to-azure).

1. [Voorbereiden van de Azure-resources](tutorial-prepare-azure.md) voor replicatie van machines lokale.

2. Een site-naar-site VPN-verbinding tussen de lokale site en de Azure-netwerk.  

3. On-premises Active Directory uitbreiden.

4. [On-premises VMware-servers voorbereiden](tutorial-prepare-on-premises-vmware.md).

5. [Instellen van herstel na noodgevallen](tutorial-vmware-to-azure.md) naar Azure voor lokale virtuele machines.

### <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>DFSR uitbreiden naar een virtuele machine van Azure IaaS

1. Een site-naar-site VPN-verbinding tussen de lokale site en de Azure-netwerk. 

2. On-premises Active Directory uitbreiden.

3. [Maken en inrichten van een bestandsserver VM](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) op de virtuele Azure-netwerk.  
    Zorg ervoor dat de virtuele machine is toegevoegd aan het dezelfde Azure-netwerk met cross-connectiviteit met de on-premises omgeving. 

4. Installeren en [DFS-replicatie configureren](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) op Windows Server.

5. [Implementeren van een DFS-naamruimte](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).

6. Met de DFS-naamruimte is geïmplementeerd, kunt u gedeelde mappen van de productie noodherstelsites failover door het bijwerken van de mapdoelen DFS-naamruimte. Na deze DFS namespace wijzigingen worden gerepliceerd via Active Directory gebruikers zijn verbonden met de juiste mapdoelen transparant.

### <a name="use-azure-file-sync-to-replicate-your-on-premises-files"></a>Azure bestand Sync gebruiken voor replicatie van uw lokale bestanden
Met de Azure-bestand Sync-service, kunt u de gewenste bestanden repliceren naar de cloud. U kunt vervolgens de gewenste bestandslocaties vanuit de cloud koppelen en doorgaan serviceaanvragen van de clientcomputers in het geval van een noodherstel en niet beschikbaar zijn van de lokale bestandsserver.

De aanbevolen aanpak van de integratie van Azure File-synchronisatie met Azure Site Recovery is:
1. De file server machines beveiligen met behulp van Azure Site Recovery. Volg de stappen in [herstel na noodgevallen naar Azure instellen voor de lokale virtuele VMware-machines](tutorial-vmware-to-azure.md).

2. Azure bestand Sync gebruiken om te repliceren van bestanden van de computer die als een bestandsserver, naar de cloud fungeert.

3. Gebruik de functie herstel abonnement in Azure Site Recovery om toe te voegen scripts voor het koppelen van de Azure-bestandsshare op de bestandsserver failover virtuele machine in Azure.

De volgende stappen details met de Azure-bestand Sync-service:

1. [Een opslagaccount maken in Azure](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Als u geografisch redundante opslag met leestoegang kiest (aanbevolen) voor uw storage-accounts, hebt u alleen toegang tot uw gegevens van de secundaire regio als er een ramp optreedt. Zie voor meer informatie [Azure File share disaster recovery strategieën](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

2. [Een bestandsshare maken](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).

3. [Azure File-synchronisatie implementeren](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) in uw lokale bestandsserver.

4. Maak een groep voor synchronisatie. Eindpunten in een groep voor synchronisatie wordt gesynchroniseerd met elkaar. Een groep voor synchronisatie moet ten minste één cloudeindpunt dat vertegenwoordigt een Azure-bestandsshare, en één servereindpunt, waarmee een pad op de lokale Windows-server bevatten.  
    Uw bestanden wordt nu gesynchroniseerd blijven in uw Azure-bestandsshare en de lokale server.

5. Als er een ramp in uw on-premises-omgeving, moet u een failover uitvoeren met behulp van een herstelplan. Het script voor het koppelen van de Azure-bestandsshare en toegang tot de share in de virtuele machine toevoegen.

> [!NOTE]
> Zorg ervoor dat poort 445 geopend is. Het SMB-protocol maakt gebruik van Azure Files. SMB communiceert via TCP-poort 445. Controleer of de firewall poort 445 van de clientcomputer blokkeert.


## <a name="perform-a-test-failover"></a>Een testfailover uitvoeren

1. Ga naar de Azure-portal en selecteer de Recovery Services-kluis.

2. Selecteer het herstelplan gemaakt voor de server-omgeving van het bestand.

3. Selecteer **Testfailover**.

4. Selecteer het herstelpunt en het virtuele Azure-netwerk om de test failover-proces te starten.

5. Wanneer de secundaire-omgeving actief is, voert u uw validaties.

6. Wanneer de validaties voltooid zijn, kunt u **opschonen testfailover** van het herstelplan en de testfailover omgeving is opgeschoond.

Zie voor meer informatie over het uitvoeren van een testfailover [testen van failover naar Azure in Site Recovery](site-recovery-test-failover-to-azure.md).

Zie voor instructies over het uitvoeren van de testfailover voor Active Directory en DNS, [Testfailover-overwegingen voor Active Directory en DNS](site-recovery-active-directory.md).

## <a name="perform-a-failover"></a>Een failover uitvoeren

1. Ga naar de Azure-portal en selecteer de Recovery Services-kluis.

2. Selecteer het herstelplan gemaakt voor de server-omgeving van het bestand.

3. Selecteer **Failover**.

4. Selecteer het herstelpunt dat om de failoverproces te starten.

Zie voor meer informatie over het uitvoeren van een failover [-Failover in Site Recovery](site-recovery-failover.md).
