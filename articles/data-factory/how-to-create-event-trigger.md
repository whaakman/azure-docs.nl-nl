---
title: Triggers op basis van gebeurtenissen maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van een trigger in Azure Data Factory die een pijplijn in reactie op een gebeurtenis wordt uitgevoerd.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: douglasl
ms.openlocfilehash: a9c15b239ee0bd0dde0b1f11691565b2676e3d07
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062118"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Een trigger die wordt uitgevoerd een pijplijn in reactie op een gebeurtenis maken

In dit artikel beschrijft de triggers op basis van gebeurtenissen die u in uw Data Factory-pijplijnen kunt maken.

Architectuur gebeurtenisafhankelijke (EDA) is een algemene gegevens integratie patroon dat betrekking heeft op productie, detectie, verbruik en de reactie op gebeurtenissen. Gegevens integratiescenario's vereisen vaak Data Factory-klanten voor het activeren van pijplijnen op basis van gebeurtenissen. Data Factory is nu geïntegreerd met [Azure gebeurtenis raster](https://azure.microsoft.com/services/event-grid/), waarmee u activeren pijplijnen op een gebeurtenis.

## <a name="data-factory-ui"></a>Gebruikersinterface van Data Factory

### <a name="create-a-new-event-trigger"></a>Maak een nieuwe gebeurtenistrigger

Een typische gebeurtenis is de aankomst van een bestand of het verwijderen van een bestand in uw Azure Storage-account. U kunt een trigger dat met deze gebeurtenis in de Data Factory-pijplijn overeenkomt maken.

> [!NOTE]
> Deze integratie ondersteunt alleen versie 2 opslagaccounts (Algemeen).

![Nieuwe gebeurtenistrigger maken](media/how-to-create-event-trigger/event-based-trigger-image1.png)

### <a name="select-the-event-trigger-type"></a>Selecteer het type gebeurtenis trigger

Als u het bestand in de opslaglocatie ontvangt en de bijbehorende blob wordt gemaakt, wordt deze gebeurtenis wordt geactiveerd en wordt uw Data Factory-pijplijn wordt uitgevoerd. U kunt een trigger dat met een blob-gebeurtenis voor het maken van een blob verwijdering gebeurtenis of beide gebeurtenissen, in uw Data Factory-pijplijnen overeenkomt kunt maken.

![Selecteer triggertype als gebeurtenis](media/how-to-create-event-trigger/event-based-trigger-image2.png)

### <a name="configure-the-event-trigger"></a>De gebeurtenistrigger configureren

Met de **blobpad begint met** en **blobpad eindigt met** eigenschappen, kunt u de containers, mappen en blob-namen die u wilt ontvangen van gebeurtenissen. U kunt verschillende patronen voor beide **blobpad begint met** en **blobpad eindigt met** eigenschappen, zoals wordt weergegeven in de voorbeelden verderop in dit artikel. Ten minste één van deze eigenschappen is vereist.

![De gebeurtenistrigger configureren](media/how-to-create-event-trigger/event-based-trigger-image3.png)

## <a name="json-schema"></a>JSON-schema

De volgende tabel bevat een overzicht van de schema-elementen die betrekking op gebeurtenis gebaseerde triggers hebben:

| **JSON-Element** | **Beschrijving** | **Type** | **Toegestane waarden** | **Vereist** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **Bereik** | De Azure Resource Manager-resource-ID van het Opslagaccount. | Reeks | ID van de Azure Resource Manager | Ja |
| **Gebeurtenissen** | Het type gebeurtenissen die ervoor zorgen dat deze trigger wordt geactiveerd. | Matrix    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Ja, elke combinatie. |
| **blobPathBeginsWith** | De blobpad moet beginnen met het patroon dat is opgegeven voor de trigger wordt geactiveerd. Bijvoorbeeld ' records/blobs december /' wordt alleen worden gestart de trigger voor blobs in de map december onder de container records. | Reeks   | | Er moet ten minste één van deze eigenschappen worden opgegeven: blobPathBeginsWith, blobPathEndsWith. |
| **blobPathEndsWith** | De blobpad moet eindigen met het patroon dat is opgegeven voor de trigger wordt geactiveerd. Bijvoorbeeld wordt 'december/boxes.csv' alleen de trigger voor blobs met de naam in vakken in een map december gestart. | Reeks   | | Er moet ten minste één van deze eigenschappen worden opgegeven: blobPathBeginsWith, blobPathEndsWith. |

## <a name="examples-of-event-based-triggers"></a>Voorbeelden van triggers op basis van gebeurtenissen

Deze sectie bevat voorbeelden van de instellingen op basis van gebeurtenissen trigger.

-   **Blobpad begint met**('/ containername /') – ontvangt van gebeurtenissen voor elke blob in de container.
-   **Blobpad begint met**('/ blobs/containername/mapnaam') – gebeurtenissen voor alle blobs in de container containername en de mapnaam map ontvangt.
-   **Blobpad begint met**('/ containername/blobs/foldername/file.txt') – gebeurtenissen voor een blob met de naam bestand.txt in de mapnaam map onder de container containername ontvangt.
-   **Blobpad eindigt met**('bestand.txt') – Receive-opdrachten gebeurtenissen voor een blob met de naam bestand.txt op elk willekeurig pad.
-   **Blobpad eindigt met**('/ containername/blobs/file.txt') – gebeurtenissen voor een blob met de naam bestand.txt onder de container containername ontvangt.
-   **Blobpad eindigt met**('foldername/file.txt') – Receive-opdrachten gebeurtenissen voor een blob met de naam bestand.txt in de map mapnaam onder elke container.

> [!NOTE]
> U moet nemen de `/blobs/` segment van het pad wanneer u de container en map, container en bestand of de container, map opgeven en het bestand.

## <a name="using-blob-events-trigger-properties"></a>Met behulp van de Trigger-eigenschappen van Blob-gebeurtenissen

Wanneer een blob gebeurtenissen trigger wordt gestart, maakt twee variabelen die beschikbaar zijn voor uw pijplijn: *folderPath* en *fileName*. Voor toegang tot deze variabelen, gebruikt de `@triggerBody().fileName` of `@triggerBody().folderPath` expressies.

Neem bijvoorbeeld een trigger die is geconfigureerd om te worden gestart wanneer een blob is gemaakt met `.csv` als de waarde van `blobPathEndsWith`. Als een CSV-bestand naar de storage-account wordt neergezet de *folderPath* en *fileName* beschrijven de locatie van het CSV-bestand. Bijvoorbeeld: *folderPath* heeft de waarde `/containername/foldername/nestedfoldername` en *fileName* heeft de waarde `filename.csv`.

## <a name="next-steps"></a>Volgende stappen
Zie voor gedetailleerde informatie over triggers [Pipeline-uitvoering en triggers](concepts-pipeline-execution-triggers.md#triggers).
