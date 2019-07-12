---
title: Azure Migrate ondersteuningsmatrix voor Hyper-V-evaluatie en migratie
description: Geeft een overzicht van instellingen en beperkingen voor Hyper-V-evaluatie en migratie met behulp van de service Azure Migrate.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/02/2019
ms.author: raynew
ms.openlocfilehash: f6edbe19429b38d68aea1f1ecfe426c9b2d194d0
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811347"
---
# <a name="support-matrix-for-hyper-v-assessment-and-migration"></a>Ondersteuningsmatrix voor Hyper-V-evaluatie en migratie

U kunt de [Azure Migrate-service](migrate-overview.md) om te beoordelen en migreren van machines naar de Microsoft Azure-cloud. In dit artikel bevat een overzicht van instellingen voor de ondersteuning en beperkingen voor het beoordelen en migreren van on-premises Hyper-V-machines.



## <a name="hyper-v-scenarios"></a>Hyper-V-scenario 's

De tabel bevat een overzicht van ondersteunde scenario's voor Hyper-V-machines.

**Implementatie** | **Details*** 
--- | --- 
**On-premises Hyper-V-VM's beoordelen** | [Instellen van](tutorial-prepare-hyper-v.md) uw eerste evaluatie.<br/><br/> [Voer](scale-hyper-v-assessment.md) een grootschalige evaluatie.
**Hyper-V-machines migreren naar Azure** | [Probeer](tutorial-migrate-hyper-v.md) migratie naar Azure.

    

## <a name="azure-migrate-projects"></a>Azure Migrate-projecten

**Ondersteuning** | **Details**
--- | ---
Azure-machtigingen | U moet machtigingen voor Inzender of eigenaar in het abonnement voor het maken van een Azure Migrate-project.
Virtuele Hyper-V-machines | Beoordeling van maximaal 10.000 Hyper-V-machines in een enkel project.

Een project kan bestaan zowel VMware-VM's en Hyper-V-machines, totdat de evaluatie.


## <a name="assessment-hyper-v-host-requirements"></a>Evaluatie-Hyper-V-host-vereisten

| **Ondersteuning**                | **Details**               
| :-------------------       | :------------------- |
| **Host-implementatie**       | De Hyper-V-host kan zelfstandig of in een cluster geïmplementeerd. |
| **Machtigingen**           | U moet beheerdersmachtigingen hebben op de Hyper-V-host. |
| **Host het besturingssysteem** | WindowsServer 2016 of Windows Server 2012 R2.<br/> U kunt geen VM's op Hyper-V-hosts met Windows Server 2019 beoordelen. |
| **Externe communicatie van PowerShell**   | Moet zijn ingeschakeld op elke host. |
| **Hyper-V Replica**       | Als u Hyper-V Replica gebruiken (of u meerdere virtuele machines met de dezelfde VM-id's hebt), en beide de oorspronkelijke en de gerepliceerde VM's detecteren met behulp van Azure Migrate, de evaluatie die worden gegenereerd door Azure Migrate mogelijk niet nauwkeurig. |


## <a name="assessment-hyper-v-vm-requirements"></a>Vereisten voor evaluatie-Hyper-V VM 's

| **Ondersteuning**                  | **Details**               
| :----------------------------- | :------------------- |
| **Besturingssysteem** | Alle [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) en [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) besturingssystemen die worden ondersteund door Azure. |
| **Machtigingen**           | U moet beheerdersmachtigingen hebben op elke Hyper-V virtuele machine die u wilt beoordelen. |
| **Integratieservices**       | [Hyper-V-integratieservices](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) moet worden uitgevoerd op virtuele machines die u hebt geëvalueerd, om informatie over het besturingssysteem vastleggen. |
| **Vereiste wijzigingen voor Azure** | Sommige virtuele machines mogelijk moeten worden gewijzigd zodat ze in Azure uitvoeren kunnen. Met Azure Migrate kunt deze wijzigingen automatisch voor de volgende besturingssystemen:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> -Debian 7,8<br/><br/> Voor andere besturingssystemen moet u aanpassingen handmatig vóór de migratie. De relevante artikelen bevatten instructies over hoe u dit doet. |
| **Opstarten voor Linux**                 | Als er zich bevinden op een specifieke partitie, moet zich bevinden op de besturingssysteemschijf en niet worden verdeeld over meerdere schijven.<br/> Als bevinden maakt deel uit van de hoofdpartitie (/), moet klikt u vervolgens de partitie '/' zich op de schijf met het besturingssysteem en andere schijven zijn niet verdeeld. |
| **Opstarten via UEFI**                  | Virtuele machines met opstarten via UEFI worden niet ondersteund voor migratie. |
| **Versleutelde schijven/volumes**    | Virtuele machines met versleutelde schijven/volumes worden niet ondersteund voor migratie. |
| **RDM/passthrough-schijven**      | Als virtuele machines RDM of passthrough-schijven hebt, wordt deze schijven wordt niet gerepliceerd naar Azure. |
| **NFS**                        | NFS-volumes die zijn gekoppeld als volumes op de virtuele machines wordt niet worden gerepliceerd. |
| **Doelschijf**                | Azure Migrate-evaluaties aanbevelen migratie van virtuele Azure-machines met beheerde schijven alleen. |


## <a name="assessment-appliance-requirements"></a>Vereisten voor evaluatie-apparaat

Voor evaluatie voert Azure Migrate een lichtgewicht toestel voor Hyper-V virtuele machines detecteren en verzenden van de gegevens van de metagegevens en prestaties van de virtuele machine naar Azure migreren. Het apparaat wordt uitgevoerd op een Hyper-V-VM en u instellen met een gecomprimeerde Hyper-V virtuele harde schijf die u vanuit de Azure-portal downloaden. De volgende tabel geeft een overzicht van de vereisten van het apparaat.

| **Ondersteuning**                | **Details**               
| :-------------------       | :------------------- |
| **Azure Migrate-project**  |  Een apparaat kan worden gekoppeld aan een enkel project.<br/> U kunt maximaal 5000 Hyper-V-VM's detecteren met een enkel apparaat.
| **Hyper-V-beperkingen**    |  U implementeren het apparaat als een Hyper-V-VM.<br/> Het opgegeven VM-apparaat is Hyper-V-VM versie 5.0.<br/> De VM-host moet worden uitgevoerd op Windows Server 2012 R2 of hoger.<br/> Moet voldoende ruimte voor het toewijzen van 16 GB RAM-geheugen, 4 virtuele processors en 1 externe switch voor de virtuele machine van het apparaat.<br/> Apparaat vereist een statisch of dynamisch IP-adres, en toegang tot internet.
| **Hyper-V-apparaat**      |  Het apparaat is ingesteld als een Hyper-V-VM.<br/> De VHD die is opgegeven voor het downloaden is Hyper-V-VM versie 5.0.
| **Host**                   | De VM-host met de virtuele machine van het apparaat moet worden uitgevoerd op Windows Server 2012 R2 of hoger.<br/> Er is onvoldoende ruimte voor het toewijzen van 16 GB RAM-geheugen, 4 virtuele processors en één externe switch voor de virtuele machine van het apparaat nodig.<br/> Apparaat vereist een statisch of dynamisch IP-adres, en toegang tot internet. |
| **Migratieondersteuning voor**      | Als u wilt beginnen met het repliceren van machines, moet de migratie van Gateway-service op het apparaat 1.18.7141.12919 of hoger. Meld u aan bij de toestel-web-app om de versie te controleren. |

## <a name="assessment-appliance-url-access"></a>Toegang tot de URL's van evaluatie-apparaat

Voor het evalueren van virtuele machines, moet het apparaat Azure Migrate verbinding met internet.

- Wanneer u het apparaat implementeert, heeft Azure Migrate een controle van de netwerkverbinding tot de URL's in de onderstaande tabel wordt samengevat.
- Als u een URL-gebaseerde firewall.proxy, kunt u toegang tot de URL's in de tabel, ervoor te zorgen dat de proxy wordt omgezet een CNAME-records hebt ontvangen tijdens het opzoeken van de URL's.
- Als u een onderschept proxy hebt, moet u mogelijk het servercertificaat importeren van de proxyserver op het apparaat. 

    
**URL** | **Details**  
--- | --- 
*.portal.azure.com | Navigatie naar Azure portal
*.windows.net | Meld u aan bij uw Azure-abonnement
*.microsoftonline.com | Het maken van Azure Active Directory-toepassingen voor apparaat communicatie van de service.
management.azure.com | Het maken van Azure Active Directory-toepassingen voor apparaat communicatie van de service.
dc.services.visualstudio.com | Logboekregistratie en bewaking 
*.vault.azure.net | Geheimen in Azure Key Vault beheren tijdens de communicatie tussen het apparaat en de service.


## <a name="assessment-port-requirements"></a>Evaluatie-port requirements for Windows

De volgende tabel geeft een overzicht van Poortvereisten voor evaluatie.

**apparaat** | **verbinding**
--- | --- 
**Apparaat** | Binnenkomende verbindingen op TCP-poort 3389 voor verbindingen met extern bureaublad op het apparaat toestaan.<br/> Binnenkomende verbindingen op poort 44368 voor externe toegang tot de toestel-management-app met behulp van de URL: https://<appliance-ip-or-name>:44368<br/> Uitgaande verbindingen op poort 443 voor het verzenden van metagegevens voor de detectie en prestaties voor het migreren van Azure.
**Hyper-V-host/cluster** | Binnenkomende verbindingen op WinRM poort 5985 (HTTP) en 5986 (HTTPS) voor het ophalen van metagegevens voor de configuratie en prestaties van de Hyper-V-virtuele machines met behulp van een sessie Common Information Model (CIM).

## <a name="migration-hyper-v-host-requirements"></a>Vereisten voor migratie-Hyper-V-host

| **Ondersteuning**                | **Details**               
| :-------------------       | :------------------- |
| **Host-implementatie**       | De Hyper-V-host kan zelfstandig of in een cluster geïmplementeerd. |
| **Machtigingen**           | U moet beheerdersmachtigingen hebben op de Hyper-V-host. |
| **Host het besturingssysteem** | WindowsServer 2019, WindowsServer 2016 of Windows Server 2012 R2. |

## <a name="migration-hyper-v-vm-requirements"></a>Vereisten voor migratie-Hyper-V VM 's

| **Ondersteuning**                  | **Details**               
| :----------------------------- | :------------------- |
| **Besturingssysteem** | Alle [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) en [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) besturingssystemen die worden ondersteund door Azure. |
| **Machtigingen**           | U moet beheerdersmachtigingen hebben op elke Hyper-V virtuele machine die u wilt beoordelen. |
| **Integratieservices**       | [Hyper-V-integratieservices](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) moet worden uitgevoerd op virtuele machines die u hebt geëvalueerd, om informatie over het besturingssysteem vastleggen. |
| **Vereiste wijzigingen voor Azure** | Sommige virtuele machines mogelijk moeten worden gewijzigd zodat ze in Azure uitvoeren kunnen. Met Azure Migrate kunt deze wijzigingen automatisch voor de volgende besturingssystemen:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> -Debian 7,8<br/><br/> Voor andere besturingssystemen moet u aanpassingen handmatig vóór de migratie. De relevante artikelen bevatten instructies over hoe u dit doet. |
| **Opstarten voor Linux**                 | Als er zich bevinden op een specifieke partitie, moet zich bevinden op de besturingssysteemschijf en niet worden verdeeld over meerdere schijven.<br/> Als bevinden maakt deel uit van de hoofdpartitie (/), moet klikt u vervolgens de partitie '/' zich op de schijf met het besturingssysteem en andere schijven zijn niet verdeeld. |
| **Opstarten via UEFI**                  | Virtuele machines met opstarten via UEFI worden niet ondersteund voor migratie. |
| **Versleutelde schijven/volumes**    | Virtuele machines met versleutelde schijven/volumes worden niet ondersteund voor migratie. |
| **RDM/passthrough-schijven**      | Als virtuele machines RDM of passthrough-schijven hebt, wordt deze schijven wordt niet gerepliceerd naar Azure. |
| **NFS**                        | NFS-volumes die zijn gekoppeld als volumes op de virtuele machines wordt niet worden gerepliceerd. |
| **Doelschijf**                | U kunt migreren naar virtuele Azure-machines met beheerde schijven alleen. |




## <a name="migration-hyper-v-host-url-access"></a>Toegang tot de migratie-Hyper-V-host-URL 's

De volgende tabel geeft een overzicht van URL-toegangsvereisten voor Hyper-V-hosts.

**URL** | **Details**  
--- | ---
login.microsoftonline.com | En identiteitsbeheer beheer van toegang tot Active Directory.
*.backup.windowsazure.com | Overdracht van replicatiegegevens en coördinatie.
*.hypervrecoverymanager.windowsazure.com | Verbinding maken met Azure Migrate-service-URL's.
*.blob.core.windows.net | Gegevens uploaden naar storage-accounts.
dc.services.visualstudio.com | App-logboeken die worden gebruikt voor het controleren van interne te uploaden.
time.windows.com | Controleert of de tijdsynchronisatie tussen de systeemtijd en de algemene tijd.

## <a name="migration-port-access"></a>Migratie-port toegang

De volgende tabel geeft een overzicht van Poortvereisten voor Hyper-V-hosts en VM's voor VM-migratie.

**apparaat** | **verbinding**
--- | --- 
Hyper-V-hosts/VM 's | Uitgaande verbindingen op HTTPS-poort 443 voor het verzenden van gegevens van de VM-replicatie naar Azure Migrate.

  
## <a name="migration-vm-disk-support"></a>Ondersteuning voor migratie-VM-schijven 

**Ondersteuning** | **Details**
--- | ---
Gemigreerde schijven | VM's kunnen alleen worden gemigreerd naar managed disks (standard HHD, premium SSD) in Azure.
   

## <a name="next-steps"></a>Volgende stappen

[Voorbereiden voor de evaluatie van Hyper-V-VM](tutorial-prepare-hyper-v.md) voor migratie.




 
