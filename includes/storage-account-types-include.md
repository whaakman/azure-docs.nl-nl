---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/02/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: b278cce8f885f31f9d59fd389261812e04e58405
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54066987"
---
Azure Storage biedt drie typen opslagaccounts. Elk type biedt ondersteuning voor verschillende functies en heeft een eigen prijsmodel. Houd rekening met deze verschillen voordat u een opslagaccount om te bepalen welk type account dat wordt aanbevolen voor uw toepassingen kunt maken. De drie typen opslagaccounts zijn:

* **Voor algemeen gebruik v2-accounts**: Basic opslagaccounttype voor blobs, bestanden, wachtrijen en tabellen. Aanbevolen voor de meeste scenario's met behulp van Azure Storage.
* **Accounts voor algemeen gebruik v1**: Verouderde accounttype voor blobs, bestanden, wachtrijen en tabellen. Voor algemeen gebruik v2-accounts gebruiken, indien mogelijk.
* **BLOB storage-accounts**: Alleen-BLOB storage-accounts. Voor algemeen gebruik v2-accounts gebruiken, indien mogelijk. 

De volgende tabel beschrijft de soorten opslagaccounts en de bijbehorende mogelijkheden:

| Type opslagaccount | Ondersteunde services                       | Ondersteunde prestatielagen | Ondersteunde toegangslagen               | Opties voor gegevensreplicatie                                                | Implementatiemodel<sup>1</sup>  | Versleuteling<sup>2</sup> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------------|--------------------------------------------------------------------|-------------------|------------|
| Algemeen gebruik V2   | BLOB, bestand, wachtrij, tabel en schijf       | Standard, Premium           | Hot, Cool archief<sup>3</sup> | LRS-, ZRS<sup>4</sup>, GRS, RA-GRS | Resource Manager | Versleuteld  |
| Algemeen gebruik V1   | BLOB, bestand, wachtrij, tabel en schijf       | Standard, Premium           | N/A                                  | LRS, GRS, RA-GRS                                                   | Resource Manager, klassiek  | Versleuteld  |
| Blob Storage         | BLOB (blok-blobs en toevoeg-blobs alleen) | Standard                    | Hot, Cool archief<sup>3</sup>                            | LRS, GRS, RA-GRS                                                   | Resource Manager  | Versleuteld  |

<sup>1</sup>met het Azure Resource Manager-implementatiemodel wordt aanbevolen. Opslagaccounts met behulp van het klassieke implementatiemodel kunnen nog steeds worden gemaakt op bepaalde locaties en bestaande klassieke accounts worden nog steeds ondersteund. Zie voor meer informatie, [Azure Resource Manager en klassieke implementatie: Implementatiemodellen en de status van uw resources begrijpen](../articles/azure-resource-manager/resource-manager-deployment-model.md).

<sup>2</sup>alle opslagaccounts zijn versleuteld met behulp van Storage Service Encryption (SSE) voor data-at-rest. Zie voor meer informatie, [Azure Storage-Serviceversleuteling voor Data-at-Rest](../articles/storage/common/storage-service-encryption.md).

<sup>3</sup>de Archive-laag is beschikbaar op het niveau van een afzonderlijke blob alleen, niet op het niveau van de storage-account. Alleen blok-blobs en toevoeg-blobs kunnen worden gearchiveerd. Zie voor meer informatie, [Azure Blob-opslag: Hot, Cool, en Archiefopslaglaag](../articles/storage/blobs/storage-blob-storage-tiers.md).

<sup>4</sup>zone-redundante opslag (ZRS) is alleen beschikbaar voor algemeen gebruik v2 standard storage-accounts. Zie voor meer informatie over ZRS [Zone-redundante opslag (ZRS): Maximaal beschikbare toepassingen voor Azure Storage](../articles/storage/common/storage-redundancy-zrs.md). Zie voor meer informatie over andere opties voor gegevensreplicatie [Azure Storage-replicatie](../articles/storage/common/storage-redundancy.md).
