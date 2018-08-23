---
title: Een cloud service-container maken met PowerShell | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u een cloud service-container maken met PowerShell. De container als host fungeert voor web-en werkrollen.
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: ''
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: 0a05b4fadf2cd7e794680907d39f355882a4330c
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42059097"
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Een Azure PowerShell-opdracht gebruiken om te maken van een lege cloudservicecontainer
In dit artikel wordt uitgelegd hoe u snel maken van een Cloud Services-container met behulp van Azure PowerShell-cmdlets. Volg de onderstaande stappen:

1. Installeer de Microsoft Azure PowerShell-cmdlet uit de [Azure PowerShell downloadt](http://aka.ms/webpi-azps) pagina.
2. Open de PowerShell-opdrachtprompt.
3. Gebruik de [Add-AzureAccount](/powershell/module/servicemanagement/azure/add-azureaccount?view=azuresmps-4.0.0) aan te melden.

   > [!NOTE]
   > Raadpleeg voor meer instructies over het installeren van de Azure PowerShell-cmdlet en verbinding te maken met uw Azure-abonnement [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).
   >
   >
4. Gebruik de **New-AzureService** cmdlet om een lege Azure cloud service-container te maken.

   ```powershell
   New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. Ga als volgt in dit voorbeeld voor het aanroepen van de cmdlet:

   ```powershell
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Voor meer informatie over het maken van de Azure-cloud-service, voert u de volgende uit:

```
Get-help New-AzureService
```

### <a name="next-steps"></a>Volgende stappen
* Voor het beheren van de cloud service-implementatie, verwijzen naar de [Get-AzureService](/powershell/module/servicemanagement/azure/Get-AzureService?view=azuresmps-4.0.0), [Remove-AzureService](/powershell/module/servicemanagement/azure/Remove-AzureService?view=azuresmps-4.0.0), en [Set-AzureService](/powershell/module/servicemanagement/azure/set-azureservice?view=azuresmps-4.0.0) opdrachten. U kunt ook verwijzen naar [cloudservices configureren](cloud-services-how-to-configure-portal.md) voor meer informatie.
* Voor het publiceren van uw cloud service-project naar Azure, raadpleegt u de **PublishCloudService.ps1** codevoorbeeld van [gearchiveerde cloud services-opslagplaats](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Scripts/cloud-services-continuous-delivery).
