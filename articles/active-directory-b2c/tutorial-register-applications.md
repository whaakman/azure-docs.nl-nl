---
title: 'Zelfstudie: uw toepassingen registreren in Azure Active Directory B2C | Microsoft Docs'
description: Informatie over het registreren van uw toepassingen in Azure Active Directory B2C met behulp van de Azure portal.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 01/11/2019
ms.author: davidmu
ms.openlocfilehash: 511e1e9f29e6ae7602a977819f5295f76236595d
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54248982"
---
# <a name="tutorial-register-your-applications-in-azure-active-directory-b2c"></a>Zelfstudie: Registreren van uw toepassingen in Azure Active Directory B2C

Voordat u uw [toepassingen](active-directory-b2c-apps.md) kan communiceren met Azure Active Directory (Azure AD) B2C, ze moeten worden geregistreerd in een tenant die u beheert. Deze zelfstudie leert u hoe u het registreren van toepassingen met behulp van de Azure portal.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een web-app registreren
> * Een web-API registreren
> * Een mobiele of native toepassing registreren

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit nog niet hebt gemaakt uw eigen [Azure AD B2C-Tenant](tutorial-create-tenant.md), Maak nu een. U kunt een bestaande tenant gebruiken.

## <a name="register-a-web-application"></a>Een web-app registreren

1. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.

    ![Schakel over naar de abonnementsmap](./media/tutorial-register-applications/switch-directories.png)

2. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
3. Selecteer **toepassingen**, en selecteer vervolgens **toevoegen**.

    ![Toepassing toevoegen](./media/tutorial-register-applications/add-application.png)

4. Voer een naam voor de toepassing. Bijvoorbeeld, *webapp1*.
5. Voor **inclusief web-app / web-API** en **impliciete stroom toestaan**, selecteer **Ja**.
6. Voor **antwoord-URL**, voer een eindpunt waar Azure AD B2C moet tokens retourneert die door uw toepassing worden aangevraagd. Bijvoorbeeld, kunt u dit instellen op het lokaal luisteren op `https://localhost:44316` als u het poortnummer dat nog niet weet, kunt u een tijdelijke aanduiding opgeven en dit later wijzigen. Voor testdoeleinden kunt u deze instellen op `https://jwt.ms`, wordt de inhoud van een token voor inspectie. Voor deze zelfstudie, ingesteld op `https://jwt.ms`. 

    Het antwoord op de URL met het schema beginnen moet `https`, en alle antwoord-URL-waarden moeten één DNS-domein delen. Bijvoorbeeld, als de toepassing heeft een antwoord-URL van `https://login.contoso.com`, u kunt toevoegen aan deze net als deze URL `https://login.contoso.com/new`. Of u kunt verwijzen naar een DNS-subdomein van `login.contoso.com`, zoals `https://new.login.contoso.com`. Als u wilt dat een toepassing met `login-east.contoso.com` en `login-west.contoso.com` als antwoord-URL's, moet u deze antwoord-URL's in deze volgorde toevoegen: `https://contoso.com`, `https://login-east.contoso.com`, `https://login-west.contoso.com`. U kunt de laatste twee toevoegen, omdat ze subdomeinen van de eerste antwoord-URL, `contoso.com`.

7. Klik op **Create**.

    ![Eigenschappen van de toepassing instellen](./media/tutorial-register-applications/application-properties.png)

### <a name="create-a-client-secret"></a>Een clientgeheim maken

Als u geen toepassing een code voor een token uitwisselt, moet u het toepassingsgeheim van een maken.

1. Selecteer **sleutels** en klik vervolgens op **sleutel genereren**.

    ![Sleutels genereren](./media/tutorial-register-applications/generate-keys.png)

2. Selecteer **opslaan** om de sleutel weer te geven. Noteer de waarde van **App-sleutel**. U gebruikt de waarde als het toepassingsgeheim in de code van uw toepassing.

    ![De sleutel opslaan](./media/tutorial-register-applications/save-key.png)
    
3. Selecteer **API-toegang**, klikt u op **toevoegen**, en selecteer uw web-API en scopes (machtigingen).

    ![API-toegang configureren](./media/tutorial-register-applications/api-access.png)

## <a name="register-a-web-api"></a>Een web-API registreren

1. Selecteer **toepassingen**, en selecteer vervolgens **toevoegen**.
3. Voer een naam voor de toepassing. Bijvoorbeeld, *webapi1*.
4. Voor **inclusief web-app / web-API** en **impliciete stroom toestaan**, selecteer **Ja**.
5. Voor **antwoord-URL**, voer een eindpunt waar Azure AD B2C moet tokens retourneert die door uw toepassing worden aangevraagd. Bijvoorbeeld, kunt u dit instellen op het lokaal luisteren op `https://localhost:44316`. Als u het poortnummer dat nog niet weet, kunt u een tijdelijke aanduiding opgeven en dit later wijzigen.
6. Voor **App ID URI**, voer de id die wordt gebruikt voor uw web-API. De volledige id-URI, inclusief het domein, wordt voor u gegenereerd. Bijvoorbeeld `https://contosotenant.onmicrosoft.com/api`.
7. Klik op **Create**.
8. Selecteer de *webapi1* toepassing die u hebt gemaakt en selecteer vervolgens **gepubliceerd bereiken** om toe te voegen meer bereiken indien nodig. Standaard de `user_impersonation` bereik is gedefinieerd. De `user_impersonation` bereik biedt andere toepassingen de mogelijkheid om toegang tot deze API namens de aangemelde gebruiker. Als u wilt, de `user_impersonation` bereik kan worden verwijderd.

    ![Gepubliceerde bereiken instellen](./media/tutorial-register-applications/published-scopes.png)


## <a name="register-a-mobile-or-native-application"></a>Een mobiele of native toepassing registreren

1. Selecteer **toepassingen**, en selecteer vervolgens **toevoegen**.
2. Voer een naam voor de toepassing. Bijvoorbeeld, *nativeapp1*.
3. Voor **inclusief web-app / web-API**, selecteer **Nee**.
4. Voor **systeemeigen client opnemen**, selecteer **Ja**.
5. Voor **omleidings-URI**, voer een geldige omleidings-URI met een aangepast schema. Er zijn twee belangrijke overwegingen bij het kiezen van een omleidings-URI:

    - **De unieke** -het schema van de omleidings-URI moet uniek zijn voor elke toepassing. In het voorbeeld `com.onmicrosoft.contoso.appname://redirect/path`, `com.onmicrosoft.contoso.appname` het schema. Dit patroon moet worden gevolgd. Als twee toepassingen hetzelfde schema delen, krijgt de gebruiker een keuze voor het kiezen van een toepassing. Als de gebruiker een foute keuze maakt, het aanmelden is mislukt.
    - **Volledige** -de omleidings-URI moet een schema en een pad. Het pad moet ten minste één forward slash bevatten na het domein. Bijvoorbeeld, `//contoso/` werkt en `//contoso` is mislukt. Zorg ervoor dat de omleidings-URI bevat geen speciale tekens zoals onderstrepingstekens bevatten.

6. Klik op **Create**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u:

> [!div class="checklist"]
> * Een web-app registreren
> * Een web-API registreren
> * Een mobiele of native toepassing registreren

> [!div class="nextstepaction"]
> [Gebruikersstromen maken in Azure Active Directory B2C](tutorial-create-user-flows.md)