---
title: Upgrade uitvoeren in Mobile Services in Azure App Service - Node.js
description: Meer informatie over het eenvoudige upgrade van uw Mobile Services-toepassing in een App Service-mobiele App
services: app-service\mobile
documentationcenter: 
author: conceptdev
manager: yochayk
editor: 
ms.assetid: c58f6df0-5aad-40a3-bddc-319c378218e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 9a641bc667d07d1b674d2864c1f29151d527f46a
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Upgrade van uw bestaande Mobile Service van een Node.js-Azure App service
App Service Mobile is een nieuwe manier om mobiele toepassingen bouwen met Microsoft Azure. Zie voor meer informatie, [wat zijn Mobile Apps?].

Dit artikel wordt beschreven hoe u een bestaande toepassing van de Node.js-back-end van Azure Mobile Services bijwerken naar een nieuwe App Service Mobile Apps. Terwijl u deze upgrade uitvoert, moet uw bestaande Mobile Services-toepassing kunt blijven werken.  Als u upgraden van een back-end voor Node.js-toepassing wilt, raadpleegt u [upgraden van uw .NET Mobile Services](app-service-mobile-net-upgrading-from-mobile-services.md).

Wanneer een mobiele back-end wordt bijgewerkt naar Azure App Service, wordt het toegang heeft tot alle functies van de App Service en worden in rekening gebracht volgens [App Service-prijzen], niet Mobile Services-prijzen.

## <a name="migrate-vs-upgrade"></a>Migreren upgraden
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Het is raadzaam dat u [uitvoeren van een migratie](app-service-mobile-migrating-from-mobile-services.md) voordat u doorgaat door middel van een upgrade. Op deze manier kunt u beide versies van uw toepassing op de dezelfde App Service-Plan geplaatst en er zijn geen extra kosten in rekening gebracht.
>
>

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Verbeteringen in Mobile Apps Node.js server SDK
Een upgrade naar de nieuwe [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) bevat veel verbeteringen, waaronder:

* Op basis van de [Express framework](http://expressjs.com/en/index.html), de nieuwe knooppunt-SDK is licht en ontworpen om te houden met nieuwe versies van het knooppunt als ze afkomstig zijn uit. U kunt het gedrag van toepassingen met een Express-middleware aanpassen.
* Aanzienlijke prestatieverbeteringen vergeleken met de Mobile Services SDK.
* U kunt nu een website host samen met uw mobiele back-end; Bovendien is het eenvoudig de Azure Mobile SDK toevoegen aan een bestaande express.v4-toepassing.
* Gebouwd voor verschillende platforms en lokale ontwikkeling, kunnen de Mobile Apps SDK worden ontwikkeld en lokaal uitvoeren op Windows, Linux en OS x-platforms. Het is nu eenvoudig te gebruiken algemene knooppunt-technieken zoals uitgevoerd [Mocha](https://mochajs.org/) tests voorafgaand aan de implementatie.

## <a name="overview"></a>Upgrade basisoverzicht
Azure App Service heeft een pakket compatibiliteit verstrekt om u te helpen bij het bijwerken van een back-end voor Node.js.  Na de upgrade hebt u een nieuwe site die kan worden geïmplementeerd naar een nieuwe App Service-site.

De Mobile Services-client-SDK's zijn **niet** compatibel is met de nieuwe server voor Mobile Apps SDK. U moet wijzigingen niet aan een site die momenteel bedient gepubliceerde clients publiceren, zodat de continuïteit van de service voor uw app. In plaats daarvan moet u een nieuwe mobiele app die als een duplicaat fungeert maken. U kunt deze toepassing op de dezelfde App Service-abonnement om te vermijden extra financiële kosten plaatsen.

U moet dan twee versies van de toepassing: blijft hetzelfde, en fungeert gepubliceerde apps in het wild en de andere die u kunt vervolgens een upgrade en doel met een nieuwe client-release. U kunt verplaatsen en testen van uw code in uw tempo, maar moet u ervoor zorgen dat u alle oplossingen voor problemen op beide toegepast. Als u denkt dat een gewenste aantal client-apps in het wild hebt bijgewerkt naar de nieuwste versie, u de oorspronkelijke gemigreerde app verwijderen kunt als u wenst. Hiervoor niet eventuele extra financieel kosten als gehost in dezelfde App Service-abonnement als uw mobiele App.

Het volledige overzicht voor het upgradeproces is als volgt:

1. Download uw bestaande (gemigreerde) Azure Mobile Service.
2. Het project niet converteren naar een Azure Mobile Apps met behulp van het pakket compatibiliteit.
3. Corrigeer eventuele verschillen (zoals de verificatie-instellingen).
4. De geconverteerde mobiele Apps van Azure-project implementeren in een nieuwe App Service.
5. Een nieuwe versie van uw clienttoepassing die gebruikmaakt van de nieuwe mobiele App.
6. (Optioneel) Verwijder uw oorspronkelijke gemigreerde mobile service-app.

Verwijderen kan optreden wanneer verkeer niet wordt weergegeven op uw oorspronkelijke gemigreerde mobiele service.

## <a name="install-npm-package"></a>Installeren van de vereisten
U moet [Node] installeren op uw lokale machine.  Ook moet u het pakket compatibiliteit installeren.  Nadat het knooppunt is geïnstalleerd, kunt u de volgende opdracht uitvoeren vanaf een nieuwe cmd of PowerShell-prompt:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Verkrijgen van uw Azure Mobile Services-Scripts
* Meld u aan bij de [Azure Portal].
* Met behulp van **alle Resources** of **App Services**, zoeken naar uw Mobile Services-site.
* Binnen de site, klikt u op **extra** -> **Kudu** -> **gaat** de Kudu-website te openen.
* Klik op **Console voor foutopsporing** -> **PowerShell** om de console voor foutopsporing te openen.
* Navigeer naar `site/wwwroot/App_Data/config` door te klikken op elke map op zijn beurt
* Klik op het downloadpictogram naast de `scripts` directory.

Dit wordt gedownload, de scripts in een ZIP-indeling.  Maak een nieuwe map op uw lokale machine en pak deze uit de `scripts.ZIP` bestand in de map.  Hiermee maakt u een `scripts` directory.

## <a name="scaffold-app"></a>Ondersteuning van de nieuwe back-end voor Azure Mobile Apps
Voer de volgende opdracht uit de map waarin de map scripts:

```scaffold-mobile-app scripts out```

Hiermee maakt u een ondersteunde back-end Azure Mobile Apps in de `out` directory.  Hoewel niet vereist, is een goed idee om te controleren de `out` map in een bron code opslagplaats van uw keuze.

## <a name="deploy-ama-app"></a>Uw back-end van Azure Mobile Apps implementeren
Tijdens de implementatie moet u het volgende doen:

1. Maak een nieuwe mobiele App in de [Azure Portal].
2. Voer de `createViews.sql` script op basis van de database verbonden.
3. Koppel de database die is gekoppeld aan uw mobiele Service in uw nieuwe App Service.
4. Andere bronnen (zoals Notification Hubs) koppelen aan de nieuwe App Service.
5. De gegenereerde code implementeren op de nieuwe site.

### <a name="create-a-new-mobile-app"></a>Een nieuwe mobiele App maken
1. Meld u aan bij de [Azure Portal].
2. Klik op **+NIEUW** > **Web en mobiel** > **Mobile App** en geef een naam op voor de nieuwe back-end van Mobile App.
3. Selecteer voor de **Resourcegroep** een bestaande resourcegroep of maak een nieuwe (gebruik dezelfde naam als uw app).

    U kunt ofwel een ander App Service-abonnement selecteren of een nieuw maken. Voor meer informatie over App-Services plannen en hoe u een nieuw plan maakt in een andere prijscategorie servicetier en op de gewenste locatie, Zie [gedetailleerd overzicht van Azure App Service-plannen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
4. Voor het **App Service-abonnement** is het standaardabonnement (in de [standaardcategorie](https://azure.microsoft.com/pricing/details/app-service/)) geselecteerd. U kunt ook een ander abonnement selecteren of [Maak een nieuwe](../app-service/app-service-plan-manage.md#create-an-app-service-plan). Instellingen voor de App Service-abonnement bepalen de [locatie, functies, kosten en rekenresources](https://azure.microsoft.com/pricing/details/app-service/) die zijn gekoppeld aan uw app.

    Nadat u voor een beslissing hebt genomen met betrekking tot een bepaald abonnement, klikt u op **Maken**. Hiermee maakt u de back-end voor de mobiele app.

### <a name="run-createviewssql"></a>CreateViews.SQL uitvoeren
De gegenereerde app bevat een bestand met de naam `createViews.sql`.  Dit script moet worden uitgevoerd met de doeldatabase.  De verbindingsreeks voor de doeldatabase kan worden verkregen van de gemigreerde mobiele service uit de **instellingen** pagina onder **verbindingsreeksen**.  Dit sjabloon heet `MS_TableConnectionString`.

U kunt dit script in SQL Server Management Studio of Visual Studio kunt uitvoeren.

### <a name="link-the-database-to-your-app-service"></a>De Database koppelen aan uw App Service
De bestaande database koppelen aan uw App Service:

* In de [Azure Portal], opent u uw App Service.
* Selecteer **alle instellingen** -> **gegevensverbindingen**.
* Klik op **+ toevoegen**.
* Selecteer in de vervolgkeuzelijst **SQL-Database**
* Onder **SQL-Database**, selecteer uw bestaande database en klik vervolgens op **Selecteer**.
* Onder **verbindingsreeks**, de gebruikersnaam en wachtwoord invoeren voor de database en klik vervolgens op **OK**.
* In de **gegevensverbindingen toevoegen** pagina, klikt u op **OK**.

Gebruikersnaam en wachtwoord kunnen worden gevonden door de verbindingsreeks voor de doeldatabase in uw gemigreerde Mobile Service weer te geven.

### <a name="set-up-authentication"></a>Verificatie instellen
Mobiele Apps van Azure kunt u voor het configureren van Azure Active Directory, Facebook, Google, Microsoft en Twitter verificatie in de service.  Aangepaste verificatie moet afzonderlijk worden ontwikkeld.  Raadpleeg de [Authenticatieconcepten] documentatie en [verificatie Quick Start] documentatie voor meer informatie.  

## <a name="updating-clients"></a>Mobiele clients bijwerken
Zodra u een operationele back-end voor mobiele apps hebt, kunt u werken op een nieuwe versie van uw clienttoepassing waarin deze worden verbruikt. Mobile Apps bevat ook een nieuwe versie van de client-SDK's en net als bij de upgrade van de server hierboven, moet u alle verwijzingen naar de Mobile Services SDK's verwijderen voordat u de Mobile Apps-versie installeert.

Een van de belangrijkste wijzigingen tussen de versies is dat de constructors zijn niet langer vereist voor de sleutel van een toepassing.
U nu doorgeven gewoon in de URL van uw mobiele App. Bijvoorbeeld: op de .NET-clients de `MobileServiceClient` constructor is nu:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net" // URL of the Mobile App
        );

U kunt lezen over het installeren van de nieuwe SDK's en met behulp van de nieuwe structuur via de onderstaande koppelingen:

* [Android versie 2.2 of hoger](app-service-mobile-android-how-to-use-client-library.md)
* [iOS-versie 3.0.0 of hoger](app-service-mobile-ios-how-to-use-client-library.md)
* [.NET (Windows/Xamarin) versie 2.0.0 of hoger](app-service-mobile-dotnet-how-to-use-client-library.md)
* [Apache Cordova-versie 2.0 of hoger](app-service-mobile-cordova-how-to-use-client-library.md)

Als uw toepassing maakt gebruik van pushmeldingen, noteer de specifieke registratie-instructies voor elk platform, zijn omdat er er enkele wijzigingen ook.

Wanneer u de nieuwe clientversie gereed hebt, try it out in op basis van uw project bijgewerkte server. Na de validatie of deze werkt, kunt u een nieuwe versie van uw toepassing op klanten vrijgeven. Zodra uw klanten hebt om deze updates te ontvangen, kunt u uiteindelijk de Mobile Services-versie van uw app verwijderen. U hebt op dit moment volledig bijgewerkt naar een App Service Mobile App met behulp van de meest recente server-Mobile Apps SDK.

<!-- URLs. -->

[Azure Portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[wat zijn Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: https://github.com/Azure/azure-webjobs-sdk/wiki
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[App Service-prijzen]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Authenticatieconcepten]: ../app-service/app-service-authentication-overview.md
[verificatie Quick Start]: app-service-mobile-auth.md

[Azure Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
