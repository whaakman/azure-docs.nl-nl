---
title: " Beheren van een processerver worden uitgevoerd in Azure (Resource Manager) | Microsoft Docs"
description: Dit artikel wordt beschreven hoe u een failback processerver (Resource Manager) instelt In Azure.
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
ms.workload: storage-backup-recovery
ms.date: 11/23/2017
ms.author: anoopkv
ms.openlocfilehash: 035336efa6be0d00c41baba168eaffd80939cc82
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-a-process-server-running-in-azure-resource-manager"></a>Beheren van een processerver worden uitgevoerd in Azure (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](./site-recovery-vmware-setup-azure-ps-resource-manager.md)
> * [Klassieke](./site-recovery-vmware-setup-azure-ps-classic.md)

Tijdens de failback, is het raadzaam processerver in Azure implementeren als er hoge latentie tussen het virtuele netwerk van Azure en uw on-premises netwerk. Dit artikel wordt beschreven hoe u kunt instellen, configureren en beheren van de processervers worden uitgevoerd in Azure.

> [!NOTE]
> In dit artikel wordt gebruikt als u hebt gebruikt **Resource Manager** als het implementatiemodel voor de virtuele machines tijdens de failover. Als u hebt gebruikt **klassieke** Volg de stappen in als het implementatiemodel [hoe instellen en configureren van een Failback processerver (klassiek)](./site-recovery-vmware-setup-azure-ps-classic.md)

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a>Processerver op Azure implementeren
1. In de kluis > **Site Recovery-infrastructuur** (onder de kop 'Beheren') > **configuratieservers** (onder 'Voor VMware en fysieke Machines' post), selecteer de configuratieserver.
2. Klik in de detailpagina configuratieserver dat wordt geopend op plusteken (+ verwerken server)

  ![Proces Servergalerie toevoegen](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps.png)

3.  Op de **toevoegen processerver** pagina, selecteert u de volgende waarden

  ![Proces server galerie-item toevoegen](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-1.png)
|**Veldnaam**|**Waarde**|
|-|-|
|Kies waar u uw processerver wilt implementeren|Selecteer de waarde **een failback processerver in Azure implementeren** |
|Abonnement|Selecteer het Azure-abonnement waarbij u de virtuele machines failover|
|Resourcegroep|U kunt een resourcegroep om deze processerver implementeren of Kies voor het implementeren van de processerver in een bestaande resourcegroep maken|
|Locatie|Selecteer het Azure-Datacenter waarin de virtuele machines wanneer failover in|
|Azure-netwerk|Selecteer de virtuele Azure-Network(VNet) die de virtuele machines waarbij in een failover uitgevoerd. Als u virtuele machines in meerdere Azure VNets failover, moet u een processerver geïmplementeerd per VNet|

4. Vul in de rest van de eigenschappen voor de processerver

  ![Samenvatting processerver toevoegen](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-2.png)
|**Veldnaam**|**Waarde**|
|-|-|
|Servernaam|De naam van & hostnaam voor de virtuele machine van de proces-server weergeven|
| Gebruikersnaam|De naam van een gebruiker die een beheerder op deze virtuele machine wordt|
|Opslagaccount|Naam van het Opslagaccount waar de virtuele machine virtuele schijf worden geplaatst|
|Subnet|Het subnet van het Azure VNet waarmee de virtuele machine is verbonden|
| IP-adres|IP-adres dat u de processerver wilt vanuit één keer wordt opgestart|
5. Klik op OK om te beginnen met het implementeren van de virtuele machine voor de proces-server.

> [!NOTE]
> Als u deze processerver voor failback gebruiken, moet u bij de lokale configuratie-server registreren.

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>Registreren van de processerver (uitgevoerd in Azure) naar een configuratie-Server (met lokale)

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-the-process-server-to-latest-version"></a>Upgraden van de processerver naar de nieuwste versie.

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-the-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a>De registratie van de processerver (uitgevoerd in Azure) van een configuratie-Server (lokaal worden uitgevoerd)

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
