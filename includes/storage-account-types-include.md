---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/23/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 14de5283197778464993b80b312a690662e066ab
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443749"
---
Azure Storage biedt verschillende typen opslagaccounts. Elk type biedt ondersteuning voor verschillende functies en heeft een eigen prijsmodel. Houd rekening met deze verschillen voordat u een opslagaccount om te bepalen welk type account dat wordt aanbevolen voor uw toepassingen kunt maken. De typen opslagaccounts zijn:

- **Voor algemeen gebruik v2-accounts**: Basic opslagaccounttype voor blobs, bestanden, wachtrijen en tabellen. Aanbevolen voor de meeste scenario's met behulp van Azure Storage.
- **Accounts voor algemeen gebruik v1**: Verouderde accounttype voor blobs, bestanden, wachtrijen en tabellen. Voor algemeen gebruik v2-accounts gebruiken, indien mogelijk.
- **Blob storage-accounts blokkeren**: Alleen-BLOB storage-accounts met premium-prestatiekenmerken. Aanbevolen voor scenario's met hoge transacties-tarieven, met behulp van kleinere objecten of het vereisen van consistent met lage opslaglatentie.
- **Storage-accounts FileStorage**: Bestanden alleen-lezen-storage-accounts met premium-prestatiekenmerken. Aanbevolen voor enterprise- of schaaltoepassingen met hoge prestaties.
- **BLOB storage-accounts**: Alleen-BLOB storage-accounts. Voor algemeen gebruik v2-accounts gebruiken, indien mogelijk.

De volgende tabel beschrijft de soorten opslagaccounts en de bijbehorende mogelijkheden:

| Type opslagaccount | Ondersteunde services                       | Ondersteunde prestatielagen      | Ondersteunde toegangslagen         | Opties voor gegevensreplicatie               | Implementatiemodel<div role="complementary" aria-labelledby="deployment-model"><sup>1</sup></div> | Versleuteling<div role="complementary" aria-labelledby="encryption"><sup>2</sup></div> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------|-----------------------------------|------------------------------|------------------------|
| Algemeen gebruik V2   | BLOB, bestand, wachtrij, tabel en schijf       | Standard, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | Hot, Cool en Archive<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS, ZRS<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div> | Resource Manager             | Versleuteld              |
| Algemeen gebruik V1   | BLOB, bestand, wachtrij, tabel en schijf       | Standard, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | N/A                            | LRS, GRS, RA-GRS                  | Resource Manager, klassiek    | Versleuteld              |
| Blok-blobopslag   | BLOB (blok-blobs en toevoeg-blobs alleen) | Premium                       | N/A                            | LRS                               | Resource Manager             | Versleuteld              |
| FileStorage   | U kunt alleen bestanden | Premium                       | N/A                            | LRS                               | Resource Manager             | Versleuteld              |
| Blob Storage         | BLOB (blok-blobs en toevoeg-blobs alleen) | Standard                      | Hot, Cool en Archive<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS                  | Resource Manager             | Versleuteld              |

<div id="deployment-model"><sup>1</sup>met het Azure Resource Manager-implementatiemodel wordt aanbevolen. Opslagaccounts met behulp van het klassieke implementatiemodel kunnen nog steeds worden gemaakt op bepaalde locaties en bestaande klassieke accounts worden nog steeds ondersteund. Zie voor meer informatie, <a href="https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model">Azure Resource Manager en klassieke implementatie: Implementatiemodellen en de status van uw resources begrijpen</a>.</div>

<div id="encryption"><sup>2</sup>alle opslagaccounts zijn versleuteld met behulp van Storage Service Encryption (SSE) voor data-at-rest. Zie voor meer informatie, <a href="https://docs.microsoft.com/azure/storage/common/storage-service-encryption">Azure Storage-Serviceversleuteling voor Data-at-Rest</a>.</div>

<div id="archive"><sup>3</sup>de Archive-laag is beschikbaar op het niveau van een afzonderlijke blob alleen, niet op het niveau van de storage-account. Alleen blok-blobs en toevoeg-blobs kunnen worden gearchiveerd. Zie voor meer informatie, <a href="https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers">Azure Blob-opslag: Hot, Cool, en Archiefopslaglaag</a>.</div>

<div id="zone-redundant-storage"><sup>4</sup>zone-redundante opslag (ZRS) is alleen beschikbaar voor algemeen gebruik v2 standard storage-accounts. Zie voor meer informatie over ZRS <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs">Zone-redundante opslag (ZRS): Maximaal beschikbare toepassingen voor Azure Storage</a>. Zie voor meer informatie over andere opties voor gegevensreplicatie <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy">Azure Storage-replicatie</a>.</div>

<div id="premium-performance"><sup>5</sup>premium-prestaties voor algemeen gebruik v2 en algemeen gebruik v1-accounts is beschikbaar voor schijven en pagina-blob.</div>
