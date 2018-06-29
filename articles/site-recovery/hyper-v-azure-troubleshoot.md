---
title: Problemen met Hyper-V naar Azure replicatie met Azure Site Recovery | Microsoft Docs
description: Hierin wordt beschreven hoe het oplossen van problemen met Hyper-V naar Azure replicatie met Azure Site Recovery
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 04/09/2018
ms.author: rayne
ms.openlocfilehash: 8b16f495a21ff0b3da415390877777650a9fbb82
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048200"
---
# <a name="troubleshoot-hyper-v-to-azure-replication-and-failover"></a>Problemen met Hyper-V tot Azure-replicatie en failover

In dit artikel worden veelvoorkomende problemen beschreven die u tegenkomen kunt bij het repliceren van lokale Hyper-V-machines naar Azure met behulp van [Azure Site Recovery](site-recovery-overview.md).

## <a name="enable-protection-issues"></a>Problemen met beveiliging inschakelen

Als u problemen ondervindt bij het inschakelen van beveiliging voor Hyper-V-machines, controleert u het volgende:

1. Controleer of uw Hyper-V-hosts en virtuele machines in overeenstemming met alle [vereisten en voorwaarden](hyper-v-azure-support-matrix.md).
2. Als Hyper-V-servers bevinden zich in clouds voor System Center Virtual Machine Manager (VMM), controleert u of u hebt voorbereid de [VMM-server](hyper-v-prepare-on-premises-tutorial.md#prepare-vmm-optional).
3. Controleer of de Hyper-V Virtual Machine Management-service wordt uitgevoerd op Hyper-V-hosts.
4. Controleren op problemen die worden weergegeven in het logboek voor Hyper-V-VMMS\Admin op de virtuele machine. Dit logboekbestand bevindt zich in **logboeken toepassingen en Services** > **Microsoft** > **Windows**.
5. Controleer op de Gast-VM of dat WMI ingeschakeld en toegankelijk is.
  - [Meer informatie over](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/) basistest WMI.
  - [Problemen met](https://aka.ms/WMiTshooting) WMI.
  - [Problemen met ](https://technet.microsoft.com/library/ff406382.aspx#H22) problemen met WMI-scripts en services.
5. Zorg ervoor dat de nieuwste versie van de Integration Services wordt uitgevoerd op de Gast-VM.
    - [Controleer](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) dat u de meest recente versie hebt.
    - [Houd](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#keep-integration-services-up-to-date) Integration Services up-to-date.
    
## <a name="replication-issues"></a>Problemen met replicatie

Problemen oplossen met initiële en lopende replicatie als volgt:

1. Zorg ervoor dat u de [meest recente versie](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx) van Site Recovery-services.
2. Controleren of replicatie is onderbroken:
  - Controleer de status van de virtuele machine in de Hyper-V Manager-console.
  - Als het is essentieel, met de rechtermuisknop op de virtuele machine > **replicatie** > **replicatiestatus weergeven**.
  - Als replicatie is onderbroken, klikt u op **replicatie hervatten**.
3. Controleer of de vereiste services worden uitgevoerd. Als ze niet zijn, start u deze opnieuw.
    - Als u Hyper-V zonder VMM repliceert, controleert u dat deze services worden uitgevoerd op de Hyper-V-host:
        - Virtual Machine Management-service
        - Microsoft Azure Recovery Services Agent-service
        - Microsoft Azure Site Recovery-service
        - WMI-Provider Host-service
    - Als u met VMM in de omgeving repliceert, moet u controleren of deze services zijn uitgevoerd:
        - Controleer of de Virtual Machine Management-service, de Microsoft Azure Recovery Services Agent en de WMI-Provider Host-service worden uitgevoerd op de Hyper-V-host.
        - Zorg ervoor dat de System Center Virtual Machine Manager-Service wordt uitgevoerd op de VMM-server.
4. Controleer de verbinding tussen de Hyper-V-server en Azure. U doet dit door Taakbeheer te openen op de Hyper-V-host. Op de **prestaties** tabblad **Open Broncontrole**. Op de **netwerk** tabblad > **Processess met netwerkactiviteit**, Controleer of cbengine.exe actief grote volumes (MB) met gegevens verzendt.
5. Controleer of de Hyper-V-hosts verbinding met de Azure-opslag-blob-URL maken kunnen. Om dit te doen, selecteert en Controleer **cbengine.exe**. Weergave **TCP-verbindingen** om te controleren of de verbinding van de host naar de Azure storage-blob.
6. Controleer prestatieproblemen, zoals hieronder wordt beschreven.
    
### <a name="performance-issues"></a>Prestatieproblemen

Netwerk-bandbreedtebeperkingen kunnen invloed hebben op replicatie. Oplossen van problemen als volgt:

1. [Controleer](https://support.microsoft.com/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage) als er bandbreedte of beperking van beperkingen in uw omgeving.
2. Voer de [implementatie Planner profiler](hyper-v-deployment-planner-run.md).
3. Na het uitvoeren van de profiler, volgt u de [bandbreedte](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) en [opslag](hyper-v-deployment-planner-analyze-report.md#vm-storage-placement-recommendation) aanbevelingen.
4. Controleer [gegevens verloop beperkingen](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits). Als er veel gegevens verloop in op een virtuele machine, het volgende doen:
  - Controleer als uw VM is gemarkeerd voor hersynchronisatie.
  - Ga als volgt [deze stappen](https://blogs.technet.microsoft.com/virtualization/2014/02/02/hyper-v-replica-debugging-why-are-very-large-log-files-generated/) voor het onderzoeken van de bron van het verloop.
  - Verloop kan optreden als de HRL-logboekbestanden groter zijn dan 50% van de beschikbare schijfruimte. Als dit het probleem, moet u meer opslagruimte inrichten voor alle virtuele machines waarop het probleem optreedt.
  - Controleer dat of de replicatie wordt niet onderbroken. Als dit het geval is, gaat u door de wijzigingen schrijven naar de hrl-bestand, wat aan de toegenomen grootte bijdragen kan.
 

## <a name="critical-replication-state-issues"></a>Problemen met de status van de kritieke replicatie

1. Controleer de replicatiestatus van, verbinding maken met de lokale Hyper-V Manager-console, selecteert u de virtuele machine en status controleren.

    ![Replicatiestatus](media/hyper-v-azure-troubleshoot/replication-health1.png)
    

2. Klik op **replicatiestatus weergeven** om de details te bekijken:

    - Als replicatie is onderbroken, met de rechtermuisknop op de virtuele machine > **replicatie** > **replicatie hervatten**.
    - Als een virtuele machine op een Hyper-V-host geconfigureerd in Site Recovery naar een andere Hyper-V-host in hetzelfde cluster of een zelfstandige computer migreert, wordt niet replicatie voor de virtuele machine beïnvloed. NET Controleer of de nieuwe Hyper-V-host aan alle vereisten voldoet, en is geconfigureerd in Site Recovery.

## <a name="app-consistent-snapshot-issues"></a>Problemen met App-consistente momentopname

Een app-consistente momentopname is een momentopname van een punt in tijd van de toepassingsgegevens in de virtuele machine. Volume Shadow Copy Service (VSS) zorgt ervoor dat apps op de virtuele machine in een consistente status wanneer de momentopname wordt gemaakt.  Deze sectie beschrijft enkele veelvoorkomende problemen die u mogelijk ondervindt.

### <a name="vss-failing-inside-the-vm"></a>VSS is mislukt in de virtuele machine

1. Controleer of de nieuwste versie van de integratieservices geïnstalleerd en wordt uitgevoerd is.  Controleer of er een update beschikbaar is met de volgende opdracht vanaf een opdrachtprompt PowerShell op de Hyper-V-host: **get-vm | Selecteer de naam, status, IntegrationServicesState**.
2. Controleer of de VSS-services actief is en goed zijn:
    - U doet dit door zich aanmelden op de Gast-VM. Open een opdrachtprompt admin vervolgens en voer de volgende opdrachten om te controleren of alle VSS-schrijvers in orde zijn.
        - **Vssadmin lijst schrijvers**
        - **Vssadmin lijst schaduwen**
        - **Vssadmin lijst met providers**
    - Controleer de uitvoer. Als schrijvers zich in een mislukte status, het volgende doen:
        - Controleer het toepassingslogboek op de virtuele machine op fouten van VSS-bewerking.
    - Probeer het opnieuw starten van deze services die zijn gekoppeld aan de writer is mislukt:
        - Volume Shadow Copy
         - Azure Site Recovery VSS-Provider
    - Nadat u dit doet, wacht u een paar uur om te zien als toepassingsconsistente momentopnamen is gegenereerd.
    - Als een laatste toevlucht opnieuw op de virtuele machine. Dit kan mogelijk worden services die zich in niet-reagerende status opgelost.
3. Controleer er geen dynamische schijven in de virtuele machine. Dit wordt niet ondersteund voor toepassingsconsistente momentopnamen. U kunt controleren in Schijfbeheer (diskmgmt.msc).

    ![Dynamische schijf](media/hyper-v-azure-troubleshoot/dynamic-disk.png)
    
4. Controleer of u een iSCSI-schijf is gekoppeld aan de VM niet hebt. Nee, dit wordt niet ondersteund.
5. Controleer of de Backup-service is ingeschakeld. Controleer of dit op **Hyper-V-instellingen** > **Integration Services**.
6. Zorg ervoor dat er zijn geen conflicten met apps maken van VSS-momentopnamen. Als meerdere apps wilt VSS-momentopnamen worden gemaakt op de dezelfde tijd conflicten optreden. Als bijvoorbeeld een back-up app duurt VSS-momentopnamen wanneer Site Recovery door uw beleid voor wachtwoordreplicatie is gepland om een momentopname.   
7. Controleer of de virtuele machine een hoge verloop zich voordoet:
    - U kunt de dagelijkse gegevenswijzigingssnelheid meten voor de Gast-VM's met behulp van prestatiemeteritems op Hyper-V-host. U doet dit door de volgende teller inschakelen Een voorbeeld van deze waarde op de VM-schijven voor 5 tot 15 minuten Aggregrate het verloop VM ophalen.
        - Categorie: Hyper-V virtuele opslagapparaat 'apparaat'
        - Teller: ' Bytes schrijven per seconde '</br>
        - Het verloop van deze gegevens wordt verhoogd of op een hoog niveau, afhankelijk van hoeveel de virtuele machine of de apps zich blijven.
        - Het gemiddelde bron schijf-gegevensverloop is 2 MB/s voor standard-opslag voor siteherstel. [Meer informatie](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits)
    - Bovendien kunt u [schaalbaarheidsdoelen van opslag controleren](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#scalability-targets-for-a-storage-account).
8. Voer de [implementatie Planner](hyper-v-deployment-planner-run.md).
9. Bekijk de aanbevelingen voor [netwerk](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) en [opslag](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input).


### <a name="vss-failing-inside-the-hyper-v-host"></a>VSS is mislukt in de Hyper-V-Host

1. Controleer de gebeurtenislogboeken voor VSS-fouten en aanbevelingen:
    - Open het gebeurtenislogboek van Hyper-V-beheer in op de Hyper-V-hostserver **logboeken** > **logboeken toepassingen en Services** > **Microsoft**  >  **Windows** > **Hyper-V** > **Admin**.
    - Controleer of er zijn gebeurtenissen die duiden op fouten van app-consistente momentopname te maken.
    - Een veelvoorkomende fout is: "set van VSS-momentopname voor virtuele machine 'XYZ' genereren van de Hyper-V is mislukt: een tijdelijke fout opgetreden. Opnieuw starten van de VSS-service mogelijk problemen oplossen als de service niet reageert."

2. VSS-momentopnamen genereren voor de virtuele machine, Controleer of de Hyper-V-integratieservices zijn geïnstalleerd op de virtuele machine en dat de back-up (VSS) Integration-Service is ingeschakeld.
    - Zorg ervoor dat de Integration Services VSS-service/daemons worden uitgevoerd op de Gast en zich in een **OK** status.
    - U kunt dit controleren vanuit een PowerShell-sessie met verhoogde bevoegdheid op de Hyper-V-host met de opdracht **et VMIntegrationService - VMName<VMName>-naam VSS** ook kunt u deze informatie vinden door de aanmelding bij de Gast-VM. [Meer informatie](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services).
    - Zorg ervoor dat de back-up/VSS-integratieservices op de virtuele machine actief en in goede staat. Deze services opnieuw starten als dat niet het geval is, en en de Hyper-V Volume Shadow Copy-aanvrager-service op de Hyper-V-hostserver.

### <a name="common-errors"></a>Veelvoorkomende fouten

**Foutcode** | **Bericht** | **Details**
--- | --- | ---
**0x800700EA** | ' Set van VSS-momentopname voor virtuele machine genereren van de Hyper-V is mislukt: Er is meer gegevens beschikbaar. (0x800700EA). VSS-momentopname ingesteld generatie kan mislukken als de back-upbewerking wordt uitgevoerd.<br/><br/> Replicatiebewerking voor de virtuele machine is mislukt: Er is meer gegevens beschikbaar. " | Controleer of uw virtuele machine dynamische schijf is ingeschakeld. Nee, dit wordt niet ondersteund.
**0x80070032** | ' Hyper-V Volume Shadow Copy aanvrager kan geen verbinding maken met virtuele machine <. / VMname > omdat de versie komt niet overeen met de versie die werd verwacht door Hyper-V | Controleer of de meest recente Windows-updates zijn geïnstalleerd.<br/><br/> [Upgrade](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services.md#keep-integration-services-up-to-date) naar de nieuwste versie van Integration Services.



## <a name="collect-replication-logs"></a>De replicatielogboeken verzamelen

Alle Hyper-V-replicatie gebeurtenis worden vastgelegd in het logboek Hyper-V-VMMS\Admin in **logboeken toepassingen en Services** > **Microsoft** > **Windows**. Bovendien kunt u inschakelen een analytische logboek voor de Hyper-V Virtual Machine Management-Service als volgt:

1. Controleer de logboeken voor analyse en foutopsporing kan worden weergegeven in Logboeken. Klik hiervoor in Logboeken op **weergave** > **analytische weergeven en logboeken voor foutopsporing.**. Het analytische logboek wordt weergegeven onder **Hyper-V-VMMS**.
2. In de **acties** deelvenster, klikt u op **logboek inschakelen**. 

    ![Logboek inschakelen](media/hyper-v-azure-troubleshoot/enable-log.png)
    
3. Nadat deze ingeschakeld, wordt deze weergegeven in **Prestatiemeter**, als een **gebeurtenistraceersessies** onder **gegevensverzamelaarsets**. 
4. Als u de verzamelde informatie, stop de traceringssessie door het logboek uit te schakelen. Het logboek opslaan en opnieuw openen in Logboeken of andere hulpprogramma's gebruiken om deze te converteren als vereist.


### <a name="event-log-locations"></a>Gebeurtenislogboek-locaties

**Gebeurtenislogboek** | **Details** |
--- | ---
**Toepassingen en Service-logboeken/Microsoft/beheer voor virtuele machines/Server/Admin** (VMM-server) | De logboeken voor het oplossen van problemen met VMM.
**Toepassingen en Service MicrosoftAzureRecoveryServices-logboeken/replicatie** (Hyper-V-host) | De logboeken voor het oplossen van problemen met Microsoft Azure Recovery Services Agent. 
**Toepassingen en Service Microsoft-logboeken/Azure Site Recovery/Provider/operationeel** (Hyper-V-host)| De logboeken voor het oplossen van problemen met Microsoft Azure Site Recovery Service.
**Toepassingen en Service-logboeken/Microsoft/Windows/Hyper-V-VMMS/Admin** (Hyper-V-host) | Logboeken van de Hyper-V VM management problemen kunt oplossen.

### <a name="log-collection-for-advanced-troubleshooting"></a>Logboekgegevens verzameld voor geavanceerde probleemoplossing

Deze hulpprogramma's kunnen helpen bij geavanceerde probleemoplossing:

-   Voor VMM uitvoeren met behulp logboek verzameling van Site Recovery de [ondersteuning Diagnostics Platform (SDP) hulpprogramma](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx).
-   Voor Hyper-V zonder VMM, [downloaden van het hulpprogramma](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab), en voer deze uit op de Hyper-V-host voor het verzamelen van de logboeken.

