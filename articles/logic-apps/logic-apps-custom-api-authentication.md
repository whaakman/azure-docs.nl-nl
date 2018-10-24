---
title: Verificatie toevoegen aan aangepaste API's - Azure Logic Apps | Microsoft Docs
description: Verificatie instellen voor het aanroepen van aangepaste API's van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/22/2017
ms.openlocfilehash: 62f71f29f813cd30c7d8e3c7f37f41677cf07364
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49956371"
---
# <a name="secure-calls-to-custom-apis-from-azure-logic-apps"></a>Beveiligde aanroepen naar aangepaste API's van Azure Logic Apps

Als u wilt aanroepen naar uw API's beveiligen, kunt u verificatie van Azure Active Directory (Azure AD) via de Azure-portal instellen, zodat u hoeft uw code bijwerken. Of u kunt verificatie vereisen en afdwingen via de code van uw API.

## <a name="authentication-options-for-your-api"></a>Verificatie-opties voor uw API

U kunt aanroepen naar uw aangepaste API op de volgende manieren beveiligen:

* [Er zijn geen codewijzigingen](#no-code): uw API beschermen met [Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md) via Azure portal, dus u hoeft te uw code bijwerken of opnieuw implementeren van uw API.

  > [!NOTE]
  > Standaard biedt de Azure AD-verificatie in Azure portal inschakelen geen fijnmazig autorisatie. Bijvoorbeeld, deze verificatie Hiermee vergrendelt u uw API op slechts een specifieke tenant, niet op een specifieke gebruiker of de app. 

* [Uw API code bijwerken](#update-code): uw API beveiligen door af te dwingen [certificaatverificatie](#certificate), [basisverificatie](#basic), of [Azure AD-verificatie](#azure-ad-code) via de code.

<a name="no-code"></a>

### <a name="authenticate-calls-to-your-api-without-changing-code"></a>Verificatie van aanroepen naar de API zonder code te wijzigen

Hier volgen de algemene stappen voor deze methode:

1. Maak twee identiteiten van Azure Active Directory (Azure AD)-toepassing: één voor uw logische app en één voor uw web-app (of de API-app).

2. Als u wilt verifiëren aanroepen naar uw API, door de referenties (client-ID en geheim) te gebruiken voor de service-principal die is gekoppeld aan de Azure AD-identiteit voor uw logische app.

3. De toepassings-id's opnemen in de definitie van uw logische app.

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a>Deel 1: Een Azure AD-toepassing-id voor uw logische app maken

Uw logische app maakt gebruik van deze Azure AD-identiteit te verifiëren bij Azure AD. U hebt alleen voor het instellen van deze identiteit voor uw directory. Bijvoorbeeld, kunt u dezelfde identiteit gebruiken voor al uw logic apps, zelfs als u de unieke id's voor elke logische app kunt maken. U kunt deze identiteiten in de Azure-portal instellen of [PowerShell](#powershell).

**De toepassings-id voor uw logische app maken in Azure portal**

1. In de [Azure-portal](https://portal.azure.com "https://portal.azure.com"), kiest u **Azure Active Directory**. 

2. Controleer of u zich in dezelfde map als uw web-app of API-app.

   > [!TIP]
   > Als u wilt schakelen tussen mappen, kiest u uw profiel en selecteer een andere directory. Of kies **overzicht** > **schakelen tussen mappen**.

3. In het menu directory onder **beheren**, kiest u **App-registraties** > **nieuwe toepassing registreren**.

   > [!TIP]
   > Standaard bevat de lijst met app-registraties alle app-registraties in uw directory. Als u alleen uw app-registraties, naast het zoekvak, selecteer **mijn apps**. 

   ![Maken van nieuwe app-registratie](./media/logic-apps-custom-api-authentication/new-app-registration-azure-portal.png)

4. Geef een naam op voor de toepassingsidentiteit van uw, laat u **toepassingstype** ingesteld op **Web-app / API**, bieden een unieke tekenreeks die is opgemaakt als een domein voor **aanmeldings-URL**, en kies  **Maak**.

   ![Geef naam op en aanmeldings-URL voor de toepassings-id](./media/logic-apps-custom-api-authentication/logic-app-identity-azure-portal.png)

   De toepassings-id die u hebt gemaakt voor uw logische app nu wordt weergegeven in de lijst met app-registraties.

   ![Toepassings-id voor uw logische app](./media/logic-apps-custom-api-authentication/logic-app-identity-created.png)

5. Selecteer uw nieuwe toepassing-id in de lijst van de app-registraties. Kopieer en bewaar de **toepassings-ID** gebruiken als de "client-ID voor uw logische app in deel 3.

   ![Kopiëren en opslaan van toepassings-ID voor de logische app](./media/logic-apps-custom-api-authentication/logic-app-application-id.png)

6. Als uw toepassing identity-instellingen zijn niet zichtbaar, kiest u **instellingen** of **alle instellingen**.

7. Onder **API-toegang**, kiest u **sleutels**. Onder **beschrijving**, Geef een naam op voor uw sleutel. Onder **verloopt**, selecteer een tijdsduur voor de sleutel.

   De sleutel die u maakt fungeert als de toepassings-id 'geheim' of het wachtwoord voor uw logische app.

   ![Sleutel voor de id van logische app maken](./media/logic-apps-custom-api-authentication/create-logic-app-identity-key-secret-password.png)

8. Kies op de werkbalk **opslaan**. Onder **waarde**, de sleutel wordt nu weergegeven. 
**Zorg ervoor dat u kopiëren en opslaan van uw sleutel** voor later gebruik omdat de sleutel wordt verborgen wanneer u laat de **sleutels** pagina.

   Wanneer u uw logische app in deel 3 configureert, geeft u deze sleutel als het "geheim aan" of het wachtwoord.

   ![Kopieer en bewaar de sleutel voor later](./media/logic-apps-custom-api-authentication/logic-app-copy-key-secret-password.png)

<a name="powershell"></a>

**De toepassings-id voor uw logische app maken in PowerShell**

U kunt deze taak via Azure Resource Manager met PowerShell uitvoeren. Voer deze opdrachten uit in PowerShell:

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. Zorg ervoor dat u kopieert de **Tenant-ID** (GUID voor uw Azure AD-tenant), de **toepassings-ID**, en het wachtwoord die u hebt gebruikt.

Informatie voor meer informatie over hoe u [een service-principal maken met PowerShell voor toegang tot resources](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a>Deel 2: Een Azure AD-toepassing-id voor uw web-app of API-app maken

Als uw web-app of API-app al is geïmplementeerd, kunt u verificatie inschakelen en de toepassings-id maken in Azure portal. Anders kunt u [authentication inschakelen wanneer u met een Azure Resource Manager-sjabloon implementeert](#authen-deploy). 

**De toepassings-id maken en schakelt u verificatie in Azure portal voor geïmplementeerde apps**

1. In de [Azure-portal](https://portal.azure.com "https://portal.azure.com"), zoek en selecteer uw web-app of API-app. 

2. Onder **instellingen**, kiest u **verificatie/autorisatie**. Onder **App Service-verificatie**, schakelt u verificatie **op**. Onder **verificatieproviders**, kiest u **Azure Active Directory**.

   ![Verificatie inschakelen](./media/logic-apps-custom-api-authentication/custom-web-api-app-authentication.png)

3. Maak nu een toepassings-id voor uw web-app of API-app zoals hier wordt weergegeven. Op de **Azure Active Directory-instellingen** pagina, stelt u **beheermodus** naar **Express**. Kies **nieuwe AD-App maken**. Geef een naam op voor de toepassingsidentiteit van uw en kies **OK**. 

   ![Toepassings-id voor uw web-app of API-app maken](./media/logic-apps-custom-api-authentication/custom-api-application-identity.png)

4. Kies **Opslaan** op de pagina **Verificatie/autorisatie**.

Nu moet u de client-ID en tenant-ID vinden voor de toepassings-id die is gekoppeld aan uw web-app of API-app. U gebruikt deze id's in deel 3. Zo kunt u doorgaan met deze stappen voor het Azure-portal.

**De client-ID van de toepassingsidentiteit van de en de tenant-ID vinden voor uw web-app of API-app in Azure portal**

1. Onder **verificatieproviders**, kiest u **Azure Active Directory**. 

   ![Azure Active Directory kiezen](./media/logic-apps-custom-api-authentication/custom-api-app-identity-client-id-tenant-id.png)

2. Op de **Azure Active Directory-instellingen** pagina, stelt u **beheermodus** naar **Geavanceerd**.

3. Kopieer de **Client-ID**, en op te slaan die GUID voor gebruik in deel 3.

   > [!TIP] 
   > Als **Client-ID** en **Url-verlener** niet worden weergegeven, probeer te vernieuwen van de Azure-portal en Herhaal stap 1.

4. Onder **Url-verlener**, kopiëren en opslaan van alleen de GUID voor deel 3. U kunt ook deze GUID gebruiken in uw web-app of de sjabloon voor API-app-implementatie, indien nodig.

   Deze GUID wordt van uw specifieke tenant GUID ('tenant-ID') en moet worden weergegeven in deze URL: `https://sts.windows.net/{GUID}`

5. Zonder uw wijzigingen worden opgeslagen, sluit u de **Azure Active Directory-instellingen** pagina.

<a name="authen-deploy"></a>

**Verificatie inschakelen wanneer u met een Azure Resource Manager-sjabloon implementeert**

U moet nog steeds een Azure AD-toepassings-id voor uw web-app of API-app die verschilt van de identiteit van de app maken voor uw logische app. Volg de vorige stappen in deel 2 voor de Azure-portal voor het maken van de toepassings-id. 

U kunt ook de stappen in deel 1 maar zorg ervoor dat u uw web-app of API-app werkelijke `https://{URL}` voor **aanmeldings-URL** en **App ID URI**. U hebt om op te slaan van de client-ID en de tenant-ID voor gebruik in de sjabloon voor de implementatie van uw app en ook voor deel 3 van deze stappen.

> [!NOTE]
> Wanneer u de Azure AD-identiteit voor uw web-app of API-app maakt, moet u de Azure portal, niet PowerShell gebruiken. De PowerShell-commandlet instellen niet de vereiste machtigingen voor gebruikers aanmelden bij een website.

Nadat u de client-ID en tenant-ID hebt ontvangen, zijn onder andere deze id als een subbron van uw web-app of API-app in uw sjabloon voor de implementatie:

``` json
"resources": [ {
    "apiVersion": "2015-08-01",
    "name": "web",
    "type": "config",
    "dependsOn": ["[concat('Microsoft.Web/sites/','parameters('webAppName'))]"],
    "properties": {
        "siteAuthEnabled": true,
        "siteAuthSettings": {
            "clientId": "{client-ID}",
            "issuer": "https://sts.windows.net/{tenant-ID}/",
        }
    }
} ]
```

Automatisch implementeren een lege web-app en een logische app, samen met Azure Active Directory-verificatie, [hier de volledige sjabloon bekijken](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), of klik op **implementeren in Azure** hier:

[![Implementeren in Azure](media/logic-apps-custom-api-authentication/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

#### <a name="part-3-populate-the-authorization-section-in-your-logic-app"></a>Deel 3: Het vullen van de autorisatie-sectie in uw logische app

De vorige sjabloon heeft al in deze sectie autorisatie is ingesteld, maar als u rechtstreeks de logische app schrijft, moet u de volledige autorisatie-sectie opnemen.

Open de definitie van uw logische app in de weergave van code, gaat u naar de **HTTP** sectie actie vindt de **autorisatie** uit en neem deze regel:

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| Element | Vereist | Beschrijving | 
| ------- | -------- | ----------- | 
| tenant | Ja | De GUID voor de Azure AD-tenant | 
| Doelgroep | Ja | De GUID voor de doelresource die u openen wilt, die de client-id van de toepassings-id voor uw web-app of API-app | 
| ClientId | Ja | De GUID voor de client aanvragen van toegang, die de client-id van de toepassings-id voor uw logische app | 
| geheim | Ja | De sleutel of het wachtwoord van de toepassings-id voor de client die het toegangstoken wordt aangevraagd | 
| type | Ja | Het verificatietype. De waarde voor ActiveDirectoryOAuth verificatie, heeft `ActiveDirectoryOAuth`. | 
|||| 

Bijvoorbeeld:

``` json
{
   "actions": {
      "some-action": {
         "conditions": [],
         "inputs": {
            "method": "post",
            "uri": "https://your-api-azurewebsites.net/api/your-method",
            "authentication": {
               "tenant": "tenant-ID",
               "audience": "client-ID-from-azure-ad-app-for-web-app-or-api-app",
               "clientId": "client-ID-from-azure-ad-app-for-logic-app",
               "secret": "key-from-azure-ad-app-for-logic-app",
               "type": "ActiveDirectoryOAuth"
            }
         },
      }
   }
}
```

<a name="update-code"></a>

### <a name="secure-api-calls-through-code"></a>Beveiligde API-aanroepen via code

<a name="certificate"></a>

#### <a name="certificate-authentication"></a>Certificaatverificatie

U kunt clientcertificaten gebruiken voor het valideren van de inkomende aanvragen van uw logische app aan uw web-app of API-app. Als u uw code instelt, meer [wederzijdse TLS-verificatie configureren](../app-service/app-service-web-configure-tls-mutual-auth.md).

In de **autorisatie** sectie, voeg deze regel: 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| Element | Vereist | Beschrijving | 
| ------- | -------- | ----------- | 
| type | Ja | Het verificatietype. Voor SSL-clientcertificaten, de waarde moet `ClientCertificate`. | 
| wachtwoord | Ja | Het wachtwoord voor toegang tot de clientcertificaat (PFX-bestand) | 
| PFX | Ja | De met base64 gecodeerde inhoud van het clientcertificaat (PFX-bestand) | 
|||| 

<a name="basic"></a>

#### <a name="basic-authentication"></a>Basisverificatie

Voor het valideren van binnenkomende aanvragen van uw logische app aan uw web-app of API-app, kunt u basisverificatie wordt gebruikt, zoals een gebruikersnaam en wachtwoord. Basisverificatie is een algemeen patroon en kunt u deze verificatie in elke taal die wordt gebruikt voor het bouwen van uw web-app of API-app.

In de **autorisatie** sectie, voeg deze regel:

`{"type": "basic", "username": "username", "password": "password"}`.

| Element | Vereist | Beschrijving | 
| ------- | -------- | ----------- | 
| type | Ja | Het verificatietype dat u wilt gebruiken. Voor basisverificatie, de waarde moet `Basic`. | 
| gebruikersnaam | Ja | De gebruikersnaam die u wilt gebruiken voor verificatie | 
| wachtwoord | Ja | Het wachtwoord dat u wilt gebruiken voor verificatie | 
|||| 

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a>Azure Active Directory-verificatie via code

Standaard biedt de Azure AD-verificatie in Azure portal inschakelen geen fijnmazig autorisatie. Bijvoorbeeld, deze verificatie Hiermee vergrendelt u uw API op slechts een specifieke tenant, niet op een specifieke gebruiker of de app. 

Als u wilt beperken API-toegang tot uw logische app via de programmacode, pak de header die de JSON webtoken (JWT) heeft. Controleren van de beller-id en aanvragen die niet overeenkomen met afwijzen.

<!-- Going further, to implement this authentication entirely in your own code, 
and not use the Azure portal, learn how to 
[authenticate with on-premises Active Directory in your Azure app](../app-service/app-service-authentication-overview.md).

To create an application identity for your logic app and use that identity to call your API, 
you must follow the previous steps. -->

## <a name="next-steps"></a>Volgende stappen

* [Implementeren en aangepaste API's aanroepen vanuit logic app workflows](../logic-apps/logic-apps-custom-api-host-deploy-call.md)