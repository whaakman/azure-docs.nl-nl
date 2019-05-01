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
ms.openlocfilehash: 58edadb553730b646f23f4981d6cbf1bdbfe76d5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577718"
---
# <a name="quickstart---consume-vmware-vms-on-azure"></a>Snelstart: virtuele VMware-machines op Azure gebruiken

Voor het maken van een virtuele machine in Azure portal, gebruikt u sjablonen voor virtuele machines die uw beheerder CloudSimple heeft ingeschakeld voor uw abonnement. Deze VM-sjablonen zijn gevonden op de VMware-infrastructuur.

## <a name="cloudsimple-vm-creation-on-azure-requires-a-vm-template"></a>CloudSimple VM wordt gemaakt op Azure vereist dat een VM-sjabloon

Een virtuele machine maken in uw privécloud van de vCenter-gebruikersinterface. Volg de instructies in voor het maken van een sjabloon, [klonen van een virtuele Machine naar een sjabloon in de vSphere-webclient](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). De VM-sjabloon op de vCenter-privécloud Store.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Een virtuele machine maken in Azure portal

1. Klik in het menu links op **+** of **een Resource maken**.

2. Klik in het menu links op **Compute**, en klik vervolgens op **CloudSimple virtuele Machine**.

3. Klik op **bevestigen** om te controleren of dat u wilt maken van een nieuwe virtuele machine.

4. De basisconfiguratie hebt ingesteld zoals beschreven in de volgende tabel en klik vervolgens op **volgende: Grootte**.

    | Veld | Description |
    | ------------ | ------------- |
    | Abonnement | Azure-abonnement dat is gekoppeld aan de implementatie van uw privécloud.  |
    | Resourcegroep | Implementatiegroep waaraan u de virtuele machine wordt toegewezen. U kunt een bestaande groep selecteren of een nieuwe maken. |
    | Name | Unieke naam in voor de virtuele machine.  |
    | Locatie | Azure-regio waarin deze virtuele machine wordt gehost.  |
    | Resourcegroep | Fysieke resources voor de virtuele machine. Selecteer in de beschikbare resourcegroepen. |
    | vSphere-sjabloon | Het type sjabloon dat besturingssysteem voor de virtuele machine.  |
    | Gebruikersnaam | De gebruikersnaam van de beheerder van de virtuele machine. |
    | Wachtwoord van wachtwoord bevestigen | Wachtwoord voor de beheerder van de virtuele machine.  |

5. Selecteer het aantal kernen en capaciteit van het geheugen voor de virtuele machine.

6. (Optioneel) Als u weergeven van CPU-virtualisatie met volledige bij het gastbesturingssysteem wilt, selecteert u de **beschikbaar maakt voor Guest OS** selectievakje.
Deze selectie kan toepassingen die hardwarevirtualisatie uit te voeren op virtuele machines zonder binaire vertaling of paravirtualisatie vereisen. Zie voor meer informatie het artikel VMware [blootstellen VMware Hardware ondersteunde virtualisatie](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html).

7. Klik op **Next: Configuratie**.

8. Configureer netwerkinterfaces en schijven, zoals beschreven in de volgende tabellen.

    Voor netwerkinterfaces, klikt u op **netwerkinterface toevoegen** en configureer de volgende instellingen.

    | Beheer | Description |
    | ------------ | ------------- |
    | Name | Voer een unieke naam voor de interface.  |
    | Netwerk | Selecteer in de lijst met geconfigureerde netwerken in uw Privécloud vSphere.  |
    | Netwerkadapter | Selecteer een vSphere-adapter uit de lijst met beschikbare typen die zijn geconfigureerd voor de virtuele machine. Zie voor meer informatie, VMware knowledge base-artikel [kiezen van een netwerkadapter voor uw virtuele machine](https://kb.vmware.com/s/article/1001805). |
    | Tijdens het opstarten inschakelen | Kies of u wilt de NIC-hardware in te schakelen wanneer de virtuele machine wordt opgestart. De standaardwaarde is **inschakelen**. |

    Voor schijven, klikt u op **toevoegen schijf** en configureer de volgende instellingen.

    | Item | Description |
    | ------------ | ------------- |
    | Name | Voer een unieke naam voor de schijf.  |
    | Grootte | Selecteer een van de beschikbare grootten.  |
    | SCSI-Controller | Selecteer een SCSI-controller. De beschikbare controllers verschillen voor verschillende besturingssystemen worden ondersteund.  |
    | Modus | Bepaalt hoe de schijf maakt deel uit van momentopnamen. Kies een van de volgende opties: <br> -Onafhankelijke permanente: Alle gegevens die naar de schijf geschreven permanent wordt geschreven.<br> -Niet-permanente onafhankelijke: Wijzigingen die zijn geschreven naar de schijf worden verwijderd wanneer u uitschakelen of opnieuw instellen van de virtuele machine.  Onafhankelijke niet-permanente modus kunt u de virtuele machine wordt altijd opnieuw opgestart in dezelfde staat. Zie voor meer informatie de [VMware-documentatie.](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html)

9. Controleer de instellingen. Eventuele wijzigingen wilt aanbrengen, klikt u op de tabbladen aan de bovenkant.

10. Klik op **maken** en sla de instellingen op de virtuele machine maken.

## <a name="next-steps"></a>Volgende stappen

* [Lijst met CloudSimple virtuele machines weergeven](https://docs.azure.cloudsimple.com/azurelistvms/)
* [CloudSimple virtuele machine van Azure beheren](https://docs.azure.cloudsimple.com/azureoverviewpage/)