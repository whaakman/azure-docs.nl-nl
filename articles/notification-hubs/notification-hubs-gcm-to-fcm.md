---
title: Azure Notification Hubs en de migratie van Google Firebase Cloud Messaging (FCM)
description: Hierin wordt beschreven hoe Azure Notification Hubs de Google GCM voor de migratie van het FCM-adressen.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 04/10/2019
ms.author: jowargo
ms.openlocfilehash: 4cbfc67bc66e84b4743f3326db40872241e5d474
ms.sourcegitcommit: f24b62e352e0512dfa2897362021b42e0cb9549d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59506400"
---
# <a name="azure-notification-hubs-and-the-google-firebase-cloud-messaging-fcm-migration"></a>Azure Notification Hubs en de migratie van Google Firebase Cloud Messaging (FCM)

## <a name="current-state"></a>Huidige status

Google aangekondigd wanneer de migratie van Google Cloud Messaging (GCM) naar Firebase Cloud Messaging (FCM), pushservices zoals die van ons om aan te passen hoe er meldingen verzonden naar de Android-apparaten om de wijziging mogelijk te maken had.

Er bijgewerkt van de back-end van onze service en updates gepubliceerd op onze API en SDK's, indien nodig. Met onze-implementatie, hebben we besloten voor compatibiliteit met bestaande GCM notification-schema's om te beperken de gevolgen zijn voor klanten. Dit betekent dat we momenteel meldingen verzenden naar Android-apparaten met FCM in FCM Legacy-modus. Uiteindelijk gaan we de waarde true is ondersteuning toegevoegd voor FCM, met inbegrip van de nieuwe functies en indeling nettolading. Dat is een langere wijziging en de huidige migratie is gericht op het handhaven van compatibiliteit met bestaande toepassingen en SDK's. U kunt de GCM- of FCM-SDK's gebruiken in uw app (samen met onze SDK) en zorgen wij ervoor dat de melding correct is verzonden.

Sommige klanten heeft onlangs een e-mailbericht ontvangen van Google-waarschuwing over apps met behulp van een GCM-eindpunt voor meldingen. Dit is slechts een waarschuwing, en er niets is verbroken: Android-meldingen van uw app nog steeds naar Google worden verzonden voor verwerking en Google nog steeds verwerkt. Sommige klanten die het GCM-eindpunt expliciet in de serviceconfiguratie van de opgegeven zijn nog steeds met behulp van het eindpunt van de afgeschafte. We dit gat al heeft geïdentificeerd en gewerkt aan het probleem is opgelost als Google het e-mailbericht verzonden.

We dat afgeschaft eindpunt vervangen en de oplossing wordt geïmplementeerd.

## <a name="going-forward"></a>Voortaan

Veelgestelde vragen over het FCM van Google verplicht u niet verder niets te doen. In de [FCM Veelgestelde vragen over](https://developers.google.com/cloud-messaging/faq), Google gezegd "client-SDK's en GCM tokens blijven werken voor onbepaalde tijd. Echter, kunt u zich niet aan de nieuwste versie van Google Play Services in uw Android-app als doel, tenzij u naar FCM migreert."

Als uw app gebruikmaakt van de GCM-bibliotheek, gaat u verder en volg de instructies van Google om te upgraden naar de FCM-bibliotheek in uw app. Onze SDK is compatibel met, dus u hoeft te bijwerken van alles in uw app aan onze kant (zolang u hoogte met onze SDK-versie bent).

## <a name="questions-and-answers"></a>Vragen en antwoorden

Hier ziet u enkele antwoorden op veelgestelde vragen die we horen vaak van klanten:

**V:** Wat moet ik doen om compatibel zijn met de afsluitdatum (Google de huidige afsluitdatum 29 mei is en kan worden gewijzigd)?

**A:** Er is niets. Er wordt compatibiliteit met bestaande schema's voor GCM-melding. Uw GCM-sleutel blijven werken normaal net als elke GCM-SDK's en bibliotheken die worden gebruikt door uw toepassing.

Als/wanneer u besluit om te upgraden naar de FCM-SDK's en bibliotheken om te profiteren van nieuwe functies, uw GCM-sleutel wordt nog steeds werken. U kunt overstappen naar een FCM-sleutel als u wilt, maar zorg ervoor dat u toevoegt Firebase aan uw bestaande GCM-project dat bij het maken van het nieuwe Firebase-project. Dit zorgt ervoor dat voor neerwaartse compatibiliteit met uw klanten met oudere versies van de app die nog steeds gebruikmaken van GCM-SDK's en bibliotheken.

Als u een nieuw FCM-project maken en niet koppelen aan het bestaande GCM-project, zodra u Notification Hubs met het nieuwe FCM-geheim bijwerken verliest u de mogelijkheid om pushmeldingen te verzenden naar uw huidige app-installaties, omdat de nieuwe FCM-sleutel geen koppeling naar de oude GCM heeft het project.

**V:** Waarom krijg ik dit e-mailbericht over oude GCM-eindpunten wordt gebruikt? Wat heb ik doen?

**A:** Er is niets. We hebben is migreren naar de nieuwe eindpunten en is binnenkort klaar, zodat er geen wijziging noodzakelijk is. Niets wordt verbroken, onze één gemiste eindpunt veroorzaakt gewoon waarschuwingsberichten van Google.

**V:** Hoe kan ik overstappen op de nieuwe FCM-SDK's en bibliotheken zonder dat bestaande gebruikers te?

A: Op elk gewenst moment bijwerken. Google heeft nog niet aangekondigd voor elke afschaffing van bestaande GCM-SDK's en bibliotheken. Om te controleren of u pushmeldingen kunt verzenden naar uw bestaande gebruikers niet verbreken, zorg ervoor dat wanneer u de nieuwe Firebase-project dat u aan uw bestaande GCM-project koppelen wilt maken. Dit zorgt ervoor dat nieuwe Firebase geheimen werkt voor gebruikers met de oudere versies van uw app met GCM-SDK's en bibliotheken, evenals nieuwe gebruikers van uw app met FCM-SDK's en bibliotheken.

**V:** Wanneer kan ik nieuwe FCM-functies en schema's gebruiken voor Mijn meldingen?

**A:** Zodra er een update gepubliceerd op onze API en SDK's, kom – verwachten we hebben iets voor u in de komende maanden.
