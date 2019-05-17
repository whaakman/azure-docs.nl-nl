---
title: Integreren met Microsoft identity-platform | Azure
description: Meer informatie over aanbevolen procedures en algemene vergissingen bij het integreren met het Microsoft identity-platform (v2.0).
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: ryanwi
ms.reviewer: lenalepa, sureshja, jesakowi
ms.custom: aaddev
ms.openlocfilehash: 0d44e5d9f0d1ed893a16c318e3cfa0ee1bfc123b
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65823352"
---
# <a name="microsoft-identity-platform-integration-checklist"></a>Controlelijst voor Microsoft identity-platform-integratie

De controlelijst voor de Microsoft identity-platform-integratie is bedoeld om u te begeleiden tot een hoge kwaliteit en veilige-integratie. Deze aanbevolen procedures worden gemarkeerd en dus Controleer de lijst regelmatig om te controleren of dat u behoudt de kwaliteit en beveiliging van uw app-integratie met het identiteitsplatform van algemene vergissingen bij het integreren met het Microsoft identity-platform. De controlelijst is niet bedoeld om te controleren van de gehele toepassing. De inhoud van de controlelijst worden gewijzigd, omdat we verbeteringen aan het platform aanbrengen.

Als u net begint, lees de [documentatie](index.yml) voor meer informatie over de basisbeginselen van verificatie, toepassingsscenario's in Microsoft identity-platform en meer.

## <a name="testing-your-integration"></a>Uw integratie testen

Gebruik de volgende controlelijst om ervoor te zorgen dat uw toepassing daadwerkelijk is geïntegreerd met de [identiteitsplatform van Microsoft](https://docs.microsoft.com/azure/active-directory/develop/).

### <a name="basics"></a>Basics

|   |   |
|---|---|
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Lees de [voor Microsoft-Platform](https://go.microsoft.com/fwlink/?linkid=2090497&clcid=0x409). Zorg ervoor dat uw toepassing aan de voorwaarden die worden beschreven voldoet als ze zijn ontworpen om gebruikers en het platform te beschermen. |

### <a name="ownership"></a>Eigendom

|   |   |
|---|---|
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Zorg ervoor dat de gegevens die zijn gekoppeld aan het account dat u gebruikt om te registreren en beheren van apps is bijgewerkt. |

### <a name="branding"></a>Huisstijl

|   |   |
|---|---|
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Voldoen aan de [Huisstijlrichtlijnen voor toepassingen](howto-add-branding-in-azure-ad-apps.md). |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Geef een beschrijvende naam en logo voor uw toepassing. Deze informatie wordt weergegeven op de toestemmingsprompt van uw toepassing. Zorg ervoor dat uw naam en het logo vertegenwoordiger van uw bedrijf/product zijn zodat gebruikers gefundeerde beslissingen kunnen nemen. Zorg ervoor dat u niet de enige handelsmerken bent schenden. |

### <a name="privacy"></a>Privacy

|   |   |
|---|---|
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Vindt u koppelingen met de gebruiksvoorwaarden en privacyverklaring van uw app. |

### <a name="security"></a>Beveiliging

|   |   |
|---|---|
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Eigendom van alle de omleidings-URI's onderhouden en de DNS-records voor deze up-to-date houden. Gebruik geen jokertekens (*) in de URI's. Zorg ervoor dat alle URI's zijn beveiligd en versleuteld (bijvoorbeeld met behulp van https-schema's) voor web-apps. Voor openbare clients gebruiken platform-specifieke omleidings-URI's indien van toepassing (voornamelijk voor iOS en Android). Anders gebruiken omleidings-URI's met een grote hoeveelheid aanvraaggrootte om conflicten te voorkomen dat bij het aanroepen van terug naar uw app. Als uw app uit een geïsoleerde webagent wordt gebruikt, mag u https://login.microsoftonline.com/nativeclient. Controleer en verwijder alle niet-gebruikte of onnodige omleidings-URI's op een geregelde. |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Als uw app is geregistreerd in een map, minimaliseren en de lijst met eigenaren registratie handmatig te controleren. |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Geen ondersteuning voor de [OAuth2 impliciete stroom verlenen](v2-oauth2-implicit-grant-flow.md) , tenzij dit expliciet vereist. Meer informatie over het geldige scenario [hier](v1-oauth2-implicit-grant-flow.md#suitable-scenarios-for-the-oauth2-implicit-grant). |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Gebruik geen [resource-eigenaar wachtwoord referentie stroom (ROPC)](v2-oauth-ropc.md), die rechtstreeks wachtwoorden van gebruikers worden verwerkt. Deze stroom een hoge mate van blootstelling van vertrouwen en de gebruiker vereist en moet alleen worden gebruikt wanneer andere, beter te beveiligen, stromen kunnen niet worden gebruikt. |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Beveiligen en beheren van de referenties van uw app. Gebruik [referenties van het certificaat](active-directory-certificate-credentials.md), niet wachtwoordreferenties (client geheimen). Als u de wachtwoordreferenties van een gebruiken moet, geen deze handmatig instellen. Geen referenties opslaan in code of configuratieversie en nooit zodat ze worden verwerkt door de mens. Gebruik indien mogelijk [beheerde identiteiten voor een Azure-resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) of [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) voor het opslaan en regelmatig draaien uw referenties. |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Zorg ervoor dat uw toepassing de minste bevoegdheden machtigingen worden aangevraagd. Vraag alleen voor de machtigingen die uw toepassing dit echt nodig heeft en alleen wanneer u ze nodig hebt. Informatie over de verschillende [typen machtigingen](v1-permissions-and-consent.md#types-of-permissions). Gebruik alleen de machtigingen van de toepassing indien nodig; gedelegeerde machtigingen gebruik waar mogelijk. Zie voor een volledige lijst met machtigingen voor Microsoft Graph, [machtigingen verwijzing](https://docs.microsoft.com/graph/permissions-reference). |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Als u van een API met behulp van de Microsoft identity-platform beveiligen wilt, zorgvuldig zien via de machtigingen voor die het beschikbaar te maken. Overweeg wat de juiste granulariteit voor uw oplossing is en welke machtiging(en) toestemming van een beheerder vereist. Controleer op de verwachte machtigingen in de binnenkomende tokens voordat u een autorisatie beslissingen te nemen. |

### <a name="implementation"></a>Implementatie

|   |   |
|---|---|
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Gebruik oplossingen voor moderne verificatie (OAuth 2.0, [OpenID Connect](v2-protocols-oidc.md)) gebruikers veilig aanmelden. |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Niet implementeren van de protocollen – use [ondersteund met een Microsoft-verificatiebibliotheken](reference-v2-libraries.md) (MSAL, server-middleware). Zorg ervoor dat u de meest recente versie van de verificatiebibliotheek die u hebt geïntegreerd met. |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Als de gegevens die uw app nodig heeft, beschikbaar via is [Microsoft Graph](https://developer.microsoft.com/graph), machtigingen voor deze gegevens met behulp van de Microsoft Graph-eindpunt in plaats van de afzonderlijke API aanvragen. |

### <a name="end-user-experience"></a>Eindgebruikerservaring

|   |   |
|---|---|
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | [Inzicht in de ervaring toestemming](application-consent-experience.md) en configureer de stukjes toestemmingsprompt van uw app zodat eindgebruikers en beheerders onvoldoende gegevens beschikbaar zijn om te bepalen of ze vertrouwen dat uw app. |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Beperk het aantal keren dat een gebruiker aanmeldingsreferenties opgeven moet tijdens het gebruik van uw app door te proberen op de achtergrond verificatie (op de achtergrond token acquisition) voordat u interactieve stromen. |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Gebruik niet 'prompt = toestemming' voor elke aanmelding. Gebruik alleen prompt = toestemming als u hebt vastgesteld dat u vragen om toestemming om aanvullende machtigingen (bijvoorbeeld moet als u de vereiste machtigingen van uw app hebt gewijzigd). |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Indien van toepassing, Verrijk uw toepassing met gebruikersgegevens. Gebruik de [Microsoft Graph API](https://developer.microsoft.com/graph) is een eenvoudige manier om dit te doen. De [Graph explorer](https://developer.microsoft.com/graph/graph-explorer) hulpprogramma waarmee u aan de slag kunt. |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | De volledige set machtigingen die uw app nodig heeft, zodat beheerders eenvoudig toestemming aan hun tenant verlenen kunnen registreren. Gebruik [incrementele toestemming](azure-ad-endpoint-comparison.md#incremental-and-dynamic-consent) tijdens runtime om gebruikers te begrijpen waarom te helpen uw app machtigingen die kunnen betrekking hebben op of gebruikers wanneer op de eerste keer is aangevraagd verwarren wordt aangevraagd. |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Implementeer een [ervaring voor eenmalige afmelding opschonen](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-6-SignOut). Privacy en beveiliging vereist is, en zorgt voor een goede gebruikerservaring. |

### <a name="testing"></a>Testen

|   |   |
|---|---|
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Test voor [beleid voor voorwaardelijke toegang](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-6-SignOut) die invloed kunnen zijn op uw gebruikers de mogelijkheid om uw toepassing te gebruiken. |
| ![Selectievakje](./media/active-directory-integration-checklist/checkbox-two.svg) | Test uw toepassing met alle mogelijke accounts die u wilt ondersteunen (voor een voorbeeld, werk of schoolaccounts, persoonlijke Microsoft-accounts, accounts van kinderen en onafhankelijke accounts). |

## <a name="additional-resources"></a>Aanvullende resources

Gedetailleerde informatie over v2.0 verkennen:

* [Microsoft identity-platform (v2.0-overzicht)](v2-overview.md)
* [Referentie voor Microsoft identity platform-protocollen](active-directory-v2-protocols.md)
* [Naslaginformatie voor Access-tokens](access-tokens.md)
* [Naslaginformatie voor id-tokens](id-tokens.md)
* [Verificatiebibliotheken verwijzen naar](reference-v2-libraries.md)
* [Machtigingen en toestemming in Microsoft identity-platform](v2-permissions-and-consent.md)
* [Microsoft Graph API](https://developer.microsoft.com/graph)
