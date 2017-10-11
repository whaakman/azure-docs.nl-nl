---
title: Azure Notification Hubs
description: Informatie over het toevoegen van push notification-mogelijkheden met Azure Notification Hubs.
author: ysxu
manager: erikre
editor: 
services: notification-hubs
documentationcenter: 
ms.assetid: fcfb0ce8-0e19-4fa8-b777-6b9f9cdda178
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 1/17/2017
ms.author: yuaxu
ms.openlocfilehash: a1be0b13cd1feb582a23965df142e44d90ac6851
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs"></a>Azure Notification Hubs
## <a name="overview"></a>Overzicht
Azure Notification Hubs bieden een eenvoudig te gebruiken, meerdere platforms, uitgebreid push-engine. Met een enkel platformoverschrijdende API-aanroep kunt u eenvoudig gerichte en gepersonaliseerde pushmeldingen verzenden naar elk mobiel platform van elke cloud of on-premises back-end.

Notification Hubs werkt prima voor enterprise- als consumentenscenario's. Hier volgen enkele voorbeelden klanten Notification Hubs voor gebruiken:

* Belangrijk nieuws meldingen verzenden naar miljoenen gebruikers met een lage latentie.
* Locatie gebaseerde coupons aan gebruikerssegmenten willen verzenden.
* Gebeurtenis-gerelateerde meldingen verzenden aan gebruikers of groepen voor media/Sport/Financiën/gaming-toepassingen.
* Push-aanbiedingen inhoud naar apps te benaderen markt aan klanten.
* Waarschuw gebruikers van bedrijfsgebeurtenissen zoals nieuwe berichten en werkitems.
* Codes voor multi-factor authentication verzenden.

## <a name="what-are-push-notifications"></a>Wat zijn pushmeldingen?
Pushmeldingen is een vorm van app-naar-gebruiker communicatie waar gebruikers van mobiele apps bepaalde gewenste gegevens, meestal in een pop-upvenster of dialoogvenster worden gewaarschuwd. Gebruikers kunnen doorgaans kiezen om te bekijken of verwijderen van het bericht en de mobiele app, die de melding heeft gecommuniceerd kiezen de voormalige wordt geopend.

Pushmeldingen is essentieel voor consumenten-apps in oplopende betrokkenheid van Apps en het gebruik en voor zakelijke apps bij het communiceren up-to-date bedrijfsgegevens. Het is de beste app aan gebruiker communicatie omdat het energie-efficiënte voor mobiele apparaten, voor de afzenders meldingen flexibele en beschikbaar is terwijl de overeenkomstige apps zijn niet actief.

Voor meer informatie over pushmeldingen voor enkele populaire platforms:
* [iOS](https://developer.apple.com/notifications/)
* [Android](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)
* [Windows](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx)

## <a name="how-push-notifications-work"></a>Hoe werken pushmeldingen?
Pushmeldingen worden geleverd via een platformspecifieke infrastructuur, het zogenaamde *Platformmeldingssystemen* (PNSes). Ze bieden barebone push functionaliteiten levering bericht op een apparaat met een opgegeven verwerken en hebben geen algemene interface. Versies van een app, de ontwikkelaar moeten een melding verzenden voor alle klanten via de iOS, Android en Windows werken met APNS (Apple Push Notification Service), FCM (Firebase Cloud Messaging) en WNS (Windows Notification Service) tijdens de batchverwerking de verzendt.

Op hoog niveau, moet u dit is de werking van push:

1. De clientapp besluit wil ontvangen pushes daarom neemt contact op met de bijbehorende PNS om op te halen van de unieke en tijdelijke push-ingang. Het ingangstype is afhankelijk van het systeem (bijvoorbeeld WNS heeft URI's terwijl APNS tokens heeft).
2. De clientapp slaat deze ingang op in de back-end app of de provider.
3. Voor het verzenden van een pushmelding contact back-end van de app op met de PNS met behulp van de ingang naar een specifieke client-App.
4. De PNS stuurt de melding door naar het apparaat dat is opgegeven door de ingang.

![][0]

## <a name="the-challenges-of-push-notifications"></a>De uitdagingen voor pushmeldingen
Hoewel PNSes krachtige, laat ze veel werk voor appontwikkelaars om te implementeren, zelfs algemene push notification scenario's, zoals het uitzenden of verzenden van pushmeldingen aan gesegmenteerde gebruikers.

Push is een van de meest aangevraagde functies in mobiele cloudservices, omdat de werking complexe infrastructuur die niet aan de app zakelijke logica is vereist. Enkele van de infrastructurele uitdagingen zijn:

* **Platformafhankelijkheid**: 

  * De back-end moet complex en moeilijk te onderhouden platformafhankelijk logica om meldingen te verzenden naar apparaten op verschillende platforms zoals PNSes zijn geen unified hebben.
* **Schaal**:

  * Per PNS-richtlijnen moeten de apparaattokens bij elke starten van de app worden vernieuwd. Dit betekent dat de back-end is omgaan met een grote hoeveelheid verkeer en database tot alleen de tokens up-to-date te houden. Wanneer het aantal apparaten honderden en miljarden groeit, is de kosten van het maken en onderhouden van deze infrastructuur enorme.
  * De meeste PNSes bieden geen ondersteuning voor uitzending naar meerdere apparaten. Dit betekent dat een eenvoudige uitzending naar een miljoen apparaten leidt tot een miljoen aanroepen naar de PNSes. Deze hoeveelheid verkeer schalen met een minimale latentie is zeer lastig.
* **Routering**:
  
  * Hoewel PNSes een manier om berichten te verzenden naar apparaten bieden, worden de meeste apps meldingen gericht op gebruikers of belangengroepen. Dit betekent dat de back-end moet ervoor zorgen dat een register apparaten koppelen aan belangengroepen, gebruikers, eigenschappen, enzovoort. Deze overhead worden toegevoegd aan de tijd op de markt onderhoudskosten en een app.

## <a name="why-use-notification-hubs"></a>Waarom werken met Notification Hubs?
Notification Hubs elimineert alle complexiteit die zijn gekoppeld aan het inschakelen van push op uw eigen. De infrastructuur van meerdere platforms, uitgebreid push notification minder push-gerelateerde codes en vereenvoudigt uw back-end. Met Notification Hubs zijn apparaten alleen verantwoordelijk voor het registreren van hun PNS-ingangen met een hub, terwijl de back-end berichten naar gebruikers of belangengroepen, verzendt zoals wordt weergegeven in de volgende afbeelding:

![][1]

Notification hubs is uw kant-en-klare push-engine met de volgende voordelen:

* **Cross-platforms**

  * Ondersteuning voor alle primaire push-platforms, waaronder iOS, Android, Windows, en Kindle en Baidu.
  * Een algemene interface naar push op alle platforms in platformspecifieke of platformonafhankelijk indelingen met geen platform-specifieke werk.
  * Apparaat verwerken op één plek.
* **Cross-back-ends**
  
  * Cloud of on-premises
  * .NET, Node.js, Java, enzovoort.
* **Groot aantal afleveringswijzen**:

  * *Uitzending naar een of meerdere platforms*: U kunt direct uitzenden naar miljoenen apparaten op platforms met één API-aanroep.
  * *Push naar apparaat*: U kunt meldingen op afzonderlijke apparaten richten.
  * *Push naar gebruiker*: functies Tags en sjablonen kunt u alle apparaten van een gebruiker die meerdere platforms.
  * *Het segment met dynamische tags-push*: functie labels kunt u segment apparaten en push ze aan uw wensen of u naar een segment of een expressie van segmenten (bijvoorbeeld het actieve van leven en in Seattle niet voor de nieuwe gebruiker verzendt). In plaats van wordt beperkt tot pub sub, kunt u bijwerken apparaat labels overal en altijd en overal.
  * *Gelokaliseerde push*: sjablonen functie helpt u bij het bereiken van lokalisatie zonder back endcode.
  * *Achtergrond push*: U kunt het patroon push naar pull kunnen door achtergrond meldingen verzenden naar apparaten en activering van deze om bepaalde worden of acties te voltooien.
  * *Geplande push*: U kunt plannen voor het verzenden van meldingen op elk gewenst moment.
  * *Direct push*: kunt u registreren apparaten met onze service overslaan en direct push naar een lijst met apparaten ingangen batch.
  * *Persoonlijke push*: apparaat push variabelen helpt bij het verzenden van apparaat-specifieke persoonlijke pushmeldingen met aangepaste sleutel / waarde-paren.
* **Uitgebreide telemetrie**
  
  * Algemene push, apparaat, fout en bewerking telemetrie is beschikbaar in de Azure-portal en programmatisch.
  * Houdt elke push vanuit een aanroep van uw eerste aanvraag voor de service is de pushes uit batchverwerking per bericht telemetrie.
  * Platform Notification System Feedback communiceert alle feedback van platform Notification System om te helpen bij foutopsporing.
* **Schaalbaarheid** 
  
  * Snelle berichten verzenden naar miljoenen apparaten zonder sharding moet worden veranderd of het apparaat.
* **Beveiliging**

  * Shared Access Secret (SAS) of federatieve verificatie.

## <a name="integration-with-app-service-mobile-apps"></a>Integratie met App Service Mobile Apps
Voor een naadloze werking van alle Azure-services biedt [App Service Mobile Apps] ingebouwde ondersteuning voor pushmeldingen via Notification Hubs. Met [App Service Mobile Apps] kunnen ontwikkelaars van ondernemingen en systeemintegrators gebruikmaken van een zeer schaalbaar, algemeen beschikbaar ontwikkelplatform voor mobiele toepassingen, dat uitgebreide mogelijkheden biedt voor ontwikkelaars van mobiele apps.

Mobiele apps van ontwikkelaars kunnen gebruikmaken van Notification Hubs in de volgende werkstroom:

1. PNS-ingang van het apparaat ophalen
2. Apparaten registreren met Notification Hubs via de handige Client SDK voor Mobile Apps registreren API
   * Om beveiligingsreden worden alle tags op registraties door mobiele apps verwijderd. Gebruik Notification Hubs om vanuit de back-end rechtstreeks tags aan apparaten koppelen.
3. Meldingen verzenden vanuit de back-end van uw app met Notification Hubs

Hier zijn een aantal voordelen voor ontwikkelaars:

* **Mobiele Apps Client-SDK's**: deze SDK's van meerdere platforms bieden eenvoudige API's voor registratie en neem contact op met de notification hub automatisch met de mobiele app gekoppeld. Ontwikkelaars hoeven niet op zoek naar de aanmeldgegevens voor Notification Hubs of met een extra service te werken.

  * *Push naar gebruiker*: de SDK's automatisch het opgegeven apparaat met Mobile Apps geverifieerde gebruikers-ID om in te schakelen, te pushen naar gebruikersscenario labelen.
  * *Push naar apparaat*: de SDK's gebruiken automatisch de installatie-ID van mobiele Apps als GUID om te registreren met Notification Hubs, Hierdoor hoeven de moeite van het onderhouden van meerdere service-GUID's.
* **Installatiemodel**: mobiele Apps werken met Notification Hubs meest recente pushmodel alle pusheigenschappen beschikbaar die zijn gekoppeld aan een apparaat in een JSON-installatie die wordt uitgelijnd met een Push Notification Services en eenvoudig te gebruiken.
* **Flexibiliteit**: ontwikkelaars kunnen altijd beslissen rechtstreeks met Notification Hubs werkt ook met de integratie.
* **Geïntegreerde ervaring in [Azure-portal]**: Push-als een mogelijkheid visueel wordt weergegeven in Mobile Apps en ontwikkelaars kunnen eenvoudig met de bijbehorende notification hub via mobiele Apps werken.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de Notification Hubs vindt u in de volgende onderwerpen:

* **[Hoe klanten Notification Hubs gebruiken]**
* **[Zelfstudies en handleidingen voor Notification Hubs]**
* **Notification Hubs aan de slag zelfstudies**: [iOS], [Android], [universele Windows-], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android]

[0]: ./media/notification-hubs-overview/registration-diagram.png
[1]: ./media/notification-hubs-overview/notification-hub-diagram.png
[Hoe klanten Notification Hubs gebruiken]: http://azure.microsoft.com/services/notification-hubs
[Zelfstudies en handleidingen voor Notification Hubs]: http://azure.microsoft.com/documentation/services/notification-hubs
[iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
[Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
[universele Windows-]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
[Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
[Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
[Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
[Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
[Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
[Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
[App Service Mobile Apps]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
[templates]: notification-hubs-templates-cross-platform-push-messages.md
[Azure-portal]: https://portal.azure.com
[tags]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
