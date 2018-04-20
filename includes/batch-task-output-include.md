---
title: bestand opnemen
description: bestand opnemen
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0dcb608553d4455403c073e34e83bccb81cc64db
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2018
---
Een taak die wordt uitgevoerd in Azure Batch kan uitvoergegevens produceren wanneer deze wordt uitgevoerd. De taak uitvoer gegevens vaak moet worden opgeslagen voor ophalen door andere taken in de taak, de clienttoepassing die de taak, of beide uitgevoerd. Taken uitvoergegevens schrijven naar het bestandssysteem van een Batch-rekenknooppunt, maar de gegevens op het knooppunt worden verwijderd wanneer deze wordt teruggezet of wanneer het knooppunt de pool verlaat. Taken mogelijk ook een bewaarperiode bestand, waarna de bestanden die zijn gemaakt door de taak zijn verwijderd. Daarom is het belangrijk om vast te leggen van de uitvoer van de taak die u later naar een gegevensarchief, zoals hebt [Azure Storage](https://docs.microsoft.com/azure/storage/).

Zie voor account opslagopties in Batch de [overzicht Batch-functies](../articles/batch/batch-api-basics.md#azure-storage-account).
