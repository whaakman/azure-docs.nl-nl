---
title: Azure Automation-Runbooktypen | Microsoft Docs
description: 'Beschrijft de verschillende soorten runbooks die u in Azure Automation en overwegingen die u in aanmerking nemen gebruiken kunt moet bij het bepalen van welke type moet worden gebruikt. '
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 9265c975-4281-4819-a84f-d86641277f36
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/01/2017
ms.author: bwren
ms.openlocfilehash: e4a8ab0e68d6614fea1b44f0115a1c633f145277
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-automation-runbook-types"></a>Azure Automation-runbooktypen
Azure Automation biedt ondersteuning voor verschillende soorten runbooks die worden kort beschreven in de volgende tabel.  De onderstaande secties bevatten meer informatie over elk met inbegrip van de overwegingen over het gebruik van elk type.

| Type | Beschrijving |
|:--- |:--- |
| [Grafisch](#graphical-runbooks) |Op basis van Windows PowerShell en gemaakt en bewerkt geheel vanuit de grafische editor in Azure-portal. |
| [Grafische PowerShell Workflow](#graphical-runbooks) |Op basis van Windows PowerShell-werkstroom en gemaakt en bewerkt volledig in de grafische editor in Azure-portal. |
| [PowerShell](#powershell-runbooks) |Tekstrunbook op basis van Windows PowerShell-script. |
| [PowerShell-werkstroom](#powershell-workflow-runbooks) |Tekstrunbook op basis van Windows PowerShell-werkstroom. |
| [Python](#python-runbooks) |Tekstrunbook op basis van Python. |

## <a name="graphical-runbooks"></a>Grafische runbooks
[Grafische](automation-runbook-types.md#graphical-runbooks) en grafische PowerShell Workflow-runbooks zijn gemaakt en bewerkt met de grafische editor in Azure portal.  U kunt exporteren naar een bestand en importeer ze in een andere automation-account, maar u kunt maken of bewerken met een ander hulpprogramma.  Grafische runbooks PowerShell-code genereren, maar u kunt rechtstreeks weergeven of wijzigen van de code. Grafische runbooks kan niet worden geconverteerd naar een van de [tekstindelingen](automation-runbook-types.md), noch kan een tekstrunbook worden geconverteerd naar grafische indeling. Grafische runbooks kunnen worden geconverteerd naar grafische PowerShell Workflow-runbooks tijdens het importeren en vice versa.

### <a name="advantages"></a>Voordelen
* Visual authoring model insert-koppeling configureren  
* Richt u op hoe de gegevens door het proces loopt  
* Visueel vertegenwoordigen processen voor Apparaatbeheer  
* Andere runbooks bevatten als onderliggende runbooks op hoog niveau werkstromen maken  
* Modulaire programmering worden gebruikers aangemoedigd  


### <a name="limitations"></a>Beperkingen
* Kan runbook buiten Azure-portal niet bewerken.
* Kan een Code-activiteit met PowerShell-code om uit te voeren complexe logica in beslag.
* Kan bekijken of de PowerShell-code die wordt gemaakt met de grafische werkstroom rechtstreeks te bewerken. Houd er rekening mee dat u de code die u in Code activiteiten maakt kunt weergeven.

## <a name="powershell-runbooks"></a>PowerShell-runbooks
PowerShell-runbooks zijn gebaseerd op Windows PowerShell.  Rechtstreeks bewerkt u de code van het runbook met de teksteditor in de Azure portal.  U kunt ook een willekeurige offline teksteditor gebruiken en [importeren van het runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) in Azure Automation.

### <a name="advantages"></a>Voordelen
* Alle complexe logica met PowerShell-code zonder de extra complexiteit van PowerShell Workflow geïmplementeerd. 
* Start Runbook sneller dan PowerShell Workflow-runbooks omdat hoeft niet te worden gecompileerd voordat u.

### <a name="limitations"></a>Beperkingen
* Moet bekend zijn met het PowerShell-scripts.
* Kan niet worden gebruikt [parallelle verwerking](automation-powershell-workflow.md#parallel-processing) meerdere acties parallel uitvoeren.
* Kan niet worden gebruikt [controlepunten](automation-powershell-workflow.md#checkpoints) runbook in geval van een fout bij hervatten.
* PowerShell Workflow-runbooks en grafische runbooks kunnen alleen worden opgenomen als onderliggende runbooks met de cmdlet Start-AzureAutomationRunbook waarmee u een nieuwe taak maakt.

### <a name="known-issues"></a>Bekende problemen
Hieronder vindt u huidige bekende problemen met de PowerShell-runbooks.

* PowerShell-runbooks kunnen niet ophalen van een niet-versleutelde [variabelenactivum](automation-variables.md) met een null-waarde.
* Kan de PowerShell-runbooks niet ophalen een [variabelenactivum](automation-variables.md) met  *~*  in de naam.
* Get-Process in een lus in een PowerShell runbook vastlopen na ongeveer 80 iteraties. 
* Een PowerShell-runbook kan mislukken als wordt geprobeerd een zeer grote hoeveelheid gegevens in één keer naar de uitvoerstroom schrijven.   U kunt gewoonlijk dit probleem omzeilen door het uitvoeren van alleen de informatie die u nodig hebt bij het werken met grote objecten.  Bijvoorbeeld, in plaats van het uitvoeren van ongeveer *Get-Process*, kunt u de vereiste velden met uitvoeren *Get-Process | Selecteer de procesnaam, CPU*.

## <a name="powershell-workflow-runbooks"></a>PowerShell Workflow-runbooks
PowerShell Workflow-runbooks zijn tekst runbooks op basis van [Windows PowerShell-werkstroom](automation-powershell-workflow.md).  Rechtstreeks bewerkt u de code van het runbook met de teksteditor in de Azure portal.  U kunt ook een willekeurige offline teksteditor gebruiken en [importeren van het runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) in Azure Automation.

### <a name="advantages"></a>Voordelen
* Alle complexe logica met PowerShell Workflow-code implementeren.
* Gebruik [controlepunten](automation-powershell-workflow.md#checkpoints) runbook in geval van een fout bij hervatten.
* Gebruik [parallelle verwerking](automation-powershell-workflow.md#parallel-processing) meerdere acties parallel uitvoeren.
* Andere grafische runbooks en PowerShell Workflow-runbooks kunt opnemen als onderliggende runbooks voor het maken van werkstromen op hoog niveau.

### <a name="limitations"></a>Beperkingen
* Auteur moet bekend zijn met PowerShell Workflow.
* Runbook moet maken met de extra complexiteit van PowerShell Workflow, zoals [gedeserialiseerd objecten](automation-powershell-workflow.md#code-changes).
* Runbook duurt langer dan de PowerShell-runbooks starten, omdat het moet worden gecompileerd voordat u.
* PowerShell-runbooks kunnen alleen worden opgenomen als onderliggende runbooks met de cmdlet Start-AzureAutomationRunbook waarmee u een nieuwe taak maakt.

## <a name="python-runbooks"></a>Python-runbooks
Python runbooks compileren onder Python 2.  U kunt de code van het runbook met de teksteditor in de Azure portal rechtstreeks bewerken of kunt u een offline teksteditor en [importeren van het runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) in Azure Automation.

### <a name="advantages"></a>Voordelen
* Gebruikmaken van de robuuste standaardbibliotheek van Python.

### <a name="limitations"></a>Beperkingen
* Moet bekend bent met Python-scripts.
* Alleen Python 2 wordt ondersteund op dit moment, wat betekent dat bepaalde functies van Python 3 zal mislukken.

### <a name="known-issues"></a>Bekende problemen
Hieronder vindt u huidige bekende problemen met Python-runbooks.

* Als u wilt gebruikmaken van derden bibliotheken, het runbook moet worden uitgevoerd op een [Windows Hybrid Runbook Worker](https://docs.microsoft.com/azure/automation/automation-windows-hrw-install) of [Linux Hybrid Runbook Worker](https://docs.microsoft.com/azure/automation/automation-linux-hrw-install) met de bibliotheken die al is geïnstalleerd op de computer voordat de runbook wordt gestart.

## <a name="considerations"></a>Overwegingen
U moet rekening houden de volgende aanvullende overwegingen bij het bepalen van welke type moet worden gebruikt voor een bepaald runbook.

* U kunt runbooks niet converteren van grafische tekstuele type of vice versa.
* Er gelden beperkingen met behulp van runbooks met verschillende typen als een onderliggend runbook.  Zie [onderliggende runbooks in Azure Automation](automation-child-runbooks.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het ontwerpen van grafisch runbook, [grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md)
* Om te begrijpen van de verschillen tussen PowerShell en PowerShell-werkstromen voor runbooks, Zie [Learning Windows PowerShell Workflow](automation-powershell-workflow.md)
* Zie voor meer informatie over het maken of importeren van een Runbook [een Runbook maken of importeren](automation-creating-importing-runbook.md)

