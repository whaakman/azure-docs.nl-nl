---
title: Geef een toegangs token door via een aangepast beleid voor uw toepassing in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over hoe u een toegangs token kunt door geven voor OAuth 2.0-id-providers als een claim via een aangepast beleid voor uw toepassing in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 2253e9f6331662f0ead0251f9affd7996e02aa31
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846878"
---
# <a name="pass-an-access-token-through-a-custom-policy-to-your-application-in-azure-active-directory-b2c"></a>Geef een toegangs token door via een aangepast beleid voor uw toepassing in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Een [aangepast beleid](active-directory-b2c-get-started-custom.md) in azure Active Directory (Azure AD) B2C biedt gebruikers van uw toepassing de mogelijkheid zich aan te melden of zich aan te melden met een id-provider. Als dit gebeurt, ontvangt Azure AD B2C een [toegangs token](active-directory-b2c-reference-tokens.md) van de ID-provider. Azure AD B2C gebruikt dat token om informatie over de gebruiker op te halen. U voegt een claim type en uitvoer claim toe aan uw aangepaste beleid om het token door te geven aan de toepassingen die u registreert in Azure AD B2C.

Azure AD B2C ondersteunt het door geven van het toegangs token van [OAuth 2,0](active-directory-b2c-reference-oauth-code.md) -en [OpenID Connect Connect](active-directory-b2c-reference-oidc.md) -id-providers. Voor alle andere id-providers wordt de claim leeg geretourneerd.

## <a name="prerequisites"></a>Vereisten

- Het aangepaste beleid is geconfigureerd met een OAuth 2,0-of OpenID Connect Connect-ID-provider.

## <a name="add-the-claim-elements"></a>De claim elementen toevoegen

1. Open uw *TrustframeworkExtensions. XML-* bestand en voeg het volgende **claim** type-element toe `identityProviderAccessToken` met een id van aan het **ClaimsSchema** -element:

    ```XML
    <BuildingBlocks>
      <ClaimsSchema>
        <ClaimType Id="identityProviderAccessToken">
          <DisplayName>Identity Provider Access Token</DisplayName>
          <DataType>string</DataType>
          <AdminHelpText>Stores the access token of the identity provider.</AdminHelpText>
        </ClaimType>
        ...
      </ClaimsSchema>
    </BuildingBlocks>
    ```

2. Voeg het **output claim** -element toe aan het **TechnicalProfile** -element voor elke OAuth 2,0-ID-provider waarvoor u het toegangs token voor wilt gebruiken. In het volgende voor beeld ziet u het element dat is toegevoegd aan het technische profiel voor Facebook:

    ```XML
    <ClaimsProvider>
      <DisplayName>Facebook</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Facebook-OAUTH">
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="identityProviderAccessToken" PartnerClaimType="{oauth2:access_token}" />
          </OutputClaims>
          ...
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

3. Sla het bestand *TrustframeworkExtensions. XML* op.
4. Open uw Relying Party-beleids bestand, zoals *SignUpOrSignIn. XML*, en voeg het element **output claim** toe aan de **TechnicalProfile**:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
      <TechnicalProfile Id="PolicyProfile">
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="identityProviderAccessToken" PartnerClaimType="idp_access_token"/>
        </OutputClaims>
        ...
      </TechnicalProfile>
    </RelyingParty>
    ```

5. Sla het beleids bestand op.

## <a name="test-your-policy"></a>Uw beleid testen

Bij het testen van uw toepassingen in azure AD B2C kan het nuttig zijn om het Azure AD B2C-token te `https://jwt.ms` retour neren om de claims daarin te kunnen bekijken.

### <a name="upload-the-files"></a>De bestanden uploaden

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map gebruikt met uw Azure AD B2C-tenant door te klikken op het **Map- en abonnementsfilter** in het bovenste menu en de map te kiezen waarin uw tenant zich bevindt.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer een **Framework voor identiteits ervaring**.
5. Klik op het tabblad Aangepaste beleids regels op **beleid uploaden**.
6. Selecteer **het beleid overschrijven als dit bestaat**, en zoek en selecteer het bestand *TrustframeworkExtensions. XML* .
7. Klik op **Uploaden**.
8. Herhaal stap 5 tot en met 7 voor het Relying Party bestand, zoals *SignUpOrSignIn. XML*.

### <a name="run-the-policy"></a>Het beleid uitvoeren

1. Open het beleid dat u hebt gewijzigd. Bijvoorbeeld *B2C_1A_signup_signin*.
2. Selecteer voor **toepassing**de toepassing die u eerder hebt geregistreerd. Voor een overzicht van het token in het onderstaande voor beeld moet de antwoord `https://jwt.ms`- **URL** worden weer gegeven.
3. Klik op **Nu uitvoeren**.

    Het volgende voor beeld ziet er ongeveer uit:

    ![Gedecodeer token in jwt.ms met idp_access_token-blok gemarkeerd](./media/idp-pass-through-custom/idp-pass-through-custom-token.PNG)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over tokens in de [Azure Active Directory-token verwijzing](active-directory-b2c-reference-tokens.md).
