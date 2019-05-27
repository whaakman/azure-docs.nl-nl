---
title: bestand opnemen
description: bestand opnemen
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 7ba4c90811bd8051ed9c307d9f9fa33e08e69dc7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66127530"
---
Een taak die wordt uitgevoerd in Azure Batch kan uitvoergegevens te produceren wanneer deze wordt uitgevoerd. De taak uitvoer gegevens vaak moeten kunnen worden opgeslagen voor het ophalen van andere taken in de taak, de clienttoepassing die de taak, of beide uitgevoerd. Taken uitvoergegevens schrijven naar het bestandssysteem van een Batch-rekenknooppunt, maar de gegevens op het knooppunt worden verwijderd wanneer deze wordt teruggezet of wanneer het knooppunt de pool verlaat. Taken mogelijk ook een bewaarperiode van bestand, waarna de bestanden die zijn gemaakt door de taak worden verwijderd. Daarom is het belangrijk om vast te leggen van de uitvoer van de taak die u later een gegevensarchief zoals [Azure Storage](https://docs.microsoft.com/azure/storage/).

Zie voor opslagaccountopties in Batch het [Overzicht van Batch-functies](../articles/batch/batch-api-basics.md#azure-storage-account).
