---
title: Een functie-app in Linux maken vanuit Azure portal | Microsoft Docs
description: Leer hoe u uw eerste serverloze Azure-functie kunt maken met behulp van Azure Portal.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 02/28/2019
ms.author: glenga
ms.custom: ''
ms.openlocfilehash: cc99bc4345c388f22e72957590f3917a85e214e0
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57339805"
---
# <a name="create-a-function-app-on-linux-in-an-azure-app-service-plan"></a>Een functie-app in Linux maken in een Azure App Service-plan

Met Azure Functions kunt u uw functies voor Linux hosten in een standaardcontainer van Azure App Service. Dit artikel begeleidt u bij het gebruik van de [Azure-portal](https://portal.azure.com) te maken van een Linux-host functie-app die wordt uitgevoerd in een [App Service-plan](functions-scale.md#app-service-plan). U kunt ook [uw eigen aangepaste container](functions-create-function-linux-custom-image.md) gebruiken.

![Functie-app maken in Azure Portal](./media/create-function-app-linux-app-service-plan/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u met uw Azure-account aan bij Azure Portal op <https://portal.azure.com>.

## <a name="create-a-function-app"></a>Een functie-app maken

U moet beschikken over een functie-app om de uitvoering van uw functies in Linux te hosten. De functie-app biedt een omgeving waarin uw functiecode kan worden uitgevoerd. U kunt er functies mee groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen. In dit artikel maakt u een App Service-plan bij het maken van uw functie-app.

1. Selecteer de **een resource maken** knop gevonden in de linkerbovenhoek van Azure portal, klikt u vervolgens selecteren **Compute** > **functie-App**.

    ![Een functie-app maken in Azure Portal](./media/create-function-app-linux-app-service-plan/function-app-create-flow.png)

2. Gebruik voor de functie-app de instellingen die in de tabel onder de afbeelding zijn opgegeven.

    ![Instellingen voor nieuwe functie-apps definiëren](./media/create-function-app-linux-app-service-plan/function-app-create-flow2.png)

    | Instelling      | Voorgestelde waarde  | Description                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam van app** | Wereldwijd unieke naam | Naam waarmee uw nieuwe functie-app wordt aangeduid. Geldige tekens zijn `a-z`, `0-9` en `-`.  | 
    | **Abonnement** | Uw abonnement | Het abonnement waarmee deze nieuwe functie-app is gemaakt. | 
    | **[Resourcegroep](../azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Naam voor de nieuwe resourcegroep waarin uw functie-app moet worden gemaakt. |
    | **Besturingssysteem** | Linux | De functie-app op Linux wordt uitgevoerd. |
    | **Publiceren** | Code | De standaard Linux-container voor uw **Runtimestack** wordt gebruikt. U moet opgeven, is de code van de functie app-project. Een andere optie is voor het publiceren van een aangepaste [Docker-installatiekopie](functions-create-function-linux-custom-image.md). |
    | **[Hostingplan](functions-scale.md)** | App Service-plan | Hostingabonnement dat definieert hoe resources worden toegewezen aan uw functie-app. Wanneer u in een App Service-plan uitvoert, kunt u bepalen de [schalen van uw functie-app](functions-scale.md).  |
    | **App Service-plan /-locatie** | Plan maken | Kies **nieuw** en geef een **App Service-plan** naam. Kies een **locatie** in een [regio](https://azure.microsoft.com/regions/) in de buurt of in de buurt van andere services uw functies toegang hebben tot. Kies de gewenste  **[prijscategorie](https://azure.microsoft.com/pricing/details/app-service/linux/)**. <br/>U kunt zowel Windows als Linux-functie-apps niet uitvoeren in hetzelfde App Service-plan. |
    | **Runtimestack** | Voorkeurstaal | Kies een runtime die uw favoriete functieprogrammeertaal ondersteunt. Kies **.NET** voor C#- en F#-functies. [Ondersteuning voor Python](functions-reference-python.md) is beschikbaar als preview op dit moment. |
    | **[Opslag](../storage/common/storage-quickstart-create-account.md)** |  Wereldwijd unieke naam |  Maak een opslagaccount die wordt gebruikt door uw functie-app. Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. U kunt ook een bestaand account gebruiken dat voldoet aan de [vereisten voor een opslagaccount](functions-scale.md#storage-account-requirements). |
    | **[Application Insights](functions-monitoring.md)** | Ingeschakeld | Application Insights is standaard uitgeschakeld. U wordt aangeraden nu Application Insights-integratie inschakelen en het kiezen van een hosting-locatie in de buurt van uw App Service-plan-locatie. Als u dit later doen wilt, raadpleegt u [Monitor Azure Functions](functions-monitoring.md).  |

3. Selecteer **Maken** om de functie-app in te richten en te implementeren.

4. Selecteer het meldingspictogram in de rechterbovenhoek van de portal en zoek het bericht **Implementatie voltooid**.

    ![Instellingen voor nieuwe functie-apps definiëren](./media/create-function-app-linux-app-service-plan/function-app-create-notification.png)

5. Selecteer **Naar de resource gaan** om uw nieuwe functie-app te bekijken.

> [!TIP]
> Als u problemen ondervindt bij het vinden van de functie-app in de portal, [voegt u functie-apps toe aan uw favorieten in Azure Portal](functions-how-to-use-azure-function-app-settings.md#favorite).

Vervolgens maakt u een functie in de nieuwe functie-app. Zelfs nadat de functie-app beschikbaar is, is het duurt een paar minuten volledig is geïnitialiseerd.

## <a name="create-function"></a>Een door HTTP geactiveerde functie maken

Deze sectie leest u hoe u een functie maakt in uw nieuwe functie-app in de portal.

> [!NOTE]
> De portal ontwikkelervaring is handig voor het Azure Functions proberen. Voor de meeste scenario's, kunt u uw functies lokaal ontwikkelen en publiceren van het project naar uw functie-app met behulp van [Visual Studio Code](functions-create-first-function-vs-code.md#create-an-azure-functions-project) of de [Azure Functions Core Tools](functions-run-local.md#create-a-local-functions-project).  

1. Kies in de nieuwe functie-app, de **overzicht** tabblad, en nadat deze volledig is geladen kiest **+ nieuwe functie**.

    ![Een nieuwe functie maken vanuit het tabblad Overzicht](./media/create-function-app-linux-app-service-plan/overview-create-function.png)

1. In de **snelstartgids** tabblad **portal**, en selecteer **doorgaan**.

    ![Uw functie-ontwikkelplatform kiezen.](./media/create-function-app-linux-app-service-plan/function-app-quickstart-choose-portal.png)

1. Kies **WebHook + API** en selecteer vervolgens **Maken**.

    ![De Quick Start van Azure Functions in Azure Portal.](./media/create-function-app-linux-app-service-plan/function-app-quickstart-node-webhook.png)

Een functie wordt gemaakt met een taalspecifieke sjabloon voor een door HTTP getriggerde functie.

U kunt de nieuwe functie nu uitvoeren door een HTTP-aanvraag te verzenden.

## <a name="test-the-function"></a>De functie testen

1. Klik in de nieuwe functie rechtsboven op **</> Functie-URL ophalen**, selecteer **Standaard (functietoets)** en klik vervolgens op **Kopiëren**. 

    ![De functie-URL vanuit Azure Portal kopiëren](./media/create-function-app-linux-app-service-plan/function-app-develop-tab-testing.png)

2. Plak de URL van de functie in de adresbalk van uw browser. Voeg waarde `&name=<yourname>` voor de querytekenreeks toe aan het einde van deze URL, en druk op de toets `Enter` op het toetsenbord om de aanvraag uit te voeren. U ziet het geretourneerde antwoord van de functie nu in de browser.  

    Het volgende voorbeeld toont het antwoord in de browser:

    ![Het antwoord van de functie in de browser.](./media/create-function-app-linux-app-service-plan/function-app-browser-testing.png)

    De aanvraag-URL bevat een sleutel die standaard is vereist, en waarmee u via HTTP toegang hebt tot de functie.

3. Wanneer uw functie wordt uitgevoerd, wordt traceringsinformatie naar de logboeken geschreven. Als u de traceringsuitvoer van de vorige uitvoering wilt zien, gaat u terug naar de functie in de portal en klikt u op de pijl onder aan het scherm om de **logboeken** uit te vouwen.

   ![De viewer voor functielogboeken in Azure Portal.](./media/create-function-app-linux-app-service-plan/function-view-logs.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt een functie-app met een eenvoudige door HTTP geactiveerde functie gemaakt.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Zie [Azure Functions HTTP bindings](functions-bindings-http-webhook.md) (Azure Functions-HTTP-bindingen) voor meer informatie.
