---
title: Een cloud service-container maken met PowerShell | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u een cloud service-container maken met PowerShell. De container fungeert als host voor web-en werkrollen.
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: 
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: b55a0dd7800448c50897af784092b4a60fa7a25e
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Een Azure PowerShell-opdracht gebruiken om te maken van een leeg cloud service-container
In dit artikel wordt uitgelegd hoe u snel een Cloud-Services en container te maken met Azure PowerShell-cmdlets. Volg de volgende stappen uit:

1. Installeer de Microsoft Azure PowerShell-cmdlet uit de [Azure PowerShell downloadt](http://aka.ms/webpi-azps) pagina.
2. Open de PowerShell-opdrachtprompt.
3. Gebruik de [Add-AzureAccount](/powershell/module/Azure/Add-AzureAccount?view=azuresmps-4.0.0) aan te melden.

   > [!NOTE]
   > Raadpleeg voor verdere instructies over het installeren van de Azure PowerShell-cmdlet en verbinding maken met uw Azure-abonnement [installeren en configureren van Azure PowerShell](/powershell/azure/overview).
   >
   >
4. Gebruik de **New-AzureService** cmdlet voor het maken van een leeg Azure cloud service-container.

   ```powershell
   New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. Ga als volgt in dit voorbeeld voor het aanroepen van de cmdlet:

   ```powershell
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Voor meer informatie over het maken van de Azure-cloud-service worden uitgevoerd:

```
Get-help New-AzureService
```

### <a name="next-steps"></a>Volgende stappen
* Voor het beheren van de cloud service-implementatie, verwijzen naar de [Get-AzureService](/powershell/module/Azure/Get-AzureService?view=azuresmps-4.0.0), [Remove-AzureService](/powershell/module/Azure/Remove-AzureService?view=azuresmps-4.0.0), en [Set-AzureService](/powershell/module/azure/set-azureservice?view=azuresmps-4.0.0) opdrachten. U kunt ook verwijzen naar [het configureren van cloudservices](cloud-services-how-to-configure-portal.md) voor meer informatie.
* Als u wilt uw cloudserviceproject publiceren naar Azure, verwijzen naar de **PublishCloudService.ps1** codevoorbeeld van [gearchiveerde cloud services-opslagplaats](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Scripts/cloud-services-continuous-delivery).
