---
title: Herstel van diagnostische gegevens en de fout voor Azure Import/Export-taken | Microsoft Docs
description: Informatie over het inschakelen van uitgebreide logboekregistratie voor Microsoft Azure Import/Export-service-taken.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 2a54752f933b91265d0aa8add61ca0707615931b
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526324"
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a>Herstel van diagnostische gegevens en de fout voor Azure Import/Export-taken
Voor elk station dat is verwerkt, wordt in de Azure Import/Export-service een foutenlogboek maakt in het bijbehorende opslagaccount. U kunt ook uitgebreide logboekregistratie inschakelen door in te stellen de `LogLevel` eigenschap `Verbose` bij het aanroepen van de [taak plaatsen](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) of [Update taakeigenschappen](/rest/api/storageimportexport/jobs#Jobs_Update) bewerkingen.

 Standaard logboeken worden geschreven naar een container met de naam `waimportexport`. U kunt een andere naam opgeven door in te stellen de `DiagnosticsPath` eigenschap bij het aanroepen van de `Put Job` of `Update Job Properties` bewerkingen. De logboeken worden opgeslagen als blok-blobs met de volgende naamconventie gebruikt: `waies/jobname_driveid_timestamp_logtype.xml`.

 U kunt de URI van de logboeken voor een taak ophalen door het aanroepen van de [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) bewerking. De URI voor het uitgebreide logboek wordt geretourneerd in de `VerboseLogUri` eigenschap voor elke schijf, terwijl de URI voor het foutenlogboek wordt geretourneerd in de `ErrorLogUri` eigenschap.

De gegevens voor logboekregistratie kunt u de volgende problemen identificeren.

## <a name="drive-errors"></a>Station fouten

De volgende items zijn geclassificeerd als station fouten:

-   Fouten in de toegang tot of het lezen van het manifestbestand

-   Onjuiste BitLocker-sleutels

-   Schijf lezen/schrijven-fouten

## <a name="blob-errors"></a>BLOB-fouten

De volgende items zijn geclassificeerd als blob fouten:

-   Onjuiste of conflicterende blob of namen

-   Ontbrekende bestanden

-   BLOB is niet gevonden

-   Afgekapte bestanden (de bestanden op de schijf kleiner zijn dan die in het manifest is opgegeven)

-   Beschadigd bestand inhoud (voor importtaken gedetecteerd met een MD5-controlesom komt niet overeen)

-   Beschadigde blob-metagegevens en de eigenschap bestanden (gedetecteerd met een MD5-controlesom komt niet overeen)

-   Onjuist schema voor de blob-eigenschappen en/of de bestanden met metagegevens

Mogelijk zijn er gevallen waarbij sommige onderdelen van een taak importeren of exporteren ze niet worden voltooid is, terwijl er wordt nog steeds de algehele taak voltooid. In dit geval kunt u uploaden of downloaden van de ontbrekende onderdelen van de gegevens via het netwerk of u kunt een nieuwe taak om over te dragen van de gegevens maken. Zie de [naslaginformatie voor Azure Import/Export-hulpprogramma](storage-import-export-tool-how-to-v1.md) voor informatie over het herstellen van de gegevens via het netwerk.

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van de Import/Export-service REST-API](storage-import-export-using-the-rest-api.md)
