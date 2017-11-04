---
title: Bewaken en beheren van de Stream Analytics-taken met PowerShell | Microsoft Docs
description: Informatie over het gebruik van Azure PowerShell-cmdlets en om te controleren en beheren van de Stream Analytics-taken.
keywords: Azure powershell, azure powershell-cmdlets, powershell-opdracht powershell-scripts
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 514f454e-d18c-4081-8304-ab48577e15e8
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: b8d362a2789c4e1f5594baa2b86a16e523757037
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Bewaken en beheren van de Stream Analytics-taken met Azure PowerShell-cmdlets
Informatie over het controleren en beheren van resources in de Stream Analytics met Azure PowerShell-cmdlets en powershell-scripts die basic Stream Analytics-taken uitvoeren.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Vereisten voor het uitvoeren van Azure PowerShell-cmdlets voor Stream Analytics
* Maak een Azure-resourcegroep in uw abonnement. Hier volgt een voorbeeld Azure PowerShell-script. Zie voor informatie Azure PowerShell [installeren en configureren van Azure PowerShell](/powershell/azure/overview);  

Azure PowerShell 0.9.8:  

         # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure PowerShell 1.0:  

         # Log in to your Azure account
        Login-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>



> [!NOTE]
> Stream Analytics-taken via een programma gemaakt beschikt niet over bewaking standaard ingeschakeld.  U kunt handmatig inschakelen in de Azure Portal controleren door te navigeren naar de pagina van de Monitor van de taak en te klikken op de knop inschakelen of u kunt dit programmatisch doen door de stappen die zich bevindt op [Azure Stream Analytics - Monitor Stream Analytics-taken Via programmacode](stream-analytics-monitor-jobs.md).
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Azure PowerShell-cmdlets voor Stream Analytics
De volgende Azure PowerShell-cmdlets kunnen worden gebruikt om te controleren en beheren van Azure Stream Analytics-taken. Houd er rekening mee dat Azure PowerShell verschillende versies heeft. 
**In de voorbeelden weergegeven met de eerste opdracht voor Azure PowerShell 0.9.8, is de tweede opdracht is voor Azure PowerShell 1.0.** De Azure PowerShell 1.0-opdrachten hebben altijd 'AzureRM' in de opdracht.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Informatie over de taak over een specifieke taak in de resourcegroep opgehaald of een lijst met alle Stream Analytics-taken gedefinieerd in de Azure-abonnement of de opgegeven resourcegroep.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

Deze PowerShell-opdracht retourneert informatie over alle Stream Analytics-taken in de Azure-abonnement.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Deze PowerShell-opdracht retourneert informatie over alle Stream Analytics-taken in de resourcegroep StreamAnalytics, standaard, centraal, VS.

**Voorbeeld 3**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Deze PowerShell-opdracht retourneert informatie over de Stream Analytics-taak StreamingJob in de resourcegroep StreamAnalytics, standaard, centraal, VS.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Informatie over een specifieke invoer opgehaald of een lijst met alle van de invoerwaarden die zijn gedefinieerd in een opgegeven Stream Analytics-taak.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Deze PowerShell-opdracht retourneert informatie over de invoerwaarden die zijn gedefinieerd in de taak StreamingJob.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Deze PowerShell-opdracht retourneert informatie over de invoer met de naam gedefinieerd in de taak StreamingJob EntryStream.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
Informatie over een specifieke uitvoer opgehaald of een lijst met alle van de resultaten die zijn gedefinieerd in een opgegeven Stream Analytics-taak.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Deze PowerShell-opdracht retourneert informatie over de uitvoer die is gedefinieerd in de taak StreamingJob.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Deze PowerShell-opdracht retourneert informatie over de uitvoer met de naam gedefinieerd in de taak StreamingJob uitvoer.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
Hiermee haalt u informatie over het quotum van het streaming-eenheden in een opgegeven regio.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

Deze PowerShell-opdracht retourneert informatie over de quota en het gebruik van de streaming-eenheden in de regio VS-midden.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Hiermee haalt u informatie over een specifieke transformatie gedefinieerd in een Stream Analytics-taak.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Deze PowerShell-opdracht retourneert informatie over de transformatie StreamingJob aangeroepen in de taak StreamingJob.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Nieuwe AzureStreamAnalyticsInput | Nieuwe AzureRMStreamAnalyticsInput
Maakt u een nieuwe invoer binnen een Stream Analytics-taak of updates van een bestaande opgegeven invoer.

De naam van de invoer kan worden opgegeven in het bestand .json of op de opdrachtregel. Als beide zijn opgegeven, moet de naam op de opdrachtregel hetzelfde zijn als het certificaat in het bestand.

Als u opgeeft invoer die al bestaat en geef de parameter – Force, vraagt de cmdlet al dan niet worden vervangen de bestaande invoer.

Als u de parameter forceren en geef een bestaande naam invoeren, wordt de invoer vervangen zonder bevestiging.

Raadpleeg voor gedetailleerde informatie over de JSON-bestandsstructuur en inhoud de [maken invoer (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] sectie van de [Stream Analytics Management REST API-verwijzing Bibliotheek][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Deze PowerShell-opdracht maakt een nieuwe invoer van het bestand Input.json. Als een bestaande invoer met de naam die is opgegeven in het definitiebestand voor invoer is al gedefinieerd, wordt al dan niet worden vervangen door de cmdlet gevraagd.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Deze PowerShell-opdracht maakt een nieuwe invoer in de taak EntryStream aangeroepen. Als een bestaande invoer met deze naam al is gedefinieerd, vraagt de cmdlet al dan niet worden vervangen.

**Voorbeeld 3**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Deze PowerShell-opdracht wordt vervangen door de definitie van de bestaande invoerbron EntryStream aangeroepen met de definitie uit het bestand.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Nieuwe AzureStreamAnalyticsJob | Nieuwe AzureRMStreamAnalyticsJob
Maakt een nieuwe Stream Analytics-taak in Microsoft Azure of updates van de definitie van een bestaande opgegeven taak.

De naam van de taak kan worden opgegeven in het bestand .json of op de opdrachtregel. Als beide zijn opgegeven, moet de naam op de opdrachtregel hetzelfde zijn als het certificaat in het bestand.

Als u een taaknaam opgeeft die al bestaat en geef de parameter – Force, vraagt de cmdlet al dan niet worden vervangen de bestaande taak.

Als u de parameter forceren en geef de taaknaam van een bestaande, wordt de taakdefinitie vervangen zonder bevestiging.

Raadpleeg voor gedetailleerde informatie over de JSON-bestandsstructuur en inhoud de [Stream Analytics-taak maken] [ msdn-rest-api-create-stream-analytics-job] sectie van de [Stream Analytics Management REST API Reference Library] [stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Deze PowerShell-opdracht maakt een nieuwe taak van de definitie in JobDefinition.json. Als een bestaande taak met de naam die is opgegeven in het definitiebestand voor de taak is al gedefinieerd, wordt al dan niet worden vervangen door de cmdlet gevraagd.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Deze PowerShell-opdracht vervangt de taakdefinitie voor StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Nieuwe AzureStreamAnalyticsOutput | Nieuwe AzureRMStreamAnalyticsOutput
Maakt een nieuwe uitvoer binnen een Stream Analytics-taak of een bestaande uitvoer-updates.  

De naam van de uitvoer kan worden opgegeven in het bestand .json of op de opdrachtregel. Als beide zijn opgegeven, moet de naam op de opdrachtregel hetzelfde zijn als het certificaat in het bestand.

Als u geeft een uitvoer die al bestaat en geef de parameter – Force, vraagt de cmdlet al dan niet worden vervangen de bestaande uitvoer.

Als u de parameter forceren en geef de naam van een bestaande uitvoer, wordt de uitvoer wordt vervangen zonder bevestiging.

Raadpleeg voor gedetailleerde informatie over de JSON-bestandsstructuur en inhoud de [uitvoer maken (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] sectie van de [Stream Analytics Management REST API-verwijzing Bibliotheek][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Deze PowerShell-opdracht maakt u een nieuwe uitvoer 'uitvoer' in de taak StreamingJob aangeroepen. Als een bestaande uitvoer met deze naam al is gedefinieerd, vraagt de cmdlet al dan niet worden vervangen.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Deze PowerShell-opdracht wordt de definitie voor 'uitvoer' in de taak StreamingJob vervangen.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Nieuwe AzureStreamAnalyticsTransformation | Nieuwe AzureRMStreamAnalyticsTransformation
Maakt u een nieuwe transformatie binnen een Stream Analytics-taak of updates van de bestaande transformatie.

De naam van de transformatie kan worden opgegeven in het bestand .json of op de opdrachtregel. Als beide zijn opgegeven, moet de naam op de opdrachtregel hetzelfde zijn als het certificaat in het bestand.

Als u opgeeft dat al bestaat de transformatie en geef de parameter – Force, vraagt de cmdlet al dan niet worden vervangen de bestaande transformatie.

Als u de parameter forceren en geef de naam van een bestaande transformatie, de transformatie wordt vervangen zonder bevestiging.

Raadpleeg voor gedetailleerde informatie over de JSON-bestandsstructuur en inhoud de [transformatie maken (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] sectie van de [Stream Analytics Management REST-API Referentiebibliotheek][stream.analytics.rest.api.reference].

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Deze PowerShell-opdracht maakt u een nieuwe transformatie StreamingJobTransform aangeroepen in de taak StreamingJob. Als er al een bestaande transformatie is gedefinieerd met deze naam, vraagt de cmdlet al dan niet worden vervangen.

**Voorbeeld 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 Deze PowerShell-opdracht wordt de definitie van StreamingJobTransform in de taak StreamingJob vervangen.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Verwijder AzureStreamAnalyticsInput | Verwijder AzureRMStreamAnalyticsInput
Een specifieke invoer wordt asynchroon worden verwijderd uit een Stream Analytics-taak in Microsoft Azure.  
Als u de parameter – Force, de invoer worden verwijderd zonder bevestiging.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Deze PowerShell-opdracht verwijdert de invoer EventStream in de taak StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Verwijder AzureStreamAnalyticsJob | Verwijder AzureRMStreamAnalyticsJob
Asynchroon Hiermee verwijdert u een specifieke Stream Analytics-taak in Microsoft Azure.  
Als u de parameter – Force, de taak worden verwijderd zonder bevestiging.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Deze PowerShell-opdracht verwijdert u de taak StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Verwijder AzureStreamAnalyticsOutput | Verwijder AzureRMStreamAnalyticsOutput
Een specifieke uitvoer wordt asynchroon worden verwijderd uit een Stream Analytics-taak in Microsoft Azure.  
Als u de parameter – Force, wordt de uitvoer verwijderd zonder bevestiging.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Hiermee PowerShell-opdracht verwijdert u de uitvoer van de uitvoer in de taak StreamingJob.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Start AzureStreamAnalyticsJob | Start AzureRMStreamAnalyticsJob
Asynchroon implementeert en een Stream Analytics-taak start in Microsoft Azure.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Deze PowerShell-opdracht start de taak StreamingJob met een begintijd aangepaste uitvoer ingesteld op 12 December 2012 12:12:12 UTC.

### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Stop AzureStreamAnalyticsJob | Stop AzureRMStreamAnalyticsJob
Asynchroon stopt een Stream Analytics-taak wordt uitgevoerd in Microsoft Azure en ongedaan wijst bronnen toe die zijn die zijn gebruikt. De taakdefinitie en metagegevens blijven beschikbaar in uw abonnement via de Azure-portal en de API's zodanig dat de taak kan worden bewerkt en opnieuw opgestart. U wordt niet in rekening gebracht voor een taak gestopt.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Deze PowerShell-opdracht wordt de taak StreamingJob gestopt.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Test AzureStreamAnalyticsInput | Test AzureRMStreamAnalyticsInput
Test de mogelijkheid van Stream Analytics verbinding maken met een opgegeven invoer.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Deze PowerShell-opdracht wordt de verbindingsstatus van de invoer EntryStream in StreamingJob gecontroleerd.  

### <a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Test AzureStreamAnalyticsOutput | Test AzureRMStreamAnalyticsOutput
Test de mogelijkheid van Stream Analytics verbinding maken met een opgegeven uitvoer.

**Voorbeeld 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Deze opdracht PowerShell tests de verbindingsstatus van de uitvoer van de uitvoer in StreamingJob.  

## <a name="get-support"></a>Ondersteuning krijgen
Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics). 

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

