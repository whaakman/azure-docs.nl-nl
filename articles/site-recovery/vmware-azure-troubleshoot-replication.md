---
title: Problemen met replicatie voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat informatie over probleemoplossing voor algemene problemen met replicatie tijdens herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2018
ms.author: ramamill
ms.openlocfilehash: 1c37b764b47856d3a369228d3f224f2a464029bb
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790653"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>Problemen met replicatie voor virtuele VMware-machines en fysieke servers

U ontvangt mogelijk een bericht specifieke fout bij het beveiligen van uw virtuele VMware-machines of fysieke servers met Azure Site Recovery. In dit artikel beschrijft enkele veelvoorkomende problemen die optreden tijdens het repliceren van on-premises virtuele VMware-machines en fysieke servers naar Azure met [Azure Site Recovery](site-recovery-overview.md).


## <a name="initial-replication-issues"></a>Initiële replicatie oplossen

In veel gevallen zijn initiële replicatie-fouten die we op ondersteuning optreden vanwege problemen met de netwerkverbinding tussen de serverproces bronserver of verwerken van server naar Azure. De meeste gevallen kunt u deze problemen kunt oplossen door de onderstaande stappen te volgen.

### <a name="verify-the-source-machine"></a>Controleer of de bron-VM
* Gebruik van de bronserver machine vanaf de opdrachtregel, Telnet te pingen van de processerver met https-poort (standaard 9443), zoals hieronder wordt weergegeven om te zien of er problemen met de netwerkverbinding of de firewall port blokkerende problemen zijn.

    `telnet <PS IP address> <port>`
> [!NOTE]
    > Gebruik van Telnet, PING niet gebruiken om te controleren.  Als Telnet niet is geïnstalleerd, volgt u de lijst met stappen [hier](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)

Kan geen verbinding maken, binnenkomende poort 9443 op de processerver toestaan als controleren als het probleem nog steeds beëindigd. Er zijn enkele gevallen waar de processerver is achter DMZ, die het probleem is veroorzaakt.

* Controleer de status van service `InMage Scout VX Agent – Sentinel/OutpostStart` als deze niet wordt uitgevoerd en controleer of als het probleem blijft bestaan.   

### <a name="verify-the-process-server"></a>Controleer of de processerver

* **Controleer als processerver actief van gegevens naar Azure pushen is**

Open in proces-servermachine Taakbeheer (druk op Ctrl-Shift-Esc). Ga naar het tabblad prestaties en klikt u op de koppeling 'Open Broncontrole'. Uit Resource Manager, gaat u naar het tabblad netwerk. Controleer als cbengine.exe in 'Processen met Network Activity' actief groot (in MB/s) van de gegevens worden verzonden.

![Replicatie inschakelen](./media/vmware-azure-troubleshoot-replication/cbengine.png)

Als dat niet het geval is, volgt u de onderstaande stappen uitvoeren:

* **Controleer of de processerver is geen verbinding maken van Azure Blob**: Selecteer en controleer cbengine.exe om de 'TCP-verbindingen"om te zien of er connectiviteit van de processerver naar Azure Storage blob URL weer te geven.

![Replicatie inschakelen](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

Als geen Ga vervolgens naar het Configuratiescherm > Services, Controleer of de volgende services actief en werkend zijn:

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     *
(Opnieuw) Elke service, die niet wordt uitgevoerd en de controle starten als het probleem blijft bestaan.

* **Controleer of de processerver is geen verbinding maken met Azure openbaar IP-adres met behulp van poort 443**

Open de meest recente CBEngineCurr.errlog van `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` en zoek naar: 443 en verbinding poging is mislukt.

![Replicatie inschakelen](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

Als er problemen zijn, klikt u vervolgens vanaf de opdrachtregel processerver telnet gebruiken om te pingen van uw Azure openbare IP-adres (gemaskeerd bovenstaande afbeelding) gevonden in de CBEngineCurr.currLog met behulp van poort 443.

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
Als u kan geen verbinding maken, klikt u vervolgens controleren of het toegangsprobleem vanwege een firewall of Proxy, zoals beschreven in de volgende stap.


* **Controleer als IP-adressen gebaseerde firewallregels op de processerver toegang niet blokkeert**: Als u een IP-adressen gebaseerde firewallregels gebruikt op de server, downloadt u de volledige lijst van Microsoft Azure Datacenter IP-bereiken van [hier](https://www.microsoft.com/download/details.aspx?id=41653) en toe te voegen aan uw firewallconfiguratie om ervoor te zorgen dat communicatie met Azure (en de poort HTTPS (443)).  Sta de IP-adresbereiken voor de Azure-regio van uw abonnement en voor de regio US - west toe (deze worden gebruikt voor toegangs- en identiteitsbeheer).

* **Controleer als URL-gebaseerde firewall op de processerver toegang niet blokkeert**:  Als u een URL-gebaseerde firewall-regels op de server, controleert u of dat de volgende URL's worden toegevoegd aan de firewall-configuratie.

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

* **Controleer als Proxy-instellingen op de processerver toegang niet blokkeert**.  Als u een proxyserver, controleert u of dat de naam van de proxy-server wordt omgezet door de DNS-server.
Om te controleren wat u hebt opgegeven op het moment van de installatie van de configuratieserver. Ga naar de registersleutel

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

Nu ervoor te zorgen dat dezelfde instellingen worden gebruikt door Azure Site Recovery-agent om gegevens te verzenden.
Zoeken in Microsoft Azure Backup

Open het en klikt u op actie > Eigenschappen wijzigen. Onder het tabblad Proxy-configuratie ziet u de proxy-adres hetzelfde moet zoals aangegeven in de registerinstellingen. Als dat niet het geval is, wijzig deze aan hetzelfde adres.


* **Controleer als vertraging bandbreedte niet op de processerver wordt beperkt**:  De bandbreedte vergroten en controleer of het probleem nog steeds bestaat.

## <a name="source-machine-to-be-protected-through-site-recovery-is-not-listed-on-azure-portal"></a>Bron-VM moet worden beveiligd met Site Recovery niet wordt weergegeven op de Azure portal

Bij het kiezen van de bronmachine replicatie met Azure Site Recovery in te schakelen, de computer mogelijk niet beschikbaar voor u kunt doorgaan met de volgende oorzaken hebben

* Als er twee virtuele machines onder het vCenter met dezelfde instantie UUID, klikt u vervolgens de eerste virtuele machine is gedetecteerd door de configuratieserver zijn worden weergegeven in de portal. Zorg ervoor dat er geen twee virtuele machines hetzelfde exemplaar UUID hebben om op te lossen.
* Zorg ervoor dat u hebt toegevoegd de juiste vCenter-referenties tijdens het instellen van van de configuratie met behulp van OVF-sjabloon of geïntegreerde set up. Als u wilt controleren of de referenties die zijn toegevoegd, raadpleegt u de richtlijnen voor gedeelde [hier](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
* Als de machtigingen die voor toegang tot vCenter niet voldoende bevoegdheden hebben, kan dit leiden tot fouten in het detecteren van virtuele machines. Zorg ervoor dat de machtigingen die [hier](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) worden toegevoegd aan het account van de vCenter-gebruiker.
* Als de virtuele machine is al beveiligd met Site Recovery, wordt klikt u vervolgens het niet meer beschikbaar voor beveiliging. Zorg ervoor dat de virtuele machine die u in de portal zoekt is nog niet beveiligd door een andere gebruiker of onder andere abonnementen.

## <a name="protected-virtual-machines-are-greyed-out-in-the-portal"></a>Beveiligde virtuele machines uit grijs worden weergegeven in de portal

Virtuele machines die worden gerepliceerd in Site Recovery worden grijs weergegeven als er dubbele vermeldingen in het systeem. Raadpleeg de richtlijnen gegeven [hier](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) verwijderen van de verouderde vermeldingen en los het probleem.

## <a name="next-steps"></a>Volgende stappen
Als u meer hulp nodig hebt, klikt u vervolgens boek uw query [Azure Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). We hebben een actieve community en een van onze technici kunnen om u te helpen.
