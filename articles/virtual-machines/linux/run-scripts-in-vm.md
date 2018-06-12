---
title: Scripts uitvoeren in een Azure Linux VM
description: Dit onderwerp wordt beschreven voor het uitvoeren van scripts in een virtuele machine
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/02/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: cb811e3dba7be87c83b9893db682475351ada1c1
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266996"
---
# <a name="run-scripts-in-your-linux-vm"></a>Scripts uitvoeren in uw Linux-VM

Als u taken automatiseren of oplossen van problemen, moet u wellicht opdrachten uitvoert in een virtuele machine. Het volgende artikel bevat een kort overzicht van de functies die beschikbaar zijn voor het uitvoeren van scripts en opdrachten binnen uw virtuele machines.

## <a name="custom-script-extension"></a>Aangepaste scriptextensie

De [aangepaste Scriptextensie](../extensions/custom-script-linux.md) wordt voornamelijk gebruikt voor de post-implementatie configuratie en software-installatie.

* Download en scripts uitvoeren in virtuele machines in Azure.
* Kan worden uitgevoerd met behulp van Azure Resource Manager-sjablonen, Azure CLI, REST-API, PowerShell of Azure-portal.
* Scriptbestanden kunnen worden gedownload van Azure storage of GitHub, of van uw PC bij uitvoering vanuit de Azure-portal.
* PowerShell-script uitgevoerd op Windows-machines en Bash-scripts in Linux-machines.
* Dit is handig voor post-implementatieconfiguratie, software-installatie, en andere configuratie of beheertaken.

## <a name="run-command"></a>Opdracht uitvoeren

De [-opdracht uitvoeren](run-command.md) functie kunt u virtuele machine en Toepassingsbeheer en het oplossen van problemen met behulp van scripts en is beschikbaar zelfs wanneer de computer is niet bereikbaar is, bijvoorbeeld als de Gast-firewall beschikt niet over de RDP of SSH-poort Open.

* Scripts uitvoeren in virtuele machines in Azure.
* Kan worden uitgevoerd met [Azure-portal](run-command.md), [REST-API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), of [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand)
* Snel uitvoeren van een script en bekijk de uitvoer en Herhaal zo nodig in de Azure portal.
* Script kan rechtstreeks worden getypt of u een van de ingebouwde scripts kunt uitvoeren.
* PowerShell-script uitgevoerd op Windows-machines en Bash-scripts in Linux-machines.
* Dit is handig voor virtuele machine en Toepassingsbeheer en voor het uitvoeren van scripts in virtuele machines die niet bereikbaar zijn.

## <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker

De [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md) biedt algemeen machine-, toepassings- en omgeving beheer van de gebruiker aangepaste scripts die worden opgeslagen in een Automation-account.

* Scripts uitvoeren in Azure, en niet-Azure-machines.
* Kan worden uitgevoerd met Azure portal, Azure CLI, REST-API, PowerShell, webhook.
* Scripts opgeslagen en beheerd in een Automation-Account.
* Uitvoeren van PowerShell, PowerShell Workflow, Python of grafische runbooks
* Geen limiet tijd voor script uitvoeringstijd.
* Meerdere scripts kunnen gelijktijdig worden uitgevoerd.
* Volledige scriptuitvoer wordt geretourneerd en opgeslagen.
* De geschiedenis van de taak is beschikbaar voor 90 dagen.
* Scripts kunnen worden uitgevoerd als lokaal systeem of met de gebruiker opgegeven referenties.
* Vereist [handmatige installatie](../../automation/automation-windows-hrw-install.md)

## <a name="serial-console"></a>Seriële console

De [seriële console](serial-console.md) rechtstreekse toegang biedt aan een VM, vergelijkbaar met een toetsenbord verbonden met de virtuele machine.

* Opdrachten uitvoeren in virtuele machines in Azure.
* Kan worden uitgevoerd met behulp van een console op basis van tekst naar de computer in de Azure portal.
* Meld u aan met de machine met een lokale gebruikersaccount.
* Dit is nuttig wanneer toegang tot de virtuele machine, ongeacht het netwerk of besturingssysteem status van de machine is vereist.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de verschillende functies die beschikbaar zijn voor het uitvoeren van scripts en opdrachten binnen uw virtuele machines.

* [Aangepaste scriptextensie](../extensions/custom-script-linux.md)
* [Opdracht uitvoeren](run-command.md)
* [Hybride Runbook Worker](../../automation/automation-hybrid-runbook-worker.md)
* [Seriële console](serial-console.md)