---
title: Scripts uitvoeren in een Azure Linux VM
description: In dit onderwerp wordt beschreven hoe u scripts binnen een virtuele machine uit te voeren
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/02/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 3881a6d0ee1139fa481908f09b6e96efda24a5cd
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55979000"
---
# <a name="run-scripts-in-your-linux-vm"></a>Scripts uitvoeren in uw Linux-VM

Voor het automatiseren van taken of oplossen van problemen, wellicht u opdrachten kunt uitvoeren in een virtuele machine. Het volgende artikel geeft een kort overzicht van de functies die beschikbaar zijn voor het uitvoeren van scripts en -opdrachten binnen uw virtuele machines.

## <a name="custom-script-extension"></a>Aangepaste scriptextensie

De [Custom Script Extension](../extensions/custom-script-linux.md) wordt voornamelijk gebruikt voor post-implementatie-configuratie en software-installatie.

* Downloaden en uitvoeren van scripts in virtuele machines van Azure.
* Kan worden uitgevoerd met behulp van Azure Resource Manager-sjablonen, Azure CLI, REST-API, PowerShell of Azure-portal.
* Script-bestanden worden gedownload uit Azure storage of GitHub of opgegeven van de PC bij uitvoering vanuit de Azure-portal.
* PowerShell-script uitgevoerd op Windows-machines en Bash-scripts in Linux-machines.
* Dit is handig voor post-implementatieconfiguratie, software-installatie, en andere configuratie of beheertaken.

## <a name="run-command"></a>Opdracht uitvoeren

De [opdracht uitvoeren](run-command.md) functie kunt u virtuele machine en Toepassingsbeheer en problemen oplossen met behulp van scripts, en is beschikbaar, zelfs wanneer de computer is niet bereikbaar is, bijvoorbeeld als de Gast-firewall beschikt niet over de RDP of SSH-poort Open.

* Scripts uitvoeren in virtuele machines van Azure.
* Kan worden uitgevoerd met [Azure-portal](run-command.md), [REST-API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), of [PowerShell](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand)
* Snel uitvoeren van een script en weergave van de uitvoer en Herhaal zo nodig in Azure portal.
* Script rechtstreeks kan worden getypt of u een van de ingebouwde scripts kunt uitvoeren.
* PowerShell-script uitgevoerd op Windows-machines en Bash-scripts in Linux-machines.
* Dit is handig voor de virtuele machine en Toepassingsbeheer en voor het uitvoeren van scripts in virtuele machines die niet bereikbaar zijn.

## <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker

De [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md) biedt algemeen beheer voor machine, de toepassing en de omgeving van de gebruiker aangepaste scripts die zijn opgeslagen in een Automation-account.

* Scripts uitvoeren in Azure en niet-Azure-machines.
* Kan worden uitgevoerd met behulp van Azure portal, Azure CLI, REST-API, PowerShell, webhook.
* Scripts die zijn opgeslagen en beheerd in een Automation-Account.
* PowerShell, PowerShell-werkstroom, Python of grafisch runbooks uitvoeren
* Geen beperking op script keer uitvoeren.
* Meerdere scripts kunnen gelijktijdig worden uitgevoerd.
* Uitvoer van het volledige script wordt geretourneerd en opgeslagen.
* Taakgeschiedenis 90 dagen beschikbaar.
* Scripts kunnen worden uitgevoerd als lokaal systeem of met de gebruiker opgegeven referenties.
* Vereist [handmatige installatie](../../automation/automation-windows-hrw-install.md)

## <a name="serial-console"></a>Seriële console

De [seriële console](serial-console.md) voorziet in directe toegang tot een VM, vergelijkbaar met een toetsenbord dat is verbonden met de virtuele machine.

* Opdrachten uitvoeren in virtuele machines van Azure.
* Kan worden uitgevoerd met behulp van een op tekst gebaseerde console op de machine in Azure portal.
* Meld u aan bij de computer met een lokale gebruikersaccount toegekend.
* Dit is handig wanneer u toegang tot de virtuele machine is vereist, ongeacht het netwerk of besturingssysteem status van de machine.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de verschillende functies die beschikbaar zijn voor het uitvoeren van scripts en -opdrachten binnen uw virtuele machines.

* [Aangepaste scriptextensie](../extensions/custom-script-linux.md)
* [Opdracht uitvoeren](run-command.md)
* [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md)
* [Seriële console](serial-console.md)