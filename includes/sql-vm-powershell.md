---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.openlocfilehash: 3dc799ecc75589279c8d1c73062a8f2157761330
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56213000"
---
## <a name="start-your-powershell-session"></a>Een PowerShell-sessie starten
 

Voer de [ **Connect-Az Account** ](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount) -cmdlet en u krijgt een aanmeldingsscherm uw referenties in te voeren. Gebruik de referenties waarmee u zich aanmeldt bij de Azure-portal.

```powershell
Connect-AzAccount
```

Als u meerdere abonnementen gebruiken de [ **Set AzContext** ](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext) cmdlet om te selecteren welk abonnement uw PowerShell-sessie wilt gebruiken. Als u wilt zien welk abonnement de huidige PowerShell-sessie wordt gebruikt, [ **Get-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/get-azcontext). Als u wilt zien van al uw abonnementen, [ **Get-AzSubscription**](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription).

```powershell
Set-AzContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```

