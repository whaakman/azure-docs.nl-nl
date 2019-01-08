---
title: Oplossen van problemen met replicatie voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat informatie over probleemoplossing voor algemene problemen met replicatie tijdens herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2018
ms.author: ramamill
ms.openlocfilehash: c53dc81da9469c0628adbd3751dc818997fa4d05
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54063675"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>Problemen met replicatie voor virtuele VMware-machines en fysieke servers

U kunt een specifiek foutbericht tegenkomen wanneer u uw virtuele VMware-machines of fysieke servers beschermen met behulp van Azure Site Recovery. In dit artikel beschrijft enkele veelvoorkomende problemen die optreden mogelijk wanneer u on-premises VMware-machines en fysieke servers naar Azure met behulp van repliceren [siteherstel](site-recovery-overview.md).

## <a name="initial-replication-issues"></a>Initiële replicatie oplossen

Initiële replicatiefouten worden vaak veroorzaakt door problemen met de netwerkverbinding tussen de bron- en de processerver of tussen de processerver en Azure. In de meeste gevallen kunt u deze problemen kunt oplossen door de stappen in de volgende secties te voltooien.

### <a name="check-the-source-machine"></a>Controleer de bron-VM

De volgende lijst toont manieren waarop u op de bronmachine controleren kunt:

*  Op de opdrachtregel op de bronserver, moet u Telnet gebruiken om te pingen van de processerver via het HTTPS-poort (de standaard HTTPS-poort is 9443) met de volgende opdracht. Voor problemen met de netwerkverbinding en problemen dat blok de firewallpoort wordt gecontroleerd.


   `telnet <process server IP address> <port>`


   > [!NOTE]
   > Telnet gebruiken om te controleren. Gebruik geen `ping`. Als Telnet niet is geïnstalleerd, voert u de stappen in [Telnet-Client installeren](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx).

   Als u geen verbinding met de processerver maken, kunt u de binnenkomende poort 9443 op de processerver. Bijvoorbeeld, u moet mogelijk binnenkomende poort 9443 op de processerver is toegestaan als het netwerk een perimeternetwerk heeft of gescreend subnet wordt genoemd. Controleer vervolgens of het probleem nog steeds voordoet.

*  Controleer de status van de **InMage Scout VX Agent-Sentinel/OutpostStart** service. Als de service niet actief is, start de service en Ga na of het probleem nog steeds voordoet.   

### <a name="check-the-process-server"></a>Controleer de processerver

De volgende lijst toont manieren waarop u op de processerver controleren kunt:

*  **Controleer of de processerver actief van gegevens naar Azure pushen is**.

   1. Open Taakbeheer (druk op Ctrl + Shift + Esc) op de processerver.
   2. Selecteer de **prestaties** tabblad, en selecteer vervolgens de **Open Broncontrole** koppeling. 
   3. Op de **Broncontrole** weergeeft, schakelt de **netwerk** tabblad. Onder **processen met Network Activity**, controleert u of **cbengine.exe** actief een grote hoeveelheid gegevens verzendt.

        ![Schermafbeelding van de volumes onder processen met netwerkactiviteit](./media/vmware-azure-troubleshoot-replication/cbengine.png)

   Als cbengine.exe is niet een grote hoeveelheid gegevens verzendt, voltooi de stappen in de volgende secties.

*  **Controleer of de processerver verbinding met Azure Blob-opslag maken kunt**.

   Selecteer **cbengine.exe**. Onder **TCP-verbindingen**, controleert u of er naar de URL van de Blog van het Azure-opslag met van de processerver is.

   ![Schermafbeelding van verbinding tussen cbengine.exe en de URL van de Azure Blob-opslag](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

   Als er geen connectiviteit van de processerver op de Blog van het Azure storage-URL, in het Configuratiescherm is, selecteert u **Services**. Controleren of de volgende services worden uitgevoerd:

   *  cxprocessserver
   *  InMage Scout VX Agent-Sentinel/Outpost
   *  Microsoft Azure Recovery Services-agent
   *  Microsoft Azure Site Recovery-service
   *  tmansvc

   Starten of opnieuw starten van een service die actief is. Controleer of het probleem nog steeds voordoet.

*  **Controleer of de processerver verbinding met Azure openbaar IP-adres maken kan via poort 443**.

   Open in %programfiles%\Microsoft Azure Recovery Services-Agent\Temp, de meest recente CBEngineCurr.errlog-bestand. Zoek in het bestand **443** of voor de tekenreeks **verbindingspoging is mislukt**.

   ![Schermafbeelding van de fout zich in de map Temp](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

   Als er problemen worden weergegeven, klikt u op de opdrachtregel op de processerver, kunt u Telnet gebruiken om te pingen van uw Azure openbare IP-adres (het IP-adres wordt gemaskeerd in de vorige afbeelding). U kunt uw Azure openbare IP-adres vinden in het bestand CBEngineCurr.currLog via poort 443:

   `telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443`

   Als u geen verbinding kunt maken, moet u controleren of het toegangsprobleem afkomstig vanwege een firewall of proxy-instellingen, is zoals beschreven in de volgende stap.

*  **Controleer of de IP-adressen gebaseerde firewall op de processerver blokkeert de toegang**.

   Als u IP-adressen gebaseerde firewallregels op de server gebruikt, downloadt u de volledige lijst van [Microsoft Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). De IP-adresbereiken toevoegen aan uw firewallconfiguratie om ervoor te zorgen dat de firewall naar Azure (en de standaard HTTPS-poort 443)-communicatie toestaat. IP-adresbereiken voor de Azure-regio van uw abonnement en de regio VS-Azure West (gebruikt voor toegangsbeheer en identiteitsbeheer) toestaan.

*  **Controleer of een URL-gebaseerde firewall op de processerver blokkeert de toegang**.

   Als u een URL-gebaseerde firewall-regel op de server gebruikt, voegt u de URL's die worden vermeld in de volgende tabel om de configuratie van de firewall:

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

*  **Controleer of proxy-instellingen op de processerver blokkeert de toegang**.

   Als u een proxyserver gebruikt, zorgt u ervoor dat de naam van de proxyserver wordt omgezet door de DNS-server. Om te controleren of de waarde die u hebt opgegeven bij het instellen van de configuratieserver, gaat u naar de registersleutel **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings**.

   Vervolgens kunt u zorgen dat dezelfde instellingen worden gebruikt door de Azure Site Recovery-agent om gegevens te verzenden: 
      
   1. Zoeken naar **van Microsoft Azure Backup**. 
   2. Open **Microsoft Azure Backup**, en selecteer vervolgens **actie** > **eigenschappen wijzigen**. 
   3. Op de **proxyconfiguratie** tabblad ziet u de proxy-adres. Het proxyadres moet hetzelfde zijn als de proxy-adres dat wordt weergegeven in de registerinstellingen zijn. Als dat niet het geval is, moet u hetzelfde adres wijzigen.

*  **Controleer of de bandbreedte beperken op de processerver wordt beperkt**.

   De bandbreedte vergroten en controleer vervolgens of het probleem nog steeds voordoet.

## <a name="source-machine-isnt-listed-in-the-azure-portal"></a>Broncomputer niet wordt weergegeven in de Azure-portal

Wanneer u probeert om te selecteren van de bronmachine replicatie in te schakelen met behulp van Site Recovery, de computer mogelijk niet beschikbaar voor een van de volgende redenen:

*  Als twee virtuele machines onder het vCenter hetzelfde exemplaar UUID hebt, wordt de eerste virtuele machine is gedetecteerd door de configuratieserver wordt weergegeven in de Azure-portal. U lost dit probleem, zorg ervoor dat er geen twee virtuele machines hetzelfde exemplaar UUID.
*  Zorg ervoor dat u de referenties van de juiste vCenter toegevoegd bij het instellen van de configuratieserver met behulp van de OVF-sjabloon of geïntegreerde setup. Als u wilt controleren of de referenties die u hebt toegevoegd tijdens de installatie, Zie [referenties wijzigen voor automatische detectie](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
*  Als de machtigingen voor toegang tot vCenter die niet de vereiste machtigingen hebt, is mislukt voor het detecteren van virtuele machines kan optreden. Zorg ervoor dat de machtigingen die worden beschreven in [een account voorbereiden voor automatische detectie](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) worden toegevoegd aan het account van de vCenter-gebruiker.
*  Als de virtuele machine is al beveiligd met Site Recovery, de virtuele machine niet beschikbaar om te selecteren voor beveiliging in de portal. Zorg ervoor dat de virtuele machine die u in de portal zoekt al is niet beveiligd door een andere gebruiker of onder een ander abonnement.

## <a name="protected-virtual-machines-arent-available-in-the-portal"></a>Beveiligde virtuele machines zijn niet beschikbaar in de portal

Virtuele machines die worden gerepliceerd in Site Recovery niet beschikbaar zijn in de Azure-portal als er dubbele vermeldingen in het systeem. Zie voor meer informatie over het verwijderen van verouderde vermeldingen en los het probleem, [Azure Site Recovery van VMware naar Azure: Het opschonen van dubbele of verouderde vermeldingen](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx).

## <a name="next-steps"></a>Volgende stappen

Als u meer hulp nodig hebt, stel uw vraag in de [Azure Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). We hebben een actieve community en een van onze technici u kan helpen.
