---
title: Aanpassing van taal in aangepaste beleidsregels voor Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het gebruik van inhoud in het aangepaste beleid voor meerdere talen te lokaliseren.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/13/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6269ac65e5db20521346d5312bcbadd0905c36e2
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440561"
---
# <a name="language-customization-in-custom-policies"></a>Aanpassing van taal in aangepaste beleidsregels

> [!NOTE]
> Deze functie is beschikbaar als openbare preview.
> 

In aangepaste beleidsregels werkt aanpassing van taal hetzelfde als in de ingebouwde beleidsregels.  Zie de ingebouwde [documentatie](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-language-customization) met de beschrijving van het gedrag in de manier waarop een taal wordt gekozen op basis van de parameters en de browserinstellingen.

## <a name="enable-supported-languages"></a>Enable ondersteunde talen
Als ui-landinstellingen niet is opgegeven en de browser van de gebruiker wordt gevraagd om een van deze talen, worden de ondersteunde talen voor de gebruiker weergegeven.  

Ondersteunde talen zijn gedefinieerd in `<BuildingBlocks>` in de volgende indeling:

```XML
<BuildingBlocks>
  <Localization>
    <SupportedLanguages DefaultLanguage="en">
      <SupportedLanguage>qps-ploc</SupportedLanguage>
      <SupportedLanguage>en</SupportedLanguage>
    </SupportedLanguages>
  </Localization>
</BuildingBlocks>
```

Ondersteunde talen en in welke taal gedragen zich op dezelfde manier als in de ingebouwde beleidsregels.

## <a name="enable-custom-language-strings"></a>Tekenreeksen voor aangepaste taal inschakelen

Het maken van aangepaste taal tekenreeksen zijn twee stappen vereist:
1. Bewerk de `<ContentDefinition>` voor de pagina om op te geven van een resource-ID voor de gewenste talen
2. Maak de `<LocalizedResources>` met de bijbehorende id's in uw `<BuildingBlocks>`

Houd er rekening mee dat u kunt een `<ContentDefinition>` en `<BuildingBlock>` in zowel uw extensiebestand of het bestand met de Relying Party, afhankelijk van of u wilt dat de wijzigingen worden in uw Traceringstabellen beleid of niet.

### <a name="edit-the-contentdefinition-for-the-page"></a>De ContentDefinition voor de pagina bewerken

Voor elke pagina die u wilt om te lokaliseren, kunt u de `<ContentDefinition>` welke taal resources om te controleren of voor elke taal.

```XML
<ContentDefinition Id="api.signuporsignin">
      <LocalizedResourcesReferences>
        <LocalizedResourcesReference Language="fr" LocalizedResourcesReferenceId="fr" />
        <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="en" />
      </LocalizedResourcesReferences>
</ContentDefinition>
```

Frans (fr) en aangepaste tekenreeksen voor Engels (en) in dit voorbeeld worden toegevoegd aan de Unified-pagina voor registreren of aanmelden.  De `LocalizedResourcesReferenceId` voor elk `LocalizedResourcesReference` is hetzelfde als de landinstellingen, maar u kunt een willekeurige tekenreeks als de ID.  Voor elke combinatie van taal en een pagina die u moet een bijbehorend `<LocalizedResources>` weergegeven in de volgende.


### <a name="create-the-localizedresources"></a>De LocalizedResources maken

Uw onderdrukkingen zijn opgenomen in uw `<BuildingBlocks>` en er is een `<LocalizedResources>` voor elke pagina en de taal die u hebt opgegeven in de `<ContentDefinition>` voor elke pagina.  Elke overschrijven is opgegeven als een `<LocalizedString>` zoals in het volgende voorbeeld:

```XML
<BuildingBlocks>
  <Localization>
    <LocalizedResources Id="en">
      <LocalizedStrings>
        <LocalizedString ElementType="ClaimsProvider" StringId="SignInWithLogonNameExchange">Local Account Sign-in</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="DisplayName">Username</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="UserHelpText">Username used for signing in.</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="PatternHelpText">The username you provided is not valid.</LocalizedString>
        <LocalizedString ElementType="UxElement" StringId="button_signin">Sign In Now</LocalizedString>
        <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInvalidPassword">Your password is incorrect.</LocalizedString>
      </LocalizedStrings>
    </LocalizedResources>
  </Localization>
</BuildingBlocks>
```

Er zijn vier typen tekenreeks elementen op de pagina:

**ClaimsProvider** -Labels voor uw id-providers (Facebook, Google, Azure AD enz.) **ClaimType** -Labels voor de kenmerken en de bijbehorende help-tekst of validatiefouten veld **UxElement** - andere elementen op de pagina die er standaard, zoals knoppen, koppelingen of tekst tekenreeks**ErrorMessage** -formulier validatie-foutberichten

Zorg ervoor dat de `StringId`s overeenkomen voor de pagina die u gebruikt deze onderdrukkingen anders het wordt geblokkeerd door de Beleidsvalidatie van.  