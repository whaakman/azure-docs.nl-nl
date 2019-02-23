---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: dc527a4e1bdf9648ddfc9f582b0c146197214f26
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56740867"
---
De `Logging` instellingen beheren van ASP.NET Core logboekregistratie ondersteuning voor de container. U kunt de dezelfde configuratie-instellingen en waarden voor de container die u voor een ASP.NET Core-toepassing gebruikt. 

De volgende logboekregistratie-providers worden ondersteund door de container:

|Provider|Doel|
|--|--|
|[Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|De ASP.NET Core `Console` provider voor logboekregistratie. Alle configuratie-instellingen voor ASP.NET Core en standaardwaarden voor deze provider voor logboekregistratie worden ondersteund.|
|[Foutopsporing](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|De ASP.NET Core `Debug` provider voor logboekregistratie. Alle configuratie-instellingen voor ASP.NET Core en standaardwaarden voor deze provider voor logboekregistratie worden ondersteund.|
|[Schijf](#disk-logging)|De provider van de JSON-logboekregistratie. Deze provider voor logboekregistratie schrijft logboekgegevens naar het koppelpunt van de uitvoer.|

### <a name="disk-logging"></a>Logboekregistratie van schijf

De `Disk` logboekregistratie-provider ondersteunt de volgende configuratie-instellingen:  

| Name | Gegevenstype | Description |
|------|-----------|-------------|
| `Format` | Reeks | De indeling van de uitvoer voor de logboekbestanden.<br/> **Opmerking:** Deze waarde moet worden ingesteld op `json` om in te schakelen van de provider voor logboekregistratie. Als deze waarde is opgegeven zonder ook een koppelpunt uitvoer op te geven bij het instantiëren van een container, wordt er een fout optreedt. |
| `MaxFileSize` | Geheel getal | De maximale grootte in megabytes (MB) van een logboekbestand. Wanneer de grootte van het huidige logboekbestand voldoet aan of deze waarde overschrijdt, wordt een nieuw logboekbestand wordt gestart door de provider voor logboekregistratie. Als -1 is opgegeven, wordt de grootte van het logboekbestand alleen beperkt door de maximale bestandsgrootte, indien aanwezig, voor het koppelen van de uitvoer. De standaardwaarde is 1. |

Zie voor meer informatie over het configureren van ondersteuning voor ASP.NET Core-logboekregistratie [instellingen bestandsconfiguratie](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1).
