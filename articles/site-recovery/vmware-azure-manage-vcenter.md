---
title: Beheren van VMware vCenter-servers voor herstel na noodgevallen van virtuele VMware-machines naar Azure met Azure Site Recovery | Microsoft-Docs
description: Dit artikel wordt beschreven hoe toevoegen en beheren van VMware vCenter voor herstel na noodgevallen van virtuele VMware-machines naar Azure met Azure Site Recovery.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: ramamill
ms.openlocfilehash: 9694c682f171ab715812b05fed2064c9bbcd36b3
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518079"
---
# <a name="manage-vmware-vcenter-server"></a>Beheren van VMware vCenter-server

Dit artikel worden de verschillende Site Recovery-bewerkingen die kunnen worden uitgevoerd op een VMware vCenter. Controleer of de [vereisten](vmware-physical-azure-support-matrix.md#replicated-machines) voordat u begint.


## <a name="set-up-an-account-for-automatic-discovery"></a>Instellen van een account voor automatische detectie

Site Recovery moet toegang hebben tot de VMware voor de processerver voor het automatisch detecteren van virtuele machines en voor failover en failback van virtuele machines. Maak een account om toegang te krijgen als volgt:

1. Meld u aan bij de configuratie van server-machine.
2. Open de lancering van de cspsconfigtool.exe met behulp van de snelkoppeling op het bureaublad.
3. Klik op **-Account toevoegen** op de **Account beheren** tabblad.

   ![add-account](./media/vmware-azure-manage-vcenter/addaccount.png)
1. Geef de accountgegevens en klikt u op **OK** toe te voegen.  Het account moet hebben de bevoegdheden in de volgende tabel samengevat. 

Het duurt ongeveer 15 minuten voor de accountgegevens van worden gesynchroniseerd met de Site Recovery-service.

### <a name="account-permissions"></a>Accountmachtigingen

|**Taak** | **Account** | **Machtigingen** | **Details**|
|--- | --- | --- | ---|
|**Automatische detectie/migreren (zonder failback)** | U moet ten minste een alleen-lezen-gebruiker | Datacentrumobject –> doorgeven naar onderliggend object, rol=Alleen-lezen | Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven naar onderliggend** -object aan de onderliggende objecten (vSphere-hosts, gegevensopslag, virtuele machines en netwerken).|
|**Replicatie/Failover** | U moet ten minste een alleen-lezen-gebruiker| Datacentrumobject –> doorgeven naar onderliggend object, rol=Alleen-lezen | Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven naar onderliggend** object aan de onderliggende objecten (vSphere-hosts, gegevensopslag, virtuele machines en netwerken).<br/><br/> Dit is handig voor migratiedoeleinden, maar niet voor volledige replicatie, failover en failback.|
|**Failover-replicatie/failback** | We raden u een gebruikersrol (AzureSiteRecoveryRole) maakt met de vereiste machtigingen, en vervolgens de rol toewijzen aan een VMware-gebruiker of groep | Datacentrumobject doorgeven naar onderliggend Object, rol = AzureSiteRecoveryRole<br/><br/> Gegevensopslag –> Ruimte toewijzen, browsen in de gegevensopslag, bestandsbewerkingen op laag niveau, bestand verwijderen, bestanden van virtuele machines bijwerken<br/><br/> Netwerk –> Netwerk toewijzen<br/><br/> Resource –> VM toewijzen aan resourcegroep, uitgeschakelde VM migreren, ingeschakelde VM migreren<br/><br/> Taken –> Taak maken, taak bijwerken<br/><br/> Virtuele machine –> Configuratie<br/><br/> Virtuele machine –> Interactie –> vraag beantwoorden, apparaatverbinding, CD's configureren, diskettes configureren, uitschakelen, inschakelen, VMware-hulpprogramma's installeren<br/><br/> Virtuele machine -> Inventaris –> Maken, registreren, registratie opheffen<br/><br/> Virtuele machine –> Inrichten –> Downloaden van virtuele machine toestaan, toestaan dat bestanden van virtuele machines worden geüpload<br/><br/> Virtuele machine –> Momentopnamen -> Momentopnamen verwijderen | Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.<br/><br/> Om toegang te beperken, wijzen de **geen toegang** rol met de **doorgeven naar onderliggend** -object aan de onderliggende objecten (vSphere-hosts, gegevensopslag, virtuele machines en netwerken).|


## <a name="add-vmware-server-to-the-vault"></a>VMware-server toevoegen aan de kluis

1. Open uw kluis in Azure portal > **Site Recovery-infrastructuur** > **Configuration servers**, en opent u de configuratieserver.
2. Op de **Details** pagina, klikt u op **+ vCenter**.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials"></a>Referenties wijzigen

De referenties waarmee verbinding met de vCenter-server of de ESXi-host als volgt wijzigen:

1. Meld u aan bij de configuratieserver en start de cspsconfigtool.exe vanaf het bureaublad.
2. Klik op **-Account toevoegen** op de **Account beheren** tabblad.

   ![add-account](./media/vmware-azure-manage-vcenter/addaccount.png)
3. Geef de nieuwe accountgegevens en klikt u op **OK** toe te voegen. Het account moet hebben de bevoegdheden die zijn vermeld [hierboven](#account-permissions).
4. Open de kluis op de Azure portal > **Site Recovery-infrastructuur** > **Configuration servers**, en opent u de configuratieserver.
5. In de **Details** pagina, klikt u op **Server vernieuwen**.
6. Nadat de Server vernieuwen-taak is voltooid, selecteert u de vCenter-Server, om te openen van de vCenter **samenvatting** pagina.
7. Selecteer het zojuist toegevoegde account in de **vCenter server/vSphere-hostaccount** veld en klikt u op **opslaan**.

   ![modify-account](./media/vmware-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-server"></a>Een vCenter-server verwijderen

1. Open uw kluis in Azure portal > **Site Recovery-infrastructuur** > **Configuration servers**, en opent u de configuratieserver.
2. Op de **Details** pagina, selecteert u de vCenter-server.
3. Klik op de **verwijderen** knop.

   ![delete-account](./media/vmware-azure-manage-vcenter/delete-vcenter.png)

## <a name="modify-the-vcenter-ip-address-and-port"></a>De vCenter-IP-adres en poort wijzigen

1. Meld u aan bij Azure Portal.
2. Navigeer naar **Recovery Services-kluis** > **Site Recovery-infrastructuur** > **configuratieservers**.
3. Klik op de configuratieserver het vCenter is toegewezen aan.
4. In de **vCenter-servers** sectie, klikt u op het vCenter dat u wilt wijzigen.
5. Bijwerken van de IP-adres en poort van de vCenter-in de desbetreffende velden op de pagina van de vCenter-overzicht, en sla uw wijzigingen op.

   ![add_ip_new_vcenter](media/vmware-azure-manage-vcenter/add-ip.png)

6. Wijzigingen van kracht, wacht u 15 minuten of [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server).

## <a name="migrate-all-protected-virtual-machines-to-a-new-vcenter"></a>Alle beveiligde virtuele machines migreren naar een nieuwe vCenter

Als u wilt migreren alle virtuele machines met de vCenter-nieuwe, Voeg een ander vCenter-account niet. Dit kan leiden tot er zijn dubbele vermeldingen. Alleen het IP-adres van de vCenter-nieuwe bijwerken:

1. Meld u aan bij Azure Portal.
2. Navigeer naar **Recovery Services-kluis** > **Site Recovery-infrastructuur** > **configuratieservers**.
3. Klik op de configuratieserver de oude vCenter is toegewezen aan.
4. In de **vCenter-servers** sectie, klikt u op het vCenter dat u van plan bent om te migreren van.
5. Bijgewerkt op de pagina van de vCenter-overzicht, het IP-adres van de nieuwe vCenter in het veld **vCenter server/vSphere-hostnaam of IP-adres**. Sla uw wijzigingen op.

Als het IP-adres wordt bijgewerkt, kan Site Recovery-onderdelen wordt gestart detectie-informatie van virtuele machines van de nieuwe vCenter ontvangen. Dit heeft geen gevolgen voor de voortdurende replicatie-activiteiten.

## <a name="migrate-few-protected-virtual-machines-to-a-new-vcenter"></a>Aantal beveiligde virtuele machines migreren naar een nieuwe vCenter

> [!NOTE]
> In deze sectie is van toepassing alleen wanneer u migreert enkele van uw beveiligde virtuele machines naar een nieuwe vCenter. Als u wilt dat een nieuwe set van virtuele machines beschermen tegen een nieuwe vCenter [nieuwe details voor vCenter toevoegen aan de configuratieserver](#add-vmware-server-to-the-vault) en beginnen met  **[beveiliging inschakelen](vmware-azure-tutorial.md#enable-replication)**.

Een paar virtuele machines verplaatsen naar een nieuwe vCenter:

1. [De details van de nieuwe vCenter toevoegen aan de configuratieserver](#add-vmware-server-to-the-vault).
2. [Schakel de replicatie van de virtuele machines](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) u van plan bent om te migreren.
3. Voltooi de migratie van de geselecteerde virtuele machines met de vCenter-nieuwe.
4. Nu beveiligen gemigreerde virtuele machines door [de nieuwe vCenter selecteren wanneer u beveiliging inschakelt](vmware-azure-tutorial.md#enable-replication).

> [!TIP]
> Als het aantal virtuele machines gemigreerd **hogere** dat het aantal virtuele machines die zijn opgeslagen in de oude vCenter, het IP-adres van de nieuwe vCenter met behulp van de instructies hier bijwerken. Voor het aantal virtuele machines die worden bewaard op de oude vCenter [replicatie uitschakelen](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure); [nieuwe details voor vCenter toevoegen aan de configuratieserver](#add-vmware-server-to-the-vault), en start  **[beveiliging inschakelen](vmware-azure-tutorial.md#enable-replication)**.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

1. Als beveiligde virtuele machines van één ESXi-host naar een andere worden verplaatst, is dit van invloed op replicatie?

    Nee, dit is niet van invloed op de doorlopende replicatie. Echter [ervoor te zorgen voor het implementeren van de hoofddoelserver met voldoende machtigingen](vmware-azure-reprotect.md#deploy-a-separate-master-target-server)

2. Wat zijn de poortnummers die wordt gebruikt om te voor communicatie tussen vCenter en andere Site Recovery onderdelen?

    De standaardpoort is 443. Configuratieserver wordt toegang tot informatie over de vCenter/vSphere-host via deze poort. Als u deze gegevens bijwerken wilt, klikt u op [hier](#modify-the-vcenter-ip-address-and-port).
