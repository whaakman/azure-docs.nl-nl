---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 02/19/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: d97a2350765ac321cf77f8a9f84825c88d0c9185
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56824250"
---
Azure biedt de volgende ingebouwde RBAC-rollen voor toegang tot gegevens in de opslag:

- [Eigenaar van gegevens (Preview) voor Blob Storage](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner-preview): Eigendom instellen en beheren van POSIX-toegangsbeheer voor Azure Data Lake Storage Gen2 (preview). Zie voor meer informatie, [toegangsbeheer in Azure Data Lake Storage Gen2](../articles/storage/blobs/data-lake-storage-access-control.md).
- [Gegevensbijdrager voor Blob (Preview)](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor-preview): Gebruiken om machtigingen voor lezen/schrijven/verwijderen naar Blob storage-resources te verlenen.
- [Gegevenslezer voor Opslagblob (Preview)](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader-preview): Gebruiken om Blob storage-resources alleen-lezen machtigingen te verlenen.
- [Gegevensbijdrager voor wachtrij (Preview)](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor-preview): Gebruiken om de machtigingen lezen, schrijven en verwijderen in Azure-wachtrijen te verlenen.
- [Gegevenslezer voor Opslagwachtrij (Preview)](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader-preview): Gebruiken om Azure-wachtrijen alleen-lezen machtigingen te verlenen.

Voor meer informatie over hoe u de ingebouwde rollen voor Azure Storage zijn gedefinieerd, Zie [begrijpen roldefinities](../articles/role-based-access-control/role-definitions.md#management-and-data-operations-preview).

Azure Storage biedt ook ondersteuning voor aangepaste RBAC-rollen. Zie voor meer informatie, [aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer](../articles/role-based-access-control/custom-roles.md). 
