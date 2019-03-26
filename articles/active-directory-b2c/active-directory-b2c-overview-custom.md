---
title: Aangepaste beleidsregels van Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over Azure Active Directory B2C aangepast beleid.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 7921454cc9269278db58fcc50bc63ca49b41b1e0
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58417930"
---
# <a name="custom-policies-in-azure-active-directory-b2c"></a>Aangepaste beleidsregels in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Aangepaste beleidsregels zijn configuratiebestanden die het gedrag van uw Azure Active Directory (Azure AD) B2C-tenant definiëren. Gebruikersstromen zijn vooraf gedefinieerd in de Azure AD B2C-portal voor de meest algemene taken voor de identiteit. Aangepaste beleidsregels kunnen volledig worden bewerkt door een identiteitsontwikkelaar om veel verschillende taken te voltooien.

## <a name="comparing-user-flows-and-custom-policies"></a>Vergelijking van de gebruikersstromen en aangepaste beleidsregels

| | Gebruikersstromen | Aangepast beleid |
|-|-------------------|-----------------|
| Doelgebruikers | Alle ontwikkelaars van toepassingen met of zonder kennis van de identiteit. | Identiteitsprofessionals, systeemintegrators, adviseurs en interne identiteit teams. Ze vertrouwd bent met OpenIDConnect stromen en informatie over id-providers en verificatie op basis van claims. |
| Configuratiemethode | Azure-portal met een gebruiksvriendelijke-gebruikersinterface (UI). | XML-bestanden rechtstreeks te bewerken en vervolgens geüpload naar Azure portal. |
| UI-aanpassing | Volledige UI-aanpassing met inbegrip van HTML, CSS en JavaScript.<br><br>Meertalige ondersteuning met aangepaste tekenreeksen. | Dezelfde |
| Aanpassing van kenmerk | Standaardentiteiten en aangepaste kenmerken. | Dezelfde |
| Token-en-sessie | Aangepaste token en meerdere sessie-opties. | Dezelfde |
| Id-providers | Vooraf gedefinieerde lokale of sociale provider en de meeste OIDC id-providers, zoals Federatie met Azure Active Directory-tenants. | OIDC-standaarden gebaseerde, OAUTH en SAML.  Verificatie is ook mogelijk met behulp van integratie met REST-API's. |
| Identity-taken | Meld u aan of aanmelden met lokaal of veel sociale accounts.<br><br>Self-service voor wachtwoord opnieuw instellen.<br><br>Profiel bewerken.<br><br>Multi-Factor Authentication.<br><br>Tokens en sessies aanpassen.<br><br>Toegang tot het token van de stromen. | Voer de dezelfde taken uit als gebruikersstromen met behulp van aangepaste id-providers of gebruik van aangepaste bereiken.<br><br>Een gebruikersaccount inrichten in een ander systeem op het moment van inschrijving.<br><br>Verzendt een welkomstbericht per e-mail via uw eigen e-provider.<br><br>Gebruik een archief van de gebruiker buiten Azure AD B2C.<br><br>Valideren van de gebruiker opgegeven gegevens met een vertrouwde systeem met behulp van een API. |

## <a name="policy-files"></a>Beleidsbestanden

Deze drie soorten beleidsbestanden worden gebruikt:

- **Base-bestand** -bevat de meeste van de definities. Het is raadzaam dat u een minimum aantal wijzigingen in dit bestand aanbrengt om te helpen bij het oplossen van problemen en op lange termijn onderhouden van uw beleid.
- **Extensiebestand** -bevat de unieke configuratiewijzigingen voor uw tenant.
- **Relying Party (RP) bestand** -één taak gerichte bestand die wordt opgeroepen rechtstreeks door de toepassing of service (ook bekend als een Relying Party). Elke unieke taak vereist een eigen RP en, afhankelijk van de huisstijl van uw vereisten, het aantal mogelijk "totaal van de toepassingen x totale aantal use cases."

Gebruikersstromen in Azure AD B2C volgt u de drie bestandspatroon hierboven afgebeelde, maar de ontwikkelaar ziet alleen de RP-bestand, terwijl de Azure-portal wijzigingen op de achtergrond in het extensiebestand aanbrengt.

## <a name="custom-policy-core-concepts"></a>Aangepast beleid belangrijkste concepten

De klant identiteits- en toegangsbeheer management (CIAM)-service in Azure omvat:

- Een gebruiker-map die toegankelijk is via Microsoft Graph en dat gebruikersgegevens voor zowel lokale accounts als federatieve accounts bevat.
- Toegang tot de **Identity-Ervaringsframework** die regelt de vertrouwensrelatie tussen gebruikers en entiteiten en geeft u claims tussen deze om een identiteit en toegang management-taak te voltooien. 
- Een beveiligingstokenservice (STS) die problemen met ID-tokens, vernieuwen tokens, en toegang-tokens (en gelijkwaardige SAML-asserties ondertekend) en valideert zodanig beveiligen van resources.

Azure AD B2C communiceert met de id-providers, gebruikers en andere systemen, en met de lokale map in de takenreeks voor het bereiken van een taak identiteit. Bijvoorbeeld, meld u aan een gebruiker, een nieuwe gebruiker registreren of een wachtwoord opnieuw instellen. De Identity-Ervaringsframework en een beleid (ook wel een gebruikersbeleving of een framework vertrouwensbeleid) meerdere partijen vertrouwensrelatie te worden ingesteld en expliciet definieert de actoren, de acties die de protocollen en de volgorde van de stappen om te voltooien.

De Identiteitservaring-Framework is een volledig worden geconfigureerd, op beleid gebaseerde, cloud-gebaseerde Azure-platform die regelt de vertrouwensrelatie tussen entiteiten in de standaard-protocol indelingen, zoals OpenIDConnect, OAuth, SAML, WSFed en enkele niet-standaard zijn, bijvoorbeeld over de REST Op basis van API--systeem claims worden uitgewisseld. Het framework maakt gebruiksvriendelijke, wit etiket ervaringen die ondersteuning bieden voor HTML en CSS.

Een aangepast beleid bestaat uit een of meer XML-bestanden die in een hiërarchische keten naar elkaar verwijzen. De XML-elementen definiëren het schema van de claims, claimtransformaties, inhoudsdefinities, claims-providers, technische profielen en gebruiker reis indelingsstappen, onder andere elementen. Een aangepast beleid is toegankelijk als een of meer XML-bestanden die worden uitgevoerd door de Identity-Ervaringsframework wordt aangeroepen door een relying party. Ontwikkelaars aangepaste beleid configureren, moeten de vertrouwde relaties definiëren in zorgvuldige details om op te nemen van de metagegevens van eindpunten, exacte claims exchange-definities en geheimen, sleutels en certificaten configureren, indien nodig door elke id-provider.

### <a name="inheritance-model"></a>Van overnamemodel

Wanneer een toepassing de RP-beleidsbestand aanroept, voegt de Identiteitservaring-Framework in Azure AD B2C alle elementen van base-bestand, vanuit het extensiebestand en klik in de RP-beleidsbestand voor het samenstellen van het huidige beleid van kracht.  Elementen van hetzelfde type en met de naam in de RP-bestand wordt overschreven die in de extensies en extensies onderdrukkingen base.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md)
