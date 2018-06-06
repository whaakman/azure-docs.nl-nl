---
title: Aanpassing van de taal in Azure Active Directory B2C aangepast beleid | Microsoft Docs
description: Informatie over het gebruik localize inhoud in aangepaste beleidsregels voor meerdere talen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 11/13/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c8deabd4d0a4126365b014875624525d5b1f3063
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711753"
---
# <a name="language-customization-in-custom-policies"></a>Aanpassing van de taal in het aangepaste beleid

> [!NOTE]
> Deze functie is openbare preview.
> 

In aangepaste beleidsregels werkt aanpassing van de taal hetzelfde als in de ingebouwde beleid.  Zie de ingebouwde [documentatie](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-language-customization) die het gedrag in hoe een taal is gekozen op basis van de parameters en browserinstellingen beschrijft.

## <a name="enable-supported-languages"></a>Enable ondersteunde talen
Als ui landinstellingen is niet opgegeven en browser van de gebruiker om een van deze talen vraagt, worden de ondersteunde talen aan de gebruiker weergegeven.  

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

Standaardtaal en ondersteunde talen gedragen zich op dezelfde manier als in de ingebouwde beleid.

## <a name="enable-custom-language-strings"></a>Aangepaste taal tekenreeksen inschakelen

Maken van aangepaste taal tekenreeksen zijn twee stappen vereist:
1. Bewerk de `<ContentDefinition>` voor de pagina om op te geven van een resource-ID voor de gewenste talen
2. Maak de `<LocalizedResources>` met overeenkomende id's in uw `<BuildingBlocks>`

Houd er rekening mee dat u kunt plaatsen een `<ContentDefinition>` en `<BuildingBlock>` in zowel het extensiebestand of het relying beleidsbestand, afhankelijk van of u wilt dat de wijzigingen in uw Traceringstabellen beleid of niet worden.

### <a name="edit-the-contentdefinition-for-the-page"></a>De ContentDefinition voor de pagina bewerken

Voor elke pagina die u wilt localize, kunt u in de `<ContentDefinition>` welke taalbronnen om te zoeken voor elke taalcode.

```XML
<ContentDefinition Id="api.signuporsignin">
      <LocalizedResourcesReferences>
        <LocalizedResourcesReference Language="fr" LocalizedResourcesReferenceId="fr" />
        <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="en" />
      </LocalizedResourcesReferences>
</ContentDefinition>
```

Frans (fr) en aangepaste tekenreeksen voor Engels (en) in dit voorbeeld worden toegevoegd aan de Unified registreren of aanmelden pagina.  De `LocalizedResourcesReferenceId` voor elk `LocalizedResourcesReference` is hetzelfde als de landinstellingen, maar u kunt een willekeurige tekenreeks gebruiken als de ID.  Voor elke combinatie van taal en een pagina die u moet een bijbehorend `<LocalizedResources>` weergegeven in de volgende.


### <a name="create-the-localizedresources"></a>De LocalizedResources maken

Uw onderdrukkingen zijn opgenomen in uw `<BuildingBlocks>` en er is een `<LocalizedResources>` voor elke pagina en de taal die u hebt opgegeven in de `<ContentDefinition>` voor elke pagina.  Elke overschrijving is opgegeven als een `<LocalizedString>` zoals in het volgende voorbeeld:

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

Er zijn vier typen van string-elementen op de pagina:

**ClaimsProvider** -Labels voor de id-providers (Facebook, Google, Azure AD enz.) **ClaimType** -Labels voor de kenmerken en de bijbehorende help-tekst of veld validatiefouten **UxElement** - andere elementen op de pagina die er standaard zoals knoppen, koppelingen of tekst string**ErrorMessage** -formulier validatiefoutberichten

Zorg ervoor dat de `StringId`s overeen voor de pagina die u gebruikt deze overschrijvingen anders is geblokkeerd door een voor Beleidsvalidatie tijdens het uploaden.  