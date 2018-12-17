---
title: 'Azure Notification Hubs: Frequently Asked Questions (Veelgestelde vragen) | Microsoft Docs'
description: Veelgestelde vragen over het ontwerpen/implementeren van oplossingen van Notification Hubs
services: notification-hubs
documentationcenter: mobile
author: dimazaid
manager: kpiteira
editor: spelluru
keywords: pushmelding, pushmeldingen, iOS-pushmeldingen, pushmeldingen voor android, ios-push, android-push
ms.assetid: 7b385713-ef3b-4f01-8b1f-ffe3690bbd40
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 10/14/2018
ms.author: dimazaid
ms.openlocfilehash: 140994de4b1be61f16593e450d156b81727a9f52
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53437830"
---
# <a name="push-notifications-with-azure-notification-hubs-frequently-asked-questions"></a>Pushmeldingen verzenden met Azure Notification Hubs: Veelgestelde vragen
## <a name="general"></a>Algemeen
### <a name="what-is-the-resource-structure-of-notification-hubs"></a>Wat is de resource-structuur van Notification Hubs?

Azure Notification Hubs heeft twee niveaus van de resource: hubs en naamruimten. Een hub is een één push-resource die de platformonafhankelijke push-gegevens van één app kan bevatten. Een naamruimte is een verzameling van hubs in één regio.

Aanbevolen toewijzing komt overeen met een naamruimte met een app. Binnen een naamruimte hebt u een productie-hub die geschikt is voor uw app in productie, een test-hub die geschikt is voor uw test-app, enzovoort.

### <a name="what-is-the-price-model-for-notification-hubs"></a>Wat is het prijsmodel voor Notification Hubs?
De meest recente prijsinformatie vindt u op de [prijzen van Notification Hubs] pagina. Notification Hubs wordt in rekening gebracht op het niveau van de naamruimte. (Klik voor de definitie van een naamruimte, Zie "Wat is de resource-structuur van Notification Hubs?") Notification Hubs biedt drie lagen:

* **Gratis**: Deze laag is een goed uitgangspunt voor het verkennen van pushmogelijkheden. Het wordt niet aanbevolen voor productie-apps. U krijgt 500 apparaten verbinden en 1 miljoen pushes inbegrepen per naamruimte per maand, met geen garantie van service level agreement (SLA).
* **Basic**: Deze laag (of de Standard-laag) wordt aanbevolen voor kleinere productie-apps. U krijgt 200.000 apparaten en 10 miljoen pushes inbegrepen per naamruimte per maand als een basislijn. Groei quotumopties worden opgenomen.
* **Standard**: Deze laag wordt aanbevolen voor middelgrote tot grote productie-apps. U krijgt 10 miljoen apparaten en 10 miljoen pushes inbegrepen per naamruimte per maand als een basislijn. Quotum toename van de opties en uitgebreide telemetrie mogelijkheden zijn opgenomen.

Standard-functies:
* **Uitgebreide telemetrie**: U kunt Notification Hubs telemetrie Per bericht gebruiken voor het bijhouden van een push-aanvragen en Feedback van Platform Notification System voor foutopsporing.
* **Multitenancy**: Op het niveau van een naamruimte kunt u werken met de referenties van Platform Notification System. Deze optie kunt u eenvoudig onderverdeeld tenants in hubs binnen dezelfde naamruimte.
* **Geplande push**: U kunt meldingen op elk gewenst moment worden verzonden plannen.

### <a name="what-is-the-notification-hubs-sla"></a>Wat is de SLA van Notification Hubs?
Voor de lagen Basic en Standard Notification Hubs kunnen correct geconfigureerde toepassingen pushmeldingen te verzenden of registratiebeheerbewerkingen voeren ten minste 99,9 procent van de tijd. Voor meer informatie over de SLA, gaat u naar de [Notification Hubs SLA](https://azure.microsoft.com/support/legal/sla/notification-hubs/) pagina.

> [!NOTE]
> Omdat pushmeldingen te verzenden, is afhankelijk van derden Platform Notification Systems (zoals Apple APNS en Google FCM), is er geen SLA-garantie voor de levering van deze berichten. Nadat de batches Notification Hubs stuurt naar Platform Notification Systems (SLA gegarandeerd), is de verantwoordelijkheid van de Platform Notification Systems leveren de pushes (er wordt geen SLA gegarandeerd).

### <a name="which-customers-are-using-notification-hubs"></a>Welke klanten Notification Hubs gebruiken?
Veel klanten Notification Hubs gebruiken. Sommige die aandacht vereist die worden hier vermeld:

* Sotsji 2014: Honderden belangengroepen, 3 miljoen apparaten en 150 miljoen meldingen verzonden binnen twee weken. [Casestudy: Olympische]
* Skanska: [Casestudy: Skanska]
* Seattle tijden: [Casestudy: Seattle tijden]
* Mural.LY: [Casestudy: Mural.LY]
* 7Digital: [Casestudy: 7Digital]
* Bing-Apps: Tientallen miljoenen apparaten verzenden 3 miljoen berichten per dag.

### <a name="how-do-i-upgrade-or-downgrade-my-hub-or-namespace-to-a-different-tier"></a>Hoe ik upgraden en downgraden van mijn hub of de naamruimte naar een andere laag?
Ga naar de  **[Azure Portal]** > **Notification Hubs-naamruimten** of **Notification Hubs**. Selecteer de resource die u wilt bijwerken, en Ga naar **prijscategorie**. Houd rekening met de volgende vereisten:

* De bijgewerkte prijscategorie is van toepassing op *alle* hubs in de naamruimte die u samenwerkt.
* Als het aantal apparaten de limiet van de laag die u overschrijdt naar downgraden wilt, moet u de apparaten verwijderen voordat u downgraden.


## <a name="design-and-development"></a>Ontwerpen en ontwikkelen
### <a name="which-server-side-platforms-do-you-support"></a>Welke server-side-platforms ondersteund?
Server SDK's zijn beschikbaar voor .NET, Java, Node.js, PHP en Python. Notification Hubs-API's zijn gebaseerd op REST-interfaces, zodat u rechtstreeks met de REST-API's werken kunt als u van verschillende platforms gebruikmaakt of niet dat extra afhankelijkheid wilt. Voor meer informatie gaat u naar de [Notification Hubs REST-API 's] pagina.

### <a name="which-client-platforms-do-you-support"></a>Welke clientplatforms ondersteund?
Pushmeldingen worden ondersteund voor [iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android China (via de Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) en [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Chrome-Apps](notification-hubs-chrome-push-notifications-get-started.md), en [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari). Voor meer informatie gaat u naar de [Notification Hubs aan de slag-zelfstudies] pagina.

### <a name="do-you-support-text-message-email-or-web-notifications"></a>Ondersteund SMS-bericht, e-mail of web meldingen?
Notification Hubs is met name bedoeld voor het verzenden van meldingen voor mobiele apps. Biedt geen e-mailadres of tekst zijn. Echter platforms van derden die deze mogelijkheden kunnen worden geïntegreerd met Notification Hubs voor het verzenden van native pushmeldingen met behulp van [Mobile Apps].

Notification Hubs beschikt niet over een in de browser push notification service voor de levering buiten het vak. Klanten kunnen deze functie met SignalR boven op de ondersteunde platforms voor serverzijde implementeren. Als u wilt voor het verzenden van meldingen in de sandbox Chrome-browser-Apps, Zie de [Zelfstudie voor chrome-Apps].

### <a name="how-are-mobile-apps-and-azure-notification-hubs-related-and-when-do-i-use-them"></a>Hoe worden Mobile Apps en Azure Notification Hubs die betrekking hebben en wanneer gebruik ik deze?
Hebt u een bestaande mobiele app voor back-end en u wilt toevoegen van alleen de mogelijkheid om pushmeldingen te verzenden, kunt u Azure Notification Hubs. Als u instellen van uw mobiele app back end helemaal wilt, kunt u overwegen de functie Mobile Apps van Azure App Service. Een mobiele app wordt automatisch een notification hub inricht, zodat u eenvoudig pushmeldingen vanuit de back-end voor mobiele app verzenden kunt. Prijzen voor Mobile Apps bevat de basiskosten in rekening gebracht voor een notification hub. U betaalt alleen wanneer u de opgenomen pushes overschrijdt. Voor meer informatie over kosten, gaat u naar de [App Service-prijzen] pagina.

### <a name="how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>Het aantal apparaten kan ik ondersteuning als ik pushmeldingen via Notification Hubs verzenden?
Raadpleeg de [prijzen van Notification Hubs] pagina voor meer informatie over het aantal ondersteunde apparaten.

Als u ondersteuning nodig voor meer dan 10 miljoen geregistreerde apparaten hebt, [contact met ons opnemen](https://azure.microsoft.com/overview/contact-us/) rechtstreeks en we helpen u uw oplossing schalen.

### <a name="how-many-push-notifications-can-i-send-out"></a>Hoeveel pushmeldingen kunnen verzenden ik?
Afhankelijk van de geselecteerde laag Azure Notification Hubs automatisch omhoog wordt geschaald uitgaande op basis van het aantal meldingen die worden gebruikt met het systeem.

> [!NOTE]
> De gebruikskosten voor het algehele kunt verhogen op basis van het aantal pushmeldingen die worden geleverd. Zorg ervoor dat u rekening houden met de limieten van de laag die worden beschreven op de [prijzen van Notification Hubs] pagina.
> 
> 

Onze klanten Notification Hubs gebruiken voor het verzenden van miljoenen pushmeldingen dagelijks. U hoeft niet te doen niets om te schalen van het bereik van uw pushmeldingen te verzenden als u Azure Notification Hubs.

### <a name="how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>Hoe lang het duurt voor verzonden pushmeldingen naar het bereiken van mijn apparaat?
In een scenario voor normaal gebruik, waarbij de inkomende belasting consistent is en zelfs, Azure Notification Hubs ten minste kan verwerken *1 miljoen pushmeldingen verzendt een minuut*. Deze snelheid kan afwijken afhankelijk van het aantal tags, de aard van de binnenkomende verzendt, en andere externe factoren.

Tijdens de geschatte levertijd, de service berekent de doelen per platform en routes berichten naar de Push Notification Service (PNS) op basis van de geregistreerde tags of code-expressies. Het is de verantwoordelijkheid van de PNS om meldingen te verzenden naar het apparaat.

De PNS garandeert geen SLA voor de levering van meldingen. Echter, de meeste pushmeldingen worden geleverd aan doelapparaten binnen een paar minuten (doorgaans binnen 10 minuten) vanaf het moment dat ze worden verzonden naar Notification Hubs. Enkele meldingen kunnen langer duren.

> [!NOTE]
> Azure Notification Hubs is een beleid voor het verwijderen van alle pushmeldingen die niet worden bezorgd bij de PNS binnen 30 minuten. Deze vertraging kan optreden voor een aantal oorzaken hebben, maar de meeste meestal omdat de PNS is beperking van uw toepassing.
> 
> 

### <a name="is-there-any-latency-guarantee"></a>Is er een garantie latentie?
Vanwege de aard van pushmeldingen te verzenden (ze worden geleverd door een externe, platform-specifieke PNS), is er geen garantie latentie. Normaal gesproken de meerderheid van de push-meldingen worden bezorgd binnen een paar minuten.

### <a name="what-do-i-need-to-consider-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>Wat heb ik nodig om te overwegen bij het ontwerpen van een oplossing met naamruimten en notification hubs?
#### <a name="mobile-appenvironment"></a>Mobiele app /-omgeving
* Een notification hub per mobiele app, per omgeving gebruiken.
* In een multitenant-scenario, moet elke tenant een afzonderlijke hub hebben.
* Nooit delen de dezelfde notification hub voor productie- en testomgevingen. Met deze procedure kan problemen veroorzaken bij het verzenden van meldingen. (Apple biedt Sandbox en Push-productie-eindpunten, elk met afzonderlijke referenties).
* Standaard kunt u testmeldingen verzenden naar uw geregistreerde apparaten via de Azure portal of de Azure geïntegreerd onderdeel in Visual Studio. De drempelwaarde wordt ingesteld op 10-apparaten die worden willekeurig geselecteerd uit de registratie van toepassingen.

> [!NOTE]
> Als uw hub oorspronkelijk is geconfigureerd met een Apple-certificaat voor sandbox en vervolgens opnieuw is geconfigureerd voor het gebruik van een certificaat voor Apple-productie, wordt de oorspronkelijke apparaattokens zijn ongeldig. Ongeldige tokens ertoe leiden dat pushes mislukken. Uw productie- en testomgevingen te scheiden en gebruik van verschillende hubs voor verschillende omgevingen.
> 
> 

#### <a name="pns-credentials"></a>PNS-referenties
Wanneer een mobiele app is geregistreerd bij het ontwikkelaarsportal van een platform (bijvoorbeeld Apple of Google), worden een app-id en -beveiliging-tokens verzonden. De back-end van de app biedt deze tokens aan van het platform PNS zodat pushmeldingen te verzenden naar apparaten kunnen worden verzonden. Beveiligingstokens kunnen zich in de vorm van certificaten (bijvoorbeeld, Apple iOS of Windows Phone) of beveiligingssleutels (bijvoorbeeld Google Android of Windows). Ze moeten worden geconfigureerd in notification hubs. Configuratie wordt doorgaans uitgevoerd op het niveau van de notification hub, maar kan ook worden uitgevoerd op het niveau van de naamruimte in een scenario voor meerdere tenants.

#### <a name="namespaces"></a>Naamruimten
Naamruimten kan worden gebruikt voor het groeperen van de implementatie. Ze kunnen ook worden gebruikt voor alle meldingshubs voor alle tenants van dezelfde app in een scenario voor meerdere tenants.

#### <a name="geo-distribution"></a>Geo-distributie
Geo-distributie is niet altijd kritieke in scenario's voor pushmeldingen. Verschillende PNSes (bijvoorbeeld APNS of GCM) die het leveren van pushmeldingen naar apparaten worden niet gelijkmatig verdeeld.

Hebt u een toepassing die wereldwijd wordt gebruikt, kunt u hubs in verschillende naamruimten maken met behulp van de Notification Hubs-service in verschillende Azure-regio's over de hele wereld.

> [!NOTE]
> Deze benadering wordt niet aanbevolen omdat de beheerkosten, met name voor registraties toeneemt. Het moet worden uitgevoerd alleen als er een expliciete behoefte is.
> 
> 

### <a name="should-i-do-registrations-from-the-app-back-end-or-directly-through-client-devices"></a>Moet ik doen registraties van de back-end van de app of rechtstreeks via client apparaten?
Registraties vanaf back-end van de app zijn nuttig wanneer u hebt om clients te verifiëren voordat u de registratie. Ze zijn ook handig wanneer u tags die moeten worden gemaakt of gewijzigd door de app back-end op basis van app-logica. Voor meer informatie gaat u naar de [Registratie van de back-end-richtlijnen] en [richtlijnen voor back-end-registratie 2] pagina's.

### <a name="what-is-the-push-notification-delivery-security-model"></a>Wat is het beveiligingsmodel voor levering van push notification?
Azure Notification Hubs maakt gebruik van een [handtekening voor gedeelde toegang](../storage/common/storage-dotnet-shared-access-signature-part-1.md)-gebaseerd beveiligingsmodel. U kunt de SAS-tokens gebruiken op het niveau van de naamruimte root of op het niveau van de gedetailleerde notification hub. Shared access signature-tokens kunnen worden ingesteld om te voldoen aan de regels van verschillende autorisatie, bijvoorbeeld, voor het verzenden van de machtigingen voor berichten of om te luisteren naar melding machtigingen voor. Zie voor meer informatie de [Notification Hubs-beveiligingsmodel] document.

### <a name="how-should-i-handle-sensitive-payload-in-push-notifications"></a>Hoe moet ik omgaan met gevoelige nettolading bij pushmeldingen?
Alle meldingen worden geleverd aan doelapparaten door de PNS van het platform. Wanneer een melding wordt verzonden naar Azure Notification Hubs, verwerkt en doorgegeven aan de respectieve PNS.

Alle verbindingen van de afzender met de Azure Notification Hubs om de PNS gebruik van HTTPS.

> [!NOTE]
> Azure Notification Hubs wordt niet geregistreerd voor de nettolading van berichten op geen enkele manier.
> 
> 

Voor het verzenden van gevoelige nettoladingen, wordt u aangeraden een beveiligde Push-patroon. De afzender levert een ping-melding met een bericht-id aan het apparaat zonder de nettolading van de gevoelige. Wanneer de app op het apparaat de payload ontvangt, roept de app een beveiligde API om rechtstreeks op te halen van de berichtgegevens. Voor richtlijnen voor hoe u dit patroon wilt implementeren, gaat u naar de [Notification Hubs beveiligde Push-zelfstudie] pagina.

## <a name="operations"></a>Bewerkingen
### <a name="what-support-is-provided-for-disaster-recovery"></a>Welke ondersteuning is bedoeld voor herstel na noodgevallen?
We bieden metagegevens disaster recovery dekking aan onze kant (de naam van de Notification Hubs, de verbindingsreeks en andere essentiële informatie). Wanneer een noodherstelscenario wordt geactiveerd, registratiegegevens is de *alleen segment* van de infrastructuur voor Notification Hubs die verloren gaat. U moet voor het implementeren van een oplossing voor het opnieuw bevolken deze gegevens in uw nieuwe hub na herstel:

1. Maak een secundaire Notification hubs in een ander datacenter. Het is raadzaam om het maken van een vanaf het begin af te schermen u van een disaster recovery-gebeurtenis die van invloed zijn op de beheermogelijkheden. U kunt ook een op het moment van de disaster recovery-gebeurtenis maken.

2. Vul de secundaire notification hub met de registraties van uw primaire notification hub. Wordt niet aanbevolen wilt registraties op beide hubs onderhouden en houd ze synchroon is net als registraties zijn. Met deze procedure werkt niet goed vanwege de inherente neiging van registraties verloopt op de PNS-zijde. Notification Hubs schoont ze ontvangen van feedback van PNS over verlopen of ongeldig registraties.  

We hebben twee aanbevelingen voor back-ends Apps:

* Gebruik een app back-end die wordt onderhouden door een bepaalde set registraties aan het einde. Vervolgens kan een bulkinvoeging uitvoeren in de secundaire notification hub.

* Gebruik een app back-end die een reguliere dump van registraties opgehaald uit de primaire notification hub als een back-up. Vervolgens kan een bulkinvoeging uitvoeren in de secundaire notification hub.

> [!NOTE]
> Registraties exporteren/importeren functionaliteit beschikbaar is in de Standard-laag wordt beschreven in de [registraties exporteren/importeren] document.
> 
> 

Als u geen een back-end, wanneer de app wordt gestart op doelapparaten, uitvoeren een nieuwe registratie ze in de secundaire notification hub. De secundaire notification hub is uiteindelijk alle actieve apparaten die zijn geregistreerd.

Er is een bepaalde periode apparaten met niet-geopende apps wanneer geen meldingen ontvangt.

### <a name="is-there-audit-log-capability"></a>Is er audit log mogelijkheid?
Alle beheerbewerkingen van Notification Hubs gaat u naar Logboeken voor bewerkingen die worden weergegeven in de [Azure Portal].

## <a name="monitoring-and-troubleshooting"></a>Bewaking en probleemoplossing
### <a name="what-troubleshooting-capabilities-are-available"></a>Welke mogelijkheden voor probleemoplossing zijn beschikbaar?
Azure Notification Hubs biedt verschillende functies voor het oplossen van problemen, met name voor de meest voorkomende scenario van verloren meldingen. Zie voor meer informatie, de [Notification Hubs oplossen van problemen] technisch document.

### <a name="what-telemetry-features-are-available"></a>Welke telemetriefuncties zijn beschikbaar?
Met Azure Notification Hubs kunnen weergeven van telemetriegegevens in de [Azure Portal]. Details van de metrische gegevens zijn beschikbaar op de [Notification Hubs metrische gegevens] pagina.

> [!NOTE]
> Meldingen over voltooide betekent alleen dat pushmeldingen zijn geleverd op de externe PNS (bijvoorbeeld, APNS van Apple) of GCM van Google. Het is de verantwoordelijkheid van de PNS om te leveren van de meldingen aan doelapparaten. De PNS maakt normaal gesproken niet beschikbaar voor levering van metrische gegevens aan derden.  
> 
> 

We bieden ook de mogelijkheid voor het exporteren van de telemetrische gegevens via een programma (in de Standard-laag). Zie voor meer informatie, de [Voorbeeld van Notification Hubs metrische gegevens].

[Azure Portal]: https://portal.azure.com
[Prijzen van Notification Hubs]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Notification Hubs SLA]: http://azure.microsoft.com/support/legal/sla/
[Casestudy: Olympische]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[Casestudy: Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[Casestudy: Seattle tijden]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[Casestudy: Mural.LY]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[Casestudy: 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[Notification Hubs REST-API 's]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[Notification Hubs aan de slag-zelfstudies]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Zelfstudie voor chrome-Apps]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[Registratie van de back-end-richtlijnen]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Richtlijnen voor back-end-registratie 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[Notification Hubs-beveiligingsmodel]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[Notification Hubs beveiligde Push-zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[Notification Hubs oplossen van problemen]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[Notification Hubs metrische gegevens]: ../azure-monitor/platform/metrics-supported.md#microsoftnotificationhubsnamespacesnotificationhubs
[Voorbeeld van Notification Hubs metrische gegevens]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[Registraties exporteren/importeren]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure Portal]: https://portal.azure.com
[complete samples]: https://github.com/Azure/azure-notificationhubs-samples
[Mobile Apps]: https://azure.microsoft.com/services/app-service/mobile/
[App Service-prijzen]: https://azure.microsoft.com/pricing/details/app-service/
