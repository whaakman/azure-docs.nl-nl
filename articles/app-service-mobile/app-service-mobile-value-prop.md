---
title: Mobile Apps in Azure App Service
description: Lees welke voordelen App Service heeft voor de mobiele apps in uw onderneming.
services: app-service\mobile
documentationcenter: ''
author: elamalani
manager: yochayk
editor: ''
ms.assetid: 4e96cb9d-a632-4cf6-8219-0810d8ade3f9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 55f33821a748789fb11b44dc08c367961b4258f4
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67440279"
---
# <a name="getting-started"> </a>Mobile Apps in Azure App Service

> [!NOTE]
> Visual Studio App Center investeert in nieuwe en geïntegreerde services centraal staat in de ontwikkeling van mobiele Apps. Ontwikkelaars kunnen gebruikmaken van **bouwen**, **Test** en **verdelen** services voor het instellen van de pijplijn voor continue integratie en levering. Zodra de app is geïmplementeerd, ontwikkelaars controleren de status en het gebruik van het gebruik van de app de **Analytics** en **Diagnostics** -services en Communiceer met gebruikers met behulp van de **Push** de service. Ontwikkelaars kunnen ook gebruikmaken van **Auth** om hun gebruikers te verifiëren en **gegevens** service behouden en synchroniseren van app-gegevens in de cloud. Bekijk [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-value-prop) vandaag nog.
>

Azure App Service is een volledig beheerde [PaaS-](https://azure.microsoft.com/overview/what-is-paas/)aanbieding (Platform as a Service) voor professionele ontwikkelaars. De service biedt een uitgebreide reeks mogelijkheden voor web-, mobiele en integratiescenario's. 

De functie Mobile Apps in Azure App Service is een zeer schaalbaar, wereldwijd beschikbaar platform waarmee ontwikkelaars in zakelijke ondernemingen en systeemintegrators mobiele toepassingen kunnen ontwikkelen.

![Visueel overzicht van de mogelijkheden van Mobile Apps](./media/app-service-mobile-value-prop/overview.png)

## <a name="why-mobile-apps"></a>Waarom Mobile Apps?
Met de functie Mobile Apps kunt u het volgende doen:

* **Systeemeigen en platformoverschrijdende apps bouwen**: of u nu systeemeigen iOS-, Android- of Windows-apps, of platformoverschrijdende Xamarin- of Cordova-apps (Phonegap) bouwt, u hebt altijd voordeel van App Service door systeemeigen SDK's te gebruiken.
* **Verbinding maken met uw bedrijfssystemen**: met de functie Mobile Apps kunt u in slechts enkele minuten zakelijke aanmeldingen toevoegen en verbinding maken met uw bedrijfsresources, on-premises of in de cloud.
* **Apps bouwen die offline beschikbaar zijn met gegevenssynchronisatie**: maak uw mobiele werknemers productiever door apps te bouwen die offline werken en door gebruik te maken van Mobile Apps om gegevens op de achtergrond te synchroniseren wanneer er verbinding is met een van uw gegevensbronnen of SaaS-API's (Software as a Service) in de onderneming.
* **Pushmeldingen in enkele seconden naar miljoenen klanten verzenden**: houd contact met uw klanten door gebruik te maken van directe pushmeldingen op elk apparaat, afgestemd op de eigen behoeften van de klant en verzonden op het gewenste moment.

## <a name="mobile-apps-features"></a>Functies van Mobile Apps
De volgende functies zijn belangrijk wanneer u mobiele apps ontwikkelt die zijn ingeschakeld voor de cloud:

* **Verificatie en autorisatie**: ondersteuning voor id-providers, zoals Azure Active Directory, voor ondernemingsverificatie, plus providers van sociale netwerken, zoals Facebook-, Google-, Twitter- en Microsoft-accounts. Mobile Apps biedt een OAuth 2.0-service voor elke provider. Daarnaast kunt u de SDK voor de id-provider ook integreren voor providerspecifieke functionaliteit.

    Meer informatie over de [verificatiefuncties].

* **Gegevenstoegang**: Mobile Apps biedt een voor mobiele apparaten geschikte OData v3-gegevensbron die is gekoppeld aan Azure SQL Database of een on-premises SQL-server. Omdat deze service kan worden gebaseerd op Entity Framework, kunt u eenvoudig integreren met andere NoSQL- en SQL-gegevensproviders, zoals [Azure Table Storage], MongoDB, [Azure Cosmos DB] en SaaS API-providers zoals Office 365 en Salesforce.com.

* **Offlinesynchronisatie**: met behulp van de client-SDK's kunt u eenvoudig robuuste en responsieve mobiele toepassingen ontwikkelen die met een offline gegevensset werken. U kunt deze gegevensset automatisch met de gegevens in de back-end synchroniseren en er is ook ondersteuning voor conflictoplossing.

  Meer informatie over de [gegevensfuncties].

* **Pushmeldingen**: de client-SDK's kunnen probleemloos worden geïntegreerd met de registratiemogelijkheden van Azure Notification Hubs, zodat u pushmeldingen naar miljoenen gebruikers tegelijk kunt verzenden.

  Meer informatie over de [functies voor pushmeldingen].

* **Client-SDK 's**: er is een volledige set client-SDK's voor systeemeigen ontwikkeling ([iOS], [Android] en [Windows]), platformoverschrijdende ontwikkeling ([Xamarin.iOS en Xamarin.Android], [Xamarin.Forms]) en hybride toepassingsontwikkeling ([Apache Cordova]). Elke client-SDK is beschikbaar met een MIT-licentie en is open source.

## <a name="azure-app-service-features"></a>Functies van Azure App Service
De volgende platformfuncties zijn handig voor mobiele productiesites:

* **Automatische schaling**: met App Service kunt u snel omhoog of uitschalen om in te spelen op de inkomende belasting van klanten. U kunt handmatig het aantal VM's en de grootte ervan selecteren of automatische schaling instellen, zodat de back-end voor uw mobiele app wordt geschaald op basis van uw belasting of schema.

  Lees meer over [Automatische schaling].

* **Faseringsomgevingen**: met App Service kunt u meerdere versies van uw site uitvoeren, zodat u A/B-tests, tests in de productieomgeving als onderdeel van een groter DevOps-plan en in-place fasering van een nieuwe back-end kunt uitvoeren.

  Lees meer over [Faseringsomgevingen].

* **Doorlopende implementatie**: App Service kan worden geïntegreerd met veelgebruikte _broncodebeheersystemen_ (Source Control Management), zodat u eenvoudig een nieuwe versie van uw back-end kunt implementeren.

  Lees meer over [implementatieopties](../app-service/deploy-local-git.md).

* **Virtuele netwerken**: App Service kan verbinding maken met on-premises resources met behulp van een virtueel netwerk, Azure ExpressRoute of hybride verbindingen.

  Lees meer over [hybride verbindingen], [virtuele netwerken] en [ExpressRoute].

* **Geïsoleerde / toegewezen omgevingen**: u kunt App Service uitvoeren in een volledig geïsoleerde en toegewezen omgeving, zodat Azure App Service-apps veilig kunnen worden uitgevoerd. Deze omgeving is ideaal voor toepassingsworkloads die op grote schaal worden uitgevoerd, of waarvoor isolatie of beveiligde netwerktoegang nodig is.

  Lees meer over [App Service-omgevingen].

## <a name="next-steps"></a>Volgende stappen

Begin met de zelfstudie [Aan de slag] om snel bekend te raken met Mobile Apps in Azure App Service. De zelfstudie bevat de basisinformatie voor het produceren van een mobiele back-end en een client van uw keuze. Er wordt ook aandacht besteed aan het integreren van verificatie, offlinesynchronisatie en pushmeldingen. U kunt de zelfstudie meerdere keren volgen, steeds voor een andere clienttoepassing.

Zie ons [leeroverzicht] voor meer informatie over Azure Mobile Apps.
Zie [Azure App Service] voor meer informatie over het Azure App Service-platform.

<!-- URLs. -->
[Migrate your mobile service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[aan de slag]: app-service-mobile-ios-get-started.md
[Azure Table storage]:../cosmos-db/table-storage-how-to-use-dotnet.md
[Azure Cosmos DB]: ../cosmos-db/sql-api-get-started.md
[verificatiefuncties]: ./app-service-mobile-auth.md
[gegevensfuncties]: ./app-service-mobile-offline-data-sync.md
[functies voor pushmeldingen]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin.iOS en Xamarin.Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin.Forms]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[Automatische schaling]: ../app-service/web-sites-scale.md
[Faseringsomgevingen]: ../app-service/deploy-staging-slots.md
[hybride verbindingen]: ../biztalk-services/integration-hybrid-connection-overview.md
[virtuele netwerken]: ../app-service/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service/environment/app-service-app-service-environment-network-configuration-expressroute.md
[App Service-omgevingen]: ../app-service/environment/intro.md
[leeroverzicht]: https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/
