---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 880b630ae48eda086f6454f0d7108d27d3403b77
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176703"
---
Als u in een niet-DHCP-omgeving opstarten, volgt u deze stappen voor het implementeren van de virtuele machine voor uw Data Box-Gateway.

1. [Verbinding maken met de Windows PowerShell-interface van het apparaat](#connect-to-the-powershell-interface).
2. Gebruik de `Get-HcsIpAddress` cmdlet om de netwerkinterfaces die zijn ingeschakeld op uw virtuele apparaat weer te geven. Als voor uw apparaat één netwerkinterface is ingeschakeld, wordt `Ethernet` als standaardnaam aan deze interface toegewezen.

    Het volgende voorbeeld ziet u het gebruik van deze cmdlet:

    ```
    [10.100.10.10]: PS>Get-HcsIpAddress

    OperationalStatus : Up
    Name              : Ethernet
    UseDhcp           : True
    IpAddress         : 10.100.10.10
    Gateway           : 10.100.10.1
    ```

3. Gebruik de cmdlet `Set-HcsIpAddress` om het netwerk te configureren. Zie het volgende voorbeeld:

    ```
    Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1
    ```

