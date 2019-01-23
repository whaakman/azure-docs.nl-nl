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
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: bd9df12cbe941b868c769daccd02c1d81b39f7bd
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54465357"
---
# <a name="security-model-of-azure-notification-hubs"></a>Beveiligingsmodel van Azure Notification Hubs

## <a name="overview"></a>Overzicht

Dit onderwerp beschrijft het beveiligingsmodel van Azure Notification Hubs. Omdat Notification Hubs een Service Bus-entiteit, implementeert ze de dezelfde beveiligingsmodel als Service Bus. Zie voor meer informatie de [Service Bus-verificatie](https://msdn.microsoft.com/library/azure/dn155925.aspx) onderwerpen.

## <a name="shared-access-signature-security-sas"></a>Shared Access Signature Security (SAS)

Notification Hubs implementeert een schema op entiteitsniveau security SAS (Shared Access Signature) genoemd. In dit schema kunt messaging-entiteiten op te geven tot 12 autorisatieregels in de beschrijving die verlenen van rechten voor die entiteit.

Elke regel bevat een naam, de waarde van een sleutel (gedeelde geheim genoemd) en een set rechten, zoals wordt beschreven in de sectie "Beveiligingsclaims." Bij het maken van een Notification Hub, twee regels worden automatisch gemaakt: één met Listen-rechten (die gebruikmaakt van de client-app) en één met alle rechten (die gebruikmaakt van de back-end).

Bij het uitvoeren van registratiebeheer van de van de client-apps, als de gegevens worden verzonden via meldingen is geen gevoelige (bijvoorbeeld updates weer), een veelgebruikte manier voor toegang tot een Meldingshub die is de waarde van de sleutel van de regel luisteren naar alleen-lezen toegang geven tot de client-app en de waarde van de sleutel van de regel voor volledige toegang geven tot de back-end.

Het is niet raadzaam dat u de sleutelwaarde in Windows Store-client-apps insluiten. Een manier om te voorkomen dat de sleutelwaarde insluiten is dat de client-app vanuit de back-end bij het opstarten worden opgehaald.

Het is belangrijk om te begrijpen dat de sleutel met luisteren toegang kan een client-app te registreren voor een label. Als uw app moet registraties beperken tot specifieke labels voor specifieke clients (bijvoorbeeld wanneer tags vertegenwoordigt een gebruikers-id's), moet de registraties uitvoeren op de back-end van uw app. Zie voor meer informatie, beheer van de registratie. Houd er rekening mee dat op deze manier kan de client-app wordt geen directe toegang tot Notification Hubs.

## <a name="security-claims"></a>Beveiligingsclaims

Net als bij andere entiteiten, Notification Hub-bewerkingen zijn toegestaan voor drie beveiligingsclaims: Luisteren, verzenden en beheren.

| Claim   | Description                                          | Toegestane bewerkingen |
| ------- | ---------------------------------------------------- | ------------------ |
| Luisteren  | Maken of bijwerken, lezen en één registraties te verwijderen | Registratie maken/bijwerken<br><br>Lezen registratie<br><br>Alle registraties voor een ingang lezen<br><br>Registratie verwijderen |
| Verzenden    | Berichten verzenden naar de notification hub                | Bericht verzenden |
| Beheren  | CRUDs op Notification Hubs (inclusief het bijwerken van de PNS-referenties en sleutels) en lezen registraties op basis van tags |Meldingshubs maken/bijwerken/lezen/verwijderen<br><br>Registraties lezen op label |

Notification Hubs verleend door Microsoft Azure Access Control-tokens en handtekening-tokens die zijn gegenereerd met gedeelde sleutels die zijn geconfigureerd op de Notification Hub-mailclaims geaccepteerd.
