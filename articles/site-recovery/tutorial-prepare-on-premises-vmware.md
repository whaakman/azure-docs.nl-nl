---
title: On-premises VMware-servers voorbereiden op herstel na noodgevallen van virtuele VMware-machines naar Azure | Microsoft Docs
description: Informatie over het on-premises VMware-servers voorbereiden op herstel na noodgevallen in Azure maken met de Azure Site Recovery-service.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 90a4582c-6436-4a54-a8f8-1fee806b8af7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: af09c5602c53be4377ba19e68ff3486bcfefe0ea
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="prepare-on-premises-vmware-servers-for-disaster-recovery-to-azure"></a>On-premises VMware-servers voorbereiden op herstel na noodgevallen naar Azure

Deze zelfstudie laat zien hoe u uw on-premises VMware-infrastructuur voorbereidt wanneer u een VMware-machines repliceren naar Azure wilt maken. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Voorbereiden van een account op de vCenter-server of vSphere ESXi-host, voor het automatiseren van VM-detectie
> * Voorbereiden van een account voor automatische installatie van de Mobility-service op de virtuele VMware-machines
> * Overzicht van VMware server-vereisten
> * Overzicht van VMware VM-vereisten

In deze zelfstudie reeks laten we zien hoe back-up van een enkele virtuele machine met Azure Site Recovery. Als u van plan bent om te beschermen van meerdere virtuele machines van VMware, downloadt u de [implementatie Planner hulpprogramma](https://aka.ms/asr-deployment-planner) voor VMware-replicatie. Dit hulpprogramma u gegevens verzamelen over de compatibiliteit van de VM, schijven per virtuele machine en gegevensverloop per schijf. Het hulpprogramma vallen ook netwerkbandbreedte en de Azure-infrastructuur die nodig zijn voor een geslaagde failover voor replicatie en test. [Meer informatie](site-recovery-deployment-planner.md) over het programma wordt uitgevoerd.

Dit is de tweede zelfstudie in de reeks. Zorg ervoor dat u hebt [instellen van de Azure-onderdelen](tutorial-prepare-azure.md) zoals beschreven in de vorige zelfstudie.

## <a name="prepare-an-account-for-automatic-discovery"></a>Voorbereiden van een account voor automatische detectie

Site Recovery moet toegang tot de VMware-servers:

- Detecteert automatisch virtuele machines. Ten minste een alleen-lezen-account is vereist.
- Indelen van replicatie, failover en failback. U moet een account dat bewerkingen zoals het maken en verwijderen van schijven en dat kan worden uitgevoerd op virtuele machines.

Als volgt te werk om het account te maken:

1. Voor het gebruik van een specifiek account, maakt u een rol op het niveau van de vCenter. De rol, zoals een naam geven **Azure_Site_Recovery**.
2. De rol machtigingen samengevat in de onderstaande tabel toewijzen.
3. Maak een gebruiker op de vCenter-server of vSphere-host. De rol wordt toegewezen aan de gebruiker.

### <a name="vmware-account-permissions"></a>Machtigingen van de VMware-account

**Taak** | **Rolmachtigingen /** | **Details**
--- | --- | ---
**VM-detectie** | Ten minste een alleen-lezen-gebruiker<br/><br/> Data Center-object doorgeven aan het onderliggende Object –> rol = alleen-lezen | Gebruiker datacenter niveau toewijzen en toegang heeft tot alle objecten in het datacenter.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven dat onderliggende** object naar de onderliggende objecten (vSphere-hosts, datastores, VM's en -netwerken).
**Een volledige replicatie, failover en failback** |  Een gebruikersrol (Azure_Site_Recovery) maakt met de vereiste machtigingen, en vervolgens de rol toewijzen aan een VMware-gebruiker of groep<br/><br/> Data Center-object doorgeven aan het onderliggende Object –> rol Azure_Site_Recovery =<br/><br/> Datastore -> ruimte toewijzen, gegevensopslag, op laag niveau bestandsbewerkingen bladeren, bestand verwijderen, bestanden van virtuele machine bijwerken<br/><br/> Netwerk -> netwerk toewijzen<br/><br/> Resource -> VM toewijzen aan de resourcegroep, migreren van virtuele machine is uitgeschakeld, stroomvoorziening op virtuele machine migreren<br/><br/> Taken maken taak, updatetaak -><br/><br/> Virtuele machine-configuratie ><br/><br/> Virtuele machine -> Interact -> vraag beantwoorden, apparaatverbinding, CD's configureren, configureren van diskettes, uitschakelen, inschakelen, VMware-hulpprogramma's installeren<br/><br/> Virtuele machine -> inventaris -> maken, registreren, registratie<br/><br/> Virtuele machine inrichten -> -> downloaden van de virtuele machine toestaan, toestaan uploaden van bestanden van virtuele machine<br/><br/> Virtuele machine-momentopnamen >-Remove momentopnamen > | Gebruiker datacenter niveau toewijzen en toegang heeft tot alle objecten in het datacenter.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven dat onderliggende** object naar de onderliggende objecten (vSphere-hosts, datastores, VM's en -netwerken).

## <a name="prepare-an-account-for-mobility-service-installation"></a>Voorbereiden van een account voor de installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op de virtuele machine die u wilt repliceren. Site Recovery wordt deze service automatisch geïnstalleerd wanneer u replicatie voor de virtuele machine inschakelt. Automatische installatie moet u een account die door Site Recovery wordt gebruikt voor toegang tot de virtuele machine voorbereiden. U moet dit account opgeven bij het instellen van herstel na noodgevallen in de Azure-console.

1. Bereid een domein of lokale account met machtigingen op de virtuele machine te installeren.
2. Uitschakelen om te installeren op Windows virtuele machines, als u niet een domeinaccount, externe gebruiker toegangsbeheer op de lokale computer.
   - Uit het register, onder **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, de DWORD-vermelding toevoegen **LocalAccountTokenFilterPolicy**, met een waarde van 1.
3. Bereid een root-account op de bronserver Linux wilt installeren op virtuele Linux-machines.


## <a name="check-vmware-server-requirements"></a>Controleer de vereisten voor VMware-server

Zorg ervoor dat de VMware-servers aan de volgende vereisten voldoen.

**Onderdeel** | **Vereiste**
--- | ---
**vCenter-server** | vCenter 6.5, 6.0 of 5.5
**vSphere-host** | vSphere 6.5 6.0, 5.5

## <a name="check-vmware-vm-requirements"></a>Controleer de vereisten voor VMware VM

Zorg ervoor dat de virtuele machine aan de Azure-vereisten in de volgende tabel samengevat voldoet.

**VM-vereiste** | **Details**
--- | ---
**Grootte van de besturingssysteemschijf** | Maximaal 2048 GB.
**Het aantal schijven voor besturingssysteem** | 1
**Aantal gegevensschijven** | 64- of minder
**De grootte van VHD gegevensschijf** | 4095 GB
**Netwerkadapters** | Meerdere netwerkadapters worden ondersteund
**Gedeelde VHD** | Niet ondersteund
**FC-schijf** | Niet ondersteund
**Harde schijf-indeling** | VHD- of VHDX.<br/><br/> Hoewel VHDX wordt momenteel niet ondersteund in Azure, Site Recovery automatisch geconverteerd VHDX naar VHD wanneer u een failover naar Azure. Wanneer u terug naar de lokale virtuele machines schakelt blijven gebruiken van de VHDX-indeling.
**Bitlocker** | Wordt niet ondersteund. Uitschakelen voordat u replicatie voor een virtuele machine inschakelen.
**VM-naam** | Tussen 1 en 63 tekens.<br/><br/> Alleen letters, cijfers en afbreekstreepjes. De VM-naam moet beginnen en eindigen met een letter of cijfer.
**VM-type** | Generatie 1 - Linux of Windows<br/><br/>Generatie 2 - alleen Windows

De virtuele machine moet ook een ondersteund besturingssysteem worden uitgevoerd. Zie de [Site Recovery-ondersteuningsmatrix](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) voor een volledige lijst met ondersteunde versies.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Tijdens een failover-scenario is het raadzaam vanaf uw on-premises netwerk verbinding maken met uw gerepliceerde virtuele machines in Azure.

Als u wilt verbinding maken met Windows-VM's met RDP na een failover, het volgende doen:

1. Als u wilt openen via het internet, schakelt u RDP op de lokale virtuele machine voordat failover wordt uitgevoerd. Zorg ervoor dat TCP en UDP-regels worden toegevoegd voor de **openbare** profiel, en dat RDP is toegestaan in **Windows Firewall** > **Apps toegestaan** voor alle profielen.
2. Als u wilt openen via site-naar-site VPN, schakelt u RDP in op de on-premises machine. RDP moet worden toegestaan de **Windows Firewall** -> **toegestane apps en functies** voor **domein- en persoonlijke** netwerken.
   Controleer of de SAN-beleid van het besturingssysteem is ingesteld op **OnlineAll**. [Meer informatie](https://support.microsoft.com/kb/3031135). Er moet geen Windows-updates in behandeling op de virtuele machine wanneer u een failover activeren. Als er, kunt u niet aanmelden met de virtuele machine totdat de update is voltooid.
3. Controleer op de Windows Azure virtuele machine na een failover **opstarten diagnostics** om een schermopname van de virtuele machine weer te geven. Als u geen verbinding maken, Controleer of de virtuele machine wordt uitgevoerd en deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Voor verbinding met virtuele Linux-machines met behulp van SSH na een failover, het volgende doen:

1. Controleer op de lokale machine vóór de failover, dat de Secure Shell-service is ingesteld op automatisch starten op opstarten van het systeem. Controleer dat de firewallregels voor een SSH-verbinding toestaan.

2. Op de Azure virtuele machine na een failover, kunt u binnenkomende verbindingen voor de SSH-poort voor de netwerkbeveiligingsgroepen op failover VM en voor het Azure-subnet waarmee deze verbonden.
   [Een openbaar IP-adres toevoegen](site-recovery-monitoring-and-troubleshooting.md) voor de virtuele machine. U kunt controleren **opstarten diagnostics** om een schermopname van de virtuele machine weer te geven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Herstel na noodgevallen naar Azure instellen voor virtuele VMware-machines](tutorial-vmware-to-azure.md)
