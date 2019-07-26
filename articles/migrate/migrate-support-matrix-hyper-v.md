---
title: Azure Migrate-ondersteunings matrix voor Hyper-V-evaluatie en-migratie
description: Geeft een overzicht van de instellingen en beperkingen voor Hyper-V-evaluatie en migratie met behulp van de Azure Migrate-service.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: raynew
ms.openlocfilehash: 507ca6daa30a19b73848d6d3cf253390baf496af
ms.sourcegitcommit: 57a7d4f67635212f5bf0c56e58fd87c8ec366f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372471"
---
# <a name="support-matrix-for-hyper-v-assessment-and-migration"></a>Ondersteuningsmatrix voor Hyper-V-evaluatie en -migratie

U kunt de [Azure migrate-service](migrate-overview.md) gebruiken voor het beoordelen en migreren van machines naar de Microsoft Azure Cloud. Dit artikel bevat een overzicht van de ondersteunings instellingen en beperkingen voor het beoordelen en migreren van on-premises virtuele Hyper-V-machines.



## <a name="hyper-v-scenarios"></a>Hyper-V-scenario's

De tabel bevat een overzicht van de ondersteunde scenario's voor virtuele Hyper-V-machines.

**Implementatie** | **Details***
--- | ---
**On-premises virtuele Hyper-V-machines evalueren** | [Stel](tutorial-prepare-hyper-v.md) uw eerste beoordeling in.<br/><br/> [Voer](scale-hyper-v-assessment.md) een grootschalige evaluatie uit.
**Hyper-V-VM's migreren naar Azure** | [Probeer](tutorial-migrate-hyper-v.md) de migratie naar Azure uit te proberen.



## <a name="azure-migrate-projects"></a>Azure Migrate projecten

**Ondersteuning** | **Details**
--- | ---
Azure-machtigingen | U hebt machtigingen voor Inzender of eigenaar nodig in het abonnement om een Azure Migrate project te maken.
Virtuele Hyper-V-machines | Evalueer Maxi maal 10.000 Hyper-V-Vm's in één project.

Een project kan zowel virtuele VMware-machines als virtuele Hyper-V-machines bevatten, tot aan de evaluatie limieten.

**Geografie** Er zijn een aantal geographs waarin een Azure Migrate project kan worden gemaakt. Hoewel u alleen projecten in deze geografische gebieden kunt maken, kunt u uw machines nog steeds evalueren of migreren voor andere doel locaties. De Geografie van het project wordt alleen gebruikt om de gedetecteerde meta gegevens op te slaan.


 **Geografie** | **Opslag locatie van meta gegevens**
 --- | ---
 Azure Government | VS (overheid) - Virginia
 Azië en Stille Oceaan | Zuidoost-Azië of Azië-oost
 Europa | Zuid-Europa of Europa-west
 Verenigd Koninkrijk | UK-zuid of UK-west
 Verenigde Staten | VS-midden, VS-West 2


 > [!NOTE]
 > Ondersteuning voor Azure Government is momenteel alleen beschikbaar voor de [oudere versie](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) van Azure Migrate.


## <a name="assessment-hyper-v-host-requirements"></a>Beoordeling-vereisten voor de Hyper-V-host

| **Ondersteuning**                | **Details**               
| :-------------------       | :------------------- |
| **Implementatie van host**       | De Hyper-V-host kan zelfstandig of in een cluster worden geïmplementeerd. |
| **Machtigingen**           | U hebt beheerders machtigingen nodig op de Hyper-V-host. |
| **Besturings systeem van host** | Windows Server 2016 of Windows Server 2012 R2.<br/> U kunt geen Vm's beoordelen die zich bevinden op Hyper-V-hosts waarop Windows Server 2019 wordt uitgevoerd. |
| **Externe communicatie met Power shell**   | Moet op elke host zijn ingeschakeld. |
| **Hyper-V replica**       | Als u Hyper-V replica gebruikt (of als u meerdere virtuele machines met dezelfde VM-id's hebt), en zowel de oorspronkelijke als de gerepliceerde Vm's detecteert met Azure Migrate, is de door Azure Migrate gegenereerde evaluatie mogelijk niet nauw keurig. |


## <a name="assessment-hyper-v-vm-requirements"></a>Beoordeling-vereisten voor virtuele Hyper-V-machines

| **Ondersteuning**                  | **Details**               
| :----------------------------- | :------------------- |
| **Besturingssysteem** | Alle [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) -en [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) -besturings systemen die worden ondersteund door Azure. |
| **Machtigingen**           | U hebt beheerders machtigingen nodig op elke Hyper-V-VM die u wilt beoordelen. |
| **Integratie Services**       | [Hyper-V-integratie Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) moeten worden uitgevoerd op vm's die u controleert, om informatie over het besturings systeem vast te leggen. |
| **Vereiste wijzigingen voor Azure** | Voor sommige Vm's zijn mogelijk wijzigingen vereist zodat ze kunnen worden uitgevoerd in Azure. Azure Migrate worden deze wijzigingen automatisch aangebracht voor de volgende besturings systemen:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> -SUSE Linux Enterprise Server 12 SP1 +<br/> -Ubuntu 14.04 LTS, 16.04 LTS, 18.04 LTS<br/> -Debian 7, 8<br/><br/> Voor andere besturings systemen moet u hand matig aanpassingen maken voordat u de migratie uitvoert. De relevante artikelen bevatten instructies over hoe u dit doet. |
| **Linux-opstart**                 | Als/boot zich op een toegewezen partitie bevindt, moet deze zich op de besturingssysteem schijf bevinden en niet over meerdere schijven worden verspreid.<br/> Als/boot deel uitmaakt van de hoofd partitie (/), moet de partitie '/' zich op de besturingssysteem schijf bevindt en niet over andere schijven beschikken. |
| **UEFI-opstart**                  | Vm's met UEFI-opstart bewerkingen worden niet ondersteund voor migratie. |
| **Versleutelde schijven/volumes**    | Vm's met versleutelde schijven/volumes worden niet ondersteund voor migratie. |
| **RDM/passthrough-schijven**      | Als Vm's een RDM-of passthrough-schijf hebben, worden deze schijven niet gerepliceerd naar Azure. |
| **NFS**                        | NFS-volumes die zijn gekoppeld als volumes op de Vm's, worden niet gerepliceerd. |
| **Doel schijf**                | Azure Migrate-evaluaties raden alleen migratie naar Azure-Vm's met Managed disks aan. |


## <a name="assessment-appliance-requirements"></a>Beoordeling-vereisten voor apparaten

Voor evaluatie voert Azure Migrate een licht gewicht in voor de detectie van Hyper-V-Vm's en het verzenden van meta gegevens en prestatie gegevens van de virtuele machine naar Azure Migrate. Het apparaat wordt uitgevoerd op een Hyper-V-VM en u gebruikt een gecomprimeerde Hyper-V-VHD die u downloadt van de Azure Portal. De volgende tabel bevat een overzicht van de vereisten voor het apparaat.

| **Ondersteuning**                | **Details**               
| :-------------------       | :------------------- |
| **Azure Migrate project**  |  Een apparaat kan worden gekoppeld aan een enkel project.<br/> U kunt Maxi maal 5000 virtuele Hyper-V-machines detecteren met één apparaat.
| **Beperkingen voor Hyper-V**    |  U implementeert het apparaat als een Hyper-V-VM.<br/> De virtuele machine van het apparaat is Hyper-V VM versie 5,0.<br/> Op de VM-host moet Windows Server 2012 R2 of later worden uitgevoerd.<br/> Er is voldoende ruimte nodig om 16 GB RAM, 4 virtuele processors en 1 externe switch voor de toestel-VM toe te wijzen.<br/> Voor het apparaat is een statisch of dynamisch IP-adres en Internet toegang vereist.
| **Hyper-V-apparaat**      |  Het apparaat wordt ingesteld als een Hyper-V-VM.<br/> De virtuele harde schijf die moet worden gedownload, is Hyper-V VM versie 5,0.
| **Host**                   | Op de VM-host waarop de toestel-VM wordt uitgevoerd, moet Windows Server 2012 R2 of later worden uitgevoerd.<br/> Er is voldoende ruimte nodig om 16 GB RAM, 4 virtuele processors en één externe switch voor de toestel-VM toe te wijzen.<br/> Voor het apparaat is een statisch of dynamisch IP-adres en Internet toegang vereist. |
| **Migratie ondersteuning**      | Als u computers wilt repliceren, moet de migratie Gateway Service op het apparaat 1.18.7141.12919 of hoger zijn. Meld u aan bij de Web-App van het apparaat om de versie te controleren. |

## <a name="assessment-appliance-url-access"></a>Beoordeling: toestel-URL-toegang

Voor het beoordelen van Vm's heeft het Azure Migrate-apparaat Internet verbinding nodig.

- Wanneer u het apparaat implementeert, controleert Azure Migrate een connectiviteits controle op de Url's die in de onderstaande tabel worden samenvatten.
- Als u een firewall op basis van een URL gebruikt. proxy, toegang tot de Url's in de tabel toestaan, zodat de proxy alle CNAME-records verhelpt die zijn ontvangen tijdens het opzoeken van de Url's.
- Als u een onderschepende proxy hebt, moet u mogelijk het server certificaat van de proxy server importeren naar het apparaat.


**URL** | **Details**  
--- | ---
*.portal.azure.com | Navigeren naar de Azure Portal
*.windows.net | Meld u aan bij uw Azure-abonnement
*.microsoftonline.com | Het maken van Azure Active Directory-toepassingen voor het apparaat aan service communicatie.
management.azure.com | Het maken van Azure Active Directory-toepassingen voor het apparaat aan service communicatie.
dc.services.visualstudio.com | Logboekregistratie en bewaking
*.vault.azure.net | Geheimen in Azure Key Vault beheren bij de communicatie tussen het apparaat en de service.


## <a name="assessment-port-requirements"></a>Beoordeling-poort vereisten

De volgende tabel bevat een overzicht van de poort vereisten voor evaluatie.

**Apparaatconfiguratie** | **verbinding**
--- | ---
**Apparaat** | Binnenkomende verbindingen op TCP-poort 3389 om extern bureau blad-verbindingen met het apparaat toe te staan.<br/> Binnenkomende verbindingen op poort 44368 voor externe toegang tot de app voor het beheren van apparaten met behulp van de URL: https://< apparaat-IP-of-name >: 44368<br/> Uitgaande verbindingen op poort 443 voor het verzenden van meta gegevens voor detectie en prestaties naar Azure Migrate.
**Hyper-V-host/cluster** | Binnenkomende verbindingen op WinRM-poorten 5985 (HTTP) en 5986 (HTTPS) voor het ophalen van meta gegevens over de configuratie en prestaties van de virtuele Hyper-V-machines met behulp van een Common Information Model (CIM)-sessie.

## <a name="migration-hyper-v-host-requirements"></a>Migratie-vereisten voor Hyper-V-host

| **Ondersteuning**                | **Details**               
| :-------------------       | :------------------- |
| **Implementatie van host**       | De Hyper-V-host kan zelfstandig of in een cluster worden geïmplementeerd. |
| **Machtigingen**           | U hebt beheerders machtigingen nodig op de Hyper-V-host. |
| **Besturings systeem van host** | Windows Server 2019, Windows Server 2016 of Windows Server 2012 R2. |

## <a name="migration-hyper-v-vm-requirements"></a>Migratie-VM-vereisten voor Hyper-V

| **Ondersteuning**                  | **Details**               
| :----------------------------- | :------------------- |
| **Besturingssysteem** | Alle [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) -en [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) -besturings systemen die worden ondersteund door Azure. |
| **Machtigingen**           | U hebt beheerders machtigingen nodig op elke Hyper-V-VM die u wilt beoordelen. |
| **Integratie Services**       | [Hyper-V-integratie Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) moeten worden uitgevoerd op vm's die u controleert, om informatie over het besturings systeem vast te leggen. |
| **Vereiste wijzigingen voor Azure** | Voor sommige Vm's zijn mogelijk wijzigingen vereist zodat ze kunnen worden uitgevoerd in Azure. Azure Migrate worden deze wijzigingen automatisch aangebracht voor de volgende besturings systemen:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> -SUSE Linux Enterprise Server 12 SP1 +<br/> -Ubuntu 14.04 LTS, 16.04 LTS, 18.04 LTS<br/> -Debian 7, 8<br/><br/> Voor andere besturings systemen moet u hand matig aanpassingen maken voordat u de migratie uitvoert. De relevante artikelen bevatten instructies over hoe u dit doet. |
| **Linux-opstart**                 | Als/boot zich op een toegewezen partitie bevindt, moet deze zich op de besturingssysteem schijf bevinden en niet over meerdere schijven worden verspreid.<br/> Als/boot deel uitmaakt van de hoofd partitie (/), moet de partitie '/' zich op de besturingssysteem schijf bevindt en niet over andere schijven beschikken. |
| **UEFI-opstart**                  | Vm's met UEFI-opstart bewerkingen worden niet ondersteund voor migratie. |
| **Versleutelde schijven/volumes**    | Vm's met versleutelde schijven/volumes worden niet ondersteund voor migratie. |
| **RDM/passthrough-schijven**      | Als Vm's een RDM-of passthrough-schijf hebben, worden deze schijven niet gerepliceerd naar Azure. |
| **NFS**                        | NFS-volumes die zijn gekoppeld als volumes op de Vm's, worden niet gerepliceerd. |
| **Doel schijf**                | U kunt alleen migreren naar Azure-Vm's met Managed disks. |




## <a name="migration-hyper-v-host-url-access"></a>Migratie: Hyper-V-host-URL-toegang

De volgende tabel bevat een overzicht van de vereisten voor URL-toegang voor Hyper-V-hosts.

**URL** | **Details**  
--- | ---
login.microsoftonline.com | Toegangs beheer en identiteits beheer met behulp van Active Directory.
*.backup.windowsazure.com | Replicatie gegevens overdragen en coördinatie.
*.hypervrecoverymanager.windowsazure.com | Verbinding maken met Azure Migrate service-Url's.
*.blob.core.windows.net | Gegevens uploaden naar opslag accounts.
dc.services.visualstudio.com | App-logboeken uploaden die worden gebruikt voor interne bewaking.
time.windows.com | Hiermee wordt de tijd synchronisatie tussen systeem en Global time gecontroleerd.

## <a name="migration-port-access"></a>Migratie-poort toegang

De volgende tabel bevat een overzicht van de poort vereisten op Hyper-V-hosts en virtuele machines voor de migratie van Vm's.

**Apparaatconfiguratie** | **verbinding**
--- | ---
Hyper-V-hosts/Vm's | Uitgaande verbindingen op HTTPS-poort 443 voor het verzenden van VM-replicatie gegevens naar Azure Migrate.


## <a name="migration-vm-disk-support"></a>Migratie: ondersteuning voor VM-schijf

**Ondersteuning** | **Details**
--- | ---
Gemigreerde schijven | Vm's kunnen alleen worden gemigreerd naar Managed disks (Standard HHD, Premium SSD) in Azure.


## <a name="next-steps"></a>Volgende stappen

[Bereid u voor op de Hyper-V VM-evaluatie](tutorial-prepare-hyper-v.md) voor migratie.
