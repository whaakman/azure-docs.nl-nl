---
title: Redis cli gebruiken met Azure Redis-Cache | Microsoft Docs
description: Informatie over het gebruik van redis cli met Azure Redis-Cache.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: wesmc
ms.openlocfilehash: facc3e2079c8f5e19266f5379762d71754a6ed84
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32182589"
---
# <a name="how-to-use-the-redis-command-line-tool-with-azure-redis-cache"></a>Het opdrachtregelprogramma Redis gebruiken met Azure Redis-Cache

*redis cli.exe* is een populair opdrachtregelprogramma voor interactie met een Redis-Cache als een client. Dit hulpprogramma is ook beschikbaar voor gebruik met Azure Redis-Cache.

Het hulpprogramma is beschikbaar voor Windows-platforms downloaden de [Redis-opdrachtregelprogramma's voor Windows](https://github.com/MSOpenTech/redis/releases/). 

Als u het opdrachtregelprogramma uitvoeren op een ander platform wilt, downloadt u Redis-Cache van [ http://redis.io/download ](https://redis.io/download).

## <a name="gather-cache-access-information"></a>Cache toegangsinformatie verzamelen

U kunt de informatie die nodig is voor toegang tot de cache met behulp van drie methoden verzamelen:

1. Met behulp van Azure CLI [az redis lijst-sleutels](https://docs.microsoft.com/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys)
2. Met behulp van Azure PowerShell [Get-AzureRmRedisCacheKey](https://docs.microsoft.com/powershell/module/azurerm.rediscache/Get-AzureRmRedisCacheKey?view=azurermps-4.4.1)
3. Met behulp van de Azure-portal.

In deze sectie haalt u de sleutels uit de Azure-portal.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-access-for-redis-cliexe"></a>Toegang voor redis-cli.exe

Met Azure Redis-Cache, alleen de SSL-poort (6380) is standaard ingeschakeld. De `redis-cli.exe` opdrachtregelprogramma biedt geen ondersteuning voor SSL. Hebt u twee configuratieopties om deze te gebruiken:

1. [Schakel de niet-SSL-poort (6379)](cache-configure.md#access-ports) - **deze configuratie wordt niet aanbevolen** omdat in deze configuratie de sneltoetsen worden verzonden via TCP in ongecodeerde tekst. Deze wijziging kan toegang tot uw cache vormen. Het enige scenario waarin u kunt overwegen om deze configuratie is wanneer u alleen toegang een test-cache tot.

2. Download en installeer [stunnel](https://www.stunnel.org/downloads.html).

    Voer **stunnel GUI Start** starten van de server.

    Met de rechtermuisknop op het pictogram voor de server stunnel in de taakbalk en klikt u op **logboek-venster weergeven**.

    Klik op het menu venster Logboek stunnel **configuratie** > **-configuratie bewerken** om de huidige configuratiebestand te openen.

    Voeg de volgende vermelding voor *redis cli.exe* onder de **Service definities** sectie. Invoegen in plaats van de Cachenaam van uw werkelijke `yourcachename`. 

    ```
    [redis-cli]
    client = yes
    accept = 127.0.0.1:6380
    connect = yourcachename.redis.cache.windows.net:6380
    ```

    Opslaan en sluiten van het configuratiebestand. 
  
    Klik op het menu venster Logboek stunnel **configuratie** > **opnieuw laden configuratie**.


## <a name="connect-using-the-redis-command-line-tool"></a>Verbinding maken met het opdrachtregelprogramma Redis.

Als u stunnel, *redis cli.exe*, en geef alleen uw *poort*, en *toegangssleutel* (primair of secundair) verbinding maken met de cache.

```
redis-cli.exe -p 6380 -a YourAccessKey
```

![stunnel met redis cli](media/cache-how-to-redis-cli-tool/cache-redis-cli-stunnel.png)

Als u een test-cache met de **onbeveiligde** niet-SSL-poort, voer `redis-cli.exe` en geef uw *hostnaam*, *poort*, en *toegangssleutel*(primair of secundair) verbinding maken met de test-cache.

```
redis-cli.exe -h yourcachename.redis.cache.windows.net -p 6379 -a YourAccessKey
```

![stunnel met redis cli](media/cache-how-to-redis-cli-tool/cache-redis-cli-non-ssl.png)




## <a name="next-steps"></a>Volgende stappen

Meer informatie over het gebruik van de [Redis-Console](cache-configure.md#redis-console) opdrachten geven.

