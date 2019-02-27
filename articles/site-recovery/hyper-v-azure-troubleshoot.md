---
title: Oplossen van Hyper-V naar Azure-noodherstel met Azure Site Recovery | Microsoft Docs
description: Hierin wordt beschreven hoe disaster recovery problemen oplossen met Hyper-V naar Azure met Azure Site Recovery-replicatie
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 703bf7425962ddcb5e7566f99c461ca37d237b68
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56868422"
---
# <a name="troubleshoot-hyper-v-to-azure-replication-and-failover"></a>Hyper-V naar Azure-replicatie en failover oplossen

In dit artikel worden veelvoorkomende problemen beschreven die u kunt tegenkomen tijdens bij het repliceren van on-premises Hyper-V-machines naar Azure, met behulp van [Azure Site Recovery](site-recovery-overview.md).

## <a name="enable-protection-issues"></a>Problemen met beveiliging inschakelen

Als u problemen ondervindt bij het inschakelen van de beveiliging voor Hyper-V-machines, controleert u de volgende aanbevelingen:

1. Controleer dat de Hyper-V-hosts en virtuele machines te voldoen aan alle [vereisten en voorwaarden](hyper-v-azure-support-matrix.md).
2. Als Hyper-V-servers bevinden zich in System Center Virtual Machine Manager (VMM)-clouds, controleert u of u hebt voorbereid de [VMM-server](hyper-v-prepare-on-premises-tutorial.md#prepare-vmm-optional).
3. Controleer of de Hyper-V Virtual Machine Management-service wordt uitgevoerd op Hyper-V-hosts.
4. Controleren op problemen die worden weergegeven in de Hyper-V-VMMS\Admin aanmelden bij de virtuele machine. Dit logboekbestand bevindt zich in **logboeken toepassingen en Services** > **Microsoft** > **Windows**.
5. Controleer op de Gast-VM, of dat WMI ingeschakeld en toegankelijk is is.
  - [Meer informatie over](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/) basic WMI-test.
  - [Problemen oplossen](https://aka.ms/WMiTshooting) WMI.
  - [Problemen oplossen](https://technet.microsoft.com/library/ff406382.aspx#H22) problemen met WMI-scripts en -services.
6. Zorg ervoor dat de nieuwste versie van Integration Services wordt uitgevoerd op de Gast-VM.
    - [Controleer](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) dat u de meest recente versie hebt.
    - [Houd](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#keep-integration-services-up-to-date) Integration Services up-to-date.
    
## <a name="replication-issues"></a>Problemen met replicatie

Problemen oplossen met initiële en lopende replicatie als volgt:

1. Zorg ervoor dat u de [meest recente versie](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx) van Site Recovery-services.
2. Controleer of de replicatie onderbroken:
  - Controleer de status van de virtuele machine in de Hyper-V Manager-console.
  - Als het is essentieel, met de rechtermuisknop op de virtuele machine > **replicatie** > **replicatiestatus weergeven**.
  - Als de replicatie is onderbroken, klikt u op **replicatie hervatten**.
3. Controleer of de vereiste services worden uitgevoerd. Als ze niet, start u deze opnieuw.
    - Als u virtuele Hyper-V zonder VMM uitvoert, controleert u of deze services worden uitgevoerd op de Hyper-V-host:
        - Virtual Machine Management-service
        - Microsoft Azure Recovery Services-Agent-service
        - Microsoft Azure Site Recovery-service
        - WMI-Provider Host-service
    - Als u met VMM in de omgeving repliceert, controleert u dat deze services worden uitgevoerd:
        - Controleer of de Virtual Machine Management-service, Microsoft Azure Recovery Services-Agent en de WMI-Provider Host-service worden uitgevoerd op de Hyper-V-host.
        - Zorg ervoor dat de System Center Virtual Machine Manager-Service wordt uitgevoerd op de VMM-server.
4. Controleer de connectiviteit tussen de Hyper-V-server en Azure. Als u wilt controleren op connectiviteit, opent u Taakbeheer op de Hyper-V-host. Op de **prestaties** tabblad **Open Broncontrole**. Op de **netwerk** tabblad > **verwerken met Network Activity**, controleert u of cbengine.exe actief grote volumes (MB/s) van de gegevens worden verzonden.
5. Als de Hyper-V-hosts worden verbonden met de Azure storage blob URL controleren Om te controleren als de hosts kunnen verbinden, selecteer en Controleer **cbengine.exe**. Weergave **TCP-verbindingen** om te controleren of de connectiviteit van de host naar de Azure storage-blob.
6. Controleer prestatieproblemen, zoals hieronder wordt beschreven.
    
### <a name="performance-issues"></a>Prestatieproblemen

Netwerk-bandbreedtebeperkingen kunnen gevolgen hebben voor replicatie. Oplossen van problemen als volgt:

1. [Controleer](https://support.microsoft.com/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage) als er bandbreedte of beperking van beperkingen in uw omgeving.
2. Voer de [Deployment Planner profiler](hyper-v-deployment-planner-run.md).
3. Nadat de profiler is uitgevoerd, volgt u de [bandbreedte](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) en [opslag](hyper-v-deployment-planner-analyze-report.md#vm-storage-placement-recommendation) aanbevelingen.
4. Controleer [gegevensverloop beperkingen](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits). Als u ziet een hoog gegevensverloop op een virtuele machine, het volgende doen:
  - Controleer als uw virtuele machine is gemarkeerd voor hersynchronisatie.
  - Ga als volgt [stappen](https://blogs.technet.microsoft.com/virtualization/2014/02/02/hyper-v-replica-debugging-why-are-very-large-log-files-generated/) voor het onderzoeken van de bron van het verloop.
  - Verloop kan optreden als de HRL-logboekbestanden groter zijn dan 50% van de beschikbare schijfruimte. Als dit het probleem, moet u meer opslagruimte inrichten voor alle virtuele machines waarop het probleem zich voordoet.
  - Controleer dat of de replicatie wordt niet onderbroken. Als dit het geval is, gaat u door de wijzigingen schrijven naar het hrl-bestand, wat aan de toegenomen grootte bijdragen kan.
 

## <a name="critical-replication-state-issues"></a>Problemen met de status van kritieke replicatie

1. Verbinding maken met de on-premises Hyper-V Manager-console om te controleren of de replicatiestatus, selecteer de virtuele machine en status controleren.

    ![Replicatiestatus](media/hyper-v-azure-troubleshoot/replication-health1.png)
    

2. Klik op **replicatiestatus weergeven** om de details te bekijken:

    - Als replicatie is onderbroken, met de rechtermuisknop op de virtuele machine > **replicatie** > **replicatie hervatten**.
    - Als een virtuele machine op een Hyper-V-host geconfigureerd in Site Recovery worden gemigreerd naar een andere Hyper-V-host in hetzelfde cluster of naar een zelfstandige virtuele machine, wordt niet replicatie voor de virtuele machine beïnvloed. Alleen wordt gecontroleerd dat de nieuwe Hyper-V-host aan alle vereisten voldoet, en is geconfigureerd in Site Recovery.

## <a name="app-consistent-snapshot-issues"></a>Problemen met App-consistente momentopname

Een app-consistente momentopname is een point-in-time-momentopname van de toepassingsgegevens in de virtuele machine. Volume Shadow Copy Service (VSS) zorgt ervoor dat apps op de virtuele machine in een consistente status hebben wanneer de momentopname wordt gemaakt.  In deze sectie worden enkele veelvoorkomende problemen die u mogelijk ondervindt.

### <a name="vss-failing-inside-the-vm"></a>VSS binnen de virtuele machine is mislukt

1. Controleer of de nieuwste versie van integratieservices geïnstalleerd en worden uitgevoerd is.  Controleer of er een update beschikbaar is door het uitvoeren van de volgende opdracht uit vanaf een verhoogde PowerShell-prompt op de Hyper-V-host: **get-vm | Selecteer de naam, status, IntegrationServicesState**.
2. Controleer of de VSS-services actief is en in orde zijn:
    - Om te controleren of de services, moet u zich aanmelden bij de Gast-VM. Vervolgens open een opdrachtprompt met beheerdersrechten en voer de volgende opdrachten om te controleren of alle VSS-schrijvers in orde zijn.
        - **Vssadmin list writers**
        - **Vssadmin list shadows**
        - **Vssadmin list-providers**
    - Controleer de uitvoer. Als schrijvers zich in een mislukte status, het volgende doen:
        - Controleer het gebeurtenislogboek van toepassing op de virtuele machine voor VSS-bewerking fouten.
    - Probeer het opnieuw starten van deze services die zijn gekoppeld aan de mislukte writer:
        - Volume Shadow Copy
         - Azure Site Recovery VSS Provider
    - Nadat u dit hebt gedaan, wacht u een paar uur om te zien als de app-consistente momentopnamen is gegenereerd.
    - Probeer het opnieuw opstarten van de virtuele machine als laatste redmiddel. Dit kan services die zich in niet meer reageert op te lossen.
3. U hebt geen dynamische schijven in de virtuele machine controleren Dit wordt niet ondersteund voor app-consistente momentopnamen. U kunt controleren of u in Schijfbeheer (diskmgmt.msc).

    ![Dynamische schijf](media/hyper-v-azure-troubleshoot/dynamic-disk.png)
    
4. Controleer of u een iSCSI-schijf die is gekoppeld aan de virtuele machine niet hebt. Nee, dit wordt niet ondersteund.
5. Controleer of de Backup-service is ingeschakeld. Controleer of deze is ingeschakeld **Hyper-V-instellingen** > **Integration Services**.
6. Zorg ervoor dat er zijn geen conflicten met apps VSS-momentopnamen te maken. Als u meerdere apps wilt VSS-momentopnamen op de dezelfde tijd conflicten optreden. Bijvoorbeeld als een back-up-app duurt VSS-momentopnamen wanneer Site Recovery door uw replicatiebeleid is gepland om een momentopname.   
7. Controleer als de virtuele machine een hoog verloop zich voordoet:
    - U kunt de dagelijkse veranderingssnelheid van gegevens meten voor de Gast-VM's, met behulp van prestatiemeteritems op Hyper-V-host. Inschakelen als u wilt meten de veranderingssnelheid van gegevens, het volgende item. Een voorbeeld van deze waarde in de VM-schijven samenvoegen voor 5 tot 15 minuten, om op te halen van het verloop van de virtuele machine.
        - Categorie: "Hyper-V virtuele opslagapparaat"
        - Item: "Geschreven Bytes per seconde"</br>
        - Het verloop van deze gegevens toe- of blijven op een hoog niveau, afhankelijk van hoeveel de virtuele machine of de apps zijn.
        - Het gemiddelde gegevensverloop van schijf is 2 MB/s voor standard-opslag voor Site Recovery. [Meer informatie](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits)
    - Daarnaast kunt u [schaalbaarheidsdoelen opslag controleren](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets).
8. Voer de [Deployment Planner](hyper-v-deployment-planner-run.md).
9. Bekijk de aanbevelingen voor [netwerk](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) en [opslag](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input).


### <a name="vss-failing-inside-the-hyper-v-host"></a>VSS is mislukt in de Hyper-V-Host

1. Controleer het gebeurtenislogboek voor VSS-fouten en aanbevelingen:
    - Open het gebeurtenislogboek van Hyper-V-beheer in op de Hyper-V-hostserver **logboeken** > **logboeken toepassingen en Services** > **Microsoft**  >  **Windows** > **Hyper-V** > **Admin**.
    - Controleer of er zijn geen gebeurtenissen die duiden op fouten van de app-consistente momentopname.
    - Een veelvoorkomende fout is: "Hyper-V is mislukt voor het genereren van VSS-momentopname instellen voor de virtuele machine 'XYZ': Een niet-tijdelijke fout opgetreden. De VSS-service opnieuw te starten mogelijk problemen oplossen als de service reageert niet."

2. Voor het genereren van VSS-momentopnamen voor de virtuele machine die Hyper-V-integratieservices zijn geïnstalleerd op de virtuele machine, en dat de back-up (VSS) Integration-Service is ingeschakeld.
    - Zorg ervoor dat de Integration Services VSS-service/daemons worden uitgevoerd op de Gast en zich in een **OK** staat.
    - U kunt dit controleren vanuit een sessie met verhoogde bevoegdheden PowerShell op de Hyper-V-host met de opdracht **et-VMIntegrationService - VMName<VMName>-naam VSS** u kunt deze informatie ook ophalen door aan te melden bij de Gast-VM. [Meer informatie](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services).
    - Zorg ervoor dat de back-up/VSS-integratieservices op de virtuele machine actief is en in orde. Als dat niet het geval is, start deze services en de Hyper-V Volume Shadow Copy-aanvrager-service op de Hyper-V-hostserver.

### <a name="common-errors"></a>Algemene fouten

**Foutcode** | **Bericht** | **Details**
--- | --- | ---
**0x800700EA** | "Hyper-V is mislukt voor het genereren van VSS-momentopname instellen voor de virtuele machine: Meer gegevens zijn beschikbaar. (0x800700EA). VSS-momentopname ingesteld generatie kan mislukken als de back-upbewerking uitgevoerd wordt.<br/><br/> De replicatiebewerking voor virtuele machine is mislukt: Meer gegevens zijn beschikbaar." | Controleer of de virtuele machine dynamische schijf is ingeschakeld heeft. Nee, dit wordt niet ondersteund.
**0x80070032** | "Hyper-V Volume Shadow Copy aanvrager kan geen verbinding maken met virtuele machine <. / VMname > omdat de versie komt niet overeen met de versie die door Hyper-V wordt verwacht | Controleer of de meest recente Windows-updates zijn geïnstalleerd.<br/><br/> [Upgrade](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#keep-integration-services-up-to-date) naar de nieuwste versie van integratieservices.



## <a name="collect-replication-logs"></a>Verzamelen om de replicatielogboeken

Alle Hyper-V-replicatie gebeurtenis worden vastgelegd in het logboek voor Hyper-V-VMMS\Admin, zich in **logboeken toepassingen en Services** > **Microsoft** > **Windows**. Bovendien kunt u inschakelen een analytische logboek voor de Hyper-V Virtual Machine Management-Service als volgt:

1. Controleer de logboeken voor analyse en foutopsporing in de Event Viewer kan worden weergegeven. De logboeken om beschikbaar te maken, in de logboeken, klikt u op **weergave** > **weergeven analytische en foutopsporing in Logboeken.**. De analytische logboek wordt weergegeven onder **Hyper-V-VMMS**.
2. In de **acties** deelvenster, klikt u op **logboek inschakelen**. 

    ![Logboek inschakelen](media/hyper-v-azure-troubleshoot/enable-log.png)
    
3. Nadat deze ingeschakeld, wordt deze weergegeven in **Prestatiemeter**, als een **gebeurtenistraceersessie** onder **gegevensverzamelaarsets**. 
4. Als u wilt de verzamelde gegevens weergeven, stopt u de traceringssessie door het uitschakelen van het logboek. Vervolgens slaat u het logboek en opnieuw openen in Logboeken of andere hulpprogramma's gebruiken om deze te converteren zoals vereist.


### <a name="event-log-locations"></a>Gebeurtenislogboek-locaties

**Gebeurtenislogboek** | **Details** |
--- | ---
**Toepassingen en Service-logboeken/Microsoft/beheer voor virtuele machines/Server/Admin** (VMM-server) | Hiermee wordt geregistreerd voor het oplossen van problemen met VMM.
**Toepassingen en -logboeken/MicrosoftAzureRecoveryServices/replicatie** (Hyper-V-host) | Hiermee wordt geregistreerd voor het oplossen van problemen met Microsoft Azure Recovery Services-Agent. 
**Toepassingen en Service-logboeken/Microsoft/Azure Site Recovery/Provider/Operational** (Hyper-V-host)| Hiermee wordt geregistreerd voor het oplossen van problemen met Microsoft Azure Site Recovery-Service.
**Toepassingen en Service-logboeken/Microsoft/Windows/Hyper-V-VMMS/Admin** (Hyper-V-host) | Hiermee wordt geregistreerd voor het oplossen van problemen met Hyper-V-VM.

### <a name="log-collection-for-advanced-troubleshooting"></a>Logboekverzameling voor geavanceerde probleemoplossing

Deze hulpprogramma's kunnen helpen bij geavanceerde probleemoplossing:

-   Voor VMM, uitvoeren met behulp log verzameling van Site Recovery de [ondersteuning Diagnostics Platform (SDP) hulpprogramma](https://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx).
-   Voor Hyper-V zonder VMM, [downloaden van het hulpprogramma](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab), en voer deze uit op de Hyper-V-host naar de logboeken te verzamelen.

