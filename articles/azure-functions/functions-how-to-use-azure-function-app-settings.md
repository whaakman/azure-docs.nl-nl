---
title: Azure-functie-App-instellingen configureren | Microsoft Docs
description: Informatie over het configureren van instellingen voor Azure-functie-app.
services: ''
documentationcenter: .net
author: ggailey777
manager: jeconnoc
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: azure-functions
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: glenga
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 92ca09040836dfc55a9d709b12a0ee01192d6bac
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957386"
---
# <a name="how-to-manage-a-function-app-in-the-azure-portal"></a>Over het beheren van een functie-app in Azure portal 

In Azure Functions biedt een functie-app de uitvoeringscontext voor de afzonderlijke functies. Functie-app-gedrag van toepassing op alle functies die worden gehost door een bepaalde functie-app. In dit onderwerp wordt beschreven hoe configureren en beheren van uw functie-apps in Azure portal.

Als u wilt beginnen, gaat u naar de [Azure-portal](https://portal.azure.com) en meld u aan bij uw Azure-account. Typ de naam van uw functie-app in de zoekbalk boven in de portal en selecteer deze in de lijst. Na het selecteren van uw functie-app, ziet u de volgende pagina:

![Overzicht van de functie-app in Azure portal](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

U kunt navigeren naar alles wat u nodig hebt voor het beheren van uw functie-app in het bijzonder op de overzichtspagina de **[toepassingsinstellingen](#settings)** en **[platformfuncties](#platform-features)**.

## <a name="settings"></a>Toepassingsinstellingen

De **toepassingsinstellingen** tabblad onderhoudt instellingen die worden gebruikt door uw functie-app.

![Instellingen voor functie-app in Azure portal.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

Deze instellingen, worden versleuteld opgeslagen en u moet selecteren **waarden weergeven** om de waarden in de portal te bekijken.

Selecteer om een instelling toe **nieuwe toepassingsinstelling** en de nieuwe sleutel / waarde-paar toevoegen.

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

Wanneer u een functie-app lokaal ontwikkelt, worden deze waarden worden bijgehouden in het bestand local.settings.json project.

## <a name="platform-features"></a>Platformfuncties

![Tabblad voor functie-app-platform-functies.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

Functie-apps uitvoeren in, en worden beheerd, door het Azure App Service-platform. Daarom hebben uw functie-apps toegang tot de meeste van de functies van Azure kerngeheugens voor webhosting platform. De **platformfuncties** tabblad is waar u toegang hebben tot de vele functies van de App Service-platform die u in uw functie-apps gebruiken kunt. 

> [!NOTE]
> Niet alle App Service-functies zijn beschikbaar wanneer een functie-app wordt uitgevoerd op het verbruik hostingplan.

De rest van dit onderwerp richt zich op de volgende App Service-functies in Azure portal die nuttig voor de functies zijn:

+ [App Service-editor](#editor)
+ [Console](#console)
+ [Geavanceerde hulpmiddelen (Kudu)](#kudu)
+ [Implementatie-opties](#deployment)
+ [CORS](#cors)
+ [Verificatie](#auth)
+ [API-definitie](#swagger)

Zie voor meer informatie over het werken met App Service-instellingen [Azure App Service-instellingen configureren](../app-service/configure-common.md).

### <a name="editor"></a>App Service-Editor

| | |
|-|-|
| ![Functie-app-App Service-editor.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | De App Service-editor is een geavanceerde portal-editor die u gebruiken kunt om JSON-configuratiebestanden en taxibedrijven codebestanden te wijzigen. Als u deze optie kiest, wordt een afzonderlijk browsertabblad met een basic-editor gestart. Hiermee kunt u integreren met de Git-opslagplaats, uitvoeren en fouten opsporen in code en functie-app-instellingen wijzigen. Deze editor bevat een uitgebreide ontwikkelomgeving voor uw functies in vergelijking met de standaardblade voor de functie-app.    |

![De App Service-editor](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <a name="console"></a>Console

| | |
|-|-|
| ![Functie-app-console in de Azure-portal](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | De console in de portal is een ideale developer-hulpprogramma als u liever om te communiceren met de functie-app vanaf de opdrachtregel. Veelgebruikte opdrachten zijn directory en maken van het bestand en navigatie, evenals batch-bestanden en scripts uitvoeren. |

![Functie-app-console](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <a name="kudu"></a>Geavanceerde hulpmiddelen (Kudu)

| | |
|-|-|
| ![Functie-app Kudu in Azure portal](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | De geavanceerde hulpprogramma's voor App Service (ook wel bekend als Kudu) bieden toegang tot geavanceerde beheerfuncties van uw functie-app. U beheert van kudu bovendien, systeemgegevens, app-instellingen, omgevingsvariabelen, site-uitbreidingen, HTTP-headers en servervariabelen. U kunt ook starten **Kudu** door te bladeren naar de SCM-eindpunt voor uw functie-app, zoals `https://<myfunctionapp>.scm.azurewebsites.net/` |

![Kudu configureren](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><a name="deployment">Implementatie-opties

| | |
|-|-|
| ![Opties voor de functie-app-implementatie in Azure portal](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | Functions kunt u bij ontwikkelen van uw functiecode aan te geven op uw lokale computer. Vervolgens kunt u uw lokale functie-app-project uploaden naar Azure. Naast de traditionele upload via FTP wordt Functions kunt u uw functie-app implementeren met populaire continue integratie-oplossingen, zoals GitHub, Azure DevOps, Dropbox, Bitbucket en anderen. Zie voor meer informatie, [continue implementatie voor Azure Functions](functions-continuous-deployment.md). Als u wilt uploaden handmatig met behulp van FTP- of lokale Git, moet u ook [configureren van de referenties voor implementatie](functions-continuous-deployment.md#credentials). |


### <a name="cors"></a>CORS

| | |
|-|-|
| ![Functie-app CORS in Azure portal](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | Om te voorkomen dat schadelijke code wordt uitgevoerd in uw services, blokkeert App Service aanroepen naar uw functie-apps uit externe bronnen. Functions biedt ondersteuning voor cross-origin resource sharing (CORS) zodat u bij het definiëren van een '' goedgekeurde lijst met toegestane oorsprongen waaruit uw functies externe aanvragen kunnen accepteren.  |

![Functie-App configureren CORS](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <a name="auth"></a>Verificatie

| | |
|-|-|
| ![Verificatie van de functie-app in Azure portal](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | Wanneer functies een HTTP-trigger gebruikt, kunt u vereisen dat aanroepen voor het eerst worden geverifieerd. App Service biedt ondersteuning voor Azure Active Directory-verificatie en meld u aan met providers van sociale netwerken, zoals Facebook, Microsoft en Twitter. Zie voor meer informatie over het configureren van specifieke verificatieproviders [overzicht van Azure App Service-verificatie](../app-service/overview-authentication-authorization.md). |

![Verificatie configureren voor een functie-app](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <a name="swagger"></a>API-definitie

| | |
|-|-|
| ![Functie-app API swagger-definitie in Azure portal](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | Functions biedt ondersteuning voor Swagger om toe te staan van clients op uw HTTP-geactiveerde functies eenvoudiger te gebruiken. Voor meer informatie over het maken van API-definities met Swagger, Bezoek [een RESTful API hosten met CORS in Azure App Service](../app-service/app-service-web-tutorial-rest-api.md). U kunt ook Functions-proxy's gebruiken voor het definiëren van een enkele API-gebied voor meerdere functies. Zie voor meer informatie, [werken met Azure Functions-proxy's](functions-proxies.md). |

![Functie-App-API configureren](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a>Volgende stappen

+ [Azure App Service-instellingen configureren](../app-service/configure-common.md)
+ [Doorlopende implementatie voor Azure Functions](functions-continuous-deployment.md)



