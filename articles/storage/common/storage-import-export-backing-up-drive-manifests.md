---
title: Back-ups van de Azure Import/Export station manifesten | Microsoft Docs
description: Informatie over hoe u uw station manifesten voor de Microsoft Azure Import/Export-service automatisch een back-up gemaakt.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 594abd80-b834-4077-a474-d8a0f4b7928a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 33eb8e1eea8f8aa7b79ef3e54f2b1ed88dc794ae
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a>Back-ups van schijf manifesten voor Azure Import/Export-taken

Station manifesten kunnen automatisch een back-up naar BLOB's door in te stellen de `BackupDriveManifest` eigenschap `true` in de [taak plaatsen](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) of [Update taakeigenschappen](/rest/api/storageimportexport/jobs#Jobs_Update) REST-API-bewerkingen. Standaard de manifesten station zijn geen back-up gemaakt. De back-ups manifest station worden opgeslagen als blok-blobs in een container in het opslagaccount die is gekoppeld aan de taak. De containernaam is standaard `waimportexport`, maar u kunt opgeven dat een andere naam in de `DiagnosticsPath` eigenschap bij het aanroepen van de `Put Job` of `Update Job Properties` bewerkingen. De back-manifest blob zijn met de naam in de volgende indeling: `waies/jobname_driveid_timestamp_manifest.xml`.

 U kunt de URI van de manifesten back-station voor een taak ophalen door het aanroepen van de [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) bewerking. De blob URI wordt geretourneerd als de `ManifestUri` eigenschap voor elk station.

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van de Import/Export-service REST-API](storage-import-export-using-the-rest-api.md)
