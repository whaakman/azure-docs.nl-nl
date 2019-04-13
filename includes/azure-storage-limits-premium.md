---
title: bestand opnemen
description: bestand opnemen
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 03/23/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: da796f8eeb6b24dfbbe8418cc728f09b424228cf
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/12/2019
ms.locfileid: "59528489"
---
### <a name="premium-performance-block-blob-storage"></a>Premium-prestaties blok-blobopslag

Een premium prestaties block blob storage-account is geoptimaliseerd voor toepassingen die gebruikmaken van kleinere, kilobyte bereik, objecten. Dit is ideaal voor toepassingen waarvoor hoge transactietarieven of opslag met consistente lage latentie. Premium-prestaties blok-blobopslag is ontworpen om te schalen met uw toepassingen. Als u van plan bent om te implementeren toepassingen waarvoor honderden of duizenden aanvragen per seconde of petabytes aan capaciteit, kunt u contact met ons indienen van een ondersteuningsaanvraag in de [Azure-portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="premium-performance-filestorage-preview"></a>Premium-prestaties FileStorage (preview)

Premium-bestanden gebruiken een unieke storage-account met de naam **FileStorage (preview)**. Dit accounttype is ontworpen voor werkbelastingen met hoge IOPS, hoge doorvoer met consistente lage latentie. Premium-bestand storage schaalt met de ingerichte sharegrootte.

|Onderwerp  |Doel  |
|---------|---------|
|Maximale grootte van ingericht     |5 TiB (openbare preview), 100 TiB (beperkte openbare preview-versie)     |
|Shares   |Onbeperkt  |
|IOPS     |100.000 (beperkte openbare preview-versie)    |
|Inkomend verkeer|4,136 MiB/s     |
|Uitgaand verkeer|6,204 MiB/s |

 Voor premium-bestand delen schaal doelen, raadpleegt u de [Premium bestanden schalen doelen](../articles/storage/common/storage-scalability-targets.md#premium-files-scale-targets) sectie.

### <a name="premium-performance-page-blob-storage"></a>Premium-prestaties pagina blob-opslag

Premium-prestaties, algemeen gebruik v1 of v2-accounts voor opslag hebben de volgende schaalbaarheidsdoelen:

| Totale capaciteit                            | Totale bandbreedte voor een account voor lokaal redundante opslag                     |
| ------------------------------------------------- | --------------------------------------------------------------------------- |
| Capaciteit van de schijf: 35 TB <br>Snapshot-capaciteit: 10 TB | Omhoog naar 50 gigabits per seconde voor binnenkomende<sup>1</sup> + uitgaande<sup>2</sup> |

<sup>1</sup> alle gegevens (aanvragen) die worden verzonden naar een opslagaccount

<sup>2</sup> alle gegevens (reacties) die worden ontvangen van een storage-account

Als u premium storage-accounts voor prestaties voor niet-beheerde schijven gebruikt en uw toepassing is groter dan de schaalbaarheidsdoelen van een enkel opslagaccount, is het raadzaam om te migreren naar managed disks. Als u niet migreren naar managed disks wilt, moet u uw toepassing gebruik meerdere opslagaccounts maken. Partities voor uw gegevens vervolgens over de storage-accounts. Bijvoorbeeld, als u koppelen 51-TB schijven op meerdere virtuele machines wilt, verdeeld over deze twee opslagaccounts. 35 TB is de limiet voor één premium storage-account. Zorg ervoor dat één premium performance storage-account nooit meer dan 35 TB van de ingerichte schijven heeft.