---
title: "Maken van een functie die kan worden geïntegreerd met Azure Logic Apps | Microsoft Docs"
description: "Maak een functie die kan worden geïntegreerd met Azure Logic Apps en cognitieve Azure-Services tweet gevoel categoriseren en meldingen verzenden wanneer gevoel slecht is."
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
ms.date: 12/08/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 8137892c4360a6b55cfe48d62226c2421a791d5e
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a>Maken van een functie die kan worden geïntegreerd met Azure Logic Apps

Azure Functions integreert met Azure Logic Apps in de ontwerpfunctie voor Logic Apps. Deze integratie kunt u de verwerkingskracht van functies in integraties met andere Azure en services van derden gebruiken. 

Deze zelfstudie ziet u het gebruik van functies met Logic Apps en cognitieve Microsoft-Services op Azure gevoel uit Twitter posts analyseren. Een functie HTTP geactiveerd categoriseert tweets als groen, geel of rood op basis van de score gevoel. Een e-mailbericht wordt verzonden wanneer slechte gevoel wordt gedetecteerd. 

![afbeelding van de eerste twee stappen van de app in App-ontwerper voor logica](media/functions-twitter-email/designer1.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een API-Resource cognitieve Services.
> * Maak een functie die tweet gevoel ingedeeld.
> * Maak een logische app die is verbonden met Twitter.
> * Detectie van gevoel toevoegen aan de logische app. 
> * De logische app verbinden met de functie.
> * Stuur een e-mail op basis van het antwoord van de functie.

## <a name="prerequisites"></a>Vereisten

+ Een actieve [Twitter](https://twitter.com/) account. 
+ Een [Outlook.com](https://outlook.com/) account (voor het verzenden van meldingen).
+ Als startpunt van dit onderwerp dienen de resources die zijn gemaakt in [Create your first function from the Azure portal](functions-create-first-azure-function.md) (Uw eerste functie maken vanuit Azure Portal).  
Als u dit nog niet hebt gedaan, moet u deze stappen nu voltooien om uw functie-app te maken.

## <a name="create-a-cognitive-services-resource"></a>Maak een resource cognitieve Services

De cognitieve Services-API's zijn beschikbaar in Azure als afzonderlijke resources. De tekst Analytics-API gebruiken voor het detecteren van het gevoel over de tweets wordt bewaakt.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

3. Klik op **AI en analyse** > **Tekstanalyse API**. Vervolgens gebruikt u de instellingen die zijn opgegeven in de tabel en controleer de voorwaarden accepteren **vastmaken aan dashboard**.

    ![Pagina cognitieve resource maken](media/functions-twitter-email/cog_svcs_resource.png)

    | Instelling      |  Voorgestelde waarde   | Beschrijving                                        |
    | --- | --- | --- |
    | **Naam** | MyCognitiveServicesAccnt | Kies een unieke naam. |
    | **Locatie** | VS - west | De dichtstbijzijnde gebruiken. |
    | **Prijscategorie** | F0 | Beginnen met de laagste categorie. Als u buiten aanroepen uitvoert, kan worden uitgebreid naar een hogere laag.|
    | **Resourcegroep** | myResourceGroup | Gebruik dezelfde resourcegroep voor alle services in deze zelfstudie.|

4. Klik op **maken** om uw bron te maken. Nadat deze is gemaakt, selecteert u uw nieuwe cognitieve Services resource vastgemaakt aan het dashboard. 

5. Klik in de kolom linkernavigatievenster **sleutels**, en kopieert u de waarde van **Key 1** en op te slaan. U gebruikt deze sleutel verbinding maken met de logische app uw cognitieve Services-API. 
 
    ![Sleutels](media/functions-twitter-email/keys.png)

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-the-function-app"></a>De functie-app maken

Functies biedt een uitstekende manier om de offload van taken in een werkstroom van logic apps. Deze zelfstudie gebruikt een functie HTTP is geactiveerd voor het verwerken van tweet gevoel scores van cognitieve Services en een categoriewaarde retourneren.  

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

## <a name="create-an-http-triggered-function"></a>Maken van een functie HTTP is geactiveerd  

1. Vouw de functie-app uit en klik op de knop **+** naast **Functies**. Als dit de eerste functie in de functie-app is, selecteert u **Aangepaste functie**. U ziet nu de volledige set het functiesjablonen.

    ![De Quick Start-pagina van Functions in Azure Portal](media/functions-twitter-email/add-first-function.png)

2. Typ in het zoekveld `http` en kies vervolgens **C#** voor de HTTP-trigger-sjabloon. 

    ![Kies de HTTP-trigger](./media/functions-twitter-email/select-http-trigger-portal.png)

3. Typ een **naam** kiezen voor de functie `Function` voor  **[verificatieniveau](functions-bindings-http-webhook.md#http-auth)**, en selecteer vervolgens **maken**. 

    ![De functie HTTP geactiveerd maken](./media/functions-twitter-email/select-http-trigger-portal-2.png)

    Hiermee maakt u een C# scriptfunctie met de HTTP-Trigger-sjabloon. Uw code wordt weergegeven in een nieuw venster als `run.csx`.

4. Vervang de inhoud van de `run.csx` het bestand met de volgende code en klik vervolgens op **opslaan**:

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
    Deze functiecode retourneert een kleurcategorie op basis van de gevoel score ontvangen in de aanvraag. 

4. U kunt de functie testen, klikt u op **testen** helemaal rechts op het tabblad testresultaten uitbreiden. Typ een waarde van `0.2` voor de **aanvraagtekst**, en klik vervolgens op **uitvoeren**. Een waarde van **rood** in de hoofdtekst van het antwoord wordt geretourneerd. 

    ![De functie testen in de Azure portal](./media/functions-twitter-email/test.png)

U hebt nu een functie die gevoel scores ingedeeld. Vervolgens kunt u een logische app, die de functie kan worden geïntegreerd met uw Twitter en cognitieve Services-API maken. 

## <a name="create-a-logic-app"></a>Een logische app maken   

1. Klik in de Azure-portal op de **nieuw** knop gevonden in de linkerbovenhoek van de Azure portal.

2. Klik op **Enterprise Integration** > **logische App**. Vervolgens gebruikt u de instellingen die zijn opgegeven in de tabel, selectievakje **vastmaken aan dashboard**, en klik op **maken**.
 
4. Typ een **naam** zoals `TweetSentiment`, gebruik van de instellingen die zijn opgegeven in de tabel, accepteer de voorwaarden en Controleer **vastmaken aan dashboard**.

    ![Logische app maken in de Azure portal](./media/functions-twitter-email/new_logic_app.png)

    | Instelling      |  Voorgestelde waarde   | Beschrijving                                        |
    | ----------------- | ------------ | ------------- |
    | **Naam** | TweetSentiment | Kies een passende naam voor uw app. |
    | **Resourcegroep** | myResourceGroup | Kies dezelfde bestaande resourcegroep als voor. |
    | **Locatie** | VS - oost | Kies een locatie dicht bij u. |    

4. Kies **vastmaken aan dashboard**, en klik vervolgens op **maken** uw logische app maken. 

5. Nadat de app is gemaakt, klikt u op de nieuwe logische app vastgemaakt aan het dashboard. Klik in de ontwerpfunctie voor Logic Apps, schuif naar beneden en klikt u op de **lege logische App** sjabloon. 

    ![Lege Logic Apps-sjabloon](media/functions-twitter-email/blank.png)

U kunt nu de ontwerpfunctie van Logic Apps services en triggers toevoegt aan uw app.

## <a name="connect-to-twitter"></a>Verbinding maken met Twitter

Maak eerst een verbinding met uw Twitter-account. De logische app worden opgevraagd op tweets die worden uitgevoerd met de app te activeren.

1. Klik in de ontwerpfunctie voor de **Twitter** service en klik op de **wanneer een nieuwe tweet wordt gepost** trigger. Aanmelden bij uw Twitter-account en autoriseren van Logic Apps om uw account te gebruiken.

2. Gebruik de instellingen van de trigger Twitter zoals opgegeven in de tabel. 

    ![Instellingen voor Twitter-connector](media/functions-twitter-email/azure_tweet.png)

    | Instelling      |  Voorgestelde waarde   | Beschrijving                                        |
    | ----------------- | ------------ | ------------- |
    | **Zoektekst** | #Azure | Gebruik een hashtag die populaire voor het genereren van nieuwe tweets in het gekozen interval. Wanneer met behulp van de laag gratis en uw hashtag te populair is, kunt u snel van de quota voor transactie gebruiken in uw cognitieve Services-API. |
    | **Frequentie** | Minuut | De frequentie eenheid wordt gebruikt voor het polling-Twitter.  |
    | **Interval** | 15 | De verstreken tijd tussen Twitter-aanvragen in de frequentie eenheden. |

3.  Klik op **opslaan** verbinding maken met uw Twitter-account. 

Uw app is nu verbonden met Twitter. Vervolgens maakt verbinding u met tekstanalyse voor het detecteren van de gevoel verzamelde tweets.

## <a name="add-sentiment-detection"></a>Detectie van gevoel toevoegen

1. Klik op **nieuwe stap**, en vervolgens **een actie toevoegen**.

    ![Nieuwe stap, waarna een actie toevoegen](media/functions-twitter-email/new_step.png)

2. In **kiest u een actie**, klikt u op **Tekstanalyse**, en klik vervolgens op de **detecteren gevoel** in te grijpen.

    ![Sentiment detecteren](media/functions-twitter-email/detect_sent.png)

3. Typ de naam van een verbinding zoals `MyCognitiveServicesConnection`, plakt u de sleutel voor uw cognitieve Services-API die u opgeslagen en klik op **maken**.  

4. Klik op **tekst voor het analyseren van** > **Tweet tekst**, en klik vervolgens op **opslaan**.  

    ![Sentiment detecteren](media/functions-twitter-email/ds_tta.png)

Nu gevoel detectie is geconfigureerd, kunt u een verbinding toevoegen aan de functie die de uitvoer van de score gevoel verbruikt.

## <a name="connect-sentiment-output-to-your-function"></a>Verbinding maken met gevoel uitvoer van de functie

1. Klik in de ontwerpfunctie voor Logic Apps **nieuwe stap** > **een actie toevoegen**, en klik vervolgens op **Azure Functions**. 

2. Klik op **kiest u een Azure-functie**, selecteer de **CategorizeSentiment** functie die u eerder hebt gemaakt.  

    ![Azure vak voor de functie kiezen met een Azure-functie](media/functions-twitter-email/choose_fun.png)

3. In **aanvraagtekst**, klikt u op **Score** en vervolgens **opslaan**.

    ![Score](media/functions-twitter-email/trigger_score.png)

De functie wordt nu geactiveerd wanneer een score gevoel wordt verzonden door de logische app. Een kleurcode categorie wordt geretourneerd naar de logische app door de functie. Vervolgens voegt u een e-mailbericht dat wordt verzonden als een waarde gevoel van **rood** wordt geretourneerd vanaf de functie. 

## <a name="add-email-notifications"></a>E-mailmeldingen toevoegen

Het laatste deel van de werkstroom is voor het activeren van een e-mailbericht wanneer de gevoel wordt berekend als _rood_. In dit onderwerp gebruikt een Outlook.com-connector. U kunt vergelijkbare stappen voor het gebruik van een connector Gmail of Office 365 Outlook uitvoeren.   

1. Klik in de ontwerpfunctie voor Logic Apps **nieuwe stap** > **een voorwaarde toevoegen**. 

2. Klik op **kiest u een waarde**, klikt u vervolgens op **hoofdtekst**. Selecteer **gelijk is aan**, klikt u op **kiest u een waarde** en het type `RED`, en klik op **opslaan**. 

    ![Een voorwaarde toevoegen aan de logische app.](media/functions-twitter-email/condition.png)

3. In **als waar**, klikt u op **een actie toevoegen**, zoeken naar `outlook.com`, klikt u op **e-mailbericht verzenden**, en meld u aan bij uw account Outlook.com.
    
    ![Kies een actie voor de voorwaarde.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > Als u geen een Outlook.com-account hebt, kunt u een andere connector, zoals Gmail of Office 365 Outlook

4. In de **e-mailbericht verzenden** actie, gebruik de e-mailinstellingen als in de tabel opgegeven. 

    ![Configureer het e-mailbericht voor het verzenden van een e-actie.](media/functions-twitter-email/send_email.png)

    | Instelling      |  Voorgestelde waarde   | Beschrijving  |
    | ----------------- | ------------ | ------------- |
    | **Aan** | Typ uw e-mailadres | Het e-mailadres dat de melding ontvangt. |
    | **Onderwerp** | Negatieve tweet gevoel gedetecteerd  | De onderwerpregel van het e-mailmeldingen.  |
    | **Hoofdtekst** | Tweet tekst, locatie | Klik op de **Tweet tekst** en **locatie** parameters. |

5.  Klik op **Opslaan**.

Nu dat de werkstroom voltooid is, kunt u de logische app inschakelen en de functie op het werk ziet.

## <a name="test-the-workflow"></a>De werkstroom testen

1. Klik in de ontwerpfunctie voor Logic App **uitvoeren** om de app te starten.

2. Klik in de linkerkolom **overzicht** voor de status van de logische app. 
 
    ![Uitvoeringsstatus van Logic app](media/functions-twitter-email/over1.png)

3. (Optioneel) Klik op een van de wordt uitgevoerd om de details van de uitvoering te bekijken.

4. Ga naar de functie, bekijk de logboeken en controleert u of gevoel waarden zijn ontvangen en verwerkt.
 
    ![Logboeken van de functie weergeven](media/functions-twitter-email/sent.png)

5. Wanneer een potentieel negatieve gevoel wordt gedetecteerd, krijgt u een e-mailbericht. Als u een e-mailbericht hebt ontvangen, kunt u de functiecode om terug te keren rood telkens wanneer:

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    Nadat u hebt gecontroleerd dat e-mailmeldingen, wijzigt u terug naar de oorspronkelijke code:

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > Nadat u deze zelfstudie hebt voltooid, moet u de logische app uitschakelen. Door het uitschakelen van de app u voorkomen dat u in rekening gebracht voor uitvoeringen en met behulp van de transacties in uw cognitieve Services-API.

Nu hebt u gezien hoe eenvoudig het is het integreren van functies in een werkstroom Logic Apps.

## <a name="disable-the-logic-app"></a>De logische app uitschakelen

Als u wilt uitschakelen op de logische app, **overzicht** en klik vervolgens op **uitschakelen** aan de bovenkant van het scherm. Hierdoor wordt voorkomen dat de logische app uitgevoerd en steeds kosten zonder te verwijderen van de app. 

![De functie Logboeken](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Maak een API-Resource cognitieve Services.
> * Maak een functie die tweet gevoel ingedeeld.
> * Maak een logische app die is verbonden met Twitter.
> * Detectie van gevoel toevoegen aan de logische app. 
> * De logische app verbinden met de functie.
> * Stuur een e-mail op basis van het antwoord van de functie.

Ga naar de volgende zelfstudie voor meer informatie over het maken van een zonder Server API voor de functie.

> [!div class="nextstepaction"] 
> [Een serverloze API maken met behulp van Azure Functions](functions-create-serverless-api.md)

Zie voor meer informatie over Logic Apps, [Azure Logic Apps](../logic-apps/logic-apps-overview.md).

