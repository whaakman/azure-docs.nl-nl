---
title: Verbinding maken met Azure blob-opslag - Azure Logic Apps | Microsoft Docs
description: Maken en beheren van de blobs in Azure-opslag met Azure Logic Apps
author: ecfan
manager: cfowler
ms.author: estfan
ms.date: 05/21/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 15d737cd85f70717bfdf15dfb3d179f977b63c72
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "34723429"
---
# <a name="create-and-manage-blobs-in-azure-blob-storage-with-azure-logic-apps"></a>Maken en beheren van de blobs in Azure blob storage met Azure Logic Apps

Dit artikel laat zien hoe u kunt toegang tot en beheer van bestanden die zijn opgeslagen als blobs in uw Azure storage-account in een logische app met de Azure Blob Storage-connector. Op die manier kunt u logische apps voor het automatiseren van taken en werkstromen voor het beheren van uw bestanden maken. U kunt bijvoorbeeld bouwen van logic apps die maken, verkrijgen, bijwerken en verwijderen van bestanden in uw opslagaccount.

Stel dat u een hulpprogramma dat wordt bijgewerkt op een Azure-website. die fungeert als de trigger voor uw logische app. Als deze gebeurtenis gebeurt, kunt u uw logische app bijwerken van een bestand in de blob storage-container, dit een bewerking in uw logische app is kunt hebben. 

Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Voor connector-specifieke technische informatie, Zie de <a href="https://docs.microsoft.com/connectors/azureblobconnector/" target="blank">Azure Blob Storage-connector verwijzing</a>.

## <a name="prerequisites"></a>Vereisten

* Een [Azure storage-account en storage-container](../storage/blobs/storage-quickstart-blobs-portal.md)

* De logische app waar u toegang wilt tot uw Azure blob storage-account hebben. U kunt uw logische app starten met een Azure Blob Storage-trigger, moet u een [lege logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

<a name="add-trigger"></a>

## <a name="add-blob-storage-trigger"></a>Blob storage-trigger toevoegen

In Azure Logic Apps elke logische app moet beginnen met een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde is voldaan. Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine maakt een logische app-exemplaar en de werkstroom van uw app wordt gestart.

Dit voorbeeld ziet u hoe u kunt een logic app werkstroom starten met de **Azure Blob-opslag - als een blob is toegevoegd of gewijzigd (alleen eigenschappen)** trigger wanneer een blob eigenschappen opgehaald toegevoegd of bijgewerkt in uw storage-container. 

1. In de Azure-portal of Visual Studio maakt een lege logische app, dat wordt geopend Logic App-ontwerper. In dit voorbeeld wordt de Azure-portal.

2. Voer in het zoekvak 'azure blob' als filter. Selecteer de gewenste trigger in de lijst triggers.

   In dit voorbeeld wordt deze trigger: **Azure Blob-opslag - als een blob is toegevoegd of gewijzigd (alleen eigenschappen)**

   ![Trigger selecteren](./media/connectors-create-api-azureblobstorage/azure-blob-trigger.png)

3. Als u wordt gevraagd om de details voor verbinding [maken van de blob-opslag-verbinding nu](#create-connection). Of als uw verbinding al aanwezig is, geeft u de benodigde gegevens voor de trigger.

   Selecteer voor dit voorbeeld wordt de container en map die u wilt bewaken.

   1. In de **Container** Selecteer het pictogram van de map.

   2. Kies in de lijst met mappen de punthaak rechts ( **>** ), en blader vervolgens totdat u zoeken naar en selecteer de gewenste map. 

      ![Selecteer de map](./media/connectors-create-api-azureblobstorage/trigger-select-folder.png)

   3. Selecteer de interval en frequentie voor hoe vaak de trigger wordt de map gecontroleerd op wijzigingen.

4. Wanneer u, klik op de werkbalk ontwerpen bent klaar kiezen **opslaan**.

5. Nu doorgaan met een of meer acties toe te voegen aan uw logische app voor de taken die u wilt uitvoeren met de trigger-resultaten.

<a name="add-action"></a>

## <a name="add-blob-storage-action"></a>Blob storage actie toevoegen

In Azure Logic Apps een [actie](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een stap in de werkstroom die na een trigger of een andere actie. In dit voorbeeld wordt de logische app begint met de [terugkeerpatroon trigger](../connectors/connectors-native-recurrence.md).

1. Open uw logische app in App-ontwerper voor logica in de Azure-portal of Visual Studio. In dit voorbeeld wordt de Azure-portal.

2. Kies in de ontwerpfunctie Logic App, onder de trigger of actie **nieuwe stap** > **een actie toevoegen**.

   ![Een actie toevoegen](./media/connectors-create-api-azureblobstorage/add-action.png) 

   Als u een actie tussen bestaande stappen toevoegen, wilt u de muisaanwijzer over de verbindende pijl. 
   Kies het plusteken (**+**) die wordt weergegeven, en kies vervolgens **een actie toevoegen**.

3. Voer in het zoekvak 'azure blob' als filter. Selecteer de actie die u wilt in de lijst van acties.

   In dit voorbeeld wordt met deze actie: **Azure Blob Storage - blob-inhoud ophalen**

   ![Selecteer actie](./media/connectors-create-api-azureblobstorage/azure-blob-action.png) 

4. Als u wordt gevraagd om de details voor verbinding [maken van de verbinding van Azure Blob Storage nu](#create-connection). Of als uw verbinding al aanwezig is, geeft u de benodigde gegevens voor de actie. 

   Selecteer het bestand dat u wilt dat voor dit voorbeeld.

   1. Van de **Blob** Selecteer het pictogram van de map.
  
      ![Selecteer de map](./media/connectors-create-api-azureblobstorage/action-select-folder.png)

   2. Zoek en selecteer het bestand dat u wilt dat op basis van de blob **Id** getal. U kunt dit vinden **Id** nummer in de blob-metagegevens die wordt geretourneerd door de eerder beschreven blob storage-trigger.

5. Wanneer u, klik op de werkbalk ontwerpen bent klaar kiezen **opslaan**.
Als u wilt uw logische app testen, zorg ervoor dat de geselecteerde map een blob bevat.

In dit voorbeeld wordt alleen de inhoud van een blob. Toevoegen om weer te geven van de inhoud, een andere actie die u een bestand met de blob maakt met behulp van een andere connector. Bijvoorbeeld, een OneDrive-actie die u een bestand op basis van de blobinhoud maakt toevoegen.

<a name="create-connection"></a>

## <a name="connect-to-storage-account"></a>Verbinding maken met de storage-account

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="connector-reference"></a>Connector-verwijzing

Zie voor technische details, zoals triggers, acties en limieten, zoals is beschreven door de connector Swagger-bestand, de [van connector-verwijzingspagina](/connectors/azureblobconnector/). 

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
