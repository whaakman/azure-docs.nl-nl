---
title: Een Azure-functie aanroepen vanuit Microsoft Flow | Microsoft Docs
description: Een aangepaste connector maken en vervolgens een functie met behulp van deze connector aanroepen.
services: functions
keywords: cloud-apps, cloud services, Microsoft Flow, bedrijfsprocessen, business-toepassing
author: ggailey777
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: glenga
ms.reviewer: sunayv
ms.custom: ''
ms.openlocfilehash: 662c78fc7074b0dafc53c393962aa4b578779095
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092255"
---
# <a name="call-a-function-from-microsoft-flow"></a>Een functie van Microsoft Flow aanroepen

[Microsoft Flow](https://flow.microsoft.com/) kunt u eenvoudig voor het automatiseren van werkstromen en bedrijfsprocessen tussen uw favoriete apps en services. Professionele ontwikkelaars kunnen Azure Functions gebruiken voor het uitbreiden van de mogelijkheden van Microsoft Flow, terwijl het afschermen van flow builders van de technische details.

Maakt u een stroom in dit onderwerp op basis van een scenario voor wind maar voor onderhoud. Dit onderwerp leest u over het aanroepen van de functie die u hebt gedefinieerd in [OpenAPI-definitie voor een functie maken](functions-openapi-definition.md). De functie bepaalt of een noodherstelproces op een windturbine rendabel is. Als dit kosteneffectief is, wordt de stroom verzendt een e-mail om aan te bevelen de herstelbewerking.

Zie voor meer informatie over het aanroepen van dezelfde functie uit PowerApps [een functie van PowerApps aanroepen](functions-powerapps-scenario.md).

In dit onderwerp leert u hoe u:

> [!div class="checklist"]
> * Maak een lijst in SharePoint.
> * Een API-definitie exporteren.
> * Een verbinding toevoegen aan de API.
> * Maak een stroom om e-mail te verzenden als een reparatie rendabel is.
> * De stroom uitvoeren.

## <a name="prerequisites"></a>Vereisten

+ Een actief [Microsoft Flow-account](https://flow.microsoft.com/documentation/sign-up-sign-in/) met hetzelfde teken in referenties als uw Azure-account. 
+ SharePoint, die u als een gegevensbron voor deze stroom gebruiken. Zich aanmelden voor [een proefversie van Office 365](https://signup.microsoft.com/Signup?OfferId=467eab54-127b-42d3-b046-3844b860bebf&dl=O365_BUSINESS_PREMIUM&ali=1) als u SharePoint nog niet hebt.
+ Voltooi de zelfstudie [OpenAPI-definitie voor een functie maken](functions-openapi-definition.md).

## <a name="create-a-sharepoint-list"></a>Een SharePoint-lijst maken
U beginnen met het maken van een lijst die u als een gegevensbron voor de stroom gebruikt. De lijst met bevat de volgende kolommen.

| Lijstkolom     | Gegevenstype           | Opmerkingen                                    |
|-----------------|---------------------|------------------------------------------|
| **Titel**           | Eén tekstregel | Naam van de turbine                      |
| **LastServiceDate** | Date                |                                          |
| **MaxOutput**       | Aantal              | Uitvoer van de turbine, in KwH            |
| **ServiceRequired** | Ja/Nee              |                                          |
| **EstimatedEffort** | Aantal              | Geschatte tijd voor het herstel in uren |

1. Klik of tik in de SharePoint-site **nieuw**, klikt u vervolgens **lijst**.

    ![Maak een nieuwe SharePoint-lijst](./media/functions-flow-scenario/new-list.png)

2. Voer de naam `Turbines`en klik of tik op **maken**.

    ![Geef een naam op voor de nieuwe lijst](./media/functions-flow-scenario/create-list.png)

    De **maar** lijst wordt gemaakt met de standaard **titel** veld.

    ![Maar lijst](./media/functions-flow-scenario/initial-list.png)

3. Klik of tik op ![pictogram Nieuw item](./media/functions-flow-scenario/icon-new.png) vervolgens **datum**.

    ![Voeg één regel tekst toe](./media/functions-flow-scenario/add-column.png)

4. Voer de naam `LastServiceDate`en klik of tik op **maken**.

    ![LastServiceDate kolom maken](./media/functions-flow-scenario/date-column.png)

5. Herhaal de laatste twee stappen voor de andere drie kolommen in de lijst:

    1. **Aantal** > "MaxOutput"

    2. **Ja/Nee** > "ServiceRequired"

    3. **Aantal** > "EstimatedEffort"

Dat is alles voorlopig: u hebt een lege lijst dat lijkt op de volgende afbeelding. U kunt gegevens aan de lijst toevoegen nadat u de stroom maken.

![Lege lijst](media/functions-flow-scenario/empty-list.png)

[!INCLUDE [Export an API definition](../../includes/functions-export-api-definition.md)]

## <a name="add-a-connection-to-the-api"></a>Een verbinding toevoegen aan de API
De aangepaste API (ook wel bekend als een aangepaste connector) is beschikbaar in Microsoft Flow, maar u moet verbinding maken met de API voordat u deze in een stroom gebruiken kunt.

1. In [flow.microsoft.com](https://flow.microsoft.com), klik op het tandwielpictogram (in de rechterbovenhoek) en klik vervolgens op **verbindingen**.

    ![Stroomverbindingen](media/functions-flow-scenario/flow-connections.png)

1. Klik op **verbinding maken**, schuif omlaag naar de **Turbine herstellen** -connector, en klik erop.

    ![Verbinding maken](media/functions-flow-scenario/create-connection.png)

1. Voer de API-sleutel en klikt u op **verbinding maken**.

    ![API-sleutel invoeren en maken](media/functions-flow-scenario/api-key.png)

> [!NOTE]
> Als u de stroom met anderen deelt, moet elke persoon die werkt op of de stroom gebruikt ook de API-sleutel verbinding maken met de API invoeren. Dit gedrag veranderen in de toekomst en we in dit onderwerp om te geven die wordt bijgewerkt.


## <a name="create-a-flow"></a>Stroom maken

U bent nu klaar om te maken van een stroom die gebruikmaakt van de aangepaste connector en de SharePoint-lijst die u hebt gemaakt.

### <a name="add-a-trigger-and-specify-a-condition"></a>Een trigger toevoegen en geef een voorwaarde

U eerst een stroom maken met een leeg (zonder een sjabloon) en voegt een *trigger* die wordt geactiveerd wanneer een item in de SharePoint-lijst wordt gemaakt. U voegt een *voorwaarde* om te bepalen wat er daarna gebeurt.

1. In [flow.microsoft.com](https://flow.microsoft.com), klikt u op **mijn stromen**, klikt u vervolgens **leeg item maken**.

    ![Leeg item maken](media/functions-flow-scenario/create-from-blank.png)

2. Klik op de SharePoint-trigger **wanneer een item wordt gemaakt**.

    ![Kies een trigger](media/functions-flow-scenario/choose-trigger.png)

    Als u niet al bent aangemeld bij SharePoint, wordt u gevraagd om dit te doen.

3. Voor **siteadres**, voer de naam van uw SharePoint-site, en voor **lijstnaam**, voert u de lijst die de turbine gegevens bevat.

    ![Kies een trigger](media/functions-flow-scenario/site-list.png)

4. Klik op **nieuwe stap**, klikt u vervolgens **een voorwaarde toevoegen**.

    ![Een voorwaarde toevoegen](media/functions-flow-scenario/add-condition.png)

    Microsoft Flow voegt twee vertakkingen toe aan de stroom: **als Ja** en **als er geen**. U toevoegen stappen aan een of beide vertakkingen na het definiëren van de voorwaarde die u wilt vergelijken.

    ![Voorwaarde vertakkingen](media/functions-flow-scenario/condition-branches.png)

5. Op de **voorwaarde** -kaart, klik op het eerste vak en selecteer vervolgens **ServiceRequired** uit de **dynamische inhoud** in het dialoogvenster.

    ![Veld voor de voorwaarde selecteren](media/functions-flow-scenario/condition1-field.png)

6. Voer een waarde van `True` voor de voorwaarde.

    ![Geef true voor voorwaarde](media/functions-flow-scenario/condition1-yes.png)

    De waarde wordt weergegeven als `Yes` of `No` in de SharePoint-lijst, maar deze wordt opgeslagen als een *Booleaanse*, een van beide `True` of `False`. 

7. Klik op **stroom maken** aan de bovenkant van de pagina. Klik op **Update Flow** periodiek.

Voor alle items in de lijst hebt gemaakt, wordt de stroom gecontroleerd of de **ServiceRequired** veld is ingesteld op `Yes`, gaat u naar de **als Ja** vertakking of het **als er geen** vertakking als van toepassing. Om tijd te besparen, in dit onderwerp dat u alleen acties voor de **als Ja** vertakking.

### <a name="add-the-custom-connector"></a>De aangepaste connector toevoegen

U nu toevoegen de aangepaste connector die de functie in Azure aanroept. U kunt de aangepaste connector toevoegen aan de stroom net als een standaard-connector. 

1. In de **als Ja** vertakking, klikt u op **een actie toevoegen**.

    ![Een actie toevoegen](media/functions-flow-scenario/condition1-yes-add-action.png)

2. In de **een actie kiezen** in het dialoogvenster, zoekt u `Turbine Repair`, selecteert u de actie **Turbine reparatie - kosten berekend**.

    ![Kies een actie](media/functions-flow-scenario/choose-turbine-repair.png)

    De volgende afbeelding ziet u de kaart die wordt toegevoegd aan de stroom. De velden en beschrijvingen zijn afkomstig van de OpenAPI-definitie voor de connector.

    ![Standaardinstellingen van de kosten worden berekend](media/functions-flow-scenario/calculates-costs-default.png)

3. Op de **kosten worden berekend** -kaart, gebruikt u de **dynamische inhoud** in het dialoogvenster voor het selecteren van invoer voor de functie. Microsoft Flow wordt numerieke velden, maar niet het datumveld, omdat de OpenAPI-definitie Hiermee wordt aangegeven dat **uur** en **capaciteit** numeriek zijn.

    Voor **uur**, selecteer **EstimatedEffort**, en voor **capaciteit**, selecteer **MaxOutput**.

    ![Kies een actie](media/functions-flow-scenario/calculates-costs-fields.png)

     Nu toevoegen u een andere voorwaarde op basis van de uitvoer van de functie.

4. Aan de onderkant van de **als Ja** vertakking, klikt u op **meer**, klikt u vervolgens **een voorwaarde toevoegen**.

    ![Een voorwaarde toevoegen](media/functions-flow-scenario/condition2-add.png)

5. Op de **voorwaarde 2** -kaart, klik op het eerste vak en selecteer vervolgens **bericht** uit de **dynamische inhoud** in het dialoogvenster.

    ![Veld voor de voorwaarde selecteren](media/functions-flow-scenario/condition2-field.png)

6. Voer een waarde van `Yes`. De stroom gaat u naar de volgende **als Ja** vertakking of **als er geen** vertakking gebaseerd op of het bericht dat wordt geretourneerd door de functie (Controleer het herstel) Ja of Nee is (niet de reparatie maken). 

    ![Typ Ja voor voorwaarde](media/functions-flow-scenario/condition2-yes.png)

De stroom moet er nu uitzien zoals in de volgende afbeelding.

![Typ Ja voor voorwaarde](media/functions-flow-scenario/flow-checkpoint1.png)

### <a name="send-email-based-on-function-results"></a>Verzenden van e-mail op basis van functie resultaten

Op dit moment de functie heeft geretourneerd in de stroom een **bericht** waarde van `Yes` of `No` van de functie, evenals andere informatie over de kosten en potentiële omzet. In de **als Ja** vertakking van de tweede voorwaarde, ontvangt u een e-mailbericht, maar u kunt een willekeurig aantal items, zoals terugschrijven naar de SharePoint-lijst of bij het starten een [goedkeuringsproces](https://flow.microsoft.com/documentation/modern-approvals/).

1. In de **als Ja** vertakking van de tweede voorwaarde, klikt u op **een actie toevoegen**.

    ![Een actie toevoegen](media/functions-flow-scenario/condition2-yes-add-action.png)

2. In de **een actie kiezen** in het dialoogvenster, zoekt u `email`, selecteer vervolgens een e-mailactie verzenden op basis van het e-mailsysteem u (in dit geval Outlook) gebruiken.

    ![Verzenden met Outlook een e-mailbericht](media/functions-flow-scenario/outlook-send-email.png)

3. Op de **een e-mailbericht verzenden** kaart, een e-mailbericht opstellen. Geef een geldige naam in uw organisatie voor de **naar** veld. In de onderstaande afbeelding ziet u de andere velden zijn een combinatie van tekst en tokens van de **dynamische inhoud** in het dialoogvenster. 

    ![E-mailvelden](media/functions-flow-scenario/email-fields.png)

    De **titel** token afkomstig is van de SharePoint-lijst en **CostToFix** en **RevenueOpportunity** zijn geretourneerd door de functie.

    De voltooide stroom ziet er als de volgende afbeelding (we links van de eerste **als er geen** vertakking om ruimte te besparen).

    ![Volledige stroom](media/functions-flow-scenario/complete-flow.png)

4. Klik op **Update Flow** aan de bovenkant van de pagina, klikt u vervolgens op **gedaan**.

## <a name="run-the-flow"></a>De stroom uitvoeren

Nu dat de stroom is voltooid, kunt u een rij toevoegt aan de SharePoint-lijst en Zie hoe de stroom reageert.

1. Ga terug naar de SharePoint-lijst en klikt u op **snel bewerken**.

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

    ![Snel bewerken gereed](media/functions-flow-scenario/quick-edit-done.png)

    Wanneer u het item toevoegt, wordt de stroom, waarmee u eens volgende geactiveerd.

4. In [flow.microsoft.com](https://flow.microsoft.com), klikt u op **mijn stromen**, klikt u vervolgens op de stroom die u hebt gemaakt.

    ![Mijn stromen](media/functions-flow-scenario/my-flows.png)

5. Onder **RUN HISTORY**, klikt u op de stroom is uitgevoerd.

    ![Uitvoer.gesch](media/functions-flow-scenario/run-history.png)

    Als de uitvoering voltooid is, kunt u de flow-bewerkingen op de volgende pagina bekijken. Als de uitvoering is mislukt voor een bepaalde reden, biedt de volgende pagina informatie over probleemoplossing.

6. Vouw de kaarten om te zien wat er is opgetreden tijdens de stroom. Vouw bijvoorbeeld de **kosten worden berekend** kaart om te zien van de invoer en uitvoer van de functie. 

    ![Berekent de kosten voor invoer en uitvoer](media/functions-flow-scenario/calculates-costs-outputs.png)

7. Controleer het e-mailaccount voor de persoon die u hebt opgegeven in de **naar** veld van de **een e-mailbericht verzenden** kaart. Het e-mailbericht verzonden vanuit de stroom moet lijken op de volgende afbeelding.

    ![E-mailadres van Flow](media/functions-flow-scenario/flow-email.png)

    U kunt zien hoe de tokens zijn vervangen door de juiste waarden uit de SharePoint-lijst en de functie.

## <a name="next-steps"></a>Volgende stappen
In dit onderwerp hebt u geleerd hoe u:

> [!div class="checklist"]
> * Maak een lijst in SharePoint.
> * Een API-definitie exporteren.
> * Een verbinding toevoegen aan de API.
> * Maak een stroom om e-mail te verzenden als een reparatie rendabel is.
> * De stroom uitvoeren.

Zie voor meer informatie over Microsoft Flow, [aan de slag met Microsoft Flow](https://flow.microsoft.com/documentation/getting-started/).

Zie voor meer informatie over andere interessante scenario's die gebruikmaken van Azure Functions, [een functie van PowerApps aanroepen](functions-powerapps-scenario.md) en [een functie maken die kan worden geïntegreerd met Azure Logic Apps](functions-twitter-email.md).
