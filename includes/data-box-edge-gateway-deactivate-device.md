---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: aebb82690a7a49aba071ed64349d37d516208cca
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57555038"
---
Als u wilt uw apparaat opnieuw instelt, moet u alle gegevens op de gegevensschijf en de opstartschijf van uw apparaat veilig wissen. 

Gebruik de `Reset-HcsAppliance` cmdlet om te wissen uit de gegevensschijven en de opstartschijf of alleen de gegevensschijven. De `ClearData` en `BootDisk` switches kunnen u de gegevensschijven en de opstartschijf respectievelijk wissen.

Als u het apparaat opnieuw instellen in de lokale web-UI, alleen de gegevensschijven veilig worden gewist, maar de opstartschijf blijft intact. De opstartschijf bevat de apparaatconfiguratie.

1. [Verbinding maken met de PowerShell-interface](#connect-to-the-powershell-interface).
2. Typ in de opdrachtprompt:

    `Reset-HcsAppliance -ClearData -BootDisk`

    Het volgende voorbeeld laat zien hoe het gebruik van deze cmdlet:
    ```
    [10.128.24.33]: PS>Reset-HcsAppliance -ClearData -BootDisk
    
    Confirm
    Are you sure you want to perform this action?
    Performing the operation "Reset-HcsAppliance" on target "ShouldProcess appliance".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"): N
    ```
