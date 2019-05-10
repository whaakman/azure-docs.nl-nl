---
title: Azure VMware-oplossing door CloudSimple Quickstart - virtuele VMware-machines op Azure gebruiken
description: Meer informatie over het configureren en gebruiken van VMware-VM's vanuit Azure portal met behulp van Azure VMware-oplossing door CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/11/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c0bb8d7a5a1ea30b704b44c9337cd28043597ff7
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65209524"
---
# <a name="quickstart---consume-vmware-vms-on-azure"></a>Snelstart: virtuele VMware-machines op Azure gebruiken

Voor het maken van een virtuele machine in Azure portal, gebruikt u sjablonen voor virtuele machines die uw beheerder CloudSimple heeft ingeschakeld voor uw abonnement. Deze VM-sjablonen zijn gevonden op de VMware-infrastructuur.

## <a name="cloudsimple-vm-creation-on-azure-requires-a-vm-template"></a>CloudSimple VM wordt gemaakt op Azure vereist dat een VM-sjabloon

Een virtuele machine maken in uw privécloud van de vCenter-gebruikersinterface. Volg de instructies in voor het maken van een sjabloon, [klonen van een virtuele Machine naar een sjabloon in de vSphere-webclient](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). De VM-sjabloon op de vCenter-privécloud Store.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Een virtuele machine maken in Azure portal

1. Selecteer **Alle services**.

2. Zoeken naar **CloudSimple virtuele Machines**.

3. Klik op **Toevoegen**.

    ![CloudSimple virtuele machine maken](media/create-cloudsimple-virtual-machine.png)

4. Voer basisinformatie Klik **Next: grootte**.

    ![Maak CloudSimple virtuele machine - basisbeginselen](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Veld | Description |
    | ------------ | ------------- |
    | Abonnement | Azure-abonnement dat is gekoppeld aan uw Privécloud.  |
    | Resourcegroep | De resourcegroep waaraan de virtuele machine wordt toegewezen. U kunt een bestaande groep selecteren of een nieuwe maken. |
    | Name | Unieke naam in voor de virtuele machine.  |
    | Locatie | Azure-regio waarin deze virtuele machine wordt gehost.  |
    | Private Cloud | CloudSimple Privécloud waar u wilt maken van de virtuele machine. |
    | Resourcegroep | Toegewezen resourcegroep voor de virtuele machine. Selecteer in de beschikbare resourcegroepen. |
    | vSphere-sjabloon | vSphere-sjabloon voor de virtuele machine.  |
    | Gebruikersnaam | Gebruikersnaam van de beheerder van de virtuele machine (voor Windows-sjablonen)|
    | Wachtwoord |  Wachtwoord voor de beheerder van de virtuele machine (voor Windows-sjablonen). |
    | Wachtwoord bevestigen | Bevestig het wachtwoord |

5. Selecteer het aantal kernen en capaciteit van het geheugen voor de virtuele machine en klik op **volgende configuraties:**. Selecteer het selectievakje in als u een volledige CPU-virtualisatie met bij het gastbesturingssysteem beschikbaar wilt maken. Toepassingen waarvoor hardware-virtualisatie kunnen uitvoeren op virtuele machines zonder binaire vertaling of paravirtualisatie. Zie voor meer informatie het artikel VMware <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">blootstellen VMware Hardware ondersteunde virtualisatie</a>.

    ![CloudSimple VM - grootte maken](media/create-cloudsimple-virtual-machine-size.png)

6. Configureren van netwerkinterfaces en schijven, zoals beschreven in de volgende tabellen en klik op **revisie + maken**.

    ![CloudSimple virtuele machine - configuraties maken](media/create-cloudsimple-virtual-machine-configurations.png)

    Voor netwerkinterfaces, klikt u op **netwerkinterface toevoegen** en configureer de volgende instellingen.
    
    | Besturingselement | Description |
    | ------------ | ------------- |
    | Name | Voer een unieke naam voor de interface.  |
    | Netwerk | Selecteer in de lijst met geconfigureerde gedistribueerde poortgroep in uw Privécloud vSphere.  |
    | Netwerkadapter | Selecteer een vSphere-adapter uit de lijst met beschikbare typen die zijn geconfigureerd voor de virtuele machine. Zie voor meer informatie, VMware knowledge base-artikel <a href="https://kb.vmware.com/s/article/1001805" target="_blank">kiezen van een netwerkadapter voor uw virtuele machine</a>. |
    | Tijdens het opstarten inschakelen | Kies of u wilt de NIC-hardware in te schakelen wanneer de virtuele machine wordt opgestart. De standaardwaarde is **inschakelen**. |

    Voor schijven, klikt u op **toevoegen schijf** en configureer de volgende instellingen.

    | Item | Description | 
    | ------------ | ------------- | 
    | Name | Voer een unieke naam voor de schijf.  | 
    | Grootte | Selecteer een van de beschikbare grootten.  | 
    | SCSI-Controller | Selecteer een SCSI-controller voor de schijf.  |
    | modus | Bepaalt hoe de schijf maakt deel uit van momentopnamen. Kies een van de volgende opties: <br> -Onafhankelijke permanente: Alle gegevens die naar de schijf geschreven permanent wordt geschreven.<br> -Niet-permanente onafhankelijke: Wijzigingen die zijn geschreven naar de schijf worden verwijderd wanneer u uitschakelen of opnieuw instellen van de virtuele machine.  Onafhankelijke niet-permanente modus kunt u de virtuele machine wordt altijd opnieuw opgestart in dezelfde staat. Zie voor meer informatie de <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">VMware-documentatie</a>.

7. Nadat de validatie is voltooid, Controleer de instellingen en klik op **maken**. Eventuele wijzigingen wilt aanbrengen, klikt u op de tabbladen aan de bovenkant of klik op.

    ![CloudSimple virtuele machine maken - controleren](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Volgende stappen

* [Lijst met CloudSimple virtuele machines weergeven](https://docs.azure.cloudsimple.com/azure-create-vm/#view-list-of-cloudsimple-virtual-machines)
* [CloudSimple virtuele machine van Azure beheren](https://docs.azure.cloudsimple.com/azure-manage-vm/)
