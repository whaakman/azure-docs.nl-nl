---
title: Annuleren en verwijderen van een Azure Import/Export-taak | Microsoft Docs
description: Informatie over het annuleren en verwijderen van taken voor de Microsoft Azure Import/Export-service.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: fd3d66f0-1dbb-4c75-9223-307d5abaeefc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 3524f1677baaa218b009b8498b851390c7b9da9a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38698686"
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Annuleren en verwijderen van de Azure Import/Export-taken

 Om aan te vragen dat een taak wordt geannuleerd voordat deze de `Packaging` staat, neemt u contact de [taakeigenschappen bijwerken](/rest/api/storageimportexport/jobs#Jobs_Update) bewerking en stel de `CancelRequested` element `true`. De taak is geannuleerd op basis van best-effort. Als er stations zijn bij het overbrengen van gegevens, blijven gegevens worden overgedragen, zelfs nadat de annulering is aangevraagd.

 Een geannuleerde taak wordt verplaatst naar de `Completed` status en worden bewaard gedurende 90 dagen, waarna deze worden verwijderd.

 Als wilt verwijderen van een taak, roept de [taak verwijderen](/rest/api/storageimportexport/jobs#Jobs_Delete) bewerking voordat de taak is verzonden (dat wil zeggen, terwijl de taak wordt de `Creating` status). U kunt ook een taak verwijderen wanneer deze zich in de `Completed` staat. Nadat een taak is verwijderd, zijn de informatie en de status niet meer toegankelijk via de REST-API of de Azure-portal.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van de Import/Export-service REST-API](storage-import-export-using-the-rest-api.md)
