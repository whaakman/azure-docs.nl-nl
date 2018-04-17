---
title: SSO-sessie beheren met behulp van aangepaste beleidsregels - Azure AD B2C | Microsoft Docs
description: Informatie over het beheren van de SSO-sessies met aangepast beleid in Azure AD B2C.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 10/20/2017
ms.author: davidmu
ms.openlocfilehash: ca7160d39d5d26ca69345ce636f22afbe44b25db
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="azure-ad-b2c-single-sign-on-sso-session-management"></a>Azure AD B2C: Eenmalige aanmelding (SSO) sessiebeheer

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure AD B2C kan een beheerder om te bepalen hoe een Azure AD B2C communiceert met een gebruiker nadat de gebruiker al is geverifieerd. Dit wordt gedaan door sessiebeheer voor eenmalige aanmelding. De beheerder kan bijvoorbeeld bepalen of de selectie van de id-providers wordt weergegeven of dat de details van het lokale account moeten opnieuw worden ingevoerd. In dit artikel wordt beschreven hoe de SSO-instellingen configureren voor Azure AD B2C.

## <a name="overview"></a>Overzicht

SSO-sessiebeheer bestaat uit twee delen. De eerste omgaat met de interactie van de gebruiker rechtstreeks met Azure AD B2C en de andere overeenkomsten met een van de gebruiker interactie met externe partijen zoals Facebook. Azure AD B2C niet overschrijven of negeren van SSO-sessies die kunnen worden gehouden door externe partijen. In plaats daarvan wordt de doorsturen via Azure AD B2C om te krijgen tot de externe partij 'onthouden', moet de gebruiker te selecteren van de identiteitsprovider sociale of enterprise reprompt voorkomen. De ultieme SSO beslissing blijft met de externe partij.

## <a name="how-does-it-work"></a>Hoe werkt het?

SSO-sessie management maakt gebruik van dezelfde heeft als andere technische profiel in het aangepaste beleid. Wanneer een stap orchestration wordt uitgevoerd, de technische profiel die zijn gekoppeld aan de stap wordt gevraagd om een `UseTechnicalProfileForSessionManagement` verwijzing. Als dit bestaat, wordt de waarnaar wordt verwezen provider van de SSO-sessie wordt gecontroleerd om te zien of de gebruiker een sessie deelnemer is. Als dus de SSO-sessie-provider wordt gebruikt voor het opnieuw bevolken de sessie. Op dezelfde manier als de uitvoering van een orchestration stap voltooid is, de provider wordt gebruikt voor het opslaan van informatie in de sessie als een sessie-provider voor eenmalige aanmelding is opgegeven.

Azure AD B2C is gedefinieerd een aantal SSO sessie providers die kunnen worden gebruikt:

* NoopSSOSessionProvider
* DefaultSSOSessionProvider
* ExternalLoginSSOSessionProvider
* SamlSSOSessionProvider

Eenmalige aanmelding management klassen zijn opgegeven met behulp van de `<UseTechnicalProfileForSessionManagement ReferenceId=“{ID}" />` element van een technische profiel.

### <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

Als de naam bepaalt, wordt deze provider geen effect. Deze provider kan worden gebruikt voor het onderdrukken van SSO-gedrag voor een specifieke technische profiel.

### <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

Deze provider kan worden gebruikt voor het opslaan van claims in een sessie. Deze provider is doorgaans waarnaar wordt verwezen in een technische profiel voor het beheer van lokale accounts. 

> [!NOTE]
> Wanneer u de DefaultSSOSessionProvider voor het opslaan van claims in een sessie gebruikt, moet u ervoor zorgen dat claims die moeten worden geretourneerd naar de toepassing of gebruikt door de voorwaarden in de volgende stappen zijn opgeslagen in de sessie of uitgebreid door een leesbewerking van het gebruikersprofiel voor in de map. Dit zorgt ervoor dat de verificatie-reis niet op ontbrekende claims uitvoeren.

```XML
<TechnicalProfile Id="SM-AAD">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="objectId" />
        <PersistedClaim ClaimTypeReferenceId="newUser" />
        <PersistedClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" />
    </PersistedClaims>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="objectIdFromSession" DefaultValue="true" />
    </OutputClaims>
</TechnicalProfile>
```

Gebruiken om claims toe te voegen in de sessie, de `<PersistedClaims>` element van het technische profiel. Wanneer de provider wordt gebruikt voor de sessie, de persistente opnieuw bevolken claims toegevoegd aan de eigenschappenverzameling van claims. `<OutputClaims>` wordt gebruikt voor het ophalen van claims van de sessie.

### <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

Deze provider wordt gebruikt voor het onderdrukken van het scherm 'Kies identiteitsprovider'. Dit is doorgaans waarnaar wordt verwezen in een technische profiel geconfigureerd voor een externe id-provider, zoals Facebook. 

```XML
<TechnicalProfile Id="SM-SocialLogin">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

Deze provider wordt gebruikt voor het beheren van de Azure AD B2C SAML-sessies tussen apps, evenals externe SAML-id-providers.

```XML
<TechnicalProfile Id="SM-Reflector-SAML">
    <DisplayName>Session Management Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="IncludeSessionIndex">false</Item>
        <Item Key="RegisterServiceProviders">false</Item>
    </Metadata>
</TechnicalProfile>
```

Er zijn twee metagegevensitems in het technische profiel:

| Item | Standaardwaarde | Mogelijke waarden | Beschrijving
| --- | --- | --- | --- |
| IncludeSessionIndex | true | waar/onwaar | Hiermee wordt aangegeven met de provider dat de index van de sessie moet worden opgeslagen. |
| RegisterServiceProviders | true | waar/onwaar | Hiermee wordt aangegeven dat alle SAML-providers die een bewering zijn verleend door de provider moet registreren. |

Wanneer u de provider voor het opslaan van een SAML-sessie-id-provider, moeten de bovenstaande items beide niet false zijn. Wanneer u de provider voor het opslaan van de B2C-SAML-sessie, moet de bovenstaande items true of wordt weggelaten als de standaardinstellingen van toepassing zijn.

>[!NOTE]
> SAML-sessie afmelden vereist de `SessionIndex` en `NameID` te voltooien.

## <a name="next-steps"></a>Volgende stappen

We graag feedback en suggesties! Als u problemen met dit onderwerp ondervindt, boeken bij Stack Overflow met behulp van de tag ['azure ad b2c'](https://stackoverflow.com/questions/tagged/azure-ad-b2c). Voor functieaanvragen, de stemmen voor hen in onze [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

