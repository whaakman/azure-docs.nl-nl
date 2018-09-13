---
title: VM's importeren uit een andere lab in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het importeren van virtuele machines van een andere lab in de huidige lab in Azure DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 8460f09e-482f-48ba-a57a-c95fe8afa001
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: spelluru
ms.openlocfilehash: 4585d151e286917c67586a02539a10ade32bdd4c
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646178"
---
# <a name="import-vms-from-another-lab-in-azure-devtest-labs"></a>VM's importeren uit een andere lab in Azure DevTest Labs
De service Azure DevTest Labs aanzienlijk wordt verbeterd beheer van virtuele machines (VM's) voor de ontwikkeling en testactiviteiten. Hiermee kunt u een virtuele machine van een lab verplaatsen naar een andere als het team of wijzigen van de vereisten voor de infrastructuur. Hier volgen enkele algemene scenario's denkbaar waarin om dit te doen: 

- Een persoon in het team wordt verplaatst naar een andere groep binnen de onderneming en wil development VM's uitvoeren met het nieuwe team lab.
- De groep heeft de abonnement quotum bereikt en wil om op te splitsen up teams in meerdere abonnementen.
- Het bedrijf wil verplaatsen naar een Express-Route (of enige andere nieuwe netwerktopologie) en het team wil verplaatsen van virtuele machines om deze nieuwe infrastructuur te gebruiken.

## <a name="solution-and-constraints"></a>Oplossing en beperkingen
Azure DevTest Labs kunt een lab-eigenaar voor het importeren van virtuele machines in een testomgeving bron naar een bestemming lab. De eigenaar van het testlab kunt desgewenst een nieuwe naam voor de doel-VM in het proces. Het importproces bevat de afhankelijkheden, zoals schijven, schema's, netwerkinstellingen, enzovoort. Het duurt enige tijd en wordt beïnvloed door het aantal of de grootte van de schijven die zijn gekoppeld aan de bronmachine en de weg naar de bestemming (bijvoorbeeld VS-Oost regio naar regio Zuidoost-Azië). Als tijdens het importeren voltooid is, blijft de bron-VM afsluiten en de nieuwe op een wordt uitgevoerd in de doel-testomgeving.

Er zijn twee belangrijke beperkingen rekening mee moet houden bij het plannen voor het importeren van virtuele machines naar een andere lab:

- Importeren van virtuele machines tussen abonnementen en over meerdere regio's worden ondersteund, maar de abonnementen moeten zijn gekoppeld aan dezelfde Azure Active Directory-tenant.
- Virtuele machines moet niet in een claimbare staat in de bron-testomgeving.

Als u een virtuele machine importeren uit een testomgeving naar een andere, moet u bovendien de eigenaar van de virtuele machine in de bron-lab en de eigenaar van het testlab in de doel-testomgeving.

## <a name="steps-to-import-a-vm-from-another-lab"></a>Stappen voor het importeren van een virtuele machine uit een andere lab
U kunt op dit moment een virtuele machine in een testomgeving importeren in een ander alleen met behulp van Azure PowerShell en REST-API.

### <a name="use-powershell"></a>PowerShell gebruiken
Download het PowerShell-script bestand ImportVirtualMachines.ps1 van [Azure DevTest Lab Git-opslagplaats](https://github.com/Azure/azure-devtestlab/tree/master/Scripts/ImportVirtualMachines) naar uw lokale schijf. 

#### <a name="import-a-single-vm"></a>Een enkele virtuele machine importeren
Voer het script ImportVirtualMachines.ps1 voor het importeren van een enkele virtuele machine uit een bron-lab in een lab bestemming. U kunt een nieuwe naam voor de virtuele machine die wordt gekopieerd met behulp van de paramer DestinationVirtualMachineName opgeven. 

```powershell
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source VM>" `
                            -SourceDevTestLabName "<Name of the lab that contains the source VM>" `
                            -SourceVirtualMachineName "<Name of the machine. Optional. If not specified, all VMs are copied>" `
                            -DestinationSubscriptionId "<ID of the target/destination subscription>" `
                            -DestinationDevTestLabName "<Name of the lab to which the VM is copied>" `
                            -DestinationVirtualMachineName "<New name for the VM. Optional>"
```


#### <a name="importing-all-vms"></a>Importeren van alle virtuele machines
Wanneer u het script ImportVirtualMachines.ps1 uitvoert als u een virtuele machine in het lab bron niet opgeeft, importeert het script alle virtuele machines in de bron-lab in de doel-testomgeving. 

```powershell
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source VM>" `
                            -SourceDevTestLabName "<Name of the lab that contains the source VM>" `
                            -DestinationSubscriptionId "<ID of the target/destination subscription>" `
                            -DestinationDevTestLabName "<Name of the lab to which the VMs are copied>"
```

### <a name="use-rest-api"></a>REST API gebruiken
Aanroepen van de REST-API op basis van de doel-/ doel-lab en doorgeven in het lab bron, het abonnement en de VM-informatie als parameters zoals weergegeven in het volgende voorbeeld: 

```json
POST https://management.azure.com/subscriptions/<ID of the target/destination subscription>/resourceGroups/<Name of the resource group that contains the destination lab>/providers/Microsoft.DevTestLab/labs/<Name of the lab to which the VMs are copied>/ImportVirtualMachine?api-version=2017-04-26-preview
{
   sourceVirtualMachineResourceId: "/subscriptions/<ID of the subscription that contains the source VM>/resourcegroups/<Name of the resource group that contains the source lab>/providers/microsoft.devtestlab/labs/<Name of the lab that contains the source VM>/virtualmachines/MyVm",
   destinationVirtualMachineName: "MyVmNew"
}
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over grootte wijzigen van een virtuele machine [vergroten of verkleinen van een virtuele machine](devtest-lab-resize-vm.md).
- Zie voor informatie over een virtuele machine opnieuw te implementeren, [een virtuele machine opnieuw implementeren](devtest-lab-redeploy-vm.md).


