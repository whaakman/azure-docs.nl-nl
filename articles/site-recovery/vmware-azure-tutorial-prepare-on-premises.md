---
title: On-premises VMware-servers voorbereiden op herstel van VMware-VM’s naar Azure na een noodgeval| Microsoft Docs
description: Leer hoe u on-premises VMware-servers kunt voorbereiden op herstel naar Azure na een noodgeval met behulp van de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/08/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 07f62775c9286250d33635febe01dbad4362df12
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="prepare-on-premises-vmware-servers-for-disaster-recovery-to-azure"></a>On-premises VMware-servers voorbereiden op herstel naar Azure na een noodgeval

Deze tutorial laat u zien hoe u uw on-premises VMware-infrastructuur kunt voorbereiden wanneer u VMware-VM's wilt repliceren naar Azure. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een account voorbereiden op de vCenter-server of vSphere ESXi-host om VM-detectie te automatiseren
> * Een account voorbereiden voor de automatische installatie van de Mobility-service op VMware VM’s
> * Overzicht van VMware-serververeisten
> * Overzicht van VMware-VM-vereisten

In deze zelfstudiereeks laten we zien hoe u een back-up kunt maken van één VM met behulp van Azure Site Recovery. Als u van plan bent meerdere VMware-VM's te beschermen, moet u het hulpprogramma [Deployment Planner](https://aka.ms/asr-deployment-planner) voor VMware-replicatie downloaden. Met dit hulpprogramma verzamelt u informatie over VM-compatibiliteit, schijven per VM en gegevensverloop per schijf. Het hulpprogramma behandelt ook de netwerkbandbreedtevereisten en de Azure-infrastructuur die nodig is voor een succesvolle replicatie en testfailover. [Meer informatie](site-recovery-deployment-planner.md) over het uitvoeren van het hulpprogramma.

Dit is de tweede zelfstudie in de reeks. Zorg ervoor dat u [de Azure-onderdelen hebt ingesteld](tutorial-prepare-azure.md) zoals beschreven in de vorige zelfstudie.

## <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Automatisch VM’s detecteren. Er is minimaal een alleen-lezen-account vereist.
- Replicatie, failover en failback orkestreren. U hebt een account nodig dat bewerkingen kan uitvoeren als het maken en verwijderen van schijven en het inschakelen van VM’s.

Ga als volgt te werk om het account te maken:

1. Als u een vast account wilt gebruiken, maakt u een rol op vCenter-niveau. Geef de rol een naam zoals **Azure_Site_Recovery**.
2. Wijs de rol de rechten toe die worden samengevat in onderstaande tabel.
3. Maak een gebruiker op de vCenter-server of vSphere-host. Wijs de rol toe aan de gebruiker.

### <a name="vmware-account-permissions"></a>Machtigingen voor VMware-account

**Taak** | **Rol/machtigingen** | **Details**
--- | --- | ---
**VM-detectie** | Ten minste een alleen-lezen-gebruiker<br/><br/> Datacentrumobject –> doorgeven naar onderliggend object, rol=Alleen-lezen | Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.<br/><br/> Wijs om de toegang te beperken de rol **Geen toegang** met het object **Doorgeven naar onderliggend object** toe aan de onderliggende objecten (vSphere-hosts, gegevensopslag, VM’s en netwerken).
**Volledige replicatie, failover en failback** |  Maak een rol (Azure_Site_Recovery) met de vereiste machtigingen en wijs de rol toe aan een VMware-gebruiker of -groep<br/><br/> Datacentrumobject –> Doorgeven naar onderliggend object, rol=Azure_Site_Recovery<br/><br/> Gegevensopslag –> Ruimte toewijzen, browsen in de gegevensopslag, bestandsbewerkingen op laag niveau, bestand verwijderen, bestanden van virtuele machines bijwerken<br/><br/> Netwerk –> Netwerk toewijzen<br/><br/> Resource –> VM toewijzen aan resourcegroep, uitgeschakelde VM migreren, ingeschakelde VM migreren<br/><br/> Taken –> Taak maken, taak bijwerken<br/><br/> Virtuele machine –> Configuratie<br/><br/> Virtuele machine –> Interactie –> vraag beantwoorden, apparaatverbinding, CD's configureren, diskettes configureren, uitschakelen, inschakelen, VMware-hulpprogramma's installeren<br/><br/> Virtuele machine -> Inventaris –> Maken, registreren, registratie opheffen<br/><br/> Virtuele machine –> Inrichten –> Downloaden van virtuele machine toestaan, toestaan dat bestanden van virtuele machines worden geüpload<br/><br/> Virtuele machine –> Momentopnamen -> Momentopnamen verwijderen | Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.<br/><br/> Wijs om de toegang te beperken de rol **Geen toegang** met het object **Doorgeven naar onderliggend object** toe aan de onderliggende objecten (vSphere-hosts, gegevensopslag, VM’s en netwerken).

## <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op de VM die u wilt repliceren. Site Recovery installeert deze service automatisch wanneer u replicatie voor de VM inschakelt. Voor automatische installatie moet u een account voorbereiden waarmee Site Recovery toegang kan krijgen tot de VM. U geeft dit account op bij het instellen van herstel na noodgevallen in de Azure-console.

1. Bereid een domein of lokaal account met machtigingen voor om op de VM te installeren.
2. Als u op Windows-VM’s wilt installeren en u geen domeinaccount gebruikt, moet u toegangsbeheer voor externe gebruikers uitschakelen op de lokale computer.
   - Voeg in het register onder **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** de DWORD-vermelding **LocalAccountTokenFilterPolicy** toe met de waarde 1.
3. Voor installatie op Linux-VM’s bereidt u een superuser voor op de bron-Linux-server.


## <a name="check-vmware-server-requirements"></a>Vereisten voor VMware-servers controleren

Zorg ervoor dat VMware-servers aan de volgende vereisten voldoen.

**Onderdeel** | **Vereiste**
--- | ---
**vCenter-server** | vCenter 6.5, 6.0 of 5.5
**vSphere-host** | vSphere 6.5, 6.0, 5.5

## <a name="check-vmware-vm-requirements"></a>Vereisten voor VMware-VM’s controleren

Zorg ervoor dat de VM voldoet aan de Azure-vereisten die worden samengevat in de volgende tabel.

**VM-vereiste** | **Details**
--- | ---
**Grootte van de besturingssysteemschijf** | Maximaal 2048 GB.
**Aantal besturingssysteemschijven** | 1
**Aantal gegevensschijven** | 64 of minder
**Grootte van VHD-gegevensschijf** | Maximaal 4095 GB
**Netwerkadapters** | Meerdere netwerkadapters worden ondersteund
**Gedeelde VHD** | Niet ondersteund
**FC-schijf** | Niet ondersteund
**Harde-schijfindeling** | VHD of VHDX.<br/><br/> Hoewel VHDX op dit moment niet wordt ondersteund in Azure, zet Site Recovery VHDX automatisch om in VHD wanneer u een failover-overschakeling uitvoert naar Azure. Wanneer u een failover uitvoert naar on-premises, blijven VM’s gebruikmaken van de VHDX-indeling.
**Bitlocker** | Wordt niet ondersteund. Uitschakelen voordat u replicatie voor een virtuele machine inschakelt.
**VM-naam** | 1 tot 63 tekens.<br/><br/> Alleen letters, cijfers en afbreekstreepjes. De VM-naam moet beginnen en eindigen met een letter of cijfer.
**VM-type** | Generatie 1 - Linux of Windows<br/><br/>Generatie 2 - alleen Windows

Op de VM moet ook een ondersteund besturingssysteem worden uitgevoerd. Zie de [ondersteuningsmatrix voor VMware en fysieke servers](vmware-physical-azure-support-matrix.md#replicated-machines) voor een volledige lijst met ondersteunde versies.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Tijdens een failover-scenario kunt u het best verbinding maken met uw gerepliceerde VM’s in Azure vanuit uw on-premises netwerk.

Als u na een failover verbinding wilt maken met Windows-VM’s met behulp van RDP, doet u het volgende:

1. Om toegang te krijgen via het internet, schakelt u voor de failover RDP in op de on-premises VM. Zorg ervoor dat TCP- en UDP-regels zijn toegevoegd voor het profiel **Openbaar** en dat RDP is toegestaan in **Windows Firewall** > **Toegestane apps** voor alle profielen.
2. Voor toegang via VPN tussen sites schakelt u RDP in op de on-premises computer. RDP moet toegestaan zijn in de **Windows Firewall** -> **Toegestane apps en onderdelen** voor **Domein en Privé**-netwerken.
   Controleer of het SAN-beleid van het besturingssysteem is ingesteld op **OnlineAll**. [Meer informatie](https://support.microsoft.com/kb/3031135). Er mogen geen Windows-updates in behandeling zijn op de VM wanneer u een failover activeert. Als die er zijn, kunt u pas weer bij de virtuele machine inloggen als de update is voltooid.
3. Controleer na een failover **Diagnostische gegevens over opstarten** op de Windows Azure VM om een schermopname van de VM weer te geven. Als u geen verbinding kunt maken, controleer dan of de VM actief is en bekijk deze [tips voor het oplossen van problemen](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Als u na een failover verbinding wilt maken met Linux-VM’s met behulp van SSH, doet u het volgende:

1. Controleer voor de failover op de on-premises computer of de Secure Shell-service is ingesteld om automatisch te starten bij het opstarten van het systeem. Controleer of SSH-verbindingen zijn toegestaan door de firewallregels.

2. Sta na de failover op de Azure VM inkomende verbindingen toe met de SSH-poort voor de netwerkbeveiligingsgroepsregels voor de VM waarvoor een failover is uitgevoerd en voor het Azure-subnet waarmee deze is verbonden.
   [Voeg een openbaar IP-adres toe](site-recovery-monitoring-and-troubleshooting.md) voor de VM. U kunt de **Diagnostische gegevens over opstarten** controleren om een schermopname van de VM weer te geven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Herstel van VMware-VM’s naar Azure na een noodgeval instellen](vmware-azure-tutorial.md)
