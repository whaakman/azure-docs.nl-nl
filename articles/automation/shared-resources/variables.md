---
title: Variabele assets in Azure Automation
description: Variabele assets zijn waarden die beschikbaar voor alle runbooks en DSC-configuraties in Azure Automation zijn.  In dit artikel wordt uitgelegd dat de details van de variabelen en hoe u werkt met hen in tekstuele en grafische ontwerpen.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 04/01/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fc26c0357dcb071c4c75e8684fe47144a04177e4
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58806886"
---
# <a name="variable-assets-in-azure-automation"></a>Variabele assets in Azure Automation

Variabele assets zijn waarden die beschikbaar voor alle runbooks en DSC-configuraties in uw automation-account zijn. Ze kunnen worden beheerd vanuit de Azure portal, PowerShell of in een runbook of DSC-configuratie. Automation-variabelen zijn handig voor de volgende scenario's:

- Een waarde tussen meerdere runbooks of configuraties van DSC delen.

- Een waarde tussen meerdere taken van de hetzelfde runbook of DSC-configuratie delen.

- Een waarde beheren vanuit de portal of vanuit de PowerShell-opdrachtregel die wordt gebruikt door runbooks of DSC-configuraties, zoals een set algemene configuratie-items, zoals specifieke lijst van de namen van de virtuele machine, een specifieke resourcegroep en de naam van een AD-domein.  

Omdat het Automation-variabelen zijn opgeslagen, zijn ze beschikbaar zelfs als het runbook of DSC-configuratie is mislukt. Dit gedrag kunt een waarde worden ingesteld door een runbook dat vervolgens wordt gebruikt door een andere, of wordt gebruikt door de hetzelfde runbook of DSC-configuratie de volgende keer dat deze wordt uitgevoerd.

Wanneer een variabele is gemaakt, kunt u opgeven dat deze is opgeslagen versleuteld. Gecodeerde variabelen worden veilig opgeslagen in Azure Automation en de waarde kan niet worden opgehaald uit de [Get-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Get-AzureRmAutomationVariable) cmdlet die wordt meegeleverd als onderdeel van de Azure PowerShell-module. De enige manier dat een versleutelde waarde kan worden opgehaald, is vanuit de **Get-AutomationVariable** activiteit in een runbook of DSC-configuratie.

>[!NOTE]
>Beveiligde activa in Azure Automation zijn referenties, certificaten, verbindingen en gecodeerde variabelen. Deze apparaten worden versleuteld en opgeslagen in Azure Automation met behulp van een unieke sleutel die wordt gegenereerd voor elk automation-account. Deze sleutel wordt opgeslagen in een systeem beheerd Key Vault. Voordat u een beveiligd bedrijfsmiddel opslaat, is de sleutel geladen uit Key Vault en vervolgens worden gebruikt voor het versleutelen van de asset. Dit proces wordt beheerd door Azure Automation.

## <a name="variable-types"></a>Typen variabelen

Wanneer u een variabele met de Azure portal maakt, moet u een gegevenstype in de vervolgkeuzelijst opgeven zodat het juiste besturingselement voor het invoeren van de variabele waarde kunt weergeven door de portal. De variabele is niet beperkt tot dit gegevenstype. De variabele met behulp van Windows PowerShell als u wilt dat een waarde van een ander type op te geven, moet u instellen. Als u opgeeft **niet gedefinieerd**, en vervolgens de waarde van de variabele wordt ingesteld op **$null**, en u moet de waarde met de [Set-AzureRMAutomationVariable](/powershell/module/AzureRM.Automation/Set-AzureRmAutomationVariable) cmdlet of **Set-AutomationVariable** activiteit. U kunt maken of wijzigen van de waarde voor een variabele voor complex type in de portal, maar u kunt opgeven dat een waarde van elk type met behulp van Windows PowerShell. Complexe typen worden geretourneerd als een [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject).

U kunt meerdere waarden in een enkele variabele opslaan door het maken van een matrix of een hash-tabel en deze opslaan in de variabele.

Hier volgen een lijst met variabelen die beschikbaar zijn in Automation:

* String
* Geheel getal
* DateTime
* Booleaans
* Null

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell-cmdlets

De cmdlets in de volgende tabel worden gebruikt voor AzureRM, maken en beheren van automation-referentie-assets met Windows PowerShell. Ze geleverd als onderdeel van de [AzureRM.Automation module](/powershell/azure/overview), die beschikbaar is voor gebruik in Automation-runbooks en DSC-configuraties.

| Cmdlets | Description |
|:---|:---|
|[Get-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Get-AzureRmAutomationVariable)|De waarde van een bestaande variabele opgehaald.|
|[New-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/New-AzureRmAutomationVariable)|Maakt een nieuwe variabele en stelt u de waarde ervan.|
|[Remove-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationVariable)|Hiermee verwijdert u een bestaande variabele.|
|[Set-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Set-AzureRmAutomationVariable)|Hiermee stelt u de waarde voor een bestaande variabele.|

## <a name="activities"></a>Activiteiten

De activiteiten in de volgende tabel worden gebruikt voor toegang tot de referenties in een runbook en DSC-configuraties.

| Activiteiten | Description |
|:---|:---|
|Get-AutomationVariable|De waarde van een bestaande variabele opgehaald.|
|Set-AutomationVariable|Hiermee stelt u de waarde voor een bestaande variabele.|

> [!NOTE]
> Vermijd het gebruik van variabelen in de parameter-Name van **Get-AutomationVariable** in een runbook of DSC-configuratie omdat dit detecteren van afhankelijkheden tussen runbooks of DSC-configuratie en automatisering bemoeilijken kan variabelen in de ontwerpfase.

De functies in de volgende tabel worden gebruikt voor toegang tot en variabelen in een Python2-runbook op te halen.

|Python2-functies|Beschrijving|
|:---|:---|
|automationassets.get_automation_variable|De waarde van een bestaande variabele opgehaald. |
|automationassets.set_automation_variable|Hiermee stelt u de waarde voor een bestaande variabele. |

> [!NOTE]
> U moet de module 'automationassets' aan de bovenkant van uw Python-runbook importeren om toegang tot de functies van de asset.

## <a name="creating-a-new-automation-variable"></a>Het maken van een nieuw Automation-variabele

### <a name="to-create-a-new-variable-with-the-azure-portal"></a>Een nieuwe variabele maken met de Azure-portal

1. Via uw Automation-account, klikt u op de **activa** tegel en klik vervolgens op de **activa** Selecteer **variabelen**.
2. Op de **variabelen** tegel, selecteer **toevoegen van een variabele**.
3. Vul in de opties op de **nieuwe variabele** blad en klik op **maken** opslaan van de nieuwe variabele.

### <a name="to-create-a-new-variable-with-windows-powershell"></a>Een nieuwe variabele maken met Windows PowerShell

De [New-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/New-AzureRmAutomationVariable) cmdlet maakt een nieuwe variabele en stelt u de oorspronkelijke waarde. U kunt ophalen met de waarde met behulp van [Get-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Get-AzureRmAutomationVariable). Als de waarde een eenvoudig type is, wordt die hetzelfde type geretourneerd. Als het een complex type is een **PSCustomObject** wordt geretourneerd.

De volgende voorbeeldopdrachten laten zien hoe u een variabele van het typetekenreeks maakt en vervolgens de waarde te retourneren.

```powershell
New-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" 
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
–Encrypted $false –Value 'My String'
$string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value
```

De volgende voorbeeldopdrachten laten zien hoe u een variabele maken met een complex type en geeft u de eigenschappen. In dit geval een virtuele machine object uit **Get-AzureRmVm** wordt gebruikt.

```powershell
$vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm

$vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
$vmName = $vmValue.Name
$vmIpAddress = $vmValue.IpAddress
```

## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a>Met behulp van een variabele in een runbook of DSC-configuratie

Gebruik de **Set-AutomationVariable** activiteit in op de waarde van een Automation-variabele instellen in een PowerShell-runbook of DSC-configuratie, en de **Get-AutomationVariable** te halen. Gebruik niet de **Set-AzureRMAutomationVariable** of **Get-AzureRMAutomationVariable** -cmdlets in een runbook of DSC-configuratie omdat ze minder efficiënt dan de werkstroomactiviteiten zijn. U de waarde van beveiligde variabelen met ook niet ophalen **Get-AzureRMAutomationVariable**. De enige manier om een nieuwe variabele uit binnen een runbook of DSC-configuratie maken, is met de [New-AzureRMAutomationVariable](/powershell/module/AzureRM.Automation/New-AzureRmAutomationVariable) cmdlet.

### <a name="textual-runbook-samples"></a>Tekstuele runbookvoorbeelden

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a>Instellen en het ophalen van een eenvoudige waarde van een variabele

De volgende voorbeeldopdrachten laten zien hoe instellen en ophalen van een variabele in een tekstuele runbook. In dit voorbeeld wordt ervan uitgegaan dat variabelen van het type geheel getal met de naam *NumberOfIterations* en *NumberOfRunnings* en een variabele van het typetekenreeks met de naam *SampleMessage* hebben is gemaakt.

```powershell
$NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
$NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
$SampleMessage = Get-AutomationVariable -Name 'SampleMessage'

Write-Output "Runbook has been run $NumberOfRunnings times."

for ($i = 1; $i -le $NumberOfIterations; $i++) {
    Write-Output "$i`: $SampleMessage"
}
Set-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)
```

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a>Instellen en ophalen van een complex object in een variabele

De volgende voorbeeldcode laat zien hoe u een variabele met een complexe waarde in een tekstuele runbook bijwerken. In dit voorbeeld wordt een virtuele machine van Azure is opgehaald met **Get-AzureVM** en opgeslagen in een bestaande automatiseringsvariabele.  Zoals uitgelegd in [variabeletypen](#variable-types), dit wordt opgeslagen als een PSCustomObject.

```powershell
$vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
Set-AutomationVariable -Name "MyComplexVariable" -Value $vm
```

De waarde is opgehaald van de variabele en gebruikt voor het starten van de virtuele machine in de volgende code.

```powershell
$vmObject = Get-AutomationVariable -Name "MyComplexVariable"
if ($vmObject.PowerState -eq 'Stopped') {
    Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
}
```

#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a>Instellen en ophalen van een verzameling in een variabele

De volgende voorbeeldcode laat zien hoe het gebruik van een variabele met een verzameling complexe waarden in een tekstuele runbook. In dit voorbeeld, meerdere virtuele machines van Azure worden opgehaald met **Get-AzureVM** en opgeslagen in een bestaande automatiseringsvariabele. Zoals uitgelegd in [variabeletypen](#variable-types), dit wordt opgeslagen als een verzameling van PSCustomObjects.

```powershell
$vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}
Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms
```

In de volgende code, is de verzameling opgehaald van de variabele en gebruikt voor het starten van elke virtuele machine.

```powershell
$vmValues = Get-AutomationVariable -Name "MyComplexVariable"
ForEach ($vmValue in $vmValues)
{
    if ($vmValue.PowerState -eq 'Stopped') {
        Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
    }
}
```

#### <a name="setting-and-retrieving-a-variable-in-python2"></a>Instellen en ophalen van een variabele in Python2

De volgende voorbeeldcode laat zien hoe u een variabele gebruiken, stelt u een variabele en een uitzondering voor een niet-bestaande variabele in een Python2-runbook verwerken.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a variable
value = automationassets.get_automation_variable("test-variable")
print value

# set a variable (value can be int/bool/string)
automationassets.set_automation_variable("test-variable", True)
automationassets.set_automation_variable("test-variable", 4)
automationassets.set_automation_variable("test-variable", "test-string")

# handle a non-existent variable exception
try:
    value = automationassets.get_automation_variable("non-existing variable")
except AutomationAssetNotFound:
    print "variable not found"
```

### <a name="graphical-runbook-samples"></a>Voorbeelden van een grafisch runbook

In een grafisch runbook, voegt u de **Get-AutomationVariable** of **Set-AutomationVariable** met de rechtermuisknop op de variabele in het deelvenster bibliotheek van de grafische editor en de activiteit selecteren wilt.

![Variabele aan het canvas toevoegen](../media/variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a>Waarden instellen in een variabele

De volgende afbeelding toont een voorbeeld-activiteiten bij te werken van een variabele met een eenvoudige waarde in een grafisch runbook. In dit voorbeeld **Get-AzureRmVM** een enkel Azure-machine en de naam van de computer worden opgeslagen op een bestaande automatiseringsvariabele van het type tekenreeks opgehaald. Het maakt niet uit of de [koppeling is een pijplijn of een reeks](../automation-graphical-authoring-intro.md#links-and-workflow) omdat u slechts één object in de uitvoer verwacht.

![Eenvoudige variabele instellen](../media/variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het met elkaar verbinden van activiteiten in het grafisch ontwerpen, [koppelingen in het grafisch ontwerpen](../automation-graphical-authoring-intro.md#links-and-workflow)
- Zie [Mijn eerste grafische runbook](../automation-first-runbook-graphical.md) om aan de slag te gaan met grafische runbooks
