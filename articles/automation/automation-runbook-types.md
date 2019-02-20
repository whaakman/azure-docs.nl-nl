---
title: Azure Automation-Runbooktypen
description: 'Beschrijft de verschillende typen runbooks die u in Azure Automation en overwegingen waarmee u rekening houden gebruiken kunt moet bij het bepalen van welk type te gebruiken. '
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 940a5839fe2c2eea11d9570d3dca48cd514e21af
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56417241"
---
# <a name="azure-automation-runbook-types"></a>Azure Automation-runbooktypen

Azure Automation biedt ondersteuning voor verschillende typen runbooks die worden kort beschreven in de volgende tabel.  De onderstaande secties bevatten meer informatie over elk waarbij u rekening houdt van het moment waarop het gebruik van elk type.

| Type | Description |
|:--- |:--- |
| [Grafisch](#graphical-runbooks) |Op basis van Windows PowerShell en gemaakt en bewerkt volledig in grafische editor in Azure portal. |
| [Grafische PowerShell-werkstroom](#graphical-runbooks) |Op basis van Windows PowerShell-werkstroom en gemaakt en bewerkt volledig in de grafische editor in Azure portal. |
| [PowerShell](#powershell-runbooks) |Tekstrunbook op basis van Windows PowerShell-script. |
| [PowerShell-werkstroom](#powershell-workflow-runbooks) |Tekstrunbook op basis van Windows PowerShell-werkstroom. |
| [Python](#python-runbooks) |Tekstrunbook op basis van Python. |

## <a name="graphical-runbooks"></a>Grafische runbooks

[Grafische](automation-runbook-types.md#graphical-runbooks) en grafische PowerShell Workflow-runbooks zijn gemaakt en bewerkt met de grafische editor in Azure portal.  U kunt exporteren naar een bestand en importeer ze in een ander automation-account. Maar u kunt maken of bewerken met een ander hulpprogramma. Grafische runbooks PowerShell-code genereren, maar u niet rechtstreeks weergeven of wijzigen van de code. Grafische runbooks kan niet worden geconverteerd naar een van de [tekstindelingen](automation-runbook-types.md), noch kan een tekstrunbook worden geconverteerd naar grafische indeling. Grafische runbooks kunnen worden geconverteerd naar grafische PowerShell Workflow-runbooks tijdens het importeren en andersom.

### <a name="advantages"></a>Voordelen

* Visueel ontwerpen model insert-koppeling configureren  
* Richt u op hoe gegevens door het proces stromen  
* Visueel vertegenwoordigen beheerprocessen  
* Andere runbooks bevatten als onderliggende runbooks op hoog niveau werkstromen maken  
* Modulaire programmeren worden gebruikers aangemoedigd  

### <a name="limitations"></a>Beperkingen

* Kan runbook buiten Azure-portal niet bewerken.
* Een activiteit met PowerShell-code voor het uitvoeren van complexe logica mogelijk.
* Kan bekijken of de PowerShell-code die is gemaakt door de grafische workflow rechtstreeks bewerken. U kunt de code die u in een Code-activiteiten maakt weergeven.

## <a name="powershell-runbooks"></a>PowerShell-runbooks

PowerShell-runbooks zijn gebaseerd op Windows PowerShell.  Rechtstreeks bewerkt u de code van het runbook met behulp van de teksteditor in Azure portal.  U kunt ook een offline teksteditor gebruiken en [Importeer het runbook](manage-runbooks.md) in Azure Automation.

### <a name="advantages"></a>Voordelen

* Implementeer alle complexe logica met PowerShell-code zonder de extra complexiteit van de PowerShell-werkstroom.
* Runbook start sneller dan PowerShell Workflow-runbooks omdat hoeft niet te worden gecompileerd voordat wordt uitgevoerd.

### <a name="limitations"></a>Beperkingen

* Moet bekend zijn met de PowerShell-scripts.
* Kan niet worden gebruikt [parallelle verwerking](automation-powershell-workflow.md#parallel-processing) meerdere acties parallel uitvoeren.
* Kan niet worden gebruikt [controlepunten](automation-powershell-workflow.md#checkpoints) runbook hervatten als er een fout is opgetreden.
* Grafische runbooks en PowerShell Workflow-runbooks kunnen alleen worden opgenomen als onderliggende runbooks met behulp van de cmdlet Start-AzureAutomationRunbook, waarmee een nieuwe taak wordt gemaakt.

### <a name="known-issues"></a>Bekende problemen

Hieronder vindt u huidige bekende problemen met PowerShell-runbooks.

* PowerShell-runbooks kunnen niet ophalen van een niet-versleutelde [variabeleasset](automation-variables.md) met een null-waarde.
* PowerShell-runbooks kunnen niet worden opgehaald een [variabeleasset](automation-variables.md) met *~* in de naam.
* Get-Process in een lus in een PowerShell runbook kan vastlopen na ongeveer 80 herhalingen.
* Een PowerShell-runbook kan mislukken als wordt geprobeerd een grote hoeveelheid gegevens in één keer naar de uitvoerstroom schrijven.   U kunt doorgaans dit probleem omzeilen door het uitvoeren van alleen de informatie die u nodig hebt bij het werken met grote objecten.  Bijvoorbeeld, in plaats van het uitvoeren van ongeveer *Get-Process*, kunt u alleen de vereiste velden met uitvoer *Get-Process | Schakel ProcessName, CPU*.

## <a name="powershell-workflow-runbooks"></a>PowerShell Workflow-runbooks

PowerShell Workflow-runbooks zijn tekst-runbooks op basis van [Windows PowerShell-werkstroom](automation-powershell-workflow.md).  Rechtstreeks bewerkt u de code van het runbook met behulp van de teksteditor in Azure portal.  U kunt ook een offline teksteditor gebruiken en [Importeer het runbook](manage-runbooks.md) in Azure Automation.

### <a name="advantages"></a>Voordelen

* Alle complexe logica met PowerShell Workflow-code implementeren.
* Gebruik [controlepunten](automation-powershell-workflow.md#checkpoints) runbook hervatten als er een fout is opgetreden.
* Gebruik [parallelle verwerking](automation-powershell-workflow.md#parallel-processing) meerdere acties parallel uitvoeren.
* Andere grafische runbooks en PowerShell Workflow-runbooks kunt opnemen als onderliggende runbooks op hoog niveau werkstromen maken.

### <a name="limitations"></a>Beperkingen

* Auteur moet bekend zijn met de PowerShell-werkstroom.
* Runbook moet omgaan met extra complexiteit van de PowerShell-werkstroom, zoals [gedeserialiseerd objecten](automation-powershell-workflow.md#code-changes).
* Runbook langer duurt om te beginnen dan PowerShell-runbooks omdat ze moeten worden gecompileerd voordat wordt uitgevoerd.
* PowerShell-runbooks kunnen alleen worden opgenomen als onderliggende runbooks met behulp van de cmdlet Start-AzureAutomationRunbook, waarmee een nieuwe taak wordt gemaakt.

## <a name="python-runbooks"></a>Python-runbooks

Python-runbooks worden gecompileerd onder Python 2.  U kunt de code van het runbook met behulp van de teksteditor in Azure portal of met een offline teksteditor rechtstreeks bewerken en [Importeer het runbook](manage-runbooks.md) in Azure Automation.

### <a name="advantages"></a>Voordelen

* Gebruikmaken van de robuuste Python-bibliotheken.

### <a name="limitations"></a>Beperkingen

* Moet bekend zijn met het Python-scripts.
* Alleen Python 2 wordt ondersteund op dit moment, wat betekent dat bepaalde functies van Python 3 mislukken.
* Voor het gebruik van bibliotheken van derden, moet u [importeren van het pakket](python-packages.md) in het Automation-Account voor deze moet worden gebruikt.

## <a name="considerations"></a>Overwegingen

Rekening gehouden met de volgende aanvullende overwegingen bij het bepalen van welk type moet worden gebruikt voor een bepaald runbook.

* U kunt runbooks kan niet converteren van grafische tekstuele type of andersom.
* Zijn er beperkingen met behulp van runbooks van verschillende typen als een onderliggend runbook. Zie voor meer informatie, [onderliggende runbooks in Azure Automation](automation-child-runbooks.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het ontwerpen van een grafisch runbook, [grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md)
* Om te weten wat de verschillen tussen PowerShell en PowerShell-werkstromen voor runbooks, Zie [Windows PowerShell-werkstroom leren](automation-powershell-workflow.md)
* Zie voor meer informatie over het maken of importeren van een Runbook [een Runbook maken of importeren](manage-runbooks.md)

