---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/28/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 5df0fe23183b53f13d6c6545e248724324434cde
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029773"
---
De volgende replicatieopties zijn beschikbaar voor een opslagaccount:

* [Lokaal redundante opslag (LRS)](../articles/storage/common/storage-redundancy-lrs.md): Een eenvoudige, goedkope strategie voor replicatie. Gegevens worden synchroon drie keer binnen de primaire regio gerepliceerd.
* [Zone-redundante opslag (ZRS)](../articles/storage/common/storage-redundancy-zrs.md): Replicatie voor scenario's die hoge Beschik baarheid vereisen. Gegevens worden synchroon gerepliceerd over drie Azure-beschikbaarheids zones in de primaire regio.
* [Geografisch redundante opslag (GRS)](../articles/storage/common/storage-redundancy-grs.md): Kruis regionale replicatie om te beschermen tegen regionale storingen. Gegevens worden synchroon gerepliceerd in de primaire regio en vervolgens asynchroon gerepliceerd naar de secundaire regio. Voor lees toegang tot gegevens in de secundaire regio schakelt u geografisch redundante opslag met lees toegang (RA-GRS) in.
* [Geo-zone-redundante opslag (GZRS) (preview)](../articles/storage/common/storage-redundancy-gzrs.md): Replicatie voor scenario's waarbij hoge Beschik baarheid en maximale duurzaamheid zijn vereist. Gegevens worden synchroon gerepliceerd over drie Azure-beschikbaarheids zones in de primaire regio en vervolgens asynchroon gerepliceerd naar de secundaire regio. Voor lees toegang tot gegevens in de secundaire regio schakelt u geo-zone-redundante opslag met lees toegang (RA-GZRS) in.
