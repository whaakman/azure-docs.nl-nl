---
title: Problemen met beveiliging VMware of fysieke naar Azure | Microsoft Docs
description: In dit artikel beschrijft de algemene replicatiefouten van de VMware-machine en het oplossen van deze
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: asgang
ms.openlocfilehash: 7a8cd09731ccdf7ad7385f7e707125c3902ebdf2
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="troubleshoot-on-premises-vmwarephysical-server-replication-issues"></a>Problemen met lokale VMware of fysieke server replicatie
U wordt een specifieke foutbericht weergegeven bij het beveiligen van uw virtuele VMware-machines of fysieke servers met Azure Site Recovery. In dit artikel vindt u details van de meestvoorkomende foutberichten aangetroffen, samen met stappen om op te lossen ze voor probleemoplossing.


## <a name="initial-replication-is-stuck-at-0"></a>Initiële replicatie is vastgelopen bij % 0
De meeste van de initiële replicatiefouten die we op ondersteuning optreden zijn vanwege problemen met de netwerkverbinding tussen het serverproces bronserver of het proces van server naar Azure.
De meeste gevallen kunt u deze problemen kunt oplossen door de onderstaande stappen te volgen.

###<a name="check-the-following-on-source-machine"></a>Controleer het volgende op de BRONMACHINE
* Gebruik vanaf de opdrachtregel van de bronserver machine Telnet te pingen van de processerver met https-poort (standaard 9443), zoals hieronder wordt weergegeven om te zien of er geen problemen met de netwerkverbinding of de firewall poort blokkerende problemen zijn.

    `telnet <PS IP address> <port>`
> [!NOTE]
    > Gebruik van Telnet, PING niet gebruiken om te controleren.  Als Telnet niet is geïnstalleerd, volgt u de lijst met [hier](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)

Kan geen verbinding maken, toestaan van binnenkomende poort 9443 op de processerver als controleren als het probleem nog steeds afgesloten. Er zijn enkele gevallen waar de processerver is achter DMZ die dit probleem is veroorzaakt.

* Controleer de status van service `InMage Scout VX Agent – Sentinel/OutpostStart` als het is niet uitgevoerd en Controleer als het probleem blijft bestaan.   

###<a name="check-the-following-on-process-server"></a>Controleer het volgende op de PROCESSERVER

* **Controleer als processerver is actief gegevens worden gepusht naar Azure**

Open Taakbeheer (druk op Ctrl-Shift-Esc) van de processerver-machine. Ga naar het tabblad prestaties en klik op de koppeling 'Open Broncontrole'. Van Resource Manager, gaat u naar het tabblad netwerk. Controleer als cbengine.exe in 'Processen met netwerkactiviteit' actief grote hoeveelheid (in MB) gegevens verzendt.

![Replicatie inschakelen](./media/site-recovery-protection-common-errors/cbengine.png)

Als dat niet het geval is, volg de onderstaande stappen uit:

* **Controleer of de processerver kunnen verbinding maken met Azure Blob**: selecteren en controleren van cbengine.exe om weer te geven van de 'TCP-verbindingen' om te zien of er verbinding tussen de processerver en de URL van Azure Storage-blob.

![Replicatie inschakelen](./media/site-recovery-protection-common-errors/rmonitor.png)

Als geen vervolgens gaat u naar Configuratiescherm > Services, Controleer of de volgende services actief zijn:

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     *
(Re) Start een service, die niet wordt uitgevoerd en Controleer als het probleem blijft bestaan.

* **Controleer of de processerver verbinding kunnen maken met Azure openbaar IP-adres met behulp van poort 443**

Open de meest recente CBEngineCurr.errlog van `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` en zoek naar: 443 en verbinding poging is mislukt.

![Replicatie inschakelen](./media/site-recovery-protection-common-errors/logdetails1.png)

Als er problemen zijn, klikt u vervolgens vanaf de opdrachtregel processerver telnet gebruiken te pingen van uw Azure openbare IP-adres (gemaskeerd in bovenstaande afbeelding) gevonden in de CBEngineCurr.currLog met poort 443.

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
Als u nog geen verbinding maken, controleert u of als het probleem toegang veroorzaakt door firewall of Proxy wordt, zoals beschreven in de volgende stap.


* **Controleer of de IP-adressen gebaseerde firewall op de processerver toegang niet blokkeert**: als u een IP-adressen gebaseerde firewallregels gebruikt op de server, downloadt u de volledige lijst met Microsoft Azure Datacenter IP-adresbereiken van [hier](https://www.microsoft.com/download/details.aspx?id=41653) en toe te voegen aan de configuratie van uw firewall om te controleren of ze kunnen communiceren met Azure (en de HTTPS (443)-poort) toestaan.  Sta de IP-adresbereiken voor de Azure-regio van uw abonnement en voor de regio VS - west toe (deze worden gebruikt voor toegangs- en identiteitsbeheer).

* **Controleer of de URL gebaseerde firewall op de processerver toegang niet blokkeert**: als u een URL gebaseerde firewallregels op de server gebruikt, moet de volgende URL's zijn toegevoegd aan de firewall-configuratie.

  `*.accesscontrol.windows.net:` Word gebruikt voor toegangs- en identiteitsbeheer

  `*.backup.windowsazure.com:` Wordt gebruikt voor overdracht en indeling van replicatiegegevens

  `*.blob.core.windows.net:`Gebruikt voor toegang tot het opslagaccount dat winkels gegevens gerepliceerde

  `*.hypervrecoverymanager.windowsazure.com:` Wordt gebruikt voor bewerkingen en indeling in het kader van replicatiebeheer

  `time.nist.gov`en `time.windows.com`: gebruikt om te controleren van de synchronisatie tussen system en globale tijd.

URL's voor **Azure Government Cloud**:

`* .ugv.hypervrecoverymanager.windowsazure.us`

`* .ugv.backup.windowsazure.us`

`* .ugi.hypervrecoverymanager.windowsazure.us`

`* .ugi.backup.windowsazure.us`

* **Controleer als Proxy-instellingen op de processerver zijn de toegang niet blokkeert**.  Als u een proxyserver gebruikt, controleert u de dat naam van de proxyserver wordt omgezet door de DNS-server.
Om te controleren wat u hebt opgegeven op het moment van de installatie van de configuratieserver. Ga naar de registersleutel

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

Nu ervoor te zorgen dat dezelfde instellingen worden gebruikt door Azure Site Recovery-agent om gegevens te verzenden.
Back-up van Microsoft Azure Search

![Replicatie inschakelen](./media/site-recovery-protection-common-errors/mab.png)

Open het en klik op actie > Eigenschappen wijzigen. Tabblad proxyconfiguratie ziet u de proxyadres mag niet hetzelfde zijn als het wordt weergegeven door de registerinstellingen. Als dat niet het geval is, moet u wijzigen naar hetzelfde adres.

![Replicatie inschakelen](./media/site-recovery-protection-common-errors/mabproxy.png)

* **Controleer als u bandbreedte niet op processerver beperkt**: de bandbreedte vergroten en controleer of het probleem blijft bestaan.

##<a name="next-steps"></a>Volgende stappen
Als u meer hulp nodig hebt, klikt u vervolgens uw query boeken [Azure Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). We hebben een actieve community en een van onze technici is het mogelijk om u te helpen.
