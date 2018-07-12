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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38763080"
---
Nee: verwijderen van een servereindpunt is niet als een server opnieuw opstarten. Verwijderen en opnieuw maken van het servereindpunt is bijna nooit een geschikte oplossing voor het oplossen van problemen met synchronisatie, cloud-opslaglagen of andere aspecten van Azure File Sync. Een destructieve bewerking voor het verwijderen van een servereindpunt is en kan leiden tot verlies van gegevens in het geval met bestanden gelaagde aanwezig zijn buiten de server eindpunt-naamruimte (Zie [waarom gelaagde bestanden bestaan buiten de naamruimte van de eindpunt](../articles/storage/files/storage-files-faq.md#afs-tiered-files-out-of-endpoint) voor meer informatie) of in de bestanden niet toegankelijk zijn voor gelaagde bestanden in de naamruimte van de eindpunt. Deze problemen wordt niet opgelost wanneer het servereindpunt wordt opnieuw gemaakt. Gelaagde bestanden kunnen bestaan binnen uw server eindpunt-naamruimte, zelfs als u nog nooit aangemeld met cloud-opslaglagen ingeschakeld. Daarom raden we aan het servereindpunt niet te verwijderen, tenzij u wilt stoppen met het gebruik van Azure File Sync met deze specifieke map of expliciet om dit te doen hebt gekregen van een Microsoft-technicus. Zie voor meer informatie over servereindpunten verwijderen [verwijderen van een servereindpunt](../articles/storage/files/storage-sync-files-server-endpoint.md#remove-a-server-endpoint).    