---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.openlocfilehash: c6666f4417cde9e0f77cc965ded1d6bdb5dced34
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66165823"
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

