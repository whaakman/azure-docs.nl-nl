---
title: Een functie in Azure maken die wordt geactiveerd door Blob Storage | Microsoft Docs
description: Gebruik Azure Functions voor het maken van een functie zonder server die wordt aangeroepen door items die aan Azure Blob Storage zijn toegevoegd.
services: azure-functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: d6bff41c-a624-40c1-bbc7-80590df29ded
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: db95f3991cfc36e0588f94aa7053bf3f5a794222
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-triggered-by-azure-blob-storage"></a>Een door Azure Blob Storage geactiveerde functie maken

Ontdek hoe u een functie maakt die wordt geactiveerd wanneer bestanden worden geüpload naar of bijgewerkt in Azure Blob Storage.

![Bekijk het bericht in de logboeken.](./media/functions-create-storage-blob-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Vereisten

+ De [Microsoft Azure Storage Explorer](http://storageexplorer.com/) downloaden en installeren.
+ Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Een Azure-functie-app maken

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![De functie-app is gemaakt.](./media/functions-create-first-azure-function/function-app-create-success.png)

Vervolgens maakt u een functie in de nieuwe functie-app.

<a name="create-function"></a>

## <a name="create-a-blob-storage-triggered-function"></a>Een door Blob Storage geactiveerde functie maken

1. Vouw de functie-app uit en klik op de knop **+** naast **Functies**. Als dit de eerste functie in de functie-app is, selecteert u **Aangepaste functie**. U ziet nu de volledige set het functiesjablonen.

    ![De Quick Start-pagina van Functions in Azure Portal](./media/functions-create-storage-blob-triggered-function/add-first-function.png)

2. Selecteer de sjabloon **BlobTrigger** voor de gewenste taal en gebruik de instellingen die zijn opgegeven in de tabel.

    ![Maak de door Blob Storage geactiveerde functie.](./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal.png)

    | Instelling | Voorgestelde waarde | Beschrijving |
    |---|---|---|
    | **Pad**   | mycontainer/{name}    | Locatie in Blob Storage die wordt bewaakt. De bestandsnaam van de blob wordt doorgegeven in de binding als de _naam_-parameter.  |
    | **Opslagaccountverbinding** | AzureWebJobStorage | U kunt de opslagaccountverbinding gebruiken die al door de functie-app wordt gebruikt of u kunt een nieuwe maken.  |
    | **Een naam voor de functie opgeven** | Uniek in uw functie-app | Naam van deze door Blob geactiveerde functie. |

3. Klik op **Maken** om de functie te maken.

Vervolgens maakt u verbinding met uw Azure Storage-account en maakt u de **mycontainer**-container.

## <a name="create-the-container"></a>De container maken

1. Klik in de functie op **Integreren**, vouw **Documentatie** uit en kopieer de **Accountnaam** en de **Accountsleutel**. Met deze referenties kunt u verbinding maken met het opslagaccount. Als u uw opslagaccount al hebt verbonden, gaat u naar stap 4.

    ![Haal de verbindingsreferenties voor het opslagaccount op.](./media/functions-create-storage-blob-triggered-function/functions-storage-account-connection.png)

1. Voer het hulpprogramma [Microsoft Azure Storage Explorer](http://storageexplorer.com/) uit, klik op het verbindingspictogram aan de linkerkant, kies **Een opslagaccountnaam en -sleutel gebruiken** en klik op **Volgende**.

    ![Voer het hulpprogramma Storage Account Explorer uit.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-1.png)

1. Voer de **Accountnaam** en de **Accountsleutel** van stap 1 in. Klik op **Volgende** en vervolgens op **Verbinding maken**. 

    ![Voer de opslagreferenties in en maak verbinding.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-2.png)

1. Vouw het gekoppelde opslagaccount uit. Klik met de rechtermuisknop op **Blob-containers**, klik op **Blob-container maken**, typ `mycontainer` en druk op Enter.

    ![Maak een opslagwachtrij.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-create-blob-container.png)

U hebt nu een blob-container en u kunt de functie testen door een bestand naar de container te uploaden.

## <a name="test-the-function"></a>De functie testen

1. Blader in Azure Portal naar de functie, vouw de **Logboeken** onderaan de pagina uit en zorg ervoor dat logboekstreaming niet wordt onderbroken.

1. Vouw in Storage Explorer uw opslagaccount, **Blob-containers** en **mycontainer** uit. Klik op **Uploaden** en klik vervolgens op **Bestanden uploaden...**.

    ![Upload een bestand naar de blob-container.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-upload-file-blob.png)

1. Klik in het dialoogvenster **Bestanden uploaden** op het veld **Bestanden**. Blader naar een bestand op uw lokale computer, zoals een afbeeldingsbestand, selecteer dit en klik op **Openen** en vervolgens op **Uploaden**.

1. Ga terug naar de functielogboeken en controleer of de blob is gelezen.

   ![Bekijk het bericht in de logboeken.](./media/functions-create-storage-blob-triggered-function/functions-blob-storage-trigger-view-logs.png)

    >[!NOTE]
    > Wanneer uw functie-app in het standaardverbruiksabonnement wordt uitgevoerd, kan er een vertraging van maximaal een aantal minuten zitten tussen de blob die wordt toegevoegd of bijgewerkt en de functie die wordt geactiveerd. Overweeg om uw functie-app in een App Service-plan uit te voeren, als u lage latentie in uw door blob geactiveerde functies nodig hebt.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt een functie gemaakt die wordt uitgevoerd wanneer er een blob wordt toegevoegd aan of bijgewerkt in Blob Storage. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Zie [Azure Functions Blob storage bindings](functions-bindings-storage-blob.md) (Blob-opslagbindingen in Azure Functions) voor meer informatie over de Blob-opslagtriggers.
