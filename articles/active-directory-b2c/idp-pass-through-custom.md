---
title: Een toegangstoken via een aangepast beleid doorgeven aan uw toepassing in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over hoe u kunt doorgeven een toegangstoken voor OAuth 2.0-id-providers als een claim via een aangepast beleid aan uw toepassing in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 784387b119bff6445015419adfd3bc0e52eee43f
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58402640"
---
# <a name="pass-an-access-token-through-a-custom-policy-to-your-application-in-azure-active-directory-b2c"></a>Een toegangstoken via een aangepast beleid doorgeven aan uw toepassing in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Een [aangepast beleid](active-directory-b2c-get-started-custom.md) in Azure Active Directory (Azure AD) B2C biedt gebruikers van uw toepassing de mogelijkheid om te registreren of aanmelden met een id-provider. Als dit gebeurt, Azure AD B2C ontvangt een [toegangstoken](active-directory-b2c-reference-tokens.md) van de id-provider. Azure AD B2C gebruikt dit token voor het ophalen van informatie over de gebruiker. U voegt een claimtype en de uitvoer claim naar het aangepaste beleid om het token door naar de toepassingen die u in Azure AD B2C registreert toe. 

Azure AD B2C biedt ondersteuning voor het doorgeven van het toegangstoken van [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) en [OpenID Connect](active-directory-b2c-reference-oidc.md) id-providers. Voor alle andere id-providers, wordt de claim leeg geretourneerd.

## <a name="prerequisites"></a>Vereisten

- Het aangepaste beleid is geconfigureerd met een id-provider van OAuth 2.0 of OpenID Connect.

## <a name="add-the-claim-elements"></a>De claimelementen toevoegen 

1. Open uw *TrustframeworkExtensions.xml* -bestand en voeg de volgende **ClaimType** element met een id van `identityProviderAccessToken` naar de **ClaimsSchema** element:

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

2. Voeg de **OutputClaim** element dat u wilt de **TechnicalProfile** -element voor elk OAuth 2.0-id-provider die u wilt dat het toegangstoken voor. Het volgende voorbeeld ziet u het element toegevoegd aan het technische profiel Facebook:

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

3. Sla de *TrustframeworkExtensions.xml* bestand.
4. Open uw relying party beleid-bestand, zoals *SignUpOrSignIn.xml*, en voeg de **OutputClaim** element op de **TechnicalProfile**:

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

5. Sla het beleidsbestand.

## <a name="test-your-policy"></a>Het beleid testen

Bij het testen van uw toepassingen in Azure AD B2C, kan het nuttig zijn om de Azure AD B2C-token dat is geretourneerd naar `https://jwt.ms` om te kunnen controleren van de claims in het.

### <a name="upload-the-files"></a>De bestanden uploaden

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map gebruikt met uw Azure AD B2C-tenant door te klikken op het **Map- en abonnementsfilter** in het bovenste menu en de map te kiezen waarin uw tenant zich bevindt.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **Identity-Ervaringsframework**.
5. Klik op de pagina aangepast beleid **uploaden beleid**.
6. Selecteer **het beleid overschrijven als deze bestaat**, en zoek en selecteer de *TrustframeworkExtensions.xml* bestand.
7. Klik op **Uploaden**.
8. Herhaal de stappen 5 tot en met 7 voor de relying party-bestand, zoals *SignUpOrSignIn.xml*.

### <a name="run-the-policy"></a>Het beleid uitvoeren

1. Open het beleid dat u hebt gewijzigd. Bijvoorbeeld, *B2C_1A_signup_signin*.
2. Voor **toepassing**, selecteer uw toepassing die u eerder hebt geregistreerd. Om te zien van het token in het onderstaande voorbeeld de **antwoord-URL** moet worden weergegeven `https://jwt.ms`.
3. Klik op **Nu uitvoeren**.

    U ziet er ongeveer uitzien als in het volgende voorbeeld:

    ![Gedecodeerde token](./media/idp-pass-through-custom/idp-pass-through-custom-token.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de tokens in de [naslaginformatie over Azure Active Directory-tokens](active-directory-b2c-reference-tokens.md).





