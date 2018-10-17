---
title: Beveiligen van Active Directory en DNS met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een oplossing voor noodherstel voor Active Directory implementeren met behulp van Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: mayg
ms.openlocfilehash: 1072c903e9bd9ccb3d63cae426283a677d10077e
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49354288"
---
# <a name="use-azure-site-recovery-to-protect-active-directory-and-dns"></a>Azure Site Recovery gebruiken om te beveiligen van Active Directory en DNS

Zakelijke toepassingen zoals SharePoint, Dynamics AX en SAP, is afhankelijk van Active Directory en DNS-infrastructuur te laten functioneren. Bij het instellen van herstel na noodgevallen voor toepassingen, moet u vaak herstellen Active Directory en DNS voordat u andere toepassingsonderdelen, om te controleren of de functionaliteit van de juiste toepassing kunt herstellen.

U kunt [Site Recovery](site-recovery-overview.md) te maken van een herstelplan na noodgevallen voor Active Directory. Wanneer een onderbreking optreedt, kunt u een failover starten. U kunt Active Directory van en die worden uitgevoerd in een paar minuten hebben. Als u kunt Active Directory voor meerdere toepassingen hebt geïmplementeerd op de primaire site, bijvoorbeeld kunt voor SharePoint en SAP, u failover uitvoeren voor de volledige site. U kunt eerst een failover Active Directory met behulp van Site Recovery. Failover vervolgens naar de andere toepassingen, met behulp van toepassingsspecifieke herstelplannen.

In dit artikel wordt uitgelegd hoe u een oplossing voor noodherstel voor Active Directory maken. Het bevat de vereisten en instructies voor failover. U moet bekend zijn met Active Directory- en Site Recovery voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Als u naar Azure repliceert, [Azure-resources voorbereiden](tutorial-prepare-azure.md), met inbegrip van een abonnement, een Azure Virtual Network, een storage-account en een Recovery Services-kluis.
* Raadpleeg de [ondersteuningsvereisten](site-recovery-support-matrix-to-azure.md) voor alle onderdelen.

## <a name="replicate-the-domain-controller"></a>De domeincontroller worden gerepliceerd

- U moet instellen [Site Recovery-replicatie](#enable-protection-using-site-recovery), op ten minste één virtuele machine die als host fungeert voor een domeincontroller of DNS.
- Als u hebt [meerdere domeincontrollers](#environment-with-multiple-domain-controllers) in uw omgeving, ook moet u instellen een [extra domeincontroller](#protect-active-directory-with-active-directory-replication) op de doelsite. De extra domeincontroller kan zijn in Azure of in een secundaire on-premises datacenter.
- Als u slechts een paar toepassingen en één domeincontroller hebt, kunt u een failover wordt uitgevoerd de gehele site. In dit geval wordt u aangeraden Site Recovery voor het repliceren van de domeincontroller met de doelsite (ofwel in Azure of in een secundaire on-premises datacenter). U kunt de dezelfde gerepliceerde domeincontroller of DNS-virtuele machine voor [testfailover](#test-failover-considerations).
- - Als u veel toepassingen en meer dan één domeincontroller in uw omgeving hebt, of als u van plan bent failover wilt uitvoeren voor enkele toepassingen op een tijdstip, naast het repliceren van de domain controller virtuele machine met Site Recovery, is het raadzaam dat u om een instelt[extra domeincontroller](#protect-active-directory-with-active-directory-replication) op de doelsite (ofwel in Azure of in een secundaire on-premises datacenter). Voor [testfailover](#test-failover-considerations), kunt u de domeincontroller die gerepliceerd door Site Recovery kunt gebruiken. Voor de failover, kunt u de extra domeincontroller op de doelsite.

## <a name="enable-protection-with-site-recovery"></a>Schakel de beveiliging met Site Recovery

Site Recovery kunt u de virtuele machine die als host fungeert voor de domeincontroller of DNS beveiligen.

### <a name="protect-the-vm"></a>De VM te beveiligen
De domeincontroller die wordt gerepliceerd met behulp van Site Recovery wordt gebruikt voor [testfailover](#test-failover-considerations). Zorg ervoor dat het voldoet aan de volgende vereisten:

1. De domeincontroller is een algemene catalogusserver.
2. De domeincontroller moet de eigenaar van de FSMO-rol voor rollen die nodig zijn tijdens een test-failover. Anders wordt deze rollen moet worden [overgenomen](http://aka.ms/ad_seize_fsmo) na de failover.

### <a name="configure-vm-network-settings"></a>VM-netwerkinstellingen configureren
Voor de virtuele machine die als host fungeert voor de domeincontroller of DNS-server, in Site Recovery, configureert u netwerkinstellingen onder de **berekening en netwerk** instellingen van de gerepliceerde virtuele machine. Dit zorgt ervoor dat de virtuele machine is gekoppeld aan het juiste netwerk na een failover.

## <a name="protect-active-directory"></a>Active Directory beveiligen

### <a name="site-to-site-protection"></a>Site-naar-site-beveiliging
Maak een domeincontroller op de secundaire site. Wanneer u de server naar een domeincontroller promoveert, moet u de naam van het domein dat wordt gebruikt op de primaire site opgeven. U kunt de **Active Directory: Sites en Services** module voor het configureren van instellingen op de site-koppeling-object waaraan de sites worden toegevoegd. Instellingen configureren op een site-koppeling, kunt u bepalen wanneer er replicatie plaatsvindt tussen twee of meer sites, en hoe vaak vindt plaats. Zie voor meer informatie, [replicatie tussen sites plannen](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Site-naar-Azure-beveiliging
Maak eerst een domeincontroller in een Azure-netwerk. Wanneer u de server naar een domeincontroller promoveert, moet u de dezelfde domeinnaam die wordt gebruikt op de primaire site opgeven.

Configureer vervolgens opnieuw de DNS-server voor het virtuele netwerk voor gebruik van de DNS-server in Azure.

![Azure-netwerk](./media/site-recovery-active-directory/azure-network.png)

### <a name="azure-to-azure-protection"></a>Azure-naar-Azure-beveiliging
Maak eerst een domeincontroller in een Azure-netwerk. Wanneer u de server naar een domeincontroller promoveert, moet u de dezelfde domeinnaam die wordt gebruikt op de primaire site opgeven.

Configureer vervolgens opnieuw de DNS-server voor het virtuele netwerk voor gebruik van de DNS-server in Azure.

## <a name="test-failover-considerations"></a>Testfailover-overwegingen
Om te voorkomen dat invloed op werkbelastingen voor productie, optreedt test-failover in een netwerk dat is geïsoleerd van het productienetwerk.

De meeste toepassingen vereist de aanwezigheid van een domeincontroller of een DNS-server. Daarom voordat de toepassing een failover uitvoert, moet u een domeincontroller in het geïsoleerde netwerk moet worden gebruikt voor test-failover. De eenvoudigste manier om dit te doen is met Site Recovery voor het repliceren van een virtuele machine die als host fungeert voor een domeincontroller of DNS. Voer een testfailover van de domain controller virtuele machine voordat u een testfailover van het herstelplan te gaan voor de toepassing uitvoert. Hier ziet hoe u dat doen:

1. Gebruik Site Recovery naar [repliceren](vmware-azure-tutorial.md) de virtuele machine die als host fungeert voor de domeincontroller of DNS.
2. Maak een geïsoleerd netwerk. Een virtueel netwerk die u in Azure maakt is geïsoleerd van andere netwerken standaard. U wordt aangeraden dat u hetzelfde IP-adresbereik gebruiken voor dit netwerk die u in uw productienetwerk gebruiken. Geen site-naar-site beschikbaar is op dit netwerk inschakelen.
3. Geef een DNS-IP-adres in het geïsoleerde netwerk. Gebruik het IP-adres dat u verwacht dat de DNS-virtuele machine om op te halen. Als u naar Azure repliceert, geeft u het IP-adres voor de virtuele machine die wordt gebruikt voor failover. In de gerepliceerde virtuele machine, het IP-adres invoeren in de **berekening en netwerk** instellingen, selecteer de **doel-IP** instellingen.

    ![Testen met Azure-netwerk](./media/site-recovery-active-directory/azure-test-network.png)

    > [!TIP]
    > Site Recovery probeert te maken van virtuele testmachines in een subnet met dezelfde naam en met behulp van de IP-adres dat opgegeven in de **berekening en netwerk** instellingen van de virtuele machine. Als een subnet met dezelfde naam is niet beschikbaar in de Azure-netwerk dat opgegeven voor de testfailover, wordt de virtuele testmachine wordt gemaakt in het alfabet eerste subnet.
    >
    > Als het doel-IP-adres deel van het geselecteerde subnet uitmaakt, wordt Site Recovery probeert te maken van de virtuele testmachine van de failover met behulp van de doel-IP-adres. Als het doel-IP-adres maakt geen deel uit van het geselecteerde subnet, worden de virtuele testmachine van de failover wordt gemaakt met behulp van het volgende beschikbare IP-adres in het geselecteerde subnet.
    >
    >

### <a name="test-failover-to-a-secondary-site"></a>Test-failover naar een secundaire site

1. Als u naar een andere on-premises site repliceert en het gebruik van DHCP, [DNS en DHCP-instellen voor test-failover](hyper-v-vmm-test-failover.md#prepare-dhcp).
2. Voer een failovertest uit van de virtuele machine van de domain controller die wordt uitgevoerd in de geïsoleerde netwerk. Gebruik de meest recente beschikbare *toepassingsconsistent* herstelpunt van de domain controller virtuele machine te doen de testfailover.
3. Voert een testfailover uit voor het herstelplan bevat virtuele machines die de toepassing wordt uitgevoerd.
4. Wanneer de test is voltooid, *opschonen van de testfailover* op de virtuele machine van de domain controller. Deze stap worden verwijderd van de domeincontroller die is gemaakt voor test-failover.


### <a name="remove-references-to-other-domain-controllers"></a>Verwijzingen naar andere domeincontrollers verwijderen
Wanneer u een testfailover hebt gestart, niet alle domeincontrollers in het testnetwerk opnemen. Als u wilt verwijderen verwijzingen naar andere domeincontrollers die aanwezig zijn in uw productieomgeving, moet u mogelijk [overnemen van FSMO-Active Directory-rollen](http://aka.ms/ad_seize_fsmo) en [metagegevens opruimen](https://technet.microsoft.com/library/cc816907.aspx) voor domeincontrollers ontbreekt .


### <a name="issues-caused-by-virtualization-safeguards"></a>Problemen veroorzaakt door de veiligheidsmaatregelen voor virtualisatie

> [!IMPORTANT]
> Enkele van de configuraties die worden beschreven in deze sectie zijn niet standaard configuraties met een domeincontroller. Als u niet dat deze wijzigingen aanbrengen in een productie-domeincontroller wilt, kunt u een domeincontroller die toegewezen voor Site Recovery te gebruiken voor test-failover. Deze alleen wijzigingen aanbrengen op die domeincontroller.  
>
>

Vanaf Windows Server 2012, [extra veiligheidsmaatregelen zijn ingebouwd in Active Directory Domain Services (AD DS)](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100). Met deze beveiligingen bescherming van gevirtualiseerde domeincontrollers op basis van USN-terugdraaiacties als de onderliggende hypervisor-platform ondersteunt **VM-GenerationID**. Azure ondersteunt **VM-GenerationID**. Domeincontrollers met Windows Server 2012 of later op Azure virtuele machines hebben als gevolg hiervan, deze aanvullende veiligheidsmaatregelen nemen.


Wanneer **VM-GenerationID** opnieuw wordt ingesteld, de **InvocationID** waarde van de AD DS-database is ook opnieuw instellen. Bovendien de RID-groep is verwijderd en SYSVOL is gemarkeerd als niet-bindende. Zie voor meer informatie, [Inleiding tot virtualisatie van Active Directory Domain Services](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) en [veilig virtualiseren DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/).

Failover wordt uitgevoerd naar Azure kan leiden tot **VM-GenerationID** opnieuw in te stellen. Opnieuw instellen van **VM-GenerationID** aanvullende veiligheidsmaatregelen nemen wordt geactiveerd wanneer de domain controller virtuele machine wordt gestart in Azure. Dit kan leiden tot een *aanzienlijke vertraging* om Meld u aan bij de virtuele machine van de domain controller.

Omdat deze domeincontroller alleen in een test-failover gebruikt wordt, veiligheidsmaatregelen voor virtualisatie niet noodzakelijk. Om ervoor te zorgen dat de **VM-GenerationID** waarde voor de virtuele machine van de domain controller niet wijzigen, kunt u de waarde van de volgende DWORD naar **4** in de lokale domeincontroller:


`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\gencounter\Start`


#### <a name="symptoms-of-virtualization-safeguards"></a>Symptomen van veiligheidsmaatregelen voor virtualisatie

Als de virtualisatiebeveiliging geactiveerd na een testfailover, ziet u mogelijk een of meer van de volgende symptomen:  

* De **GenerationID** waarde wijzigingen.

    ![Wijziging van generatie-ID](./media/site-recovery-active-directory/Event2170.png)

* De **InvocationID** waarde wijzigingen.

    ![Wijziging van de aanroep-ID](./media/site-recovery-active-directory/Event1109.png)

* SYSVOL en NETLOGON-shares zijn niet beschikbaar.

    ![SYSVOL-share](./media/site-recovery-active-directory/sysvolshare.png)

    ![NtFrs SYSVOL](./media/site-recovery-active-directory/Event13565.png)

* DFSR-databases worden verwijderd.

    ![DFSR-databases zijn verwijderd](./media/site-recovery-active-directory/Event2208.png)


### <a name="troubleshoot-domain-controller-issues-during-test-failover"></a>Domeincontroller oplossen tijdens de testfailover oplossen

> [!IMPORTANT]
> Sommige van de configuraties die worden beschreven in deze sectie zijn niet standaard configuraties met een domeincontroller. Als u niet dat deze wijzigingen aanbrengen in een productie-domeincontroller wilt, kunt u een domeincontroller die toegewezen voor Site Recovery-test-failover. Breng de wijzigingen alleen voor die specifieke domeincontroller.  
>
>

1. Voer bij de opdrachtprompt de volgende opdracht om te controleren of SYSVOL en NETLOGON-mappen worden gedeeld:

    `NET SHARE`

2. Voer de volgende opdracht uit om ervoor te zorgen dat de domeincontroller goed werkt bij de opdrachtprompt:

    `dcdiag /v > dcdiag.txt`

3. Zoek de volgende tekst in het uitvoerlogboek voor. De tekst wordt bevestigd dat de domeincontroller correct werkt.

    * "doorgegeven test connectiviteit"
    * "doorgegeven test reclame"
    * "doorgegeven test MachineAccount"

Als de vorige voorwaarden wordt voldaan, is het waarschijnlijk dat de domeincontroller correct werkt. Als dat niet het geval is, kunt u de volgende stappen:

1. Voer een terugzetbewerking van de domeincontroller. Houd rekening met de volgende informatie:
    * Hoewel u kunt beter geen [FRS-replicatie](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), als u FRS-replicatie, volgt u de stappen voor een bindende terugzetbewerking. Het proces wordt beschreven in [met behulp van de registersleutel BurFlags om opnieuw te initialiseren van File Replication-Service](https://support.microsoft.com/kb/290762).

        Zie het blogbericht voor meer informatie over BurFlags [D2 en D4: Wat is het voor?](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * Als u replicatie voor DFS-replicatie gebruikt, moet u de stappen voor een bindende terugzetbewerking uitvoeren. Het proces wordt beschreven in [een bindende en niet-bindende synchronisatie afdwingen voor DFSR gerepliceerd SYSVOL (like "D4/D2' voor FRS)](https://support.microsoft.com/kb/2218556).

        U kunt ook de PowerShell-functies gebruiken. Zie voor meer informatie, [DFSR-SYSVOL gezaghebbende/niet-bindende terugzetbewerking PowerShell functies](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/).

2. De initiële synchronisatie-vereiste omzeilen door de volgende registersleutel in te stellen **0** in de lokale domeincontroller. Als de DWORD-waarde niet bestaat, kunt u deze onder de **Parameters** knooppunt.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations`

    Zie voor meer informatie, [oplossen van DNS-gebeurtenis-ID 4013: de DNS-server kan niet worden geladen is AD geïntegreerde DNS-zones](https://support.microsoft.com/kb/2001093).

3. De vereiste dat een globale-catalogusserver beschikbaar zijn voor het valideren van de gebruikersaanmelding uitschakelen. Om dit te doen, in de on-premises domeincontroller, kunt u de volgende registersleutel instellen op **1**. Als de DWORD-waarde niet bestaat, kunt u deze onder de **Lsa** knooppunt.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures`

    Zie voor meer informatie, [uitschakelen van de vereiste dat een globale-catalogusserver beschikbaar zijn voor het valideren van gebruikersaanmeldingen](http://support.microsoft.com/kb/241789).

### <a name="dns-and-domain-controller-on-different-machines"></a>DNS- en domeincontroller op verschillende computers

Als u de domeincontroller en DNs op dezelfde virtuele machine, kunt u deze procedure overslaan.


Als DNS bevindt zich niet op dezelfde virtuele machine als de domeincontroller, moet u een DNS-VM voor de testfailover maken. U kunt een nieuwe DNS-server gebruiken en de vereiste zones maken. Als uw Active Directory-domein contoso.com is, kunt u bijvoorbeeld een DNS-zone maken met de naam contoso.com. De items die overeenkomen met de Active Directory moeten worden bijgewerkt in DNS als volgt:

1. Zorg ervoor dat deze instellingen voldaan is voordat een andere virtuele machine in het herstelplan wordt gestart:
   * De zone moet de naam achter de naam van de hoofdmap forest.
   * De zone moet bestandsondersteuning.
   * De zone moet worden ingeschakeld voor veilige en niet beveiligde updates.
   * De conflictoplosser van de virtuele machine die als host fungeert voor de domeincontroller moet verwijzen naar het IP-adres van de DNS-virtuele machine.

2. Voer de volgende opdracht uit op de virtuele machine die als host fungeert voor de domeincontroller:

    `nltest /dsregdns`

3. Voer de volgende opdrachten voor het toevoegen van een zone op de DNS-server en een vermelding voor de zone toevoegen aan DNS niet-beveiligde updates toestaan:

    `dnscmd /zoneadd contoso.com  /Primary`

    `dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1`

    `dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>`

    `dnscmd /config contoso.com /allowupdate 1`

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [enterprise werklasten beveiligen met Azure Site Recovery](site-recovery-workload.md).
