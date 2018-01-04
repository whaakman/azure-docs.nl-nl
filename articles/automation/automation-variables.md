---
title: Variabele assets in Azure Automation | Microsoft Docs
description: Variabele assets zijn waarden die beschikbaar voor alle runbooks en in Azure Automation DSC-configuraties zijn.  Dit artikel wordt uitgelegd dat de details van de variabelen en hoe u ermee in tekstvorm en grafisch ontwerpen.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: b880c15f-46f5-4881-8e98-e034cc5a66ec
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/09/2017
ms.author: magoedte;bwren
ms.openlocfilehash: e38d2b751090cfdc078de4e8c683c6bb9b48fac3
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="variable-assets-in-azure-automation"></a>Variabele assets in Azure Automation

Variabele assets zijn waarden die beschikbaar voor alle runbooks en DSC-configuraties in uw automation-account zijn. Ze kunnen worden gemaakt, gewijzigd en opgehaald uit de Azure portal, Windows PowerShell en vanuit een runbook of de DSC-configuratie. Automation-variabelen zijn handig voor de volgende scenario's:

- Een waarde tussen meerdere runbooks of DSC-configuraties delen.

- Een waarde tussen meerdere taken van de DSC-configuratie of hetzelfde runbook delen.

- Een waarde beheren vanuit de portal of vanuit de Windows PowerShell-opdrachtregel die wordt gebruikt door runbooks of DSC-configuraties, zoals een set algemene configuratie-items zoals bepaalde lijst van de VM-namen, een specifieke resourcegroep, een naam voor AD-domein, enzovoort.  

Automation-variabelen worden behouden zodat ze beschikbaar blijven zelfs als het runbook of de DSC-configuratie is mislukt.  Hierdoor kunt ook een waarde worden ingesteld door een runbook dat vervolgens wordt gebruikt door een ander of wordt gebruikt door de hetzelfde runbook of de DSC-configuratie de volgende keer dat deze wordt uitgevoerd.

Wanneer een variabele is gemaakt, kunt u opgeven dat deze worden opgeslagen versleuteld.  Als een variabele is versleuteld, het veilig in Azure Automation opgeslagen wordt en wordt de waarde kan niet worden opgehaald uit de [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) cmdlet die wordt geleverd als onderdeel van de Azure PowerShell-module.  De enige manier om dat er een gecodeerde waarde kan worden opgehaald, is van de **Get-AutomationVariable** activiteit in een runbook of de DSC-configuratie.

> [!NOTE]
> Beveiligde activa in Azure Automation zijn referenties, certificaten, verbindingen en gecodeerde variabelen. Deze activa zijn versleuteld en opgeslagen in de Azure Automation met een unieke sleutel die wordt gegenereerd voor elk automation-account. Deze sleutel is versleuteld met een basiscertificaat en opgeslagen in Azure Automation. Voordat u een beveiligd bedrijfsmiddel op te slaan, wordt de sleutel voor het automation-account worden ontsleuteld met het basiscertificaat en vervolgens worden gebruikt voor het versleutelen van de asset.

## <a name="variable-types"></a>Typen variabelen

Wanneer u een variabele met de Azure portal maakt, kunt u een gegevenstype uit de vervolgkeuzelijst moet opgeven, zodat het juiste besturingselement voor het invoeren van de variabele waarde door de portal kunt weergeven. De variabele is niet beperkt tot dit gegevenstype, maar u moet de variabele met Windows PowerShell als u wilt opgeven van een waarde van een ander type instellen. Als u opgeeft **niet gedefinieerd**, wordt de waarde van de variabele wordt ingesteld op **$null**, en moet u instellen dat de waarde met de [Set AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) cmdlet of **Set-AutomationVariable** activiteit.  U kunt maken of wijzig de waarde voor een variabele voor complex type in de portal, maar u kunt een waarde van een type met Windows PowerShell opgeven. Complexe typen worden geretourneerd als een [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).

U kunt meerdere waarden in een enkele variabele opslaan door het maken van een matrix of een hashtabel en op de variabele op te slaan.

Hier volgen een lijst met variabelen die beschikbaar zijn in Automation:

* Tekenreeks
* Geheel getal
* Datum en tijd
* Boole-waarde
* Null

## <a name="scripting-the-creation-and-management-of-variables"></a>Scripts voor het maken en beheren van variabelen

De cmdlets in de volgende tabel worden gebruikt voor het maken en beheren van Automation-variabelen met Windows PowerShell. Ze worden verzonden als onderdeel van de [Azure PowerShell-module](../powershell-install-configure.md) die beschikbaar is voor gebruik in Automation-runbooks en DSC-configuratie.

|Cmdlets|Beschrijving|
|:---|:---|
|[Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx)|Haalt de waarde van een bestaande variabele.|
|[Nieuwe AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx)|Maakt een nieuwe variabele en stelt u de waarde ervan.|
|[Verwijder AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt619354.aspx)|Hiermee verwijdert u een bestaande variabele.|
|[Set-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603601.aspx)|De waarde voor een bestaande variabele ingesteld.|

De workflow-activiteiten in de volgende tabel worden gebruikt voor toegang tot Automation-variabelen in een runbook. Ze zijn alleen beschikbaar voor gebruik in een runbook of de DSC-configuratie en niet worden geleverd als onderdeel van de Azure PowerShell-module.

|Workflow-activiteiten|Beschrijving|
|:---|:---|
|Get-AutomationVariable|Haalt de waarde van een bestaande variabele.|
|Set-AutomationVariable|De waarde voor een bestaande variabele ingesteld.|

> [!NOTE] 
> Vermijd het gebruik van variabelen in de parameter-Name van **Get-AutomationVariable** in een runbook of de DSC-configuratie omdat dit detecteren van afhankelijkheden tussen runbooks of DSC-configuratie en de Automation-variabelen tijdens de ontwerpfase bemoeilijken kan.

De functies in de volgende tabel worden gebruikt voor toegang tot en het ophalen van variabelen in een runbook Python2. 

|Python2 functies|Beschrijving|
|:---|:---|
|automationassets.get_automation_variable|Haalt de waarde van een bestaande variabele. |
|automationassets.set_automation_variable|De waarde voor een bestaande variabele ingesteld. |

> [!NOTE] 
> U moet de module 'automationassets' boven aan uw runbook Python importeren om de toegang tot de functies van de asset.

## <a name="creating-a-new-automation-variable"></a>Een nieuwe automatiseringsvariabele maken

### <a name="to-create-a-new-variable-with-the-azure-portal"></a>Een nieuwe variabele maken met de Azure-portal

1. Van uw Automation-account, klikt u op de **activa** tegel en klik vervolgens op de **activa** blade Selecteer **variabelen**.
2. Op de **variabelen** tegel, selecteer **toevoegen van een variabele**.
3. Het voltooien van de opties op de **nieuwe variabele** blade en klik op **maken** opslaan van de nieuwe variabele.

### <a name="to-create-a-new-variable-with-windows-powershell"></a>Een nieuwe variabele maken met Windows PowerShell

De [nieuw AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) cmdlet maakt een nieuwe variabele en stelt u de initiële waarde. U kunt ophalen met de waarde met behulp van [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx). Als de waarde een eenvoudig type is, wordt dat hetzelfde type geretourneerd. Als het een complex type en vervolgens een **PSCustomObject** wordt geretourneerd.

De volgende voorbeeldopdrachten laten zien hoe een variabele van het type tekenreeks maken en vervolgens de waarde te retourneren.

    New-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" 
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
    –Encrypted $false –Value 'My String'
    $string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

De volgende voorbeeldopdrachten laten zien hoe een variabele maken met een complex type en vervolgens de eigenschappen ervan te retourneren. In dit geval wordt een virtuele machine object uit **Get-AzureRmVm** wordt gebruikt.

    $vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
    New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $vmValue.Name
    $vmIpAddress = $vmValue.IpAddress



## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a>Met behulp van een variabele in een runbook of de DSC-configuratie

Gebruik de **Set-AutomationVariable** activiteit is de waarde van een Automation-variabele instellen in een PowerShell-runbook of de DSC-configuratie en de **Get-AutomationVariable** te halen.  Gebruik niet de **Set AzureAutomationVariable** of **Get-AzureAutomationVariable** cmdlets in een runbook of de DSC-configuratie omdat ze minder efficiënt dan de werkstroomactiviteiten zijn.  U de waarde van beveiligde variabelen met ook niet ophalen **Get-AzureAutomationVariable**.  De enige manier om een nieuwe variabele van binnen een runbook of de DSC-configuratie maken is met de [nieuw AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx) cmdlet.


### <a name="textual-runbook-samples"></a>Tekstueel runbook-voorbeelden

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a>Instellen en een eenvoudige waarde ophalen van een variabele

De volgende voorbeeldopdrachten laten zien hoe instelt en ophaalt van een variabele in een tekstueel runbook. In dit voorbeeld wordt ervan uitgegaan dat variabelen van het type integer naam *NumberOfIterations* en *NumberOfRunnings* en een variabele van het type tekenreeks met de naam *SampleMessage* al zijn gemaakt.

    $NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a>Bijwerken en het ophalen van een complex object in een variabele

De volgende voorbeeldcode laat zien hoe een variabele met een complexe waarde in een tekstueel runbook bijwerken. In dit voorbeeld wordt een virtuele machine van Azure is opgehaald met **Get-AzureVM** en opgeslagen in een bestaand Automation-variabele.  Zoals uitgelegd in [typen variabelen](#variable-types), dit wordt opgeslagen als een PSCustomObject.

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm

De waarde is opgehaald van de variabele en gebruikt voor het starten van de virtuele machine in de volgende code.

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a>Bijwerken en het ophalen van een verzameling in een variabele

De volgende voorbeeldcode laat zien hoe het gebruik van een variabele met een verzameling complexe waarden in een tekstueel runbook. In dit voorbeeld meerdere virtuele machines in Azure worden opgehaald met **Get-AzureVM** en opgeslagen in een bestaand Automation-variabele.  Zoals uitgelegd in [typen variabelen](#variable-types), dit wordt opgeslagen als een verzameling van PSCustomObjects.

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

De verzameling is opgehaald van de variabele en gebruikt voor het starten van elke virtuele machine in de volgende code.

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }
    
#### <a name="setting-and-retrieving-a-variable-in-python2"></a>Bijwerken en het ophalen van een variabele in Python2
De volgende voorbeeldcode laat zien hoe een variabele, het instellen van een variabele en het verwerken van een uitzondering voor een niet-bestaande variabele in een runbook Python2.

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


### <a name="graphical-runbook-samples"></a>Grafische runbook-voorbeelden

In een grafisch runbook, voegt u de **Get-AutomationVariable** of **Set-AutomationVariable** door met de rechtermuisknop op de variabele in het deelvenster bibliotheek van de grafische editor en de activiteit die u wilt selecteren.

![Variabele aan canvas toevoegen](media/automation-variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a>Instellen van waarden in een variabele
De volgende afbeelding toont een voorbeeld van activiteiten voor het bijwerken van een variabele met een eenvoudige waarde in een grafisch runbook. In dit voorbeeld wordt een enkele virtuele machine van Azure is opgehaald met **Get-AzureRmVM** en naam van de computer is opgeslagen in een bestaand Automation-variabele van het type tekenreeks.  Het maakt niet uit of de [koppeling is een pijplijn of een reeks](automation-graphical-authoring-intro.md#links-and-workflow) omdat alleen we een enkel object in de uitvoer verwachten.

![Eenvoudige variabele instellen](media/automation-variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het aansluiten van activiteiten bij elkaar in grafisch ontwerpen, [koppelingen in het grafisch ontwerpen](automation-graphical-authoring-intro.md#links-and-workflow)
* Zie [Mijn eerste grafische runbook](automation-first-runbook-graphical.md) om aan de slag te gaan met grafische runbooks 

