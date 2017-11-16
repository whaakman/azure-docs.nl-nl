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
ms.openlocfilehash: cd703feb7bf5af765fc3a5448464499aa7b48d6a
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Een Azure PowerShell-opdracht gebruiken om te maken van een leeg cloud service-container
In dit artikel wordt uitgelegd hoe u snel een Cloud-Services en container te maken met Azure PowerShell-cmdlets. Volg de volgende stappen uit:

1. Installeer de Microsoft Azure PowerShell-cmdlet uit de [Azure PowerShell downloadt](http://aka.ms/webpi-azps) pagina.
2. Open de PowerShell-opdrachtprompt.
3. Gebruik de [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) aan te melden.

   > [!NOTE]
   > Raadpleeg voor verdere instructies over het installeren van de Azure PowerShell-cmdlet en verbinding maken met uw Azure-abonnement [installeren en configureren van Azure PowerShell](/powershell/azure/overview).
   >
   >
4. Gebruik de **New-AzureService** cmdlet voor het maken van een leeg Azure cloud service-container.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. Ga als volgt in dit voorbeeld voor het aanroepen van de cmdlet:

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Voor meer informatie over het maken van de Azure-cloud-service worden uitgevoerd:

```
Get-help New-AzureService
```

### <a name="next-steps"></a>Volgende stappen
* Voor het beheren van de cloud service-implementatie, verwijzen naar de [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), en [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) opdrachten. U kunt ook verwijzen naar [het configureren van cloudservices](cloud-services-how-to-configure-portal.md) voor meer informatie.
* Als u wilt uw cloudserviceproject publiceren naar Azure, verwijzen naar de **PublishCloudService.ps1** codevoorbeeld van [continue leveringsmethode voor de cloudservice in Azure](cloud-services-dotnet-continuous-delivery.md).
