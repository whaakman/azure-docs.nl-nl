---
title: Azure-oplossing VMware tijdens CloudSimple Quick Start - VMware-VM's op Azure gebruiken
description: In deze snelstartgids leert u hoe u voor het configureren en gebruiken van VMware-VM's vanuit de Azure-portal met behulp van Azure VMware-oplossing door CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/11/2019
ms.topic: quickstart
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: b430efbc931d72de4b095a7eac4c1e7ca496b1b9
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393499"
---
# <a name="quickstart-consume-vmware-vms-on-azure"></a>Quickstart: VMware-VM's in Azure gebruiken

Voor het maken van een virtuele machine in Azure portal, kunt u sjablonen voor virtuele machines beschikbaar op de vCenter-privécloud. Een vCenter-beheerder kunt u extra sjablonen maken in de privécloud.

## <a name="create-a-vm-template"></a>Een VM-sjabloon maken

Maak eerst een virtuele machine in uw persoonlijke cloud met behulp van de vCenter-gebruikersinterface. Voor het maken van een sjabloon, volg de instructies in het artikel VMware [klonen van een virtuele Machine naar een sjabloon in de vSphere-webclient](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). De VM-sjabloon op de vCenter-privécloud Store.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Een virtuele machine maken in Azure portal

1. Selecteer **Alle services**.

2. Zoeken naar **CloudSimple virtuele Machines**.

3. Selecteer **Toevoegen**.

    ![Selecteer toevoegen](media/create-cloudsimple-virtual-machine.png)

4. Voer de volgende informatie over de virtuele machine en selecteer vervolgens **volgende: Grootte**.

    ![Maak CloudSimple virtuele Machine - basisbeginselen](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Veld | Description |
    | ------------ | ------------- |
    | **Abonnement** | De Azure-abonnement dat is gekoppeld aan uw privécloud.  |
    | **Resourcegroep** | De resourcegroep waaraan de virtuele machine wordt toegewezen. U kunt een bestaande groep selecteren of een nieuwe maken. |
    | **Naam** | Een unieke naam voor de virtuele machine.  |
    | **Locatie** | De Azure-regio waarin de virtuele machine wordt gehost.  |
    | **Private Cloud** | De CloudSimple privécloud waarin u wilt maken van de virtuele machine. |
    | **ResourcePool** | Een toegewezen resourcegroep voor de virtuele machine. Selecteer in de beschikbare resourcegroepen. |
    | **vSphere-sjabloon** | De vSphere-sjabloon voor de virtuele machine.  |
    | **Gebruikersnaam** | De gebruikersnaam van de VM-beheer (voor Windows-sjablonen).|
    | **Wachtwoord** |  Het wachtwoord van de VM-beheer (voor Windows-sjablonen). |
    | **Wachtwoord bevestigen** | Het wachtwoord dat is opgegeven in het vorige veld. |

5. Selecteer het aantal kernen en de capaciteit van het geheugen voor de virtuele machine. Selecteer **beschikbaar maakt voor Guest OS** als u wilt weergeven van de volledige CPU-virtualisatie op de Gast-besturingssysteem. Toepassingen waarvoor hardware-virtualisatie kunnen uitvoeren op virtuele machines zonder binaire vertaling of paravirtualisatie. Zie voor meer informatie het artikel VMware <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">blootstellen VMware Hardware ondersteunde virtualisatie</a>. Wanneer u klaar bent, selecteert u **volgende: Configuraties**.

    ![CloudSimple VM - grootte maken](media/create-cloudsimple-virtual-machine-size.png)

6. Configureren van netwerkinterfaces en schijven, zoals beschreven in de volgende tabellen en selecteer vervolgens **revisie + maken**.

    ![Virtuele Machine voor CloudSimple - configuraties maken](media/create-cloudsimple-virtual-machine-configurations.png)

    Selecteer voor netwerkinterfaces, **netwerkinterface toevoegen** en configureer vervolgens de volgende instellingen:
    
    | Instelling | Description |
    | ------------ | ------------- |
    | **Naam** | Voer een unieke naam voor de interface.  |
    | **Netwerk** | Selecteer in de lijst met geconfigureerde gedistribueerde poortgroepen in uw privécloud vSphere.  |
    | **Adapter** | Selecteer een vSphere-adapter uit de lijst met beschikbare typen die zijn geconfigureerd voor de virtuele machine. Zie voor meer informatie het artikel VMware <a href="https://kb.vmware.com/s/article/1001805" target="_blank">kiezen van een netwerkadapter voor uw virtuele machine</a>. |
    | **Tijdens het opstarten inschakelen** | Kies of u wilt de NIC-hardware in te schakelen wanneer de virtuele machine wordt opgestart. De standaardwaarde is **inschakelen**. |

    Voor schijven, selecteert u **toevoegen schijf** en configureer vervolgens de volgende instellingen:

    | Instelling | Description |
    | ------------ | ------------- |
    | **Naam** | Voer een unieke naam voor de schijf.  |
    | **Grootte** | Selecteer een van de beschikbare grootten.  |
    | **SCSI-Controller** | Selecteer een SCSI-controller voor de schijf.  |
    | **modus** | De modus geeft aan hoe de schijf maakt deel uit van momentopnamen. Kies een van de volgende opties: <br> **Onafhankelijke permanente**: Alle wijzigingen die zijn geschreven naar de schijf geschreven definitief.<br> **Niet-permanente onafhankelijke**: Wijzigingen die zijn geschreven naar de schijf worden verwijderd wanneer u uitschakelen of opnieuw instellen van de virtuele machine. Onafhankelijke niet-permanente modus kunt u de virtuele machine wordt altijd opnieuw opgestart in dezelfde staat. Zie voor meer informatie de <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">VMware-documentatie</a>.

7. Nadat de validatie is voltooid, Controleer de instellingen en selecteer **maken**. Als u wilt wijzigen, selecteert u de tabbladen boven of selecteer **vorige: Configuraties**.

    ![Virtuele Machine voor CloudSimple - beoordeling maken + maken](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Volgende stappen

* [Lijst met CloudSimple virtuele machines weergeven](https://docs.azure.cloudsimple.com/azure-create-vm/#view-list-of-cloudsimple-virtual-machines)
* [CloudSimple virtuele machines van Azure beheren](https://docs.azure.cloudsimple.com/azure-manage-vm/)
