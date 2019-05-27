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
ms.openlocfilehash: 026717dff2b6883eb643497dec91226e4afe8133
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66115593"
---
Azure biedt de volgende ingebouwde RBAC-rollen voor het verlenen van toegang tot blob- en wachtrijservices gegevens met behulp van Azure AD en OAuth:

- [Storage Blob Gegevenseigenaar](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner): Eigendom instellen en beheren van POSIX-toegangsbeheer voor Azure Data Lake Storage Gen2 (preview). Zie voor meer informatie, [toegangsbeheer in Azure Data Lake Storage Gen2](../articles/storage/blobs/data-lake-storage-access-control.md).
- [Gegevensbijdrager voor Blob](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor): Gebruiken om machtigingen voor lezen/schrijven/verwijderen naar Blob storage-resources te verlenen.
- [Gegevenslezer voor Opslagblob](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader): Gebruiken om Blob storage-resources alleen-lezen machtigingen te verlenen.
- [Gegevensbijdrager voor wachtrij](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Gebruiken om de machtigingen lezen, schrijven en verwijderen in Azure-wachtrijen te verlenen.
- [Gegevenslezer voor Opslagwachtrij](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader): Gebruiken om Azure-wachtrijen alleen-lezen machtigingen te verlenen.
- [Opslaggegevens wachtrij bericht Processor](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor): Gebruiken om toegang te verlenen peek, ophalen en machtigingen voor het verwijderen naar berichten in wachtrijen van Azure Storage.
- [Opslaggegevens wachtrij bericht afzender](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender): Gebruiken om te verlenen machtigingen toevoegen aan berichten in wachtrijen van Azure Storage.

> [!NOTE]
> Houd er rekening mee dat RBAC-roltoewijzingen maximaal vijf minuten duurt worden doorgegeven.
