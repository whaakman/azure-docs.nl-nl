---
title: Aangepaste beleidsregels van Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over Azure Active Directory B2C aangepast beleid.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 0724227da425eb2faeee9ac4ae8449782e62a241
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445959"
---
# <a name="azure-active-directory-b2c-custom-policies"></a>Azure Active Directory B2C: Aangepast beleid

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="what-are-custom-policies"></a>Wat is aangepast beleid?

Aangepaste beleidsregels zijn configuratiebestanden die het gedrag van uw Azure AD B2C-tenant definiëren. Terwijl **ingebouwde beleidsregels** zijn vooraf gedefinieerd in de portal voor Azure AD B2C voor de meest algemene taken voor identiteit, aangepaste beleidsregels kunnen volledig worden bewerkt door een identiteitsontwikkelaar om uit te voeren een bijna onbeperkt aantal taken. Lees verder voor te bepalen of aangepaste beleidsregels voor u en uw identiteitsscenario zijn.

## <a name="comparing-built-in-policies-and-custom-policies"></a>Vergelijking van de ingebouwde en aangepaste beleid

| | Ingebouwd beleid | Aangepast beleid |
|-|-------------------|-----------------|
|Doelgebruikers | Alle app-ontwikkelaars met of zonder kennis van identiteit | Identiteitsprofessionals: systeemintegrators, adviseurs en interne identiteit teams. Ze vertrouwd bent met OpenIDConnect stromen en begrijpen van id-providers en verificatie op basis van claims |
|Configuratiemethode | Azure-portal met een gebruiksvriendelijke gebruikersinterface | XML-bestanden rechtstreeks te bewerken en vervolgens geüpload naar Azure portal |
|UI-aanpassing | Volledige UI-aanpassing, met inbegrip van HTML, CSS en javascript-ondersteuning (aangepast domein vereist)<br><br>Meertalige ondersteuning met aangepaste tekenreeksen | Dezelfde |
| Aanpassing van kenmerk | Standaardentiteiten en aangepaste kenmerken | Dezelfde |
|Token-en-sessie | Aangepaste token en meerdere sessie-opties | Dezelfde |
|Id-providers| **Vandaag**: vooraf gedefinieerde lokale, sociale provider<br><br>**Toekomstige**: Standards-based OIDC, SAML, OAuth | **Vandaag**: Standards-based OIDC, OAUTH, SAML<br><br>**Toekomstige**: WsFed |
|Identity-taken (voorbeelden) | Meld u aan of aanmelden met lokaal en veel sociale accounts<br><br>Self-service voor wachtwoord opnieuw instellen<br><br>Profiel bewerken<br><br>Multi-factor Auth-scenario 's<br><br>Tokens en sessies aanpassen<br><br>Toegangstoken stromen | Voer de dezelfde taken uit als ingebouwde beleidsregels met behulp van aangepaste id-providers of aangepaste bereiken gebruiken<br><br>Gebruikers inrichten in een ander systeem op het moment van inschrijving<br><br>Verzenden van een welkomstbericht per e-mail via uw eigen e-provider<br><br>Gebruik een gebruikersarchief buiten B2C<br><br>Valideren van de gebruiker opgegeven gegevens met een vertrouwde systeem via API |

## <a name="policy-files"></a>Beleidsbestanden

Een aangepast beleid wordt weergegeven als een of meer XML-bestanden die in een hiërarchische keten naar elkaar verwijzen. De XML-elementen definiëren: schema voor Claims, claims transformaties, inhoudsdefinities claims providers/technische profielen en gebruiker reis indelingsstappen, onder andere elementen.

We raden het gebruik van de drie typen van bestanden voor Groepsbeleid:

- **Een BASE-bestand**, die de meeste van de definities en die Azure een compleet voorbeeld biedt bevat.  We raden dat u een minimum aantal wijzigingen aanbrengt in dit bestand om te helpen bij het oplossen van problemen en op lange termijn onderhouden van uw beleid
- **een extensiebestand** die de unieke configuratiewijzigingen bevat voor uw tenant
- **een Relying Party (RP)-bestand** dat wil zeggen het één taak gerichte-bestand dat wordt aangeroepen rechtstreeks door de toepassing of service (ook wel Relying Party).  Lees het artikel op bestand beleidsdefinities voor meer informatie.  Elke unieke taak vereist een eigen RP en, afhankelijk van de huisstijl van uw vereisten voor het aantal mogelijk "totaal van de toepassingen x totale aantal use cases."


Ingebouwde beleidsregels in Azure AD B2C volgt u de drie bestandspatroon hierboven afgebeelde, maar de ontwikkelaar ziet alleen het bestand Relying Party (RP), terwijl de portal wijzigingen op de achtergrond in het bestand extensies aanbrengt.

## <a name="core-concepts-you-should-know-when-using-custom-policies"></a>Belangrijkste concepten waarover die u weten moet wanneer u aangepaste beleidsregels

### <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C

Azure klant identiteits- en toegangsbeheer (CIAM) service management. De service omvat:

1. De map van een gebruiker in de vorm van een speciale Azure Active Directory toegankelijk via Microsoft Graph en dat bevat gebruikersgegevens voor zowel lokale accounts als federatieve accounts 
2. Toegang tot de **Identity-Ervaringsframework** die regelt de vertrouwensrelatie tussen gebruikers en entiteiten en geeft u claims tussen hen om uit te voeren van een taak voor het beheer van identiteit/toegang 
3. Een beveiligingstokenservice (STS) uitgeven van ID-tokens, vernieuwen tokens, en toegang-tokens (en gelijkwaardige SAML-asserties ondertekend) en valideren van deze resources beveiligen.

Azure AD B2C communiceert met de id-providers, gebruikers en andere systemen, en met de lokale map in de takenreeks voor het bereiken van een taak identiteit (bijvoorbeeld aanmelding een gebruiker een nieuwe gebruiker registreren, een wachtwoord opnieuw instellen). Het onderliggende platform waarmee meerdere partijen vertrouwensrelatie te worden ingesteld en deze stappen uitgevoerd heet de Identiteitservaring-Framework en een beleid (ook wel een gebruikersbeleving of een framework vertrouwensbeleid) expliciet definieert de actoren, de acties, de protocollen, en de de volgorde van stappen volgen om te voltooien.

### <a name="identity-experience-framework"></a>Identity-ervaringsframework

Een volledig worden geconfigureerd, op beleid gebaseerde, cloud-gebaseerde Azure-platform die regelt de vertrouwensrelatie tussen entiteiten (ruim Claims Providers) in de standaard-protocol worden indelingen, zoals OpenIDConnect, OAuth, SAML, WSFed en enkele niet-standaard zijn (bijvoorbeeld REST API-based systeem naar systeem claims worden uitgewisseld). De I2E maakt gebruiksvriendelijke, wit etiket ervaringen die ondersteuning bieden voor HTML, CSS en javascript.  De Identity-Ervaringsframework worden momenteel alleen beschikbaar in de context van de Azure AD B2C-service en de prioriteit voor taken met betrekking tot CIAM werkt.

### <a name="built-in-policies"></a>Ingebouwd beleid

Vooraf gedefinieerde configuratie bestanden die het gedrag van Azure AD B2C om u te vaak uitvoeren van de meest directe identiteit taken (dat wil zeggen, gebruikersregistratie, aanmelding, wachtwoord opnieuw instellen) gebruikt en communiceren met vertrouwde partijen waarvan relatie ook vooraf is gedefinieerd in Azure AD B2C ( bijvoorbeeld Facebook id-provider, LinkedIn, Microsoft-Account, Google-accounts).  Ingebouwde beleidsregels bieden in de toekomst ook voor aanpassing van de id-providers die zich doorgaans in de enterprise-realm, zoals Azure Active Directory Premium, Active Directory/ADFS, Salesforce-ID-Provider enzovoort.


### <a name="custom-policies"></a>Aangepast beleid

De configuratiebestanden die het gedrag van Identiteitservaring-Framework in uw Azure AD B2C-tenant definiëren. Een aangepast beleid is toegankelijk als een of meer XML-bestanden (Zie beleidsbestanden definities) die worden uitgevoerd door de Identity-Ervaringsframework wordt aangeroepen door een relying party (bijvoorbeeld een toepassing). Aangepast beleid kunnen rechtstreeks worden bewerkt door een identiteitsontwikkelaar om uit te voeren een bijna onbeperkt aantal taken. Ontwikkelaars aangepaste beleid configureren, moeten de vertrouwde relaties definiëren in zorgvuldige details om op te nemen van de metagegevens van eindpunten, exacte claims exchange-definities en geheimen, sleutels en certificaten configureren, indien nodig door elke id-provider.

## <a name="policy-file-definitions-for-identity-experience-framework-trust-frameworks"></a>Bestand beleidsdefinities voor Identity-Ervaringsframework vertrouwensrelatie frameworks

### <a name="policy-files"></a>Beleidsbestanden

Een aangepast beleid wordt weergegeven als een of meer XML-bestanden die in een hiërarchische keten naar elkaar verwijzen. De XML-elementen definiëren: schema voor Claims, claims transformaties, inhoudsdefinities claims providers/technische profielen en gebruiker reis indelingsstappen, onder andere elementen.  We raden het gebruik van de drie typen van bestanden voor Groepsbeleid:

- **Een BASE-bestand** bevat de meeste van de definities en die Azure een compleet voorbeeld biedt.  We raden dat u een minimum aantal wijzigingen aanbrengt in dit bestand om te helpen bij het oplossen van problemen en op lange termijn onderhouden van uw beleid
- **een extensiebestand** die de unieke configuratiewijzigingen bevat voor uw tenant
- **een Relying Party (RP)-bestand** is één taak gerichte bestand die wordt opgeroepen rechtstreeks door de toepassing of service (ook wel Relying Party).  Lees het artikel op bestand beleidsdefinities voor meer informatie.  Elke unieke taak vereist een eigen RP en zijn, afhankelijk van de huisstijl van uw vereisten voor het nummer van de 'totale van toepassingen x totale aantal use cases'.

![Beleid voor bestandstypen](media/active-directory-b2c-overview-custom/active-directory-b2c-overview-custom-policy-files.png)

| Type beleid-bestand | Naam van voorbeelden | Aanbevolen gebruik | Neemt over van |
|---------------------|--------------------|-----------------|---------------|
| BASE |TrustFrameworkBase.xml<br><br>Mytenant.onmicrosoft.com-B2C-1A_BASE1.xml | Bevat de core claims schema, claimtransformaties, claims-providers en gebruiker reizen geconfigureerd door Microsoft<br><br>Minimale wijzigingen aanbrengt in dit bestand | Geen |
| Extensie (EXT) | TrustFrameworkExtensions.xml<br><br>Mytenant.onmicrosoft.com-B2C-1A_EXT.xml | Uw wijzigingen in de BASE-bestand hier consolideren<br><br>Aangepaste claims-providers<br><br>Gewijzigde gebruiker reizen<br><br>Uw eigen aangepaste schema-definities | BASE-bestand |
| Relying Party (RP) | B2C_1A_sign_up_sign_in.xml| Token shape en sessie-instellingen hier wijzigen| Extensions(ext) bestand |

### <a name="inheritance-model"></a>Van overnamemodel

Wanneer een toepassing de RP-beleidsbestand aanroept, toevoegen de Identiteitservaring-Framework in B2C alle elementen van basis, en vervolgens van EXTENSIES, en tot slot uit het bestand van de RP-beleid voor het samenstellen van het huidige beleid van kracht.  Elementen van hetzelfde type en met de naam in de RP-bestand wordt overschrijven die in de EXTENSIES EXTENSIES BASE vervangt.

**Ingebouwd beleid** gaat u als volgt de hierboven afgebeelde 3-bestandspatroon in Azure AD B2C, maar de ontwikkelaar ziet alleen het bestand Relying Party (RP), terwijl de portal wijzigingen in de achtergrond in het bestand extensies niet aanbrengt.  Alle Azure AD B2C deelt een Basisbeleid-bestand dat is onder het beheer van de Azure B2C-team en regelmatig wordt bijgewerkt.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md)
