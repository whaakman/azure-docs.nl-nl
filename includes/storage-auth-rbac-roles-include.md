---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/21/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 4c66f35537cff87eb53a6cfe9bc0389f038f1a10
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449763"
---
Azure biedt de volgende ingebouwde RBAC-rollen voor toegang tot gegevens in de opslag:

- [Storage Blob Gegevenseigenaar](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner-preview): Eigendom instellen en beheren van POSIX-toegangsbeheer voor Azure Data Lake Storage Gen2 (preview). Zie voor meer informatie, [toegangsbeheer in Azure Data Lake Storage Gen2](../articles/storage/blobs/data-lake-storage-access-control.md).
- [Gegevensbijdrager voor Blob](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor-preview): Gebruiken om machtigingen voor lezen/schrijven/verwijderen naar Blob storage-resources te verlenen.
- [Gegevenslezer voor Opslagblob](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader-preview): Gebruiken om Blob storage-resources alleen-lezen machtigingen te verlenen.
- [Gegevensbijdrager voor wachtrij](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor-preview): Gebruiken om de machtigingen lezen, schrijven en verwijderen in Azure-wachtrijen te verlenen.
- [Gegevenslezer voor Opslagwachtrij](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader-preview): Gebruiken om Azure-wachtrijen alleen-lezen machtigingen te verlenen.
- [Opslaggegevens wachtrij bericht Processor](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor-preview): Gebruiken om toegang te verlenen peek, ophalen en machtigingen voor het verwijderen naar berichten in wachtrijen van Azure Storage.
- [Opslaggegevens wachtrij bericht afzender](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender-preview): Gebruiken om te verlenen machtigingen toevoegen aan berichten in wachtrijen van Azure Storage.

> [!IMPORTANT]
> RBAC-roltoewijzingen duurt maximaal vijf minuten worden doorgegeven.

Voor meer informatie over hoe u de ingebouwde rollen voor Azure Storage zijn gedefinieerd, Zie [begrijpen roldefinities](../articles/role-based-access-control/role-definitions.md#management-and-data-operations-preview). Zie voor meer informatie over het maken van aangepaste RBAC-rollen [aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer](../articles/role-based-access-control/custom-roles.md). 
