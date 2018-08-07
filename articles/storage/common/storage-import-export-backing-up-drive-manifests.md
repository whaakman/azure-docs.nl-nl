---
title: Back-ups maken van stationsmanifesten Azure Import/Export | Microsoft Docs
description: Leer hoe u met uw stationsmanifesten voor de Microsoft Azure Import/Export-service automatisch een back-up gemaakt.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 933c0121a4f718ff812fc921bd6e04983fc69931
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39520534"
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a>Back-ups van schijf manifesten voor de Azure Import/Export-taken

Stationsmanifesten kunnen worden automatisch back-ups op blobs door in te stellen de `BackupDriveManifest` eigenschap `true` in de [taak plaatsen](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) of [Update taakeigenschappen](/rest/api/storageimportexport/jobs#Jobs_Update) REST API-bewerkingen. Standaard de stationsmanifesten niet back-ups. De back-ups manifest station zijn opgeslagen als blok-blobs in een container in het opslagaccount dat is gekoppeld aan de taak. De containernaam van de is standaard `waimportexport`, maar kunt u een andere naam in de `DiagnosticsPath` eigenschap bij het aanroepen van de `Put Job` of `Update Job Properties` bewerkingen. De back-manifest blob zijn met de naam in de volgende indeling: `waies/jobname_driveid_timestamp_manifest.xml`.

 U kunt de URI van de back-up stationsmanifesten voor een taak ophalen door het aanroepen van de [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) bewerking. De blob URI wordt geretourneerd in de `ManifestUri` eigenschap voor elk station.

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van de Import/Export-service REST-API](storage-import-export-using-the-rest-api.md)
