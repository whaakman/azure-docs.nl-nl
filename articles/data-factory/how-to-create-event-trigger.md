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
ms.date: 10/11/2018
ms.author: douglasl
ms.openlocfilehash: 20ee69654a6b19365c9b7c46e1fa11e102168365
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309344"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Een trigger die een pijplijn in reactie op een gebeurtenis wordt uitgevoerd

Dit artikel beschrijft de triggers op basis van gebeurtenissen die u in uw Data Factory-pijplijnen kunt maken.

Gebeurtenisafhankelijke architectuur (EDA) is een algemene integratie patroon voor gegevens die betrekking heeft op productie, detectie, verbruik en de reactie op gebeurtenissen. Scenario's voor gegevensintegratie vereisen vaak Data Factory-klanten voor het activeren van pijplijnen op basis van gebeurtenissen. Data Factory is nu geïntegreerd met [Azure Event Grid](https://azure.microsoft.com/services/event-grid/), waarmee u activeert pijplijnen op een gebeurtenis.

Bekijk de volgende video voor een inleiding van tien minuten en demonstratie van deze functie:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Event-based-data-integration-with-Azure-Data-Factory/player]


> [!NOTE]
> Afhankelijk van de integratie in dit artikel beschreven [Azure Event Grid](https://azure.microsoft.com/services/event-grid/). Zorg ervoor dat uw abonnement is geregistreerd bij de resourceprovider van Event Grid. Zie voor meer informatie, [resourceproviders en typen](../azure-resource-manager/resource-manager-supported-services.md#portal).

## <a name="data-factory-ui"></a>Gebruikersinterface van Data Factory

### <a name="create-a-new-event-trigger"></a>Een nieuwe gebeurtenistrigger maken

Een typische gebeurtenis is de aankomst van een bestand of de verwijdering van een bestand in uw Azure Storage-account. U kunt een trigger die op deze gebeurtenis in uw Data Factory-pijplijn reageert kunt maken.

> [!NOTE]
> Deze integratie ondersteunt alleen versie 2 Storage-accounts (algemeen gebruik).

![Nieuwe gebeurtenistrigger maken](media/how-to-create-event-trigger/event-based-trigger-image1.png)

### <a name="configure-the-event-trigger"></a>Configureren van de gebeurtenistrigger

Met de **blobpad begint met** en **eindigt op het pad naar** eigenschappen, kunt u de containers, mappen en waarvoor u wenst te ontvangen van gebeurtenissen van blob-namen. U kunt verschillende patronen gebruiken voor zowel **blobpad begint met** en **eindigt op het pad naar** eigenschappen, zoals wordt weergegeven in de voorbeelden verderop in dit artikel. Ten minste één van deze eigenschappen is vereist.

![Configureren van de gebeurtenistrigger](media/how-to-create-event-trigger/event-based-trigger-image2.png)

### <a name="select-the-event-trigger-type"></a>Selecteer het type gebeurtenis trigger

Als u het bestand binnenkomt in de opslaglocatie van uw en de bijbehorende blob is gemaakt, wordt deze gebeurtenis wordt geactiveerd en wordt uw Data Factory-pijplijn wordt uitgevoerd. U kunt een trigger die op een gebeurtenis van een blob gemaakt, een blob-verwijdering gebeurtenis of beide gebeurtenissen in uw Data Factory-pijplijnen reageert maken.

![Selecteer triggertype als gebeurtenis](media/how-to-create-event-trigger/event-based-trigger-image3.png)

### <a name="map-trigger-properties-to-pipeline-parameters"></a>Trigger-eigenschappen toewijzen aan pijplijnparameters

Wanneer een gebeurtenistrigger wordt geactiveerd voor een specifieke blob, de gebeurtenis worden vastgelegd voor de map pad en de naam van de blob in de eigenschappen `@triggerBody().folderPath` en `@triggerBody().fileName`. Als u de waarden van deze eigenschappen in een pijplijn, moet u de eigenschappen worden toegewezen aan de pijplijnparameters. Nadat u de eigenschappen toewijzen aan parameters, kunt u toegang tot de waarden die zijn vastgelegd door de trigger via de `@pipeline().parameters.parameterName` expressie in de pijplijn.

![Toewijzing van eigenschappen aan pijplijnparameters](media/how-to-create-event-trigger/event-based-trigger-image4.png)

Bijvoorbeeld, in de vorige schermafbeelding. de trigger is geconfigureerd om te worden geactiveerd wanneer een blobpad hebben die eindigt op `.csv` in het Opslagaccount dat wordt gemaakt. Als gevolg hiervan, wanneer een blob met de `.csv` extensie overal in het Opslagaccount is gemaakt de `folderPath` en `fileName` eigenschappen vastleggen van de locatie van de nieuwe blob. Bijvoorbeeld, `@triggerBody().folderPath` heeft een waarde, zoals `/containername/foldername/nestedfoldername` en `@triggerBody().fileName` heeft een waarde, zoals `filename.csv`. Deze waarden worden toegewezen in het voorbeeld in op de pijplijnparameters `sourceFolder` en `sourceFile`. U kunt deze gebruiken in de pijplijn als `@pipeline().parameters.sourceFolder` en `@pipeline().parameters.sourceFile` respectievelijk.

## <a name="json-schema"></a>JSON-schema

De volgende tabel bevat een overzicht van de schema-elementen die betrekking op gebeurtenissen gebaseerde triggers hebben:

| **JSON-Element** | **Beschrijving** | **Type** | **Toegestane waarden** | **Vereist** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **Bereik** | De Azure Resource Manager-resource-ID van het Opslagaccount. | Reeks | Azure Resource Manager-ID | Ja |
| **gebeurtenissen** | Het type gebeurtenissen die ervoor zorgen dat deze trigger wordt geactiveerd. | Matrix    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Ja, een combinatie. |
| **blobPathBeginsWith** | De blobpad moet beginnen met het patroon dat is opgegeven voor de trigger wordt geactiveerd. Bijvoorbeeld, '/ records/blobs/december /' wordt alleen wordt geactiveerd de trigger voor blobs in de map december onder de container records. | Reeks   | | Er moet ten minste één van deze eigenschappen worden opgegeven: blobPathBeginsWith, blobPathEndsWith. |
| **blobPathEndsWith** | De blobpad moet eindigen met het patroon dat is opgegeven voor de trigger wordt geactiveerd. Bijvoorbeeld wordt 'december/boxes.csv' alleen de trigger voor blobs met de naam van de vakken in een map december gestart. | Reeks   | | Er moet ten minste één van deze eigenschappen worden opgegeven: blobPathBeginsWith, blobPathEndsWith. |

## <a name="examples-of-event-based-triggers"></a>Voorbeelden van triggers op basis van gebeurtenissen

Deze sectie bevat voorbeelden van triggerinstellingen voor de op basis van gebeurtenissen.

-   **Blobpad begint met**('/ containername /'), ontvangt u gebeurtenissen voor elke blob in de container.
-   **Blobpad begint met**('/ containername/blobs/mapnaam'), ontvangt u gebeurtenissen voor alle blobs in de containername container en de mapnaam map. U kunt ook verwijzen naar een submap. bijvoorbeeld, ' / containername/blobs/mapnaam/submapnaam /'.
-   **Blobpad begint met**('/ containername/blobs/foldername/file.txt') – gebeurtenissen voor een blob met de naam bestand.txt in de map mapnaam onder de container containername ontvangt.
-   **Het pad naar eindigt met**('bestand.txt') – Receive-opdrachten gebeurtenissen voor een blob met de naam bestand.txt op elk pad.
-   **Het pad naar eindigt met**('/ containername/blobs/file.txt') – gebeurtenissen voor een blob met de naam bestand.txt onder container containername ontvangt.
-   **Het pad naar eindigt met**('foldername/file.txt') – Receive-opdrachten gebeurtenissen voor een blob met de naam bestand.txt in mapnaam map onder elke container.

> [!NOTE]
> U moet nemen de `/blobs/` segment van het pad wanneer u de container en map, container bestand of de container, map en en bestand.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over triggers [pijplijnen uitvoeren en triggers](concepts-pipeline-execution-triggers.md#triggers).
