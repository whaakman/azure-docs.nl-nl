---
title: 'Azure Active Directory B2C: Aan de slag met aangepast beleid | Microsoft Docs'
description: Hoe u aan de slag met Azure Active Directory B2C aangepast beleid
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja;parahk;gsacavdm
ms.openlocfilehash: 4f14dbf4b66f10290cd4f98d56a005f97cc6a207
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-get-started-with-custom-policies"></a>Azure Active Directory B2C: Aan de slag met aangepast beleid

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Nadat u de stappen in dit artikel hebt voltooid, het aangepaste beleid 'lokale account' ondersteunt registreren of aanmelden via een e-mailadres en wachtwoord. Ook bereidt u uw omgeving voor het toevoegen van de id-providers (zoals Facebook of Azure Active Directory). We raden u aan de volgende stappen uit voordat u lezen over andere maakt gebruik van Azure Active Directory (Azure AD) B2C identiteit ervaring Framework.

## <a name="prerequisites"></a>Vereisten

Voordat u doorgaat, zorg ervoor dat u hebt een Azure AD B2C-tenant een container voor alle gebruikers, apps en beleidsregels is. Als u nog geen een hebt, moet u [een Azure AD B2C-tenant maken](active-directory-b2c-get-started.md). We raden alle ontwikkelaars voltooien van de ingebouwde beleid Azure AD B2C-scenario's en hun toepassingen configureert met ingebouwde beleid voordat u doorgaat. Uw toepassingen werken met beide soorten beleid wanneer u een kleine wijziging aanbrengt in de naam van het beleid aan te roepen van het aangepaste beleid.

>[!NOTE]
>Als aangepast beleid bewerken, moet u een geldige Azure-abonnement is gekoppeld aan uw tenant. Als u nog niet hebt [uw Azure AD B2C-tenant gekoppeld aan een Azure-abonnement](active-directory-b2c-how-to-enable-billing.md) of uw Azure-abonnement is uitgeschakeld, wordt de knop identiteit ervaring Framework niet beschikbaar.

## <a name="add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies"></a>Ondertekening en versleuteling sleutels toevoegen aan uw B2C-tenant voor gebruik door het aangepaste beleid

1. Open de **identiteit ervaring Framework** blade in de instellingen van uw Azure AD B2C-tenant.
2. Selecteer **beleid sleutels** om de sleutels die beschikbaar zijn in uw tenant weer te geven.
3. Maak B2C_1A_TokenSigningKeyContainer als deze niet bestaat:<br>
    a. Selecteer **Toevoegen**. <br>
    b. Selecteer **genereren**.<br>
    c. Voor **naam**, gebruik `TokenSigningKeyContainer`. <br> 
    Het voorvoegsel `B2C_1A_` kunnen automatisch worden toegevoegd.<br>
    d. Voor **sleuteltype**, gebruik **RSA**.<br>
    e. Voor **datums**, gebruikt u de standaardinstellingen. <br>
    f. Voor **sleutelgebruik**, gebruik **handtekening**.<br>
    g. Selecteer **Maken**.<br>
4. Maak B2C_1A_TokenEncryptionKeyContainer als deze niet bestaat:<br>
 a. Selecteer **Toevoegen**.<br>
 b. Selecteer **genereren**.<br>
 c. Voor **naam**, gebruik `TokenEncryptionKeyContainer`. <br>
   Het voorvoegsel `B2C_1A`_ kunnen automatisch worden toegevoegd.<br>
 d. Voor **sleuteltype**, gebruik **RSA**.<br>
 e. Voor **datums**, gebruikt u de standaardinstellingen.<br>
 f. Voor **sleutelgebruik**, gebruik **versleuteling**.<br>
 g. Selecteer **Maken**.<br>
5. B2C_1A_FacebookSecret maken. <br>
Als u al een toepassingsgeheim Facebook, als de beleidssleutel van een toevoegen aan uw tenant. Anders moet u de sleutel maken met de aanduidingswaarde van een tijdelijke zodat het beleid gevalideerd.<br>
 a. Selecteer **Toevoegen**.<br>
 b. Voor **opties**, gebruik **handmatige**.<br>
 c. Voor **naam**, gebruik `FacebookSecret`. <br>
 Het voorvoegsel `B2C_1A_` kunnen automatisch worden toegevoegd.<br>
 d. In de **geheim** Voer uw FacebookSecret van developers.facebook.com of `0` als tijdelijke aanduiding. *Dit is geen uw Facebook-app-ID.* <br>
 e. Voor **sleutelgebruik**, gebruik **handtekening**. <br>
 f. Selecteer **maken** en Bevestig maken.

## <a name="register-identity-experience-framework-applications"></a>Identiteit ervaring Framework-toepassingen registreren

Azure AD B2C, moet u twee extra toepassingen die worden gebruikt door de engine voor het aanmelden en meld u aan gebruikers registreren.

>[!NOTE]
>U moet twee toepassingen maken die inschakelen aanmelden via lokale accounts: IdentityExperienceFramework (een web-app) en ProxyIdentityExperienceFramework (een systeemeigen app) met de machtiging van de app IdentityExperienceFramework overgedragen. Lokale accounts bestaan alleen in uw tenant. Uw gebruikers toegang krijgen tot uw toepassingen tenant is geregistreerd, moet u zich aanmelden bij een combinatie van uniek e-mailadres en wachtwoord.

### <a name="create-the-identityexperienceframework-application"></a>De toepassing IdentityExperienceFramework maken

1. In de [Azure-portal](https://portal.azure.com), schakel over naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md).
2. Open de **Azure Active Directory** blade (niet de **Azure AD B2C** blade). U wilt selecteren **meer Services** te vinden.
3. Selecteer **App registraties**.
4. Selecteer **registratie van de nieuwe toepassing**.
   * Voor **naam**, gebruik `IdentityExperienceFramework`.
   * Voor **toepassingstype**, gebruik **Web-app/API**.
   * Voor **aanmeldings-URL**, gebruik `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`, waarbij `yourtenant` is de domeinnaam van uw Azure AD B2C-tenant.
5. Selecteer **Maken**.
6. Zodra deze is gemaakt, selecteer de zojuist gemaakte toepassing **IdentityExperienceFramework**.<br>
   * Selecteer **eigenschappen**.<br>
   * De toepassings-ID kopiëren en opslaan voor later.

### <a name="create-the-proxyidentityexperienceframework-application"></a>De toepassing ProxyIdentityExperienceFramework maken

1. Selecteer **App registraties**.
1. Selecteer **registratie van de nieuwe toepassing**.
   * Voor **naam**, gebruik `ProxyIdentityExperienceFramework`.
   * Voor **toepassingstype**, gebruik **systeemeigen**.
   * Voor **omleidings-URI**, gebruik `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`, waarbij `yourtenant` is uw Azure AD B2C-tenant.
1. Selecteer **Maken**.
1. Zodra deze is gemaakt, selecteert u de toepassing **ProxyIdentityExperienceFramework**.<br>
   * Selecteer **eigenschappen**. <br>
   * De toepassings-ID kopiëren en opslaan voor later.
1. Selecteer **vereist machtigingen**.
1. Selecteer **Toevoegen**.
1. Selecteer **selecteert u een API**.
1. Zoek de naam IdentityExperienceFramework. Selecteer **IdentityExperienceFramework** in de resultaten en klik vervolgens op **Selecteer**.
1. Schakel het selectievakje in naast **toegang IdentityExperienceFramework**, en klik vervolgens op **Selecteer**.
1. Selecteer **gedaan**.
1. Selecteer **machtiging verlenen**, en bevestig vervolgens door te selecteren **Ja**.

## <a name="download-starter-pack-and-modify-policies"></a>Starter pack downloaden en wijzigen van beleid

Aangepaste beleidsregels zijn een reeks XML-bestanden die moeten worden geüpload naar uw Azure AD B2C-tenant. We bieden starter packs u kunt snel gaan. Elk starter pack in de volgende lijst bevat het kleinste aantal technische profielen en gebruiker trajecten die nodig zijn voor het bereiken van de scenario's beschreven:
 * LocalAccounts. Maakt het gebruik van alleen lokale accounts.
 * SocialAccounts. Maakt het gebruik van sociale (of federatieve)-accounts.
 * **SocialAndLocalAccounts**. We dit bestand gebruiken voor het scenario.
 * SocialAndLocalAccountsWithMFA. Opties voor sociale, lokaal en multi-Factor Authentication zijn hier opgenomen.

Elke pack starter bevat:

* De [base bestand](active-directory-b2c-overview-custom.md#policy-files) van het beleid. Enkele wijzigingen zijn vereist voor de base.
* De [extensiebestand](active-directory-b2c-overview-custom.md#policy-files) van het beleid.  Dit bestand is waar de meeste configuratiewijzigingen worden aangebracht.
* [Relying party bestanden](active-directory-b2c-overview-custom.md#policy-files) taakspecifieke bestanden aangeroepen door uw toepassing.

>[!NOTE]
>Als uw XML-editor validatie ondersteunt, de bestanden tegen het TrustFrameworkPolicy_0.3.0.0.xsd XML-schema dat in de hoofdmap van het starter pack bevindt zich te valideren. XML-schemavalidatie identificeert fouten voordat u uploadt.

 Aan de slag:

1. Download active-directory-b2c-custom-policy-starterpack vanuit GitHub. [Het ZIP-bestand downloaden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) of uit te voeren

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```
2. Open de map SocialAndLocalAccounts.  Base-bestand (TrustFrameworkBase.xml) in deze map bevat inhoud die nodig zijn voor zowel lokale als sociale/bedrijfsnetwerk accounts. De sociale inhoud niet van invloed op de stappen voor lokale accounts slag.
3. Open TrustFrameworkBase.xml. Als u een XML-editor moet [Visual Studio Code probeert](https://code.visualstudio.com/download), een lichtgewicht platformoverschrijdende-editor.
4. In de hoofdmap `TrustFrameworkPolicy` element, update de `TenantId` en `PublicPolicyUri` kenmerken, vervangen `yourtenant.onmicrosoft.com` met de domeinnaam van uw Azure AD B2C-tenant:
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
   >`PolicyId`is de naam van het beleid dat wordt weergegeven in de portal en de naam waarmee in dit bestand wordt verwezen door andere beleidsbestanden.

5. Sla het bestand op.
6. Open TrustFrameworkExtensions.xml. Dezelfde twee wijzigingen aanbrengen door te vervangen `yourtenant.onmicrosoft.com` met uw Azure AD B2C-tenant. Controleer de dezelfde vervanging in de `<TenantId>` element voor een totaal van drie wijzigingen. Sla het bestand op.
7. Open SignUpOrSignIn.xml. Breng de dezelfde wijzigingen door te vervangen `yourtenant.onmicrosoft.com` met uw Azure AD B2C-tenant op drie locaties. Sla het bestand op.
8. Open het opnieuw instellen van het wachtwoord en profiel bewerken bestanden. Breng de dezelfde wijzigingen door te vervangen `yourtenant.onmicrosoft.com` met uw Azure AD B2C-tenant op drie locaties in elk bestand. Sla de bestanden.

### <a name="add-the-application-ids-to-your-custom-policy"></a>De toepassings-id's toevoegen aan het aangepaste beleid
De toepassings-id's toevoegen aan het extensiebestand (`TrustFrameworkExtensions.xml`):

1. Zoek het element in het extensiebestand (TrustFrameworkExtensions.xml) `<TechnicalProfile Id="login-NonInteractive">`.
2. Vervang beide exemplaren van `IdentityExperienceFrameworkAppId` met de toepassings-ID van de identiteit ervaring Framework-toepassing die u eerder hebt gemaakt. Hier volgt een voorbeeld:

   ```xml
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```
3. Vervang beide exemplaren van `ProxyIdentityExperienceFrameworkAppId` met de toepassings-ID van de Proxy identiteit ervaring Framework-toepassing die u eerder hebt gemaakt.
4. Sla het bestand uitbreidingen.

## <a name="upload-the-policies-to-your-tenant"></a>Het beleid uploaden naar uw tenant

1. In de [Azure-portal](https://portal.azure.com), schakel over naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open de **Azure AD B2C** blade.
2. Selecteer **identiteit ervaring Framework**.
3. Selecteer **uploaden beleid**.

    >[!WARNING]
    >De bestanden aangepast beleid moeten worden geüpload in de volgende volgorde:

1. TrustFrameworkBase.xml uploaden.
2. TrustFrameworkExtensions.xml uploaden.
3. SignUpOrSignin.xml uploaden.
4. Upload uw beleidsbestanden.

Wanneer een bestand is geüpload, wordt de naam van het beleidsbestand voorafgegaan door `B2C_1A_`.

## <a name="test-the-custom-policy-by-using-run-now"></a>Het aangepaste beleid testen met behulp van nu uitvoeren

1. Open **Azure AD B2C-instellingen** en Ga naar **identiteit ervaring Framework**.

   >[!NOTE]
   >**Nu uitvoeren** vereist ten minste één toepassing om te worden preregistered op de tenant. Toepassingen moeten worden geregistreerd in de B2C-tenant, door de **toepassingen** menuselectie in Azure AD B2C of met behulp van het kader van de gebruikerservaring identiteit aan te roepen ingebouwde en aangepaste beleidsregels. Er is slechts één registratie per toepassing nodig.<br><br>
   Zie voor informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.  

2. Open B2C_1A_signup_signin, de relying party (RP) aangepast-beleid die u hebt geüpload. Selecteer **nu uitvoeren**.

3. U moet mogelijk aan te melden met behulp van een e-mailadres.

4. Aanmelden met hetzelfde account om te bevestigen dat u de juiste configuratie.

>[!NOTE]
>Een veelvoorkomende oorzaak van de aanmelding mislukt is een onjuist geconfigureerde IdentityExperienceFramework-app.


## <a name="next-steps"></a>Volgende stappen

### <a name="add-facebook-as-an-identity-provider"></a>Facebook toevoegen als een id-provider
Facebook instellen:
1. [Configureer een Facebook-toepassing in developers.facebook.com](active-directory-b2c-setup-fb-app.md).
2. [Het geheim Facebook-toepassing toevoegen aan uw Azure AD B2C-tenant](#add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies).
3. Vervang de waarde van in het beleidsbestand TrustFrameworkExtensions `client_id` met de Facebook toepassings-ID:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```
4. Upload het beleidsbestand TrustFrameworkExtensions.xml naar uw tenant.
5. Testen met behulp van **nu uitvoeren** of door het beleid rechtstreeks vanuit uw geregistreerde toepassing wordt aangeroepen.

### <a name="add-azure-active-directory-as-an-identity-provider"></a>Azure Active Directory toevoegen als een id-provider
De base-bestand al gebruikt in deze introductiehandleiding bevat enkele van de inhoud die u nodig hebt voor het toevoegen van andere id-providers. Zie voor meer informatie over het instellen van aanmeldingen de [Azure Active Directory B2C: aanmelden met Azure AD-accounts](active-directory-b2c-setup-aad-custom.md) artikel.

Zie voor een overzicht van aangepaste beleidsregels in Azure AD B2C die gebruikmaken van de identiteit ervaring Framework de [Azure Active Directory B2C: aangepast beleid](active-directory-b2c-overview-custom.md) artikel. 
