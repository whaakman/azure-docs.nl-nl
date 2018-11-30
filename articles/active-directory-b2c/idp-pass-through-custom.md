---
title: Een toegangstoken via een aangepast beleid doorgeven aan uw toepassing in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over hoe u kunt doorgeven een toegangstoken voor OAuth 2.0-id-providers als een claim via een aangepast beleid aan uw toepassing in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e32e62a050b54334a37b09f001798b9a62e2e6b5
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52642373"
---
# <a name="pass-an-access-token-through-a-custom-policy-to-your-application-in-azure-active-directory-b2c"></a>Een toegangstoken via een aangepast beleid doorgeven aan uw toepassing in Azure Active Directory B2C

> [!NOTE]
> Deze functie is momenteel in openbare preview.

Een [aangepast beleid](active-directory-b2c-get-started-custom.md) in Azure Active Directory (Azure AD) B2C biedt gebruikers van uw toepassing de mogelijkheid om te registreren of aanmelden met een id-provider. Als dit gebeurt, Azure AD B2C ontvangt een [toegangstoken](active-directory-b2c-reference-tokens.md) van de id-provider. Azure AD B2C gebruikt dit token voor het ophalen van informatie over de gebruiker. U voegt een claimtype en de uitvoer claim naar het aangepaste beleid om het token door naar de toepassingen die u in Azure AD B2C registreert toe. 

Azure AD B2C ondersteunt momenteel alleen het toegangstoken van doorgeven [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) id-providers, waaronder Facebook en [Google](active-directory-b2c-custom-setup-goog-idp.md). Voor alle andere id-providers, wordt de claim leeg geretourneerd.

## <a name="prerequisites"></a>Vereisten

- Het aangepaste beleid is geconfigureerd met een OAuth 2.0-id-provider.

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
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
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





