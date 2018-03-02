---
title: "Een functie maken die kan worden geïntegreerd met Azure Logic Apps | Microsoft Docs"
description: Maak een functie die met Azure Logic Apps en Azure Cognitive Services integreert om het gevoel van een tweet te categoriseren en meldingen te verzenden wanneer het gevoel slecht is.
services: functions, logic-apps, cognitive-services
keywords: werkstroom, cloud-apps, cloudservices, bedrijfsprocessen, systeemintegratie, enterprise application integration, EAI
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 9e9369d9dc9f7298b93927b49685f4e24de8a7fd
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a>Een functie maken die kan worden geïntegreerd met Azure Logic Apps

Azure Functions integreert met Azure Logic Apps in de Logic Apps Designer. Met deze integratie kunt u de rekenkracht van Functions gebruiken in indelingen met andere Azure-services en services van derden. 

In deze zelfstudie wordt getoond hoe u in Azure Functions gebruikt met Logic Apps en Microsoft Cognitive Services om het gevoel van Twitter-posts te analyseren. Een door HTTP geactiveerde functie categoriseert tweets als groen, geel of rood, op basis van de gevoelsscore. Er wordt een e-mail verzonden als er een slecht gevoel wordt gedetecteerd. 

![afbeelding van de eerste twee stappen van de app in Logic App Designer](media/functions-twitter-email/designer1.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een API-resource voor Cognitive Services maken.
> * Een functie maken die tweetgevoelens categoriseert.
> * Een logische app maken die verbinding maakt met Twitter.
> * Detectie van gevoelens toevoegen aan de logische app. 
> * De logische app verbinden met de functie.
> * Een e-mail versturen op basis van de reactie van de functie.

## <a name="prerequisites"></a>Vereisten

+ Een actief [Twitter](https://twitter.com/)-account. 
+ Een [Outlook.com](https://outlook.com/)-account (om meldingen te verzenden).
+ Als startpunt van dit onderwerp dienen de resources die zijn gemaakt in [Create your first function from the Azure portal](functions-create-first-azure-function.md) (Uw eerste functie maken vanuit Azure Portal).  
Doorloop nu deze stappen om de functie-app te maken, als u dit nog niet hebt gedaan.

## <a name="create-a-cognitive-services-resource"></a>Een Cognitive Services-resource maken

De Cognitive Services-API's zijn als afzonderlijke resources beschikbaar in Azure. Gebruik de Text Analytics API om het gevoel van de tweets te detecteren die worden bijgehouden.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.

3. Klik op **AI en analyse** > **Tekstanalyse-API**. Gebruik vervolgens de instellingen zoals die zijn opgegeven in de tabel, accepteer de voorwaarden en schakel **Aan dashboard vastmaken** in.

    ![Cognitieve resourcepagina maken](media/functions-twitter-email/cog_svcs_resource.png)

    | Instelling      |  Voorgestelde waarde   | Beschrijving                                        |
    | --- | --- | --- |
    | **Naam** | MyCognitiveServicesAccnt | Kies een unieke naam voor het account. |
    | **Locatie** | VS - west | Gebruik de dichtstbijzijnde locatie. |
    | **Prijscategorie** | F0 | Begin met de laagste categorie. Als u geen aanroepen meer hebt, schaalt u naar een hogere categorie.|
    | **Resourcegroep** | myResourceGroup | Gebruik dezelfde resourcegroep voor alle services in deze tutorial.|

4. Klik op **Maken** om de resource te maken. Nadat deze is gemaakt, selecteert u uw nieuwe Cognitive Services-resource die is vastgemaakt aan het dashboard. 

5. Klik in de linkernavigatiekolom op **Sleutels**, kopieer de waarde van **Sleutel 1** en sla deze op. U gebruikt deze sleutel om uw logische app te verbinden aan uw Cognitive Services-API. 
 
    ![Sleutels](media/functions-twitter-email/keys.png)

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-the-function-app"></a>De functie-app maken

Functies bieden een handige manier voor de offload van verwerkingstaken in een werkstroom van een logische app. In deze zelfstudie wordt een door HTTP geactiveerde functie gebruikt om de gevoelsscore van Cognitive Services voor tweets te categoriseren en een categoriewaarde te retourneren.  

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

## <a name="create-an-http-triggered-function"></a>Een door HTTP geactiveerde functie maken  

1. Vouw de functie-app uit en klik op de knop **+** naast **Functies**. Als dit de eerste functie in de functie-app is, selecteert u **Aangepaste functie**. U ziet nu de volledige set het functiesjablonen.

    ![De Quick Start-pagina van Functions in Azure Portal](media/functions-twitter-email/add-first-function.png)

2. Typ `http` in het zoekveld en kies vervolgens**C#** voor het HTTP-activatiesjabloon. 

    ![Kies de HTTP-trigger](./media/functions-twitter-email/select-http-trigger-portal.png)

3. Voer een **Naam** in voor uw functie, kies `Function` voor **[Verificatieniveau](functions-bindings-http-webhook.md#http-auth)** en selecteer vervolgens **Maken**. 

    ![De door HTTP geactiveerde functie maken](./media/functions-twitter-email/select-http-trigger-portal-2.png)

    Hiermee wordt een C#-scriptfunctie gemaakt die gebruikmaakt van de HTTP-activeringssjabloon. Uw code wordt in een nieuw venster weergegeven als `run.csx`.

4. Vervang de inhoud van het bestand `run.csx` door de volgende code en klik vervolgens op **Opslaan**:

    ```csharp
    using System.Net;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // The sentiment category defaults to 'GREEN'. 
        string category = "GREEN";
    
        // Get the sentiment score from the request body.
        double score = await req.Content.ReadAsAsync<double>();
        log.Info(string.Format("The sentiment score received is '{0}'.",
                    score.ToString()));
    
        // Set the category based on the sentiment score.
        if (score < .3)
        {
            category = "RED";
        }
        else if (score < .6)
        {
            category = "YELLOW";
        }
        return req.CreateResponse(HttpStatusCode.OK, category);
    }
    ```
    Deze functiecode retourneert een kleurcategorie op basis van de gevoelsscore die in de aanvraag is ontvangen. 

4. Klik op **Testen** aan de rechterkant om het tabblad Testen uit te breiden en de functie te testen. Voer een waarde in van `0.2` voor de **Aanvraagtekst** en klik vervolgens op **Uitvoeren**. Er wordt een **RODE** waarde geretourneerd in de hoofdtekst van het antwoord. 

    ![Test de functie in de Azure Portal](./media/functions-twitter-email/test.png)

U hebt nu een functie die gevoelsscores categoriseert. Maak vervolgens een logische app die uw functie integreert met uw Twitter-account en Cognitive Services-API. 

## <a name="create-a-logic-app"></a>Een logische app maken   

1. Klik in Azure Portal op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

2. Klik op **Bedrijfsintegratie** > **Logische app**. Gebruik vervolgens de instellingen die zijn opgegeven in de tabel, schakel **Aan dashboard vastmaken** in en klik op **Maken**.
 
4. Voer vervolgens een **Naam** in als `TweetSentiment`, gebruik de instellingen die zijn opgegeven in de tabel, accepteer de voorwaarden en schakel **Aan dashboard vastmaken** in.

    ![Logische app maken in Azure Portal](./media/functions-twitter-email/new_logic_app.png)

    | Instelling      |  Voorgestelde waarde   | Beschrijving                                        |
    | ----------------- | ------------ | ------------- |
    | **Naam** | TweetSentiment | Kies een passende naam voor uw app. |
    | **Resourcegroep** | myResourceGroup | Kies dezelfde bestaande resourcegroep als eerder. |
    | **Locatie** | VS - oost | Kies een locatie dicht bij u in de buurt. |    

4. Schakel **Aan dashboard vastmaken** in en klik op **Maken** om uw logische app te maken. 

5. Nadat de app is gemaakt, klikt u op de nieuwe logische app die is vastgemaakt aan het dashboard. Scrol vervolgens in de Logic Apps Designer naar beneden en klik op de sjabloon **Lege logische app**. 

    ![Sjabloon Lege logische app](media/functions-twitter-email/blank.png)

U kunt nu de Logic Apps Designer gebruiken om services en triggers toe te voegen aan uw app.

## <a name="connect-to-twitter"></a>Verbinding maken met Twitter

Maak eerst verbinding met uw Twitter-account. De logische app peilt tweets, waardoor de app wordt geactiveerd.

1. Klik in de Logic Apps Designer op de service **Twitter** en klik vervolgens op de trigger **Wanneer er een nieuwe tweet wordt gepost**. Meld u aan bij uw Twitter-account en sta Logic Apps toe om uw account te gebruiken.

2. Gebruik de activeringsinstellingen van Twitter zoals die zijn opgegeven in de tabel. 

    ![Connectorinstellingen voor Twitter](media/functions-twitter-email/azure_tweet.png)

    | Instelling      |  Voorgestelde waarde   | Beschrijving                                        |
    | ----------------- | ------------ | ------------- |
    | **Zoektekst** | #Azure | Gebruik een hashtag die populair genoeg is om in het gekozen interval nieuwe tweets te genereren. Wanneer u gebruikmaakt van de gratis categorie en uw hashtag is te populair, wordt het transactiequotum in uw Cognitive Services-API mogelijk snel verbruikt. |
    | **Frequentie** | Minuut | De frequentie-eenheid die wordt gebruikt om Twitter te peilen.  |
    | **Interval** | 15 | De tijd tussen Twitter-aanvragen, in frequentie-eenheden. |

3.  Klik op **Opslaan** om verbinding te maken met uw Twitter-account. 

Uw app is nu verbonden met Twitter. Vervolgens maakt u verbinding met Text Analytics om het gevoel van de verzamelde tweets te detecteren.

## <a name="add-sentiment-detection"></a>Gevoelsdetectie toevoegen

1. Klik op **Nieuwe stap** en vervolgens op **Een actie toevoegen**.

    ![Nieuwe stap en vervolgens Een actie toevoegen](media/functions-twitter-email/new_step.png)

2. Klik in **Een actie kiezen** op **Tekstanalyse** en klik vervolgens op de actie **Gevoel detecteren**.

    ![Gevoel detecteren](media/functions-twitter-email/detect_sent.png)

3. Voer een verbindingsnaam in, bijvoorbeeld `MyCognitiveServicesConnection`, plak de sleutels van de Cognitive Services-API die u hebt bewaard en klik op **Maken**.  

4. Klik op **Te analyseren tekst** > **Tweettekst** en vervolgens op **Opslaan**.  

    ![Gevoel detecteren](media/functions-twitter-email/ds_tta.png)

Nu de gevoelsdetectie is geconfigureerd, kunt u een verbinding met uw functie toevoegen die de uitvoer van de gevoelsscore opneemt.

## <a name="connect-sentiment-output-to-your-function"></a>Gevoelsuitvoer verbinden met functie

1. Klik in de Logic Apps Designer op **Nieuwe stap** > **Een actie toevoegen** en klik vervolgens op **Azure Functions**. 

2. Klik op **Een Azure-functie kiezen** en selecteer de functie **CategorizeSentiment** die u eerder hebt gemaakt.  

    ![Azure Function-venster toont Een Azure-functie kiezen](media/functions-twitter-email/choose_fun.png)

3. Klik in **Aanvraagtekst** op **Score** en vervolgens op **Opslaan**.

    ![Score](media/functions-twitter-email/trigger_score.png)

Uw functie wordt nu geactiveerd wanneer er een gevoelsscore wordt verzonden vanuit de logische app. De functie retourneert een kleurgecodeerde categorie aan de logische app. Vervolgens voegt u een e-mailmelding toe die wordt verzonden wanneer vanuit de functie de gevoelswaarde **ROOD** wordt geretourneerd. 

## <a name="add-email-notifications"></a>E-mailmeldingen toevoegen

Het laatste deel van de werkstroom bestaat uit het activeren van een e-mail wanneer de gevoelsscore _ROOD_ is. In dit onderwerp wordt een Outlook.com-connector gebruikt. U kunt dezelfde stappen uitvoeren als u een Gmail- of Office 365 Outlook-connector wilt gebruiken.   

1. Klik in de Logic Apps Designer op **Nieuwe stap** > **Een voorwaarde toevoegen**. 

2. Klik op **Een waarde kiezen** en vervolgens op **Hoofdtekst**. Selecteer **Is gelijk aan**, klik op **Een waarde kiezen**, voer `RED` in en klik op **Opslaan**. 

    ![Voeg een voorwaarde aan de logische app toe.](media/functions-twitter-email/condition.png)

3. Klik in **INDIEN WAAR** op **Een actie toevoegen**, zoek naar `outlook.com`, klik op **Een e-mail verzenden** en meld u aan bij uw Outlook.com-account.
    
    ![Kies een actie voor de voorwaarde.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > Als u geen Outlook.com-account hebt, kunt u een andere connector kiezen, bijvoorbeeld voor Gmail of Office 365 Outlook

4. Gebruik in de actie **Een e-mail verzenden** de e-mailinstellingen zoals die zijn opgegeven in de tabel. 

    ![De e-mail configureren voor de actie Een e-mail verzenden.](media/functions-twitter-email/send_email.png)

    | Instelling      |  Voorgestelde waarde   | Beschrijving  |
    | ----------------- | ------------ | ------------- |
    | **Aan** | Voer uw e-mailadres in | Het e-mailadres dat de melding ontvangt. |
    | **Onderwerp** | Negatief gevoel in tweet gedetecteerd  | De onderwerpregel van de e-mailmelding.  |
    | **Hoofdtekst** | Tweettekst, locatie | Klik op de parameters **Tweettekst** en **Locatie**. |

5.  Klik op **Opslaan**.

Nu uw werkstroom is voltooid, kunt u de logische app inschakelen en de functie aan het werk zien.

## <a name="test-the-workflow"></a>De werkstroom testen

1. Klik in de Logic App Designer op **Uitvoeren** om de app te starten.

2. Klik in de linkerkolom op **Overzicht** om de status van de logische app te bekijken. 
 
    ![Uitvoeringsstatus van logische app](media/functions-twitter-email/over1.png)

3. (Optioneel) Klik op een uitvoering om de details ervan te bekijken.

4. Ga naar uw functie, bekijk de logboeken en controleer of de gevoelswaarden zijn ontvangen en verwerkt.
 
    ![Functielogboeken bekijken](media/functions-twitter-email/sent.png)

5. Wanneer er een mogelijk negatief gevoel wordt gedetecteerd, ontvangt u een e-mail. Als u geen e-mail hebt ontvangen, kunt u de functiecode aanpassen om elke keer ROOD te retourneren:

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    Nadat u de e-mailmeldingen hebt geverifieerd, kunt u de originele code herstellen:

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > Na voltooiing van deze zelfstudie moet u de logische app uitschakelen. Door de app uit te schakelen, voorkomt u dat u moet betalen voor uitvoeringen en dat u de transacties in uw Cognitive Services-API verbruikt.

U hebt nu gezien hoe eenvoudig het is om Functions in een Logic Apps-werkstroom te integreren.

## <a name="disable-the-logic-app"></a>De logische app uitschakelen

Klik op **Overzicht** en vervolgens op **Uitschakelen** bovenaan het scherm om de logische app uit te schakelen. De logische app wordt nu niet meer uitgevoerd. Zo voorkomt u kosten zonder dat u de app hoeft te verwijderen. 

![Functielogboeken](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een API-resource voor Cognitive Services maken.
> * Een functie maken die tweetgevoelens categoriseert.
> * Een logische app maken die verbinding maakt met Twitter.
> * Detectie van gevoelens toevoegen aan de logische app. 
> * De logische app verbinden met de functie.
> * Een e-mail versturen op basis van de reactie van de functie.

Ga door naar de volgende zelfstudie om te leren hoe u een serverloze API maakt voor uw functie.

> [!div class="nextstepaction"] 
> [Een serverloze API maken met behulp van Azure Functions](functions-create-serverless-api.md)

Raadpleeg [Azure Logic Apps](../logic-apps/logic-apps-overview.md) om meer te leren over Logic Apps.

