---
title: Oplossen van problemen met replicatie voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat informatie over probleemoplossing voor algemene problemen met replicatie tijdens herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 06/27/2019
ms.author: mayg
ms.openlocfilehash: ed04c21fc5f3aecb91483dbd1eb7ca5fbf47c3e9
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67805967"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>Problemen met replicatie voor virtuele VMware-machines en fysieke servers

Dit artikel beschrijft enkele veelvoorkomende problemen en specifieke fouten die optreden mogelijk wanneer u on-premises VMware-machines en fysieke servers naar Azure met repliceren [siteherstel](site-recovery-overview.md).

## <a name="step-1-monitor-process-server-health"></a>Stap 1: Status van proces bewaken

Site Recovery gebruikt de [processerver](vmware-physical-azure-config-process-server-overview.md#process-server) ontvangen en optimaliseren van gerepliceerde gegevens en deze verzenden naar Azure.

Het is raadzaam dat u de status van processervers in de portal, om ervoor te zorgen dat ze zijn verbonden en goed werkt en dat de replicatie vordert voor de bronmachines die zijn gekoppeld aan de processerver bewaken.

- [Meer informatie over](vmware-physical-azure-monitor-process-server.md) processervers bewaking.
- [Best praktijken beoordelen](vmware-physical-azure-troubleshoot-process-server.md#best-practices-for-process-server-deployment)
- [Problemen oplossen](vmware-physical-azure-troubleshoot-process-server.md#check-process-server-health) status van de processerver.

## <a name="step-2-troubleshoot-connectivity-and-replication-issues"></a>Stap 2: Oplossen van problemen met clientverbindingen en replicatie

Initiële en lopende replicatiefouten worden vaak veroorzaakt door problemen met de netwerkverbinding tussen de bron- en de processerver of tussen de processerver en Azure. 

Deze problemen oplossen [problemen oplossen met connectiviteit en replicatie](vmware-physical-azure-troubleshoot-process-server.md#check-connectivity-and-replication).




## <a name="step-3-troubleshoot-source-machines-that-arent-available-for-replication"></a>Stap 3: Bronmachines die niet beschikbaar zijn voor replicatie oplossen

Wanneer u probeert om te selecteren van de bronmachine replicatie in te schakelen met behulp van Site Recovery, de computer mogelijk niet beschikbaar voor een van de volgende redenen:

* **Twee virtuele machines met hetzelfde exemplaar UUID**: Als twee virtuele machines onder het vCenter hetzelfde exemplaar UUID hebt, wordt de eerste virtuele machine is gedetecteerd door de configuratieserver wordt weergegeven in de Azure-portal. U lost dit probleem, zorg ervoor dat er geen twee virtuele machines hetzelfde exemplaar UUID. In dit scenario is vaak zichtbaar in gevallen waar een back-virtuele machine actief en is aangemeld bij onze discovery-gegevens. Raadpleeg [Azure Site Recovery van VMware naar Azure: Het opschonen van dubbele of verouderde vermeldingen](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) om op te lossen.
* **Onjuiste vCenter gebruikersreferenties**: Zorg ervoor dat u de referenties van de juiste vCenter toegevoegd bij het instellen van de configuratieserver met behulp van de OVF-sjabloon of geïntegreerde setup. Als u wilt controleren of de referenties die u hebt toegevoegd tijdens de installatie, Zie [referenties wijzigen voor automatische detectie](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
* **onvoldoende bevoegdheden voor vCenter**: Als de machtigingen voor toegang tot vCenter die niet de vereiste machtigingen hebt, is mislukt voor het detecteren van virtuele machines kan optreden. Zorg ervoor dat de machtigingen die worden beschreven in [een account voorbereiden voor automatische detectie](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) worden toegevoegd aan het account van de vCenter-gebruiker.
* **Azure Site Recovery-beheerservers**: Als de virtuele machine wordt gebruikt als de beheerserver in een of meer van de volgende rollen - configuratie server /scale-out processerver / Master-doelserver, en vervolgens kunt u zich niet kiezen van de virtuele machine uit de portal. Beheerservers kunnen niet worden gerepliceerd.
* **Al beveiligd/een failover via Azure Site Recovery-services**: Als de virtuele machine is al beveiligd of failover met Site Recovery, wordt de virtuele machine niet beschikbaar om te selecteren voor beveiliging in de portal. Zorg ervoor dat de virtuele machine die u in de portal zoekt al is niet beveiligd door een andere gebruiker of onder een ander abonnement.
* **niet verbonden vCenter**: Controleer of vCenter verbonden status heeft is. Als u wilt controleren, gaat u naar de Recovery Services-kluis > Site Recovery-infrastructuur > configuratieservers > Klik op de server van de desbetreffende configuratie > Er wordt een blade geopend aan de rechterkant met details van gekoppelde servers. Controleer of vCenter is verbonden. Als deze zich in een status 'Niet verbonden', los het probleem en vervolgens [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server) in de portal. Na deze wordt virtuele machine weergegeven op de portal.
* **ESXi uitgeschakeld**: Als ESXi-host waarop de virtuele machine zich bevindt zich in de status, uitgeschakeld vervolgens virtuele machine, worden niet weergegeven of niet worden geselecteerd in de Azure portal. Inschakelen van de ESXi-host [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server) in de portal. Na deze wordt virtuele machine weergegeven op de portal.
* **Opnieuw opstarten**: Als er een opnieuw opstarten in behandeling op de virtuele machine, zal klikt u vervolgens u niet mogelijk om te selecteren van de machine in Azure portal. Zorg ervoor dat voor het voltooien van de activiteiten opnieuw opstarten in behandeling [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Na deze wordt virtuele machine weergegeven op de portal.
* **IP is niet gevonden**: Als de virtuele machine niet een geldig IP-adres dat is gekoppeld, klikt u vervolgens kunt u zich niet kunt selecteren van de machine in Azure portal. Zorg ervoor dat u een geldig IP-adres toewijzen aan de virtuele machine, [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Na deze wordt virtuele machine weergegeven op de portal.

### <a name="troubleshoot-protected-virtual-machines-greyed-out-in-the-portal"></a>Beveiligde virtuele machines in de portal grijs oplossen

Virtuele machines die worden gerepliceerd in Site Recovery niet beschikbaar zijn in de Azure-portal als er dubbele vermeldingen in het systeem. Raadpleeg voor meer informatie over het verwijderen van verouderde vermeldingen en los het probleem, [Azure Site Recovery van VMware naar Azure: Het opschonen van dubbele of verouderde vermeldingen](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx).

## <a name="common-errors-and-solutions"></a>Veelvoorkomende fouten en oplossingen

### <a name="initial-replication-issues-error-78169"></a>Problemen met de initiële replicatie [fout 78169]

Via een bovenstaande ervoor zorgen dat er zijn geen connectiviteit, bandbreedte of tijd synchroniseren verwante problemen, zorg ervoor dat:

- Er is geen antivirussoftware wordt geblokkeerd door Azure Site Recovery. Informatie over [meer](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) op uitsluitingen die vereist zijn voor Azure Site Recovery.

### <a name="missing-app-consistent-recovery-points-error-78144"></a>Ontbrekende app-consistente herstelpunten [fout 78144]

 Dit gebeurt vanwege problemen met Volume Shadow copy Service (VSS). U lost dit als volgt op: 
 
- Controleer of de geïnstalleerde versie van de Azure Site Recovery-agent is ten minste 9.22.2. 
- Controleren of de VSS-Provider is geïnstalleerd als een service in Windows-Services en Controleer ook of de MMC Component-Service om te controleren of Azure Site Recovery VSS Provider wordt weergegeven.
- Als de VSS-Provider niet is geïnstalleerd, raadpleegt u de [installatiefout probleemoplossingsartikel](vmware-azure-troubleshoot-push-install.md#vss-installation-failures).

- Als de VSS is uitgeschakeld,
    - Controleren of het opstarttype van de VSS-Provider-service is ingesteld op **automatische**.
    - Start opnieuw op de volgende services:
        - VSS-service
        - Azure Site Recovery VSS Provider
        - VDS-service

- Als u SQL- of Exchange-workloads uitvoert, controleert u de logboeken van deze toepassing schrijvers voor fouten. Veelvuldig fouten en hun oplossing zijn opgenomen in de volgende artikelen:
    -  [Auto-Close-optie van SQL Server-database is ingesteld op TRUE](https://support.microsoft.com/help/4504104)
    - [SQL Server 2008 R2 genereren van een niet-herstelbare fout](https://support.microsoft.com/help/4504103)
    - [Bekend probleem in SQL Server 2016 en 2017](https://support.microsoft.com/help/4493364)
    - [Veel voorkomende problemen met Exchange-Servers 2013 en 2016](https://support.microsoft.com/help/4037535)


### <a name="source-machines-with-high-churn-error-78188"></a>Bronmachines met hoge verloop [fout 78188]

Mogelijke oorzaken:
- De veranderingssnelheid van gegevens (geschreven bytes per seconde) op de vermelde schijven van de virtuele machine is meer dan de [Azure Site Recovery ondersteunde limieten](site-recovery-vmware-deployment-planner-analyze-report.md#azure-site-recovery-limits) voor opslagaccounttype voor de replicatie van doel.
- Er is een onverwachte piek in het verloop vanwege die grote hoeveelheden gegevens is in behandeling voor het uploaden.

Het probleem kunt oplossen:
- Zorg ervoor dat het account opslagtype (Standard of Premium) volgens de vereisten voor het tarief van verloop bij de bron is ingericht.
- Als het waargenomen verloop tijdelijk is, wacht u enkele uren voor het uploaden van de in behandeling gegevens voor meer informatie en om herstelpunten te maken.
- Als het probleem zich voordoen blijft, gebruikt u de Site Recovery [implementatieplanner](site-recovery-deployment-planner.md#overview) om u te helpen bij het plannen van replicatie.

### <a name="source-machines-with-no-heartbeat-error-78174"></a>Bronmachines met geen heartbeat [fout 78174]

Dit gebeurt wanneer de Azure Site Recovery Mobility-agent op de bronmachine niet communiceert met de configuratieserver (CS).

Los het probleem, gebruikt u de volgende stappen uit om te controleren of de netwerkverbinding van de bron-VM naar de Config-Server:

1. Controleren of de bronmachine wordt uitgevoerd.
2. Aanmelden bij de bron-VM met een account dat beheerdersrechten heeft.
3. Controleer of de volgende services worden uitgevoerd en als de services niet opnieuw te starten:
   - Svagents (InMage Scout VX Agent)
   - InMage Scout Application Service
4. Controleer de logboeken op de locatie voor de details van fouten op de bronmachine:

       C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log
    
### <a name="process-server-with-no-heartbeat-error-806"></a>Processerver met geen heartbeat [fout 806]
Als er is geen heartbeat van de proces-Server (PS), controleert u of:
1. PS Virtuele machine actief is
2. Controleer de volgende logboeken op de PS Foutdetails:

       C:\ProgramData\ASR\home\svsystems\eventmanager*.log
       and
       C:\ProgramData\ASR\home\svsystems\monitor_protection*.log

### <a name="master-target-server-with-no-heartbeat-error-78022"></a>Hoofddoelserver met geen heartbeat [fout 78022]

Dit gebeurt wanneer de Azure Site Recovery Mobility-agent op de Hoofddoelserver communiceert niet met de configuratieserver.

Gebruik de volgende stappen om te controleren of de status van het probleem op te lossen:

1. Controleren of de hoofd-doel-VM wordt uitgevoerd.
2. Meld u aan de hoofd-doel-VM met een account dat beheerdersrechten heeft.
    - Controleren of de svagents-service wordt uitgevoerd. Als deze wordt uitgevoerd, start de service opnieuw.
    - Raadpleeg de logboeken op de locatie voor de details van fout:
        
          C:\Program Files (X86)\Microsoft Azure Site Recovery\agent\svagents*log

## <a name="error-id-78144---no-app-consistent-recovery-point-available-for-the-vm-in-the-last-xxx-minutes"></a>Fout-ID 78144 - er is geen toepassingsconsistent herstelpunt beschikbaar voor de virtuele machine in de laatste 'XXX' minuten

Enkele van de meest voorkomende problemen worden hieronder vermeld.

#### <a name="cause-1-known-issue-in-sql-server-20082008-r2"></a>1 oorzaak: Bekend probleem in SQL server 2008/2008 R2 
**Voor het oplossen van** : Er is een bekend probleem met SQL server 2008/2008 R2. Raadpleeg dit KB-artikel [Azure Site Recovery-Agent of andere niet-onderdeel VSS back-up mislukt voor een server waarop SQL Server 2008 R2](https://support.microsoft.com/help/4504103/non-component-vss-backup-fails-for-server-hosting-sql-server-2008-r2)

#### <a name="cause-2-azure-site-recovery-jobs-fail-on-servers-hosting-any-version-of-sql-server-instances-with-autoclose-dbs"></a>2 oorzaak: Azure Site Recovery-taken mislukken op servers die als host fungeert voor een willekeurige versie van SQL Server-exemplaren met Auto_sluiten DB 's 
**Voor het oplossen van** : Raadpleeg de Kb [artikel](https://support.microsoft.com/help/4504104/non-component-vss-backups-such-as-azure-site-recovery-jobs-fail-on-ser) 


#### <a name="cause-3-known-issue-in-sql-server-2016-and-2017"></a>3 oorzaak: Bekend probleem in SQL Server 2016 en 2017
**Voor het oplossen van** : Raadpleeg de Kb [artikel](https://support.microsoft.com/help/4493364/fix-error-occurs-when-you-back-up-a-virtual-machine-with-non-component) 


### <a name="more-causes-due-to-vss-related-issues"></a>Problemen met betrekking tot meer oorzaken vanwege VSS:

Om het probleem verder oplossen, controleert u de bestanden op de broncomputer om de exacte foutcode van de fout:
    
    C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\Application Data\ApplicationPolicyLogs\vacp.log

Hoe kan ik de fouten in het bestand vinden?
Zoeken naar de tekenreeks 'vacpError' door het openen van het bestand vacp.log in een editor naar
        
    Ex: vacpError:220#Following disks are in FilteringStopped state [\\.\PHYSICALDRIVE1=5, ]#220|^|224#FAILED: CheckWriterStatus().#2147754994|^|226#FAILED to revoke tags.FAILED: CheckWriterStatus().#2147754994|^|

In het bovenstaande voorbeeld **2147754994** wordt de foutcode die u over de fout, vertelt zoals hieronder wordt weergegeven

#### <a name="vss-writer-is-not-installed---error-2147221164"></a>VSS-schrijver is niet geïnstalleerd - fout 2147221164 

*Voor het oplossen van*: Azure Site Recovery gebruikt voor het genereren van de toepassing consistentie tag Microsoft Volume Shadow copy Service (VSS). Een VSS-Provider voor de werking ervan om consistentie van momentopnamen voor een app te worden geïnstalleerd. Deze VSS-Provider wordt geïnstalleerd als een service. In het geval de VSS-Provider-service niet is geïnstalleerd, is het maken van de momentopname van de toepassing consistentie mislukt met de foutcode 0x80040154 'Klasse is niet geregistreerd'. </br>
Raadpleeg [artikel voor de VSS writer installatieproblemen oplossen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-troubleshoot-push-install#vss-installation-failures) 

#### <a name="vss-writer-is-disabled---error-2147943458"></a>VSS-schrijver wordt uitgeschakeld - fout 2147943458

**Voor het oplossen van**: Azure Site Recovery gebruikt voor het genereren van de toepassing consistentie tag Microsoft Volume Shadow copy Service (VSS). Een VSS-Provider voor de werking ervan om consistentie van momentopnamen voor een app te worden geïnstalleerd. Deze VSS-Provider wordt geïnstalleerd als een service. In het geval de VSS-Provider-service is uitgeschakeld, mislukt het maken van de momentopname van de toepassing consistentie met de id van de fout 'de opgegeven service is uitgeschakeld en kan niet worden started(0x80070422)'. </br>

- Als de VSS is uitgeschakeld,
    - Controleren of het opstarttype van de VSS-Provider-service is ingesteld op **automatische**.
    - Start opnieuw op de volgende services:
        - VSS-service
        - Azure Site Recovery VSS Provider
        - VDS-service

####  <a name="vss-provider-notregistered---error-2147754756"></a>VSS-PROVIDER NOT_REGISTERED - fout 2147754756

**Voor het oplossen van**: Azure Site Recovery gebruikt voor het genereren van de toepassing consistentie tag Microsoft Volume Shadow copy Service (VSS). Controleer of de service Azure Site Recovery VSS Provider is geïnstalleerd of niet. </br>

- De Provider-installatie met behulp van de volgende opdrachten opnieuw uitvoeren:
- Bestaande-provider verwijderen: C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Uninstall.cmd
- Opnieuw te installeren: C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd
 
Controleren of het opstarttype van de VSS-Provider-service is ingesteld op **automatische**.
    - Start opnieuw op de volgende services:
        - VSS-service
        - Azure Site Recovery VSS Provider
        - VDS-service

## <a name="next-steps"></a>Volgende stappen

Als u meer hulp nodig hebt, stel uw vraag in de [Azure Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). We hebben een actieve community en een van onze technici u kan helpen.
