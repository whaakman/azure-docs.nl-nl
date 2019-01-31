---
title: Met behulp van de Azure Import/Export-service REST-API | Microsoft Docs
description: Meer informatie over waar u kunt resources voor het gebruik van de Azure Import/Export-service REST API, met inbegrip van materiaal voor instructie- en referentiedocumenten vinden.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 1e8b60f37cefb81fbbbbb7823be7752dd1188dc3
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55471724"
---
# <a name="using-the-azure-importexport-service-rest-api"></a>De REST-API van de Azure-service Import/Export gebruiken

De Microsoft Azure Import/Export-service geeft een REST-API om in te schakelen programmatisch beheer van import/export-taken. U kunt de REST-API gebruiken om uit te voeren op alle van de import/export-bewerkingen die u met uitvoeren kunt de [Azure-portal](https://portal.azure.com/). Bovendien kunt u de REST-API om uit te voeren van bepaalde gedetailleerde bewerkingen, zoals het uitvoeren van query's het voltooiingspercentage van een taak die momenteel niet beschikbaar in Azure portal is.

Zie [met behulp van de Microsoft Azure Import/Export-service gegevens overbrengen naar Blob-opslag](../storage-import-export-service.md) voor een overzicht van de Import/Export-service en een zelfstudie die laat hoe u zien het gebruik van de portal te maken en beheren van importeren en exporteren van taken.

## <a name="service-endpoints"></a>Service-eindpunten

De Azure Import/Export-service is een resourceprovider voor Azure Resource Manager en biedt een set REST-API's op de volgende HTTPS-eindpunt voor het beheren van import/export-taken:

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a>Versiebeheer

Aanvragen voor de Import/Export-service moeten opgeven de `api-version` parameter en stel de waarde op `2016-11-01`.

## <a name="importexport-service-operations"></a>Import/Export-servicebewerkingen

[Een importtaak maken](../storage-import-export-creating-an-import-job.md)

[Een exporttaak maken](../storage-import-export-creating-an-export-job.md)

[Statusinformatie van een taak ophalen](storage-import-export-retrieving-state-info-for-a-job.md)

[Taken opsommen](../storage-import-export-enumerating-jobs.md)

[Taken annuleren en verwijderen](storage-import-export-cancelling-and-deleting-jobs.md)

[Back-ups maken van stationsmanifesten](../storage-import-export-backing-up-drive-manifests.md)

[Diagnoses en het herstel van fouten voor Import/Export-taken](../storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a>Volgende stappen

* [Storage Import/Export-REST](/rest/api/storageimportexport)
