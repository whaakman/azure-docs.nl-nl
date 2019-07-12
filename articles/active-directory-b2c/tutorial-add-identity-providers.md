---
title: Zelfstudie - id-providers toevoegen aan uw toepassingen - Azure Active Directory B2C
description: Informatie over het id-providers toevoegen aan uw toepassingen in Azure Active Directory B2C met behulp van de Azure portal.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 07/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 33f595dd36ac9448cc1276647f9943326b0b74c1
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67655224"
---
# <a name="tutorial-add-identity-providers-to-your-applications-in-azure-active-directory-b2c"></a>Zelfstudie: Id-providers toevoegen aan uw toepassingen in Azure Active Directory B2C

Het is wellicht wenselijk dat uw gebruikers zich in uw toepassingen bij verschillende id-providers kunnen aanmelden. Een *id-provider* maakt, onderhoudt en beheert id-gegevens en biedt tegelijkertijd verificatieservices voor toepassingen. U kunt toevoegen id-providers die worden ondersteund door Azure Active Directory (Azure AD) B2C aan uw [gebruikersstromen](active-directory-b2c-reference-policies.md) met behulp van de Azure portal.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * De identiteit van de provider-toepassingen maken
> * De id-providers toevoegen aan uw tenant
> * De id-providers toevoegen aan de gebruikersstroom

Doorgaans gebruikt u slechts één id-provider in uw toepassingen, maar u hebt de mogelijkheid om toe te voegen meer. Deze zelfstudie leert u hoe u een Azure AD-id-provider en een Facebook-id-provider toevoegt aan uw toepassing. Beide van deze id-providers toevoegen aan uw toepassing is optioneel. U kunt ook andere id-providers, zoals toevoegen [Amazon](active-directory-b2c-setup-amzn-app.md), [GitHub](active-directory-b2c-setup-github-app.md), [Google](active-directory-b2c-setup-goog-app.md), [LinkedIn](active-directory-b2c-setup-li-app.md), [Microsoft](active-directory-b2c-setup-msa-app.md), of [Twitter](active-directory-b2c-setup-twitter-app.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

[Een gebruikersstroom maken](tutorial-create-user-flows.md) zodat gebruikers kunnen zich registreren en aanmelden bij uw toepassing.

## <a name="create-applications"></a>Toepassingen maken

Id-provider toepassingen bieden de id en de sleutel voor het inschakelen van de communicatie met uw Azure AD B2C-tenant. In deze sectie van de zelfstudie maakt u een Azure AD-toepassing en een Facebook-toepassing van waaruit u profiteert van id's en sleutels de id-providers toevoegen aan uw tenant. Als u slechts een van de id-providers toevoegen wilt, moet u alleen de toepassing voor die provider maken.

### <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

Om in te schakelen aanmelding voor gebruikers van Azure AD, moet u een toepassing registreren met de Azure AD-tenant. De Azure AD-tenant is niet gelijk zijn aan uw Azure AD B2C-tenant.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zorg ervoor dat u de map met uw Azure AD-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map met uw Azure AD-tenant te kiezen.
1. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **App-registraties**.
1. Selecteer **registratie van nieuwe**.
1. Voer een naam in voor de toepassing. Bijvoorbeeld `Azure AD B2C App`.
1. Accepteer de selectie van **Accounts in deze organisatie-map alleen** voor deze toepassing.
1. Voor de **omleidings-URI**, accepteert u de waarde van **Web** en voer de volgende URL in alle kleine letters, vervangen `your-B2C-tenant-name` met de naam van uw Azure AD B2C-tenant.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Bijvoorbeeld `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.

    Alle URL's moeten nu gebruikmaken van [b2clogin.com](b2clogin.md).

1. Selecteer **registreren**, klikt u vervolgens record de **(client) toepassings-ID** die u in een latere stap.
1. Onder **beheren** selecteren in het toepassingsmenu **certificaten en geheimen**en selecteer vervolgens **nieuwe clientgeheim**.
1. Voer een **beschrijving** voor het clientgeheim. Bijvoorbeeld `Azure AD B2C App Secret`.
1. Selecteer de verloopperiode. Voor deze toepassing, accepteert u de selectie van **In 1 jaar**.
1. Selecteer **toevoegen**, noteer vervolgens de waarde van het nieuwe clientgeheim die u gebruiken in een latere stap.

### <a name="create-a-facebook-application"></a>Een Facebook-toepassing maken

Voor het gebruik van een Facebook-account als id-provider in Azure AD B2C, moet u een toepassing maakt op Facebook. Als u nog een Facebook-account hebt, kunt u krijgen via [ https://www.facebook.com/ ](https://www.facebook.com/).

1. Aanmelden bij [Facebook voor ontwikkelaars](https://developers.facebook.com/) met de referenties van uw Facebook-account.
1. Als u dit nog niet hebt gedaan, moet u registreren als een Facebook-ontwikkelaar. Om dit te doen, selecteert u **aan de slag** in de rechterbovenhoek van de pagina accepteren van Facebook-beleid en de registratie van stappen.
1. Selecteer **mijn Apps** en vervolgens **-App maken**.
1. Voer een **weergavenaam** en een geldig **Neem contact op met e-mailadres**.
1. Klik op **maken van App-ID**. Mogelijk moet u beleid voor het platform van Facebook accepteren en een online beveiliging voltooien.
1. Selecteer **instellingen** > **Basic**.
1. Kies een **categorie**, bijvoorbeeld `Business and Pages`. Deze waarde is vereist voor Facebook, maar wordt niet gebruikt door Azure AD B2C.
1. Selecteer aan de onderkant van de pagina **Platform toevoegen**, en selecteer vervolgens **Website**.
1. In **Site-URL**, voer `https://your-tenant-name.b2clogin.com/` vervangen `your-tenant-name` met de naam van uw tenant.
1. Voer een URL voor de **URL privacybeleid**, bijvoorbeeld `http://www.contoso.com/`. De URL privacybeleid is een pagina die u moet onderhouden voor privacy-informatie voor uw toepassing.
1. Selecteer **Save changes**.
1. Aan de bovenkant van de pagina, noteer de waarde van **App-ID**.
1. Naast **Appgeheim**, selecteer **weergeven** en noteer de waarde ervan. U kunt de App-ID en de App-geheim Facebook als id-provider configureren in uw tenant. **App-geheim** is een belangrijke beveiligingsreferentie die u moet veilig opslaan.
1. Selecteer het plusteken naast **producten**, klikt u vervolgens onder **aanmelden via Facebook**, selecteer **instellen**.
1. Onder **aanmelden via Facebook** Selecteer in het menu links **instellingen**.
1. In **geldig OAuth omleidings-URI's**, voer `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Vervang `your-tenant-name` met de naam van uw tenant. Selecteer **wijzigingen opslaan** aan de onderkant van de pagina.
1. Als u uw Facebook-toepassing naar Azure AD B2C, klikt u op de **Status** selector aan de bovenkant van de pagina en schakel het **op** openbaar maken van de toepassing en klik vervolgens op **bevestigen** . Op dit moment de Status te wijzigen van **ontwikkeling** naar **Live**.

## <a name="add-the-identity-providers"></a>De id-providers toevoegen

Nadat u de toepassing voor de id-provider die u wilt toevoegen hebt gemaakt, kunt u de id-provider toevoegen aan uw tenant.

### <a name="add-the-azure-active-directory-identity-provider"></a>De Azure Active Directory-id-provider toevoegen

1. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map met uw Azure AD B2C-tenant te kiezen.
1. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
1. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
1. Voer een **naam**. Voer bijvoorbeeld *Contoso Azure AD*.
1. Selecteer **type id-provider**, selecteer **Open ID Connect (Preview)** , en klik vervolgens op **OK**.
1. Klik op **deze id-provider instellen**
1. Voor **metagegevens-url**, voer de volgende URL vervangt `your-AD-tenant-domain` met de domeinnaam van uw Azure AD-tenant.

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

    Bijvoorbeeld `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`.

1. Voor **Client-ID**, voer de *(client) toepassings-ID* die u eerder hebt genoteerd.
1. Voor **clientgeheim**, voer de *clientgeheim* waarde die u eerder hebt genoteerd.
1. Geef eventueel een waarde voor **Domain_hint**. Bijvoorbeeld `ContosoAD`. [Domein-hints](../active-directory/manage-apps/configure-authentication-for-federated-users-portal.md) richtlijnen die zijn opgenomen in de verificatieaanvraag van een toepassing zijn. Ze kunnen worden gebruikt om te versnellen van de gebruiker naar de federatieve IdP-aanmelden pagina. Of ze kunnen worden gebruikt door een toepassing met meerdere tenants te versnellen van de gebruiker rechtstreeks naar de merknaam Azure AD-aanmeldingspagina voor hun tenant.
1. Selecteer **OK**.
1. Selecteer **deze id-provider claims toewijzen** en stel de volgende claims:

    - Voor **gebruikers-ID**, voer `oid`.
    - Voor **weergavenaam**, voer `name`.
    - Voor **voornaam**, voer `given_name`.
    - Voor **achternaam**, voer `family_name`.
    - Voor **e**, voer `unique_name`.

1. Selecteer **OK**en selecteer vervolgens **maken** aan uw configuratie op te slaan.

### <a name="add-the-facebook-identity-provider"></a>De Facebook-id-provider toevoegen

1. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
1. Voer een **naam**. Voer bijvoorbeeld *Facebook*.
1. Selecteer **type id-provider**, selecteer **Facebook**en selecteer vervolgens **OK**.
1. Selecteer **instellen van deze id-provider** en voer de *App-ID* die u eerder hebt genoteerd als de **Client-ID**.
1. Voer de *Appgeheim* die u hebt genoteerd als de **clientgeheim**.
1. Selecteer **OK** en selecteer vervolgens **maken** op uw Facebook-configuratie op te slaan.

## <a name="update-the-user-flow"></a>De gebruikersstroom bijwerken

In de zelfstudie die u als onderdeel van de vereisten hebt voltooid, hebt u een gebruikersstroom gemaakt voor aanmelden en meld u met de naam *B2C_1_signupsignin1*. In deze sectie maakt u de id-providers te toevoegen de *B2C_1_signupsignin1* gebruikersstroom.

1. Selecteer **gebruikersstromen (beleid)** , en selecteer vervolgens de *B2C_1_signupsignin1* gebruikersstroom.
2. Selecteer **id-providers**, selecteer de **Facebook** en **Contoso Azure AD** id-providers die u hebt toegevoegd.
3. Selecteer **Opslaan**.

## <a name="test-the-user-flow"></a>De gebruikersstroom testen

1. Selecteer op de pagina overzicht van de gebruikersstroom die u hebt gemaakt, **gebruikersstroom uitvoeren**.
1. Voor **toepassing**, selecteert u de web-App met de naam *webapp1* die u eerder hebt geregistreerd. De **antwoord-URL** moet worden weergegeven `https://jwt.ms`.
1. Selecteer **gebruikersstroom uitvoeren**, en vervolgens Meld u aan met een id-provider die u eerder hebt toegevoegd.
1. Herhaal stappen 1 t/m 3 voor de andere id-providers die u hebt toegevoegd.

Als de aanmelding van de bewerking geslaagd is, wordt u doorgestuurd naar `https://jwt.ms` weergeven met het decoderen-Token, vergelijkbaar met:

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "<key-ID>"
}.{
  "exp": 1562346892,
  "nbf": 1562343292,
  "ver": "1.0",
  "iss": "https://your-b2c-tenant.b2clogin.com/10000000-0000-0000-0000-000000000000/v2.0/",
  "sub": "20000000-0000-0000-0000-000000000000",
  "aud": "30000000-0000-0000-0000-000000000000",
  "nonce": "defaultNonce",
  "iat": 1562343292,
  "auth_time": 1562343292,
  "name": "User Name",
  "idp": "facebook.com",
  "postalCode": "12345",
  "tfp": "B2C_1_signupsignin1"
}.[Signature]
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u het volgende geleerd:

> [!div class="checklist"]
> * De identiteit van de provider-toepassingen maken
> * De id-providers toevoegen aan uw tenant
> * De id-providers toevoegen aan de gebruikersstroom

Vervolgens informatie over het aanpassen van de gebruikersinterface van de pagina's weergegeven aan gebruikers als onderdeel van hun identiteitservaring in uw toepassingen:

> [!div class="nextstepaction"]
> [De gebruikersinterface van uw toepassingen in Azure Active Directory B2C aanpassen](tutorial-customize-ui.md)
