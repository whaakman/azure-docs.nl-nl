---
title: Shell-scripts uitvoeren in een Linux-VM op Azure
description: Dit onderwerp wordt beschreven voor het uitvoeren van scripts in een Azure Linux virtuele machine met behulp van de opdracht uitvoeren
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 0e87243b4b6e8362cb840a6510c175d2712b8a1a
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285753"
---
# <a name="run-shell-scripts-in-your-linux-vm-with-run-command"></a>Shellscripts uitvoeren in uw Linux-VM met de opdracht uitvoeren

Opdracht maakt gebruik van de VM-agent om te worden uitgevoerd binnen een virtuele machine van Azure Linux shell-scripts uitvoeren. Deze scripts kunnen worden gebruikt voor algemene machine of Toepassingsbeheer en kunnen worden gebruikt om snel te onderzoeken en VM-netwerk- en problemen oplossen en ophalen van de virtuele machine terug naar een goede status.

## <a name="benefits"></a>Voordelen

Er zijn meerdere opties die kunnen worden gebruikt voor toegang tot uw virtuele machines. Opdracht kunt u scripts uitvoeren op uw virtuele machines op afstand met behulp van de VM-agent. Opdracht kan worden gebruikt via de Azure-portal [REST-API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), of [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand).

Deze functie is nuttig in alle scenario's waarbij u wilt uitvoeren van een script interactietypen een virtuele machines en is een van de enige manieren oplossen en het herstellen van een virtuele machine die niet het RDP of SSH-poort openen als gevolg van onjuiste netwerk of een gebruiker met beheerdersrechten de configuratie.

## <a name="restrictions"></a>Beperkingen

Hieronder vindt u een lijst met beperkingen die aanwezig zijn bij gebruik van de opdracht uitvoeren.

* Uitvoer is beperkt tot de laatste 4096 bytes
* De minimale tijd ongeveer 20 seconden voor een script uitvoeren
* Scripts standaard als gebruiker met verhoogde bevoegdheden op Linux wordt uitgevoerd
* Een script tegelijk kunt uitvoeren
* Scripts die om informatie (interactieve modus vraagt) worden niet ondersteund.
* U kunt een script wordt uitgevoerd niet annuleren.
* De maximale tijdsduur dat een script kunt uitvoeren is 90 minuten, na waarin het time-out wordt

## <a name="azure-cli"></a>Azure-CLI

Hieronder volgt een voorbeeld met behulp van de [az vm-opdracht uitvoeren](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) opdracht een shellscript uitvoeren op een Azure Linux VM.

```azurecli-interactive
az vm run-command invoke -g myResourceGroup -n myVm --command-id RunShellScript --scripts "sudo apt-get update && sudo apt-get install -y nginx"
```

> [!NOTE]
> Als u wilt uitvoeren van opdrachten als een andere gebruiker, kunt u `sudo -u` om op te geven van een gebruikersaccount te gebruiken.

## <a name="azure-portal"></a>Azure Portal

Navigeer naar een virtuele machine in [Azure](https://portal.azure.com) en selecteer **opdracht uitvoeren** onder **OPERATIONS**. Krijgt u een lijst met beschikbare opdrachten uit te voeren op de virtuele machine.

![Opdrachtlijst uitvoeren](./media/run-command/run-command-list.png)

Kies een andere opdracht om uit te voeren. Sommige van de opdrachten kan optioneel of vereist invoerparameters hebben. Voor deze opdrachten worden de parameters weergegeven als tekstvelden voor u de invoerwaarden opgeven. Voor elke opdracht kunt u het script dat wordt uitgevoerd door het uitbreiden van weergeven **script weergeven**. **RunShellScript** verschilt van de andere opdrachten zoals kunt u uw eigen aangepaste scripts opgeven. 

> [!NOTE]
> De ingebouwde opdrachten kunnen niet worden bewerkt.

Zodra de opdracht is gekozen, klikt u op **uitvoeren** het script uit te voeren. Het script wordt uitgevoerd en als u klaar is, retourneert de uitvoer en eventuele fouten in het uitvoervenster. De volgende schermafbeelding ziet u een voorbeeld van uitvoer uitvoeren van de **ifconfig** opdracht.

![Uitvoer van een opdracht uitvoeren](./media/run-command/run-command-script-output.png)

## <a name="available-commands"></a>Beschikbare opdrachten

Deze tabel bevat de lijst met opdrachten die beschikbaar zijn voor virtuele Linux-machines. De **RunShellScript** opdracht kan worden gebruikt voor een aangepast script die u wilt uitvoeren.

|**Naam**|**Beschrijving**|
|---|---|
|**RunShellScript**|Een Linux-shell-script wordt uitgevoerd.|
|**ifconfig**| Opvragen van de configuratie van alle netwerkinterfaces.|

## <a name="limiting-access-to-run-command"></a>Beperken van toegang tot de opdracht uitvoeren

Lijst van de opdrachten uitvoeren of worden de details van een opdracht moet de `Microsoft.Compute/locations/runCommands/read` toestemming hebben, die de ingebouwde [lezer](../../role-based-access-control/built-in-roles.md#reader) rol en hoger.

Het uitvoeren van een opdracht moet de `Microsoft.Compute/virtualMachines/runCommand/action` toestemming hebben, die de [Inzender](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol en hoger.

U kunt een van de [ingebouwde](../../role-based-access-control/built-in-roles.md) rollen of maak een [aangepaste](../../role-based-access-control/custom-roles.md) rol opdracht uitvoeren te gebruiken.

## <a name="next-steps"></a>Volgende stappen

Zie, [scripts uitvoeren in uw Linux-VM](run-scripts-in-vm.md) voor meer informatie over andere manieren om op afstand opdrachten en scripts uitvoeren in uw virtuele machine.
