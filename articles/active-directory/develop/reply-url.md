---
title: Omleidings-URI/antwoord-URL beperkingen en limieten - identiteitsplatform van Microsoft
description: Beperkingen voor antwoord-URL/Omleidings-URL's en beperkingen
author: SureshJa
ms.author: sureshja
manager: CelesteDG
ms.date: 06/29/2019
ms.topic: article
ms.subservice: develop
ms.service: active-directory
ms.reviewer: lenalepa, manrath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07be7d0c70193fec88782fea681e33d6b4cf4b40
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67486231"
---
# <a name="redirect-urireply-url-restrictions-and-limitations"></a>Beperkingen voor omleidings-URI en antwoord-URL

Een omleidings-URI, of de antwoord-URL, is de locatie dat de autorisatie-server wordt de gebruiker doorsturen naar één keer de app heeft geautoriseerd en een autorisatiecode of toegang verleend token. De code of het token is opgenomen in de omleiding URI of het antwoord het is dus belangrijk dat u de juiste locatie als onderdeel van het registratieproces app registreren token.

## <a name="maximum-number-of-redirect-uris"></a>Maximum aantal omleidings-URI 's

De volgende tabel ziet u het maximum aantal omleidings-URI's die u tijdens de registratie van uw app kunt toevoegen. 

| Accounts worden aangemeld | Maximum aantal omleidings-URI 's | Description |
|--------------------------|---------------------------------|-------------|
| Microsoft werk- of schoolaccount in de tenant Azure Active Directory (Azure AD) van een organisatie | 256 | `signInAudience` veld in het toepassingsmanifest is ingesteld op *AzureADMyOrg* of *AzureADMultipleOrgs* |
| Persoonlijke Microsoft-accounts en werk en schoolaccounts | 100 | `signInAudience` veld in het toepassingsmanifest is ingesteld op *AzureADandPersonalMicrosoftAccount* |

## <a name="maximum-uri-length"></a>Maximale lengte van de URI

U kunt maximaal 256 tekens gebruiken voor elke omleidings-URI die u aan een app-registratie toevoegt.

## <a name="restrictions-using-a-wildcard-in-uris"></a>Beperkingen met een jokerteken in URI 's

Wildcard URIs, zoals `https://*.contoso.com`, zijn handig, maar moeten worden vermeden. Gebruik van jokertekens in de omleidings-URI heeft invloed op de beveiliging. Op basis van de OAuth 2.0-specificatie ([sectie 3.1.2 van RFC 6749](https://tools.ietf.org/html/rfc6749#section-3.1.2)), URI voor een omleiding van het eindpunt moet een absolute URI. 

Het toepassingsmodel voor Azure AD biedt geen ondersteuning voor jokertekens URI's voor apps die zijn geconfigureerd voor aanmelden met persoonlijke Microsoft-accounts en werk- of schoolaccounts. Jokertekens URI's zijn echter toegestaan voor apps die zijn geconfigureerd om te melden in werk of school-accounts in Azure AD-tenant van een organisatie vandaag nog. 
 
> [!NOTE]
> De nieuwe [App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) ervaring niet kan ontwikkelaars jokertekens URI's toevoegen aan de gebruikersinterface. Toe te voegen jokerteken URI voor apps die zich aanmeldt werk of school-accounts wordt alleen ondersteund door de app-manifest editor. Voortaan, nieuwe apps niet mogelijk het gebruik van jokertekens in de omleidings-URI. Echter, oudere apps met jokertekens in redirect URI's blijven werken.

Als uw scenario dat meer omleidings-URI's dan de maximale limiet die is toegestaan vereist, in plaats van het toevoegen van een jokerteken omleidings-URI, kunt u overwegen een van de volgende methoden.

### <a name="use-a-state-parameter"></a>Een status-parameter gebruiken

Als u een aantal subdomeinen hebt, en als uw scenario moet u gebruikers bij een geslaagde verificatie omleiden naar dezelfde pagina waar ze zijn begonnen, kan met behulp van een parameter state nuttig zijn. 

In deze benadering:

1. Maak een 'gedeeld' omleidings-URI per toepassing voor het verwerken van de beveiligingstokens die u van het autorisatie-eindpunt ontvangt.
1. Uw toepassing kunt toepassingsspecifieke parameters (zoals subdomein URL waar de gebruiker die afkomstig is of iets zoals huisstijlinformatie) verzenden in de parameter state. Wanneer u een parameter state, bescherming tegen CSRF-beveiliging die zijn opgegeven in [sectie 10.12 van RFC 6749](https://tools.ietf.org/html/rfc6749#section-10.12)). 
1. De parameters toepassingsspecifieke bevat alle informatie die nodig zijn voor de toepassing om weer te geven de juiste-ervaring voor de gebruiker, dat wil zeggen, de status van de juiste toepassing maken. De Azure AD autorisatie-eindpunt stroken HTML-code van de parameter state dus zorg ervoor dat u doorgeeft niet HTML-inhoud in dit rapport.
1. Wanneer Azure AD een antwoord naar de 'gedeeld' omleidings-URI stuurt, wordt de parameter state verzonden naar de toepassing.
1. De toepassing kunt vervolgens de waarde in de parameter state gebruiken om te bepalen welke URL voor het verzenden van verdere de gebruiker. Zorg ervoor dat u voor CSRF-beveiliging valideert.

> [!NOTE]
> Deze benadering kan een waarmee is geknoeid client om te wijzigen van de extra parameters die worden verzonden in de parameter state, waardoor de gebruiker wordt omgeleid naar een andere URL op, die de [open redirector threat](https://tools.ietf.org/html/rfc6819#section-4.2.4) beschreven in RFC 6819. Daarom moet de client deze parameters beveiligen door het versleutelen van de status of de controle van het op een andere manier zoals domeinnaam in de omleidings-URI op basis van het token valideren.

### <a name="add-redirect-uris-to-service-principals"></a>Toevoegen aan service-principals omleidings-URI's

Een andere benadering is het toevoegen van omleidings-URI's aan de [service-principals](app-objects-and-service-principals.md#application-and-service-principal-relationship) die staan voor de appregistratie van uw in een Azure AD-tenant. U kunt deze aanpak gebruiken wanneer u een parameter state niet gebruiken of uw scenario moet u nieuwe omleidings-URI's toevoegen aan uw app-registratie voor elke nieuwe tenant die u ondersteunt. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [toepassingsmanifest](reference-app-manifest.md)
