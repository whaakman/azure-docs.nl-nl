---
title: Ondersteunde clusterversies in Azure Service Fabric | Microsoft Docs
description: Meer informatie over de clusterversies van het in Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chakdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/24/2018
ms.author: aljo
ms.openlocfilehash: 75e95737eecb9407a80103d1cad00d4987fe7091
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60716078"
---
# <a name="supported-service-fabric-versions"></a>Ondersteunde versies van de Service Fabric

Zorg ervoor dat uw cluster altijd wordt uitgevoerd een ondersteunde versie van Azure Service Fabric. Een minimum van 60 dagen nadat we kondigen wij de release van een nieuwe versie van Service Fabric, ondersteuning voor de vorige versie eindigt. Aankondigingen van nieuwe releases vindt u op de [Service Fabric-teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/).

Raadpleeg de volgende documenten voor meer informatie over hoe u uw een ondersteunde versie van Service Fabric-cluster:

- [Een Azure Service Fabric-cluster upgraden](service-fabric-cluster-upgrade.md)
- [De Service Fabric-versie die wordt uitgevoerd op uw zelfstandige Windows Server-cluster upgraden](service-fabric-cluster-upgrade-windows-server.md)

## <a name="supported-versions"></a>Ondersteunde versies

De volgende tabel bevat de versies van Service Fabric en de einddatum van ondersteuning.

| Service Fabric-runtime in het cluster | Rechtstreeks vanuit de clusterversie kunt upgraden |Compatibele SDK of NuGet-Pakketversie | Einde van ondersteuning |
| --- | --- |--- | --- |
| Alle clusterversies voordat u 5.3.121 | 5.1.158.* |Kleiner dan of gelijk zijn aan versie 2.3 |20 januari 2017 |
| 5.3.* | 5.1.158.* |Kleiner dan of gelijk zijn aan versie 2.3 |24 februari 2017 |
| 5.4.* | 5.1.158.* |Kleiner dan of gelijk zijn aan versie 2.4 |10 mei 2017       |
| 5.5.* | 5.4.164.* |Kleiner dan of gelijk zijn aan versie 2.5 |Augustus 10,2017    |
| 5.6.* | 5.4.164.* |Kleiner dan of gelijk zijn aan versie 2.6 |Oktober 13,2017   |
| 5.7.* | 5.4.164.* |Kleiner dan of gelijk zijn aan versie 2.7 |15 december 2017  |
| 6.0.* | 5.6.205.* |Kleiner dan of gelijk zijn aan versie 2.8 |30 maart 2018     |
| 6.1.* | 5.7.221.* |Kleiner dan of gelijk zijn aan versie 3.0 |15 juli 2018      |
| 6.2.* | 6.0.232.* |Kleiner dan of gelijk zijn aan versie 3.1 |26 oktober 2018   |
| 6.3.* | 6.1.480.* |Kleiner dan of gelijk zijn aan versie 3.2 |Maart 31 mei 2019  |
| 6.4.* | 6.2.301.* |Kleiner dan of gelijk zijn aan versie 3.3 |Huidige versie, dus geen einddatum |

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

De volgende tabel bevat de ondersteunde besturingssystemen voor de ondersteunde versies van de Service Fabric.

| Besturingssysteem | Vroegste ondersteunde versie van Service Fabric |
| --- | --- |
| Windows Server 2012 R2 | Alle versies |
| Windows Server 2016 | Alle versies |
| Windows Server 1709 | 6.0 |
| WindowsServer 1803 | 6.4 |
| Windows Server 1809 | 6.4.654.9590 |
| Windows Server 2019 | 6.4.654.9590 |
| Linux Ubuntu 16.04 | 6.0 |

