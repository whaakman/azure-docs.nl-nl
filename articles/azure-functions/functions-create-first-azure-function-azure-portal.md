<properties
   pageTitle="Een functie maken vanuit de Azure-portal | Microsoft Azure"
   description="Bouw uw eerste Azure-functie, een toepassing zonder server, in minder dan twee minuten."
   services="functions"
   documentationCenter="na"
   authors="ggailey777"
   manager="erikre"
   editor=""
   tags=""
/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="glenga"/>

#Een functie maken vanuit de Azure-portal

##Overzicht
Azure Functions is een gebeurtenisafhankelijke, compute-on-demand-ervaring die het bestaande Azure-toepassingsplatform uitbreidt met mogelijkheden voor het implementeren van code die wordt geactiveerd door gebeurtenissen in andere Azure-services, SaaS-producten en on-premises systemen. Met Azure Functions schalen uw toepassingen mee met uw behoeften en betaalt u alleen voor de resources die u gebruikt. Met Azure Functions kunt u geplande of geactiveerde code-eenheden implementeren in diverse programmeertalen. Zie [Overzicht van Azure Functions](functions-overview.md) voor meer informatie.

In dit onderwerp wordt beschreven hoe u de Azure-portal kunt gebruiken voor het maken van een eenvoudige ‘hallo wereld’ Node.js-Azure Function die wordt geactiveerd door een HTTP-trigger. Voordat u een functie in de Azure-portal kunt maken, moet u expliciet een functie-app maken in Azure App Service. Als u wilt dat de functie-app automatisch voor u wordt gemaakt, raadpleeg dan [de andere snelstartzelfstudie van Azure Functions](functions-create-first-azure-function.md). Dit is een eenvoudigere snelstartgids met een video.

##Een functie-app maken

Een functie-app fungeert als host voor de uitvoering van uw functies in Azure. Volg deze stappen om een functie-app te maken in de Azure-portal.

Voordat u uw eerste functie kunt maken, moet u een actief Azure-account hebben. Als u nog geen Azure-account hebt, zijn er [gratis accounts beschikbaar](https://azure.microsoft.com/free/).

1. Ga naar de [Azure-portal](https://portal.azure.com) en meld u aan met uw Azure-account.

2. Klik op **+ Nieuw** > **Web en mobiel** > **Functie-app**, selecteer uw **Abonnement**, typ een unieke **app-naam** die uw functie-app identificeert en geef de volgende instellingen op:

    + **[Resourcegroep](../azure-portal/resource-group-portal.md/)**: selecteer **Nieuwe maken** en voer een naam in voor de nieuwe resourcegroep. U kunt ook een bestaande resourcegroep kiezen, maar dan kunt u mogelijk geen dynamisch App Service-plan voor de functie-app maken.
    + **[App Service-plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)**: kies *Dynamisch* of *Klassiek*. 
        + **Dynamisch**: het standaardplantype voor Azure Functions. Wanneer u een dynamisch plan kiest, moet u ook de **Locatie** kiezen en de **Geheugentoewijzing** (in MB) instellen. Zie [Prijzen van Azure Functions](https://azure.microsoft.com/pricing/details/functions/) voor meer informatie over de invloed die geheugentoewijzing heeft op de kosten. 
        + **Klassiek**: voor een klassiek App Service-plan moet u een **App Service-plan/-locatie** maken of een bestaand plan of een bestaande locatie selecteren. De instellingen bepalen de [locatie, functies, kosten en rekenresources](https://azure.microsoft.com/pricing/details/app-service/) die aan uw app zijn gekoppeld.  
    + **Opslagaccount**: elke functie-app heeft een opslagaccount nodig. U kunt een bestaand opslagaccount kiezen of er een maken. 

    ![Nieuwe functie-app maken in de Azure-portal](./media/functions-create-first-azure-function-azure-portal/function-app-create-flow.png)

3. Klik op **Maken** om de nieuwe functie-app in te richten en te implementeren.  

Als de functie-app is ingericht, kunt u uw eerste functie maken.

## Een functie maken

Met deze stappen maakt u een functie vanuit de snelstartgids van Azure Functions.

1. Klik in het tabblad **Snelstartgids** op **WebHook en API** en op **JavaScript** en klik vervolgens op **Een functie maken**. Er wordt een nieuwe, vooraf gedefinieerde Node.js-functie gemaakt. 

    ![](./media/functions-create-first-azure-function-azure-portal/function-app-quickstart-node-webhook.png)

2. (Optioneel) Op dit punt in de snelstartgids kunt u ervoor kiezen om een rondleiding te volgen over de Azure Functions-functionaliteit in de portal.   Wanneer u de rondleiding hebt voltooid of overgeslagen, kunt u de nieuwe functie testen met de HTTP-trigger.

##De functie testen

Omdat de Azure Functions-snelstartgidsen functionele code bevatten, kunt u uw nieuwe functie meteen testen.

1. Ga naar het tabblad **Develop** en bekijk het venster **Code**. U zult zien dat deze Node.js-code een HTTP-aanvraag verwacht met een waarde *name* die wordt doorgegeven in de berichttekst of in een queryreeks. Wanneer de functie wordt uitgevoerd, wordt deze waarde in het antwoordbericht geretourneerd.

    ![](./media/functions-create-first-azure-function-azure-portal/function-app-develop-tab-testing.png)

2. Blader omlaag naar het tekstvak **Request body**, wijzig de waarde van de eigenschap *name* in uw naam en klik op **Run**. U ziet dat de uitvoering wordt geactiveerd door een HTTP-testaanvraag, dat informatie naar de streaminglogboeken wordt geschreven en dat het antwoord ‘hallo’ wordt weergegeven in de **Uitvoer**. 

3. Als u de uitvoering van dezelfde functie wilt activeren vanuit een ander browservenster of tabblad, kopieert u de waarde **Function URL** in het tabblad **Develop** en plakt u die in de adresbalk van een browser. Voeg de queryreekswaarde `&name=yourname` toe en druk op Enter. Dezelfde informatie wordt naar de logboeken geschreven en hetzelfde antwoord ‘hallo’ wordt in de browser weergegeven.

##Volgende stappen

Deze snelstartgids demonstreert een zeer eenvoudige uitvoering van een basisfunctie die door HTTP wordt geactiveerd. Zie de volgende onderwerpen voor meer informatie over het gebruik van de mogelijkheden van Azure Functions in uw apps.

+ [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)  
Naslaginformatie voor programmeurs over het coderen van functies en het definiëren van triggers en bindingen.
+ [Azure Functions testen](functions-test-a-function.md)  
Beschrijft de verschillende hulpprogramma’s en technieken voor het testen van uw functies.
+ [Azure Functions schalen](functions-scale.md)  
Beschrijft de serviceabonnementen die beschikbaar zijn voor Azure Functions, zoals het serviceabonnement Dynamic, en helpt u bij het kiezen van het juiste abonnement. 
+ [Wat is Azure App Service?](../app-service/app-service-value-prop-what-is.md)  
Azure Functions maakt gebruik van het Azure App Service-platform voor kernfunctionaliteit zoals implementaties, omgevingsvariabelen en diagnostische procedures. 

[AZURE.INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]



<!--HONumber=sep12_HO2-->


