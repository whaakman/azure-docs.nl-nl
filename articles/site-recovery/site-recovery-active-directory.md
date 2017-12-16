---
title: Active Directory en DNS met Azure Site Recovery beveiligen | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een noodherstel voor Active Directory met Azure Site Recovery implementeert.
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: af1d9b26-1956-46ef-bd05-c545980b72dc
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/15/2017
ms.author: manayar
ms.openlocfilehash: 5db2d424c176428d31fb99fd8288120f18fcac7a
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Active Directory en DNS met Azure Site Recovery beveiligen
Enterprise-toepassingen zoals SharePoint, Dynamics AX en SAP, is afhankelijk van Active Directory en DNS-infrastructuur te laten functioneren. Wanneer u een oplossing voor het herstel na noodgevallen voor toepassingen maakt, moet u vaak Active Directory en DNS herstellen voordat u de andere toepassingsonderdelen om te controleren of de functionaliteit van de juiste toepassing.

Site Recovery kunt u een herstelplan volledig geautomatiseerd noodherstel maken voor Active Directory. Wanneer er storingen optreden, kunt u initieer een failover binnen enkele seconden vanaf elke locatie en ophalen van Active Directory actief en werkend in enkele minuten duren. Als u Active Directory voor meerdere toepassingen zoals SharePoint en SAP in uw primaire site hebt geïmplementeerd en u de volledige site failover wilt, kunt u failover Active Directory eerst met behulp van Site Recovery en vervolgens onder de andere toepassingen die gebruikmaken van failover Toepassingsspecifiek herstelplannen.

In dit artikel wordt uitgelegd hoe u het maken van een noodherstel voor Active Directory, het uitvoeren van failovers met behulp van een herstelplan met één muisklik, de ondersteunde configuraties en vereisten.  U moet bekend bent met Active Directory en Azure Site Recovery voordat u begint.

## <a name="prerequisites"></a>Vereisten
* Een Recovery Services-kluis in een Microsoft Azure-abonnement.
* Als u naar Azure repliceert, [voorbereiden](tutorial-prepare-azure.md) Azure-resources, zoals een Azure-abonnement, een Azure-netwerk en een opslagaccount.
* Neem de ondersteuningsvereisten voor alle onderdelen.

## <a name="replicating-domain-controller"></a>Bezig met het repliceren van domeincontroller

U moet setup [replicatie van Site Recovery](#enable-protection-using-site-recovery) op ten minste één virtuele machine-domeincontroller en DNS-te hosten. Als u hebt [meerdere domeincontrollers](#environment-with-multiple-domain-controllers) in uw omgeving, naast het repliceren van de domain controller virtuele machine met Site Recovery ook hebt u voor het instellen van een [extra domeincontroller](#protect-active-directory-with-active-directory-replication)op de doelsite (Azure of een secundaire on-premises datacenter).

### <a name="single-domain-controller-environment"></a>Één domeincontroller-omgeving
Als u een aantal toepassingen en slechts één domeincontroller hebt en u wilt de hele site samen een failover, vervolgens wordt u aangeraden Site Recovery op de domeincontroller worden gerepliceerd naar de doelsite (Azure of een secundaire on-premises datacenter). Hetzelfde domein, domeincontroller en DNS-virtuele machine kan worden gebruikt voor gerepliceerd [failover testen](#test-failover-considerations) ook.

### <a name="environment-with-multiple-domain-controllers"></a>Omgeving met meerdere domeincontrollers
Als u veel toepassingen hebt en er meer dan één domeincontroller in de omgeving is of als u van plan bent een paar toepassingen tegelijk failover, raden wij aan die naast de domain controller virtuele machine met Site Recovery repliceren, moet u ook instellen van een [extra domeincontroller](#protect-active-directory-with-active-directory-replication) op de doelsite (Azure of een secundaire on-premises datacenter). Voor [testfailover](#test-failover-considerations), kunt u domeincontroller gerepliceerd door Site Recovery en voor failover, de extra domeincontroller op de doelsite.

## <a name="enable-protection-using-site-recovery"></a>Schakel de beveiliging met Site Recovery
### <a name="protect-the-virtual-machine"></a>Beveiligen van de virtuele machine
Schakel de beveiliging van de domain controller en DNS-virtuele machine in Site Recovery. De domeincontroller die wordt gerepliceerd met Site Recovery wordt gebruikt voor [testfailover](#test-failover-considerations). Zorg ervoor dat het voldoet aan de volgende vereisten:

1. De domeincontroller is een globale catalogusserver
2. De domeincontroller moet de FSMO-roleigenaar voor functies die nodig tijdens een testfailover zijn (anders deze rollen moet worden [overgenomen](http://aka.ms/ad_seize_fsmo) na de failover)

### <a name="configure-virtual-machine-network-settings"></a>Netwerkinstellingen van de virtuele machine configureren
Voor de domain controller en DNS-virtuele machine netwerkinstellingen onder de berekenings- en -instellingen van de gerepliceerde virtuele machine in Site Recovery. Dit is om ervoor te zorgen dat de virtuele machine wordt gekoppeld aan het juiste netwerk na een failover.

## <a name="protect-active-directory-with-active-directory-replication"></a>Beveiligen van Active Directory met Active Directory-replicatie
### <a name="site-to-site-protection"></a>Site-naar-site-beveiliging
Maak een domeincontroller op de secundaire site. Wanneer u de server naar een domeincontroller promoveert, geef de naam van het domein dat wordt gebruikt op de primaire site. U kunt de **Active Directory: Sites en Services** -module voor het configureren van instellingen voor het siteobject koppeling waaraan de sites worden toegevoegd. Instellingen configureren op een site-koppeling, kunt u bepalen wanneer replicatie plaatsvindt tussen twee of meer sites, en hoe vaak. Zie voor meer informatie [replicatie tussen Sites plannen](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Beveiliging van de site naar Azure
Volg de instructies voor [maken van een domeincontroller in een Azure-netwerk](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Wanneer u de server naar een domeincontroller promoveert, moet u de dezelfde domeinnaam die wordt gebruikt op de primaire site opgeven.

Vervolgens [opnieuw configureren van de DNS-server voor het virtuele netwerk](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), gebruik van de DNS-server in Azure.

![Azure-netwerk](./media/site-recovery-active-directory/azure-network.png)

### <a name="azure-to-azure-protection"></a>Beveiliging van Azure naar Azure
Volg de instructies voor [maken van een domeincontroller in een Azure-netwerk](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Wanneer u de server naar een domeincontroller promoveert, moet u de dezelfde domeinnaam die wordt gebruikt op de primaire site opgeven.

Vervolgens [opnieuw configureren van de DNS-server voor het virtuele netwerk](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), gebruik van de DNS-server in Azure.

## <a name="test-failover-considerations"></a>Testfailover-overwegingen
Testfailover optreedt in een netwerk dat is geïsoleerd van het productienetwerk, om te voorkomen dat gevolgen voor de productie-workloads.

De meeste toepassingen moet ook de aanwezigheid van een domeincontroller en een DNS-server naar de functie. Daarom voordat de failover van de toepassing wordt uitgevoerd, moet een domeincontroller worden gemaakt in de geïsoleerde netwerk moet worden gebruikt voor de testfailover. De eenvoudigste manier om dit te doen is gerepliceerd van een domein domeincontroller/DNS-virtuele machine met Site Recovery. Voer een testfailover van de virtuele machine van de domain controller voordat een testfailover uitgevoerd van het herstelplan voor de toepassing wordt uitgevoerd. Hier ziet u hoe u dat doen:

1. [Repliceren](site-recovery-replicate-vmware-to-azure.md) de domain controller en DNS-virtuele machine met Site Recovery.
2. Maak een geïsoleerd netwerk. Een virtueel netwerk in Azure gemaakt standaard is geïsoleerd van andere netwerken. Het is raadzaam dat het IP-adresbereik voor dit netwerk hetzelfde als dat van uw productienetwerk is. Geen site-naar-site-connectiviteit op dit netwerk inschakelen.
3. Geef een DNS IP-adres in het netwerk gemaakt als het IP-adres dat u verwacht dat de DNS-virtuele machine om op te halen. Als u naar Azure repliceert, geef vervolgens het IP-adres voor de virtuele machine die wordt gebruikt op failover in de **IP-adres doel** instellen in **berekening en netwerk** instellingen van de gerepliceerde virtuele machine.

    ![Azure testnetwerk](./media/site-recovery-active-directory/azure-test-network.png)

> [!TIP]
> Site Recovery probeert te maken van test-virtuele machines in een subnet met dezelfde naam en met dezelfde IP als die beschikbaar is in **berekening en netwerk** instellingen van de virtuele machine. Als een subnet met dezelfde naam niet beschikbaar in het Azure-netwerk dat is opgegeven voor testfailover is, wordt de virtuele testmachine gemaakt in het eerste subnet alfabetische volgorde. Als het doel-IP-adres deel van het gekozen subnet uitmaakt, probeert Site Recovery vervolgens de failover de virtuele machine met behulp van de IP-adres van doel maken. Als het doel-IP-adres geen deel uit van het gekozen subnet maakt, wordt dan failover van de virtuele machine gemaakt met het volgende beschikbare IP-adres in het gekozen subnet.
>
>


1. Als u naar een andere lokale site repliceert en u DHCP gebruikt, volgt u de instructies voor het [DNS en DHCP instellen voor testfailover](site-recovery-test-failover-vmm-to-vmm.md#prepare-dhcp)
2. Voer een testfailover van de domain controller virtuele machine uitvoeren in de geïsoleerde netwerk. Gebruik meest recente beschikbare **toepassing consistente** herstelpunt van de domain controller virtuele machine uitvoeren van de testfailover.
3. Een testfailover voor het herstelplan met virtuele machines van de toepassing uitvoeren.
4. Nadat het testen is voltooid, **testfailover opschonen** op de virtuele machine van de domain controller. Hiermee verwijdert u de domeincontroller die is gemaakt voor de testfailover.


### <a name="removing-reference-to-other-domain-controllers"></a>Verwijder de verwijzing naar andere domeincontrollers
Wanneer u een failovertest uitvoert, kunt u niet alle domeincontrollers in het testnetwerk brengt. Verwijder de verwijzing van andere domeincontrollers die voorkomen in uw productieomgeving, moet u mogelijk [overnemen van FSMO-Active Directory-functies](http://aka.ms/ad_seize_fsmo) en [metagegevens opruimen](https://technet.microsoft.com/library/cc816907.aspx) voor domein ontbreekt domeincontrollers.



> [!IMPORTANT]
> Enkele van de configuraties beschreven in de volgende sectie zijn niet de standaard/domain controller standaardconfiguraties. Als u niet dat deze wijzigingen aanbrengen in een productie-domeincontroller wilt, kunt klikt u vervolgens u een domeincontroller moet worden gebruikt voor een testfailover van Site Recovery en deze wijzigingen aanbrengen in die specifieke.  
>
>

### <a name="issues-because-of-virtualization-safeguards"></a>Problemen vanwege veiligheidsmaatregelen voor virtualisatie

Vanaf Windows Server 2012 [extra garanties al ingebouwd in Active Directory Domain Services](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100). Met deze beveiligingen te helpen beveiligen van gevirtualiseerde domeincontrollers tegen USN-terugdraaiacties door, zolang het onderliggende hypervisor-platform VM-GenerationID ondersteunt. Azure ondersteunt VM-GenerationID, wat betekent dat domeincontrollers met Windows Server 2012 of later op Azure virtuele machines de extra beveiliging.


Wanneer de generatie-id opnieuw wordt ingesteld, de invocationID van de AD DS-database is ook opnieuw instellen, de RID-groep is verwijderd en SYSVOL is gemarkeerd als niet-bindende. Zie voor meer informatie [Inleiding tot virtualisatie van Active Directory Domain Services](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) en [Safely Virtualizing DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/)

Mislukt via Azure kan leiden tot het opnieuw instellen van de generatie-id en die in de aanvullende beveiliging gang wanneer de domain controller virtuele machine wordt opgestart in Azure. Dit kan leiden tot een **aanzienlijke vertraging** in gebruiker kunnen aanmelden bij de virtuele machine van de domain controller. Aangezien deze domeincontroller kan alleen worden gebruikt in een testfailover, zijn er geen veiligheidsmaatregelen voor virtualisatie nodig. Om ervoor te zorgen dat de generatie-id voor de domain controller virtuele machine niet wijzigen, kunt klikt u vervolgens u de volgende DWORD-waarde tot en met 4 in de lokale domeincontroller.


        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\gencounter\Start


#### <a name="symptoms-of-virtualization-safeguards"></a>Symptomen van veiligheidsmaatregelen voor virtualisatie

Als de veiligheidsmaatregelen voor virtualisatie hebben gestart na een failovertest uitvoert, ziet u mogelijk een of meer van de volgende symptomen:  

Wijziging van generatie-ID

![Wijziging van generatie-ID](./media/site-recovery-active-directory/Event2170.png)

Wijziging van de aanroep-ID

![Wijziging van de aanroep-ID](./media/site-recovery-active-directory/Event1109.png)

SYSVOL en de Netlogon-shares zijn niet beschikbaar

![Sysvol-Share](./media/site-recovery-active-directory/sysvolshare.png)

![Ntfrs Sysvol](./media/site-recovery-active-directory/Event13565.png)

Geen DFSR-databases zijn verwijderd

![DFSR-database verwijderen](./media/site-recovery-active-directory/Event2208.png)


> [!IMPORTANT]
> Enkele van de configuraties beschreven in de volgende sectie zijn niet de standaard/domain controller standaardconfiguraties. Als u niet dat deze wijzigingen aanbrengen in een productie-domeincontroller wilt, kunt klikt u vervolgens u een domeincontroller moet worden gebruikt voor een testfailover van Site Recovery en deze wijzigingen aanbrengen in die specifieke.  
>
>


### <a name="troubleshooting-domain-controller-issues-during-test-failover"></a>Domain controller problemen tijdens de testfailover


Voer de volgende opdracht om te controleren of SYSVOL en NETLOGON mappen worden gedeeld op vanaf de opdrachtprompt:

    NET SHARE

Voer op de opdrachtprompt de volgende opdracht om ervoor te zorgen dat de domeincontroller goed werkt.

    dcdiag /v > dcdiag.txt

Zoek in het uitvoerlogboek voor voor de volgende tekst om te bevestigen dat de domeincontroller goed werkt.

* 'doorgegeven test connectiviteit'
* 'doorgegeven test reclame'
* 'doorgegeven test MachineAccount'

Als u de bovenstaande voorwaarden is voldaan, is het waarschijnlijk dat de domeincontroller goed werkt. Als dat niet het geval is, voer de volgende stappen.


* Voer een bindend terugzetten van de domeincontroller.
    * Hoewel het [niet aanbevolen het gebruik van FRS-replicatie](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), maar als u nog steeds wordt gebruikt en volg de stappen [hier](https://support.microsoft.com/kb/290762) een gezaghebbende te herstellen. U vindt meer informatie over Burflags besproken in de vorige koppeling [hier](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * Als u de DFSR-replicatie gebruikt, volg de stappen beschikbaar [hier](https://support.microsoft.com/kb/2218556) een gezaghebbende te herstellen. U kunt ook Powershell-functies die beschikbaar zijn op deze [koppeling](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/) voor dit doel.

* Vereiste voor initiële synchronisatie omzeilen door in te stellen van de volgende registersleutel in 0 op de lokale domeincontroller. Als deze DWORD niet bestaat, kunt klikt u vervolgens u dit maken onder het knooppunt 'Parameters'. U kunt meer lezen over deze [hier](https://support.microsoft.com/kb/2001093)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations

* Schakel de vereiste dat een globale catalogusserver beschikbaar is voor gebruikersaanmelding valideren door het instellen van de volgende registersleutel op 1 in de lokale domeincontroller. Als deze DWORD niet bestaat, kunt klikt u vervolgens u dit maken onder het knooppunt 'Lsa'. U kunt meer lezen over deze [hier](http://support.microsoft.com/kb/241789)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures



### <a name="dns-and-domain-controller-on-different-machines"></a>DNS- en de domeincontroller op verschillende computers
Als DNS niet in dezelfde virtuele machine als de domeincontroller, moet u een VM DNS voor de testfailover maken. Als ze op dezelfde virtuele machine, kunt u deze sectie overslaan.

U kunt een nieuwe DNS-server gebruiken en de vereiste zones maken. Als uw Active Directory-domein contoso.com is, kunt u bijvoorbeeld een DNS-zone maken met de naam contoso.com. De vermeldingen die overeenkomt met de Active Directory moeten worden bijgewerkt in DNS, als volgt:

1. Zorg ervoor dat deze instellingen zijn voldaan voordat een andere virtuele machine in het herstelplan weergegeven wordt:

   * De zone moet de naam achter de naam van de basis-forest.
   * De zone moet bestandsondersteuning.
   * De zone moet zijn ingeschakeld voor veilige en niet-beveiligde updates.
   * De resolver van de virtuele machine van de domain controller moet verwijzen naar het IP-adres van de DNS-virtuele machine.
2. Voer de volgende opdracht op domain controller virtuele machine:

    `nltest /dsregdns`
3. Een zone op de DNS-server toevoegen en voeg een vermelding voor het DNS-updates niet-beveiligde toestaan:

        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1

## <a name="next-steps"></a>Volgende stappen
[Meer informatie](site-recovery-workload.md) over het beveiligen van enterprise-werkbelastingen met Azure Site Recovery.
