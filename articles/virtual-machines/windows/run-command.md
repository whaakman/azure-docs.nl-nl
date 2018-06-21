---
title: PowerShell-scripts uitvoeren in een virtuele Windows-machine in Azure
description: In dit onderwerp wordt beschreven hoe u PowerShell-scripts in een Windows Azure virtuele machine met behulp van de opdracht uitvoeren
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 3feed9c1c8903db66a0506f09161982dadaa79ba
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284961"
---
# <a name="run-powershell-scripts-in-your-windows-vm-with-run-command"></a>PowerShell-scripts uitvoeren in uw Windows-VM met de opdracht uitvoeren

Opdracht maakt gebruik van de VM-agent om uit te voeren PowerShell-scripts in een Windows Azure VM shell uitvoeren. Deze scripts kunnen worden gebruikt voor algemene machine of Toepassingsbeheer en kunnen worden gebruikt om snel te onderzoeken en VM-netwerk- en problemen oplossen en ophalen van de virtuele machine terug naar een goede status.

## <a name="benefits"></a>Voordelen

Er zijn meerdere opties die kunnen worden gebruikt voor toegang tot uw virtuele machines. Opdracht kunt u scripts uitvoeren op uw virtuele machines op afstand met behulp van de VM-agent. Opdracht kan worden gebruikt via de Azure-portal [REST-API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), of [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand).

Deze functie is nuttig in alle scenario's waarbij u wilt uitvoeren van een script interactietypen een virtuele machines en is een van de enige manieren oplossen en het herstellen van een virtuele machine die niet het RDP of SSH-poort openen als gevolg van onjuiste netwerk of een gebruiker met beheerdersrechten de configuratie.

## <a name="restrictions"></a>Beperkingen

De volgende beperkingen van toepassing wanneer de opdracht uitvoeren:

* Uitvoer is beperkt tot de laatste 4096 bytes
* De minimale tijd voor het uitvoeren van een script is ongeveer 20 seconden
* Scripts die op Windows worden uitgevoerd als systeem
* Een script tegelijk kunt uitvoeren
* Scripts die om informatie (interactieve modus vraagt) worden niet ondersteund.
* U kunt een script wordt uitgevoerd niet annuleren.
* De maximale tijdsduur dat een script kunt uitvoeren is 90 minuten, na waarin het time-out wordt

## <a name="run-a-command"></a>Een opdracht uitvoeren

Navigeer naar een virtuele machine in [Azure](https://portal.azure.com) en selecteer **opdracht uitvoeren** onder **OPERATIONS**. Krijgt u een lijst met beschikbare opdrachten uit te voeren op de virtuele machine.

![Opdrachtlijst uitvoeren](./media/run-command/run-command-list.png)

Kies een andere opdracht om uit te voeren. Sommige van de opdrachten kan optioneel of vereist invoerparameters hebben. Voor deze opdrachten worden de parameters weergegeven als tekstvelden voor u de invoerwaarden opgeven. Voor elke opdracht kunt u het script dat wordt uitgevoerd door het uitbreiden van weergeven **script weergeven**. **RunPowerShellScript** verschilt van de andere opdrachten zoals kunt u uw eigen aangepaste scripts opgeven.

> [!NOTE]
> De ingebouwde opdrachten kunnen niet worden bewerkt.

Zodra de opdracht is gekozen, klikt u op **uitvoeren** het script uit te voeren. Het script wordt uitgevoerd en als u klaar is, retourneert de uitvoer en eventuele fouten in het uitvoervenster. De volgende schermafbeelding ziet u een voorbeeld van uitvoer uitvoeren van de **RDPSettings** opdracht.

![Uitvoer van een opdracht uitvoeren](./media/run-command/run-command-script-output.png)

## <a name="commands"></a>Opdrachten

Deze tabel bevat de lijst met opdrachten die beschikbaar is voor VM's van Windows. De **RunPowerShellScript** opdracht kan worden gebruikt voor een aangepast script die u wilt uitvoeren.

|**Naam**|**Beschrijving**|
|---|---|
|**RunPowerShellScript**|Een PowerShell-script wordt uitgevoerd|
|**EnableRemotePS**|Hiermee configureert u de machine voor het inschakelen van externe PowerShell.|
|**EnableAdminAccount**|Als het lokale Administrator-account is uitgeschakeld en als dat het geval schakelt u deze controleert.|
|**IPConfig**| Geeft gedetailleerde informatie voor het IP-adres, subnetmasker en de standaard gateway voor elke adapter die is gekoppeld aan TCP/IP.|
|**RDPSettings**|Controleert de registerinstellingen en domeininstellingen voor Groepsbeleid. Stelt beleidsacties als machine deel van een domein uitmaakt of Hiermee wijzigt u de instellingen op de standaardwaarden.|
|**ResetAccountPassword**| Hiermee stelt u het wachtwoord voor ingebouwde beheerdersaccount.|
|**ResetRDPCert**|Hiermee verwijdert u het SSL-certificaat dat is gekoppeld aan de RDP-listener en herstelt u de beveiliging van de listerner RDP in standaard. Dit script gebruiken als er problemen met het certificaat.|
|**SetRDPPort**|Stelt de standaardinstellingen of de gebruiker opgegeven poortnummer voor extern bureaublad-verbindingen. Hiermee schakelt u firewallregel voor binnenkomend toegang tot de poort.|

## <a name="powershell"></a>PowerShell

Hieronder volgt een voorbeeld met behulp van de [Invoke-AzureRmVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) cmdlet een PowerShell-script uitvoeren op een virtuele machine in Azure.

```azurepowershell-interactive
Invoke-AzureRmVMRunCommand -ResourceGroupName '<myResourceGroup>' -Name '<myVMName>' -CommandId 'RunPowerShellScript' -ScriptPath '<pathToScript>' -Parameter @{"arg1" = "var1";"arg2" = "var2"}
```

## <a name="limiting-access-to-run-command"></a>Beperken van toegang tot de opdracht uitvoeren

Lijst van de opdrachten uitvoeren of worden de details van een opdracht moet de `Microsoft.Compute/locations/runCommands/read` toestemming hebben, die de ingebouwde [lezer](../../role-based-access-control/built-in-roles.md#reader) rol en hoger.

Het uitvoeren van een opdracht moet de `Microsoft.Compute/virtualMachines/runCommand/action` toestemming hebben, die de [Inzender](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol en hoger.

U kunt een van de [ingebouwde](../../role-based-access-control/built-in-roles.md) rollen of maak een [aangepaste](../../role-based-access-control/custom-roles.md) rol opdracht uitvoeren te gebruiken.

## <a name="next-steps"></a>Volgende stappen

Zie, [scripts uitvoeren in uw Windows-VM](run-scripts-in-vm.md) voor meer informatie over andere manieren om op afstand opdrachten en scripts uitvoeren in uw virtuele machine.