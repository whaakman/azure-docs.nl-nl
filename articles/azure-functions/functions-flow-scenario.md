---
title: Een Azure-functie aanroepen vanuit Microsoft Flow | Microsoft Docs
description: Een aangepaste connector maken en vervolgens een functie met die connector aanroept.
services: functions
keywords: cloud-apps, cloud services, Microsoft Flow, bedrijfsprocessen, business-toepassing
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
ms.date: 12/14/2017
ms.author: mblythe
ms.custom: 
ms.openlocfilehash: 38d2e3f2f2aa057b50ba12138cafc512ac110f9b
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="call-a-function-from-microsoft-flow"></a>Een functie van Microsoft Flow aanroepen

[Microsoft-Flow](https://flow.microsoft.com/) kunt u gemakkelijk om werkstromen en bedrijfsprocessen tussen uw favoriete apps en services te automatiseren. Professionele ontwikkelaars kunnen Azure Functions gebruiken om de mogelijkheden van Microsoft-Flow tijdens afscherming stroom opbouwfuncties van technische informatie.

U maakt een stroom in dit onderwerp op basis van een scenario voor Windturbines voor onderhoud. Dit onderwerp leest u hoe de functie die u hebt gedefinieerd in [maken voor een functie een definitie van een OpenAPI](functions-openapi-definition.md). De functie bepaalt of een noodherstelproces op een turbine o rendabele. Als het rendabele, verzendt de stroom een e-mail aan te bevelen het herstel.

Zie voor meer informatie over het aanroepen van dezelfde functie uit PowerApps [aanroepen van een functie van PowerApps](functions-powerapps-scenario.md).

In dit onderwerp leert u hoe:

> [!div class="checklist"]
> * Een lijst maken in SharePoint.
> * Exporteren van een API-definitie.
> * Een verbinding toevoegen aan de API.
> * Maak een stroom voor het verzenden van e-mailadres als een reparatie rendabele is.
> * De stroom worden uitgevoerd.

## <a name="prerequisites"></a>Vereisten

+ Een actieve [Microsoft Flow rekening](https://flow.microsoft.com/documentation/sign-up-sign-in/) met hetzelfde teken in referenties als uw Azure-account. 
+ SharePoint u als gegevensbron voor deze stroom gebruiken. Aanmelden voor [een proefversie van Office 365](https://signup.microsoft.com/Signup?OfferId=467eab54-127b-42d3-b046-3844b860bebf&dl=O365_BUSINESS_PREMIUM&ali=1) als u SharePoint nog geen hebt.
+ Voltooi de zelfstudie [maken voor een functie een definitie van een OpenAPI](functions-openapi-definition.md).

## <a name="create-a-sharepoint-list"></a>Maken van een SharePoint-lijst
U beginnen met het maken van een lijst die u als gegevensbron voor de stroom gebruikt. De lijst bevat de volgende kolommen.

| Lijstkolom     | Gegevenstype           | Opmerkingen                                    |
|-----------------|---------------------|------------------------------------------|
| **Titel**           | Enkele regel tekst | Naam van de turbine                      |
| **LastServiceDate** | Date                |                                          |
| **MaxOutput**       | Aantal              | Uitvoer turbine in KwH            |
| **ServiceRequired** | Ja/Nee              |                                          |
| **EstimatedEffort** | Aantal              | Geschatte tijd voor het herstel in uren |

1. Tik of klik in de SharePoint-site **nieuw**, klikt u vervolgens **lijst**.

    ![Maken van nieuwe SharePoint-lijst](./media/functions-flow-scenario/new-list.png)

2. Voer de naam `Turbines`, klik of tik **maken**.

    ![Geef de naam van een nieuwe lijst](./media/functions-flow-scenario/create-list.png)

    De **Turbines** lijst wordt gemaakt met het standaard **titel** veld.

    ![Turbines lijst](./media/functions-flow-scenario/initial-list.png)

3. Klik of tik ![itempictogram nieuw](./media/functions-flow-scenario/icon-new.png) vervolgens **datum**.

    ![Voeg één regel tekstveld](./media/functions-flow-scenario/add-column.png)

4. Voer de naam `LastServiceDate`, klik of tik **maken**.

    ![De kolom LastServiceDate maken](./media/functions-flow-scenario/date-column.png)

5. Herhaal de laatste twee stappen voor de drie kolommen in de lijst:

    1. **Aantal** > 'MaxOutput'

    2. **Ja/Nee** > 'ServiceRequired'

    3. **Aantal** > 'EstimatedEffort'

Dat is alles nu - er is een lege lijst dat op de volgende afbeelding lijkt. U kunt gegevens aan de lijst toevoegen nadat u de stroom maken.

![Lege lijst](media/functions-flow-scenario/empty-list.png)

[!INCLUDE [Export an API definition](../../includes/functions-export-api-definition.md)]

## <a name="add-a-connection-to-the-api"></a>Een verbinding toevoegen aan de API
De aangepaste API (ook wel bekend als een aangepaste connector) is beschikbaar in de Microsoft-Flow, maar u moet maakt een verbinding met de API voordat u deze in een stroom gebruiken kunt.

1. In [flow.microsoft.com](https://flow.microsoft.com), klik op het pictogram tandwielpictogram (in de rechterbovenhoek) en klik vervolgens op **verbindingen**.

    ![Stroom-verbindingen](media/functions-flow-scenario/flow-connections.png)

1. Klik op **verbinding maken**, schuif omlaag naar de **Turbine reparatie** connector, en klik erop.

    ![Verbinding maken](media/functions-flow-scenario/create-connection.png)

1. Voer de API-sleutel in en klik op **verbinding maken**.

    ![Voer API-sleutel en maken](media/functions-flow-scenario/api-key.png)

> [!NOTE]
> Als u uw flow met anderen deelt, moet elke persoon die werkt op of de stroom gebruikt ook de API-sleutel verbinding maken met de API invoeren. Dit gedrag in de toekomst kan worden gewijzigd en dit onderwerp om te geven die wordt bijgewerkt.


## <a name="create-a-flow"></a>Maken van een stroom

U kunt nu gereed voor het maken van een stroom die gebruikmaakt van de aangepaste connector en de SharePoint-lijst die u hebt gemaakt.

### <a name="add-a-trigger-and-specify-a-condition"></a>Een trigger toevoegen en geef een voorwaarde

U maakt eerst een stroom van leeg (zonder een sjabloon) en voeg een *trigger* die wordt geactiveerd wanneer een item in de SharePoint-lijst wordt gemaakt. U voegt een *voorwaarde* om te bepalen wat gebeurt er nu.

1. In [flow.microsoft.com](https://flow.microsoft.com), klikt u op **mijn loopt**, klikt u vervolgens **maken van leeg**.

    ![maken van leeg](media/functions-flow-scenario/create-from-blank.png)

2. Klik op de SharePoint-trigger **wanneer een item wordt gemaakt**.

    ![Kies een trigger](media/functions-flow-scenario/choose-trigger.png)

    Als u nog niet bent aangemeld bij SharePoint, wordt u gevraagd om dit te doen.

3. Voor **siteadres**, voer de naam van uw SharePoint-site en voor **naam**, voert u de lijst die de turbine gegevens bevat.

    ![Kies een trigger](media/functions-flow-scenario/site-list.png)

4. Klik op **nieuwe stap**, klikt u vervolgens **een voorwaarde toevoegen**.

    ![Een voorwaarde toevoegen](media/functions-flow-scenario/add-condition.png)

    Microsoft-Flow voegt twee vertakkingen tot de stroom: **zo Ja** en **als er geen**. U toevoegen stappen aan een of beide vertakkingen na het definiëren van de voorwaarde die u wilt vergelijken.

    ![Voorwaarde vertakkingen](media/functions-flow-scenario/condition-branches.png)

5. Op de **voorwaarde** kaart, klikt u op het eerste vak en selecteer **ServiceRequired** van de **dynamische inhoud** in het dialoogvenster.

    ![Veld voor de voorwaarde selecteren](media/functions-flow-scenario/condition1-field.png)

6. Voer een waarde van `True` voor de voorwaarde.

    ![Voer waar voor voorwaarde](media/functions-flow-scenario/condition1-yes.png)

    De waarde wordt weergegeven als `Yes` of `No` in de SharePoint lijst, maar het wordt opgeslagen als een *Booleaanse*komt ofwel `True` of `False`. 

7. Klik op **stroom maken** boven aan de pagina. Klik op **Update stromen** regelmatig.

Voor alle items in de lijst hebt gemaakt, controleert de stroom of de **ServiceRequired** veld is ingesteld op `Yes`, gaat u naar de **zo Ja** vertakking of de **als er geen** vertakking als van toepassing. Tijd te besparen, in dit onderwerp dat u alleen acties opgeven voor de **zo Ja** vertakking.

### <a name="add-the-custom-connector"></a>De aangepaste connector toevoegen

U toevoegen de aangepaste connector die de functie in Azure aanroept. U toevoegen de aangepaste connector aan de stroom net als een standaard-connector. 

1. In de **zo Ja** vertakking, klikt u op **een actie toevoegen**.

    ![Een actie toevoegen](media/functions-flow-scenario/condition1-yes-add-action.png)

2. In de **kiest u een actie** in het dialoogvenster, zoekt u `Turbine Repair`, selecteert u de actie **Turbine reparatie - kosten berekend**.

    ![Kies een actie](media/functions-flow-scenario/choose-turbine-repair.png)

    De volgende afbeelding toont de kaart die wordt toegevoegd aan de stroom. De velden en beschrijvingen zijn afkomstig van de OpenAPI definitie voor de connector.

    ![Berekent de standaardwaarden van de kosten](media/functions-flow-scenario/calculates-costs-default.png)

3. Op de **kosten berekend** kaart, gebruikt u de **dynamische inhoud** in het dialoogvenster om te selecteren van de invoer voor de functie. Microsoft-Flow toont numerieke velden, maar niet het datumveld, omdat de definitie van de OpenAPI geeft aan dat **uren** en **capaciteit** numeriek zijn.

    Voor **uren**, selecteer **EstimatedEffort**, en voor **capaciteit**, selecteer **MaxOutput**.

    ![Kies een actie](media/functions-flow-scenario/calculates-costs-fields.png)

     Nu toevoegen u een ander probleem op basis van de uitvoer van de functie.

4. Aan de onderkant van de **zo Ja** vertakking, klikt u op **meer**, klikt u vervolgens **een voorwaarde toevoegen**.

    ![Een voorwaarde toevoegen](media/functions-flow-scenario/condition2-add.png)

5. Op de **voorwaarde 2** kaart, klikt u op het eerste vak en selecteer **bericht** van de **dynamische inhoud** in het dialoogvenster.

    ![Veld voor de voorwaarde selecteren](media/functions-flow-scenario/condition2-field.png)

6. Voer een waarde van `Yes`. De stroom gaat naar de volgende **zo Ja** vertakking of **als er geen** vertakking op basis van het of het bericht dat wordt geretourneerd door de functie (Zorg de reparatie) Ja of Nee is (het herstel niet maken). 

    ![Voer Ja voor voorwaarde](media/functions-flow-scenario/condition2-yes.png)

De stroom moet nu eruitzien als in de volgende afbeelding.

![Voer Ja voor voorwaarde](media/functions-flow-scenario/flow-checkpoint1.png)

### <a name="send-email-based-on-function-results"></a>Op basis van functie resultaten e-mail verzenden

Op dit moment de functie heeft geretourneerd in de stroom een **bericht** waarde van `Yes` of `No` van de functie, evenals andere gegevens over de kosten en potentiële ontvangsten. In de **zo Ja** vertakking van de tweede voorwaarde wordt een e-mail te sturen, maar u kunt een willekeurig aantal items, zoals terugschrijven naar de SharePoint-lijst of start kan doen een [goedkeuringsproces](https://flow.microsoft.com/documentation/modern-approvals/).

1. In de **zo Ja** vertakking van de tweede voorwaarde, klikt u op **een actie toevoegen**.

    ![Een actie toevoegen](media/functions-flow-scenario/condition2-yes-add-action.png)

2. In de **kiest u een actie** in het dialoogvenster, zoekt u `email`, selecteert u vervolgens een verzonden e-actie op basis van het e-mailsysteem u (in dit geval Outlook) gebruiken.

    ![Outlook verzenden een e-mailbericht](media/functions-flow-scenario/outlook-send-email.png)

3. Op de **e-mailbericht verzenden** kaart, het opstellen van een e-mailbericht. Geef een geldige naam in uw organisatie voor de **naar** veld. In de onderstaande afbeelding ziet u de andere velden zijn een combinatie van tekst en tokens van de **dynamische inhoud** in het dialoogvenster. 

    ![E-velden](media/functions-flow-scenario/email-fields.png)

    De **titel** token afkomstig is van de SharePoint-lijst, en **CostToFix** en **RevenueOpportunity** zijn geretourneerd door de functie.

    De voltooide stroom moet eruitzien als in de volgende afbeelding (we weggelaten de eerste **als er geen** vertakking om ruimte te besparen).

    ![Volledige stroom](media/functions-flow-scenario/complete-flow.png)

4. Klik op **Update stromen** boven aan de pagina, klikt u vervolgens op **gedaan**.

## <a name="run-the-flow"></a>De stroom uitvoeren

Nu dat de stroom is voltooid, kunt u een rij toevoegen aan de SharePoint-lijst en Zie hoe de stroom reageert.

1. Ga terug naar de SharePoint-lijst en klikt u op **snelle bewerken**.

    ![Snel bewerken](media/functions-flow-scenario/quick-edit.png)

2. Voer de volgende waarden in het raster bewerken.

    | Lijstkolom     | Waarde           |
    |-----------------|---------------------|
    | **Titel**           | Turbine 60 |
    | **LastServiceDate** | 08/04/2017 |
    | **MaxOutput**       | 2500 |
    | **ServiceRequired** | Ja |
    | **EstimatedEffort** | 10 |

3. Klik op **Gereed**.

    ![Snelle bewerken voltooid](media/functions-flow-scenario/quick-edit-done.png)

    Wanneer u het item toevoegt, wordt de stroom, waarmee u rekening houden met een overzicht van de volgende geactiveerd.

4. In [flow.microsoft.com](https://flow.microsoft.com), klikt u op **mijn loopt**, klikt u vervolgens op de stroom die u hebt gemaakt.

    ![Mijn stromen](media/functions-flow-scenario/my-flows.png)

5. Onder **geschiedenis uitvoeren**, klikt u op de stroom die wordt uitgevoerd.

    ![geschiedenis uitvoeren](media/functions-flow-scenario/run-history.png)

    Als de uitvoering voltooid is, kunt u de stroom-bewerkingen op de volgende pagina te bekijken. Als de uitvoering is mislukt voor een bepaalde reden, biedt de volgende pagina informatie over probleemoplossing.

6. Vouw de kaarten om te zien wat er is opgetreden tijdens de stroom. Vouw bijvoorbeeld de **kosten berekend** kaart om te zien van de invoer voor- en uitvoer van de functie. 

    ![Berekent de kosten en uitgangen](media/functions-flow-scenario/calculates-costs-outputs.png)

7. Controleer het e-mailaccount voor de persoon die u hebt opgegeven in de **naar** veld van de **e-mailbericht verzenden** kaart. Het e-mailbericht verzonden vanuit de stroom moet eruitzien als in de volgende afbeelding.

    ![Stroom e](media/functions-flow-scenario/flow-email.png)

    U kunt zien hoe de tokens zijn vervangen door de juiste waarden uit de SharePoint-lijst en de functie.

## <a name="next-steps"></a>Volgende stappen
In dit onderwerp, hebt u geleerd hoe:

> [!div class="checklist"]
> * Een lijst maken in SharePoint.
> * Exporteren van een API-definitie.
> * Een verbinding toevoegen aan de API.
> * Maak een stroom voor het verzenden van e-mailadres als een reparatie rendabele is.
> * De stroom worden uitgevoerd.

Zie voor meer informatie over Microsoft-Flow, [aan de slag met Microsoft-Flow](https://flow.microsoft.com/documentation/getting-started/).

Zie voor meer informatie over andere interessante scenario's die gebruikmaken van Azure Functions [aanroepen van een functie van PowerApps](functions-powerapps-scenario.md) en [maken van een functie die kan worden geïntegreerd met Azure Logic Apps](functions-twitter-email.md).