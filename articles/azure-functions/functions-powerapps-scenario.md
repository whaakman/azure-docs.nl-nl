---
title: Een functie aanroepen vanuit PowerApps | Microsoft Docs
description: Een aangepaste connector maken en vervolgens een functie met behulp van deze connector aanroepen.
services: functions
keywords: cloud-apps, cloud services, PowerApps, zakelijke processen, business-toepassing
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: glenga
ms.reviewer: sunayv
ms.custom: ''
ms.openlocfilehash: 3693b167f196b82c520703fa50ffabb27b555050
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38586973"
---
# <a name="call-a-function-from-powerapps"></a>Een functie van PowerApps aanroepen
De [PowerApps](https://powerapps.microsoft.com) platform is ontworpen voor zakelijke deskundigen om apps zonder traditionele toepassingscode te bouwen. Professionele ontwikkelaars kunnen Azure Functions gebruiken om uit te breiden de functionaliteit van PowerApps, terwijl het afschermen van PowerApps-appbouwers van de technische details.

Bouwt u een app in dit onderwerp op basis van een scenario voor wind maar voor onderhoud. Dit onderwerp leest u over het aanroepen van de functie die u hebt gedefinieerd in [OpenAPI-definitie voor een functie maken](functions-openapi-definition.md). De functie bepaalt of een noodherstelproces op een windturbine rendabel is.

![Voltooide app in PowerApps](media/functions-powerapps-scenario/finished-app.png)

Zie voor meer informatie over het aanroepen van dezelfde functie uit Microsoft Flow [een functie van Microsoft Flow aanroepen](functions-flow-scenario.md).

In dit onderwerp leert u hoe u:

> [!div class="checklist"]
> * Bereid de voorbeeldgegevens in Excel.
> * Een API-definitie exporteren.
> * Een verbinding toevoegen aan de API.
> * Een app maken en toevoegen van gegevensbronnen.
> * Besturingselementen voor het weergeven van gegevens in de app toevoegen.
> * Besturingselementen voor de functie aanroepen en weergeven van gegevens toevoegen.
> * De app om te bepalen of een herstel rendabele uitvoeren.

## <a name="prerequisites"></a>Vereisten

+ Een actief [PowerApps-account](https://docs.microsoft.com/en-us/powerapps/maker/signup-for-powerapps) met hetzelfde teken in referenties als uw Azure-account. 
+ Excel en de [Excel-voorbeeldbestand](https://procsi.blob.core.windows.net/docs/turbine-data.xlsx) die u wilt gebruiken als een gegevensbron voor uw app.
+ Voltooi de zelfstudie [OpenAPI-definitie voor een functie maken](functions-openapi-definition.md).

[!INCLUDE [Export an API definition](../../includes/functions-export-api-definition.md)]

## <a name="add-a-connection-to-the-api"></a>Een verbinding toevoegen aan de API
De aangepaste API (ook wel bekend als een aangepaste connector) is beschikbaar in PowerApps, maar u moet verbinding maken met de API voordat u deze in een app gebruiken kunt.

1. In [web.powerapps.com](https://web.powerapps.com), klikt u op **verbindingen**.

    ![PowerApps-verbindingen](media/functions-powerapps-scenario/powerapps-connections.png)

1. Klik op **nieuwe verbinding**, schuif omlaag naar de **Turbine herstellen** -connector, en klik erop.

    ![Nieuwe verbinding](media/functions-powerapps-scenario/new-connection.png)

1. Voer de API-sleutel en klikt u op **maken**.

    ![Verbinding maken](media/functions-powerapps-scenario/create-connection.png)

> [!NOTE]
> Als u uw app met anderen deelt, moet elke persoon die werkt op of de app gebruikt de API-sleutel verbinding maken met de API ook invoeren. Dit gedrag veranderen in de toekomst en we in dit onderwerp om te geven die wordt bijgewerkt.

## <a name="create-an-app-and-add-data-sources"></a>Een app maken en toevoegen van gegevensbronnen
U bent nu klaar om te maken van de app in PowerApps en de Excel-gegevens en de aangepaste API toevoegen als gegevensbronnen voor de app.

1. Kies op [web.powerapps.com](https://web.powerapps.com) achtereenvolgens **Beginnen met een lege app** > ![Phone-app-pictogram](media/functions-powerapps-scenario/icon-phone-app.png) (telefoon) > **Deze app maken**.

    ![Volledig nieuwe stroom - telefoon-app](media/functions-powerapps-scenario/create-phone-app.png)

    De app wordt geopend in PowerApps Studio voor Internet. De volgende afbeelding ziet u de verschillende onderdelen van PowerApps Studio.

    ![PowerApps Studio](media/functions-powerapps-scenario/powerapps-studio.png)

    **(A) de linker navigatiebalk**, waarin er een hiërarchische weergave van alle besturingselementen op elk scherm

    **(B) het middelste deelvenster**, zodat u ziet het scherm dat u werkt

    **(C) rechter venster**, waar u opties instelt, zoals indeling en gegevensbronnen

    **(D) de eigenschap** vervolgkeuzelijst, waarin u de eigenschappen die betrekking hebben op formules selecteren

    **(E) de formulebalk**, waar u formules (zoals in Excel) die app-gedrag bepalen toevoegen
    
    **(F) lint**, waar u besturingselementen toevoegt en past u ontwerpelementen aan

1. Voeg het Excel-bestand toe als een gegevensbron.

    De gegevens die u gaat importeren ziet er als volgt uit:

    ![Excel-gegevens te importeren](media/functions-powerapps-scenario/excel-table.png)

    1. Kies **verbinding maken met gegevens** op het app-canvas.

    1. Op de **gegevens** deelvenster, klikt u op **statische gegevens toevoegen aan uw app**.

        ![Gegevensbron toevoegen](media/functions-powerapps-scenario/add-static-data.png)

        Normaal gesproken u lezen en schrijven van gegevens vanaf een externe bron, maar u kunt de Excel-gegevens wilt toevoegen als statische gegevens omdat dit een voorbeeld is.

    1. Navigeer naar de Excel-bestand dat u hebt opgeslagen, selecteert u de **maar** tabel en klik op **Connect**.

        ![Gegevensbron toevoegen](media/functions-powerapps-scenario/choose-table.png)


1. De aangepaste API toevoegen als een gegevensbron.

    1. Op de **gegevens** deelvenster, klikt u op **gegevensbron toevoegen**.

    1. Klik op **Turbine herstellen**.

        ![Turbine repair-connector](media/functions-powerapps-scenario/turbine-connector.png)

## <a name="add-controls-to-view-data-in-the-app"></a>Besturingselementen voor het weergeven van gegevens in de app toevoegen
Nu dat de gegevensbronnen in de app beschikbaar zijn, toevoegen u een scherm aan uw app, zodat u de turbine-gegevens kunt bekijken.

1. Op de **Start** tabblad **nieuw scherm** > **lijstscherm**.

    ![Lijstscherm](media/functions-powerapps-scenario/list-screen.png)

    PowerApps wordt een scherm waarin een *galerie* om weer te geven items en andere besturingselementen waarmee zoeken, sorteren en filteren.

1. Wijzigen van de titelbalk `Turbine Repair`, en het formaat van de galerie, zodat er ruimte is om meer besturingselementen daaronder.

    ![De titel wijzigen en het formaat van de galerie](media/functions-powerapps-scenario/gallery-title.png)

1. Met de galerie met geselecteerd, in het rechter deelvenster, klikt u onder **eigenschappen**, klikt u op **CustomGallerySample**.

    ![Gegevensbron wijzigen](media/functions-powerapps-scenario/change-data-source.png)

1. In de **gegevens** Configuratiescherm, selecteer **maar** in de lijst.

    ![Gegevensbron selecteren](media/functions-powerapps-scenario/select-data-source.png)

    De gegevensset bevat een afbeelding, dus nu dat u de indeling voor een betere aansluiting op de gegevens wijzigen. 

1. Klik in de **gegevens** deelvenster, wijzigt u **lay-out** naar **titel, subtitel en hoofdtekst**.

    ![Galerie-indeling wijzigen](media/functions-powerapps-scenario/change-layout.png)

1. Als de laatste stap in de **gegevens** deelvenster, wijzigt u de velden die worden weergegeven in de galerie.

    ![Galerievelden wijzigen](media/functions-powerapps-scenario/change-fields.png)
    
    + **Body1** LastServiceDate =
    + **Subtitle2** ServiceRequired =
    + **Title2** = Title 

1. De galerie selecteert, stelt de **TemplateFill** eigenschap op de volgende formule: `If(ThisItem.IsSelected, Orange, White)`.

    ![Sjabloon opvulling formule](media/functions-powerapps-scenario/formula-fill.png)

    Het is nu gemakkelijker om te zien welke galerie-item is geselecteerd.

    ![Geselecteerde item](media/functions-powerapps-scenario/selected-item.png)

1. U hoeft niet het oorspronkelijke scherm in de app. In het linkerdeelvenster Beweeg de muisaanwijzer over **scherm1**, klikt u op **...** , en **verwijderen**.

    ![Scherm verwijderen](media/functions-powerapps-scenario/delete-screen.png)

1. Klik op **bestand**, en de naam van de app. Klik op **opslaan** in het linkermenu en klik vervolgens op **opslaan** in de rechterbenedenhoek.

Er is een heleboel andere opmaak die u doorgaans zou doen in een productie-app, maar gaan we verder met het belangrijk deel voor dit scenario - aanroepen van de functie.

## <a name="add-controls-to-call-the-function-and-display-data"></a>Besturingselementen voor de functie aanroepen en weergeven van gegevens toevoegen
U hebt een app die gegevens van de samenvatting voor elke turbine worden weergegeven, dus nu is het tijd om toe te voegen besturingselementen die de functie die u hebt gemaakt en weergeven van de gegevens die wordt geretourneerd. U toegang tot de functie op basis van de manier waarop die u deze naam in de OpenAPI-definitie; in dit geval heeft `TurbineRepair.CalculateCosts()`.

1. In het lint, op de **invoegen** tabblad **knop**. Klik op hetzelfde tabblad **Label**

    ![Knop invoegen en label](media/functions-powerapps-scenario/insert-controls.png)

1. Sleep de knop en het label onder de galerie en grootte van het label. 

1. Selecteer de knoptekst en wijzig deze in `Calculate costs`. De app moet lijken op de volgende afbeelding.

    ![App met de knop](media/functions-powerapps-scenario/move-button-label.png)

1. Selecteer de knop en voer de volgende formule voor de knop **OnSelect** eigenschap.

    ```
    If (BrowseGallery1.Selected.ServiceRequired="Yes", ClearCollect(DetermineRepair, TurbineRepair.CalculateCosts({hours: BrowseGallery1.Selected.EstimatedEffort, capacity: BrowseGallery1.Selected.MaxOutput})))
    ```
    Deze formule wordt uitgevoerd wanneer de knop wordt geklikt, en de volgende worden als het geselecteerde galerie-item heeft een **ServiceRequired** waarde van `Yes`:

    + Wist de *verzameling* `DetermineRepair` om gegevens te verwijderen uit de vorige aanroepen. Een verzameling is een variabele in tabelvorm.

    + Wordt toegewezen aan de verzameling van de gegevens die zijn geretourneerd door het aanroepen van de functie `TurbineRepair.CalculateCosts()`. 
    
        De waarden worden doorgegeven aan de functie afkomstig zijn van de **EstimatedEffort** en **MaxOutput** velden voor het geselecteerde item in de galerie. Deze velden worden niet weergegeven in de galerie, maar ze zijn nog steeds beschikbaar voor gebruik in formules.

1. Selecteer het label en voer de volgende formule voor van het label **tekst** eigenschap.

    ```
    "Repair decision: " & First(DetermineRepair).message & " | Cost: " & First(DetermineRepair).costToFix & " | Revenue: " & First(DetermineRepair).revenueOpportunity
    ```
    Deze formule gebruikt de `First()` functie voor toegang tot de eerste (en enige) rij van de `DetermineRepair` verzameling. Vervolgens wordt weergegeven de drie waarden die de functie geeft als resultaat: `message`, `costToFix`, en `revenueOpportunity`. Deze waarden zijn leeg voordat de app wordt uitgevoerd voor de eerste keer.

    De voltooide app ziet er als de volgende afbeelding.

    ![Voltooide app voordat uitvoeren](media/functions-powerapps-scenario/finished-app-before-run.png)


## <a name="run-the-app"></a>De app uitvoeren
U beschikt over een complete app. Het is nu tijd om te voeren en de functie aanroept in actie zien.

1. Klik in de rechterbovenhoek van PowerApps Studio op de knop uitvoeren: ![Knop app uitvoeren](media/functions-powerapps-scenario/f5-arrow-sm.png).

1. Selecteer een turbine met een waarde van `Yes` voor **ServiceRequired**, klikt u vervolgens op de **kosten berekenen** knop. U ziet een resultaat zoals in de volgende afbeelding.

    ![Voltooide app in PowerApps](media/functions-powerapps-scenario/finished-app.png)

1. De andere maar als u wilt zien wat wordt geretourneerd door de functie elke keer proberen.

## <a name="next-steps"></a>Volgende stappen
In dit onderwerp hebt u geleerd hoe u:

> [!div class="checklist"]
> * Bereid de voorbeeldgegevens in Excel.
> * Een API-definitie exporteren.
> * Een verbinding toevoegen aan de API.
> * Een app maken en toevoegen van gegevensbronnen.
> * Besturingselementen voor het weergeven van gegevens in de app toevoegen.
> * Besturingselementen voor de functie aanroepen en weergeven van gegevens toevoegen
> * De app om te bepalen of een herstel rendabele uitvoeren.

Zie voor meer informatie over PowerApps, [Inleiding tot PowerApps](https://powerapps.microsoft.com/tutorials/getting-started/).

Zie voor meer informatie over andere interessante scenario's die gebruikmaken van Azure Functions, [een functie van Microsoft Flow aanroepen](functions-flow-scenario.md) en [een functie maken die kan worden geïntegreerd met Azure Logic Apps](functions-twitter-email.md).