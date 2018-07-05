---
title: Toevoegen van een Azure AD-provider met behulp van ingebouwde beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Informatie over het toevoegen van een identiteitsprovider Open ID Connect (Azure AD).
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 70d3a19b715052fe658102929a1c29cf3db2d595
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443715"
---
# <a name="azure-active-directory-b2c-sign-in-using-azure-ad-accounts-through-a-built-in-policy"></a>Azure Active Directory B2C: Meld u aan met behulp van Azure AD-accounts via een ingebouwd beleid

>[!NOTE]
> Deze functie is beschikbaar als openbare preview. Gebruik de functie niet in een productieomgeving.

Dit artikel leest u hoe aanmelding voor gebruikers van een specifieke Azure Active Directory (Azure AD) organisatie ingebouwde beleidsregels inschakelen.

## <a name="create-an-azure-ad-app"></a>Een Azure AD-app maken

Om in te schakelen aanmelding voor gebruikers van een specifieke Azure AD-organisatie, moet u een toepassing registreren met de organisatie Azure AD-tenant.

>[!NOTE]
> We gebruiken 'contoso.com' voor de organisatie Azure AD-tenant en 'fabrikamb2c.onmicrosoft.com' als de Azure AD B2C-tenant in de volgende instructies.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer uw account op de bovenste balk. Uit de **Directory** Kies de organisatie Azure AD-tenant waar u uw toepassing (contoso.com) worden geregistreerd.
1. Selecteer **alle services** in het linkerdeelvenster en zoek naar 'App-registraties'.
1. Selecteer **Nieuwe toepassing registreren**.
1. Voer een naam in voor uw toepassing (bijvoorbeeld `Azure AD B2C App`).
1. Selecteer **Web-app / API** als toepassingstype.
1. Voor **aanmeldings-URL**, voer de volgende URL, waarbij `yourtenant` wordt vervangen door de naam van uw Azure AD B2C-tenant (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >De waarde voor 'yourtenant' moet alleen kleine letters bevatten in de **aanmeldings-URL**.

    ```Console
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Opslaan van de toepassings-ID, die u gebruiken in de volgende sectie als de client-ID gaat.
1. Onder de **instellingen** Selecteer **sleutels**.
1. Voer een **Sleutelbeschrijving** onder de **wachtwoorden** en stel de **duur** naar 'Verloopt nooit'. 
1. Klik op **opslaan**, en noteert u de resulterende sleutel **waarde**, die u in de volgende sectie gaat gebruiken als het clientgeheim.

## <a name="configure-azure-ad-as-an-identity-provider-in-your-tenant"></a>Azure AD als id-provider in uw tenant configureren

1. Selecteer uw account op de bovenste balk. Uit de **Directory** kiest u de Azure AD B2C-tenant (fabrikamb2c.onmicrosoft.com).
1. [Navigeer naar het menu Azure AD B2C-instellingen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in Azure portal.
1. Klik in het menu Azure AD B2C-instellingen op **id-providers**.
1. Klik op **+Toevoegen** boven aan de blade.
1. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'Contoso Azure AD'.
1. Klik op **type id-provider**, selecteer **Open ID Connect**, en klikt u op **OK**.
1. Klik op **deze id-provider instellen**
1. Voor **metagegevens-url**, voer de volgende URL, waarbij `yourtenant` wordt vervangen door de naam van uw Azure AD-tenant (bijvoorbeeld `contoso.com`):

    ```Console
    https://login.microsoftonline.com/yourtenant/.well-known/openid-configuration
    ```
1. Voor de **Client-ID** en **clientgeheim**, voer de toepassings-ID en sleutel van de vorige sectie.
1. Hou de standaardwaarde voor **bereik**, die moet worden ingesteld op `openid`.
1. Hou de standaardwaarde voor **antwoordtype**, die moet worden ingesteld op `code`.
1. Hou de standaardwaarde voor **antwoordmodus**, die moet worden ingesteld op `form_post`.
1. Geef eventueel een waarde voor **domein** (bijvoorbeeld `ContosoAD`). Dit is de waarde moet worden gebruikt als verwijzingen naar deze id-provider met behulp van *domain_hint* in de aanvraag. 
1. Klik op **OK**.
1. Klik op **deze id-provider claims toewijzen**.
1. Voor **gebruikers-ID**, voer `oid`.
1. Voor **weergavenaam**, voer `name`.
1. Voor **voornaam**, voer `given_name`.
1. Voor **achternaam**, voer `family_name`.
1. Voor **e**, invoeren `unique_name`
1. Klik op **OK**, en vervolgens **maken** aan uw configuratie op te slaan.

## <a name="next-steps"></a>Volgende stappen

Voeg de zojuist gemaakte Azure AD-id-provider aan een ingebouwde beleid en feedback te geven [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
