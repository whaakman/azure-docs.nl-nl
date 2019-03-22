---
title: Virtuele machines importeren uit een andere lab in Azure DevTest Labs | Microsoft Docs
description: Leer hoe u virtuele machines van een andere lab in de huidige testomgeving importeren.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: 8b2eee0bfd32b58cd751f8bf70aff1d4f460a353
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58340137"
---
# <a name="import-virtual-machines-from-another-lab-in-azure-devtest-labs"></a>Virtuele machines importeren uit een andere lab in Azure DevTest Labs
In dit artikel bevat informatie over hoe u virtuele machines van een andere lab in uw testomgeving importeren. 

## <a name="scenarios"></a>Scenario's
Hier volgen enkele scenario's waarin u wilt importeren van virtuele machines van een lab naar een andere lab: 

- Een persoon in het team wordt verplaatst naar een andere groep binnen de onderneming en wil uitvoeren van het bureaublad van ontwikkelaars bij het nieuwe team Azure DevTest Labs.
- De groep heeft bereikt een [abonnementsniveau quotum](../azure-subscription-service-limits.md) en wil de teams in een paar abonnementen opsplitsen
- Het bedrijf wordt verplaatst naar Express Route (of enige andere nieuwe netwerktopologie) en het team wil overstappen van de virtuele Machines om deze nieuwe infrastructuur te gebruiken

## <a name="solution-and-constraints"></a>Oplossing en beperkingen
Deze functie kunt u voor het importeren van virtuele machines in een testomgeving (bron) naar een andere lab (doel). U kunt eventueel een nieuwe naam voor de doel-VM in het proces geven. Het importproces bevat de afhankelijkheden, zoals schijven, schema's en netwerkinstellingen.

Het proces duurt enige tijd en wordt beïnvloed door de volgende factoren:

- Getal of de grootte van de schijven die zijn gekoppeld aan de bron-VM (omdat het is een kopieerbewerking en niet een verplaatsingsbewerking) 
- De afstand naar de bestemming (bijvoorbeeld VS-Oost regio aan Zuidoost-Azië).  

Zodra het proces voltooid is, blijft de bron virtuele Machine afsluiten en de nieuwe op een wordt uitgevoerd in de doel-testomgeving.

Er zijn twee belangrijke beperkingen rekening mee moet houden bij het plannen voor het importeren van virtuele machines van een lab in met een andere lab:

- Invoer van de virtuele Machine tussen abonnementen en regio's worden ondersteund, maar de abonnementen moeten zijn gekoppeld aan dezelfde Azure Active Directory-tenant.
- Er mag geen virtuele Machines in een claimbare staat in de bron-testomgeving.
- U kunt de eigenaar van de virtuele machine in de bron-lab en de eigenaar van het testlab in de doel-testomgeving.
- Deze functie wordt momenteel ondersteund alleen via Powershell en REST-API.

## <a name="use-powershell"></a>PowerShell gebruiken
ImportVirtualMachines.ps1 bestand downloaden via de [GitHub](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/ImportVirtualMachines/ImportVirtualMachines.ps1). U kunt het script gebruiken voor het importeren van een enkele virtuele machine of alle virtuele machines in het lab bron in het lab bestemming. 

### <a name="use-powershell-to-import-a-single-vm"></a>PowerShell gebruiken voor het importeren van een enkele virtuele machine
Dit powershell-script wordt uitgevoerd, moet identificeren van de bron-VM en het doel-lab en eventueel een nieuwe naam moet worden gebruikt voor de doelmachine opgeven:

```powershell 
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source lab>" `
                            -SourceDevTestLabName "<Name of the source lab>" `
                            -SourceVirtualMachineName "<Name of the VM to be imported from the source lab> " `
                            -DestinationSubscriptionId "<ID of the subscription that contians the destination lab>" `
                            -DestinationDevTestLabName "<Name of the destination lab>" `
                            -DestinationVirtualMachineName "<Optional: specify a new name for the imported VM in the destination lab>"
```

### <a name="use-powershell-to-import-all-vms-in-the-source-lab"></a>PowerShell gebruiken voor het importeren van alle virtuele machines in de bron-lab
Als de virtuele bronmachine niet is opgegeven, worden alle virtuele machines in de DevTest Labs automatisch geïmporteerd in het script.  Bijvoorbeeld:
 
```powershell
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source lab>" `
                            -SourceDevTestLabName "<Name of the source lab>" `
                            -DestinationSubscriptionId "<ID of the subscription that contians the destination lab>" `
                            -DestinationDevTestLabName "<Name of the destination lab>"
```

## <a name="use-http-rest-to-import-a-vm"></a>HTTP REST gebruiken voor het importeren van een virtuele machine
De REST-aanroep is eenvoudig. Geeft u voldoende gegevens om de bron- en -resources te identificeren. Houd er rekening mee dat de bewerking uitgevoerd via de doelresource van de testomgeving wordt.

```REST
POST https://management.azure.com/subscriptions/<DestinationSubscriptionID>/resourceGroups/<DestinationResourceGroup>/providers/Microsoft.DevTestLab/labs/<DestinationLab>/ImportVirtualMachine?api-version=2017-04-26-preview
{
   sourceVirtualMachineResourceId: "/subscriptions/<SourceSubscriptionID>/resourcegroups/<SourceResourceGroup>/providers/microsoft.devtestlab/labs/<SourceLab>/virtualmachines/<NameofVMTobeImported>",
   destinationVirtualMachineName: "<NewNameForImportedVM>"
}
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen: 

- [Beleidsregels op te stellen voor een lab](devtest-lab-get-started-with-lab-policies.md)
- [Veelgestelde vragen](devtest-lab-faq.md)
