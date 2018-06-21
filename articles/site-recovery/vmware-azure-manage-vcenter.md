---
title: " Beheren van VMware vCenter-server in Azure Site Recovery | Microsoft Docs"
description: Dit artikel wordt beschreven hoe toevoegen en beheren van VMware vCenter in Azure Site Recovery.
author: AnoopVasudavan
ms.service: site-recovery
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: anoopkv
ms.openlocfilehash: 48b6cf9b90b429520df435aee00f57ea7b588748
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284995"
---
# <a name="manage-vmware-vcenter-servers"></a>VMware vCenter-servers beheren 

Dit artikel worden de verschillende Site Recovery-bewerkingen die kunnen worden uitgevoerd op een VMware vCenter. Controleer of de [vereisten](vmware-physical-azure-support-matrix.md#replicated-machines) voordat u begint.


## <a name="set-up-an-account-for-automatic-discovery"></a>Instellen van een account voor automatische detectie

Site Recovery moet toegang tot VMware voor de processerver voor het automatisch detecteren van virtuele machines en voor failover en failback van virtuele machines. Maak een account om toegang te krijgen als volgt:

1. Meld u bij de configuratie van server-machine.
2. Open het starten van de cspsconfigtool.exe met behulp van de snelkoppeling op het bureaublad.
3. Klik op **Account toevoegen** op de **Account beheren** tabblad.

  ![add-account](./media/vmware-azure-manage-vcenter/addaccount.png)
1. Geef de accountdetails en klik op **OK** toe te voegen.  Het account moet hebben de bevoegdheden die in de volgende tabel samengevat. 

Het duurt ongeveer 15 minuten voor de accountgegevens van worden gesynchroniseerd met de Site Recovery-service.

### <a name="account-permissions"></a>Accountmachtigingen

|**Taak** | **Account** | **Machtigingen** | **Details**|
|--- | --- | --- | ---|
|**Automatische detectie/migreren (zonder failback)** | U moet ten minste een alleen-lezen-gebruiker | Datacentrumobject –> doorgeven naar onderliggend object, rol=Alleen-lezen | Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven dat onderliggende** object naar de onderliggende objecten (vSphere-hosts, datastores, virtuele machines en netwerken).|
|**Replicatie/Failover** | U moet ten minste een alleen-lezen-gebruiker| Datacentrumobject –> doorgeven naar onderliggend object, rol=Alleen-lezen | Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven dat onderliggende** -object op voor de onderliggende objecten (vSphere-hosts, datastores, virtuele machines en netwerken).<br/><br/> Dit is handig voor migratiedoeleinden, maar niet een volledige replicatie, failover en failback.|
|**Failover-replicatie/failback** | We raden u een rol (AzureSiteRecoveryRole) maken met de vereiste machtigingen en vervolgens de rol toewijzen aan een VMware-gebruiker of groep | Data Center-object doorgeven aan het onderliggende Object –> rol AzureSiteRecoveryRole =<br/><br/> Gegevensopslag –> Ruimte toewijzen, browsen in de gegevensopslag, bestandsbewerkingen op laag niveau, bestand verwijderen, bestanden van virtuele machines bijwerken<br/><br/> Netwerk –> Netwerk toewijzen<br/><br/> Resource –> VM toewijzen aan resourcegroep, uitgeschakelde VM migreren, ingeschakelde VM migreren<br/><br/> Taken –> Taak maken, taak bijwerken<br/><br/> Virtuele machine –> Configuratie<br/><br/> Virtuele machine –> Interactie –> vraag beantwoorden, apparaatverbinding, CD's configureren, diskettes configureren, uitschakelen, inschakelen, VMware-hulpprogramma's installeren<br/><br/> Virtuele machine -> Inventaris –> Maken, registreren, registratie opheffen<br/><br/> Virtuele machine –> Inrichten –> Downloaden van virtuele machine toestaan, toestaan dat bestanden van virtuele machines worden geüpload<br/><br/> Virtuele machine –> Momentopnamen -> Momentopnamen verwijderen | Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven dat onderliggende** object naar de onderliggende objecten (vSphere-hosts, datastores, virtuele machines en netwerken).|


## <a name="add-vmware-server-to-the-vault"></a>VMware-server toevoegen aan de kluis

1. Open uw kluis op de Azure portal > **Site Recovery-infrastructuur** > **Configuration servers**, en open de configuratieserver.
2. Op de **Details** pagina, klikt u op **+ vCenter**.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials"></a>Referenties wijzigen

Wijzig de referenties waarmee verbinding met de vCenter-server of ESXi-host als volgt:

1. Meld u aan bij de configuratieserver en start de cspsconfigtool.exe vanaf het bureaublad.
2. Klik op **Account toevoegen** op de **Account beheren** tabblad.

  ![add-account](./media/vmware-azure-manage-vcenter/addaccount.png)
3. Geef de nieuwe accountgegevens en op **OK** toe te voegen. Het account moet hebben de bevoegdheden die zijn vermeld [hierboven](#account-permissions).
4. Open de kluis op de Azure portal > **Site Recovery-infrastructuur** > **Configuration servers**, en open de configuratieserver.
5. In de **Details** pagina, klikt u op **Server vernieuwen**.
6. Nadat de taak van Server vernieuwen is voltooid, selecteert u de vCenter-Server te openen van de vCenter **samenvatting** pagina.
7. Selecteer de zojuist toegevoegde account in de **vCenter-server/vSphere hostaccount** veld en klikt u op **opslaan**.

    ![Wijzig-account](./media/vmware-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-server"></a>Een vCenter-server verwijderen

1. Open uw kluis in de Azure portal > **Site Recovery-infrastructuur** > **Configuration servers**, en open de configuratieserver.
2. Op de **Details** pagina, selecteert u de vCenter-server.
3. Klik op de **verwijderen** knop.

  ![delete-account](./media/vmware-azure-manage-vcenter/delete-vcenter.png)

> [!NOTE]
Als u wijzigen van de vCenter-IP-adres, de FQDN-naam of de poort wilt, moet u de vCenter-server verwijderen en toe te voegen terug naar de portal.
