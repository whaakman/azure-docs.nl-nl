---
title: Shell-scripts uitvoeren in een Linux-VM op Azure
description: In dit onderwerp wordt beschreven hoe u uitvoeren van scripts in een virtuele Azure Linux-machine met behulp van de opdracht uitvoeren
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 10/25/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: e865d4e9cbad2c2064d961bc6e407440ce8556fc
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158802"
---
# <a name="run-shell-scripts-in-your-linux-vm-with-run-command"></a>Shell-scripts uitvoeren in uw Linux-VM met de opdracht uitvoeren

Opdracht wordt de VM-agent voor het uitvoeren van shell-scripts in een Azure Linux VM uitvoeren. Deze scripts voor algemene machine of beheer van toepassingen kunnen worden gebruikt en kunnen worden gebruikt om snel opsporen en VM-netwerk- en problemen oplossen en ophalen van de virtuele machine naar een goede staat verkeren.

## <a name="benefits"></a>Voordelen

Er zijn meerdere opties die kunnen worden gebruikt voor toegang tot uw virtuele machines. Voer de opdracht kunt u scripts uitvoeren op uw virtuele machines die op afstand met behulp van de VM-agent. Voer de opdracht kan worden gebruikt via Azure portal, [REST-API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), of [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) voor virtuele Linux-machines.

Deze mogelijkheid is handig in alle scenario's waarin u wilt uitvoeren van een script in een virtuele machine en is een van de enige manieren om problemen op te herstellen van een virtuele machine die beschikt niet over de RDP of SSH-poort openen vanwege een onjuiste netwerk of de gebruiker met beheerdersrechten de configuratie.

## <a name="restrictions"></a>Beperkingen

Hieronder vindt u een lijst met beperkingen die aanwezig zijn bij het gebruik van de opdracht uitvoeren.

* Uitvoer is beperkt tot de laatste 4096 bytes
* De minimale tijd ongeveer 20 seconden voor een script uitvoeren
* Scripts standaard als gebruiker met verhoogde bevoegdheid op Linux wordt uitgevoerd
* Een script tegelijk kan worden uitgevoerd
* Scripts die voor meer informatie (interactieve modus vragen) worden niet ondersteund.
* U kunt een script uit te voeren niet annuleren
* De maximale tijd die kan worden uitgevoerd door een script is 90 minuten, na waarin het time-out wordt
* Uitgaande connectiviteit van de virtuele machine is vereist om de resultaten van het script te retourneren.

> [!NOTE]
> De opdracht uitvoeren vereist om correct te functioneren, verbinding (poort 443) met Azure openbare IP-adressen. Als de extensie heeft geen toegang tot deze eindpunten, kunnen de scripts worden uitgevoerd, maar niet de resultaten worden geretourneerd. Als u verkeer op de virtuele machine blokkeert, kunt u [servicetags](../../virtual-network/security-overview.md#service-tags) waarmee verkeer naar Azure openbare IP-adressen met behulp van de `AzureCloud` tag.

## <a name="azure-cli"></a>Azure-CLI

Hier volgt een voorbeeld met behulp van de [az vm-opdracht uitvoeren](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) opdracht een shell-script uitvoeren op een virtuele Azure Linux-machine.

```azurecli-interactive
az vm run-command invoke -g myResourceGroup -n myVm --command-id RunShellScript --scripts "sudo apt-get update && sudo apt-get install -y nginx"
```

> [!NOTE]
> Als u wilt uitvoeren van opdrachten als een andere gebruiker, kunt u `sudo -u` om op te geven van een gebruikersaccount te gebruiken.

## <a name="azure-portal"></a>Azure Portal

Navigeer naar een virtuele machine in [Azure](https://portal.azure.com) en selecteer **RunCommand-** onder **OPERATIONS**. Krijgt u een lijst van de beschikbare opdrachten uit te voeren op de virtuele machine.

![Lijst met opdrachten uitvoeren](./media/run-command/run-command-list.png)

Kies een opdracht uit te voeren. Enkele van de opdrachten mogelijk optioneel of vereiste invoerparameters. Voor deze opdrachten worden de parameters weergegeven als tekstvelden voor u de ingevoerde waarden op te geven. Voor elke opdracht die u kunt het script dat wordt uitgevoerd door het uitbreiden van weergeven **script weergeven**. **RunShellScript** wijkt af van de andere opdrachten aangezien kunt u uw eigen aangepaste script opgeven.

> [!NOTE]
> De ingebouwde opdrachten kunnen niet worden bewerkt.

Nadat de opdracht is gekozen, klikt u op **uitvoeren** het script uit te voeren. Het script wordt uitgevoerd en als u klaar bent, retourneert de uitvoer en eventuele fouten in het uitvoervenster weergegeven. De volgende schermafbeelding ziet u een voorbeeld van uitvoer wordt uitgevoerd de **ifconfig** opdracht.

![Uitvoer van de opdracht-script uitvoeren](./media/run-command/run-command-script-output.png)

## <a name="available-commands"></a>Beschikbare opdrachten

Deze tabel bevat de lijst met opdrachten die beschikbaar zijn voor virtuele Linux-machines. De **RunShellScript** opdracht kan worden gebruikt om een aangepast script dat u wilt uitvoeren.

|**Naam**|**Beschrijving**|
|---|---|
|**RunShellScript**|Een Linux-shell-script wordt uitgevoerd.|
|**ifconfig**| Opvragen van de configuratie van alle netwerkinterfaces.|

## <a name="limiting-access-to-run-command"></a>Beperken van toegang tot de opdracht uitvoeren

Lijst van de uitvoering van de opdrachten of worden de details van een opdracht moet de `Microsoft.Compute/locations/runCommands/read` machtiging, waarmee de ingebouwde [lezer](../../role-based-access-control/built-in-roles.md#reader) rol en hoger.

Het uitvoeren van een opdracht moet de `Microsoft.Compute/virtualMachines/runCommand/action` machtiging, die de [Inzender](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol en hoger.

U kunt een van de [ingebouwde](../../role-based-access-control/built-in-roles.md) rollen of maak een [aangepaste](../../role-based-access-control/custom-roles.md) rol opdracht uitvoeren te gebruiken.

## <a name="next-steps"></a>Volgende stappen

Zie, [scripts uitvoeren in uw Linux-VM](run-scripts-in-vm.md) voor meer informatie over andere manieren om op afstand opdrachten en scripts uitvoeren in uw virtuele machine.
