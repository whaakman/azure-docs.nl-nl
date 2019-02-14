---
title: Over het gebruik van redis cli met Azure Cache voor Redis | Microsoft Docs
description: Meer informatie over het gebruik van redis cli met Azure Cache voor Redis.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: yegu
ms.openlocfilehash: 318d02f5da816ae8fe2fe199b9c87b3748d5d1fc
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237566"
---
# <a name="how-to-use-the-redis-command-line-tool-with-azure-cache-for-redis"></a>Het opdrachtregelprogramma Redis gebruiken met Azure Cache voor Redis

*redis-cli.exe* is een populaire opdrachtregelprogramma voor interactie met een Azure-Cache voor Redis als een client. Dit hulpprogramma is ook beschikbaar voor gebruik met Azure Cache voor Redis.

Het hulpprogramma is beschikbaar voor Windows-platforms door het downloaden van de [Redis-opdrachtregelprogramma's voor Windows](https://github.com/MSOpenTech/redis/releases/). 

Als u het opdrachtregelprogramma uitvoeren op een ander platform wilt, Azure Cache voor Redis van downloaden [ https://redis.io/download ](https://redis.io/download).

## <a name="gather-cache-access-information"></a>Toegang tot de cache-informatie verzamelen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt de informatie die nodig is voor toegang tot de cache met behulp van drie methoden verzamelen:

1. Met behulp van Azure CLI [az redis list-keys](https://docs.microsoft.com/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys)
2. Met behulp van Azure PowerShell [Get-AzRedisCacheKey](https://docs.microsoft.com/powershell/module/az.rediscache/Get-AzRedisCacheKey)
3. Met behulp van de Azure-portal.

In deze sectie haalt u de sleutels van de Azure-portal.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-access-for-redis-cliexe"></a>Toegang inschakelen voor redis-cli.exe

Met Azure Cache voor Redis, alleen de SSL-poort (6380 zijn) is standaard ingeschakeld. De `redis-cli.exe` opdrachtregel-hulpprogramma biedt geen ondersteuning voor SSL. Hebt u twee configuratieopties om deze te gebruiken:

1. [Schakel de niet-SSL-poort (6379)](cache-configure.md#access-ports) - **deze configuratie wordt niet aanbevolen** omdat in deze configuratie worden de toegangssleutels worden verzonden via TCP in niet-versleutelde tekst. Deze wijziging kan toegang tot uw cache in gevaar komen. Het enige scenario waarin u kunt overwegen om deze configuratie is als u alleen toegang een test-cache tot.

2. Download en installeer [verbinden met stunnel](https://www.stunnel.org/downloads.html).

    Voer **verbinden met stunnel GUI Start** om de server te starten.

    Met de rechtermuisknop op het pictogram voor het verbinden met stunnelserver op de taakbalk en klikt u op **venster Logboek weergeven**.

    Klik op het menu van het logboekvenster verbinden met stunnel **configuratie** > **-configuratie bewerken** om de huidige configuratiebestand te openen.

    Voeg de volgende vermelding voor *redis-cli.exe* onder de **Service definities** sectie. Voeg de Cachenaam van uw werkelijke in plaats van `yourcachename`. 

    ```
    [redis-cli]
    client = yes
    accept = 127.0.0.1:6380
    connect = yourcachename.redis.cache.windows.net:6380
    ```

    Opslaan en sluiten van het configuratiebestand. 
  
    Klik op het menu van het logboekvenster verbinden met stunnel **configuratie** > **opnieuw laden configuratie**.


## <a name="connect-using-the-redis-command-line-tool"></a>Verbinding maken met het opdrachtregelprogramma van Redis.

Als u beveiligde tunnel, *redis-cli.exe*, en geef alleen uw *poort*, en *toegangssleutel* (primair of secundair) verbinding maken met de cache.

```
redis-cli.exe -p 6380 -a YourAccessKey
```

![beveiligde tunnel met redis-cli](media/cache-how-to-redis-cli-tool/cache-redis-cli-stunnel.png)

Als u een test-cache met de **onbeveiligde** niet-SSL-poort, voer `redis-cli.exe` en door te geven uw *hostnaam*, *poort*, en *toegangssleutel*(primair of secundair) verbinding maken met de cache testen.

```
redis-cli.exe -h yourcachename.redis.cache.windows.net -p 6379 -a YourAccessKey
```

![beveiligde tunnel met redis-cli](media/cache-how-to-redis-cli-tool/cache-redis-cli-non-ssl.png)




## <a name="next-steps"></a>Volgende stappen

Meer informatie over het gebruik van de [Redis-Console](cache-configure.md#redis-console) opdrachten geven.

