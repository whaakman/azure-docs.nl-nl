---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 4e262c9e5bb88e77bc9c09853c06f4cdb41eedaa
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58890924"
---
## <a name="preparing-for-updates"></a>Voorbereiden voor updates
U moet de volgende stappen uitvoeren voordat u het scannen en de update toepassen:

1. Maakt u een cloudmomentopname van de apparaatgegevens.
2. Zorg ervoor dat de controller vaste IP-adressen routeerbaar zijn en verbinding kan maken met Internet. Deze vaste IP-adressen wordt gebruikt om updates op uw apparaat. U kunt dit testen door de volgende cmdlet uit de Windows PowerShell-interface van het apparaat op elke domeincontroller:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network>`
   
    **Voorbeelduitvoer voor Test-Connection wanneer vaste IP-adressen verbinding met Internet maken kunt**

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

Nadat u deze handmatig controles van systeemtabel hebt voltooid, kunt u doorgaan om te scannen en de updates worden geïnstalleerd.

