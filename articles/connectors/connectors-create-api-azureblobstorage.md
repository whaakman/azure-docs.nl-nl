---
title: Verbinding maken met Azure blob-opslag - Azure Logic Apps
description: Maken en beheren van blobs in Azure storage met Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 06/20/2019
tags: connectors
ms.openlocfilehash: d9c29837e99d327112e6a9d648a5c56cc35e8555
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296614"
---
# <a name="create-and-manage-blobs-in-azure-blob-storage-with-azure-logic-apps"></a>Maken en beheren van blobs in Azure blob-opslag met Azure Logic Apps

Dit artikel wordt beschreven hoe u toegang tot en beheren van bestanden die zijn opgeslagen als blobs in uw Azure storage-account in een logische app met de Azure Blob Storage-connector. Op die manier kunt u logische apps die automatiseren van taken en werkstromen voor het beheren van uw bestanden maken. Bijvoorbeeld, kunt u logische apps die maken, ophalen, bijwerken en verwijderen van bestanden in uw storage-account maken.

Stel dat u hebt een hulpprogramma dat wordt bijgewerkt op een Azure-website. die fungeert als de trigger voor uw logische app. Wanneer dit gebeurt, kunt u uw logische app bijwerken van een bestand in uw blob storage-container, dit een bewerking in uw logische app is kunt hebben.

> [!NOTE]
> Logic Apps biedt geen ondersteuning voor rechtstreeks verbinding te maken naar Azure storage-accounts via firewalls. Voor toegang tot deze opslagaccounts, hier een van beide opties gebruiken:
>
> * Maak een [integratieserviceomgeving](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), waarmee verbinding kan maken met bronnen in een Azure-netwerk.
>
> * Als u al API Management gebruikt, kunt u deze service voor dit scenario. Zie voor meer informatie, [integratie van eenvoudige ondernemingsstructuur](https://aka.ms/aisarch).

Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Quick Start: Maak uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Connector-specifieke technische informatie, Zie de [documentatie voor Azure Blob Storage-connector](/connectors/azureblobconnector/).

## <a name="limits"></a>Limits

* Standaard Azure Blob Storage-acties kunnen lezen of schrijven van bestanden die zijn *50 MB of kleiner*. Voor het afhandelen van bestanden die groter zijn dan 50 MB, maar tot 1024 MB, ondersteuning voor Azure Blob Storage-acties [bericht logische groepen te verdelen](../logic-apps/logic-apps-handle-large-messages.md). De **blobinhoud ophalen** actie impliciet gebruikt logische groepen te verdelen.

* Triggers voor Azure Blob-opslag bieden geen ondersteuning voor logische groepen te verdelen. Bij het aanvragen van inhoud, selecteert u alleen bestanden die zijn dan 50 MB triggers of kleiner. Als u bestanden groter zijn dan 50 MB, gaat u als volgt dit patroon:

  * Gebruik van een Azure Blob Storage-trigger die eigenschappen, zoals retourneert **wanneer een blob wordt toegevoegd of gewijzigd (alleen eigenschappen)** .

  * Ga als volgt de trigger met de Azure Blob-opslag **blobinhoud ophalen** actie, die het volledige bestand wordt gelezen en impliciet gebruikt logische groepen te verdelen.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Een [Azure storage-account en storage-container](../storage/blobs/storage-quickstart-blobs-portal.md)

* De logische app waar u toegang wilt tot uw Azure blob storage-account hebben. Als uw logische app met een Azure Blob Storage-trigger wilt, moet u een [lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-trigger"></a>

## <a name="add-blob-storage-trigger"></a>Blob storage-trigger toevoegen

In Azure Logic Apps, elke logische app moet beginnen met een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde wordt voldaan. Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine een exemplaar van de logische app maakt en werkstroom van uw app wordt gestart.

Dit voorbeeld laat zien hoe kun u een werkstroom voor logische Apps met de **wanneer een blob wordt toegevoegd of gewijzigd (alleen eigenschappen)** trigger wanneer de eigenschappen van een blob wordt toegevoegd of bijgewerkt in uw storage-container.

1. In de [Azure-portal](https://portal.azure.com) of Visual Studio, maak een lege logische app, die Logic App Designer wordt geopend. Dit voorbeeld wordt de Azure-portal.

2. Typ 'azure blob' als filter in het zoekvak. Selecteer de gewenste trigger in de lijst met triggers.

   In dit voorbeeld maakt gebruik van deze trigger: **Wanneer een blob wordt toegevoegd of gewijzigd (alleen eigenschappen)**

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

1. In de [Azure-portal](https://portal.azure.com) of Visual Studio, open uw logische app in Logic App Designer. Dit voorbeeld wordt de Azure-portal.

2. Kies in de Logic App Designer onder de trigger of actie, **nieuwe stap**.

   ![Een actie toevoegen](./media/connectors-create-api-azureblobstorage/add-action.png) 

   Als u wilt toevoegen een actie tussen bestaande stappen, Beweeg de muis boven de verbindende pijl. Kies het plusteken ( **+** ) die wordt weergegeven, en selecteer **een actie toevoegen**.

3. Typ 'azure blob' als filter in het zoekvak. Selecteer de actie die u wilt in de lijst met acties.

   In dit voorbeeld wordt met deze actie: **Blobinhoud ophalen**

   ![Actie selecteren](./media/connectors-create-api-azureblobstorage/azure-blob-action.png)

4. Als u wordt gevraagd voor de verbindingsgegevens, [maken van de verbinding van Azure Blob-opslag nu](#create-connection).
Of, als de verbinding al bestaat, geeft u de benodigde gegevens voor de actie.

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

Voor technische informatie, zoals triggers en acties limieten, zoals beschreven door het openen van de connector-API (voorheen Swagger)-bestand, raadpleegt u de [van de connector-verwijzingspagina](/connectors/azureblobconnector/).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
