---
title: Zelf studie-id-providers toevoegen aan uw toepassingen-Azure Active Directory B2C
description: Meer informatie over het toevoegen van id-providers aan uw toepassingen in Azure Active Directory B2C met behulp van de Azure Portal.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 07/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 5710ccfe5d6450714e029827a795b484b1bcd2b4
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68716651"
---
# <a name="tutorial-add-identity-providers-to-your-applications-in-azure-active-directory-b2c"></a>Zelfstudie: Voeg id-providers toe aan uw toepassingen in Azure Active Directory B2C

Het is wellicht wenselijk dat uw gebruikers zich in uw toepassingen bij verschillende id-providers kunnen aanmelden. Een *id-provider* maakt, onderhoudt en beheert id-gegevens en biedt tegelijkertijd verificatieservices voor toepassingen. U kunt id-providers die worden ondersteund door Azure Active Directory (Azure AD) B2C toevoegen aan uw [gebruikers stromen](active-directory-b2c-reference-policies.md) met behulp van de Azure Portal.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * De toepassingen voor de ID-provider maken
> * De id-providers toevoegen aan uw Tenant
> * De id-providers toevoegen aan uw gebruikers stroom

Normaal gesp roken gebruikt u slechts één ID-provider in uw toepassingen, maar u hebt de mogelijkheid om meer toe te voegen. Deze zelf studie laat zien hoe u een Azure AD-ID-provider en een Facebook-ID-provider aan uw toepassing kunt toevoegen. Het toevoegen van beide id-providers aan uw toepassing is optioneel. U kunt ook andere id-providers toevoegen, zoals [Amazon](active-directory-b2c-setup-amzn-app.md), [github](active-directory-b2c-setup-github-app.md), [Google](active-directory-b2c-setup-goog-app.md), [LinkedIn](active-directory-b2c-setup-li-app.md), [micro soft](active-directory-b2c-setup-msa-app.md)of [Twitter](active-directory-b2c-setup-twitter-app.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

[Maak een gebruikers stroom](tutorial-create-user-flows.md) zodat gebruikers zich kunnen registreren en zich kunnen aanmelden bij uw toepassing.

## <a name="create-applications"></a>Toepassingen maken

ID-provider toepassingen bieden de id en de sleutel om communicatie met uw Azure AD B2C-Tenant mogelijk te maken. In deze sectie van de zelf studie maakt u een Azure AD-toepassing en een Facebook-toepassing van waaruit u id's en sleutels kunt toevoegen om de id-providers toe te voegen aan uw Tenant. Als u slechts een van de id-providers toevoegt, hoeft u alleen de toepassing voor die provider te maken.

### <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

Als u het aanmelden voor gebruikers vanuit Azure AD wilt inschakelen, moet u een toepassing registreren in de Azure AD-Tenant. De Azure AD-Tenant is niet hetzelfde als uw Azure AD B2C-Tenant.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zorg ervoor dat u de map gebruikt die uw Azure AD-Tenant bevat door te klikken op het **filter voor mappen en abonnementen** in het bovenste menu en kies de map die uw Azure AD-Tenant bevat.
1. Kies **alle services** in de linkerbovenhoek van de Azure Portal en zoek en selecteer **app-registraties**.
1. Selecteer **nieuwe registratie**.
1. Voer een naam in voor de toepassing. Bijvoorbeeld `Azure AD B2C App`.
1. Accepteer de selectie van **accounts in deze organisatie Directory alleen** voor deze toepassing.
1. Voor de omleidings- **URI**accepteert u de waarde van **Web** en voert u de volgende URL in in `your-B2C-tenant-name` kleine letters, waarbij u vervangt door de naam van uw Azure AD B2C Tenant.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Bijvoorbeeld `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.

    Alle Url's moeten nu gebruikmaken van [b2clogin.com](b2clogin.md).

1. Selecteer **registreren**en noteer vervolgens de **client-id** die u in een latere stap gebruikt.
1. Selecteer onder **beheren** in het toepassings menu de optie **Certificaten & geheimen**en selecteer vervolgens **Nieuw client geheim**.
1. Voer een **Beschrijving** in voor het client geheim. Bijvoorbeeld `Azure AD B2C App Secret`.
1. Selecteer de verval periode. Accepteer voor deze toepassing de selectie van **in één jaar**.
1. Selecteer **toevoegen**en noteer vervolgens de waarde van het nieuwe client geheim dat u in een latere stap gebruikt.

### <a name="create-a-facebook-application"></a>Een Facebook-toepassing maken

Als u een Facebook-account wilt gebruiken als een id-provider in Azure AD B2C, moet u een toepassing maken op Facebook. Als u nog geen Facebook-account hebt, kunt u het downloaden via [https://www.facebook.com/](https://www.facebook.com/).

1. Meld u aan bij [Facebook voor ontwikkel aars](https://developers.facebook.com/) met de referenties van uw Facebook-account.
1. Als u dit nog niet hebt gedaan, moet u zich registreren als Facebook-ontwikkelaar. U doet dit door aan de rechter bovenhoek van de pagina aan de **slag te gaan** , het beleid van Facebook te accepteren en de registratie stappen te volt ooien.
1. Selecteer **mijn apps** en maak vervolgens een **app**.
1. Voer een **weergave naam** in en geef een geldig **e-mail adres voor de contact persoon op**.
1. Klik op **App-ID maken**. Hiervoor moet u mogelijk het Facebook-platform beleid accepteren en een online beveiligings controle volt ooien.
1. Selecteer **instellingen** > **basis**.
1. Kies bijvoorbeeld `Business and Pages`een categorie. Deze waarde is vereist voor Facebook, maar wordt niet gebruikt door Azure AD B2C.
1. Selecteer onder aan de pagina **platform toevoegen**en selecteer vervolgens **website**.
1. `your-tenant-name` Voer`https://your-tenant-name.b2clogin.com/` in site-URL vervangen in met de naam van uw Tenant.
1. Voer een URL in voor de URL van het **Privacybeleid**, `http://www.contoso.com/`bijvoorbeeld. De URL van het privacybeleid is een pagina die u kunt onderhouden voor het verstrekken van privacy-informatie voor uw toepassing.
1. Selecteer **Save changes**.
1. Noteer de waarde van de **App-ID**aan de bovenkant van de pagina.
1. Selecteer bij **app-geheim**de waarde **weer geven** en noteer deze. U gebruikt zowel de App-ID als het geheim van de app voor het configureren van Facebook als een id-provider in uw Tenant. **App-geheim** is een belang rijke beveiligings referentie die u veilig moet opslaan.
1. Selecteer het plus teken naast **producten**en selecteer vervolgens onder **Aanmelden bij Facebook**de optie **instellen**.
1. Selecteer onder **Facebook-aanmelding** in het menu links de optie **instellingen**.
1. Voer`https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`in **geldige OAuth**omleidings-uri's in. Vervang `your-tenant-name` door de naam van uw Tenant. Selecteer **wijzigingen opslaan** onder aan de pagina.
1. Als u uw Facebook-toepassing beschikbaar wilt maken voor Azure AD B2C, klikt u op de **status** kiezer rechtsboven op de pagina en schakelt u deze **in** om de toepassing openbaar te maken en klikt u vervolgens op **bevestigen**. Op dit moment moet de status worden gewijzigd van **ontwikkeling** naar **Live**.

## <a name="add-the-identity-providers"></a>De id-providers toevoegen

Nadat u de toepassing hebt gemaakt voor de ID-provider die u wilt toevoegen, voegt u de ID-provider aan uw Tenant toe.

### <a name="add-the-azure-active-directory-identity-provider"></a>De Azure Active Directory-ID-provider toevoegen

1. Zorg ervoor dat u de map gebruikt die uw Azure AD B2C Tenant bevat door te klikken op het **filter Directory en abonnement** in het bovenste menu en de map te kiezen die uw Azure AD B2C Tenant bevat.
1. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
1. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
1. Voer een **naam**in. Voer bijvoorbeeld *Contoso Azure AD*in.
1. Selecteer een **ID-provider type**, selecteer **OpenID Connect Connect (preview)** en klik vervolgens op **OK**.
1. Klik op **Deze id-provider instellen**
1. Voor **meta gegevens-URL**voert u de volgende URL `your-AD-tenant-domain` in, waarbij u vervangt door de domein naam van uw Azure AD-Tenant.

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

    Bijvoorbeeld `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`.

1. Voer voor **client-id**de *id van de toepassing (client)* in die u eerder hebt vastgelegd.
1. Voer voor **client geheim**de waarde voor *client geheim* in die u eerder hebt vastgelegd.
1. Voer desgewenst een waarde in voor **Domain_hint**. Bijvoorbeeld `ContosoAD`. [Domein hints](../active-directory/manage-apps/configure-authentication-for-federated-users-portal.md) zijn richt lijnen die zijn opgenomen in de verificatie aanvraag van een toepassing. Ze kunnen worden gebruikt om de gebruiker te versnellen tot hun federatieve IdP-aanmeldings pagina. Of ze kunnen worden gebruikt door een toepassing met meerdere tenants om de gebruiker rechtstreeks naar de aanmeldings pagina van Azure AD te versnellen voor hun Tenant.
1. Selecteer **OK**.
1. Selecteer de **claims van deze id-provider toewijzen** en stel de volgende claims in:

    - Voer`oid`in bij **gebruikers-id**.
    - Voer`name`in bij **weergave naam**.
    - Voer`given_name`in voor de **opgegeven naam**.
    - Voer`family_name`voor achternaam in.
    - Voer`unique_name`in het **e-mail adres**in.

1. Selecteer **OK**en selecteer vervolgens **maken** om uw configuratie op te slaan.

### <a name="add-the-facebook-identity-provider"></a>De Facebook-ID-provider toevoegen

1. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
1. Voer een **naam**in. Typ bijvoorbeeld *Facebook*.
1. Selecteer het **type ID-provider**, selecteer **Facebook**en selecteer **OK**.
1. Selecteer **deze ID-provider instellen** en voer de *App-ID* in die u eerder hebt vastgelegd als de **client-id**.
1. Voer het *app-geheim* in dat u hebt genoteerd als het **client geheim**.
1. Selecteer **OK** en selecteer vervolgens **maken** om uw Facebook-configuratie op te slaan.

## <a name="update-the-user-flow"></a>De gebruikers stroom bijwerken

In de zelf studie die u als onderdeel van de vereisten hebt voltooid, hebt u een gebruikers stroom gemaakt voor het aanmelden en aanmelden met de naam *B2C_1_signupsignin1*. In deze sectie voegt u de id-providers toe aan de *B2C_1_signupsignin1* -gebruikers stroom.

1. Selecteer **gebruikers stromen (beleid)** en selecteer vervolgens de *B2C_1_signupsignin1* -gebruikers stroom.
2. Selecteer **id-providers**, selecteer de **Facebook** -en **Contoso Azure AD** -id-providers die u hebt toegevoegd.
3. Selecteer **Opslaan**.

## <a name="test-the-user-flow"></a>De gebruikers stroom testen

1. Selecteer op de pagina overzicht van de gebruikers stroom die u hebt gemaakt, de optie **gebruikers stroom uitvoeren**.
1. Selecteer voor **toepassing**de webtoepassing met de naam *webapp1* die u eerder hebt geregistreerd. De **antwoord-URL** moet `https://jwt.ms`worden weer gegeven.
1. Selecteer **gebruikers stroom uitvoeren**en meld u aan met een id-provider die u eerder hebt toegevoegd.
1. Herhaal stap 1 tot en met 3 voor de andere id-providers die u hebt toegevoegd.

Als de aanmeldings bewerking is gelukt, wordt u omgeleid naar `https://jwt.ms` waar de gedecodeerde token wordt weer gegeven, vergelijkbaar met:

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
> * De toepassingen voor de ID-provider maken
> * De id-providers toevoegen aan uw Tenant
> * De id-providers toevoegen aan uw gebruikers stroom

Vervolgens leert u hoe u de gebruikers interface kunt aanpassen van de pagina's die worden weer gegeven aan gebruikers als onderdeel van hun identiteits ervaring in uw toepassingen:

> [!div class="nextstepaction"]
> [De gebruikers interface van uw toepassingen in Azure Active Directory B2C aanpassen](tutorial-customize-ui.md)
