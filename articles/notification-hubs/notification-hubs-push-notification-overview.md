---
title: Wat is Azure Notification Hubs?
description: Leer hoe u mogelijkheden voor pushmeldingen toevoegt met Azure Notification Hubs.
author: jwargo
manager: patniko
editor: spelluru
services: notification-hubs
documentationcenter: ''
ms.assetid: fcfb0ce8-0e19-4fa8-b777-6b9f9cdda178
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: overview
ms.custom: mvc
ms.date: 04/30/2019
ms.author: jowargo
ms.openlocfilehash: 03d4c269f76a89c43dec253367d07f3bf71a06d8
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65141213"
---
# <a name="what-is-azure-notification-hubs"></a>Wat is Azure Notification Hubs?

Azure Notification Hubs biedt een eenvoudig te gebruiken en uitgeschaalde push-engine waarmee u vanuit elke back-end (cloud of on-premises) meldingen kunt verzenden naar ieder platform (iOS, Android, Windows, Kindle, Baidu, enzovoort). Notification Hubs is zeer geschikt voor zowel bedrijfs- als consumentenscenario's. Enkele voorbeelden:

- Meldingen van belangrijk nieuws met een lage latentie verzenden naar miljoenen gebruikers.
- Coupons op basis van locatie verzenden naar geïnteresseerde gebruikerssegmenten.
- Meldingen voor evenementen verzenden naar gebruikers of groepen voor bijvoorbeeld media-, sport- en financiële toepassingen.
- Aanbiedingen pushen naar toepassingen om klanten te benaderen en werven.
- Gebruikers van bedrijfsgebeurtenissen op de hoogte stellen, zoals nieuwe berichten en werkitems.
- Codes verzenden voor meervoudige verificatie.

## <a name="what-are-push-notifications"></a>Wat zijn pushmeldingen?

Pushmeldingen is een vorm van communicatie van de app-naar-gebruiker waar gebruikers van mobiele apps van bepaalde gewenste gegevens, meestal in een pop-upvenster of dialoogvenster op een mobiel apparaat worden gewaarschuwd. Gebruikers kiezen in het algemeen te bekijken of te verwijderen van het bericht. het voormalige wordt geopend kiezen de mobiele App en dat de melding gecommuniceerd. Sommige meldingen worden op de achtergrond - geleverd achter de schermen voor de app achter de schermen worden verwerkt en bepaal wat te doen.

Pushmeldingen zijn essentieel voor consumenten-apps om de betrokkenheid en het gebruik te vergroten. Voor zakelijke apps zijn ze belangrijk om actuele bedrijfsgegevens te communiceren. Het is de communicatie voor de beste app-naar-gebruiker omdat het energie-efficiënte voor mobiele apparaten, flexibele voor de afzenders meldingen en beschikbaar is wanneer de bijbehorende toepassingen zijn niet actief.

Zie de volgende onderwerpen voor meer informatie over pushmeldingen voor enkele populaire platforms:

- [Android](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)
- [iOS](https://developer.apple.com/notifications/)
- [Windows](https://msdn.microsoft.com/library/windows/apps/hh779725.aspx)

## <a name="how-push-notifications-work"></a>Hoe werken pushmeldingen?

Pushmeldingen worden bezorgd via platformspecifieke infrastructuren, *PNS (Platform Notification System)* genoemd. Ze bieden essentiële pushfunctionaliteit voor het bezorgen van een bericht op een apparaat met een opgegeven ingang, en hebben geen algemene interface. Als u wilt een melding verzonden naar alle klanten in de Android, iOS en Windows-versies van een app, moet de ontwikkelaar werken met Apple Push Notification Service(APNS) en Firebase Cloud Messaging(FCM) Windows Notification Service(WNS) afzonderlijk.

Dit zijn de algemene stappen voor het verzenden van pushberichten:

1. Een toepassing besluit dat het bedrijf wil ontvangen meldingen, zodat deze PNS contactpersonen voor het doelplatform waar de app wordt uitgevoerd en aanvragen van een unieke en tijdelijke push-ingang. Het ingangstype is afhankelijk van het systeem (WNS gebruikt bijvoorbeeld URI's terwijl APNS worden tokens gebruikt).
2. De clientapp slaat deze ingang op in de back-end app of de provider.
3. Voor het verzenden van een pushmelding ontvangen, neemt de back-end contact op met de PNS met behulp van de ingang naar doel een specifieke client-app.
4. De PNS stuurt de melding door naar het apparaat dat is opgegeven door de ingang.

![Werkstroom voor pushmeldingen](./media/notification-hubs-overview/registration-diagram.png)

## <a name="the-challenges-of-push-notifications"></a>Aandachtspunten voor pushmeldingen

Platform Notification System zijn krachtig. App-ontwikkelaars moeten echter zelfs voor meer algemene scenario’s voor pushmeldingen nog heel wat werk verrichten om deze te implementeren, bijvoorbeeld voor het broadcasten van pushmeldingen naar gesegmenteerde gebruikers.

Pushmeldingen vereisen een complexe infrastructuur die geen verband houdt met de voornaamste bedrijfslogica van de toepassing. Enkele aandachtspunten voor de infrastructuur:

- **Platformafhankelijkheid**
  - De back-end is complex en moeilijk te onderhouden afhankelijk zijn van het platform logica om meldingen te verzenden naar apparaten op verschillende platforms zoals PNSes gebundeld niet vereist.
- **Schalen**
  - Volgens de PNS-richtlijnen moeten apparaattokens steeds worden vernieuwd als de app wordt gestart. De back-end omgaat met een grote hoeveelheid verkeer en database toegang alleen de tokens om up-to-date te houden. Wanneer het aantal apparaten naar honderden, duizenden of miljoenen groeit, is de kosten voor het maken en onderhouden van deze infrastructuur enorme.
  - De meeste Platform Notification Systems bieden geen ondersteuning voor broadcasten naar meerdere apparaten. Om één bericht te broadcasten naar een miljoen apparaten, zijn een miljoen aanroepen naar de Platform Notification Systems nodig. Het is zeer lastig om een dergelijke hoeveelheid verkeer met een minimale latentie te schalen.
- **Routering**
  - Hoewel Platform Notification Systems een manier bieden om berichten naar apparaten te verzenden, worden de meeste app-meldingen gericht op bepaalde gebruikers of belangengroepen. In de back-end moet een register worden bijgehouden om apparaten te koppelen aan belangengroepen, gebruikers, eigenschappen, enzovoort. Deze overhead verlengt de implementatietijd en verhoogt de onderhoudskosten van een app.

## <a name="why-use-azure-notification-hubs"></a>Waarom kiezen voor Azure Notification Hubs?

Notification Hubs wordt voorkomen dat alle complexiteit die zijn gekoppeld aan het pushen van meldingen op uw eigen vanuit uw app back-end. De uitgeschaalde infrastructuur ondersteunt meerdere platforms, waardoor er minder push-gerelateerde code nodig is en de back-end kan worden vereenvoudigd. Met Notification Hubs hoeven apparaten alleen maar hun PNS-ingangen te registreren bij een hub, terwijl de back-end berichten verstuurt naar gebruikers of belangengroepen, zoals wordt weergegeven in de volgende afbeelding:

![Schema van Notification Hubs](./media/notification-hubs-overview/notification-hub-diagram.png)

Notification Hubs is een gebruiksklare push-engine met de volgende voordelen:

- **Platformoverschrijdend**
  - Ondersteuning voor alle belangrijke push-platforms, waaronder iOS, Android, Windows, en Kindle en Baidu.
  - Een algemene interface om te pushen naar alle platforms in platformspecifieke of platformonafhankelijk indelingen zonder platformspecifieke taken.
  - Beheer van apparaatingang op één plaats.
- **Ondersteuning voor verschillende back-ends**
  - Cloud of on-premises
  - .NET, Node.js, Java, enzovoort.
- **Veel verschillende bezorgingsmethoden**
  - Verzend dit naar een of meerdere platforms: U kunt direct uitzenden naar miljoenen apparaten op platforms met één API-aanroep.
  - Push naar apparaat: U kunt meldingen naar individuele apparaten richten.
  - Push naar de gebruiker: Tags en sjablonen kunt u alle platformen apparaten van een gebruiker te bereiken.
  - Push naar segment met dynamische tags: Tags functie kunt u apparaten segment en push ze naar wens, of u naar een segment of een expressie van segmenten (bijvoorbeeld het actieve van levens en in Seattle niet voor de nieuwe gebruiker verzendt). U bent niet beperkt tot pub sub, maar kunt tags van apparaten altijd en overal bijwerken.
  - Gelokaliseerde pushmeldingen: De functie zorgt voor lokalisatie zonder gevolgen voor de back-endcode.
  - Op de achtergrond push: U kunt de push-pull-patroon inschakelen door op de achtergrond meldingen verzenden naar apparaten en activeren van deze om bepaalde-pulls of acties te voltooien.
  - Geplande push: U kunt plannen voor het verzenden van meldingen op elk gewenst moment.
  - Direct push: U kunt overslaan registreren van apparaten met de Notification Hubs-service en rechtstreeks batch pushen naar een lijst van het apparaat verwerkt.
  - Gepersonaliseerde pushmeldingen: Apparaat push variabelen helpt bij het verzenden van apparaat-specifieke gepersonaliseerde pushmeldingen verzenden met aangepaste sleutel / waarde-paren.
- **Uitgebreide telemetrie**
  - Er zijn via Azure Portal en programmacode telemetriegegevens beschikbaar voor pushen, apparaten, fouten en bewerkingen.
  - Via de functie voor het bijhouden van telemetrie per bericht kunt u elk pushbericht volgen vanaf de eerste aanvraag tot het moment dat de Notification Hubs-service de pushberichten via een batchverwerking verstuurt.
  - Met de functie Feedback van Platform Notification System wordt alle feedback van Platform Notification Systems gecommuniceerd om te helpen bij foutopsporing.
- **Schaalbaarheid**
  - U kunt snel berichten versturen naar miljoenen apparaten zonder dat u de architectuur hoeft aan te passen of sharding hoeft toe te passen.
- **Beveiliging**
  - Shared Access Secret (SAS) of federatieve verificatie.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met het maken en een notification hub gebruiken door de [zelfstudie: Pushmeldingen naar mobiele toepassingen](notification-hubs-android-push-notification-google-fcm-get-started.md).

[0]: ./media/notification-hubs-overview/registration-diagram.png
[1]: ./media/notification-hubs-overview/notification-hub-diagram.png

[How customers are using Notification Hubs]: https://azure.microsoft.com/services/notification-hubs
[Notification Hubs tutorials and guides]: https://azure.microsoft.com/documentation/services/notification-hubs
[iOS]: https://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
[Android]: https://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
[Windows Universal]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
[Windows Phone]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
[Kindle]: https://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
[Xamarin.iOS]: https://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
[Xamarin.Android]: https://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
[Microsoft.WindowsAzure.Messaging.NotificationHub]: https://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
[Microsoft.ServiceBus.Notifications]: https://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
[App Service Mobile Apps]: https://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop/
[templates]: notification-hubs-templates-cross-platform-push-messages.md
[Azure portal]: https://portal.azure.com
[tags]: (https://msdn.microsoft.com/library/azure/dn530749.aspx)
