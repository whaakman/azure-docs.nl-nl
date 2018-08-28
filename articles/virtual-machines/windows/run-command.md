---
title: PowerShell-scripts uitvoeren in een Windows-VM in Azure
description: In dit onderwerp wordt beschreven hoe u om uit te voeren PowerShell-scripts in een Windows Azure-machine met behulp van de opdracht uitvoeren
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: f42d3fb709807d77fb0f390b6da76efc4a60dced
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43090567"
---
# <a name="run-powershell-scripts-in-your-windows-vm-with-run-command"></a>PowerShell-scripts uitvoeren in uw Windows-VM met de opdracht uitvoeren

Opdracht wordt de VM-agent om te worden uitgevoerd binnen een Azure-VM Windows PowerShell-scripts uitvoeren. Deze scripts voor algemene machine of beheer van toepassingen kunnen worden gebruikt en kunnen worden gebruikt om snel opsporen en VM-netwerk- en problemen oplossen en ophalen van de virtuele machine naar een goede staat verkeren.

## <a name="benefits"></a>Voordelen

Er zijn meerdere opties die kunnen worden gebruikt voor toegang tot uw virtuele machines. Voer de opdracht kunt u scripts uitvoeren op uw virtuele machines die op afstand met behulp van de VM-agent. Voer de opdracht kan worden gebruikt via Azure portal, [REST-API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), of [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand).

Deze mogelijkheid is handig in alle scenario's waarin u wilt uitvoeren van een script in een virtuele machine en is een van de enige manieren om problemen op te herstellen van een virtuele machine die beschikt niet over de RDP of SSH-poort openen vanwege een onjuiste netwerk of de gebruiker met beheerdersrechten de configuratie.

## <a name="restrictions"></a>Beperkingen

De volgende beperkingen zijn van toepassing wanneer u de opdracht uitvoeren:

* Uitvoer is beperkt tot de laatste 4096 bytes
* De minimale tijd voor het uitvoeren van een script is ongeveer 20 seconden
* Scripts worden uitgevoerd als systeem op Windows
* Een script tegelijk kan worden uitgevoerd
* U kunt een script uit te voeren niet annuleren
* De maximale tijd die kan worden uitgevoerd door een script is 90 minuten, na waarin het time-out wordt
* Uitgaande connectiviteit van de virtuele machine is vereist om de resultaten van het script te retourneren.

## <a name="run-a-command"></a>Een opdracht uitvoeren

Navigeer naar een virtuele machine in [Azure](https://portal.azure.com) en selecteer **RunCommand-** onder **OPERATIONS**. Krijgt u een lijst van de beschikbare opdrachten uit te voeren op de virtuele machine.

![Lijst met opdrachten uitvoeren](./media/run-command/run-command-list.png)

Kies een opdracht uit te voeren. Enkele van de opdrachten mogelijk optioneel of vereiste invoerparameters. Voor deze opdrachten worden de parameters weergegeven als tekstvelden voor u de ingevoerde waarden op te geven. Voor elke opdracht die u kunt het script dat wordt uitgevoerd door het uitbreiden van weergeven **script weergeven**. **RunPowerShellScript** wijkt af van de andere opdrachten aangezien kunt u uw eigen aangepaste script opgeven.

> [!NOTE]
> De ingebouwde opdrachten kunnen niet worden bewerkt.

Nadat de opdracht is gekozen, klikt u op **uitvoeren** het script uit te voeren. Het script wordt uitgevoerd en als u klaar bent, retourneert de uitvoer en eventuele fouten in het uitvoervenster weergegeven. De volgende schermafbeelding ziet u een voorbeeld van uitvoer wordt uitgevoerd de **RDPSettings** opdracht.

![Uitvoer van de opdracht-script uitvoeren](./media/run-command/run-command-script-output.png)

## <a name="commands"></a>Opdrachten

Deze tabel bevat de lijst met opdrachten die beschikbaar zijn voor Windows-VM's. De **RunPowerShellScript** opdracht kan worden gebruikt om een aangepast script dat u wilt uitvoeren.

|**Naam**|**Beschrijving**|
|---|---|
|**RunPowerShellScript**|Een PowerShell-script wordt uitgevoerd|
|**EnableRemotePS**|Hiermee configureert u de machine voor het inschakelen van externe PowerShell.|
|**EnableAdminAccount**|Controleert of als het lokale Administrator-account is uitgeschakeld, en als dit het geval is ingeschakeld.|
|**IP-configuratie**| Geeft gedetailleerde informatie voor de IP-adres, subnet en de standaard-gateway voor elke adapter die is gekoppeld aan TCP/IP.|
|**RDPSettings**|Controleert of registerinstellingen en domeininstellingen voor Groepsbeleid. Voorgesteld beleidsacties als machine deel van een domein uitmaakt of wijzigt u de instellingen op standaardwaarden.|
|**ResetAccountPassword**| Hiermee stelt u het wachtwoord voor ingebouwde beheerdersaccount.|
|**ResetRDPCert**|Hiermee verwijdert u het SSL-certificaat dat is gekoppeld aan de RDP-listener en de beveiliging van RDP listerner standaard hersteld. Gebruik dit script als u problemen ondervindt met het certificaat wordt weergegeven.|
|**SetRDPPort**|Stelt de standaard of de gebruiker opgegeven poortnummer voor extern bureaublad-verbindingen. Hiermee kunt firewall-regel voor binnenkomende toegang tot de poort.|

## <a name="powershell"></a>PowerShell

Hier volgt een voorbeeld met behulp van de [Invoke-AzureRmVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) cmdlet om een PowerShell-script uitvoeren op een Azure-VM.

```azurepowershell-interactive
Invoke-AzureRmVMRunCommand -ResourceGroupName '<myResourceGroup>' -Name '<myVMName>' -CommandId 'RunPowerShellScript' -ScriptPath '<pathToScript>' -Parameter @{"arg1" = "var1";"arg2" = "var2"}
```

## <a name="limiting-access-to-run-command"></a>Beperken van toegang tot de opdracht uitvoeren

Lijst van de uitvoering van de opdrachten of worden de details van een opdracht moet de `Microsoft.Compute/locations/runCommands/read` machtiging, waarmee de ingebouwde [lezer](../../role-based-access-control/built-in-roles.md#reader) rol en hoger.

Het uitvoeren van een opdracht moet de `Microsoft.Compute/virtualMachines/runCommand/action` machtiging, die de [Inzender](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol en hoger.

U kunt een van de [ingebouwde](../../role-based-access-control/built-in-roles.md) rollen of maak een [aangepaste](../../role-based-access-control/custom-roles.md) rol opdracht uitvoeren te gebruiken.

## <a name="next-steps"></a>Volgende stappen

Zie, [scripts uitvoeren in uw Windows-VM](run-scripts-in-vm.md) voor meer informatie over andere manieren om op afstand opdrachten en scripts uitvoeren in uw virtuele machine.
