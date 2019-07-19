---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 873fd8cbc211f098c93b8fb3fbe701e4a34d8487
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68320522"
---
De `Logging` instellingen beheren van ASP.NET Core logboekregistratie ondersteuning voor de container. U kunt de dezelfde configuratie-instellingen en waarden voor de container die u voor een ASP.NET Core-toepassing gebruikt. 

De volgende logboek registratie providers worden ondersteund door de container:

|Provider|Doel|
|--|--|
|[Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|De ASP.NET Core `Console` provider voor logboekregistratie. Alle configuratie-instellingen voor ASP.NET Core en standaardwaarden voor deze provider voor logboekregistratie worden ondersteund.|
|[Foutopsporing](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|De ASP.NET Core `Debug` provider voor logboekregistratie. Alle configuratie-instellingen voor ASP.NET Core en standaardwaarden voor deze provider voor logboekregistratie worden ondersteund.|
|[Schijf](#disk-logging)|De provider van de JSON-logboekregistratie. Deze provider voor logboekregistratie schrijft logboekgegevens naar het koppelpunt van de uitvoer.|

Met deze container opdracht worden logboek gegevens in de JSON-indeling opgeslagen in de uitvoer koppeling:

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
Logging:Disk:Format=json
```

Deze container opdracht bevat informatie over fout opsporing, voorafgegaan `dbug`door, terwijl de container wordt uitgevoerd:

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
Logging:Console:LogLevel:Default=Debug
```

### <a name="disk-logging"></a>Logboekregistratie van schijf

De `Disk` logboekregistratie-provider ondersteunt de volgende configuratie-instellingen:

| Name | Gegevenstype | Description |
|------|-----------|-------------|
| `Format` | Reeks | De indeling van de uitvoer voor de logboekbestanden.<br/> **Opmerking:** Deze waarde moet worden ingesteld op `json` om de logboek registratie provider in te scha kelen. Als deze waarde is opgegeven zonder ook een koppelpunt uitvoer op te geven bij het instantiëren van een container, wordt er een fout optreedt. |
| `MaxFileSize` | Geheel getal | De maximale grootte in megabytes (MB) van een logboekbestand. Wanneer de grootte van het huidige logboekbestand voldoet aan of deze waarde overschrijdt, wordt een nieuw logboekbestand wordt gestart door de provider voor logboekregistratie. Als -1 is opgegeven, wordt de grootte van het logboekbestand alleen beperkt door de maximale bestandsgrootte, indien aanwezig, voor het koppelen van de uitvoer. De standaardwaarde is 1. |

Zie voor meer informatie over het configureren van ondersteuning voor ASP.NET Core-logboekregistratie [instellingen bestandsconfiguratie](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1).

