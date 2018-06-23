---
title: Gebruik van Azure Cloud Shell LUIS weergeven | Microsoft Docs
description: Informatie over het ophalen van informatie over het gebruik in de Azure-Cloud-Shell voor LUIS.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/08/2017
ms.author: v-geberr
ms.openlocfilehash: 2de25645e5377efdd53bcc980695804d34db5ee2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345469"
---
# <a name="manage-luis-service-from-azure-cloud-shell"></a>LUIS service beheren vanuit Azure Cloud Shell
De Azure-portal kunt u PowerShell-cmdlets gebruiken om te werken met LUIS resources. 

Deze cmdlets kunt u [maken](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/new-azurermcognitiveservicesaccount?view=azurermps-6.0.0) een abonnement LUIS, get-informatie over het abonnement, met inbegrip van [gebruik](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0), en [verwijderen](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/remove-azurermcognitiveservicesaccount?view=azurermps-6.0.0) het abonnement. 

## <a name="cloud-shell-storage-account-and-authentication"></a>Cloud-shell storage-account en -verificatie
Als u wilt gebruiken van PowerShell in de Azure portal [cloud shell](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell), moet u beschikken over een Azure storage-account. Als u hebt een [opslagaccount](https://docs.microsoft.com/en-us/azure/cloud-shell/persisting-shell-storage#set-up-a-clouddrive-file-share), wordt u gevraagd een maken. Het opslagaccount kunt u voor het opslaan van de PowerShell-scripts in de cloud-shell.  

Ook moet u verificatie op Azure in de cloud-shell toegang tot bronnen. 

Wanneer u een opslagaccount hebt en worden geverifieerd, kunt u PowerShell-cmdlets uitvoeren.

## <a name="open-cloud-shell"></a>Cloud Shell openen
Wanneer u de Azure portal-cloud-shell, bent u altijd op de meest recente versie van PowerShell. 

Gebruik de **Cloud Shell starten** knop openen van de Cloud-Shell of open een browser met [ https://shell.azure.com ](https://shell.azure.com). 

<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>

## <a name="luis-endpoint-usage-information"></a>LUIS informatie over het gebruik van het eindpunt

De PowerShell-cmdlet 6.x `Get-AzureRmCognitiveServicesAccountUsage`, biedt informatie over het gebruik van Microsoft cognitieve Services inclusief LUIS. [Get-AzureRmCognitiveServicesAccountUsage](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0) moet u de resourcegroep en de naam van de bron van de service. 

De syntaxis van de opdracht is:

```
Get-AzureRmCognitiveServicesAccountUsage -ResourceGroupName my-resource-group -Name my-luis-service-name
```

In het volgende voorbeeld wordt de naam van de resourcegroep is `luis-westus-rg` en de naam van de LUIS service-abonnement is `luis-westus-1`. Deze beide namen worden gekozen wanneer de service LUIS wordt gemaakt. 

De cmdlet retourneert informatie over het gebruik van 16 van 10.000 eindpunt treffers in een periode van 30 dagen met de periode die eindigt op 7 juni gebruikt:

```
CurrentValue  : 16
Name          : LUIS.Calls
Limit         : 10000
Status        : Included
Unit          : Count
QuotaPeriod   : 30.00:00:00
NextResetTime : 2018-06-07T18:28:52Z
```

De opdracht Opslaan als een PowerShell-bestand *.ps1 in de Azure storage-account die is gekoppeld aan de cloud-shell en op elk gewenst moment worden uitgevoerd. 

![Script uitvoeren vanaf de opslag](./media/luis-how-to-manage-from-powershell/run-script-from-storage.png)

Wanneer het script is opgeslagen op de schijf van de cloud, kunt u het PowerShell-script uitvoeren van de Azure cloud-shell phone-app. 

![Script uitvoeren vanaf de opslag in telefoonapp](./media/luis-how-to-manage-from-powershell/phone-app.png)