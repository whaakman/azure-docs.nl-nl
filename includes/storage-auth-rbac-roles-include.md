---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/25/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 4f9a828e5233c88db2106bc648c07578927e0d29
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68514872"
---
Azure biedt de volgende ingebouwde RBAC-rollen voor het verlenen van toegang tot Blob-en wachtrij gegevens met behulp van Azure AD en OAuth:

- [Eigenaar van gegevens](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner)van de opslag-blob: Gebruik om het eigendom in te stellen en POSIX Access Control te beheren voor Azure Data Lake Storage Gen2. Zie [toegangs beheer in azure data Lake Storage Gen2](../articles/storage/blobs/data-lake-storage-access-control.md)voor meer informatie.
- [Inzender voor Storage BLOB-gegevens](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor): Gebruiken om machtigingen voor lezen/schrijven/verwijderen te verlenen aan Blob Storage-resources.
- [Gegevens lezer](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader)van de opslag-blob: Gebruiken om alleen-lezen-machtigingen te verlenen aan Blob Storage-resources.
- [Inzender voor opslag wachtrij gegevens](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Gebruiken om machtigingen voor lezen/schrijven/verwijderen te verlenen aan Azure-wacht rijen.
- [Gegevens lezer van de opslag wachtrij](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader): Gebruiken om alleen-lezen-machtigingen te verlenen aan Azure-wacht rijen.
- [Processor voor gegevens berichten van de opslag wachtrij](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor): Gebruiken om machtigingen voor weer geven, ophalen en verwijderen te verlenen aan berichten in Azure Storage wachtrijen.
- [Afzender gegevens bericht van opslag wachtrij](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender): Gebruik om machtigingen toe te kennen voor berichten in Azure Storage wacht rijen.

> [!NOTE]
> Houd er rekening mee dat de toewijzing van RBAC-rollen tot vijf minuten kan duren.
