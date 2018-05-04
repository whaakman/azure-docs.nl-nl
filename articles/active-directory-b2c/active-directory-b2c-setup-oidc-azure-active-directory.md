---
title: 'Azure Active Directory B2C: Een Azure AD-provider met behulp van ingebouwde beleid toevoegen | Microsoft Docs'
description: Informatie over het toevoegen van een identiteitsprovider Open ID Connect (Azure AD)
services: active-directory-b2c
documentationcenter: ''
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 7dac9545-d5f1-4136-a04d-1c5740aea499
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/27/2018
ms.author: parja
ms.openlocfilehash: 52a752df9cf199acf39596f49e7368bce27a8158
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2018
---
# <a name="azure-active-directory-b2c-sign-in-using-azure-ad-accounts-through-a-built-in-policy"></a>Azure Active Directory B2C: Aanmelden via Azure AD-accounts via een ingebouwde beleid

>[!NOTE]
> Deze functie is openbare preview. Gebruik de functie niet in een productieomgeving.

Dit artikel ziet u het inschakelen van aanmelden voor gebruikers van een specifieke ingebouwde beleidsregels van de Azure Active Directory (Azure AD)-organisatie.

## <a name="create-an-azure-ad-app"></a>Een Azure AD-app maken

Om in te schakelen aanmelden voor gebruikers met een specifiek Azure AD-organisatie, moet u een toepassing binnen de organisatie registreren Azure AD-tenant.

>[!NOTE]
> We gebruiken 'contoso.com' voor de organisatie Azure AD-tenant en 'fabrikamb2c.onmicrosoft.com' als de Azure AD B2C-tenant in de volgende instructies.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer uw account op de bovenste balk. Van de **Directory** Kies de organisatie waar u uw toepassing (contoso.com) worden geregistreerd voor Azure AD-tenant.
1. Selecteer **alle services** in het linkerdeelvenster, en zoek naar "App registraties."
1. Selecteer **Nieuwe toepassing registreren**.
1. Voer een naam voor uw toepassing (bijvoorbeeld `Azure AD B2C App`).
1. Selecteer **Web-app / API** als toepassingstype.
1. Voor **aanmeldings-URL**, voer de volgende URL waar `yourtenant` wordt vervangen door de naam van uw Azure AD B2C-tenant (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >De waarde voor 'yourtenant' moet alleen kleine letters in de **aanmeldings-URL**.

    ```Console
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Sla de ID die u in de volgende sectie gebruiken als de client-ID gaat.
1. Onder de **instellingen** blade Selecteer **sleutels**.
1. Voer een **sleutel beschrijving** onder de **wachtwoorden** en stel de **duur** naar 'Verloopt nooit'. 
1. Klik op **opslaan**, en noteer de resulterende sleutel **waarde**, die u in de volgende sectie worden gebruikt als het clientgeheim.

## <a name="configure-azure-ad-as-an-identity-provider-in-your-tenant"></a>Azure AD configureren als een id-provider in uw tenant

1. Selecteer uw account op de bovenste balk. Van de **Directory** kiest u de Azure AD B2C-tenant (fabrikamb2c.onmicrosoft.com).
1. [Navigeer naar het menu van Azure AD B2C instellingen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in de Azure portal.
1. Klik in het menu instellingen van Azure AD B2C op **identiteitsproviders**.
1. Klik op **+Toevoegen** boven aan de blade.
1. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'Contoso Azure AD'.
1. Klik op **identiteit providertype**, selecteer **Open ID Connect**, en klik op **OK**.
1. Klik op **instellen van deze id-provider**
1. Voor **metagegevens-url**, voer de volgende URL waar `yourtenant` wordt vervangen door de naam van uw Azure AD-tenant (bijvoorbeeld `contoso.com`):

    ```Console
    https://login.microsoftonline.com/yourtenant/.well-known/openid-configuration
    ```
1. Voor de **Client-ID** en **clientgeheim**, voer de toepassings-ID en sleutel van de vorige sectie.
1. Hou de standaardwaarde voor **bereik**, die moet worden ingesteld op `openid`.
1. Hou de standaardwaarde voor **antwoordtype**, die moet worden ingesteld op `code`.
1. Hou de standaardwaarde voor **antwoord modus**, die moet worden ingesteld op `form_post`.
1. U kunt desgewenst een waarde voor **domein** (bijvoorbeeld `ContosoAD`). Dit is de waarde moet worden gebruikt met betrekking tot deze id-provider met behulp van *domain_hint* in de aanvraag. 
1. Klik op **OK**.
1. Klik op **deze id-provider claims toewijzen**.
1. Voor **gebruikers-ID**, voer `oid`.
1. Voor **weergavenaam**, voer `name`.
1. Voor **voornaam**, voer `given_name`.
1. Voor **achternaam**, voer `family_name`.
1. Voor **e**, invoeren `unique_name`
1. Klik op **OK**, en vervolgens **maken** aan uw configuratie op te slaan.

## <a name="next-steps"></a>Volgende stappen

Voeg het zojuist gemaakte Azure AD-id-provider aan een ingebouwde beleid en feedback te geven [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
