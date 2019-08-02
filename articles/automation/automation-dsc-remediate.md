---
title: Niet-compatibele Azure Automation status configuratie servers herstellen
description: Configuraties op aanvraag opnieuw Toep assen op servers waar de configuratie status is gedrijfd
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.topic: conceptual
ms.date: 07/17/2019
manager: nirb
ms.openlocfilehash: a6c7639cb4988eb13dfaa1c151085cda6e53b5d3
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68614496"
---
# <a name="remediate-non-compliant-dsc-servers"></a>Niet-compatibele DSC-servers herstellen

Wanneer servers zijn geregistreerd met Azure Automation status configuratie, wordt de ' configuratie modus ' ingesteld op ApplyOnly, ApplyandMonitor of ApplyAndAutoCorrect.
Als de modus niet is ingesteld op auto correctie, blijven servers die van een compatibele status zijn, om welke reden dan ook niet-compatibel, totdat deze hand matig zijn gecorrigeerd.

Azure Compute biedt een functie met de naam run-opdracht waarmee klanten scripts in virtuele machines kunnen uitvoeren.
Dit document bevat voorbeeld scripts voor deze functie wanneer de configuratie-drift hand matig wordt gecorrigeerd.

## <a name="correct-drift-of-windows-virtual-machines-using-powershell"></a>Juiste drift van virtuele Windows-machines met behulp van Power shell

Zie de documentatie pagina [Power shell-scripts uitvoeren in uw Windows-VM met de opdracht uitvoeren](/azure/virtual-machines/windows/run-command)voor stapsgewijze instructies voor het uitvoeren van de opdracht functie op virtuele Windows-machines.

Gebruik de `Update-DscConfiguration` cmdlet om een configuratie knooppunt van Azure Automation status af te dwingen om de nieuwste configuratie te downloaden en toe te passen.
Zie de cmdlet [-documentatie-update-DscConfiguration](/powershell/module/psdesiredstateconfiguration/update-dscconfiguration)voor meer informatie.

```powershell
Update-DscConfiguration -Wait -Verbose
```

## <a name="correct-drift-of-linux-virtual-machines"></a>Juiste drift van virtuele Linux-machines

Vergelijk bare functionaliteit is momenteel niet beschikbaar voor Linux-servers.
De enige optie is om het registratie proces te herhalen.
Voor Azure-knoop punten kan een drift-correctie worden uitgevoerd vanuit de portal of met AZ Automation cmdlets.
Details over dit proces worden beschreven op de pagina onboarding- [machines voor beheer door Azure Automation status configuratie](/azure/automation/automation-dsc-onboarding#azure-portal).
Voor hybride knoop punten kan een drift-correctie worden uitgevoerd met behulp van de meegeleverde python-scripts.
Zie de documentatie in [Power shell DSC voor Linux opslag plaats voor](https://github.com/Microsoft/PowerShell-DSC-for-Linux#performing-dsc-operations-from-the-linux-computer)meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Zie [Azure Automation status configuratie](/powershell/module/azurerm.automation/#automation) -cmdlets voor informatie over de Power shell-cmdlet.
- Voor een voor beeld van het gebruik van Azure Automation status configuratie in een pijp lijn voor continue implementatie gaat u naar [continue implementatie met behulp van Azure Automation-status configuratie en chocolade](automation-dsc-cd-chocolatey.md)
