---
title: Aan de slag met aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Hoe u aan de slag met Azure Active Directory B2C aangepast beleid.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 36fad697758273246d567dfa1010f0e6bfc68939
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344559"
---
# <a name="azure-active-directory-b2c-get-started-with-custom-policies"></a>Azure Active Directory B2C: Aan de slag met aangepaste beleidsregels

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Nadat u de stappen in dit artikel 'lokale account' biedt ondersteuning voor het aangepaste beleid registreren of aanmelden via een e-mailadres en wachtwoord. Bereidt u ook uw omgeving voor het toevoegen van id-providers (zoals Facebook of Azure Active Directory). We raden u aan deze stappen voordat u lezen over andere maakt gebruik van de Azure Active Directory (Azure AD) B2C Identiteitservaring-Framework.

## <a name="prerequisites"></a>Vereisten

Voordat u doorgaat, zorg ervoor dat u hebt een Azure AD B2C-tenant een container voor al uw gebruikers, apps en beleidsregels is. Als u er nog geen hebt, moet u [maken van een Azure AD B2C-tenant](active-directory-b2c-get-started.md). We raden ten zeerste aan alle ontwikkelaars het voltooien van de ingebouwde beleid voor Azure AD B2C-scenario's en hun toepassingen configureren met ingebouwde beleidsregels voordat u doorgaat. Uw toepassingen werkt met beide soorten beleid nadat u een kleine wijziging aanbrengt in de naam van het beleid om aan te roepen van het aangepaste beleid.

>[!NOTE]
>Voor toegang tot aangepaste beleid bewerkt, moet u een geldig Azure-abonnement gekoppeld aan uw tenant. Als u nog niet hebt [uw Azure AD B2C-tenant gekoppeld aan een Azure-abonnement](active-directory-b2c-how-to-enable-billing.md) of uw Azure-abonnement is uitgeschakeld, wordt de knop Identiteitservaring-Framework niet beschikbaar.

## <a name="add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies"></a>Ondertekening en versleuteling sleutels toevoegen aan uw B2C-tenant voor gebruik door het aangepaste beleid

1. Open de **Identity-Ervaringsframework** blade in de instellingen van uw Azure AD B2C-tenant.
2. Selecteer **Beleidssleutels** om de sleutels die beschikbaar zijn in uw tenant weer te geven.
3. Maak B2C_1A_TokenSigningKeyContainer als deze niet bestaat:<br>
    a. Selecteer **Toevoegen**. <br>
    b. Selecteer **genereren**.<br>
    c. Voor **naam**, gebruikt u `TokenSigningKeyContainer`. <br> 
    Het voorvoegsel `B2C_1A_` mogelijk automatisch worden toegevoegd.<br>
    d. Voor **sleuteltype**, gebruikt u **RSA**.<br>
    e. Voor **datums**, gebruik de standaardinstellingen. <br>
    f. Voor **sleutelgebruik**, gebruikt u **handtekening**.<br>
    g. Selecteer **Maken**.<br>
4. Maak B2C_1A_TokenEncryptionKeyContainer als deze niet bestaat:<br>
 a. Selecteer **Toevoegen**.<br>
 b. Selecteer **genereren**.<br>
 c. Voor **naam**, gebruikt u `TokenEncryptionKeyContainer`. <br>
   Het voorvoegsel `B2C_1A`_ mogelijk automatisch worden toegevoegd.<br>
 d. Voor **sleuteltype**, gebruikt u **RSA**.<br>
 e. Voor **datums**, gebruik de standaardinstellingen.<br>
 f. Voor **sleutelgebruik**, gebruikt u **versleuteling**.<br>
 g. Selecteer **Maken**.<br>
5. B2C_1A_FacebookSecret maken. <br>
Als u al een toepassingsgeheim Facebook, als een sleutel voor het toevoegen aan uw tenant. Anders moet u de sleutel maken met een tijdelijke aanduiding zodat het beleid gevalideerd.<br>
 a. Selecteer **Toevoegen**.<br>
 b. Voor **opties**, gebruikt u **handmatig**.<br>
 c. Voor **naam**, gebruikt u `FacebookSecret`. <br>
 Het voorvoegsel `B2C_1A_` mogelijk automatisch worden toegevoegd.<br>
 d. In de **geheim** voert u uw FacebookSecret van developers.facebook.com of `0` als tijdelijke aanduiding. *Dit is niet uw Facebook-app-ID.* <br>
 e. Voor **sleutelgebruik**, gebruikt u **handtekening**. <br>
 f. Selecteer **maken** en controleer of het maken.

## <a name="register-identity-experience-framework-applications"></a>Identiteitservaring-Framework-toepassingen registreren

Azure AD B2C, moet u het registreren van twee extra toepassingen die worden gebruikt door de engine voor het aanmelden en meld u aan gebruikers.

>[!NOTE]
>U moet twee toepassingen maken die inschakelen aanmelden met behulp van de lokale accounts: IdentityExperienceFramework (een web-app) en ProxyIdentityExperienceFramework (een systeemeigen app) met toestemming van de app IdentityExperienceFramework overgedragen. Lokale accounts bestaat alleen in uw tenant. Uw gebruikers zich aanmelden met een combinatie van unieke e-mailadres en wachtwoord voor toegang tot uw toepassingen tenant is geregistreerd.

### <a name="create-the-identityexperienceframework-application"></a>De IdentityExperienceFramework-toepassing maken

1. In de [Azure-portal](https://portal.azure.com), schakel over naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md).
2. Open de **Azure Active Directory** blade (niet de **Azure AD B2C** blade). Mogelijk moet u selecteren **meer Services** te vinden.
3. Selecteer **App-registraties**.
4. Selecteer **Nieuwe toepassing registreren**.
   * Voor **naam**, gebruikt u `IdentityExperienceFramework`.
   * Voor **toepassingstype**, gebruikt u **Web-app/API**.
   * Voor **aanmeldings-URL**, gebruikt u `https://yourtenant.b2clogin.com/yourtenant.onmicrosoft.com`, waarbij `yourtenant` is de domeinnaam van uw Azure AD B2C-tenant.
5. Selecteer **Maken**.
6. Zodra deze is gemaakt, selecteert u de zojuist gemaakte toepassing **IdentityExperienceFramework**.<br>
   * Selecteer **eigenschappen**.<br>
   * Kopieer de toepassings-ID en bewaar het voor later.

### <a name="create-the-proxyidentityexperienceframework-application"></a>De ProxyIdentityExperienceFramework-toepassing maken

1. Selecteer **App-registraties**.
1. Selecteer **Nieuwe toepassing registreren**.
   * Voor **naam**, gebruikt u `ProxyIdentityExperienceFramework`.
   * Voor **toepassingstype**, gebruikt u **systeemeigen**.
   * Voor **omleidings-URI**, gebruikt u `https://yourtenant.b2clogin.com/yourtenant.onmicrosoft.com`, waarbij `yourtenant` is uw Azure AD B2C-tenant.
1. Selecteer **Maken**.
1. Zodra deze is gemaakt, selecteert u de toepassing **ProxyIdentityExperienceFramework**.<br>
   * Selecteer **eigenschappen**. <br>
   * Kopieer de toepassings-ID en bewaar het voor later.
1. Selecteer **vereiste machtigingen**.
1. Selecteer **Toevoegen**.
1. Selecteer **Een API selecteren**.
1. Zoeken naar de naam IdentityExperienceFramework. Selecteer **IdentityExperienceFramework** in de resultaten en klik vervolgens op **Selecteer**.
1. Schakel het selectievakje in naast **toegang IdentityExperienceFramework**, en klik vervolgens op **Selecteer**.
1. Selecteer **Done**.
1. Selecteer **machtigingen verlenen**, en klik vervolgens om te bevestigen **Ja**.

## <a name="download-starter-pack-and-modify-policies"></a>Beginnerspakket downloaden en wijzigen van beleid

Aangepaste beleidsregels zijn een set XML-bestanden die moeten worden geüpload naar uw Azure AD B2C-tenant. We bieden starter packs om u snel de slag. Elk pack starter in de volgende lijst bevat het kleinste aantal technische profielen en gebruiker reizen die nodig zijn voor het bereiken van de scenario's beschreven:
 * LocalAccounts. Hiermee kunt het gebruik van alleen lokale accounts.
 * SocialAccounts. Hiermee kunt het gebruik van sociale (of federatieve)-accounts.
 * **SocialAndLocalAccounts**. We gebruiken dit bestand voor het scenario.
 * SocialAndLocalAccountsWithMFA. Opties voor sociale netwerken, lokale en multi-Factor Authentication zijn hier opgenomen.

Elk pack starter bevat:

* De [base bestand](active-directory-b2c-overview-custom.md#policy-files) van het beleid. Enkele wijzigingen zijn vereist voor het grondtal.
* De [extensiebestand](active-directory-b2c-overview-custom.md#policy-files) van het beleid.  Dit bestand is waar de meeste configuratiewijzigingen worden aangebracht.
* [Relying party bestanden](active-directory-b2c-overview-custom.md#policy-files) taakspecifieke bestanden met de naam van uw toepassing.

>[!NOTE]
>Als uw XML-editor biedt ondersteuning voor validatie, controleert u de bestanden op basis van de TrustFrameworkPolicy_0.3.0.0.xsd XML-schema dat in de hoofdmap van het starter-pack bevindt zich. XML-schemavalidatie identificeert fouten voordat u uploadt.

 Laten we beginnen:

1. Download active-directory-b2c-custom-policy-starterpack vanuit GitHub. [Download het ZIP-bestand](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) of uit te voeren

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```
2. Open de map SocialAndLocalAccounts.  De base-bestand (TrustFrameworkBase.xml) in deze map bevat inhoud die nodig zijn voor zowel lokale als sociale/zakelijke accounts. De sociale Media-inhoud niet van invloed op de stappen voor het ophalen van lokale accounts van en uitvoeren.
3. Open TrustFrameworkBase.xml. Als u een XML-editor, moet [probeer Visual Studio Code](https://code.visualstudio.com/download), een lichtgewicht editor voor meerdere platforms.
4. In de hoofdmap `TrustFrameworkPolicy` element, update de `TenantId` en `PublicPolicyUri` kenmerken, vervangen `yourtenant.onmicrosoft.com` met de naam van het domein van uw Azure AD B2C-tenant:
   ```xml
    <TrustFrameworkPolicy
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
    PolicySchemaVersion="0.3.0.0"
    TenantId="yourtenant.onmicrosoft.com"
    PolicyId="B2C_1A_TrustFrameworkBase"
    PublicPolicyUri="http://yourtenant.onmicrosoft.com">
    ```
   >[!NOTE]
   >`PolicyId` is de naam van het beleid dat wordt weergegeven in de portal en de naam waarop dit beleidsbestand wordt verwezen door andere beleidsbestanden.

5. Sla het bestand op.
6. Open TrustFrameworkExtensions.xml. Breng de dezelfde twee wijzigingen door te vervangen `yourtenant.onmicrosoft.com` met uw Azure AD B2C-tenant. De dezelfde vervanging in de `<TenantId>` -element voor een totaal van drie wijzigingen. Sla het bestand op.
7. Open SignUpOrSignIn.xml. Breng de dezelfde wijzigingen door te vervangen `yourtenant.onmicrosoft.com` met uw Azure AD B2C-tenant op drie locaties. Sla het bestand op.
8. Open het wachtwoord opnieuw instellen en het profiel bewerken bestanden. Breng de dezelfde wijzigingen door te vervangen `yourtenant.onmicrosoft.com` met uw Azure AD B2C-tenant op drie locaties in elk bestand. Sla de bestanden.

### <a name="add-the-application-ids-to-your-custom-policy"></a>De toepassings-id's toevoegen aan uw aangepaste beleid
De toepassings-id's toevoegen aan het extensiebestand (`TrustFrameworkExtensions.xml`):

1. Zoek het element in het extensiebestand (TrustFrameworkExtensions.xml) `<TechnicalProfile Id="login-NonInteractive">`.
2. Vervang beide exemplaren van `IdentityExperienceFrameworkAppId` met toepassings-ID van de Identiteitservaring-Framework-toepassing die u eerder hebt gemaakt. Hier volgt een voorbeeld:

   ```xml
   <Item Key="IdTokenAudience">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```
3. Vervang beide exemplaren van `ProxyIdentityExperienceFrameworkAppId` met toepassings-ID van de Proxy Identiteitservaring-Framework-toepassing die u eerder hebt gemaakt.
4. Sla het extensiebestand.

## <a name="upload-the-policies-to-your-tenant"></a>Het beleid voor uploaden naar uw tenant

1. In de [Azure-portal](https://portal.azure.com), schakel over naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open de **Azure AD B2C** blade.
2. Selecteer **Identity-Ervaringsframework**.
3. Selecteer **beleid uploaden**.

    >[!WARNING]
    >Het aangepaste beleidsbestanden moeten worden geüpload in de volgende volgorde:

1. TrustFrameworkBase.xml uploaden.
2. TrustFrameworkExtensions.xml uploaden.
3. SignUpOrSignin.xml uploaden.
4. Upload uw andere beleidsbestanden.

Wanneer een bestand wordt geüpload, de naam van het beleidsbestand dat wordt voorafgegaan door `B2C_1A_`.

## <a name="test-the-custom-policy-by-using-run-now"></a>Het aangepaste beleid testen met behulp van nu uitvoeren

1. Open **Azure AD B2C-instellingen** en Ga naar **Identity-Ervaringsframework**.

   >[!NOTE]
   >**Nu uitvoeren** vereist ten minste één toepassing vooraf op de tenant worden geregistreerd. Toepassingen moeten worden geregistreerd in de B2C-tenant, door de **toepassingen** menu-optie in Azure AD B2C of met behulp van de Identiteitservaring-Framework om aan te roepen ingebouwde en aangepaste beleidsregels. Er is slechts één registratie per toepassing nodig.<br><br>
   Zie voor meer informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.  

2. Open B2C_1A_signup_signin, de relying party (RP) aangepast-beleid dat u hebt geüpload. Selecteer **nu uitvoeren**.

3. U zou het mogelijk om u te registreren met behulp van een e-mailadres.

4. Meld u aan met hetzelfde account om te bevestigen dat u de juiste configuratie hebt.

>[!NOTE]
>Een veelvoorkomende oorzaak van de aanmelding mislukt is een onjuist geconfigureerde IdentityExperienceFramework-app.


## <a name="next-steps"></a>Volgende stappen

### <a name="add-facebook-as-an-identity-provider"></a>Facebook toevoegen als een id-provider
Facebook instellen:
1. [Configureren van een Facebook-toepassing in developers.facebook.com](active-directory-b2c-setup-fb-app.md).
2. [De Facebook-toepassing-geheim toevoegen aan uw Azure AD B2C-tenant](#add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies).
3. Vervang de waarde van in het beleidsbestand TrustFrameworkExtensions `client_id` met Facebook toepassings-ID:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```
4. Het TrustFrameworkExtensions.xml beleid-bestand uploaden naar uw tenant.
5. Test met behulp van **nu uitvoeren** of door het beleid rechtstreeks vanuit uw geregistreerde toepassing aan te roepen.

### <a name="add-azure-active-directory-as-an-identity-provider"></a>Azure Active Directory als id-provider toevoegen
De base-bestand dat al eerder gebruikt in deze introductiehandleiding bevat enkele van de inhoud die u nodig hebt voor het toevoegen van andere id-providers. Zie voor meer informatie over het instellen van aanmeldingen bij de [Azure Active Directory B2C: Meld u aan met behulp van Azure AD-accounts](active-directory-b2c-setup-aad-custom.md) artikel.

Zie voor een overzicht van aangepaste beleidsregels in Azure AD B2C die gebruikmaken van de Identiteitservaring-Framework, de [Azure Active Directory B2C: aangepast beleid](active-directory-b2c-overview-custom.md) artikel. 
