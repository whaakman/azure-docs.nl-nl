---
title: Aangepaste connectors met Azure AD - Azure Logic Apps beveiligen | Microsoft Docs
description: Beveiliging en verificatie toevoegen aan uw API en de connector met Azure Active Directory (Azure AD)
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: bcee842cb880002daaa3ae9d9110ee1734941b6d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="add-security-to-your-api-and-connector-with-azure-active-directory-azure-ad"></a>Beveiliging toevoegen aan uw API en de connector met Azure Active Directory (Azure AD)

Toevoegen als u wilt beveiligen aanroepen tussen uw API en aangepaste connector, Azure AD-verificatie voor uw Web-API en de connector. Maken van twee apps van Azure Active Directory (Azure AD) voor dit scenario: een Azure AD-app beveiligt uw Web-API, terwijl de andere Azure AD-app uw registratie van de connector beveiligt en gedelegeerde toegang voegt. 

Deze zelfstudie maakt gebruik van de Azure Resource Manager-API als voorbeeld. Azure Resource Manager kunt u de onderdelen voor een oplossing die u hebt gemaakt in Azure, zoals databases, virtuele machines en web-apps beheren. Een aangepaste voor Azure Resource Manager-connector kan nuttig zijn wanneer u een Azure-resources beheren vanaf uw werkstromen wilt maken. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u geen een abonnement hebt, kunt u beginnen met een [gratis Azure-account](https://azure.microsoft.com/free/). Anders zich aanmelden voor een [abonnement met betalen naar gebruik](https://azure.microsoft.com/pricing/purchase-options/).

* Voor deze zelfstudie de [OpenAPI voorbeeldbestand voor Azure Resource Manager](https://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json)

  > [!NOTE] 
  > Het voorbeeldbestand OpenAPI bevat geen definitie van alle bewerkingen voor Azure Resource Manager en bevat momenteel alleen de bewerking voor [lijst van alle abonnementen](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_List). U kunt deze OpenAPI bewerken of maken van een andere OpenAPI-bestand met de [online OpenAPI editor](http://editor.swagger.io/).
  >
  > U kunt deze zelfstudie toepassen op een RESTful-API waarin u wilt gebruiken van Azure AD-verificatie.

## <a name="1-create-your-first-azure-ad-app-for-securing-your-web-api"></a>1. Uw eerste Azure AD-app voor het beveiligen van uw Web-API maken

Uw eerste Azure AD-app wordt verificatie uitgevoerd als de aangepaste verbindingslijn uw API, de Azure Resource Manager-API in dit voorbeeld is wordt aangeroepen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Als u meer dan één Azure Active Directory-tenant hebt, moet u bevestigen dat u bent aangemeld bij de correcte directory door te controleren van de map onder uw gebruikersnaam. 

   ![Bevestig uw directory](./media/custom-connector-azure-active-directory-authentication/check-user-directory.png)

   > [!TIP]
   > Wijzig de mappen, kiest u uw gebruikersnaam en vervolgens de directory selecteren die u wilt.

2. Kies in het Azure hoofdmenu **Azure Active Directory** zodat u uw huidige adreslijst kunt bekijken.

   !['Azure Active Directory' kiezen](./media/custom-connector-azure-active-directory-authentication/azure-active-directory.png)

   > [!TIP]
   > Als het hoofdmenu Azure wordt niet weergegeven **Azure Active Directory**, kies **meer services**. In de **Filter** vak, typt u 'Azure Active Directory' als uw filter, en kies vervolgens **Azure Active Directory**.

3. Klik in het menu directory onder **beheren**, kies **App registraties**. Kies in de lijst met geregistreerde apps **+ een nieuwe toepassing registreren**.

   ![Kies 'App registraties', '+ nieuwe toepassingsregistratie '](./media/custom-connector-azure-active-directory-authentication/add-app-registrations.png)

4. Onder **maken**, geef de details op voor uw Azure AD-app, zoals beschreven in de tabel en kies vervolgens **maken**. 

   ![Azure AD-app maken](./media/custom-connector-azure-active-directory-authentication/create-app1-registration.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Naam** | *Web-api-app-naam* | De naam voor uw Web-API Azure AD-app | 
   | **Toepassingstype** | **Web-app / API** | Type van uw app | 
   | **Aanmeldings-URL** | `https://login.windows.net` | | 
   |||| 

5. Als u terugkeert naar de map **App registraties** , selecteert u uw Azure AD-app.

   ![Selecteer uw Azure AD-app in lijst](./media/custom-connector-azure-active-directory-authentication/app1-registration-created.png)

6. Wanneer de pagina details van de app wordt weergegeven, zorg ervoor dat u **kopiëren en opslaan van de app *toepassings-ID* ergens veilig**. In dat geval moet u deze ID in voor later gebruik.

   !['Toepassings-ID voor later gebruik opslaan](./media/custom-connector-azure-active-directory-authentication/application-id-app1.png)

7. Nu een antwoord-URL opgeven voor uw Azure AD-app. In de app **instellingen** menu kiezen **antwoord-URL's**. Deze URL opgeven en kies vervolgens **opslaan**.

   ![Antwoord-URL 's](./media/custom-connector-azure-active-directory-authentication/add-reply-url1.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Antwoord-URL 's** | Voer de URL voor uw eigen API: </br>`https://{your-web-app-root-URL}/.auth/login/aad/callback` | | 
   | **Gedelegeerde machtigingen** | {niet nodig} | | 
   | **Sleutel van de client** | {niet nodig} | | 
   |||| 

   > [!TIP]
   > Als de **instellingen** menu niet eerder is weergegeven, kiest u **instellingen** hier:
   >
   > ![Kies 'Instellingen'](./media/custom-connector-azure-active-directory-authentication/show-app1-settings-menu.png)

## <a name="2-create-your-second-azure-ad-app-for-your-custom-connector"></a>2. Tweede Azure AD-app voor uw aangepaste connector maken

De tweede Azure AD-app de registratie van uw aangepaste connector beveiligt en voegt gedelegeerde toegang toe aan de Web-API die is beveiligd door de eerste Azure AD-app. 

> [!IMPORTANT]
> Zorg ervoor dat zowel uw Azure AD-apps in dezelfde map bestaan.

1. Ga terug naar de **App registraties** lijst en kies **+ een nieuwe toepassing registreren** opnieuw.

   ![Kies 'App registraties', '+ nieuwe toepassingsregistratie '](./media/custom-connector-azure-active-directory-authentication/add-app-registrations2.png)

2. Onder **maken**, geef de details op voor uw tweede Azure AD-app, zoals beschreven in de tabel en kies vervolgens **maken**. 

   ![Azure AD-app maken](./media/custom-connector-azure-active-directory-authentication/create-app2-registration.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Naam** | *uw connector-naam* | De naam voor de connector Azure AD-app | 
   | **Toepassingstype** | **Web-app / API** | Type van uw app | 
   | **Aanmeldings-URL** | `https://login.windows.net` | | 
   |||| 

3. Als u terugkeert naar de map **App registraties** , selecteert u uw tweede Azure AD-app.

   ![Selecteer uw tweede Azure AD-app in lijst](./media/custom-connector-azure-active-directory-authentication/app2-registration-created.png)

4. Wanneer de pagina details van de app wordt weergegeven, zorg ervoor dat u ook **kopiëren en opslaan van deze app *toepassings-ID* ergens veilig te**. In dat geval moet u deze ID in voor later gebruik.

   !['Toepassings-ID voor later gebruik opslaan](./media/custom-connector-azure-active-directory-authentication/application-id-app2.png)

5. Nu een antwoord-URL opgeven voor uw Azure AD-app. In de app **instellingen** menu kiezen **antwoord-URL's**. Deze URL opgeven en kies vervolgens **opslaan**.

   | Instelling | Voorgestelde waarde | 
   | ------- | --------------- | 
   | **Antwoord-URL 's** | Voer de URL voor de aangepaste Azure Resource Manager-connector:`https://msmanaged-na.consent.azure-apim.net/redirect` | 
   ||| 

   > [!TIP]
   > Als de **instellingen** menu niet eerder is weergegeven, kiest u **instellingen** hier:
   >
   > ![Kies 'Instellingen'](./media/custom-connector-azure-active-directory-authentication/show-app2-settings-menu.png)

6. Terug in de **instellingen** menu kiezen **vereist machtigingen** > **toevoegen**.

   ![Vereiste machtigingen > toevoegen](./media/custom-connector-azure-active-directory-authentication/add-api-access1-select-permissions.png)

7. Wanneer de **API toevoegen toegang** menu wordt geopend, kiest u **selecteert u een API** > 
**Windows Azure Service Management API** > **selecteren **.

   ![Een API selecteren](./media/custom-connector-azure-active-directory-authentication/add-api-access2-select-api.png)

8. Op de **API toevoegen toegang** menu kiezen **machtigingen selecteren**. Onder **overgedragen machtigingen**, kies **Access Azure Service Management als gebruikers organisatie** > **Selecteer**.

   ![Kies 'Gedelegeerde machtigingen' > ' toegang tot Azure-beheer als gebruikers van de organisatie '](./media/custom-connector-azure-active-directory-authentication/add-api-access3-select-permissions.png)

   Voor andere opties:

   | Optie | Voorgestelde waarde | Beschrijving | 
   | ------ | --------------- | ----------- | 
   | **Gedelegeerde machtigingen** | | Selecteer de machtiging voor gedelegeerde toegang tot uw Web-API | 
   |||| 

9. Nu op de **API toevoegen toegang** menu kiezen **gedaan**.

   ![Menu 'Add-API-toegang' > 'Uitgevoerd'](./media/custom-connector-azure-active-directory-authentication/add-api-access4-done.png)

10. Nu genereren een *clientsleutel*, of 'geheim' voor de connector Azure AD-app. 

    1. Terug op de **instellingen** menu kiezen **sleutels**. 
    Geef een naam voor uw sleutel met 16 tekens, selecteert u een vervalperiode in en kies vervolgens **opslaan**.

       ![De sleutel van een client maken](./media/custom-connector-azure-active-directory-authentication/add-key.png)

    2. Wanneer uw gegenereerde sleutel wordt weergegeven, **Zorg ervoor dat u kopiëren en deze sleutel ergens veilig opslaan** voordat u laat de **sleutels** pagina.
    
       ![Handmatig kopiëren en opslaan van uw sleutel](./media/custom-connector-azure-active-directory-authentication/save-key.png)

11. Na het opslaan van uw sleutel, kunt u veilig sluiten de **instellingen** menu.

## <a name="3-add-azure-ad-authentication-to-your-web-api-app"></a>3. Azure AD-verificatie toevoegen aan uw Web-API-app

Schakel nu verificatie voor uw Web-API met uw eerste app in Azure AD. Informatie voor meer informatie over [het configureren van uw App Service-toepassing voor het gebruik van Azure Active Directory-aanmelding](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

1. In de [Azure-portal](https://portal.azure.com), vinden van uw Web-API-app die u eerder hebt gepubliceerd in [aangepaste connectors maken op basis van Web-API's](../logic-apps/custom-connector-build-web-api-app-tutorial.md).

2. Onder **instellingen**, kies **verificatie / autorisatie**. 

   1. Onder **verificatie van App Service**, kies **op**.
   2. Voor **te ondernemen actie wanneer de aanvraag is niet geverifieerd**, selecteer **aanmelden met Azure Active Directory**.
   3. Voor **verificatieproviders**, selecteer **Azure Active Directory**. 

   ![Kies ' verificatie / autorisatie '](./media/custom-connector-azure-active-directory-authentication/web-api-app-authentication.png)

3. Op de **Azure Active Directory-instellingen** pagina:

   1. Onder **beheermodus**, kies **Express**.

   2. Nu de Azure AD-app die gebruikmaakt van uw Web-API-app voor verificatie selecteren. 
   Kies **bestaande AD-App selecteren** > **Azure AD-App**.

   3. Onder **Azure AD-toepassingen**, selecteer de Azure AD-app die u eerder hebt gemaakt voor uw Web-API-app. 
   
   4. Kies **OK** totdat u naar terugkeren de **verificatie / autorisatie** pagina.

   Bijvoorbeeld:

   ![Kies de Azure AD-app waarmee uw Web-API-app](./media/custom-connector-azure-active-directory-authentication/web-api-app-select-azure-ad-app.png)

4. Op de **verificatie / autorisatie** pagina **opslaan**.

   ![Verificatie-instellingen opslaan](./media/custom-connector-azure-active-directory-authentication/web-api-app-azure-ad-app-save.png)

   Uw Web-API-app kan nu Azure AD gebruiken voor verificatie.

## <a name="4-set-up-azure-ad-authentication-in-your-web-apis-openapi-file"></a>4. Azure AD-verificatie in uw Web-API OpenAPI bestand instellen

Open het bestand OpenAPI voor uw Web-API-app zodat u kunt toevoegen de `securityDefintions` object en Azure AD authentication onder de `host` eigenschap. Hier volgt een voorbeeld waarin de `host` eigenschap en `securityDefinitions` object:

``` json
// Your OpenAPI file header appears here...

"host": "{your-web-api-app-root-url}",
"schemes": [
    "https" // Make sure this is https!
],
"securityDefinitions": {
    "AAD": {
        "type": "oauth2",
        "flow": "accessCode",
        "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
        "tokenUrl": "https://login.windows.net/common/oauth2/token",
        "scopes": {}
    }
},

// Your OpenAPI document continues here...
```
U kunt nu klaar om te maken en registreren van uw aangepaste connector.

## <a name="next-steps"></a>Volgende stappen

* [De connector registreren](../logic-apps/logic-apps-custom-connector-register.md)
* [Veelgestelde vragen over aangepaste connector](../logic-apps/custom-connector-faq.md)
