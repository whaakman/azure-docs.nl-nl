---
title: Azure Functions Event Grid lokale foutopsporing
description: Meer informatie over het lokaal fouten opsporen in Azure functions geactiveerd door een Event Grid-gebeurtenis
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure functions, functions, serverloze architectuur
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 10/18/2018
ms.author: cshoe
ms.openlocfilehash: 7a3468d9a0d128bd51ae742189e60bb8e2af6c0e
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50097862"
---
# <a name="azure-function-event-grid-trigger-local-debugging"></a>Azure-functie Event Grid Trigger lokale foutopsporing

In dit artikel ziet u hoe u fouten opsporen in een lokale functie die verantwoordelijk is voor een Azure Event Grid-gebeurtenis die worden gegenereerd door een storage-account. 

## <a name="prerequisites"></a>Vereisten

- Maak of gebruik een bestaande functie-app
- Maken of een bestaand opslagaccount gebruiken
- Download [ngrok](https://ngrok.com/) waarmee Azure naar uw lokale functie aanroepen

## <a name="create-a-new-function"></a>Een nieuwe functie maken

Open uw functie-app in Visual Studio, met de rechtermuisknop op de naam van het project in Solution Explorer en klikt u op **toevoegen > nieuwe Azure-functie**.

In de *nieuwe Azure-functie* venster **trigger Gebeurtenisraster** en klikt u op **OK**.

![Nieuwe functie maken](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-add-function.png)

Als de functie is gemaakt, opent u het codebestand en kopieer die de URL als commentaar aan de bovenkant van het bestand. Deze locatie wordt gebruikt bij het configureren van de trigger van Event Grid.

![Locatie kopiëren](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-copy-location.png)

Stel een onderbrekingspunt in de regel die met begint `log.LogInformation`.

![Onderbrekingspunt instellen](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-set-breakpoint.png)


Volgende **druk op F5** een sessie voor foutopsporing te starten.

## <a name="allow-azure-to-call-your-local-function"></a>Dat Azure naar uw lokale functie aanroepen

Als u wilt opsplitsen in een functie Foutopsporing wordt uitgevoerd op uw computer, moet u een manier voor Azure om te communiceren met uw lokale functie vanuit de cloud inschakelen.

De [ngrok](https://ngrok.com/) hulpprogramma biedt een manier voor Azure voor het aanroepen van de functie die wordt uitgevoerd op uw computer. Start *ngrok* met de volgende opdracht:

```bash
ngrok http -host-header=localhost 7071
```
Als het hulpprogramma is ingesteld, is het opdrachtvenster ziet die vergelijkbaar is met de volgende schermafbeelding:

![Ngrok starten](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-ngrok.png)

Kopieer de **HTTPS** URL gegenereerd wanneer *ngrok* wordt uitgevoerd. Deze waarde wordt gebruikt bij het configureren van het eindpunt van de event grid-gebeurtenis.

 ## <a name="add-a-storage-event"></a>Een opslag-gebeurtenis toevoegen

Open de Azure-portal en gaat u naar een opslagaccount en klik op de **gebeurtenissen** optie.

![Gebeurtenis voor storage-account toevoegen](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-add-event.png)

In de *gebeurtenissen* venster, klikt u op de **gebeurtenisabonnement** knop. In de *zelfs abonnement* venster, klikt u op de *Eindpunttype* vervolgkeuzelijst en selecteer **Webhook**.

![Selecteer het abonnementstype](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-event-subscription-type.png)

Als het eindpunt van het type is geconfigureerd, klikt u op **selecteert u een eindpunt** het configureren van de waarde van het eindpunt.

![Eindpunt van het type selecteren](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-event-subscription-endpoint.png)

De *eindpunt abonnee* waarde bestaat uit drie verschillende waarden. Het voorvoegsel wordt de HTTPS-URL die is gegenereerd door *ngrok*. De rest van de URL wordt geleverd via de URL die is gevonden in de functie code-bestand met de naam van de functie toegevoegd aan het einde. Beginnen met de URL van het functiecodebestand de *ngrok* URL vervangt `http://localhost:7071` en de functie vervangt de naam `{functionname}`.

De volgende schermafbeelding ziet u hoe de uiteindelijke URL eruit moet zien:

![Eindpunt selecteren](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-event-subscription-endpoint-selection.png)

Nadat u de juiste waarde hebt ingevoerd, klikt u op **keuze bevestigen**.

> [!IMPORTANT]
> Elke keer dat u start *ngrok*, de HTTPS-URL is opnieuw gegenereerd en de waarde wordt gewijzigd. Daarom moet u een nieuwe gebeurtenisabonnement telkens wanneer u de functie met Azure via blootstellen *ngrok*.

## <a name="upload-a-file"></a>Bestand uploaden

U kunt nu een bestand uploaden naar uw storage-account voor het activeren van een Event Grid-gebeurtenis voor uw lokale functie om af te handelen. 

Open [Opslagverkenner](https://azure.microsoft.com/features/storage-explorer/) en maak verbinding met de uw storage-account. 

- Vouw **Blob-Containers** 
- Met de rechtermuisknop op en selecteer **Blob-Container maken**.
- Naam van de container **testen**
- Selecteer de *testen* container
- Klik op de **uploaden** knop
- Klik op **bestanden uploaden**
- Selecteer een bestand en upload het naar de blob-container

## <a name="debug-the-function"></a>Fouten opsporen in de functie

Nadat het Event Grid dat een nieuw bestand wordt geüpload naar de opslagcontainer herkent, wordt het onderbrekingspunt bereikt in de lokale functie.

![Ngrok starten](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-breakpoint.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt opschonen van de resources in dit artikel hebt gemaakt, verwijdert u de **testen** container in uw opslagaccount.

## <a name="next-steps"></a>Volgende stappen

- [Het wijzigen van het formaat van geüploade afbeeldingen automatiseren met behulp van Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md)
- [Trigger Gebeurtenisraster voor Azure Functions](./functions-bindings-event-grid.md)
