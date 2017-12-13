---
title: 'Azure Notification Hubs: Frequently Asked Questions (FAQ''s) | Microsoft Docs'
description: Veelgestelde vragen over het ontwerpen/implementeren van oplossingen voor Notification Hubs
services: notification-hubs
documentationcenter: mobile
author: ysxu
manager: erikre
keywords: push-melding, pushmeldingen, pushmeldingen voor iOS, android pushmeldingen, push ios, android push
editor: 
ms.assetid: 7b385713-ef3b-4f01-8b1f-ffe3690bbd40
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 01/19/2017
ms.author: yuaxu
ms.openlocfilehash: d19a1b7c8d50ef0fde3cf65c9fd469bc34a27adc
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="push-notifications-with-azure-notification-hubs-frequently-asked-questions"></a>Pushmeldingen met Azure Notification Hubs: veelgestelde vragen
## <a name="general"></a>Algemeen
### <a name="what-is-the-resource-structure-of-notification-hubs"></a>Wat is de resource-structuur van Notification Hubs?

Azure Notification Hubs heeft twee niveaus van de resource: hubs en naamruimten. Een hub is een enkel push-resource die de platformoverschrijdende push-informatie van een app kan bevatten. Een naamruimte is een verzameling van hubs in één regio.

Aanbevolen toewijzing komt overeen met een naamruimte met een app. U kunt een productie-hub die geschikt is voor uw productie-app, een test-hub die geschikt is voor uw test-app, enzovoort hebben binnen een naamruimte.

### <a name="what-is-the-price-model-for-notification-hubs"></a>Wat is het prijsmodel voor Notification Hubs?
De meest recente prijsinformatie vindt u op de [Notification Hubs prijzen] pagina. Notification Hubs wordt gefactureerd op het niveau van de naamruimte. (Klik voor de definitie van een naamruimte, Zie 'Wat is de resource-structuur van Notification Hubs?') Notification Hubs biedt drie lagen:

* **Gratis**: deze laag is een goed uitgangspunt voor het verkennen van push-mogelijkheden. Dit wordt niet aanbevolen voor productie-apps. Registreren van 500 apparaten en 1 miljoen pushes per naamruimte per maand, opgenomen met geen garantie van service level agreement (SLA).
* **Basic**: deze laag (of de prijscategorie Standard) wordt aanbevolen voor kleinere productie-apps. Registreren van 200.000 apparaten en 10 miljoen pushes per naamruimte per maand als basislijn opgenomen. Quotum groei van de opties zijn opgenomen.
* **Standaard**: deze laag wordt aanbevolen voor middelgrote tot grote productie-apps. U krijgt 10 miljoen apparaten en 10 miljoen pushes per naamruimte per maand als basislijn opgenomen. Quotum verhogen opties en uitgebreide telemetrie mogelijkheden zijn opgenomen.

Standard-laag functies:
* **Uitgebreide telemetrie**: U kunt Notification Hubs Per bericht telemetrie gebruiken voor het bijhouden van een push-aanvragen en Platform Notification System Feedback voor foutopsporing.
* **Multitenancy**: U kunt werken met Platform Notification System referenties op het niveau van een naamruimte. Deze optie kunt u eenvoudig onderverdeeld tenants in hubs binnen dezelfde naamruimte.
* **Geplande push**: U kunt meldingen worden verzonden uit op elk gewenst moment plannen.

### <a name="what-is-the-notification-hubs-sla"></a>Wat is de SLA voor Notification Hubs?
Voor Basic en Standard Notification Hubs-lagen toepassingen correct is geconfigureerd om pushmeldingen te verzenden of registratie beheerbewerkingen uitvoeren minimaal 99,9 procent van de tijd. Voor meer informatie over de SLA, gaat u naar de [Notification Hubs SLA](https://azure.microsoft.com/support/legal/sla/notification-hubs/) pagina.

> [!NOTE]
> Omdat pushmeldingen is afhankelijk van derden Platform Notification System (zoals Apple APNS en Google FCM), is er geen SLA-garantie voor de levering van deze berichten. Nadat de batches Notification Hubs stuurt naar Platform Notification System (SLA gegarandeerd), is de verantwoordelijkheid van de Platform Notification System leveren de pushes (geen SLA gegarandeerd).

### <a name="which-customers-are-using-notification-hubs"></a>Welke klanten Notification Hubs gebruiken?
Veel klanten Notification Hubs gebruiken. Sommige opmerkelijke die worden hier weergegeven:

* Sochi 2014: Honderden belangengroepen, 3 miljoen apparaten en 150 miljoen meldingen verzonden in twee weken. [Casestudy: Sochi]
* Skanska: [casestudy: Skanska]
* Seattle tijden: [casestudy: Seattle keren]
* Mural.LY: [casestudy: Mural.ly]
* 7Digital: [casestudy: 7Digital]
* Bing-Apps: Tientallen miljoenen apparaten verzenden 3 miljoen meldingen per dag.

### <a name="how-do-i-upgrade-or-downgrade-my-hub-or-namespace-to-a-different-tier"></a>Hoe ik upgraden of mijn hub of -naamruimte met een andere tier downgraden?
Ga naar de  **[Azure-portal]** > **Notification Hubs naamruimten** of **Notification Hubs**. Selecteer de resource die u wilt bijwerken, en Ga naar **prijscategorie**. Houd rekening met de volgende vereisten:

* De bijgewerkte prijscategorie is van toepassing op *alle* hubs in de naamruimte waarmee u werkt.
* Als uw apparaat aantal de limiet van de laag die u overschrijdt naar downgraden bent, moet u apparaten verwijderen voordat u downgraden.


## <a name="design-and-development"></a>Ontwerpen en ontwikkelen
### <a name="which-server-side-platforms-do-you-support"></a>Welke platforms serverzijde ondersteund?
Server SDK's zijn beschikbaar voor .NET, Java, Node.js, PHP en Python. Notification Hubs-API's zijn gebaseerd op de REST-interfaces, zodat u rechtstreeks met de REST-API's werken kunt als u met verschillende platforms of niet dat extra afhankelijkheid wilt. Ga voor meer informatie naar de [Notification Hubs REST-API's] pagina.

### <a name="which-client-platforms-do-you-support"></a>Welke clientplatforms ondersteund?
Pushmeldingen worden ondersteund voor [iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [universele Windows-](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android China (via Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) en [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Chrome-Apps](notification-hubs-chrome-push-notifications-get-started.md), en [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari). Ga voor meer informatie naar de [Notification Hubs aan de slag zelfstudies] pagina.

### <a name="do-you-support-text-message-email-or-web-notifications"></a>Ondersteund SMS-bericht, e-mail of web meldingen?
Notification Hubs is bedoeld voor het verzenden van meldingen op mobiele apps. Biedt geen e-mailadres of tekst bericht mogelijkheden. Platforms van derden die deze mogelijkheden bieden kunnen echter worden geïntegreerd met Notification Hubs voor het verzenden van native pushmeldingen met behulp van [Mobile Apps].

Notification Hubs beschikt niet over een service in de browser push notification levering gebruiksklaar. Klanten kunnen deze functie SignalR boven op de ondersteunde platforms voor serverzijde met implementeren. Als u meldingen verzenden naar de browser-apps in de sandbox Chrome wilt, raadpleegt u de [Chrome-Apps zelfstudie].

### <a name="how-are-mobile-apps-and-azure-notification-hubs-related-and-when-do-i-use-them"></a>Hoe worden Mobile Apps en Azure Notification Hubs gerelateerd en wanneer kan ik ze gebruiken?
Als u een bestaande mobiele app voor back-end en u wilt toevoegen, alleen de mogelijkheid om pushmeldingen te verzenden, kunt u Azure Notification Hubs. Als u voor het instellen van uw mobiele app terug end helemaal wilt, kunt u overwegen de functie Mobile Apps van Azure App Service. Een mobiele app wordt automatisch een notification hub levert zodat u eenvoudig pushmeldingen vanuit de back-end voor mobiele app verzenden kunt. Prijzen voor Mobile Apps omvat de base kosten voor een notification hub. U betaalt alleen wanneer u de opgenomen pushes overschrijdt. Voor meer informatie over kosten, gaat u naar de [prijzen van App Service] pagina.

### <a name="how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>Hoeveel apparaten kan ik ondersteuning als het verzenden van pushmeldingen via Notification Hubs?
Raadpleeg de [Notification Hubs prijzen] voor informatie over het aantal ondersteunde apparaten.

Als u ondersteuning nodig hebt voor meer dan 10 miljoen geregistreerde apparaten, [contact met ons opnemen](https://azure.microsoft.com/overview/contact-us/) rechtstreeks en wij helpen u schalen van uw oplossing.

### <a name="how-many-push-notifications-can-i-send-out"></a>Hoeveel pushmeldingen kunnen ik verzenden?
Afhankelijk van de geselecteerde laag schaalt Azure Notification Hubs automatisch op basis van het aantal meldingen die door het systeem.

> [!NOTE]
> De algemene gebruikskosten kunt verhogen op basis van het aantal pushmeldingen worden geleverd. Zorg ervoor dat u rekening houden met de limieten van de laag die worden beschreven op de [Notification Hubs prijzen] pagina.
> 
> 

Onze klanten Notification Hubs gebruiken voor het dagelijks miljoenen pushmeldingen verzenden. U hebt geen speciale schalen van het bereik van uw pushmeldingen, zolang u Azure Notification Hubs niets te doen.

### <a name="how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>Hoe lang duurt het voor verzonden pushmeldingen te bereiken mijn apparaat?
In een scenario normaal gebruik, waar de inkomende belasting is consistent en zelfs, Azure Notification Hubs kunnen worden verwerkt ten minste *1 miljoen push-melding verzendt een minuut*. Deze snelheid kan variëren afhankelijk van het aantal tags, de aard van de binnenkomende verzendt en andere externe factoren.

Tijdens de geschatte leveringstijd de service berekent de doelen per platform en routes berichten voor de Push Notification Service (PNS) op basis van de geregistreerde tags of code-expressies. Het is de verantwoordelijkheid van de PNS om meldingen te verzenden naar het apparaat.

De PNS wordt niet gegarandeerd dat een SLA om meldingen te bezorgen. Echter, de meeste pushmeldingen worden geleverd aan doelapparaten binnen een paar minuten (meestal binnen 10 minuten) vanaf het moment dat ze worden verzonden naar Notification Hubs. Enkele meldingen mogelijk meer tijd vergen.

> [!NOTE]
> Azure Notification Hubs heeft een beleid voor het verwijderen van eventuele pushmeldingen die niet worden bezorgd bij de PNS binnen 30 minuten. Deze vertraging kan optreden voor een aantal oorzaken hebben, maar de meeste meestal omdat de PNS is beperking uw toepassing.
> 
> 

### <a name="is-there-any-latency-guarantee"></a>Is er garantie latentie?
Vanwege de aard van pushmeldingen (ze worden geleverd door een externe, platform-specifieke PNS), is er geen garantie latentie. Het merendeel van pushmeldingen worden gewoonlijk bezorgd binnen een paar minuten.

### <a name="what-do-i-need-to-consider-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>Wat moet ik rekening moet houden bij het ontwerpen van een oplossing met naamruimten en notification hubs?
#### <a name="mobile-appenvironment"></a>Mobiele app/omgeving
* Een notification hub per mam per omgeving gebruiken.
* In een multitenant-scenario, moet elke tenant een afzonderlijke hub hebben.
* Nooit delen de dezelfde notification hub voor productie en de testomgeving. Hierdoor kan problemen veroorzaken als u meldingen verzendt. (Apple biedt Sandbox en productie Push eindpunten, elk met afzonderlijke referenties).
* Standaard kunt u testmeldingen verzenden naar uw geregistreerde apparaten via de Azure-portal of het onderdeel voor Azure geïntegreerde in Visual Studio. De drempelwaarde is ingesteld op 10-apparaten die zijn willekeurig geselecteerd uit de groep registratie.

> [!NOTE]
> Als uw hub oorspronkelijk is geconfigureerd met een certificaat voor Apple sandbox en vervolgens opnieuw is geconfigureerd voor gebruik van een productiecertificaat van Apple, wordt de oorspronkelijke apparaattokens zijn ongeldig. Ongeldige tokens veroorzaken pushes mislukken. Uw productie- en testomgevingen afzonderlijke en gebruik van verschillende hubs voor verschillende omgevingen.
> 
> 

#### <a name="pns-credentials"></a>PNS-referenties
Als een mobiele app is geregistreerd met een platform-portal voor ontwikkelaars (bijvoorbeeld van Apple of Google), worden een app-id en security-tokens verzonden. De back-end van de app biedt deze tokens voor het platform PNS zodat pushmeldingen naar apparaten kunnen worden verzonden. Beveiligingstokens kunnen zich in de vorm van certificaten (bijvoorbeeld Apple iOS of Windows Phone) of beveiligingssleutels (bijvoorbeeld Google Android of Windows). Ze moeten worden geconfigureerd in notification hubs. Configuratie gewoonlijk wordt uitgevoerd op het niveau van de notification hub, maar kan ook worden uitgevoerd op het niveau van de naamruimte in een multitenant-scenario.

#### <a name="namespaces"></a>Naamruimten
Naamruimten kan worden gebruikt voor het groeperen van de implementatie. Ze kunnen ook worden gebruikt om weer te geven van alle notification hubs voor alle tenants van dezelfde app in een multitenant-scenario.

#### <a name="geo-distribution"></a>Geo-distributie
Geo-distributie is niet altijd kritieke in push notification-scenario's. Verschillende PNSes (bijvoorbeeld APNS of GCM) die het leveren van pushmeldingen aan apparaten zijn niet gelijkmatig verdeeld.

Als u een toepassing die globaal wordt gebruikt hebt, kunt u hubs in verschillende naamruimten maken met behulp van de Notification Hubs-service in verschillende Azure-regio's over de hele wereld.

> [!NOTE]
> Deze regeling wordt niet aanbevolen omdat de bijbehorende waarde uw beheerkosten, met name voor registraties stijgt. Worden moet gedaan alleen als er een expliciete nodig is.
> 
> 

### <a name="should-i-do-registrations-from-the-app-back-end-or-directly-through-client-devices"></a>Moet ik doen registraties van de back-end van de app of rechtstreeks via de client apparaten?
Registraties van de back-end van de app zijn nuttig wanneer u om clients te verifiëren voordat u de registratie. Ze zijn ook handig wanneer u de labels die moeten worden gemaakt of gewijzigd door de app back-end op basis van logica van de app hebt. Ga voor meer informatie naar de [back-end-registratie richtlijnen] en [richtlijnen voor back-end-registratie 2] pagina's.

### <a name="what-is-the-push-notification-delivery-security-model"></a>Wat is het beveiligingsmodel push notification levering?
Azure Notification Hubs maakt gebruik van een [shared access signature voor](../storage/common/storage-dotnet-shared-access-signature-part-1.md)-gebaseerd beveiligingsmodel. U kunt de shared access signature tokens op het hoofdniveau van de naamruimte of op het niveau van gedetailleerde notification hub gebruiken. Shared access signature voor tokens kunnen volgen andere autorisatieregels, bijvoorbeeld, de machtigingen van berichten verzenden of om te luisteren naar melding machtigingen worden ingesteld. Zie voor meer informatie de [Notification Hubs beveiligingsmodel] document.

### <a name="how-should-i-handle-sensitive-payload-in-push-notifications"></a>Hoe moet ik gevoelige nettolading in pushmeldingen verwerken?
Alle meldingen worden geleverd aan doelapparaten door het platform PNS. Wanneer een melding wordt verzonden naar Azure Notification Hubs, wordt deze verwerkt en doorgegeven aan de respectieve PNS.

Alle verbindingen van de afzender op de Azure Notification Hubs naar de PNS HTTPS gebruiken.

> [!NOTE]
> De nettolading van berichten wordt niet door Azure Notification Hubs op een manier worden geregistreerd.
> 
> 

Voor het verzenden van gevoelige nettoladingen, wordt u aangeraden een patroon Secure Push. De afzender levert een ping-bericht met een bericht-id aan het apparaat zonder de gevoelige nettolading. Wanneer de app op het apparaat de nettolading ontvangt, roept de app een beveiligde API rechtstreeks om de details van bericht ophalen. Voor richtlijnen voor het implementeren van dit patroon, gaat u naar de [Notification Hubs Secure Push zelfstudie] pagina.

## <a name="operations"></a>Bewerkingen
### <a name="what-support-is-provided-for-disaster-recovery"></a>Wat wordt ondersteund voor herstel na noodgevallen?
We bieden metagegevens disaster recovery dekking aan onze kant (de naam van de Notification Hubs, de verbindingsreeks en andere essentiële informatie). Wanneer een noodherstelscenario wordt geactiveerd, registratiegegevens is de *alleen segmenteren* van de Notification Hubs-infrastructuur die verloren gaat. U moet een oplossing voor deze gegevens in uw nieuwe hub na het herstel opnieuw implementeren:

1. Maak een secundaire Notification hubs in een ander datacenter. We raden aan met het maken van een vanaf het begin af te schermen u uit een disaster recovery-gebeurtenis die invloed kan zijn op de beheermogelijkheden. U kunt ook een op het moment van het herstel na noodgevallen gebeurtenis maken.

2. Vullen van de secundaire notification hub met de registraties van uw primaire notification hub. Niet aanbevolen bij het registraties op beide hubs onderhouden en deze synchroon houden terwijl de registraties. Deze procedure werkt niet goed vanwege de intrinsieke neiging van registraties verloopt de PNS-zijde. Notification Hubs ruimt ze ontvangen PNS feedback over registraties verlopen of ongeldig.  

Wij hebben twee aanbevelingen voor back-ends van app:

* Gebruik een app back-end die wordt onderhouden door een bepaalde set registraties aan het einde. Vervolgens kan een bulksgewijs invoegen in de secundaire notification hub uitvoeren.

* Gebruik een app back-end die een reguliere dump van registraties opgehaald uit de primaire notification hub als een back-up. Vervolgens kan een bulksgewijs invoegen in de secundaire notification hub uitvoeren.

> [!NOTE]
> Registraties exporteren/importeren functionaliteit die beschikbaar zijn in de prijscategorie Standard wordt beschreven in de [registraties exporteren/importeren] document.
> 
> 

Als u geen back-end wanneer de app wordt gestart op doelapparaten, uitvoeren een nieuwe registratie ze in de secundaire notification hub. De secundaire notification hub heeft uiteindelijk de actieve apparaten geregistreerd.

Er is een periode apparaten met niet-geopende apps wanneer geen meldingen ontvangt.

### <a name="is-there-audit-log-capability"></a>Is er audit log mogelijkheid?
Alle beheerbewerkingen voor Notification Hubs gaat u naar bewerkingslogboeken, die beschikbaar worden gesteld in de [Azure-portal].

## <a name="monitoring-and-troubleshooting"></a>Bewaking en probleemoplossing
### <a name="what-troubleshooting-capabilities-are-available"></a>Welke mogelijkheden voor probleemoplossing zijn beschikbaar?
Azure Notification Hubs biedt verschillende functies voor het oplossen van problemen met name voor de meest voorkomende scenario van verwijderde meldingen. Zie voor meer informatie de [Notification Hubs probleemoplossing] witboek.

### <a name="what-telemetry-features-are-available"></a>Welke telemetrie-functies zijn beschikbaar?
Azure Notification Hubs kunt bekijken van telemetriegegevens in de [Azure-portal]. Details van de metrische gegevens zijn beschikbaar op de [Notification Hubs metrische gegevens] pagina.

> [!NOTE]
> Geslaagde meldingen betekenen gewoon zijn aan de externe PNS (bijvoorbeeld APNS voor Apple) of GCM voor Google pushmeldingen geleverd. Het is de verantwoordelijkheid van de PNS om te leveren de meldingen aan doelapparaten. De PNS maakt normaal gesproken niet beschikbaar voor levering metrische gegevens aan derden.  
> 
> 

We bieden ook de mogelijkheid de telemetriegegevens programmatisch (in de prijscategorie Standard) exporteren. Zie voor meer informatie de [Notification Hubs metrische gegevens voorbeeld].

[Azure-portal]: https://portal.azure.com
[Notification Hubs prijzen]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Notification Hubs SLA]: http://azure.microsoft.com/support/legal/sla/
[Casestudy: Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[Casestudy: Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[Casestudy: Seattle keren]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[Casestudy: Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[Casestudy: 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[Notification Hubs REST-API's]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[Notification Hubs aan de slag zelfstudies]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Chrome-Apps zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[back-end-registratie richtlijnen]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[richtlijnen voor back-end-registratie 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[Notification Hubs beveiligingsmodel]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[Notification Hubs Secure Push zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[Notification Hubs probleemoplossing]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[Notification Hubs metrische gegevens]: https://msdn.microsoft.com/library/dn458822.aspx
[Notification Hubs metrische gegevens voorbeeld]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[registraties exporteren/importeren]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure Portal]: https://portal.azure.com
[complete samples]: https://github.com/Azure/azure-notificationhubs-samples
[Mobile Apps]: https://azure.microsoft.com/services/app-service/mobile/
[prijzen van App Service]: https://azure.microsoft.com/pricing/details/app-service/
