---
title: Configureren van Azure functie App-instellingen | Microsoft Docs
description: Informatie over het configureren van Azure functie app-instellingen.
services: 
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: 
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 04/23/2017
ms.author: glenga
ms.openlocfilehash: f174766d4ab909fea0feea517a17b9d648722c2a
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/13/2018
---
# <a name="how-to-manage-a-function-app-in-the-azure-portal"></a>Het beheren van een functie-app in de Azure portal 

In de Azure Functions biedt een functie-app de uitvoeringscontext voor uw afzonderlijke functies. De functie app-gedrag van toepassing op alle functies die worden gehost door een bepaalde functie-app. In dit onderwerp wordt beschreven hoe configureren en beheren van uw apps functie in de Azure-portal.

Om te beginnen, gaat u naar de [Azure-portal](http://portal.azure.com) en meld u aan bij uw Azure-account. Typ de naam van uw functie-app in de zoekbalk boven in de portal en selecteer deze in de lijst. Na het selecteren van de functie-app, ziet u de volgende pagina:

![Overzicht van de functie-app in de Azure portal](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

## <a name="manage-app-service-settings"></a>Tabblad voor de functie app-instellingen

![Overzicht van de functie app in de Azure portal.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

De **instellingen** tabblad is waarin u de functies runtime-versie die wordt gebruikt door de functie-app kunt bijwerken. Het is ook waar u de sleutels van de host gebruikt voor het beperken van HTTP-toegang tot alle functies die worden gehost door de functie-app beheren.

Functies ondersteunt zowel verbruik die als host fungeert als App Service-hostingplannen. Zie voor meer informatie [Kies de juiste service-abonnement voor Azure Functions](functions-scale.md). Voor betere voorspelbaarheid in het plan verbruik functies kunt u platform-gebruik beperken door een dagelijkse gebruiksquotum instellen in gigabytes seconden. Zodra de dagelijkse gebruiksquotum is bereikt, wordt de functie-app is gestopt. Een functie-app gestopt als gevolg van de quota voor bestedingslimiet bereikt kan opnieuw worden ingeschakeld vanuit de context hetzelfde als het tot stand brengen van de dagelijkse quotum uitgaven zijn. Zie de [Azure Functions pagina met prijzen](http://azure.microsoft.com/pricing/details/functions/) voor meer informatie over de facturering.   

## <a name="platform-features-tab"></a>Tabblad voor platform-onderdelen

![De functie app platform functies tabblad.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

Functie apps in uitgevoerd en worden beheerd, door de Azure App Service-platform. Als zodanig hebben uw functie apps toegang tot de meeste van de functies van Azure core webhosting-platform. De **platformfuncties** tabblad is waar u de vele nieuwe functies van de App Service-platform die u in uw apps functie gebruiken kunt toegang. 

> [!NOTE]
> Niet alle App Service-functies zijn beschikbaar wanneer een functie-app wordt uitgevoerd op het verbruik hosting-plan.

De rest van dit onderwerp richt zich op de volgende App Service-functies in de Azure-portal die handig voor functies zijn:

+ [App Service-editor](#editor)
+ [Toepassingsinstellingen](#settings) 
+ [Console](#console)
+ [Geavanceerde tools (Kudu)](#kudu)
+ [Implementatie-opties](#deployment)
+ [CORS](#cors)
+ [Verificatie](#auth)
+ [API-definitie](#swagger)

Zie voor meer informatie over het werken met App Service-instellingen, [Azure App Service-instellingen configureren](../app-service/web-sites-configure.md).

### <a name="editor"></a>App Service-Editor

| | |
|-|-|
| ![De functie app-App Service-editor.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | De App Service-editor is een geavanceerde in de portal-editor die u gebruiken kunt om JSON-configuratiebestanden en codebestanden die met te wijzigen. Als u deze optie kiest, wordt een afzonderlijke browsertabblad met een basic-editor gestart. Hiermee kunt u integreren met de Git-opslagplaats, uitvoeren en foutopsporing van code en de functie app-instellingen wijzigen. Deze editor biedt een verbeterde development environment voor uw functies vergeleken met de standaardblade voor de functie-app.    |

![De App Service-editor](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <a name="settings"></a>Toepassingsinstellingen

| | |
|-|-|
| ![Toepassingsinstellingen voor functie-app.](./media/functions-how-to-use-azure-function-app-settings/function-app-application-settings.png) | De App Service **toepassingsinstellingen** blade is waar u configureren en beheren van de framework-versies, foutopsporing op afstand, app-instellingen en verbindingsreeksen. Wanneer u de functie-app met andere Azure en services van derden integreren, kunt u deze instellingen hier wijzigen. Als u wilt verwijderen van een instelling, schuif naar rechts en selecteer de **X** pictogram aan de rechterkant van de regel (niet weergegeven in de volgende afbeelding).

![Configureer toepassingsinstellingen](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-settings.png)

### <a name="console"></a>Console

| | |
|-|-|
| ![De functie app-console in de Azure portal](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | De console in de portal is een hulpprogramma developer ideaal als u liever om te communiceren met de functie-app vanaf de opdrachtregel. Veelgebruikte opdrachten zijn directory en maken van het bestand en navigatie, evenals batch-bestanden en scripts uitvoeren. |

![De functie app-console](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <a name="kudu"></a>Geavanceerde tools (Kudu)

| | |
|-|-|
| ![Functie-app Kudu in de Azure portal](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | De geavanceerde hulpprogramma's voor App Service (ook wel bekend als Kudu) bieden toegang tot geavanceerde beheerfuncties van uw app in de functie. U beheert van kudu bovendien, systeemgegevens, app-instellingen, omgevingsvariabelen, site-uitbreidingen, HTTP-headers en servervariabelen. U kunt ook starten **Kudu** door te bladeren naar het eindpunt SCM voor uw app in de functie, zoals`https://<myfunctionapp>.scm.azurewebsites.net/` |

![Kudu configureren](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><a name="deployment">Implementatie-opties

| | |
|-|-|
| ![Opties voor de functie app-implementatie in de Azure portal](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | Functies kunt u uw functiecode op uw lokale machine ontwikkelen. Vervolgens kunt u uw lokale functie app-project uploaden naar Azure. Naast het uploaden van de traditionele FTP-functies kunt u een functie-app met populaire continue integratie-oplossingen zoals GitHub, VSTS Dropbox, Bitbucket en anderen implementeren. Zie voor meer informatie [continue implementatie voor Azure Functions](functions-continuous-deployment.md). Als u wilt uploaden handmatig met behulp van FTP- of lokale Git, moet u ook [configureren van de referenties voor uw implementatie](functions-continuous-deployment.md#credentials). |


### <a name="cors"></a>CORS

| | |
|-|-|
| ![Functie-app CORS in de Azure portal](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | Om te voorkomen dat schadelijke code wordt uitgevoerd in uw services, blokkeert App Service aanroepen naar uw apps functie van externe bronnen. Functies biedt ondersteuning voor cross-origin-resource delen (CORS) zodat u het definiëren van een 'whitelist' toegestane oorsprongen waarvan van uw functies kunnen externe aanvragen accepteert.  |

![Functie-App configureren CORS](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <a name="auth"></a>Verificatie

| | |
|-|-|
| ![Verificatie van de functie app in de Azure-portal](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | Wanneer functies een HTTP-trigger gebruikt, kunt u vereisen aanroepen voor het eerst worden geverifieerd. App Service biedt ondersteuning voor Azure Active Directory-verificatie en meld u aan met sociale providers, zoals Microsoft, Facebook en Twitter. Zie voor meer informatie over het configureren van specifieke verificatieproviders [overzicht van Azure App Service-authenticatie](../app-service/app-service-authentication-overview.md). |

![Verificatie voor een functie-app configureren](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <a name="swagger"></a>API-definitie

| | |
|-|-|
| ![De functie app API swagger-definitie in de Azure portal](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | Functies biedt ondersteuning voor Swagger wilt toestaan dat clients eenvoudiger uw HTTP-geactiveerde functies gebruiken. Bezoek voor meer informatie over het maken van API-definities met Swagger [aan de slag met API-Apps en Swagger in Azure](../app-service/app-service-web-tutorial-rest-api.md). U kunt ook functies proxy's gebruiken voor het definiëren van een één API-gebied voor meerdere functies. Zie voor meer informatie [werken met Azure Functions-proxy's](functions-proxies.md). |

![API voor functie-App configureren](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a>Volgende stappen

+ [Azure App Service-instellingen configureren](../app-service/web-sites-configure.md)
+ [Doorlopende implementatie voor Azure Functions](functions-continuous-deployment.md)



