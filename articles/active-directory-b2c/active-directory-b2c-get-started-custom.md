---
title: Aan de slag met aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Hoe u aan de slag met Azure Active Directory B2C aangepast beleid.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 66ce0438e0cca62cbebb993be4940a93973c78d3
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078981"
---
# <a name="get-started-with-custom-policies-in-azure-active-directory-b2c"></a>Aan de slag met aangepaste beleidsregels in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

[Aangepaste beleidsregels](active-directory-b2c-overview-custom.md) zijn configuratiebestanden die het gedrag van uw Azure Active Directory (Azure AD) B2C-tenant definiëren. In dit artikel maakt u een aangepast beleid die ondersteuning biedt voor lokaal account registreren of aanmelden met behulp van een e-mailadres en wachtwoord. U ook voorbereiden uw omgeving voor het toevoegen van id-providers, zoals Facebook.

## <a name="prerequisites"></a>Vereisten

Als u er nog geen hebt, moet u [maken van een Azure AD B2C-tenant](tutorial-create-tenant.md) dat is gekoppeld aan uw Azure-abonnement.

## <a name="add-signing-and-encryption-keys"></a>Ondertekening en versleuteling sleutels toevoegen

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen. 
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer op de pagina overzicht **Identiteitsfunctie: PREVIEW**.

### <a name="create-the-signing-key"></a>De ondertekeningssleutel maken

1. Selecteer **Beleidssleutels** en selecteer vervolgens **toevoegen**.
2. Voor **opties**, kiest u `Generate`.
3. In **naam**, voer `TokenSigningKeyContainer`. Het voorvoegsel `B2C_1A_` mogelijk automatisch worden toegevoegd.
4. Voor **sleuteltype**, selecteer **RSA**.
5. Voor **sleutelgebruik**, selecteer **handtekening**.
6. Klik op **Create**.

### <a name="create-the-encryption-key"></a>De versleutelingssleutel maken

1. Selecteer **Beleidssleutels** en selecteer vervolgens **toevoegen**.
2. Voor **opties**, kiest u `Generate`.
3. In **naam**, voer `TokenEncryptionKeyContainer`. Het voorvoegsel `B2C_1A`_ mogelijk automatisch worden toegevoegd.
4. Voor **sleuteltype**, selecteer **RSA**.
5. Voor **sleutelgebruik**, selecteer **versleuteling**.
6. Klik op **Create**.

### <a name="create-the-facebook-key"></a>De Facebook-sleutel maken

Als u al een [Facebook toepassingsgeheim](active-directory-b2c-setup-fb-app.md), als een sleutel voor het toevoegen aan uw tenant. Anders moet u de sleutel maken met een tijdelijke aanduiding zodat het beleid gevalideerd.

1. Selecteer **Beleidssleutels** en selecteer vervolgens **toevoegen**.
2. Voor **opties**, kiest u `Manual`.
3. Voor **naam**, voer `FacebookSecret`. Het voorvoegsel `B2C_1A_` mogelijk automatisch worden toegevoegd.
4. In **geheim**, Geef uw Facebook-geheim van developers.facebook.com of `0` als tijdelijke aanduiding. Dit is het geheim, niet de toepassings-ID.
5. Voor **sleutelgebruik**, selecteer **handtekening**.
6. Klik op **Create**.

## <a name="register-an-application"></a>Een toepassing registreren

Een toepassing is in Azure Active Directory (Azure AD) B2C om in te schakelen van een gebruiker zich aanmelden en meld u aan met een lokaal account dat in uw tenant bestaat geregistreerd. Uw gebruikers zich aanmelden met een uniek e-mailadres en wachtwoord voor toegang tot de geregistreerde toepassing.

1. Kies **alle services** Zoek in de linkerbovenhoek van Azure portal en selecteer **App-registraties**.
2. Selecteer **Nieuwe toepassing registreren**.
3. Voor **naam**, voer `ProxyIdentityExperienceFramework`.
4. Voor **toepassingstype**, kiest u **systeemeigen**.
5. Voor **omleidings-URI**, voer `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com`, waarbij `your-tenant-name` is de naam van uw Azure AD B2C-tenant.
6. Klik op **Create**. Nadat deze gemaakt, kopieert u de toepassings-ID en sla deze voor later gebruik.
7. Selecteer **machtigingen verlenen**, en klik vervolgens om te bevestigen **Ja**.

## <a name="download-starter-pack-and-modify-policies"></a>Beginnerspakket downloaden en wijzigen van beleid

Aangepaste beleidsregels zijn een set XML-bestanden die moeten worden geüpload naar uw Azure AD B2C-tenant. Starter packs van de bestanden zijn bedoeld om u aan de slag snel. Elk pack starter in de volgende lijst bevat het kleinste aantal technische profielen en gebruiker reizen die nodig zijn voor het bereiken van de scenario's beschreven:

- LocalAccounts - kunt het gebruik van alleen lokale accounts.
- SocialAccounts - kunt het gebruik van sociale (of federatieve)-accounts.
- SocialAndLocalAccounts - kunt zowel het gebruik van lokale accounts en sociale accounts.
- SocialAndLocalAccountsWithMFA - Hiermee sociale netwerken, lokale en opties voor meervoudige verificatie.

Elk pack starter bevat:

- De base-bestand. Enkele wijzigingen zijn vereist voor het grondtal.
* Het extensiebestand.  Dit bestand is waar de meeste configuratiewijzigingen worden aangebracht.
* De relying party-bestanden. Taakspecifieke bestanden met de naam van uw toepassing.

>[!NOTE]
>Als uw XML-editor biedt ondersteuning voor validatie, controleert u de bestanden op basis van de TrustFrameworkPolicy_0.3.0.0.xsd XML-schema dat in de hoofdmap van het starter-pack bevindt zich. XML-schemavalidatie identificeert fouten voordat u uploadt.

1. [Download het ZIP-bestand](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) of uit te voeren:

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```

2. In de map SocialAndLocalAccounts alles van het vervangen van de bestanden bewerken `yourtenant` met de naam voor uw tenant. Bijvoorbeeld `contosoTenant.onmicrosoft.com`. Als u een XML-editor, moet [probeer Visual Studio Code](https://code.visualstudio.com/download), een lichtgewicht editor voor meerdere platforms.

### <a name="add-application-ids-to-the-custom-policy"></a>Toepassings-id's toevoegen aan het aangepaste beleid

De toepassings-ID toevoegen aan het extensiebestand *TrustFrameworkExtensions.xml*.

1. Open de *TrustFrameworkExtensions.xml* -bestand en zoek het element `<TechnicalProfile Id="login-NonInteractive">`.
2. Vervang beide de waarde van `client_id` en `resource_id` met toepassings-ID van de ProxyIdentityExperienceFramework-toepassing die u eerder hebt gemaakt.
3. Sla het extensiebestand.

## <a name="upload-the-policies"></a>Beleid uploaden

1. Selecteer op de pagina aangepast beleid van Identity-Ervaringsframework **uploaden beleid**.
1. In deze volgorde uploaden *TrustFrameworkBase.xml*, *TrustFrameworkExtensions.xml*, *SignUpOrSignin.xml*, *ProfileEdit.xml* , en *PasswordReset.xml*. Wanneer een bestand wordt geüpload, de naam van het beleidsbestand dat wordt voorafgegaan door `B2C_1A_`.

## <a name="test-the-custom-policy"></a>Het aangepaste beleid testen

1. Selecteer op de pagina aangepast beleid **B2C_1A_signup_signin**. 
2. Selecteer **nu uitvoeren**.

3. U zou het mogelijk om u te registreren met behulp van een e-mailadres.

4. Meld u aan met hetzelfde account om te bevestigen dat u de juiste configuratie hebt.

## <a name="add-facebook-as-an-identity-provider"></a>Facebook toevoegen als een id-provider

1. Configureer een [Facebook-toepassing](active-directory-b2c-setup-fb-app.md).
2. In de *TrustFrameworkExtensions.xml* bestand, vervang de waarde van `client_id` met Facebook toepassings-ID:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```
3. Upload de *TrustFrameworkExtensions.xml* bestand in uw tenant.
4. Test met behulp van **nu uitvoeren** of door het beleid rechtstreeks vanuit uw geregistreerde toepassing aan te roepen.

## <a name="next-steps"></a>Volgende stappen

- Azure Active Directory toevoegen als een id-provider. De base-bestand dat al eerder gebruikt in deze introductiehandleiding bevat enkele van de inhoud die u nodig hebt voor het toevoegen van andere id-providers. Zie voor meer informatie over het instellen van aanmeldingen bij de [instellen zich kunnen registreren en aanmelden met een Azure Active Directory-account met behulp van aangepaste beleidsregels van Active Directory B2C](active-directory-b2c-setup-aad-custom.md) artikel.
