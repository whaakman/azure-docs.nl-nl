---
title: Upgrade uitvoeren in Mobile Services in Azure App Service - Node.js
description: Leer hoe u eenvoudig uw Mobile Services-toepassing naar een App Service Mobile App upgraden
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: yochayk
editor: ''
ms.assetid: c58f6df0-5aad-40a3-bddc-319c378218e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 71be338aeb6d0234d22d412d6838e36a26797b20
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "53002115"
---
# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Uw bestaande Node.js Azure Mobile Service upgraden naar App Service
App Service Mobile is een nieuwe manier voor het bouwen van mobiele toepassingen met Microsoft Azure. Zie voor meer informatie, [Wat zijn mobiele apps?].

In dit artikel wordt beschreven hoe u een bestaande Node.js-back-endtoepassing upgraden van Azure Mobile Services naar een nieuwe App Service Mobile Apps. Hoewel u deze upgrade uitvoeren, moet uw bestaande Mobile Services-toepassing kan blijven functioneren.  Als u een Node.js-back-end-toepassing upgraden wilt, raadpleegt u [upgraden van uw .NET Mobile Services](app-service-mobile-net-upgrading-from-mobile-services.md).

Een mobiele back-end wordt bijgewerkt naar Azure App Service, wordt toegang heeft tot alle functies van App Service en worden in rekening gebracht volgens [prijzen van App Service], niet Mobile Services-prijzen.

## <a name="migrate-vs-upgrade"></a>Migreren upgraden
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Het is raadzaam dat u [een migratie uit te voeren](app-service-mobile-migrating-from-mobile-services.md) voordat er een upgrade voor aangebracht. Op deze manier kunt u beide versies van uw toepassing op de dezelfde App Service-Plan plaatsen en geen extra kosten in rekening gebracht.
>
>

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Verbeteringen in Mobile Apps Node.js server SDK
Een upgrade naar de nieuwe [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) bevat veel verbeteringen, waaronder:

* Op basis van de [Express framework](https://expressjs.com/en/index.html), de nieuwe Node-SDK is lichte en ontworpen om u te houden met nieuwe versies van het knooppunt als ze afkomstig zijn uit. U kunt het gedrag van toepassingen met Express-middleware aanpassen.
* Aanzienlijke prestatieverbeteringen in vergelijking met de Mobile Services SDK.
* U kunt nu een website hosten, samen met uw mobiele back-end; op deze manier is het eenvoudig de Azure Mobile SDK toevoegen aan een bestaande express.v4-toepassing.
* Gebouwd voor meerdere platformen en lokale ontwikkeling, kunnen de Mobile Apps SDK worden ontwikkeld en lokaal uitvoeren op Windows, Linux en OS x-platforms. Het is nu eenvoudig te gebruiken algemene knooppunt-ontwikkeltechnieken zoals uitgevoerd [Mocha](https://mochajs.org/) testen voorafgaand aan de implementatie.

## <a name="overview"></a>Basisoverzicht van upgrade
Azure App Service heeft een pakket compatibiliteit beschikbaar om te helpen bij het bijwerken van een Node.js-backend.  Na de upgrade hebt u een nieuwe site die kan worden geïmplementeerd naar een nieuwe App Service-site.

De Mobile Services-client-SDK's zijn **niet** compatibel is met de server-nieuwe Mobile Apps SDK. Zodat de continuïteit van de service voor uw app, moet u geen wijzigingen aan een site momenteel fungeert gepubliceerde clients publiceren. In plaats daarvan moet u een nieuwe mobiele app die als een duplicaat fungeert maken. U kunt deze toepassing op de dezelfde App Service-plan om te vermijden extra financiële kosten plaatsen.

U hebt dan twee versies van de toepassing: een die hetzelfde blijft en dient gepubliceerde apps in het gebruik en de andere die u kunt vervolgens een upgrade en een nieuwe versie van de client is gericht. U kunt verplaatsen en het testen van uw code in uw eigen tempo, maar moet u ervoor zorgen dat u eventuele oplossingen voor problemen op beide worden toegepast. Als u denkt dat een gewenste aantal client-apps in de natuur hebt bijgewerkt naar de nieuwste versie, u kunt de oorspronkelijke gemigreerde app verwijderen als u wilt. Het biedt geen eventuele aanvullende kosten monetaire, als die worden gehost in hetzelfde App Service-abonnement als uw mobiele App.

Het volledige overzicht voor het upgradeproces is als volgt:

1. Download uw bestaande (gemigreerde) Azure Mobile Service.
2. Het project converteren naar een Azure Mobile App met behulp van het pakket compatibiliteit.
3. Corrigeer eventuele verschillen (zoals verificatie-instellingen).
4. De geconverteerde mobiele Apps van Azure-project implementeren in een nieuw App Service.
5. Een nieuwe versie van uw clienttoepassing die gebruikmaakt van de nieuwe mobiele App.
6. (Optioneel) Verwijder de oorspronkelijke gemigreerde mobiele service-app.

Verwijdering kan optreden wanneer er geen verkeer van uw oorspronkelijke gemigreerde mobiele service.

## <a name="install-npm-package"></a> De vereiste onderdelen installeren
U moet [Node] installeren op uw lokale computer.  Ook moet u de compatibiliteit-pakket installeren.  Nadat het knooppunt is geïnstalleerd, kunt u de volgende opdracht uitvoeren vanaf een nieuwe cmd of PowerShell-prompt:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a> Verkrijgen van uw Azure Mobile Services-Scripts
* Meld u aan bij de [Azure Portal].
* Met behulp van **alle Resources** of **App Services**, vinden van uw Mobile Services-site.
* Binnen de site, klikt u op **extra** -> **Kudu** -> **gaat** openen van de Kudu-site.
* Klik op **Foutopsporingsconsole** -> **PowerShell** openen van de console voor foutopsporing.
* Navigeer naar `site/wwwroot/App_Data/config` door te klikken op elke map op zijn beurt
* Klik op het downloadpictogram naast de `scripts` directory.

Hiermee downloadt u de scripts in het ZIP-bestand.  Maak een nieuwe map op uw lokale computer en pak het `scripts.ZIP` bestand in de map.  Hiermee maakt u een `scripts` directory.

## <a name="scaffold-app"></a> De nieuwe Azure Mobile Apps-back-end-ondersteuning
Voer de volgende opdracht uit de map met de map scripts:

```scaffold-mobile-app scripts out```

Hiermee maakt u een ondersteunde Azure Mobile Apps-back-end in de `out` directory.  Hoewel niet vereist, is een goed idee om te controleren of de `out` map in een opslagplaats broncode van uw keuze.

## <a name="deploy-ama-app"></a> Uw back-end van Azure Mobile Apps implementeren
Tijdens de implementatie gaat u als volgt te werk:

1. Maak een nieuwe mobiele App in de [Azure Portal].
2. Voer de `createViews.sql` script op uw database verbonden.
3. Koppel de database die is gekoppeld aan uw mobiele Service aan uw nieuwe App Service.
4. Andere bronnen (zoals Notification Hubs) koppelen aan de nieuwe App Service.
5. De gegenereerde code implementeren in de nieuwe site.

### <a name="create-a-new-mobile-app"></a>Maak een nieuwe mobiele App
1. Meld u aan bij de [Azure Portal].
2. Klik op **+NIEUW** > **Web en mobiel** > **Mobile App** en geef een naam op voor de nieuwe back-end van Mobile App.
3. Selecteer voor de **Resourcegroep** een bestaande resourcegroep of maak een nieuwe (gebruik dezelfde naam als uw app).

    U kunt ofwel een ander App Service-abonnement selecteren of een nieuw maken. Zie [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) (Uitgebreid overzicht van Azure App Service-abonnementen) voor meer informatie over App Service-abonnementen en hoe u een nieuw abonnement maakt in een andere prijscategorie op de door u gewenste locatie.
4. Voor het **App Service-abonnement** is het standaardabonnement (in de [standaardcategorie](https://azure.microsoft.com/pricing/details/app-service/)) geselecteerd. U kunt ook een ander abonnement selecteren of [een nieuw abonnement maken](../app-service/app-service-plan-manage.md#create-an-app-service-plan). Instellingen voor de App Service-plan bepalen de [locatie, functies, kosten en rekenresources](https://azure.microsoft.com/pricing/details/app-service/) die zijn gekoppeld aan uw app.

    Nadat u voor een beslissing hebt genomen met betrekking tot een bepaald abonnement, klikt u op **Maken**. Hiermee maakt u de back-end voor de mobiele app.

### <a name="run-createviewssql"></a>CreateViews.SQL uitvoeren
De gegenereerde app bevat een bestand met de naam `createViews.sql`.  Met dit script moet worden uitgevoerd op de doeldatabase.  De verbindingsreeks voor de doeldatabase kan worden opgehaald van de gemigreerde mobiele service uit de **instellingen** pagina onder **verbindingsreeksen**.  De naam ervan is `MS_TableConnectionString`.

U kunt dit script in SQL Server Management Studio of Visual Studio kunt uitvoeren.

### <a name="link-the-database-to-your-app-service"></a>De Database koppelen aan uw App Service
De bestaande database koppelen aan uw App Service:

* In de [Azure Portal], opent u uw App Service.
* Selecteer **alle instellingen** -> **gegevensverbindingen**.
* Klik op **+ toevoegen**.
* Selecteer in de vervolgkeuzelijst **SQL-Database**
* Onder **SQL-Database**, selecteert u de bestaande database en klik op **Selecteer**.
* Onder **Connection string**, geef de gebruikersnaam en het wachtwoord voor de database, en klik vervolgens op **OK**.
* In de **gegevensverbindingen toevoegen** pagina, klikt u op **OK**.

De gebruikersnaam en het wachtwoord kunnen worden gevonden door de verbindingsreeks voor de doeldatabase in uw gemigreerde Mobile Service weer te geven.

### <a name="set-up-authentication"></a>Verificatie instellen
Azure Mobile Apps kunt u voor het configureren van Azure Active Directory, Facebook, Google, Microsoft en Twitter-verificatie in de service.  Aangepaste verificatie moet afzonderlijk worden ontwikkeld.  Raadpleeg de [verificatie op basis van] documentatie en [Snelstartgids voor verificatie] documentatie voor meer informatie.  

## <a name="updating-clients"></a>Mobiele clients bijwerken
Zodra u een operationele backend voor mobiele apps hebt, kunt u werken op een nieuwe versie van uw clienttoepassing die wordt verbruikt. Mobile Apps bevat ook een nieuwe versie van de client-SDK's en net als bij de upgrade van de server boven, u moet alle verwijzingen naar de Mobile Services SDK's verwijderen voordat u de installatie van de Mobile Apps-versies.

Een van de belangrijkste wijzigingen tussen de versies is dat de constructors niet langer vereist voor de sleutel van een toepassing.
U nu doorgeven gewoon in de URL van uw mobiele App. Bijvoorbeeld: op de .NET-clients de `MobileServiceClient` constructor is nu:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net" // URL of the Mobile App
        );

U kunt lezen over het installeren van de nieuwe SDK's en met behulp van de nieuwe structuur via de onderstaande koppelingen:

* [Android-versie 2.2 of hoger](app-service-mobile-android-how-to-use-client-library.md)
* [iOS-versie 3.0.0 of hoger](app-service-mobile-ios-how-to-use-client-library.md)
* [.NET (Windows/Xamarin) versie 2.0.0 of hoger](app-service-mobile-dotnet-how-to-use-client-library.md)
* [Apache Cordova, versie 2.0 of hoger](app-service-mobile-cordova-how-to-use-client-library.md)

Als uw toepassing maakt gebruik van push-meldingen, noteert u de registratie van de specifieke instructies voor elk platform, als er zijn enkele wijzigingen er ook.

Wanneer u de nieuwe clientversie gereed, probeer het op basis van uw project bijgewerkte server. Nadat is gevalideerd dat het werkt, kunt u een nieuwe versie van uw toepassing op klanten vrijgeven. Uiteindelijk, nadat uw klanten hebben al een kans om deze updates te ontvangen, kunt u de versie van de Mobile Services van uw app verwijderen. U hebt op dit moment volledig bijgewerkt naar een App Service Mobile App met behulp van de meest recente Mobile Apps-server-SDK.

<!-- URLs. -->

[Azure Portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Wat zijn mobiele apps?]: app-service-mobile-value-prop.md
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
[prijzen van App Service]: https://azure.microsoft.com/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Verificatie op basis van]: ../app-service/app-service-authentication-overview.md
[Snelstartgids voor verificatie]: app-service-mobile-auth.md

[Azure Portal]: https://portal.azure.com/
[OData]: https://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: https://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: https://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
