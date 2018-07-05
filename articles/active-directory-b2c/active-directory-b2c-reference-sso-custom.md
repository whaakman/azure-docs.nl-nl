---
title: Sessiebeheer voor eenmalige aanmelding met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Informatie over het beheren van eenmalige aanmelding-sessies met behulp van aangepaste beleidsregels in Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/20/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 351b48f2e2766b4974a5a41b5e95acfbd63dbfc9
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443219"
---
# <a name="azure-ad-b2c-single-sign-on-sso-session-management"></a>Azure AD B2C: Eenmalige aanmelding (SSO) sessiebeheer

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure AD B2C kan een beheerder om te bepalen hoe een Azure AD B2C communiceert met een gebruiker nadat de gebruiker is al geverifieerd. Dit wordt gedaan door sessiebeheer voor eenmalige aanmelding. De beheerder kan bijvoorbeeld bepalen of de selectie van de id-providers wordt weergegeven, of dat lokale accountdetails moeten opnieuw worden ingevoerd. In dit artikel wordt beschreven hoe u de SSO-instellingen configureren voor Azure AD B2C.

## <a name="overview"></a>Overzicht

Eenmalige aanmelding sessiebeheer bestaat uit twee delen. De eerste omgaat met de interactie van de gebruiker rechtstreeks met Azure AD B2C en de andere aanbiedingen met een van de gebruiker interactie met externe partijen, zoals Facebook. Azure AD B2C niet overschrijven of negeren van SSO-sessies die kunnen worden gehouden door externe partijen. In plaats van is de route via Azure AD B2C om te gaan naar de externe partij 'onthouden', moet de gebruiker selecteert de id-provider voor sociale of zakelijke reprompt voorkomen. De ultieme SSO beslissing blijft met de externe partij.

## <a name="how-does-it-work"></a>Hoe werkt het?

Sessiebeheer voor eenmalige aanmelding gebruikt dezelfde semantiek als elk ander technisch profiel in het aangepaste beleid. Wanneer een orchestration-stap wordt uitgevoerd, het technische profiel dat is gekoppeld aan de stap wordt gecontroleerd op een `UseTechnicalProfileForSessionManagement` verwijzing. Als er een bestaat, wordt de waarnaar wordt verwezen, SSO-sessie-provider wordt gecontroleerd om te zien of de gebruiker een deelnemer sessie. Als dus de SSO-sessie-provider wordt gebruikt voor het opnieuw bevolken de sessie. Wanneer de uitvoering van een orchestration-stap voltooid is, wordt de provider op dezelfde manier gebruikt voor het opslaan van informatie in de sessie als een sessie-provider voor eenmalige aanmelding is opgegeven.

Azure AD B2C is een aantal van de sessie-providers voor eenmalige aanmelding waarmee kan worden gedefinieerd:

* NoopSSOSessionProvider
* DefaultSSOSessionProvider
* ExternalLoginSSOSessionProvider
* SamlSSOSessionProvider

Eenmalige aanmelding management klassen zijn opgegeven met behulp van de `<UseTechnicalProfileForSessionManagement ReferenceId=“{ID}" />` element van een technisch profiel.

### <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

Als de naam aangeeft, wordt deze provider gebeurt er niets. Deze provider kan worden gebruikt voor het onderdrukken van eenmalige aanmelding gedrag voor een specifieke technische profiel.

### <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

Deze provider kan worden gebruikt voor het opslaan van claims tijdens een sessie. Deze provider wordt meestal verwezen in een technisch profiel gebruikt voor het beheren van lokale accounts. 

> [!NOTE]
> Wanneer u de DefaultSSOSessionProvider om op te slaan de claims in een sessie gebruikt, moet u ervoor zorgen dat de claims die moeten worden geretourneerd naar de toepassing of door vooraf voorwaarden in de volgende stappen gebruikt die zijn opgeslagen in de sessie of aangevuld door een leesbewerking vanuit het gebruikersprofiel in de map. Dit zorgt ervoor dat de verificatie-reis niet op ontbrekende claims uitvoeren.

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

Als u wilt toevoegen de claims in de sessie, gebruikt u de `<PersistedClaims>` element van het technische profiel. Wanneer de provider wordt gebruikt voor de sessie, het persistente opnieuw bevolken claims worden toegevoegd aan de eigenschappenverzameling claims. `<OutputClaims>` wordt gebruikt voor het ophalen van claims van de sessie.

### <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

Deze provider wordt gebruikt voor het onderdrukken van het scherm 'Kies id-provider'. Dit wordt meestal verwezen in een technisch profiel geconfigureerd voor een externe id-provider, zoals Facebook. 

```XML
<TechnicalProfile Id="SM-SocialLogin">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

Deze provider wordt gebruikt voor het beheren van de Azure AD B2C-SAML-sessies tussen apps, evenals externe SAML-id-providers.

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
| IncludeSessionIndex | true | waar/onwaar | Geeft aan met de provider dat de index van de sessie moet worden opgeslagen. |
| RegisterServiceProviders | true | waar/onwaar | Geeft aan dat alle SAML-providers die een bewering zijn uitgegeven door de provider moet worden geregistreerd. |

Wanneer u de provider voor het opslaan van een SAML-sessie-id-provider, moet de bovenstaande items beide ONWAAR zijn. Wanneer u de provider voor het opslaan van de B2C-SAML-sessie, moet de bovenstaande items ' waar ' of wordt weggelaten als de standaardwaarden ' True zijn '.

>[!NOTE]
> SAML-sessie afmelden vereist de `SessionIndex` en `NameID` om te voltooien.

## <a name="next-steps"></a>Volgende stappen

We graag feedback en suggesties! Als u problemen met dit onderwerp hebt, post plaatsen op Stack Overflow met het label ['azure-ad-b2c'](https://stackoverflow.com/questions/tagged/azure-ad-b2c). Voor functieaanvragen, stemmen ze in onze [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

