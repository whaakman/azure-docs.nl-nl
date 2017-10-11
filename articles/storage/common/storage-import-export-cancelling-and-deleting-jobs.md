---
title: Annuleren en een Azure Import/Export-taak verwijderen | Microsoft Docs
description: Informatie over het annuleren en taken voor de Microsoft Azure Import/Export-service verwijderen.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: fd3d66f0-1dbb-4c75-9223-307d5abaeefc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 1e989c72fc03697bf6d2e515ff53003703665d1a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Annuleren en het verwijderen van de Azure Import/Export-taken

 Om aan te vragen of een taak worden geannuleerd voordat deze zich in de `Packaging` status, neemt u contact de [Update taakeigenschappen](/rest/api/storageimportexport/jobs#Jobs_Update) bewerking en stel de `CancelRequested` element op de `true`. De taak is geannuleerd op basis van best-effort. Als er stations zijn bezig het overbrengen van gegevens, blijven gegevens worden overgebracht, zelfs nadat de annulering is aangevraagd.

 Een geannuleerde taak wordt verplaatst naar de `Completed` status en bijgehouden voor 90 dagen op dat moment wordt verwijderd.

 Aanroepen voor het verwijderen van een taak, de [taak verwijderen](/rest/api/storageimportexport/jobs#Jobs_Delete) bewerking voordat de taak is verzonden (dat wil zeggen, terwijl de taak wordt de `Creating` staat). U kunt ook een taak verwijderen wanneer deze zich in de `Completed` staat. Nadat een taak is verwijderd, de informatie en de status niet langer zijn toegankelijk via de REST-API of de Azure-portal.

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van de Import/Export-service REST-API](storage-import-export-using-the-rest-api.md)
