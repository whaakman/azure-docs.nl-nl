---
title: Notification Hubs-beveiliging
description: In dit onderwerp wordt uitgelegd beveiliging voor Azure notification hubs.
services: notification-hubs
documentationcenter: .net
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 6506177c-e25c-4af7-8508-a3ddca9dc07c
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 05/31/2019
ms.author: jowargo
ms.openlocfilehash: 3f5b23028094b545262e9c01640890f2c0b989ca
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431252"
---
# <a name="notification-hubs-security"></a>Notification Hubs-beveiliging

## <a name="overview"></a>Overzicht

Dit onderwerp beschrijft het beveiligingsmodel van Azure Notification Hubs.

## <a name="shared-access-signature-security-sas"></a>Shared Access Signature Security (SAS)

Notification Hubs implementeert een schema op entiteitsniveau security SAS (Shared Access Signature) genoemd. In dit schema kunt messaging-entiteiten op te geven tot 12 autorisatieregels in de beschrijving die verlenen van rechten voor die entiteit.

Elke regel bevat een naam, de waarde van een sleutel (gedeelde geheim genoemd) en een set rechten, zoals uitgelegd in [beveiligingsclaims](#security-claims). Bij het maken van een Notification Hub, twee regels worden automatisch gemaakt: één met **luisteren** rechten (die gebruikmaakt van de client-app) en één met **alle** rechten (die gebruikmaakt van de back-end).

Bij het uitvoeren van registratiebeheer van de van de client-apps, als de gegevens worden verzonden via meldingen is geen gevoelige (bijvoorbeeld updates weer), een veelgebruikte manier voor toegang tot een Meldingshub die is de waarde van de sleutel van de regel luisteren naar alleen-lezen toegang geven tot de client-app en de waarde van de sleutel van de regel voor volledige toegang geven tot de back-end.

Apps niet de sleutelwaarde in Windows Store-client-apps insluiten, moet in plaats daarvan de client-app vanuit de back-end bij het opstarten worden opgehaald.

De sleutel met de **luisteren** toegang zorgt dat een client-app te registreren voor een label. Als uw app moet registraties beperken tot specifieke labels voor specifieke clients (bijvoorbeeld wanneer tags vertegenwoordigt een gebruikers-id's), moet de back-end van uw app de registraties uitvoeren. Zie voor meer informatie, [registratiebeheer](notification-hubs-push-notification-registration-management.md). Houd er rekening mee dat op deze manier kan de client-app wordt geen directe toegang tot Notification Hubs.

## <a name="security-claims"></a>Beveiligingsclaims

Net als bij andere entiteiten, Notification Hub-bewerkingen zijn toegestaan voor drie beveiligingsclaims: **Luisteren**, **verzenden**, en **beheren**.

| Claim   | Description                                          | Toegestane bewerkingen |
| ------- | ---------------------------------------------------- | ------------------ |
| Luisteren  | Maken of bijwerken, lezen en één registraties te verwijderen | Registratie maken/bijwerken<br><br>Lezen registratie<br><br>Alle registraties voor een ingang lezen<br><br>Registratie verwijderen |
| Verzenden    | Berichten verzenden naar de notification hub                | Bericht verzenden |
| Beheren  | CRUDs op Notification Hubs (inclusief het bijwerken van de PNS-referenties en sleutels) en lezen registraties op basis van tags |Meldingshubs maken/bijwerken/lezen/verwijderen<br><br>Registraties lezen op label |

Notification Hubs accepteert handtekening tokens gegenereerd met gedeelde sleutels die zijn geconfigureerd op de Notification Hub.

Het is niet mogelijk een melding verzenden naar meer dan één naamruimte. Naamruimten zijn logische container voor notification hubs en niet betrokken bij het verzenden van meldingen.
Het niveau van de naamruimte-toegangsbeleid (referenties) kunnen worden gebruikt voor bewerkingen op naamruimteniveau, bijvoorbeeld: notification hubs weergeven, maken of verwijderen van notification hubs, enzovoort. Alleen het niveau van de hub toegangsbeleid kunt u meldingen verzendt.
