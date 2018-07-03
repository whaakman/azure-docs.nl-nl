---
title: Verbinding maken met Azure blob-opslag - Azure Logic Apps | Microsoft Docs
description: Maken en beheren van blobs in Azure storage met Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/21/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 49d08135dee4568d1a9d65ec2d22d17ee3bda2ea
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "35294676"
---
# <a name="create-and-manage-blobs-in-azure-blob-storage-with-azure-logic-apps"></a>Maken en beheren van blobs in Azure blob-opslag met Azure Logic Apps

Dit artikel wordt beschreven hoe u toegang tot en beheren van bestanden die zijn opgeslagen als blobs in uw Azure storage-account in een logische app met de Azure Blob Storage-connector. Op die manier kunt u logische apps die automatiseren van taken en werkstromen voor het beheren van uw bestanden maken. Bijvoorbeeld, kunt u logische apps die maken, ophalen, bijwerken en verwijderen van bestanden in uw storage-account maken.

Stel dat u hebt een hulpprogramma dat wordt bijgewerkt op een Azure-website. die fungeert als de trigger voor uw logische app. Wanneer dit gebeurt, kunt u uw logische app bijwerken van een bestand in uw blob storage-container, dit een bewerking in uw logische app is kunt hebben. 

Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Connector-specifieke technische informatie, Zie de <a href="https://docs.microsoft.com/connectors/azureblobconnector/" target="blank">documentatie voor Azure Blob Storage-connector</a>.

## <a name="prerequisites"></a>Vereisten

* Een [Azure storage-account en storage-container](../storage/blobs/storage-quickstart-blobs-portal.md)

* De logische app waar u toegang wilt tot uw Azure blob storage-account hebben. Als uw logische app met een Azure Blob Storage-trigger wilt, moet u een [lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

<a name="add-trigger"></a>

## <a name="add-blob-storage-trigger"></a>Blob storage-trigger toevoegen

In Azure Logic Apps, elke logische app moet beginnen met een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde wordt voldaan. Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine een exemplaar van de logische app maakt en werkstroom van uw app wordt gestart.

Dit voorbeeld laat zien hoe kun u een werkstroom voor logische Apps met de **Azure Blob Storage - wanneer een blob wordt toegevoegd of gewijzigd (alleen eigenschappen)** trigger wanneer de eigenschappen van een blob wordt toegevoegd of bijgewerkt in uw storage-container. 

1. In de Azure portal of Visual Studio, maak een lege logische app, die Logic App Designer wordt geopend. Dit voorbeeld wordt de Azure-portal.

2. Typ 'azure blob' als filter in het zoekvak. Selecteer de gewenste trigger in de lijst met triggers.

   In dit voorbeeld maakt gebruik van deze trigger: **Azure Blob Storage - wanneer een blob wordt toegevoegd of gewijzigd (alleen eigenschappen)**

   ![Trigger selecteren](./media/connectors-create-api-azureblobstorage/azure-blob-trigger.png)

3. Als u wordt gevraagd voor de verbindingsgegevens, [maken van de blob-opslag-verbinding nu](#create-connection). Of, als de verbinding al bestaat, geeft u de benodigde gegevens voor de trigger.

   Selecteer voor dit voorbeeld wordt de container en de map die u wilt bewaken.

   1. In de **Container** vak, selecteer het pictogram van de map.

   2. Kies in de lijst met mappen de punthaak rechts ( **>** ), en blader vervolgens totdat u Zoek en selecteer de gewenste map. 

      ![Map selecteren](./media/connectors-create-api-azureblobstorage/trigger-select-folder.png)

   3. Selecteer het interval en frequentie voor hoe vaak u wilt dat de trigger om te controleren of de map voor wijzigingen.

4. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**.

5. Nu doorgaan met een of meer acties toe te voegen aan uw logische app voor de taken die u wilt uitvoeren met de resultaten van de trigger.

<a name="add-action"></a>

## <a name="add-blob-storage-action"></a>Blob storage-actie toevoegen

In Azure Logic Apps, een [actie](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een stap in uw werkstroom die volgt op een trigger of een andere actie. In dit voorbeeld wordt de logische app begint met de [terugkeertrigger](../connectors/connectors-native-recurrence.md).

1. Open uw logische app in Logic App Designer in de Azure portal of Visual Studio. Dit voorbeeld wordt de Azure-portal.

2. Kies in de Logic App Designer onder de trigger of actie, **nieuwe stap** > **een actie toevoegen**.

   ![Een actie toevoegen](./media/connectors-create-api-azureblobstorage/add-action.png) 

   Als u wilt toevoegen een actie tussen bestaande stappen, Beweeg de muis boven de verbindende pijl. 
   Kies het plusteken (**+**) die wordt weergegeven, en kies vervolgens **een actie toevoegen**.

3. Typ 'azure blob' als filter in het zoekvak. Selecteer de actie die u wilt in de lijst met acties.

   In dit voorbeeld wordt met deze actie: **Azure Blob Storage - blob-inhoud ophalen**

   ![Actie selecteren](./media/connectors-create-api-azureblobstorage/azure-blob-action.png) 

4. Als u wordt gevraagd voor de verbindingsgegevens, [maken van de verbinding van Azure Blob-opslag nu](#create-connection). Of, als de verbinding al bestaat, geeft u de benodigde gegevens voor de actie. 

   Selecteer voor dit voorbeeld wordt het bestand dat u wenst.

   1. Uit de **Blob** vak, selecteer het pictogram van de map.
  
      ![Map selecteren](./media/connectors-create-api-azureblobstorage/action-select-folder.png)

   2. Zoek en selecteer het bestand dat u op basis van een van de blob wenst **Id** getal. U kunt dit vinden **Id** nummer in de metagegevens van de blob die wordt geretourneerd door de eerder beschreven blob storage-trigger.

5. Wanneer u klaar bent, op de werkbalk van de ontwerper, kiest u **opslaan**.
Als u wilt testen van uw logische app, zorg ervoor dat de geselecteerde map een blob bevat.

In dit voorbeeld worden alleen de inhoud voor een blob opgehaald. Als u wilt weergeven van de inhoud, een andere actie toevoegen die wordt een bestand met de blob gemaakt met behulp van een andere connector. Bijvoorbeeld, een OneDrive-actie die u een bestand op basis van de blob-inhoud maakt toevoegen.

<a name="create-connection"></a>

## <a name="connect-to-storage-account"></a>Verbinding maken met de storage-account

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="connector-reference"></a>Connector-verwijzing

Zie voor technische details, zoals triggers en acties limieten, zoals is beschreven in de Swagger-bestand van de connector, de [van de connector-verwijzingspagina](/connectors/azureblobconnector/). 

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
