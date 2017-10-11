---
title: Azure blob storage Connector in uw logische Apps toevoegen | Microsoft Docs
description: Aan de slag en de Azure blob storage-connector in een logische app configureren
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5dc3f75-6bea-420b-b250-183668d2848d
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/02/2017
ms.author: mandia; ladocs
ms.openlocfilehash: bc7908868828bd1628633cf9e57f8c44f8000827
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-azure-blob-storage-connector-in-a-logic-app"></a>Gebruik de Azure blob storage-connector in een logische app
De Azure Blob storage-connector gebruiken om te uploaden, bijwerken, ophalen en verwijderen van blobs in uw opslagaccount, alle binnen een logische app.  

Met Azure blob storage, u:

* De werkstroom maken door nieuwe projecten uploaden of ophalen van bestanden die onlangs zijn bijgewerkt.
* Acties bestandsmetagegevens ophalen, verwijderen van een bestand en bestanden kopiëren gebruiken. Bijvoorbeeld wanneer een hulpprogramma is bijgewerkt in een Azure-website (een trigger), vervolgens bijwerken een bestand in blob storage (een actie). 

Dit onderwerp leest u het gebruik van de blob-opslag-connector in een logische app.

Zie voor meer informatie over Logic Apps, [wat zijn logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) en [een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md).

Zie voor meer informatie over Logic Apps, [wat zijn logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) en [een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-azure-blob-storage"></a>Verbinding maken met Azure blob-opslag
Om uw logische app toegang alle services tot, maakt u eerst een *verbinding* naar de service. Een verbinding biedt connectiviteit tussen een logische app en een andere service. Bijvoorbeeld, als u wilt verbinding maken met een opslagaccount, u eerst maken een blob-opslag *verbinding*. Voer de referenties die u gebruikt om toegang tot de service die u verbinding met maakt een verbinding wilt maken. Voer dus de referenties naar uw storage-account om de verbinding te maken met Azure storage. 

#### <a name="create-the-connection"></a>De verbinding maken
> [!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="use-a-trigger"></a>Een trigger te gebruiken
Deze connector heeft geen triggers bestaan niet. Met andere triggers kunt starten van de logische app, zoals een trigger terugkeerpatroon, een HTTP-Webhook-trigger, triggers beschikbaar met andere connectors en meer. [Een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md) bevat een voorbeeld.

## <a name="use-an-action"></a>Gebruik een actie
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app.

1. Selecteer het plusteken. Ziet u verschillende mogelijkheden: **een actie toevoegen**, **een voorwaarde toevoegen**, of een van de **meer** opties.
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. Kies **een actie toevoegen**.
3. Typ 'blob' om een lijst met alle beschikbare acties in het tekstvak.
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. Kies in ons voorbeeld **AzureBlob - metagegevens van bestand ophalen met behulp van pad**. Als er al een verbinding bestaat, selecteert u de **...** Knop (objectkiezer weergeven) om een bestand te selecteren.
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    Als u wordt gevraagd om de verbindingsinformatie, voert u de details om de verbinding te maken. [De verbinding](connectors-create-api-azureblobstorage.md#create-the-connection) in dit onderwerp beschrijft deze eigenschappen. 
   
   > [!NOTE]
   > In dit voorbeeld wordt de metagegevens van een bestand ophalen. Overzicht van de metagegevens, een andere actie die u maakt een nieuw bestand met een andere connector toevoegen. Bijvoorbeeld, een OneDrive-actie die u een nieuw bestand maakt 'test' op basis van de metagegevens toevoegen. 


5. **Sla** uw wijzigingen (linkerbovenhoek van de werkbalk). Uw logische app wordt opgeslagen en automatisch kan worden ingeschakeld.

> [!TIP]
> [Opslagverkenner](http://storageexplorer.com/) is een uitstekend hulpprogramma voor het beheren van meerdere opslagaccounts.

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/azureblobconnector/). 

## <a name="next-steps"></a>Volgende stappen
[Een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md). Bekijk de beschikbare connectors in Logic Apps op onze [API's lijst](apis-list.md).

