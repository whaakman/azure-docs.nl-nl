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
ms.date: 06/24/2018
ms.author: douglasl
ms.openlocfilehash: 2bcb0d4e6af00b56d083690439be45379ce4d175
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752806"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Een trigger die wordt uitgevoerd een pijplijn in reactie op een gebeurtenis maken

In dit artikel beschrijft de triggers op basis van gebeurtenissen die u in uw Data Factory-pijplijnen kunt maken.

Architectuur gebeurtenisafhankelijke (EDA) is een algemene gegevens integratie patroon dat betrekking heeft op productie, detectie, verbruik en de reactie op gebeurtenissen. Gegevens integratiescenario's vereisen vaak Data Factory-klanten voor het activeren van pijplijnen op basis van gebeurtenissen.

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
-   **Blobpad begint met**('/ containername/mapnaam') – gebeurtenissen voor alle blobs in de container containername en de mapnaam map ontvangt.
-   **Blobpad begint met**('/ containername/foldername/file.txt') – gebeurtenissen voor een blob met de naam bestand.txt in de mapnaam map onder de container containername ontvangt.
-   **Blobpad eindigt met**('bestand.txt') – Receive-opdrachten gebeurtenissen voor een blob met de naam bestand.txt op elk willekeurig pad.
-   **Blobpad eindigt met**('/ containername/file.txt') – gebeurtenissen voor een blob met de naam bestand.txt onder de container containername ontvangt.
-   **Blobpad eindigt met**('foldername/file.txt') – Receive-opdrachten gebeurtenissen voor een blob met de naam bestand.txt in de map mapnaam onder elke container.

## <a name="next-steps"></a>Volgende stappen
Zie voor gedetailleerde informatie over triggers [Pipeline-uitvoering en triggers](concepts-pipeline-execution-triggers.md#triggers).
