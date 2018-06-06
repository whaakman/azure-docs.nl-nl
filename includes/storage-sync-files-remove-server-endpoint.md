---
title: bestand opnemen
description: bestand opnemen
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 05/31/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 33dff710d83bd12a8db343a89c6e4576d1397ba7
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738680"
---
Geen: verwijderen van een servereindpunt is niet als een server opnieuw opstarten. Verwijderen en opnieuw maken van de server het eindpunt is bijna nooit een geschikte oplossing voor het oplossen van problemen met synchronisatie, cloud tiering of andere aspecten van Azure File-synchronisatie. Verwijderen van een servereindpunt is een destructieve bewerking en kan leiden tot verlies van gegevens in het geval dat bestanden gelaagde bestaan buiten de server endpoint-naamruimte (Zie [waarom gelaagde bestanden bestaan buiten de server eindpunt naamruimte](../articles/storage/files/storage-files-faq.md#afs-tiered-files-out-of-endpoint) voor meer informatie) of in de bestanden niet toegankelijk voor gelaagde bestanden in de naamruimte van server-eindpunt. Deze problemen worden niet oplossen wanneer de server het eindpunt wordt opnieuw gemaakt. Gelaagde bestanden kunnen bestaan in de naamruimte van uw server eindpunt zelfs als u nooit cloud tiering ingeschakeld. Daarom raden we aan het servereindpunt niet te verwijderen als u wilt stoppen met behulp van Azure File-synchronisatie met deze specifieke map of expliciet om dit te doen hebt gekregen door een engineer van Microsoft. Zie voor meer informatie over verwijderen server eindpunten [verwijderen van een servereindpunt](../articles/storage/files/storage-sync-files-server-endpoint.md#remove-a-server-endpoint).    