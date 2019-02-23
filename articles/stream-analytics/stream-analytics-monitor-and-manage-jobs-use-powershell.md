---
title: Controleren en beheren van Azure Stream Analytics-taken met behulp van PowerShell
description: In dit artikel wordt beschreven hoe u Azure PowerShell en cmdlets gebruiken om te controleren en beheren van Azure Stream Analytics-taken.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 2755064190e787baa31ea0da7c375365fa6c29b5
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56733330"
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Controleren en beheren van Stream Analytics-taken met Azure PowerShell-cmdlets
Informatie over het controleren en beheren van Stream Analytics-resources met Azure PowerShell-cmdlets en powershell-scripts die eenvoudige Stream Analytics-taken uitvoeren.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Vereisten voor het uitvoeren van Azure PowerShell-cmdlets voor Stream Analytics
* Maak een Azure-resourcegroep in uw abonnement. Hier volgt een voorbeeld van Azure PowerShell-script. Zie voor meer informatie voor Azure PowerShell, [installeren en configureren van Azure PowerShell](/powershell/azure/overview);  

Azure PowerShell 0.9.8 gebruikt:  

```powershell
# Log in to your Azure account
Add-AzureAccount
# Select the Azure subscription you want to use to create the resource group if you have more han one subscription on your account.
Select-AzureSubscription -SubscriptionName <subscription name>
# If Stream Analytics has not been registered to the subscription, remove remark symbol below (#)to run the Register-AzureProvider cmdlet to register the provider namespace.
#Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
# Create an Azure resource group
New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
```

Azure PowerShell 1.0:

```powershell
# Log in to your Azure account
Connect-AzAccount
# Select the Azure subscription you want to use to create the resource group.
Get-AzSubscription –SubscriptionName "your sub" | Select-AzSubscription
# If Stream Analytics has not been registered to the subscription, remove remark symbol below (#)to run the Register-AzureProvider cmdlet to register the provider namespace.
#Register-AzResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
# Create an Azure resource group
New-AzResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
```


> [!NOTE]
> Stream Analytics-taken die zijn gemaakt via een programma geen bewaking standaard ingeschakeld.  U kunt handmatig inschakelen in Azure Portal controleren door te navigeren naar pagina van de Monitor van de taak en te klikken op de knop inschakelen of u kunt dit programmatisch doen met de volgende stappen te vinden op [Azure Stream Analytics - Monitor voor Stream Analytics-taken Programmatisch](stream-analytics-monitor-jobs.md).
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Azure PowerShell-cmdlets voor Stream Analytics
De volgende Azure PowerShell-cmdlets kan worden gebruikt om te controleren en beheren van Azure Stream Analytics-taken. Houd er rekening mee dat Azure PowerShell, zijn er verschillende versies. 
**De eerste opdracht in de voorbeelden is Azure PowerShell 0.9.8 gebruikt, de tweede opdracht is voor Azure PowerShell 1.0.** De Azure PowerShell 1.0-opdrachten hebben altijd 'AzureRM' in de opdracht.

### <a name="get-azurestreamanalyticsjob--get-azstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzStreamAnalyticsJob
Een lijst met alle Stream Analytics-taken gedefinieerd in de Azure-abonnement of de opgegeven resourcegroep gemaakt of taakinformatie ophalen over een specifieke taak binnen een resourcegroep.

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Get-AzureStreamAnalyticsJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsJob
```

Deze PowerShell-opdracht retourneert informatie over alle Stream Analytics-taken in de Azure-abonnement.

**Voorbeeld 2**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 
```

Deze PowerShell-opdracht retourneert informatie over alle Stream Analytics-taken in de resourcegroep StreamAnalytics-standaard-centraal-Verenigde Staten.

**Voorbeeld 3**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob
```

Deze PowerShell-opdracht retourneert informatie over de Stream Analytics-taak StreamingJob in de resourcegroep StreamAnalytics-standaard-centraal-Verenigde Staten.

### <a name="get-azurestreamanalyticsinput--get-azstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzStreamAnalyticsInput
Een lijst met alle van de invoerwaarden die zijn gedefinieerd in een opgegeven Stream Analytics-taak, of informatie ophalen over een specifieke invoer.

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Deze PowerShell-opdracht retourneert informatie over de invoer die zijn gedefinieerd in de taak StreamingJob.

**Voorbeeld 2**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream
```

Deze PowerShell-opdracht retourneert informatie over de invoer met de naam gedefinieerd in de taak StreamingJob EntryStream.

### <a name="get-azurestreamanalyticsoutput--get-azstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzStreamAnalyticsOutput
Een lijst met alle van de uitvoer die zijn gedefinieerd in een opgegeven Stream Analytics-taak, of informatie ophalen over een specifieke uitvoer.

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Deze PowerShell-opdracht retourneert informatie over de uitvoer die is gedefinieerd in de taak StreamingJob.

**Voorbeeld 2**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output
```

Deze PowerShell-opdracht retourneert informatie over de uitvoer met de naam gedefinieerd in de taak StreamingJob uitvoer.

### <a name="get-azurestreamanalyticsquota--get-azstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzStreamAnalyticsQuota
Hiermee haalt u informatie over het quotum van het streaming-eenheden in een opgegeven regio.

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Get-AzureStreamAnalyticsQuota –Location "Central US" 
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsQuota –Location "Central US" 
```

Deze PowerShell-opdracht retourneert informatie over de quota en het gebruik van streaming-eenheden in de regio VS-midden.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Hiermee haalt u informatie over een specifieke transformatie gedefinieerd in een Stream Analytics-taak.

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob
```

Deze PowerShell-opdracht retourneert informatie over de transformatie StreamingJob in de taak StreamingJob genoemd.

### <a name="new-azurestreamanalyticsinput--new-azstreamanalyticsinput"></a>New-AzureStreamAnalyticsInput | New-AzStreamAnalyticsInput
Hiermee maakt u een nieuwe invoer binnen een Stream Analytics-taak of een bestaande opgegeven invoer-updates.

De naam van de invoer kan worden opgegeven in het .json-bestand of op de opdrachtregel. Als beide zijn opgegeven, de naam op de opdrachtregel moet hetzelfde zijn als de waarde in het bestand.

Als u opgeven van de invoer die al bestaat en geef de parameter – Force, de cmdlet wordt u gevraagd of u wel of niet ter vervanging van de bestaande invoer.

Als u de parameter Force en geef de naam van een bestaande invoer, wordt de invoer vervangen zonder bevestiging.

Raadpleeg voor gedetailleerde informatie over de structuur van de JSON-bestand en de inhoud de [invoer maken (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] sectie van de [Stream Analytics Management REST API-verwijzing Bibliotheek][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 
```

Deze PowerShell-opdracht maakt een nieuwe invoer van het bestand Input.JSON ziet. Als een bestaande invoer met de naam die is opgegeven in het definitiebestand van de invoer is al gedefinieerd, wordt al dan niet om deze te vervangen door de cmdlet gevraagd.

**Voorbeeld 2**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream
```

Deze PowerShell-opdracht maakt u een nieuwe invoer in de taak met de naam EntryStream. Als een bestaande invoer met deze naam al is gedefinieerd, wordt al dan niet om deze te vervangen door de cmdlet gevraagd.

**Voorbeeld 3**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force
```

Deze PowerShell-opdracht vervangt de definitie van de bestaande invoerbron EntryStream aangeroepen met de definitie uit het bestand.

### <a name="new-azurestreamanalyticsjob--new-azstreamanalyticsjob"></a>New-AzureStreamAnalyticsJob | New-AzStreamAnalyticsJob
Hiermee maakt u een nieuwe Stream Analytics-taak in Microsoft Azure of updates van de definitie van een bestaande taak opgegeven.

De naam van de taak kan worden opgegeven in het .json-bestand of op de opdrachtregel. Als beide zijn opgegeven, de naam op de opdrachtregel moet hetzelfde zijn als de waarde in het bestand.

Als u opgeven de taaknaam van een die al bestaat en geef de parameter – Force, de cmdlet wordt u gevraagd of u wel of niet ter vervanging van de bestaande taak.

Als u de parameter Force en geeft u de taaknaam van een bestaande, wordt de taakdefinitie vervangen zonder bevestiging.

Raadpleeg voor gedetailleerde informatie over de structuur van de JSON-bestand en de inhoud de [Stream Analytics-taak maken] [ msdn-rest-api-create-stream-analytics-job] sectie van de [Stream Analytics Management REST API Reference Library] [stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 
```

Deze PowerShell-opdracht maakt een nieuwe taak van de definitie van JobDefinition.json. Als een bestaande taak met de naam die is opgegeven in het definitiebestand van de taak is al gedefinieerd, wordt al dan niet om deze te vervangen door de cmdlet gevraagd.

**Voorbeeld 2**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force
```

Deze PowerShell-opdracht vervangt de taakdefinitie voor StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azstreamanalyticsoutput"></a>New-AzureStreamAnalyticsOutput | New-AzStreamAnalyticsOutput
Hiermee maakt u een nieuwe uitvoer in een Stream Analytics-taak, of de uitvoer van een bestaande bijgewerkt.  

De naam van de uitvoer kan worden opgegeven in het .json-bestand of op de opdrachtregel. Als beide zijn opgegeven, de naam op de opdrachtregel moet hetzelfde zijn als de waarde in het bestand.

Als u geeft een uitvoer die er al bestaat en geef de parameter – Force, de cmdlet wordt u gevraagd of u wel of niet ter vervanging van de bestaande uitvoer.

Als u de parameter Force en geef de naam van een bestaande uitvoer, wordt de uitvoer vervangen zonder bevestiging.

Raadpleeg voor gedetailleerde informatie over de structuur van de JSON-bestand en de inhoud de [uitvoer maken (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] sectie van de [Stream Analytics Management REST API-verwijzing Bibliotheek][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output
```

Deze PowerShell-opdracht maakt u een nieuwe uitvoer met de naam 'uitvoer' in de taak StreamingJob. Als een bestaande uitvoer met deze naam al is gedefinieerd, wordt al dan niet om deze te vervangen door de cmdlet gevraagd.

**Voorbeeld 2**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force
```

Deze PowerShell-opdracht vervangt de definitie voor 'uitvoer' in de taak StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azstreamanalyticstransformation"></a>New-AzureStreamAnalyticsTransformation | New-AzStreamAnalyticsTransformation
Hiermee maakt u een nieuwe transformatie binnen een Stream Analytics-taak of updates van de bestaande transformatie.

De naam van de transformatie kan worden opgegeven in het .json-bestand of op de opdrachtregel. Als beide zijn opgegeven, de naam op de opdrachtregel moet hetzelfde zijn als de waarde in het bestand.

Als u opgeven van een transformatie die al bestaat en geef de parameter – Force, de cmdlet wordt u gevraagd of u wel of niet ter vervanging van de bestaande transformatie.

Als u de parameter Force en geeft u de naam van een bestaande transformatie, de transformatie wordt vervangen zonder bevestiging.

Raadpleeg voor gedetailleerde informatie over de structuur van de JSON-bestand en de inhoud de [transformatie maken (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] sectie van de [Stream Analytics Management REST-API Referentiebibliotheek][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform
```

Deze PowerShell-opdracht maakt een nieuwe transformatie StreamingJobTransform in de taak StreamingJob genoemd. Als een bestaande transformatie is al gedefinieerd met deze naam, vraagt de cmdlet al dan niet om deze te vervangen.

**Voorbeeld 2**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force
```

 Deze PowerShell-opdracht vervangt de definitie van StreamingJobTransform in de taak StreamingJob.

### <a name="remove-azurestreamanalyticsinput--remove-azstreamanalyticsinput"></a>Remove-AzureStreamAnalyticsInput | Remove-AzStreamAnalyticsInput
Asynchroon Hiermee verwijdert u een specifieke invoer van een Stream Analytics-taak in Microsoft Azure.  
Als u de parameter – Force, de invoer worden verwijderd zonder bevestiging.

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream
```

Azure PowerShell 1.0:  

```powershell
Remove-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream
```

Deze PowerShell-opdracht verwijdert de invoer EventStream in de taak StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azstreamanalyticsjob"></a>Remove-AzureStreamAnalyticsJob | Remove-AzStreamAnalyticsJob
Hiermee verwijdert u een specifieke Stream Analytics-taak in Microsoft Azure asynchroon.  
Als u de parameter – Force, de taak wordt verwijderd zonder bevestiging.

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 
```

Azure PowerShell 1.0:  

```powershell
Remove-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 
```

Deze PowerShell-opdracht verwijdert u de taak StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azstreamanalyticsoutput"></a>Remove-AzureStreamAnalyticsOutput | Remove-AzStreamAnalyticsOutput
Asynchroon Hiermee verwijdert u een specifieke uitvoer van een Stream Analytics-taak in Microsoft Azure.  
Als u de parameter – Force, de uitvoer worden verwijderd zonder bevestiging.

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output
```

Azure PowerShell 1.0:  

```powershell
Remove-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output
```

In dit PowerShell-opdracht verwijdert de uitvoer van de taak StreamingJob uitvoer.  

### <a name="start-azurestreamanalyticsjob--start-azstreamanalyticsjob"></a>Start-AzureStreamAnalyticsJob | Start-AzStreamAnalyticsJob
Asynchroon implementeert en start u een Stream Analytics-taak in Microsoft Azure.

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z
```

Azure PowerShell 1.0:  

```powershell
Start-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z
```

Deze PowerShell-opdracht start de taak StreamingJob met een begintijd aangepaste uitvoer ingesteld op 12 December 2012 12:12:12 UTC.

### <a name="stop-azurestreamanalyticsjob--stop-azstreamanalyticsjob"></a>Stop-AzureStreamAnalyticsJob | Stop-AzStreamAnalyticsJob
Asynchroon stopt een Stream Analytics-taak wordt uitgevoerd in Microsoft Azure en wijst er resources zijn die zijn gebruikt ongedaan maken. De taakdefinitie en de metagegevens blijven beschikbaar binnen uw abonnement via de Azure-portal en de beheer-API's, zodat de taak kan worden bewerkt en opnieuw opgestart. U, wordt niet in rekening gebracht voor een taak de status ' gestopt '.

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 
```

Azure PowerShell 1.0:  

```powershell
Stop-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 
```

Deze PowerShell-opdracht wordt de taak StreamingJob gestopt.  

### <a name="test-azurestreamanalyticsinput--test-azstreamanalyticsinput"></a>Test-AzureStreamAnalyticsInput | Test-AzStreamAnalyticsInput
Test de mogelijkheid van Stream Analytics om verbinding maken met een opgegeven invoer.

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream
```

Azure PowerShell 1.0:  

```powershell
Test-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream
```

Deze PowerShell-opdracht wordt de status van de verbinding van de invoer EntryStream in StreamingJob gecontroleerd.  

### <a name="test-azurestreamanalyticsoutput--test-azstreamanalyticsoutput"></a>Test-AzureStreamAnalyticsOutput | Test-AzStreamAnalyticsOutput
Test de mogelijkheid van Stream Analytics om verbinding maken met een opgegeven uitvoer.

**Voorbeeld 1**

Azure PowerShell 0.9.8 gebruikt:  

```powershell
Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output
```

Azure PowerShell 1.0:  

```powershell
Test-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output
```

In dit PowerShell-opdracht tests de status van de verbinding van de uitvoer uitvoer in StreamingJob.  

## <a name="get-support"></a>Ondersteuning krijgen
Voor verdere ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics). 

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: https://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301

