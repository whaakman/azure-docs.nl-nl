---
title: Annuleren en verwijderen van een Azure Import/Export-taak | Microsoft Docs
description: Informatie over het annuleren en verwijderen van taken voor de Microsoft Azure Import/Export-service.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: e11cf974a5f33020c889844dd34f51612e13036e
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39521010"
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Annuleren en verwijderen van de Azure Import/Export-taken

 Om aan te vragen dat een taak wordt geannuleerd voordat deze de `Packaging` staat, neemt u contact de [taakeigenschappen bijwerken](/rest/api/storageimportexport/jobs#Jobs_Update) bewerking en stel de `CancelRequested` element `true`. De taak is geannuleerd op basis van best-effort. Als er stations zijn bij het overbrengen van gegevens, blijven gegevens worden overgedragen, zelfs nadat de annulering is aangevraagd.

 Een geannuleerde taak wordt verplaatst naar de `Completed` status en worden bewaard gedurende 90 dagen, waarna deze worden verwijderd.

 Als wilt verwijderen van een taak, roept de [taak verwijderen](/rest/api/storageimportexport/jobs#Jobs_Delete) bewerking voordat de taak is verzonden (dat wil zeggen, terwijl de taak wordt de `Creating` status). U kunt ook een taak verwijderen wanneer deze zich in de `Completed` staat. Nadat een taak is verwijderd, zijn de informatie en de status niet meer toegankelijk via de REST-API of de Azure-portal.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van de Import/Export-service REST-API](storage-import-export-using-the-rest-api.md)
