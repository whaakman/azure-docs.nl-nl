---
title: Lokale VMware bronnen voorbereiden voor replicatie naar Azure met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van de stappen die u nodig hebt voor het repliceren van de werkbelasting op virtuele VMware-machines naar Azure-opslag
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 6aba0e89-ad7c-467e-9db2-cfb3bfe4c7d6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 3e1c589030210c2eae1ad9c02811775d9d6365d4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="step-6-prepare-on-premises-vmware-replication-to-azure"></a>Stap 6: Voorbereiden op premises VMware-replicatie naar Azure

Volg de instructies in dit artikel om voor te bereiden op lokale VMware-servers om te communiceren met Azure Site Recovery en virtuele VMWare-machines worden voorbereid voor de installatie van de Mobility-service. De Mobility-service-agent moet worden geïnstalleerd op alle lokale VM's die u wilt repliceren naar Azure.

## <a name="prepare-for-automatic-discovery"></a>Voorbereiden voor automatische detectie

Site Recovery detecteert automatisch virtuele machines waarop vSphere ESXi-hosts (met of zonder een vCenter-server). Voor automatische detectie moet de Site Recovery een account voor toegang tot hosts en -servers:

1. Een toegewezen account wil gebruiken, maakt u een rol (op het niveau vCenter met de machtigingen beschreven in de onderstaande tabel. Geef een naam zoals **Azure_Site_Recovery**.
2. Vervolgens maken van een gebruiker op de vSphere host/vCenter-server en de rol toewijzen aan de gebruiker. U kunt dit gebruikersaccount opgeven tijdens de implementatie van Site Recovery.


### <a name="vmware-account-permissions"></a>Machtigingen van de VMware-account

Site Recovery moet toegang tot VMware voor de processerver voor het automatisch detecteren van virtuele machines en voor failover en failback van virtuele machines.

- **Migreren**: als u alleen migreren van virtuele VMware-machines naar Azure wilt, zonder ooit terug mislukt, kunt u een VMware-account gebruiken met een rol alleen-lezen. Een dergelijke rol failover kan worden uitgevoerd, maar kan niet worden beveiligde bronmachines afgesloten. Dit is niet nodig zijn voor migratie.
- **Repliceren/herstellen**: als u wilt implementeren, een volledige replicatie (repliceren, failover, failback) het account moet kunnen uitvoeren van bewerkingen zoals het maken en verwijderen van schijven, virtuele machines enzovoort inschakelen.
- **Automatische detectie**: ten minste een alleen-lezen-account is vereist.


**Taak** | **Vereiste account/functie** | **Machtigingen** | **Details**
--- | --- | --- | ---
**Processerver detecteert automatisch virtuele VMware-machines** | U moet ten minste een alleen-lezen-gebruiker | Data Center-object doorgeven aan het onderliggende Object –> rol = alleen-lezen | Gebruiker datacenter niveau toewijzen en toegang heeft tot alle objecten in het datacenter.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven dat onderliggende** object naar de onderliggende objecten (vSphere-hosts, datastores, VM's en -netwerken).
**Failover** | U moet ten minste een alleen-lezen-gebruiker | Data Center-object doorgeven aan het onderliggende Object –> rol = alleen-lezen | Gebruiker datacenter niveau toewijzen en toegang heeft tot alle objecten in het datacenter.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven dat onderliggende** -object op voor de onderliggende objecten (vSphere-hosts, datastores, VM's en -netwerken).<br/><br/> Dit is handig voor migratiedoeleinden, maar niet een volledige replicatie, failover en failback.
**Failover en failback** | We raden u een rol (Azure_Site_Recovery) maken met de vereiste machtigingen en vervolgens de rol toewijzen aan een VMware-gebruiker of groep | Data Center-object doorgeven aan het onderliggende Object –> rol Azure_Site_Recovery =<br/><br/> Datastore -> ruimte toewijzen, gegevensopslag, op laag niveau bestandsbewerkingen bladeren, bestand verwijderen, bestanden van virtuele machine bijwerken<br/><br/> Netwerk -> netwerk toewijzen<br/><br/> Resource -> VM toewijzen aan de resourcegroep, migreren van virtuele machine is uitgeschakeld, stroomvoorziening op virtuele machine migreren<br/><br/> Taken maken taak, updatetaak -><br/><br/> Virtuele machine-configuratie ><br/><br/> Virtuele machine -> Interact -> vraag beantwoorden, apparaatverbinding, CD's configureren, configureren van diskettes, uitschakelen, inschakelen, VMware-hulpprogramma's installeren<br/><br/> Virtuele machine -> inventaris -> maken, registreren, registratie<br/><br/> Virtuele machine inrichten -> -> downloaden van de virtuele machine toestaan, toestaan uploaden van bestanden van virtuele machine<br/><br/> Virtuele machine-momentopnamen >-Remove momentopnamen > | Gebruiker datacenter niveau toewijzen en toegang heeft tot alle objecten in het datacenter.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven dat onderliggende** object naar de onderliggende objecten (vSphere-hosts, datastores, VM's en -netwerken).


## <a name="prepare-for-push-installation-of-the-mobility-service"></a>Voorbereiden voor de push-installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op alle virtuele machines die u wilt repliceren. Er zijn een aantal manieren voor het installeren van de service, inclusief de handmatige installatie, push-installatie van de Site Recovery processerver en de installatie met behulp van methoden zoals System Center Configuration Manager.

Als u wilt de push-installatie gebruiken, moet u een account met Site Recovery gebruiken kunt voor toegang tot de virtuele machine voorbereiden.

- U kunt een domein of lokale account gebruiken
- Als u niet een domeinaccount voor Windows moet u externe gebruiker toegangsbeheer op de lokale computer uitschakelen. Om dit te doen, in het register onder **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, de DWORD-vermelding toevoegen **LocalAccountTokenFilterPolicy**, met een waarde van 1.
- Als u wilt de registervermelding voor Windows uit een CLI toevoegen, typt u:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Voor Linux moet het account hoofdmap op de bronserver Linux zijn.



## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 7: een kluis maken](vmware-walkthrough-create-vault.md)
