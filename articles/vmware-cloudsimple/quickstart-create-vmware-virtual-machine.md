---
title: Azure VMware-oplossing van CloudSimple Quick Start-virtuele VMware-machines in azure gebruiken
description: In deze Quick Start leert u hoe u virtuele VMware-machines kunt configureren en gebruiken vanuit het Azure Portal met behulp van de Azure VMware-oplossing door CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/11/2019
ms.topic: quickstart
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: dd4faf6df54d478654c59ffc18bf8c5873d576b9
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816653"
---
# <a name="quickstart-consume-vmware-vms-on-azure"></a>Quickstart: VMware-VM's in Azure gebruiken

Als u een virtuele machine wilt maken in de Azure Portal, kunt u virtuele-machine sjablonen gebruiken die beschikbaar zijn in uw privécloud. Een vCenter-beheerder kan extra sjablonen maken in de privécloud.

## <a name="create-a-vm-template"></a>Een VM-sjabloon maken

Maak eerst een virtuele machine in uw privécloud met behulp van de vCenter-gebruikers interface. Als u een sjabloon wilt maken, volgt u de instructies in het VMware-artikel [een virtuele machine klonen naar een sjabloon in de vSphere](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html)-webclient. Sla de VM-sjabloon op in uw privécloud.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Een virtuele machine maken in de Azure Portal

1. Selecteer **Alle services**.

2. Zoek naar **CloudSimple-virtual machines**.

3. Selecteer **Toevoegen**.

    ![Selecteer toevoegen](media/create-cloudsimple-virtual-machine.png)

4. Voer de volgende informatie over de virtuele machine in en selecteer **volgende: Grootte**.

    ![CloudSimple virtual machine maken-basis beginselen](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Veld | Description |
    | ------------ | ------------- |
    | **Abonnement** | Het Azure-abonnement dat is gekoppeld aan uw privécloud.  |
    | **Resourcegroep** | De resource groep waaraan de virtuele machine wordt toegewezen. U kunt een bestaande groep selecteren of een nieuwe maken. |
    | **Name** | Een naam voor het identificeren van de virtuele machine.  |
    | **Location** | De Azure-regio waarin de virtuele machine wordt gehost.  |
    | **Privécloud** | De privécloud CloudSimple waarin u de virtuele machine wilt maken. |
    | **ResourcePool** | Een toegewezen resource groep voor de virtuele machine. Selecteer een van de beschik bare resource groepen. |
    | **vSphere-sjabloon** | De vSphere-sjabloon voor de virtuele machine.  |
    | **Gebruikersnaam** | De gebruikers naam van de VM-beheerder (voor Windows-sjablonen).|
    | **Wachtwoord** |  Het wacht woord van de VM-beheerder (voor Windows-sjablonen). |
    | **Wachtwoord bevestigen** | Het wacht woord dat in het vorige veld is opgegeven. |

5. Selecteer het aantal kernen en de geheugen capaciteit voor de virtuele machine. Selecteer **beschikbaar maken voor gast besturingssysteem** als u de volledige CPU-virtualisatie wilt weer geven voor het gast besturingssysteem. Toepassingen waarvoor hardwarematige virtualisatie vereist is, kunnen worden uitgevoerd op virtuele machines zonder binaire omzetting of paravirtualisatie. Zie voor meer informatie het VMware-artikel een door <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">VMware ondersteunde virtualisatie beschikbaar</a>maken. Wanneer u klaar bent, selecteert **u volgende: Configuraties**.

    ![CloudSimple virtuele machine maken-grootte](media/create-cloudsimple-virtual-machine-size.png)

6. Configureer netwerk interfaces en schijven zoals beschreven in de volgende tabellen en selecteer vervolgens **controleren + maken**.

    ![CloudSimple virtuele machine maken-configuraties](media/create-cloudsimple-virtual-machine-configurations.png)

    Voor netwerk interfaces selecteert u **netwerk interface toevoegen** en configureert u vervolgens de volgende instellingen:
    
    | Instelling | Description |
    | ------------ | ------------- |
    | **Name** | Voer een naam in om de interface te identificeren.  |
    | **Netwerk** | Selecteer in de lijst met geconfigureerde gedistribueerde poort groepen in uw privécloud vSphere.  |
    | **Adapter** | Selecteer een vSphere-adapter in de lijst met beschik bare typen die voor de virtuele machine zijn geconfigureerd. Zie voor meer informatie het VMware-artikel <a href="https://kb.vmware.com/s/article/1001805" target="_blank">een netwerk adapter voor uw virtuele machine kiezen</a>. |
    | **Inschakelen bij opstarten** | Kies of u de NIC-hardware wilt inschakelen wanneer de virtuele machine wordt opgestart. De standaard instelling is **ingeschakeld**. |

    Selecteer voor schijven de optie **schijf toevoegen** en configureer vervolgens de volgende instellingen:

    | Instelling | Description |
    | ------------ | ------------- |
    | **Name** | Voer een naam in om de schijf aan te duiden.  |
    | **Grootte** | Selecteer een van de beschik bare grootten.  |
    | **SCSI-controller** | Selecteer een SCSI-controller voor de schijf.  |
    | **Penmodus** | In de modus wordt aangegeven hoe de schijf deel uitmaakt van moment opnamen. Kies een van de volgende opties: <br> **Onafhankelijk permanent**: Alle wijzigingen die naar de schijf worden geschreven, worden permanent geschreven.<br> **Onafhankelijk niet-persistent**: Wijzigingen die naar de schijf worden geschreven, worden genegeerd wanneer u de virtuele machine uitschakelt of opnieuw instelt. Met een onafhankelijke, niet-permanente modus kunt u de virtuele machine in dezelfde staat altijd opnieuw opstarten. Raadpleeg de <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">VMware-documentatie</a>voor meer informatie.

7. Nadat de validatie is voltooid, controleert u de instellingen en selecteert u **maken**. Als u wijzigingen wilt aanbrengen, selecteert u de tabbladen bovenaan **of selecteert u vorige: Configuraties**.

    ![De CloudSimple-virtuele machine maken-controleren en maken](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Volgende stappen

* [Een lijst met virtuele CloudSimple-machines weer geven](https://docs.azure.cloudsimple.com/azure-create-vm/#view-list-of-cloudsimple-virtual-machines)
* [Virtuele CloudSimple-machines beheren vanuit Azure](https://docs.azure.cloudsimple.com/azure-manage-vm/)
