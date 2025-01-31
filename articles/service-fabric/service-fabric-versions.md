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
ms.date: 07/03/2019
ms.author: aljo
ms.openlocfilehash: 030e57512d10f31ca03dff2f99e57c305de49e97
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565983"
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
| 6.4.* | 6.2.301.* |Kleiner dan of gelijk zijn aan versie 3.3 |Op 15 september 2019 |
| 6.5.* | 6.4.617.* |Kleiner dan of gelijk zijn aan versie 3.4 |Huidige versie, dus geen einddatum |

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

## <a name="supported-version-names"></a>Namen van de ondersteunde versie

De volgende tabel bevat de versienamen van Service Fabric en hun bijbehorende versienummers.

| Versienaam | Windows-versienummer | Linux-versienummer |
| --- | --- | --- |
| 5.3 RTO | 5.3.121.9494 | N.v.t. |
| 5.3 CU1 | 5.3.204.9494 | N.v.t. |
| 5.3 CU2 | 5.3.301.9590 | N.v.t. |
| 5.3 CU3 | 5.3.311.9590 | N.v.t. |
| 5.4 CU2 | 5.4.164.9494 | N.v.t. |
| 5.5 CU1 | 5.5.216.0    | N.v.t. |
| 5.5 CU2 | 5.5.219.0    | N.v.t. |
| 5.5 CU3 | 5.5.227.0    | N.v.t. |
| 5.5 CU4 | 5.5.232.0    | N.v.t. |
| 5.6 RTO | 5.6.204.9494 | N.v.t. |
| 5.6 CU2 | 5.6.210.9494 | N.v.t. |
| 5.6 CU3 | 5.6.220.9494 | N.v.t. |
| 5.7 RTO | 5.7.198.9494 | N.v.t. |
| 5.7 CU4 | 5.7.221.9494 | N.v.t. |
| 6.0 RTO | 6.0.211.9494 | 6.0.120.1 |
| 6.0 CU1 | 6.0.219.9494 | 6.0.127.1 |
| 6.0 CU2 | 6.0.232.9494 | 6.0.133.1 |
| 6.1 CU1 | 6.1.456.9494 | 6.1.183.1 |
| 6.1 CU2 | 6.1.467.9494 | 6.1.185.1 |
| 6.1 CU3 | 6.1.472.9494 | N.v.t. |
| 6.1 CU4 | 6.1.480.9494 | 6.1.187.1 |
| 6.2 RTO | 6.2.269.9494 | 6.2.184.1 | 
| 6.2 CU1 | 6.2.274.9494 | 6.2.191.1 |
| 6.2 CU2 | 6.2.283.9494 | 6.2.194.1 |
| 6.2 CU3 | 6.2.301.9494 | 6.2.199.1 |
| 6.3 RTO | 6.3.162.9494 | 6.3.119.1 |
| 6.3 CU1 | 6.3.176.9494 | 6.3.124.1 |
| 6.3 CU1 | 6.3.187.9494 | 6.3.129.1 |
| 6.4 RTO | 6.4.617.9590 | 6.4.625.1 |
| 6.4 CU2 | 6.4.622.9590 | N.v.t. |
| 6.4 CU3 | 6.4.637.9590 | 6.4.634.1 |
| 6.4 CU4 | 6.4.644.9590 | 6.4.639.1 |
| 6.4 CU5 | 6.4.654.9590 | 6.4.649.1 |
| 6.4 CU6 ONDERSTEUND | 6.4.658.9590 | N.v.t. |
| 6.4 CU7 | 6.4.664.9590 | 6.4.661.1 |
| 6.4 CU8 | 6.4.670.9590 | N.v.t. |
| 6.5 RTO | 6.5.639.9590 | 6.5.435.1 |
| 6.5 CU1 | 6.5.641.9590 | 6.5.454.1 |