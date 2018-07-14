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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38750080"
---
Een taak die wordt uitgevoerd in Azure Batch kan uitvoergegevens te produceren wanneer deze wordt uitgevoerd. De taak uitvoer gegevens vaak moeten kunnen worden opgeslagen voor het ophalen van andere taken in de taak, de clienttoepassing die de taak, of beide uitgevoerd. Taken uitvoergegevens schrijven naar het bestandssysteem van een Batch-rekenknooppunt, maar de gegevens op het knooppunt worden verwijderd wanneer deze wordt teruggezet of wanneer het knooppunt de pool verlaat. Taken mogelijk ook een bewaarperiode van bestand, waarna de bestanden die zijn gemaakt door de taak worden verwijderd. Daarom is het belangrijk om vast te leggen van de uitvoer van de taak die u later een gegevensarchief zoals [Azure Storage](https://docs.microsoft.com/azure/storage/).

Zie voor opslagaccountopties in Batch het [Overzicht van Batch-functies](../articles/batch/batch-api-basics.md#azure-storage-account).
