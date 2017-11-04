---
title: Diagnostische gegevens en de fout herstel voor Azure Import/Export taken | Microsoft Docs
description: Informatie over het inschakelen van uitgebreide logboekregistratie voor Microsoft Azure Import/Export-service-taken.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 096cc795-9af6-4335-9fe8-fffa9f239a17
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 0068aae9d6780aa41a070db0eb191d0d5a165d21
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a>Diagnostische gegevens en de fout herstel voor Azure Import/Export-taken
Voor elk station dat wordt verwerkt, maakt de Azure Import/Export-service een foutenlogboek in het bijbehorende opslagaccount. U kunt ook uitgebreide logboekregistratie inschakelen door in te stellen de `LogLevel` eigenschap `Verbose` bij het aanroepen van de [taak plaatsen](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) of [Update taakeigenschappen](/rest/api/storageimportexport/jobs#Jobs_Update) bewerkingen.

 Standaard worden naar een container met de naam Logboeken geschreven `waimportexport`. U kunt een andere naam opgeven door de `DiagnosticsPath` eigenschap bij het aanroepen van de `Put Job` of `Update Job Properties` bewerkingen. De logboeken worden opgeslagen als blok-blobs met de volgende naamconventie: `waies/jobname_driveid_timestamp_logtype.xml`.

 U kunt de URI van de logboeken voor een taak ophalen door het aanroepen van de [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) bewerking. De URI voor het uitgebreide logboek wordt geretourneerd als de `VerboseLogUri` eigenschap voor elke schijf, terwijl de URI voor het foutenlogboek wordt geretourneerd als de `ErrorLogUri` eigenschap.

De logboekgegevens kunt u de volgende problemen te identificeren.

## <a name="drive-errors"></a>Station fouten

De volgende items zijn geclassificeerd als station fouten:

-   Fouten in de toegang tot of het lezen van het manifestbestand

-   Onjuiste BitLocker-sleutels

-   Schijf lezen/schrijven-fouten

## <a name="blob-errors"></a>BLOB-fouten

De volgende items zijn geclassificeerd als blob fouten:

-   Onjuiste of conflicterende blob of -namen

-   Ontbrekende bestanden

-   BLOB is niet gevonden

-   Afgekapte bestanden (de bestanden op de schijf zijn kleiner is dan de opgegeven in het manifest)

-   Beschadigd bestand inhoud (voor importtaken die is gedetecteerd met een MD5-checksum komt niet overeen)

-   Beschadigde blob-eigenschap bestanden met metagegevens en (gedetecteerd met een MD5-checksum komt niet overeen)

-   Onjuist schema voor de blob-eigenschappen en/of de bestanden met metagegevens

Mogelijk zijn er gevallen waarbij sommige onderdelen van een taak importeren of exporteren komen niet voltooid, wordt nog steeds de algehele taak voltooid. In dit geval kunt u uploaden of downloaden van de ontbrekende onderdelen van de gegevens via het netwerk of u een nieuwe taak om de gegevens kunt maken. Zie de [Azure Import/Export hulpprogramma verwijzing](storage-import-export-tool-how-to-v1.md) voor informatie over het herstellen van de gegevens via het netwerk.

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van de Import/Export-service REST-API](storage-import-export-using-the-rest-api.md)
