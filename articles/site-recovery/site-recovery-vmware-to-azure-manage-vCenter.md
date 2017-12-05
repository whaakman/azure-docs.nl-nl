---
title: " Een VMware vCenter server beheren in Azure Site Recovery | Microsoft Docs"
description: Dit artikel wordt beschreven hoe toevoegen en beheren van VMware vCenter in Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 12/04/2017
ms.author: anoopkv
ms.openlocfilehash: 2dcca54c9e1e9330bf7c2a37dc08f67817b6af47
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="manage-vmware-vcenter-server-in-azure-site-recovery"></a>Beheren van VMware vCenter-Server in Azure Site Recovery
Dit artikel worden de verschillende Site Recovery-bewerkingen die kunnen worden uitgevoerd op een VMware vCenter.

## <a name="prerequisites"></a>Vereisten

**Ondersteuning van VMware vCenter- en VMware vSphere ESX-Host** | **Details** |
|--- | --- |
|**On-premises VMware-servers** | Een of meer VMware vSphere servers, met 6.5,6.0, 5.5 met de meest recente updates. Servers moeten zich bevinden in hetzelfde netwerk bevindt als de configuratieserver (of een afzonderlijk processerver).<br/><br/> U wordt aangeraden een vCenter-server voor het beheren van hosts, met 6.5,6.0 of 5.5 met de meest recente updates. Alleen de functies die beschikbaar in 5.5 zijn worden ondersteund bij het implementeren van versie 6.0 zijn.|

## <a name="prepare-an-account-for-automatic-discovery"></a>Voorbereiden van een account voor automatische detectie
Site Recovery moet toegang tot VMware voor de processerver voor het automatisch detecteren van virtuele machines en voor failover en failback van virtuele machines.

* **Migreren**: als u alleen migreren van virtuele VMware-machines naar Azure wilt, zonder ooit weer mislukt, kunt u een VMware-account gebruiken met een rol alleen-lezen. Een dergelijke rol failover kan worden uitgevoerd, maar kan niet worden beveiligde bronmachines afgesloten. Dit is niet nodig zijn voor migratie.
* **Repliceren/herstellen**: als u wilt implementeren, een volledige replicatie (repliceren, failover, failback) het account moet kunnen uitvoeren van bewerkingen zoals het maken en verwijderen van schijven, virtuele machine inschakelen.
* **Automatische detectie**: ten minste een alleen-lezen-account is vereist.


|**Taken** | **Vereiste account/functie** | **Machtigingen** | **Details**|
|--- | --- | --- | ---|
|**Processerver detecteert automatisch virtuele VMware-machines** | U moet ten minste een alleen-lezen-gebruiker | Data Center-object doorgeven aan het onderliggende Object –> rol = alleen-lezen | Gebruiker datacenter niveau toewijzen en toegang heeft tot alle objecten in het datacenter.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven dat onderliggende** object naar de onderliggende objecten (vSphere-hosts, datastores, virtuele machines en netwerken).|
|**Failover** | U moet ten minste een alleen-lezen-gebruiker | Data Center-object doorgeven aan het onderliggende Object –> rol = alleen-lezen | Gebruiker datacenter niveau toewijzen en toegang heeft tot alle objecten in het datacenter.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven dat onderliggende** -object op voor de onderliggende objecten (vSphere-hosts, datastores, virtuele machines en netwerken).<br/><br/> Dit is handig voor migratiedoeleinden, maar niet een volledige replicatie, failover en failback.|
|**Failover en failback** | We raden u een rol (AzureSiteRecoveryRole) maken met de vereiste machtigingen en vervolgens de rol toewijzen aan een VMware-gebruiker of groep | Data Center-object doorgeven aan het onderliggende Object –> rol AzureSiteRecoveryRole =<br/><br/> Datastore -> ruimte toewijzen, gegevensopslag, op laag niveau bestandsbewerkingen bladeren, bestand verwijderen, bestanden van virtuele machine bijwerken<br/><br/> Netwerk -> netwerk toewijzen<br/><br/> Resource -> VM toewijzen aan de resourcegroep, migreren van virtuele machine is uitgeschakeld, stroomvoorziening op virtuele machine migreren<br/><br/> Taken maken taak, updatetaak -><br/><br/> Virtuele machine-configuratie ><br/><br/> Virtuele machine -> Interact -> vraag beantwoorden, apparaatverbinding, CD's configureren, configureren van diskettes, uitschakelen, inschakelen, VMware-hulpprogramma's installeren<br/><br/> Virtuele machine -> inventaris -> maken, registreren, registratie<br/><br/> Virtuele machine inrichten -> -> downloaden van de virtuele machine toestaan, toestaan uploaden van bestanden van virtuele machine<br/><br/> Virtuele machine-momentopnamen >-Remove momentopnamen > | Gebruiker datacenter niveau toewijzen en toegang heeft tot alle objecten in het datacenter.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven dat onderliggende** object naar de onderliggende objecten (vSphere-hosts, datastores, virtuele machines en netwerken).|

## <a name="create-an-account-to-connect-to-vmware-vcenter-server-vmware-vsphere-exsi-host"></a>Maak een account verbinding maken met de VMware vCenter-Server / VMware vSphere EXSi host
1. Meld u aan bij de configuratieserver en start de cspsconfigtool.exe met behulp van de snelkoppeling op het bureaublad geplaatst.
2. Klik op **Account toevoegen** op de **Account beheren** tabblad.

  ![account toevoegen](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Geef de accountdetails en klik op OK om het account toevoegt. Het account beschikken over de machtigingen die worden vermeld in de [voorbereiden van een account voor automatische detectie](#prepare-an-account-for-automatic-discovery) sectie.

  >[!NOTE]
  Het duurt ongeveer 15 minuten voor de accountgegevens van worden gesynchroniseerd met de Site Recovery-service.


## <a name="associate-a-vmware-vcenter-vmware-vsphere-esx-host-add-vcenter"></a>Koppelen van een VMware vCenter / vSphere VMware ESX-host (vCenter toevoegen)
* Blader op de Azure-portal naar *YourRecoveryServicesVault* > **Site Recovery-infrastructuur** > **Configuration servers** > *ConfigurationServer*
* Klik in de pagina met details van de server van de configuratie op de vCenter-knop +.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials-used-to-connect-to-the-vcenter-server-vsphere-esxi-host"></a>Wijzigen van referenties waarmee verbinding met de vCenter-server / vSphere ESXi-host

1. Aanmelden bij de configuratieserver en start de cspsconfigtool.exe
2. Klik op **Account toevoegen** op de **Account beheren** tabblad.

  ![account toevoegen](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Geef de nieuwe accountgegevens en klik op OK om het account toevoegt. Het account beschikken over de machtigingen die worden vermeld in de [voorbereiden van een account voor automatische detectie](#prepare-an-account-for-automatic-discovery) sectie.
4. Blader op de Azure-portal naar *YourRecoveryServicesVault* > **Site Recovery-infrastructuur** > **Configuration servers** > *ConfigurationServer*
5. Klik in de pagina met details van de server van de configuratie op de **Server vernieuwen** knop.
6. Zodra de taak van de server vernieuwen is voltooid, selecteert u de vCenter-Server te openen van de vCenter-overzichtspagina.
7. Selecteer de zojuist toegevoegde account in de **vCenter-server/vSphere hostaccount** veld en klikt u op de **opslaan** knop.

  ![Wijzig-account](./media/site-recovery-vmware-to-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-in-azure-site-recovery"></a>Verwijderen van een vCenter in Azure Site Recovery
1. Blader op de Azure-portal naar *YourRecoveryServicesVault* > **Site Recovery-infrastructuur** > **Configuration servers** > *ConfigurationServer*
2. Selecteer de vCenter-Server te openen van de vCenter-overzichtspagina in pagina met details de configuratieserver.
3. Klik op de **verwijderen** knop om te verwijderen van de vCenter

  ![Delete-account](./media/site-recovery-vmware-to-azure-manage-vcenter/delete-vcenter.png)

> [!NOTE]
Als u wilt wijzigen van de Vcenter-IP-adres/FQDN-naam, poort details moet u de vCenter-Server verwijderen en opnieuw weer toevoegen.
