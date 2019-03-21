---
title: Een functie maken met Azure for Students Starter | Microsoft Docs
description: Informatie over het maken van een Azure-functie van een Azure for Students Starter-abonnement
Customer intent: As a student, I want to be able to create a HTTP triggered Function App within the Student Starter plan so that I can easily add APIs to any project.
services: functions
documentationcenter: na
author: alexkarcher-msft
manager: ggailey777
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 02/22/2019
ms.author: alkarche
ms.openlocfilehash: 5fd9f51185b8461269d498b254d9e9d9f4118270
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57343988"
---
# <a name="create-a-function-using-azure-for-students-starter"></a>Een functie maken met Azure for Students Starter

In deze zelfstudie maken we een Hallo wereld-HTTP-functie in een Azure for Students Starter-abonnement. Ook vindt u wat er beschikbaar zijn in Azure Functions in dit abonnementstype.

Microsoft *Azure for Students Starter* helpt u op weg met de Azure-producten u nodig hebt om te ontwikkelen in de cloud zonder kosten aan u. [Meer informatie over deze aanbieding hier.](https://azure.microsoft.com/offers/ms-azr-0144p/)

Met Azure Functions kunt u uw code in een [serverloze](https://azure.microsoft.com/solutions/serverless/) omgeving uitvoeren zonder dat u eerst een virtuele machine moet maken of een webtoepassing publiceren. [Meer informatie over functies hier.](./functions-overview.md)

## <a name="create-a-function"></a>Een functie maken

 In dit onderwerp leert u hoe u Functions gebruiken voor het maken van een HTTP-geactiveerde 'Hallo wereld'-functie in Azure portal.

![Functie-app maken in Azure Portal](./media/functions-create-student-starter/function-app-in-portal-editor.png)

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u met uw Azure-account aan bij Azure Portal op <https://portal.azure.com>.

## <a name="create-a-function-app"></a>Een functie-app maken

U moet een functie-app hebben die als host fungeert voor de uitvoering van uw functies. Met een functie-app kunt u functies groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen. 

1. Selecteer de knop **Nieuw** in de linkerbovenhoek van Azure Portal. Selecteer vervolgens **Compute** > **Functie-app**.

    ![Een functie-app maken in Azure Portal](./media/functions-create-student-starter/function-app-create-flow.png)

2. Gebruik voor de functie-app de instellingen die in de tabel onder de afbeelding zijn opgegeven.

    <img src="./media/functions-create-student-starter/Function-create-start.png" width="315">

    | Instelling      | Voorgestelde waarde  | Description                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam van app** | Wereldwijd unieke naam | Naam waarmee uw nieuwe functie-app wordt aangeduid. Geldige tekens zijn `a-z`, `0-9` en `-`.  | 
    | **Abonnement** | Uw abonnement | Het abonnement waarmee deze nieuwe functie-app is gemaakt. | 
    | **[Resourcegroep](../azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Naam voor de nieuwe resourcegroep waarin uw functie-app moet worden gemaakt. |
   | **[App Service-Plan /-locatie](./functions-scale.md)** | Nieuw | De hosting plan wordt bepaald welke regio uw functie-app wordt geïmplementeerd op en de dichtheid van uw resources. Meerdere functie-Apps geïmplementeerd in hetzelfde abonnement worden alle dezelfde één gratis instantie delen. Dit is een beperking van de Student Starter-abonnement. De volledige hostingopties zijn [hier uitgelegd.](./functions-scale.md)|
    | **Runtimestack** | Voorkeurstaal | Kies een runtime die uw favoriete functieprogrammeertaal ondersteunt. Kies **.NET** voor C#- en F#-functies. |
    |**[Application Insights](./functions-monitoring.md)**| Ingeschakeld | Application Insights wordt gebruikt voor het opslaan en analyseren van uw functie-app-Logboeken. Dit is standaard ingeschakeld als u ervoor kiest een locatie die ondersteuning biedt voor Application Insights. Application Insights kunnen worden ingeschakeld voor elke functie die door een regio in de buurt handmatig te kiezen voor het implementeren van Application Insights. Zonder Application Insights zich kunt u alleen live streaminglogboeken bekijken.

3. Selecteer **App Service-plan /-locatie** hierboven om een andere locatie kiezen

4. Selecteer **nieuw** en geef vervolgens uw abonnement een unieke naam.

5. Selecteer de locatie die het dichtst bij u. [Een volledig overzicht van Azure-regio's hier zien.](https://azure.microsoft.com/global-infrastructure/regions/) 

    <img src="./media/functions-create-student-starter/Create-ASP.png" width="800">

6. Selecteer **Maken** om de functie-app in te richten en te implementeren.

    <img src="./media/functions-create-student-starter/Function-create-end.png" width="315">

7. Selecteer het meldingspictogram in de rechterbovenhoek van de portal en zoek het bericht **Implementatie voltooid**.

    ![Instellingen voor nieuwe functie-apps definiëren](./media/functions-create-student-starter/function-app-create-notification.png)

8. Selecteer **Naar de resource gaan** om uw nieuwe functie-app te bekijken.

> [!TIP]
> Als u problemen ondervindt bij het vinden van de functie-app in de portal, [voegt u functie-apps toe aan uw favorieten in Azure Portal](./functions-how-to-use-azure-function-app-settings.md#favorite).
Vervolgens maakt u een functie in de nieuwe functie-app.

## <a name="create-function"></a>Een door HTTP geactiveerde functie maken

1. Vouw uw nieuwe functie-app uit en selecteer vervolgens de **+**-knop naast **Functies**, kies **In de portal** en selecteer **Doorgaan**.

    ![De Quick Start-pagina 'Kiezen' van het Functions-platform.](./media/functions-create-student-starter/function-app-quickstart-choose-portal.png)

1. Kies **WebHook + API** en selecteer vervolgens **Maken**.

    ![De Quick Start van Azure Functions in Azure Portal.](./media/functions-create-student-starter/function-app-quickstart-node-webhook.png)

Een functie wordt gemaakt met een taalspecifieke sjabloon voor een door HTTP getriggerde functie.

U kunt de nieuwe functie nu uitvoeren door een HTTP-aanvraag te verzenden.

## <a name="test-the-function"></a>De functie testen

1. Klik in de nieuwe functie rechtsboven op **</> Functie-URL ophalen**, selecteer **Standaard (functietoets)** en klik vervolgens op **Kopiëren**. 

    ![De functie-URL vanuit Azure Portal kopiëren](./media/functions-create-student-starter/function-app-develop-tab-testing.png)

2. Plak de URL van de functie in de adresbalk van uw browser. Voeg waarde `&name=<yourname>` voor de querytekenreeks toe aan het einde van deze URL, en druk op de toets `Enter` op het toetsenbord om de aanvraag uit te voeren. U ziet het geretourneerde antwoord van de functie nu in de browser.  

    Het volgende voorbeeld toont het antwoord in de browser:

    ![Het antwoord van de functie in de browser.](./media/functions-create-student-starter/function-app-browser-testing.png)

    De aanvraag-URL bevat een sleutel die standaard is vereist, en waarmee u via HTTP toegang hebt tot de functie.

3. Wanneer uw functie wordt uitgevoerd, wordt traceringsinformatie naar de logboeken geschreven. Als u de traceringsuitvoer van de vorige uitvoering wilt zien, gaat u terug naar de functie in de portal en klikt u op de pijl onder aan het scherm om de **logboeken** uit te vouwen.

   ![De viewer voor functielogboeken in Azure Portal.](./media/functions-create-student-starter/function-view-logs.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="supported-features-in-azure-for-students-starter"></a>Ondersteunde functies in Azure for Students Starter

U hebt toegang tot de meeste van de functies van de Azure Functions-runtime, met enkele belangrijke beperkingen die hieronder worden vermeld in Azure for Students Starter:

* De HTTP-trigger is de enige triggertype wordt ondersteund.
    * Alle invoer- en alle bindingen worden ondersteund. [Bekijk hier de volledige lijst.](functions-triggers-bindings.md)
* Talen die worden ondersteund: 
    * C# (.NET Core 2)
    * JavaScript (Node.js 8 & 10)
    * F# (.NET Core 2)
    * [Zie de talen die worden ondersteund in hier hoger plannen](supported-languages.md)
* Windows is het enige ondersteunde besturingssysteem.
* Schaal is beperkt tot [één exemplaar van de gratis laag](https://azure.microsoft.com/pricing/details/app-service/windows/) tot 60 minuten per dag uitgevoerd. U wordt serverlessly schaal van 0 aan 1 exemplaar automatisch als HTTP-verkeer is ontvangen, maar er kunnen geen nieuwe.
* Alleen [de runtime 2.x](functions-versions.md) wordt ondersteund.
* Alle hulpprogramma's voor ontwikkelaars wordt ondersteund voor het bewerken en publiceren van functies. Dit omvat VS Code, Visual Studio, de Azure CLI en Azure portal. Als u gebruiken op iets anders dan de portal wilt, moet u eerst een app maken in de portal en kies vervolgens de app als een implementatiedoel in uw favoriete hulpprogramma.

## <a name="next-steps"></a>Volgende stappen

U hebt een functie-app met een eenvoudige HTTP-geactiveerde functie gemaakt! U kunt nu verkennen lokale hulpprogramma's, meer talen, controle en integraties.

 * [Uw eerste functie maken met Visual Studio](./functions-create-your-first-function-visual-studio.md)
 * [Uw eerste functie maken met Visual Studio Code](./functions-create-first-function-vs-code.md)
 * [Handleiding voor ontwikkelaars van Azure Functions-JavaScript](./functions-reference-node.md)
 * [Azure Functions gebruiken voor het verbinding maken met een Azure SQL Database](./functions-scenario-database-table-cleanup.md)
 * [Meer informatie over Azure Functions-HTTP-bindingen](./functions-bindings-http-webhook.md).
 * [Uw Azure Functions controleren](./functions-monitoring.md)
