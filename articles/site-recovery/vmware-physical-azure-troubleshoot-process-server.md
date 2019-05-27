---
title: De Azure Site Recovery-processerver oplossen
description: In dit artikel wordt beschreven hoe u problemen oplossen met de Azure Site Recovery-processerver
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 04/29/2019
ms.author: raynew
ms.openlocfilehash: 6e31308800f72d60381f1e4ecd540482ba263851
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969369"
---
# <a name="troubleshoot-the-process-server"></a>De processerver oplossen

De [Site Recovery](site-recovery-overview.md) processerver wordt gebruikt bij het instellen van herstel na noodgevallen naar Azure voor on-premises VMware-machines en fysieke servers. In dit artikel wordt beschreven hoe u problemen oplossen met de processerver, met inbegrip van problemen met replicatie en connectiviteit.

[Meer informatie](vmware-physical-azure-config-process-server-overview.md) over de processerver.

## <a name="before-you-start"></a>Voordat u begint

Voordat u begint met het oplossen van problemen:

1. Zorg ervoor dat u begrijpt hoe u [processervers bewaken](vmware-physical-azure-monitor-process-server.md).
2. Bekijk de onderstaande procedures.
3. Zorg ervoor dat u volgt [capaciteitsoverwegingen](site-recovery-plan-capacity-vmware.md#capacity-considerations), en gebruik van de richtlijn voor formaatbepaling voor de [configuratieserver](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-configuration-server-and-inbuilt-process-server) of [zelfstandige processervers](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-process-server).

## <a name="best-practices-for-process-server-deployment"></a>Aanbevolen procedures voor processerverimplementatie

Voor optimale prestaties van processervers, hebben we een aantal algemene aanbevolen procedures worden samengevat.

**Best practices** | **Details**
--- |---
**Gebruik** | Zorg ervoor dat de configuratieserver voor de zelfstandige server /-proces worden alleen gebruikt voor het beoogde doel. Niet iets anders op de machine kunnen worden uitgevoerd.
**IP-adres** | Zorg ervoor dat de processerver een statisch IPv4-adres heeft en geen NAT geconfigureerd.
**Besturingselement geheugen/CPU-gebruik** |Houd CPU- en geheugengebruik onder 70%.
**Zorg ervoor dat de vrije ruimte** | Vrije ruimte verwijst naar de cache schijfruimte op de processerver. Gegevens van replicatie is in de cache opgeslagen voordat deze wordt geüpload naar Azure.<br/><br/> Houd de vrije ruimte meer dan 25%. Als het gaat dan 20%, wordt de replicatie beperkt voor gerepliceerde machines die gekoppeld aan de processerver zijn.

## <a name="check-process-server-health"></a>Proces-Serverstatus controleren

De eerste stap bij het oplossen van problemen is om te controleren of de status en de status van de processerver. U doet dit door alle waarschuwingen bekijken, controleert u dat de vereiste services worden uitgevoerd en controleer of er een heartbeat van de processerver. Deze stappen worden samengevat in de volgende afbeelding, gevolgd door procedures voor het uitvoeren van de stappen.

![Status van de server verwerken oplossen](./media/vmware-physical-azure-troubleshoot-process-server/troubleshoot-process-server-health.png)

## <a name="step-1-troubleshoot-process-server-health-alerts"></a>Stap 1: Proces-server-statusmeldingen oplossen

De processerver wordt een aantal health waarschuwingen gegenereerd. Deze waarschuwingen en aanbevolen acties uitvoert, worden in de volgende tabel samengevat.

**Waarschuwingstype** | **Fout** | **Problemen oplossen**
--- | --- | --- 
![In orde][green] | Geen  | Processerver is verbonden en in orde is.
![Waarschuwing][yellow] | Opgegeven services niet worden uitgevoerd. | 1. Controleer of de services worden uitgevoerd.<br/> 2. Als de services worden uitgevoerd zoals verwacht, volg de instructies hieronder voor [oplossen van problemen met clientverbindingen en replicatie](#check-connectivity-and-replication).
![Waarschuwing][yellow]  | CPU-gebruik > 80% voor de laatste 15 minuten. | 1. Voeg nieuwe machines niet.<br/>2. Controleer of het aantal virtuele machines met behulp van de processerver met overeenstemt [gedefinieerd limieten](site-recovery-plan-capacity-vmware.md#capacity-considerations), en houd rekening met het instellen van een [aanvullende processerver](vmware-azure-set-up-process-server-scale.md).<br/>3. Volg de instructies hieronder voor [oplossen van problemen met clientverbindingen en replicatie](#check-connectivity-and-replication).
![Kritiek][red] |  CPU-gebruik > 95% voor de laatste 15 minuten. | 1. Voeg nieuwe machines niet.<br/>2. Controleer of het aantal virtuele machines met behulp van de processerver met overeenstemt [gedefinieerd limieten](site-recovery-plan-capacity-vmware.md#capacity-considerations), en houd rekening met het instellen van een [aanvullende processerver](vmware-azure-set-up-process-server-scale.md).<br/>3. Volg de instructies hieronder voor [oplossen van problemen met clientverbindingen en replicatie](#check-connectivity-and-replication).<br/> 4. Als het probleem zich blijft voordoen, voert u de [Deployment Planner](https://aka.ms/asr-v2a-deployment-planner) voor replicatie van VMware/fysieke server.
![Waarschuwing][yellow] | Gebruik van geheugen-> 80% voor de laatste 15 minuten. |  1. Voeg nieuwe machines niet.<br/>2. Controleer of het aantal virtuele machines met behulp van de processerver met overeenstemt [gedefinieerd limieten](site-recovery-plan-capacity-vmware.md#capacity-considerations), en houd rekening met het instellen van een [aanvullende processerver](vmware-azure-set-up-process-server-scale.md).<br/>3. Volg de instructies die zijn gekoppeld aan de waarschuwing.<br/> 4. Als het probleem zich blijft voordoen, volg de instructies hieronder voor [oplossen van problemen met clientverbindingen en replicatie](#check-connectivity-and-replication).
![Kritiek][red] | Gebruik van geheugen-> 95% voor de laatste 15 minuten. | 1. Geen nieuwe machines toevoegen en instellen van overweegt een [aanvullende processerver](vmware-azure-set-up-process-server-scale.md).<br/> 2. Volg de instructies die zijn gekoppeld aan de waarschuwing.<br/> 3. 4. Als het probleem zich blijft voordoen, volg de instructies hieronder voor [oplossen van problemen met clientverbindingen en replicatie](#check-connectivity-and-replication).<br/> 4. Als het probleem zich blijft voordoen, voert u de [Deployment Planner](https://aka.ms/asr-v2a-deployment-planner) voor problemen met replicatie van VMware/fysieke server.
![Waarschuwing][yellow] | Cachemap vrije ruimte < 30% van de laatste 15 minuten. | 1. Geen nieuwe machines toevoegen en overwegen om een [aanvullende processerver](vmware-azure-set-up-process-server-scale.md).<br/>2. Controleer of het aantal virtuele machines met behulp van de processerver met overeenstemt [richtlijnen](site-recovery-plan-capacity-vmware.md#capacity-considerations).<br/> 3. Volg de instructies hieronder voor [oplossen van problemen met clientverbindingen en replicatie](#check-connectivity-and-replication).
![Kritiek][red] |  Vrije ruimte < 25% van de laatste 15 minuten | 1. Volg de instructies die zijn gekoppeld aan de waarschuwing voor dit probleem.<br/> 2. 3. Volg de instructies hieronder voor [oplossen van problemen met clientverbindingen en replicatie](#check-connectivity-and-replication).<br/> 3. Als het probleem zich blijft voordoen, voert u de [Deployment Planner](https://aka.ms/asr-v2a-deployment-planner) voor replicatie van VMware/fysieke server.
![Kritiek][red] | Geen heartbeat van de processerver voor 15 minuten of langer. De service tmansvs communiceert niet met de configuratieserver. | (1) Controleer of de processerver actief is.<br/> 2. Controleer of de tmassvc wordt uitgevoerd op de processerver.<br/> 3. Volg de instructies hieronder voor [oplossen van problemen met clientverbindingen en replicatie](#check-connectivity-and-replication).


![tabelsleutel](./media/vmware-physical-azure-troubleshoot-process-server/table-key.png)


## <a name="step-2-check-process-server-services"></a>Stap 2: Controleer de services van de processerver

Services die moeten worden uitgevoerd op de processerver worden samengevat in de volgende tabel. Er zijn kleine verschillen in services, afhankelijk van hoe de processerver is geïmplementeerd. 

Voor alle services, met uitzondering van de Microsoft Azure Recovery Services-Agent (obengine), Controleer of het StartType is ingesteld op **automatische** of **automatisch (vertraagd starten)**.
 
**Implementatie** | **Actieve services**
--- | ---
**Processerver op de configuratieserver** | Surrogaatbestand; ProcessServerMonitor; cxprocessserver; InMage PushInstall; De Service log Upload (LogUpload); InMage Scout Application Service; Microsoft Azure Recovery Services-Agent (obengine); InMage Scout VX Agent-Sentinel/Outpost (svagents); tmansvc; World Wide Web Publishing-Service (W3SVC); MySQL; Microsoft Azure Site Recovery-Service (dra)
**Processerver die wordt uitgevoerd als een zelfstandige server** | Surrogaatbestand; ProcessServerMonitor; cxprocessserver; InMage PushInstall; De Service log Upload (LogUpload); InMage Scout Application Service; Microsoft Azure Recovery Services-Agent (obengine); InMage Scout VX Agent-Sentinel/Outpost (svagents); tmansvc.
**Processerver voor failback in Azure is geïmplementeerd** | Surrogaatbestand; ProcessServerMonitor; cxprocessserver; InMage PushInstall; De Service log Upload (LogUpload)


## <a name="step-3-check-the-process-server-heartbeat"></a>Stap 3: De heartbeat van de server proces controleren

Als er geen heartbeat van de processerver (foutcode 806), het volgende doen:

1. Controleer of de virtuele machine van de processerver actief en werkend is.
2. Deze logboeken op fouten controleren.

    C:\ProgramData\ASR\home\svsystems\eventmanager *.log  C\ProgramData\ASR\home\svsystems\monitor_protection*.log

## <a name="check-connectivity-and-replication"></a>Controleer de verbinding en replicatie

 Initiële en lopende replicatiefouten worden vaak veroorzaakt door problemen met de netwerkverbinding tussen de bronmachines en de processerver of tussen de processerver en Azure. Deze stappen worden samengevat in de volgende afbeelding, gevolgd door procedures voor het uitvoeren van de stappen.

![Problemen oplossen met connectiviteit en replicatie](./media/vmware-physical-azure-troubleshoot-process-server/troubleshoot-connectivity-replication.png)


## <a name="step-4-verify-time-sync-on-source-machine"></a>Stap 4: Controleer of de tijdsynchronisatie op bronmachine

Zorg ervoor dat de datum en tijd voor de gerepliceerde machine gesynchroniseerd. [Meer informatie](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time)

## <a name="step-5-check-anti-virus-software-on-source-machine"></a>Stap 5: Controleer de antivirussoftware op de bronmachine

Controleer dat geen antivirussoftware op de gerepliceerde machine wordt geblokkeerd door Site Recovery. Als u uitsluiten van Site Recovery van antivirusprogramma's wilt, raadpleegt u [in dit artikel](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program).

## <a name="step-6-check-connectivity-from-source-machine"></a>Stap 6: Controleer de verbinding tussen de bronmachine


1. Installeer de [Telnet-client](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx) op de broncomputer als u wilt. Gebruik geen Ping.
2. Ping de processerver op de HTTPS-poort met Telnet vanaf de broncomputer. 9443 is standaard het HTTPS-poort voor replicatieverkeer.

    `telnet <process server IP address> <port>`

3. Controleer of de verbinding voltooid is.


**Connectiviteit** | **Details** | **Actie**
--- | --- | ---
**Geslaagd** | Telnet ziet u een leeg scherm en de processerver is bereikbaar is. | Geen verdere actie vereist.
**Unsuccessful** | U geen verbinding maken | Zorg ervoor dat de binnenkomende poort 9443 is toegestaan op de processerver. Bijvoorbeeld, als u beschikt over een perimeternetwerk of een gescreend subnet wordt genoemd. Controleer de verbinding opnieuw.
**Gedeeltelijk geslaagd** | U kunt verbinding maken, maar de bronmachine rapporteert dat de processerver kan niet worden bereikt. | Ga door met de volgende procedure voor het oplossen van problemen.

## <a name="step-7-troubleshoot-an-unreachable-process-server"></a>Stap 7: Problemen met een processerver is onbereikbaar oplossen

Als de processerver is niet bereikbaar is vanaf de broncomputer, fout 78186 weergegeven. Als niet wordt opgelost, wordt dit probleem zal leiden tot beide app-consistente en crash-consistente herstelpunten niet worden gegenereerd zoals verwacht.

Problemen oplossen door te controleren of de bronmachine kunt bereiken van het IP-adres van de processerver en voer het hulpprogramma cxpsclient uit op de bronmachine, om te controleren of de end-to-end-verbinding.


### <a name="check-the-ip-connection-on-the-process-server"></a>Controleer de IP-verbinding op de processerver

Als telnet geslaagd is, maar de bronmachine rapporteert dat de processerver kan niet worden bereikt, controleert u of u het IP-adres van de processerver kan bereiken.

1. In een webbrowser, probeert te bereiken van IP-adres https://<PS_IP>:<PS_Data_Port>/.
2. Als deze controle een HTTPS-certificaat-fout, die is normaal geeft. Als u de fout negeren, ziet u een 400 - Ongeldige aanvraag. Dit betekent dat de server een aanvraag van de browser kan niet fungeren, en dat de standaard HTTPS-verbinding is prima.
3. Als dit selectievakje niet werkt, noteert u het foutbericht van de browser. Bijvoorbeeld, wordt een 407 fout duidt op een probleem met de proxy-verificatie.

### <a name="check-the-connection-with-cxpsclient"></a>Controleer de verbinding met cxpsclient

Bovendien kunt u het hulpprogramma cxpsclient om te controleren of de end-to-end-verbinding uitvoeren.

1. Voer het hulpprogramma als volgt uit:

    ```
    <install folder>\cxpsclient.exe -i <PS_IP> -l <PS_Data_Port> -y <timeout_in_secs:recommended 300>
    ```

2. Controleer de gegenereerde logboeken in deze mappen op de processerver:

    C:\ProgramData\ASR\home\svsystems\transport\log\cxps.err  C:\ProgramData\ASR\home\svsystems\transport\log\cxps.xfer



### <a name="check-source-vm-logs-for-upload-failures-error-78028"></a>Controleer de bron-VM-logboeken voor uploadfouten (fout 78028)

Probleem met gegevensuploads bronmachines geblokkeerd met de proces-service kan resulteren in zowel crash-consistente en toepassingsconsistente herstelpunten niet wordt gegenereerd. 

1. Voor het oplossen van fouten bij het uploaden van netwerk, kunt u zoeken naar fouten in dit logboek:

    C:\Program bestanden (x86) \Microsoft Azure Site Recovery\agent\svagents*.log 

2. Gebruik de rest van de procedures in dit artikel kunt u oplossen van problemen met het uploaden van gegevens.



## <a name="step-8-check-whether-the-process-server-is-pushing-data"></a>Stap 8: Controleer of de processerver van gegevens pushen

Controleer of de processerver actief van gegevens naar Azure pushen is.

  1. Open Taakbeheer (druk op Ctrl + Shift + Esc) op de processerver.
  2. Selecteer de **prestaties** tabblad > **Open Broncontrole**.
  3. In **Broncontrole** weergeeft, schakelt de **netwerk** tabblad. Onder **processen met Network Activity**, controleert u of cbengine.exe actief een grote vNotolume van gegevens verzendt.

       ![Volumes onder processen met netwerkactiviteit](./media/vmware-physical-azure-troubleshoot-process-server/cbengine.png)

  Als cbengine.exe is niet een grote hoeveelheid gegevens verzendt, voltooi de stappen in de volgende secties.

## <a name="step-9-check-the-process-server-connection-to-azure-blob-storage"></a>Stap 9: Controleer de verbinding van de server verwerken met Azure blob-opslag

1. Selecteer in de Resource-Monitor **cbengine.exe**.
2. Onder **TCP-verbindingen**, controleert u of er een verbinding van de processerver naar de Azure-opslag is.

  ![Connectiviteit tussen cbengine.exe en de URL van de Azure Blob-opslag](./media/vmware-physical-azure-troubleshoot-process-server/rmonitor.png)

### <a name="check-services"></a>Controleer de services

Als er geen verbinding van de processerver is met de URL van de Azure blob-opslag, moet u controleren dat services worden uitgevoerd.

1. Selecteer in het Configuratiescherm **Services**.
2. Controleer of de volgende services worden uitgevoerd:

    - cxprocessserver
    - InMage Scout VX Agent-Sentinel/Outpost
    - Microsoft Azure Recovery Services-agent
    - Microsoft Azure Site Recovery-service
    - tmansvc

3. Starten of opnieuw starten van een service die actief is.
4. Controleer of de processerver is verbonden en bereikbaar is. 

## <a name="step-10-check-the-process-server-connection-to-azure-public-ip-address"></a>Stap 10: Controleer de verbinding van de server proces met openbaar IP-adres van Azure

1. Op de processerver in **%programfiles%\Microsoft Azure Recovery Services-Agent\Temp**, opent u het meest recente CBEngineCurr.errlog-bestand.
2. Zoek in het bestand **443**, of voor de tekenreeks **verbindingspoging is mislukt**.

  ![Foutenlogboeken in de map Temp](./media/vmware-physical-azure-troubleshoot-process-server/logdetails1.png)

3. Als er problemen zijn, bevindt uw Azure openbare IP-adres in het bestand CBEngineCurr.currLog via poort 443:

  `telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443`

5. Op de opdrachtregel op de processerver, moet u Telnet gebruiken om te pingen van uw Azure openbare IP-adres.
6. Als u geen verbinding kunt maken, volgt u de volgende procedure.

## <a name="step-11-check-process-server-firewall-settings"></a>Stap 11: Controleer de firewall-instellingen voor proces-server. 

Controleer of toegang wordt geblokkeerd door de IP-adressen gebaseerde firewall op de processerver.

1. Voor IP-adressen gebaseerde firewallregels:

    (a) de volledige lijst van downloaden [Microsoft Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653).

    (b) het IP-adresbereiken toevoegen aan uw firewallconfiguratie, om ervoor te zorgen dat de firewall naar Azure (en de standaard HTTPS-poort 443)-communicatie toestaat.

    c) toestaan IP-adresbereiken voor de Azure-regio van uw abonnement en de regio VS-Azure West (gebruikt voor toegangsbeheer en identiteitsbeheer).

2. Voor URL-gebaseerde firewalls, toevoegen de URL's die worden vermeld in de volgende tabel om de configuratie van de firewall.

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  


## <a name="step-12-verify-process-server-proxy-settings"></a>Stap 12: Controleer of proces server proxy-instellingen 

1. Als u een proxyserver gebruikt, zorgt u ervoor dat de naam van de proxyserver wordt omgezet door de DNS-server. Controleer de waarde die u hebt opgegeven bij het instellen van de configuratieserver in registersleutel **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings**.
2. Zorg ervoor dat dezelfde instellingen worden gebruikt door de Azure Site Recovery-agent om gegevens te verzenden.

    (a) zoeken voor **van Microsoft Azure Backup**.

    (b) open **Microsoft Azure Backup**, en selecteer **actie** > **eigenschappen wijzigen**.

    c) op de **proxyconfiguratie** tabblad, het proxyadres moet hetzelfde zijn als de proxy-adres dat wordt weergegeven in de registerinstellingen. Als dat niet het geval is, moet u hetzelfde adres wijzigen.

## <a name="step-13-check-bandwidth"></a>Stap 13: Controleren van bandbreedte

Vergroot de bandbreedte tussen de processerver en Azure, en controleer vervolgens of het probleem nog steeds voordoet.


## <a name="next-steps"></a>Volgende stappen

Als u meer hulp nodig hebt, stel uw vraag in de [Azure Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). 

[green]: ./media/vmware-physical-azure-troubleshoot-process-server/green.png
[yellow]: ./media/vmware-physical-azure-troubleshoot-process-server/yellow.png
[red]: ./media/vmware-physical-azure-troubleshoot-process-server/red.png