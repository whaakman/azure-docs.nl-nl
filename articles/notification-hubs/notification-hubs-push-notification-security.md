---
title: Beveiliging voor Notification Hubs
description: Dit onderwerp wordt uitgelegd security voor Azure notification hubs.
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: 6506177c-e25c-4af7-8508-a3ddca9dc07c
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 7c3283799806135060bb8ca57ea398c93d1106bb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="security"></a>Beveiliging
## <a name="overview"></a>Overzicht
Dit onderwerp beschrijft het beveiligingsmodel van Azure Notification Hubs. Omdat Notification Hubs een Service Bus-entiteit zijn, implementeren zij het dezelfde beveiligingsmodel als Service Bus. Zie voor meer informatie de [verificatie van Service Bus](https://msdn.microsoft.com/library/azure/dn155925.aspx) onderwerpen.

## <a name="shared-access-signature-security-sas"></a>Beveiliging van Shared Access Signature (SAS)
Notification Hubs implementeert een entiteitsniveau beveiligingsschema SAS (Shared Access Signature) genoemd. Dit schema kunt berichtentiteiten declareren maximaal 12 machtigingsregels in de beschrijving die verlenen van rechten op die entiteit.

Elke regel bevat een naam, de waarde van een sleutel (gedeelde geheim genoemd) en een set rechten, zoals wordt beschreven in de sectie 'Beveiligingsclaims'. Wanneer u een Notification Hub maakt, twee regels worden automatisch gemaakt: één met luisteren rechten (die gebruikmaakt van de client-app) en één met alle rechten (die gebruikmaakt van de back-end voor de app).

Bij het uitvoeren van beheer van de registratie van client-apps, als de gegevens worden verzonden meldingen is geen gevoelige (bijvoorbeeld weer updates), een veelgebruikte manier toegang krijgen tot een Notification Hub is de waarde van de sleutel van de regel Listen alleen toegang geven tot de client-app en de waarde van de sleutel van de regel volledige toegang geven tot de back-end voor de app.

Het is niet raadzaam dat u de sleutelwaarde in client-apps voor Windows Store insluiten. Een manier om te voorkomen dat de sleutelwaarde insluiten is dat de client-app die is opgehaald van de back-end app bij het opstarten.

Het is belangrijk te weten dat de sleutel met de luister-toegang toestaat dat een client-app te registreren voor een label. Als uw app moet registraties beperken tot specifieke labels op specifieke clients (bijvoorbeeld wanneer labels vertegenwoordigen gebruikers-id's), moet de registraties uitvoeren op uw back-end voor de app. Zie voor meer informatie registratie Management. Houd er rekening mee dat op deze manier kan de client-app niet direct toegang tot Notification Hubs hebben wordt.

## <a name="security-claims"></a>Beveiligingsclaims
Net als bij andere entiteiten, Notification Hub-bewerkingen zijn toegestaan voor drie beveiligingsclaims: luisteren, te verzenden en te beheren.

| Claim | Beschrijving | Bewerkingen die zijn toegestaan |
| --- | --- | --- |
| Luisteren |Maken of bij te werken, lezen en verwijderen van één registraties |Registratie maken/bijwerken<br><br>Lezen registratie<br><br>Alle registraties voor een ingang lezen<br><br>Registratie verwijderen |
| Verzenden |Berichten verzenden met de notification hub |Bericht verzenden |
| Beheren |CRUDs van Notification Hubs (inclusief PNS-referenties en beveiligingssleutels bijwerken) en lezen registraties op basis van tags |Maken, bijwerken, lezen/verwijderen notification hubs<br><br>Registraties door code te lezen |

Notification Hubs accepteren claims verleend door Microsoft Azure Access Control-tokens en handtekening-tokens gegenereerd met gedeelde sleutels rechtstreeks op de Notification Hub is geconfigureerd.

