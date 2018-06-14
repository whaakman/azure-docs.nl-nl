---
title: Active Directory en DNS met Azure Site Recovery beveiligen | Microsoft Docs
description: In dit artikel wordt beschreven hoe een noodherstel voor Active Directory implementeren met behulp van Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/11/2018
ms.author: manayar
ms.openlocfilehash: 97923af5ed4191f66434166c4743e398f8ac635a
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
ms.locfileid: "34072603"
---
# <a name="use-azure-site-recovery-to-protect-active-directory-and-dns"></a>Azure Site Recovery gebruiken om te beveiligen van Active Directory en DNS

Enterprise-toepassingen zoals SharePoint, Dynamics AX en SAP, is afhankelijk van Active Directory en DNS-infrastructuur te laten functioneren. Wanneer u herstel na noodgevallen voor toepassingen instelt, moet u vaak Active Directory en DNS herstellen voordat u andere toepassingsonderdelen, zodat de juiste toepassingsfunctionaliteit herstellen.

U kunt [siteherstel](site-recovery-overview.md) voor het maken van een noodherstelplan voor Active Directory. Wanneer er een onderbreking van de optreedt, kunt u een failover starten. U kunt Active Directory up en wordt uitgevoerd in een paar minuten hebben. Als u Active Directory geïmplementeerd voor meerdere toepassingen in uw primaire site, bijvoorbeeld kunt voor SharePoint en SAP, u failover voor de volledige site. U kunt eerst Active Directory ite herstel met failover. Vervolgens failover uit op de andere toepassingen met behulp van toepassingsspecifieke herstelplannen.

In dit artikel wordt uitgelegd hoe het maken van een noodherstel voor Active Directory. Het bevat de vereisten en het failover-instructies. U moet bekend bent met Active Directory en de Site Recovery voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Als u naar Azure repliceert, [voorbereiden van de Azure-resources](tutorial-prepare-azure.md), met inbegrip van een abonnement, een virtueel netwerk van Azure storage-account en een Recovery Services-kluis.
* Raadpleeg de [ondersteuningsvereisten](site-recovery-support-matrix-to-azure.md) voor alle onderdelen.

## <a name="replicate-the-domain-controller"></a>De domeincontroller worden gerepliceerd.

U moet instellen [replicatie van Site Recovery](#enable-protection-using-site-recovery), op ten minste één virtuele machine die als host fungeert voor een DNS- of domeincontroller. Als u hebt [meerdere domeincontrollers](#environment-with-multiple-domain-controllers) in uw omgeving, ook moet u instellen een [extra domeincontroller](#protect-active-directory-with-active-directory-replication) op de doelsite. De extra domeincontroller kan worden in Azure of in een secundaire on-premises datacenter.

### <a name="single-domain-controller"></a>Één domeincontroller
Als u slechts enkele toepassingen en één domeincontroller hebt, kunt u tegelijk failover wordt de hele site. In dit geval wordt u aangeraden Site Recovery op de domeincontroller worden gerepliceerd naar de doelsite (ofwel in Azure of in een secundaire on-premises datacenter). U kunt de dezelfde gerepliceerde domeincontroller of de DNS-virtuele machine voor [testfailover](#test-failover-considerations).

### <a name="multiple-domain-controllers"></a>Meerdere domeincontrollers
Als u veel toepassingen en meer dan één domeincontroller in uw omgeving hebt of als u van plan bent failover enkele toepassingen op een tijdstip daarnaast voor het repliceren van de domain controller virtuele machine met Site Recovery, raden wij aan dat u een instellen[extra domeincontroller](#protect-active-directory-with-active-directory-replication) op de doelsite (ofwel in Azure of in een secundaire on-premises datacenter). Voor [testfailover](#test-failover-considerations), kunt u de domeincontroller die gerepliceerd door Site Recovery. Voor failover, kunt u de extra domeincontroller op de doelsite.

## <a name="enable-protection-with-site-recovery"></a>Schakel de beveiliging met Site Recovery

Site Recovery kunt u de virtuele machine die als host fungeert voor de domeincontroller of de DNS-beveiligen.

### <a name="protect-the-vm"></a>Beveiligen van de virtuele machine
De domeincontroller die wordt gerepliceerd met behulp van Site Recovery wordt gebruikt voor [testfailover](#test-failover-considerations). Zorg ervoor dat het voldoet aan de volgende vereisten:

1. De domeincontroller is een globale catalogusserver.
2. De domeincontroller moet de FSMO-roleigenaar voor functies die nodig zijn tijdens een testfailover. Anders wordt deze rollen moet worden [overgenomen](http://aka.ms/ad_seize_fsmo) na de failover.

### <a name="configure-vm-network-settings"></a>VM-netwerkinstellingen configureren
Voor de virtuele machine die als host fungeert voor de domeincontroller of de DNS-server, in Site Recovery netwerkinstellingen onder de **berekening en netwerk** instellingen van de gerepliceerde virtuele machine. Dit zorgt ervoor dat de virtuele machine is gekoppeld aan het juiste netwerk na een failover.

## <a name="protect-active-directory"></a>Beveiligen van Active Directory

### <a name="site-to-site-protection"></a>Site-naar-site-beveiliging
Maak een domeincontroller op de secundaire site. Wanneer u de server naar een domeincontroller promoveert, geef de naam van het domein dat wordt gebruikt op de primaire site. U kunt de **Active Directory: Sites en Services** -module voor het configureren van instellingen voor het siteobject koppeling waaraan de sites worden toegevoegd. U kunt bepalen wanneer er replicatie plaatsvindt tussen twee of meer sites en hoe vaak vindt plaats voor informatie over het configureren van instellingen op een site-koppeling. Zie voor meer informatie [replicatie tussen sites plannen](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Beveiliging van de site naar Azure
Maak eerst een domeincontroller in een Azure-netwerk. Wanneer u de server naar een domeincontroller promoveert, moet u de dezelfde domeinnaam die wordt gebruikt op de primaire site opgeven.

Configureer vervolgens opnieuw de DNS-server voor het virtuele netwerk gebruik van de DNS-server in Azure.

![Azure-netwerk](./media/site-recovery-active-directory/azure-network.png)

### <a name="azure-to-azure-protection"></a>Beveiliging van Azure naar Azure
Maak eerst een domeincontroller in een Azure-netwerk. Wanneer u de server naar een domeincontroller promoveert, moet u de dezelfde domeinnaam die wordt gebruikt op de primaire site opgeven.

Configureer vervolgens opnieuw de DNS-server voor het virtuele netwerk gebruik van de DNS-server in Azure.

## <a name="test-failover-considerations"></a>Testfailover-overwegingen
Om te voorkomen dat gevolgen voor de productie-workloads, doet testfailover zich in een netwerk dat is geïsoleerd van het productienetwerk.

De meeste toepassingen vereist de aanwezigheid van een domeincontroller of een DNS-server. Daarom voordat de toepassing een failover uitvoert, moet u een domeincontroller in het geïsoleerde netwerk moet worden gebruikt voor de testfailover. De eenvoudigste manier om dit te doen is met Site Recovery voor een virtuele machine die als host fungeert voor een domeincontroller of de DNS-replicatie. Voer vervolgens een testfailover van de domain controller virtuele machine voordat u een testfailover uitgevoerd van het herstelplan voor de toepassing uitvoert. Hier ziet u hoe u dat doen:

1. Gebruik Site Recovery voor [repliceren](vmware-azure-tutorial.md) de virtuele machine die als host fungeert voor de domeincontroller of DNS.
2. Maak een geïsoleerd netwerk. Een virtueel netwerk die u in Azure maakt is geïsoleerd van andere netwerken standaard. Het is raadzaam dat u hetzelfde IP-adresbereik voor dit netwerk die u in uw productienetwerk gebruikt. Geen site-naar-site-connectiviteit op dit netwerk inschakelen.
3. Geef een DNS IP-adres in het geïsoleerde netwerk. Gebruik het IP-adres die u verwacht dat de DNS-virtuele machine om op te halen. Als u naar Azure repliceert, geeft u het IP-adres voor de virtuele machine die wordt gebruikt op failover. Het IP-adres, in de gerepliceerde virtuele machine, invoeren in de **berekening en netwerk** -instellingen, selecteert de **IP-adres doel** instellingen.

    ![Azure testnetwerk](./media/site-recovery-active-directory/azure-test-network.png)

    > [!TIP]
    > Site Recovery probeert te maken van test-virtuele machines in een subnet met dezelfde naam en met behulp van hetzelfde IP-adres die opgegeven in de **berekening en netwerk** instellingen van de virtuele machine. Als een subnet met dezelfde naam is niet beschikbaar in het Azure-netwerk dat opgegeven voor de testfailover, wordt de virtuele testmachine gemaakt in het alfabetisch eerste subnet.
    >
    > Als het doel-IP-adres deel van het geselecteerde subnet uitmaakt, probeert de Site Recovery de failover de virtuele machine maken met behulp van het doel-IP-adres. Als het doel-IP-adres maakt geen deel uit van het geselecteerde subnet, wordt de failover de virtuele machine gemaakt met behulp van het volgende beschikbare IP-adres in het geselecteerde subnet.
    >
    >

### <a name="test-failover-to-a-secondary-site"></a>Failover naar een secundaire site testen

1. Als u naar een andere lokale site repliceert en het gebruik van DHCP, [DNS en DHCP instellen voor een testfailover](hyper-v-vmm-test-failover.md#prepare-dhcp).
2. Voer een testfailover van de domain controller virtuele machine die wordt uitgevoerd in een geïsoleerd netwerk. Gebruik de meest recente beschikbare *toepassing consistente* herstelpunt van de domain controller virtuele machine uitvoeren van de testfailover.
3. Een testfailover voor het herstelplan met virtuele machines die de toepassing wordt uitgevoerd op uitvoeren.
4. Wanneer de test is voltooid, *opruimen van de testfailover* op de virtuele machine van de domain controller. Hiermee verwijdert u de domeincontroller die is gemaakt voor de testfailover.


### <a name="remove-references-to-other-domain-controllers"></a>Verwijzingen naar andere domeincontrollers verwijderen
Wanneer u een testfailover initieert, niet alle domeincontrollers in het testnetwerk opnemen. Verwijder verwijzingen naar andere domeincontrollers die bestaan in uw productieomgeving, moet u mogelijk [overnemen van FSMO-Active Directory-functies](http://aka.ms/ad_seize_fsmo) en [metagegevens opruimen](https://technet.microsoft.com/library/cc816907.aspx) ontbrekende domeincontrollers .


### <a name="issues-caused-by-virtualization-safeguards"></a>Problemen veroorzaakt door de veiligheidsmaatregelen voor virtualisatie

> [!IMPORTANT]
> Enkele van de configuraties beschreven in deze sectie zijn niet standaard configuraties met een domeincontroller. Als u niet dat deze wijzigingen aanbrengen in een productie-domeincontroller wilt, kunt u een domeincontroller die toegewezen voor Site Recovery te gebruiken voor de testfailover. Deze alleen wijzigingen aanbrengen op die domeincontroller.  
>
>

Vanaf Windows Server 2012 [extra beveiliging zijn ingebouwd in Active Directory Domain Services (AD DS)](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100). Met deze beveiligingen bescherming van gevirtualiseerde domeincontrollers tegen USN-terugdraaiacties door als het onderliggende hypervisor-platform ondersteunt **Vmgid**. Azure ondersteunt **Vmgid**. Als gevolg hiervan hebben domeincontrollers met Windows Server 2012 of later op Azure virtual machines deze extra beveiligingsmaatregelen.


Wanneer **Vmgid** opnieuw wordt ingesteld, de **InvocationID** waarde van de AD DS-database ook opnieuw ingesteld. Bovendien worden de RID-groep is verwijderd en SYSVOL is gemarkeerd als niet-bindende. Zie voor meer informatie [Inleiding tot virtualisatie van Active Directory Domain Services](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) en [veilig virtualiseren DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/).

Mogelijk worden niet via Azure **Vmgid** opnieuw in te stellen. Opnieuw instellen van **Vmgid** aanvullende veiligheidsmaatregelen wanneer de domain controller virtuele machine wordt opgestart in Azure. Dit kan leiden tot een *aanzienlijke vertraging* kunt aanmelden bij de virtuele machine van de domain controller.

Omdat deze domeincontroller alleen in een testfailover veiligheidsmaatregelen voor virtualisatie niet noodzakelijk gebruikt wordt. Om ervoor te zorgen dat de **Vmgid** waarde voor de domain controller virtuele machine niet wijzigen, kunt u de waarde van de volgende DWORD naar **4** in de lokale domeincontroller:


`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\gencounter\Start`


#### <a name="symptoms-of-virtualization-safeguards"></a>Symptomen van veiligheidsmaatregelen voor virtualisatie

Als de virtualisatiebeveiliging geactiveerd na een failovertest uitvoert, ziet u mogelijk een of meer van de volgende symptomen:  

* De **GenerationID** waarde wijzigingen.

    ![Wijziging van generatie-ID](./media/site-recovery-active-directory/Event2170.png)

* De **InvocationID** waarde wijzigingen.

    ![Wijziging van de aanroep-ID](./media/site-recovery-active-directory/Event1109.png)

* SYSVOL en de NETLOGON-shares zijn niet beschikbaar.

    ![SYSVOL-share](./media/site-recovery-active-directory/sysvolshare.png)

    ![NtFrs SYSVOL](./media/site-recovery-active-directory/Event13565.png)

* DFSR-databases verwijderd.

    ![DFSR-databases zijn verwijderd](./media/site-recovery-active-directory/Event2208.png)


### <a name="troubleshoot-domain-controller-issues-during-test-failover"></a>Problemen met domain controller tijdens de testfailover

> [!IMPORTANT]
> Enkele van de configuraties beschreven in deze sectie zijn niet standaard configuraties met een domeincontroller. Als u niet dat deze wijzigingen aanbrengen in een productie-domeincontroller wilt, kunt u een domeincontroller die toegewezen voor de testfailover van Site Recovery. Breng de wijzigingen alleen voor die specifieke domeincontroller.  
>
>

1. Voer de volgende opdracht om te controleren of SYSVOL en NETLOGON mappen worden gedeeld bij de opdrachtprompt:

    `NET SHARE`

2. Voer de volgende opdracht om ervoor te zorgen dat de domeincontroller goed werkt bij de opdrachtprompt:

    `dcdiag /v > dcdiag.txt`

3. Zoek naar de volgende tekst in het uitvoerlogboek voor. De tekst wordt bevestigd dat de domeincontroller correct functioneert.

    * 'doorgegeven test connectiviteit'
    * 'doorgegeven test reclame'
    * 'doorgegeven test MachineAccount'

Als u de bovenstaande voorwaarden is voldaan, is het waarschijnlijk dat de domeincontroller correct functioneert. Als dit niet het geval is, kunt u de volgende stappen:

1. Voer een bindend terugzetten van de domeincontroller. Houd rekening met de volgende informatie:
    * Hoewel niet aangeraden [FRS-replicatie](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), als u FRS-replicatie, volg de stappen voor een bindende terugzetbewerking. Het proces wordt beschreven in [met behulp van de registersleutel BurFlags File Replication-Service opnieuw initialiseren](https://support.microsoft.com/kb/290762).

        Zie het blogbericht voor meer informatie over BurFlags [D2 en D4: Wat is het voor?](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * Als u de DFSR-replicatie gebruikt, volg u de stappen voor een bindende terugzetbewerking. Het proces wordt beschreven in [een bindende en niet-bindende synchronisatie afdwingen voor DFSR gerepliceerd SYSVOL (like "D4/D2' voor FRS)](https://support.microsoft.com/kb/2218556).

        U kunt ook de PowerShell-functies gebruiken. Zie voor meer informatie [DFSR SYSVOL gezaghebbende/niet-bindende terugzetbewerking PowerShell functies](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/).

2. De eerste synchronisatie-vereiste omzeilen door de volgende registersleutel in te stellen op **0** in de lokale domeincontroller. Als de DWORD-waarde niet bestaat, kunt u het maken onder de **Parameters** knooppunt.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations`

    Zie voor meer informatie [problemen met DNS-gebeurtenis-ID 4013: de DNS-server kan niet worden geladen is AD geïntegreerde DNS-zones](https://support.microsoft.com/kb/2001093).

3. Schakel de vereiste dat een globale-catalogusserver beschikbaar zijn voor het valideren van de gebruikersaanmelding. Klik hiervoor in de lokale domeincontroller zodanig dat de volgende registersleutel **1**. Als de DWORD-waarde niet bestaat, kunt u het maken onder de **Lsa** knooppunt.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures`

    Zie voor meer informatie [uitschakelen van de vereiste dat een globale-catalogusserver beschikbaar zijn voor het valideren van gebruikersaanmeldingen](http://support.microsoft.com/kb/241789).

### <a name="dns-and-domain-controller-on-different-machines"></a>DNS- en de domeincontroller op verschillende computers
Als DNS niet in dezelfde virtuele machine als de domeincontroller, moet u een DNS-virtuele machine voor de testfailover. Als DNS- en de domeincontroller niet op dezelfde virtuele machine, kunt u deze sectie overslaan.

U kunt een nieuwe DNS-server gebruiken en de vereiste zones maken. Als uw Active Directory-domein contoso.com is, kunt u bijvoorbeeld een DNS-zone maken met de naam contoso.com. De vermeldingen die met de Active Directory overeenkomen moeten worden bijgewerkt in DNS als volgt:

1. Zorg ervoor dat deze instellingen geïnstalleerd zijn voordat een andere virtuele machine in het herstelplan wordt gestart:
   * De zone moet de naam achter de naam van de basis-forest.
   * De zone moet bestandsondersteuning.
   * De zone moet zijn ingeschakeld voor beveiligde als onbeveiligde updates.
   * De resolver van de virtuele machine die als host fungeert voor de domeincontroller moet verwijzen naar het IP-adres van de DNS-virtuele machine.

2. Voer de volgende opdracht uit op de virtuele machine die als host fungeert voor de domeincontroller:

    `nltest /dsregdns`

3. Voer de volgende opdrachten voor het toevoegen van een zone op de DNS-server en een vermelding voor de zone toevoegen aan DNS onbeveiligde updates toestaan:

    `dnscmd /zoneadd contoso.com  /Primary`

    `dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1`

    `dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>`

    `dnscmd /config contoso.com /allowupdate 1`

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [enterprise werklasten beveiligen met Azure Site Recovery](site-recovery-workload.md).
