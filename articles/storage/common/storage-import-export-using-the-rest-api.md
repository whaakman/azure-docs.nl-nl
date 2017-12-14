---
title: Met de Azure Import/Export-service REST-API | Microsoft Docs
description: Meer informatie over waar u informatie over het gebruik van de Azure Import/Export-service REST API, met inbegrip van de procedures voor-en naslagmateriaal vinden.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 233f80e9-2e7f-48e0-9639-5c7785e7d743
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 9a5a97a5d9f06aa73f1ad521e112fa25f215724f
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="using-the-azure-importexport-service-rest-api"></a>De REST-API van de Azure-service Import/Export gebruiken

De Microsoft Azure Import/Export-service wordt een REST-API voor programmatisch beheer van importeren/exporteren-taken. U kunt de REST-API gebruiken om uit te voeren op alle bewerkingen voor importeren/exporteren die u met uitvoeren kunt de [Azure-portal](https://portal.azure.com/). Bovendien kunt u de REST-API om uit te voeren van bepaalde gedetailleerde bewerkingen, zoals het uitvoeren van query's de percentage voltooiing van een taak die momenteel niet beschikbaar in de Azure portal is.

Zie [via de Microsoft Azure Import/Export-service gegevens overdragen naar Blob Storage](../storage-import-export-service.md) voor een overzicht van de Import/Export-service en een zelfstudie over het gebruik van de portal te maken en beheren van importeren en exporteren van taken.

## <a name="service-endpoints"></a>Service-eindpunten

De Azure Import/Export-service is een resourceprovider voor Azure Resource Manager en biedt een set van REST-API's op de volgende HTTPS-eindpunt voor het beheren van taken voor importeren/exporteren:

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a>Versiebeheer

Aanvragen voor de Import/Export-service moeten opgeven de `api-version` parameter en stel de waarde op `2016-11-01`.

## <a name="importexport-service-operations"></a>Servicebewerkingen voor importeren/exporteren

[Een importtaak maken](../storage-import-export-creating-an-import-job.md)

[Een exporttaak maken](../storage-import-export-creating-an-export-job.md)

[Statusinformatie van een taak ophalen](storage-import-export-retrieving-state-info-for-a-job.md)

[Taken opsommen](../storage-import-export-enumerating-jobs.md)

[Taken annuleren en verwijderen](storage-import-export-cancelling-and-deleting-jobs.md)

[Een back-Up station manifesten](../storage-import-export-backing-up-drive-manifests.md)

[Diagnoses en het herstel van fouten voor Import/Export-taken](../storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a>Volgende stappen

* [Opslag voor importeren/exporteren REST](/rest/api/storageimportexport)
