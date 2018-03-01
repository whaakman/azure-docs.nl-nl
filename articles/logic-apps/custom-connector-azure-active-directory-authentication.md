---
title: Aangepaste connectors met Azure AD - Azure Logic Apps beveiligen | Microsoft Docs
description: Lees hoe u beveiliging en verificatie toevoegt aan uw API en de connector met Azure Active Directory (Azure AD)
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
ms.openlocfilehash: dae76a4230a1cfbe8970bbb1015041fde7765f49
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2018
---
# <a name="add-security-to-your-api-and-connector-with-azure-active-directory-azure-ad"></a>Beveiliging toevoegen aan uw API en de connector met Azure Active Directory (Azure AD)

Als u aanroepen tussen uw API en de aangepaste connector wilt beveiligen, voegt u Azure AD-verificatie toe aan uw web-API en de connector. Voor dit scenario gaan we twee apps van Azure Active Directory (Azure AD) maken: met de ene Azure AD-app wordt uw web-API beveiligt, terwijl de andere Azure AD-app zorgt voor beveiliging van de connector-registratie en gedelegeerde toegang toevoegt. 

In deze zelfstudie wordt de API van Azure Resource Manager als voorbeeld gebruikt. Azure Resource Manager helpt bij het beheren van de onderdelen voor een oplossing die u hebt gemaakt in Azure, zoals databases, virtuele machines en web-apps. Een aangepaste connector voor Azure Resource Manager-connector kan handig zijn wanneer u Azure-resources wilt beheren vanuit uw werkstromen. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u beginnen met een [gratis Azure-account](https://azure.microsoft.com/free/). U kunt ook kiezen voor een [Betalen per gebruik-abonnement](https://azure.microsoft.com/pricing/purchase-options/).

* Voor deze zelfstudie het [OpenAPI-voorbeeldbestand voor Azure Resource Manager](https://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json)

  > [!NOTE] 
  > In het OpenAPI-voorbeeldbestand worden niet alle bewerkingen van Azure Resource Manager gedefinieerd en het bestand bevat momenteel alleen de bewerking voor [het opvragen van alle abonnementen](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_List). U kunt deze OpenAPI bewerken of een ander OpenAPI-bestand maken met de [online OpenAPI-editor](http://editor.swagger.io/).
  >
  > U kunt deze zelfstudie toepassen op elke RESTful-API waarmee u Azure AD-verificatie wilt gebruiken.

## <a name="1-create-your-first-azure-ad-app-for-securing-your-web-api"></a>1. Uw eerste Azure AD-app maken voor het beveiligen van uw web-API

Uw eerste Azure AD-app verzorgt de verificatie op het moment dat de aangepaste connector uw API aanroept, wat in dit voorbeeld de API van Azure Resource Manager is.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Als u meer dan één Azure Active Directory-tenant hebt, controleert u of u bent aangemeld in de juiste Active Directory door naar de directory onder uw gebruikersnaam te kijken. 

   ![Active Directory controleren](./media/custom-connector-azure-active-directory-authentication/check-user-directory.png)

   > [!TIP]
   > Als u van directory wilt wijzigen, kiest u uw gebruikersnaam en selecteert u vervolgens de gewenste directory.

2. Kies in het hoofdmenu van Azure de optie **Azure Active Directory**, zodat u de huidige directory kunt bekijken.

   ![Azure Active Directory kiezen](./media/custom-connector-azure-active-directory-authentication/azure-active-directory.png)

   > [!TIP]
   > Als het hoofdmenu Azure wordt niet weergegeven **Azure Active Directory**, kies **alle services**. Typ in het vak **Filter** 'Azure Active Directory' als het filter en kies vervolgens **Azure Active Directory**.

3. Klik in het menu voor de directory, onder **Beheren**, op **App registraties**. Kies **+ Nieuwe toepassing registreren** in de lijst met geregistreerde apps.

   !['App-registraties' en '+ Nieuwe toepassing registreren' kiezen](./media/custom-connector-azure-active-directory-authentication/add-app-registrations.png)

4. Geef onder **Maken** de details op voor uw Azure AD-app, zoals wordt beschreven in de tabel, en kies vervolgens **Maken**. 

   ![Azure AD-app maken](./media/custom-connector-azure-active-directory-authentication/create-app1-registration.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Naam** | *appnaam web-API* | De naam voor de Azure AD-app voor uw web-API | 
   | **Toepassingstype** | **Web-app/-API** | Het type app | 
   | **Aanmeldings-URL** | `https://login.windows.net` | | 
   |||| 

5. Als u terugkeert naar de lijst **App registraties** van uw directory, selecteert u uw Azure AD-app.

   ![Uw Azure AD-app selecteren in lijst](./media/custom-connector-azure-active-directory-authentication/app1-registration-created.png)

6. Wanneer de pagina met details van de app wordt weergegeven, **kopieert u de *toepassings-id* van de app en bewaart u deze op een veilige plek**. U hebt deze id later nog nodig.

   ![Toepassings-id opslaan voor later gebruik](./media/custom-connector-azure-active-directory-authentication/application-id-app1.png)

7. Geef nu een antwoord-URL op voor uw Azure AD-app. Kies hiervoor **Antwoord-URL's** in het menu **Instellingen** van de app. Voer deze URL in en kies vervolgens **Opslaan**.

   ![Antwoord-URL's](./media/custom-connector-azure-active-directory-authentication/add-reply-url1.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Antwoord-URL's** | Voer deze URL in voor uw eigen API: </br>`https://{your-web-app-root-URL}/.auth/login/aad/callback` | | 
   | **Gedelegeerde machtigingen** | {niet nodig} | | 
   | **Clientsleutel** | {niet nodig} | | 
   |||| 

   > [!TIP]
   > Als het menu **Instellingen** eerder niet werd weergegeven, kiest u hier **Instellingen**:
   >
   > !['Instellingen' kiezen](./media/custom-connector-azure-active-directory-authentication/show-app1-settings-menu.png)

## <a name="2-create-your-second-azure-ad-app-for-your-custom-connector"></a>2. Uw tweede Azure AD-app voor uw aangepaste connector maken

De tweede Azure AD-app beveiligt de registratie van uw aangepaste connector en voegt gedelegeerde toegang toe aan de web-API die wordt beveiligd door de eerste Azure AD-app. 

> [!IMPORTANT]
> Zorg ervoor dat beide Azure AD-apps in dezelfde map staan.

1. Ga terug naar de lijst **App-registraties** en kies nogmaals **+ Nieuwe toepassing registreren**.

   !['App-registraties' en '+ Nieuwe toepassing registreren' kiezen](./media/custom-connector-azure-active-directory-authentication/add-app-registrations2.png)

2. Geef onder **Maken** de details op voor uw tweede Azure AD-app, zoals wordt beschreven in de tabel, en kies vervolgens **Maken**. 

   ![Azure AD-app maken](./media/custom-connector-azure-active-directory-authentication/create-app2-registration.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Naam** | *naam-van-uw-connector* | De naam voor de Azure AD-app voor uw connector | 
   | **Toepassingstype** | **Web-app/-API** | Het type app | 
   | **Aanmeldings-URL** | `https://login.windows.net` | | 
   |||| 

3. Als u terugkeert naar de lijst **App registraties** van uw directory, selecteert u de tweede Azure AD-app.

   ![Uw tweede Azure AD-app selecteren in lijst](./media/custom-connector-azure-active-directory-authentication/app2-registration-created.png)

4. Wanneer de pagina met details van de app wordt weergegeven, **kopieert u ook de *toepassings-id* van deze app en bewaart u deze eveneens op een veilige plek**. U hebt deze id later nog nodig.

   ![Toepassings-id opslaan voor later gebruik](./media/custom-connector-azure-active-directory-authentication/application-id-app2.png)

5. Geef nu een antwoord-URL op voor uw Azure AD-app. Kies hiervoor **Antwoord-URL's** in het menu **Instellingen** van de app. Voer deze URL in en kies vervolgens **Opslaan**.

   | Instelling | Voorgestelde waarde | 
   | ------- | --------------- | 
   | **Antwoord-URL's** | Voer deze URL in voor de aangepaste connector voor Azure Resource Manager:`https://msmanaged-na.consent.azure-apim.net/redirect` | 
   ||| 

   > [!TIP]
   > Als het menu **Instellingen** eerder niet werd weergegeven, kiest u hier **Instellingen**:
   >
   > !['Instellingen' kiezen](./media/custom-connector-azure-active-directory-authentication/show-app2-settings-menu.png)

6. Terug in het menu **Instellingen** kiest u **Vereiste machtigingen** > **Toevoegen**.

   ![Vereiste machtigingen > Toevoegen](./media/custom-connector-azure-active-directory-authentication/add-api-access1-select-permissions.png)

7. Wanneer het menu **API-toegang toevoegen** wordt geopend, kiest u **Een API selecteren** > 
**Windows Azure Service Management-API** > **Selecteren**.

   ![Een API selecteren](./media/custom-connector-azure-active-directory-authentication/add-api-access2-select-api.png)

8. Kies **Machtigingen selecteren** in het menu **API-toegang toevoegen**. Kies onder **Gedelegeerde machtigingen** **Toegang tot Azure Service Management als organisatiegebruikers** > **Selecteer**.

   ![Kies Gedelegeerde machtigingen > Toegang tot Azure Service Management als organisatiegebruikers](./media/custom-connector-azure-active-directory-authentication/add-api-access3-select-permissions.png)

   Voor andere opties:

   | Optie | Voorgestelde waarde | Beschrijving | 
   | ------ | --------------- | ----------- | 
   | **Gedelegeerde machtigingen** | | Selecteer machtiging voor gedelegeerde toegang tot uw web-API | 
   |||| 

9. Kies nu **Gereed** in het menu **API-toegang toevoegen**.

   ![Menu API-toegang toevoegen > Gereed](./media/custom-connector-azure-active-directory-authentication/add-api-access4-done.png)

10. Genereer nu een *clientsleutel*, of 'geheim', voor de Azure AD-app van de connector. 

    1. Ga terug naar het menu **Instellingen** en kies **Sleutels**. 
    Geef een naam op voor uw sleutel met maximaal 16 tekens, selecteer een vervalperiode en kies vervolgens **Opslaan**.

       ![Een clientsleutel maken](./media/custom-connector-azure-active-directory-authentication/add-key.png)

    2. Wanneer uw gegenereerde sleutel wordt weergegeven, moet u deze **kopiëren en op een veilige plek opslaan** voordat u de pagina **Sleutels** verlaat.
    
       ![De sleutel handmatig kopiëren en opslaan](./media/custom-connector-azure-active-directory-authentication/save-key.png)

11. Nadat u de sleutel hebt opgeslagen, kunt u het menu **Instellingen** veilig sluiten.

## <a name="3-add-azure-ad-authentication-to-your-web-api-app"></a>3. Azure AD-verificatie toevoegen aan uw web-API-app

Schakel nu verificatie in voor uw web-API met uw eerste Azure AD-app. Informatie voor meer informatie over [het configureren van uw App Service-toepassing voor het gebruik van Azure Active Directory-aanmelding](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

1. In de [Azure-portal](https://portal.azure.com), vinden van uw Web-API-app die u eerder hebt gepubliceerd in [aangepaste connectors maken op basis van Web-API's](../logic-apps/custom-connector-build-web-api-app-tutorial.md).

2. Kies **Verificatie/autorisatie** onder **Instellingen**. 

   1. Kies **Aan** onder **App Service-verificatie**.
   2. Selecteer **Aanmelden met Azure Active Directory** bij **Te ondernemen actie wanneer de aanvraag niet is geverifieerd**.
   3. Selecteer **Azure Active Directory** bij **Verificatieproviders**. 

   ![Verificatie/autorisatie kiezen](./media/custom-connector-azure-active-directory-authentication/web-api-app-authentication.png)

3. Op de pagina **Azure Active Directory-instellingen**:

   1. Kies **Express** onder **Beheermodus**.

   2. Selecteer nu de Azure AD-app die door uw web-API-app wordt gebruikt voor verificatie. 
   Kies **Bestaande AD-App selecteren** > **Azure AD-app**.

   3. Selecteer onder **Azure AD-toepassingen** de Azure AD-app die u eerder hebt gemaakt voor uw web-API-app. 
   
   4. Kies **OK** totdat u terug bent op de pagina **Verificatie/autorisatie**.

   Bijvoorbeeld:

   ![Azure AD-app kiezen die uw web-API-app voorstelt](./media/custom-connector-azure-active-directory-authentication/web-api-app-select-azure-ad-app.png)

4. Kies **Opslaan** op de pagina **Verificatie/autorisatie**.

   ![Verificatie-instellingen opslaan](./media/custom-connector-azure-active-directory-authentication/web-api-app-azure-ad-app-save.png)

   Uw web-API-app kan nu verificatie uitvoeren met behulp van Azure AD.

## <a name="4-set-up-azure-ad-authentication-in-your-web-apis-openapi-file"></a>4. Azure AD-verificatie instellen in het OpenAPI-bestand van uw web-API

Open het OpenAPI-bestand voor uw web-API-app, zodat u het object `securityDefintions` en Azure AD-verificatie kunt toevoegen onder de eigenschap `host`. Hier ziet u een voorbeeld met de eigenschap `host` en het object `securityDefinitions`:

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
U kunt uw aangepaste connector nu gaan maken en registreren.

## <a name="next-steps"></a>Volgende stappen

* [De connector registreren](../logic-apps/logic-apps-custom-connector-register.md)
* [Veelgestelde vragen over aangepaste connectors voor Azure Logic Apps, Microsoft Flow en PowerApps](../logic-apps/custom-connector-faq.md)
