---
title: 'Azure Active Directory B2C: Aangepast beleid | Microsoft Docs'
description: Een onderwerp op Azure Active Directory B2C aangepast beleid
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 1ff398a4-2079-4615-94f1-57de22c0aad6
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: parakhj
ms.openlocfilehash: 6c59075bb1eacb05599b23be3d8731fa40eabf98
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-custom-policies"></a>Azure Active Directory B2C: Aangepast beleid

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="what-are-custom-policies"></a>Wat is aangepast beleid?

Aangepaste beleidsregels zijn configuratiebestanden die het gedrag van uw Azure AD B2C-tenant definiëren. Terwijl **ingebouwde beleid** zijn vooraf gedefinieerd in de Azure AD B2C-portal voor de meest algemene taken voor identiteit, aangepaste beleidsregels kunnen worden volledig bewerkt door een ontwikkelaar identiteit een onbeperkt aantal taken in bijna voltooid. Lees verder voor bepalen of aangepaste beleidsregels bij u en uw identiteitsscenario zijn.

## <a name="comparing-built-in-policies-and-custom-policies"></a>Beleidsregels voor ingebouwde en aangepaste vergelijken

| | Ingebouwde beleid | Aangepaste beleidsregels |
|-|-------------------|-----------------|
|Doelgebruikers | Alle app-ontwikkelaars met of zonder identiteit expertise | Identity-professionals: systeemintegrators, adviseurs en interne identiteit teams. Ze vertrouwd bent met OpenIDConnect stromen en id-providers en verificatie op basis van claims begrijpen |
|Configuratiemethode | Azure-portal met een gebruiksvriendelijke gebruikersinterface | XML-bestanden rechtstreeks te bewerken en vervolgens uploaden naar de Azure-portal |
|UI-aanpassing | Volledige aanpassing van de gebruikersinterface, zoals HTML, CSS en javascript-ondersteuning (aangepast domein vereist)<br><br>Meertalige ondersteuning met een aangepaste tekenreeks | Dezelfde |
| Kenmerk aanpassing | Standaard- en aangepaste kenmerken | Dezelfde |
|Token en sessie-management | Aangepaste token en meerdere sessie-opties | Dezelfde |
|ID-Providers| **Vandaag de dag**: vooraf gedefinieerde lokale, sociale provider<br><br>**Toekomstige**: SAML, van op standaarden gebaseerde OIDC OAuth | **Vandaag de dag**: OAUTH, van op standaarden gebaseerde OIDC SAML<br><br>**Toekomstige**: WsFed |
|Identiteit taken (voorbeelden) | Registreren of aanmelden met lokale en veel sociale accounts<br><br>Self-service voor wachtwoord opnieuw instellen<br><br>Profiel bewerken<br><br>Multi-factor Auth-scenario 's<br><br>Tokens en sessies aanpassen<br><br>Toegangstoken stroomt | Voltooi de taken van ingebouwde beleidsregels met behulp van aangepaste id-providers of aangepaste bereiken gebruiken<br><br>Inrichten gebruiker in een ander systeem op het moment van inschrijving<br><br>Stuur een welkomstbericht via uw eigen e-provider<br><br>Gebruik een gebruikersarchief buiten B2C<br><br>Valideren van de gebruiker opgegeven informatie met een vertrouwde systeem via API |

## <a name="policy-files"></a>Beleidsbestanden

Een aangepast beleid wordt weergegeven als een of meer XML-bestanden die in een hiërarchische keten naar elkaar verwijzen. De XML-elementen definiëren: Claims schema, claims transformaties, definities van inhoud, profielen van claims-providers en technische en Userjourney indelingsstappen, onder andere elementen.

U wordt aangeraden het gebruik van drie soorten beleidsbestanden:

- **Een BASE bestand**, die de meeste van de definities en die Azure een compleet codevoorbeeld biedt bevat.  We raden dat u een minimum aantal wijzigingen aanbrengen in dit bestand om te helpen bij het oplossen van problemen en op lange termijn onderhoud van uw beleid
- **een extensiebestand** die de unieke configuratiewijzigingen bevat voor uw tenant
- **een Relying Party (RP)-bestand** is één taak gerichte bestand die wordt opgeroepen rechtstreeks door de toepassing of service (aka Relying Party).  Lees het artikel op beleid bestand definities voor meer informatie.  Elke unieke taak vereist een eigen RP en afhankelijk van de huisstijl van de vereisten voor het aantal mogelijk 'Totaal aantal toepassingen x totale aantal gevallen'.


Ingebouwde beleid in Azure AD B2C Volg het 3-bestandspatroon afgebeeld hierboven, maar de ontwikkelaar ziet alleen het bestand Relying Party (RP), terwijl de portal wijzigingen aangebracht op de achtergrond in het bestand Extensions worden.

## <a name="core-concepts-you-should-know-when-using-custom-policies"></a>Belangrijkste concepten waarover die u weten moet wanneer u aangepaste beleidsregels

### <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C

Azure klant identiteits- en toegangsbeheer (CIAM) service management. De service omvat:

1. Een map van de gebruiker in de vorm van een speciale Azure Active Directory toegankelijk zijn via Microsoft Graph en dat de gebruikersgegevens voor zowel lokale accounts als federatieve accounts bevat 
2. Toegang tot de **identiteit ervaring Framework** die vertrouwensrelatie tussen gebruikers en entiteiten ingedeeld en geeft u claims tussen deze twee om een identiteit/access management-taak te voltooien 
3. Een beveiligingstokenservice (STS) verlenen id tokens, vernieuwen van tokens, en toegang tokens (en gelijkwaardige SAML asserties) en valideren ze om bronnen te beveiligen.

Azure AD B2C communiceert met identiteitsproviders, gebruikers, andere systemen, en de map van de lokale gebruiker in de reeks voor een taak identiteit (bijvoorbeeld aanmelden voor een gebruiker een nieuwe gebruiker registreren, een wachtwoord opnieuw instellen). Het onderliggende platform dat meerdere partijen vertrouwensrelatie te worden ingesteld en deze stappen uitvoert, heet het kader van de gebruikerservaring identiteit en een beleid (ook wel het traject van een gebruiker of een framework vertrouwensbeleid) definieert u expliciet de actoren, de acties, de protocollen, en de volgorde van stappen te voltooien.

### <a name="identity-experience-framework"></a>Identity-ervaringsframework

Een volledig worden geconfigureerd, beleid gebaseerde, cloud-gebaseerde Azure-platform die vertrouwensrelatie tussen entiteiten (grotendeels claimproviders) ingedeeld in standaardprotocol indelingen zoals OpenIDConnect OAuth, SAML, WSFed en enkele niet-standaard waarden (bijvoorbeeld REST API-based--systeem kunnen worden uitgewisseld claims). De I2E maakt gebruiksvriendelijke, whitelabelled optreedt die ondersteuning voor HTML, CSS en javascript.  Vandaag de dag is het Framework van de gebruikerservaring identiteit alleen beschikbaar in de context van de Azure AD B2C-service en de prioriteit voor taken met betrekking tot CIAM.

### <a name="built-in-policies"></a>Ingebouwde beleid

Vooraf gedefinieerde configuratiebestanden die het gedrag van Azure AD B2C om uit te voeren van de meest vaak gebruikte id taken (dat wil zeggen gebruikersregistratie, aanmelding in wachtwoordherstel) en communiceren met vertrouwde partijen waarvan relatie ook vooraf is gedefinieerd in Azure AD B2C (voor directe voorbeeld Facebook id-provider, LinkedIn, Microsoft-Account, Google-accounts).  In de toekomst daarenboven ingebouwde beleidsregels voor aanpassing van de id-providers die meestal in de enterprise-realm zoals Azure Active Directory Premium, Active Directory AD FS, Salesforce-ID-Provider, enzovoort.


### <a name="custom-policies"></a>Aangepaste beleidsregels

De configuratiebestanden die het gedrag van identiteit ervaring Framework in uw Azure AD B2C-tenant definiëren. Er is een aangepast beleid toegankelijk als een of meer XML-bestanden (Zie beleidsbestanden definities) die worden uitgevoerd door de identiteit ervaring Framework wordt aangeroepen door een relying party (bijvoorbeeld een toepassing). Aangepast beleid kunnen rechtstreeks worden bewerkt door een ontwikkelaar identiteit een onbeperkt aantal taken in bijna voltooid. Ontwikkelaars aangepaste beleidsregels configureren, moeten de vertrouwde relaties definiëren in zorgvuldige samenvattingsniveau metagegevens-eindpunten, exacte claims exchange-definities en geheimen, sleutels en certificaten die door elke id-provider configureren.

## <a name="policy-file-definitions-for-identity-experience-framework-trustframeworks"></a>Beleid bestand definities voor de identiteit ervaring Framework Trustframeworks

### <a name="policy-files"></a>Beleidsbestanden

Een aangepast beleid wordt weergegeven als een of meer XML-bestanden die in een hiërarchische keten naar elkaar verwijzen. De XML-elementen definiëren: Claims schema, claims transformaties, definities van inhoud, profielen van claims-providers en technische en gebruiker reis indelingsstappen, onder andere elementen.  U wordt aangeraden het gebruik van drie soorten beleidsbestanden:

- **Een BASE bestand** bevat de meeste van de definities en die Azure een compleet codevoorbeeld biedt.  We raden dat u een minimum aantal wijzigingen aanbrengen in dit bestand om te helpen bij het oplossen van problemen en op lange termijn onderhoud van uw beleid
- **een extensiebestand** die de unieke configuratiewijzigingen bevat voor uw tenant
- **een Relying Party (RP)-bestand** is één taak gerichte bestand die wordt opgeroepen rechtstreeks door de toepassing of service (aka Relying Party).  Lees het artikel op beleid bestand definities voor meer informatie.  Elke unieke taak vereist een eigen RP en afhankelijk van de huisstijl van de vereisten voor het aantal mogelijk 'Totaal aantal toepassingen x totale aantal gevallen'.

![Beleid voor bestandstypen](media/active-directory-b2c-overview-custom/active-directory-b2c-overview-custom-policy-files.png)

| Bestand met beleidsregel van type | Voorbeelden bestandsnaam | Aanbevolen gebruik | Neemt over van |
|---------------------|--------------------|-----------------|---------------|
| BASE |TrustFrameworkBase.xml<br><br>Mytenant.onmicrosoft.com-B2C-1A_BASE1.xml | Bevat de core claims schema, claimtransformaties, claimproviders en gebruiker trajecten geconfigureerd door Microsoft<br><br>Minimale wijzigingen aanbrengen in dit bestand | Geen |
| Extensie (EXT) | TrustFrameworkExtensions.xml<br><br>Mytenant.onmicrosoft.com-B2C-1A_EXT.xml | Uw wijzigingen aan de BASE-bestand hier consolideren<br><br>Gewijzigde claimproviders<br><br>Gewijzigde gebruiker trajecten<br><br>Uw eigen aangepaste schemadefinities | BASE-bestand |
| Relying Party (RP) | B2C_1A_sign_up_sign_in.XML| Token vorm en sessie instellingen hier wijzigen| Extensions(ext) bestand |

### <a name="inheritance-model"></a>Overnamemodel

Wanneer een toepassing het beleidsbestand RP aanroept, toevoegen het Framework identiteit ervaring in B2C alle elementen van BASE en vervolgens van EXTENSIES, en ten slotte uit het bestand RP beleid voor het samenstellen van het huidige beleid van kracht.  Elementen van het type en de naam in het bestand RP overschrijven die in de UITBREIDINGEN en EXTENSIES onderdrukkingen BASE.

**Ingebouwde beleid** in Azure AD B2C volgen bestand 3 patroon afgebeeld hierboven, maar de ontwikkelaar ziet alleen het bestand Relying Party (RP), terwijl de portal wijzigingen aangebracht op de achtergrond in het bestand Extensions worden.  Alle Azure AD B2C deelt een BASE beleidsbestand dat wordt beheerd door het team van Azure B2C en vaak wordt bijgewerkt.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aan de slag met aangepast beleid](active-directory-b2c-get-started-custom.md)
