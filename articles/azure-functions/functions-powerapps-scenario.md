---
title: Aanroepen van een functie van PowerApps | Microsoft Docs
description: Een aangepaste connector maken en vervolgens een functie met die connector aanroept.
services: functions
keywords: cloud-apps, cloud services, PowerApps, bedrijfsprocessen, business-toepassing
documentationcenter: 
author: mgblythe
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: mblythe
ms.custom: 
ms.openlocfilehash: 1e262fde37b68bcfcee3c974deb91bd07965de19
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="call-a-function-from-powerapps"></a>Een functie van PowerApps aanroepen
De [PowerApps](https://powerapps.microsoft.com) platform is ontworpen voor zakelijke deskundigen voor het bouwen van apps zonder traditionele toepassingscode. Professionele ontwikkelaars kunnen Azure Functions gebruiken om de mogelijkheden van PowerApps, terwijl afscherming PowerApps app opbouwfuncties van technische informatie.

U bouwen een app in dit onderwerp op basis van een scenario voor onderhoud voor Windturbines. Dit onderwerp leest u hoe de functie die u hebt gedefinieerd in [maken voor een functie een definitie van een OpenAPI](functions-openapi-definition.md). De functie bepaalt of een noodherstelproces op een turbine o rendabele.

![Voltooide app in PowerApps](media/functions-powerapps-scenario/finished-app.png)

Zie voor meer informatie over het aanroepen van dezelfde functie uit de Microsoft-Flow [aanroepen van een functie van Microsoft-Flow](functions-flow-scenario.md).

In dit onderwerp leert u hoe:

> [!div class="checklist"]
> * Bereid de voorbeeldgegevens in Excel.
> * Exporteren van een API-definitie.
> * Een verbinding toevoegen aan de API.
> * Een app maken en toevoegen van gegevensbronnen.
> * Voeg andere besturingselementen om gegevens in de app weer te geven.
> * Toevoegen van besturingselementen voor de functie aanroepen en gegevens worden weergegeven.
> * Voer de app om te bepalen of een reparatie rendabele.

## <a name="prerequisites"></a>Vereisten

+ Een actieve [PowerApps account](https://powerapps.microsoft.com/tutorials/signup-for-powerapps.md) met hetzelfde teken in referenties als uw Azure-account. 
+ Excel-, omdat u Excel als een gegevensbron voor uw app gebruikt.
+ Voltooi de zelfstudie [maken voor een functie een definitie van een OpenAPI](functions-openapi-definition.md).


## <a name="prepare-sample-data-in-excel"></a>Voorbeeldgegevens in Excel voorbereiden
U beginnen met het voorbereiden van voorbeeldgegevens die u in de app gebruiken. Kopieer de volgende tabel in Excel. 

| Titel      | Breedtegraad  | Longtitude  | LastServiceDate | MaxOutput | ServiceRequired | EstimatedEffort | InspectionNotes                            |
|------------|-----------|-------------|-----------------|-----------|-----------------|-----------------|--------------------------------------------|
| Turbine 1  | 47.438401 | -121.383767 | 2/23/2017       | 2850      | Ja             | 6               | Dit is de tweede probleem van deze maand.       |
| Turbine 4  | 47.433385 | -121.383767 | 5/8/2017        | 5400      | Ja             | 6               |                                            |
| Turbine 33 | 47.428229 | -121.404641 | 6/20/2017       | 2800      |                 |                 |                                            |
| Turbine 34 | 47.463637 | -121.358824 | 2/19/2017       | 2800      | Ja             | 7               |                                            |
| Turbine 46 | 47.471993 | -121.298949 | 3/2/2017        | 1200      |                 |                 |                                            |
| Turbine 47 | 47.484059 | -121.311171 | 8/2/2016        | 3350      |                 |                 |                                            |
| Turbine 55 | 47.438403 | -121.383767 | 10/2/2016       | 2400      | Ja             | 40               | We hebben een aantal onderdelen binnenkort in deze service. |

1. Selecteer de gegevens in Excel en klik op de **Start** tabblad **opmaken als tabel**.

    ![Opmaken als tabel](media/functions-powerapps-scenario/format-table.png)

1. Selecteer een ander opmaakprofiel en klik op **OK**.

1. Met de tabel die is geselecteerd, op de **ontwerp** tabblad `Turbines` voor **tabelnaam**.

    ![De tabelnaam van de](media/functions-powerapps-scenario/table-name.png)

1. Sla de Excel-werkmap.

[!INCLUDE [Export an API definition](../../includes/functions-export-api-definition.md)]

## <a name="add-a-connection-to-the-api"></a>Een verbinding toevoegen aan de API
De aangepaste API (ook wel bekend als een aangepaste connector) is beschikbaar in PowerApps, maar u moet maakt een verbinding met de API voordat u deze in een app kunt gebruiken.

1. In [web.powerapps.com](https://web.powerapps.com), klikt u op **verbindingen**.

    ![Verbindingen met PowerApps](media/functions-powerapps-scenario/powerapps-connections.png)

1. Klik op **nieuwe verbinding**, schuif omlaag naar de **Turbine reparatie** connector, en klik erop.

    ![Nieuwe verbinding](media/functions-powerapps-scenario/new-connection.png)

1. Voer de API-sleutel in en klik op **maken**.

    ![Verbinding maken](media/functions-powerapps-scenario/create-connection.png)

> [!NOTE]
> Als u uw app met anderen deelt, moet elke persoon die werkt op of de app gebruikt de API-sleutel verbinding maken met de API ook invoeren. Dit gedrag in de toekomst kan worden gewijzigd en dit onderwerp om te geven die wordt bijgewerkt.

## <a name="create-an-app-and-add-data-sources"></a>Een app maken en gegevensbronnen toevoegen
U kunt nu klaar om te maken van de app in PowerApps en voeg de Excel-gegevens en de aangepaste API gebruiken als gegevensbronnen voor de app.

1. In [web.powerapps.com](https://web.powerapps.com), in het linkerdeelvenster klikt u op **nieuwe App**.

1. Onder **lege app**, klikt u op **telefoonindeling**.

    ![Tablet PC-app maken](media/functions-powerapps-scenario/create-phone-app.png)

    De app wordt geopend in PowerApps Studio voor webtoepassingen. De volgende afbeelding toont de verschillende onderdelen van PowerApps Studio. Deze installatiekopie is voor de voltooide app; Hier ziet u een leeg scherm eerst in het middelste deelvenster.

    ![PowerApps Studio](media/functions-powerapps-scenario/powerapps-studio.png)

    **(1) linkernavigatiebalk**, waarin u een hiërarchische weergave van alle besturingselementen op elke scherm ziet

    **(2) het middelste deelvenster**, zodat u ziet het scherm dat u werkt op

    **(3) het rechter deelvenster**, waarin u een opties instellen zoals lay-out en gegevensbronnen

    **(4) de eigenschap** vervolgkeuzelijst, waar u de eigenschappen die betrekking hebben op formules selecteren

    **(5) de formulebalk**, waar u formules (zoals in Excel) die app-gedrag definiëren toevoegen
    
    **(6) lint**, waarbij u besturingselementen toevoegen en ontwerpelementen aanpassen

1. Voeg het Excel-bestand als een gegevensbron.

    1. In het rechterdeelvenster op de **gegevens** tabblad **gegevensbron toevoegen**.

        ![Gegevensbron toevoegen](media/functions-powerapps-scenario/add-data-source.png)

    1. Klik op **statische gegevens toevoegen aan uw app**.

        ![Gegevensbron toevoegen](media/functions-powerapps-scenario/add-static-data.png)

        U zou normaal lezen en schrijven van gegevens uit een externe bron, maar u kunt de Excel-gegevens wilt toevoegen als statische gegevens omdat dit een voorbeeld is.

    1. Navigeer naar het Excel-bestand dat u hebt opgeslagen, selecteert u de **Turbines** tabel en klik op **Connect**.

        ![Gegevensbron toevoegen](media/functions-powerapps-scenario/choose-table.png)

1. De aangepaste API toevoegen als een gegevensbron.

    1. Op de **gegevens** tabblad **gegevensbron toevoegen**.

    1. Klik op **Turbine reparatie**.

        ![Turbine reparatie connector](media/functions-powerapps-scenario/turbine-connector.png)

## <a name="add-controls-to-view-data-in-the-app"></a>Besturingselementen voor gegevens weergeven in de app toevoegen
Nu de gegevensbronnen in de app beschikbaar zijn, u toevoegen een scherm naar uw app zodat u de gegevens turbine kunt bekijken.

1. Op de **Start** tabblad **nieuwe scherm** > **scherm lijst**.

    ![Scherm lijst](media/functions-powerapps-scenario/list-screen.png)

    PowerApps voegt een scherm waarin een *galerie* om weer te geven items en andere besturingselementen waarmee zoeken, sorteren en filteren.

1. Wijzigen van de titelbalk `Turbine Repair`, en het formaat van de galerie, zodat er ruimte is voor meer besturingselementen eronder.

    ![Titel wijzigen en het formaat van galerie](media/functions-powerapps-scenario/gallery-title.png)

1. In de galerie geselecteerd in het rechterdeelvenster op de **gegevens** en wijzig de gegevensbron uit **CustomGallerySample** naar **Turbines**.

    ![Gegevensbron configuratiewijziging](media/functions-powerapps-scenario/change-data-source.png)

    De gegevensset bevat een afbeelding, dus vervolgens wijzigt u de indeling aanpassen aan de gegevens. 

1. Wijzig nog steeds in het rechterdeelvenster **lay-out** naar **titel, subtitel en hoofdtekst**.

    ![Indeling van de galerie wijzigen](media/functions-powerapps-scenario/change-layout.png)

1. Als de laatste stap in het rechterdeelvenster de velden die worden weergegeven in de galerie te wijzigen.

    ![Galerie velden wijzigen](media/functions-powerapps-scenario/change-fields.png)
    
    + **Body1** LastServiceDate =
    + **Subtitle2** ServiceRequired =
    + **Title2** = Title 

1. Instellen in de galerie is geselecteerd, de **TemplateFill** eigenschap in op de volgende formule: `If(ThisItem.IsSelected, Orange, White)`.

    ![Sjabloon opvulling formule](media/functions-powerapps-scenario/formula-fill.png)

    Nu is het gemakkelijker om te zien welke galerij-item is geselecteerd.

    ![Geselecteerde item](media/functions-powerapps-scenario/selected-item.png)

1. U hoeft niet de oorspronkelijke scherm in de app. In het linkerdeelvenster Beweeg de muisaanwijzer over **scherm1**, klikt u op **...** , en **verwijderen**.

    ![Scherm verwijderen](media/functions-powerapps-scenario/delete-screen.png)

Er is een groot aantal andere opmaak die u doorgaans doen in een productie-app, maar we u aan bij een belangrijk onderdeel voor dit scenario - aanroepen van de functie verplaatst.

## <a name="add-controls-to-call-the-function-and-display-data"></a>Toevoegen van besturingselementen voor de functie aanroepen en gegevens weergeven
U hebt een app die samenvattingsgegevens voor elke turbine weergegeven, controleert de nu is het tijd om toe te voegen die Roep de functie die u hebt gemaakt en weergeven van de gegevens die wordt geretourneerd. Toegang van de functie op basis van de manier waarop die u deze naam in de definitie van OpenAPI; in dit geval heeft `TurbineRepair.CalculateCosts()`.

1. In het lint op de **invoegen** tabblad **knop**. Klik op hetzelfde tabblad **Label**

    ![Knop invoegen en label](media/functions-powerapps-scenario/insert-controls.png)

1. Sleep de knop en het label onder de galerie en grootte van het label. 

1. Selecteer de knoptekst en wijzig dit in `Calculate costs`. De app moet eruitzien als in de volgende afbeelding.

    ![App voor de knop](media/functions-powerapps-scenario/move-button-label.png)

1. Selecteer de knop en voer de volgende formule voor de knop **OnSelect** eigenschap.

    ```
    If (BrowseGallery1.Selected.ServiceRequired="Yes", ClearCollect(DetermineRepair, TurbineRepair.CalculateCosts({hours: BrowseGallery1.Selected.EstimatedEffort, capacity: BrowseGallery1.Selected.MaxOutput})))
    ```
    Deze formule wordt uitgevoerd wanneer de knop wordt geklikt en de volgende worden als de geselecteerde galerie-item heeft een **ServiceRequired** waarde van `Yes`:

    + Hiermee wist u de *verzameling* `DetermineRepair` om gegevens te verwijderen van vorige aanroepen. Een verzameling is een variabele in tabelvorm.

    + Wordt toegewezen aan de verzameling van de gegevens die zijn geretourneerd door het aanroepen van de functie `TurbineRepair.CalculateCosts()`. 
    
        De waarden worden doorgegeven aan de functie afkomstig zijn van de **EstimatedEffort** en **MaxOutput** velden voor het geselecteerde item in de galerie. Deze velden worden niet weergegeven in de galerie, maar ze zijn nog steeds beschikbaar voor gebruik in formules.

1. Selecteer het label en voer de volgende formule voor het label **tekst** eigenschap.

    ```
    "Repair decision: " & First(DetermineRepair).message & " | Cost: " & First(DetermineRepair).costToFix & " | Revenue: " & First(DetermineRepair).revenueOpportunity
    ```
    Deze formule wordt de `First()` functie voor toegang tot de eerste (en alleen) de rij van de `DetermineRepair` verzameling. Vervolgens wordt de drie waarden die de functie geretourneerd: `message`, `costToFix`, en `revenueOpportunity`. Deze waarden zijn leeg voordat de app wordt uitgevoerd voor de eerste keer.

    De voltooide app moet eruitzien als in de volgende afbeelding.

    ![Voltooide app voordat uitvoeren](media/functions-powerapps-scenario/finished-app-before-run.png)


## <a name="run-the-app"></a>De app uitvoeren
Hebt u een volledige app. Nu is het tijd om het uitvoeren en de functie aanroept in actie zien.

1. Klik op de knop uitvoeren in de rechterbovenhoek van PowerApps Studio: ![Knop app uitvoeren](media/functions-powerapps-scenario/f5-arrow-sm.png).

1. Selecteer een turbine met een waarde van `Yes` voor **ServiceRequired**, klikt u vervolgens op de **kosten berekenen** knop. Hier ziet u een resultaat zoals de volgende afbeelding.

    ![Voltooide app in PowerApps](media/functions-powerapps-scenario/finished-app.png)

1. Probeer de andere turbines om te zien wat wordt geretourneerd door de functie telkens.

## <a name="next-steps"></a>Volgende stappen
In dit onderwerp, hebt u geleerd hoe:

> [!div class="checklist"]
> * Bereid de voorbeeldgegevens in Excel.
> * Exporteren van een API-definitie.
> * Een verbinding toevoegen aan de API.
> * Een app maken en toevoegen van gegevensbronnen.
> * Voeg andere besturingselementen om gegevens in de app weer te geven.
> * Toevoegen van besturingselementen voor de functie aanroepen en gegevens weergeven
> * Voer de app om te bepalen of een reparatie rendabele.

Zie voor meer informatie over PowerApps, [Inleiding tot PowerApps](https://powerapps.microsoft.com/tutorials/getting-started/).

Zie voor meer informatie over andere interessante scenario's die gebruikmaken van Azure Functions [aanroepen van een functie van Microsoft-Flow](functions-flow-scenario.md) en [maken van een functie die kan worden geïntegreerd met Azure Logic Apps](functions-twitter-email.md).