---
title: Verificatie toevoegen aan aangepaste API's - Azure Logic Apps | Microsoft Docs
description: Verificatie voor het aanroepen van uw aangepaste API's vanuit logic apps instellen
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
ms.openlocfilehash: 2528f4318d92bbfdc1008795876f0240a5e3e4f6
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="secure-calls-to-your-custom-apis-from-logic-apps"></a>Aanroepen van uw aangepaste API's vanuit logic apps beveiligen

Als u wilt beveiligen aanroepen naar uw API's, kunt u verificatie van de Azure Active Directory (Azure AD) via de Azure portal instellen zodat u niet hoeft te werk uw code. Of u kunt vereisen en afdwingen van verificatie via de API-code.

## <a name="authentication-options-for-your-api"></a>Opties voor verificatie voor uw API

U kunt aanroepen om uw aangepaste API gebruiken op de volgende manieren te beveiligen:

* [Er is geen codewijzigingen](#no-code): uw API beveiligen met [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) via de Azure portal, dus u hoeft te werk uw code te implementeren van uw API.

  > [!NOTE]
  > Standaard biedt de Azure AD-verificatie in de Azure portal inschakelen niet fijnmazig autorisatie. Deze verificatie vergrendelt bijvoorbeeld uw API op slechts een specifieke tenant, niet op een specifieke gebruiker of de app. 

* [Werk uw API-code](#update-code): uw API beveiligen door af te dwingen [certificaatverificatie](#certificate), [basisverificatie](#basic), of [Azure AD authentication](#azure-ad-code) via code.

<a name="no-code"></a>

### <a name="authenticate-calls-to-your-api-without-changing-code"></a>Aanroepen naar uw API verifiëren zonder code te wijzigen

Hier volgen de algemene stappen voor deze methode:

1. Twee identiteiten voor Azure Active Directory (Azure AD)-toepassing maken: één voor uw logische app en één voor uw web-app (of API-app).

2. Om te verifiëren aanroepen naar uw API, moet u referenties (client-ID en geheim) gebruikt voor de service-principal die is gekoppeld aan de Azure AD-toepassings-id voor uw logische app.

3. De toepassings-id's in de definitie van de logische app opnemen.

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a>Deel 1: Een Azure AD-toepassings-id voor uw logische app maken

Uw logische app gebruikt deze identiteit van de toepassing Azure AD om te verifiëren met Azure AD. U hebt alleen voor het instellen van deze identiteit voor uw directory. Bijvoorbeeld, kunt u met dezelfde identiteit voor alle logic apps, zelfs als u de unieke id's voor elke logische app kunt maken. U kunt deze identiteiten in de Azure portal instellen of [PowerShell](#powershell).

**De toepassings-id voor uw logische app maken in de Azure portal**

1. In de [Azure-portal](https://portal.azure.com "https://portal.azure.com"), kies **Azure Active Directory**. 

2. Bevestig dat u zich in dezelfde map als uw web-app of API-app.

   > [!TIP]
   > Kies uw profiel wilt overschakelen mappen, en selecteer een andere map. Of kies **overzicht** > **Switch directory**.

3. In het menu directory onder **beheren**, kies **App registraties** > **registratie van de nieuwe toepassing**.

   > [!TIP]
   > Standaard bevat de lijst met Apps registraties alle app-registraties in uw directory. Als u wilt alleen in uw app-registraties weergeven naast het zoekvak, selecteer **mijn apps**. 

   ![Maken van nieuwe app-registratie](./media/logic-apps-custom-api-authentication/new-app-registration-azure-portal.png)

4. Geef een naam op voor de toepassingsidentiteit van uw, laat u **toepassingstype** ingesteld op **Web-app / API**, Geef een unieke tekenreeks die is opgemaakt als een domein voor **aanmeldings-URL**, en kies **maken**.

   ![Geef de naam van en aanmelding URL voor toepassings-id](./media/logic-apps-custom-api-authentication/logic-app-identity-azure-portal.png)

   De toepassings-id die u voor uw logische app hebt gemaakt, wordt weergegeven in de lijst met Apps registraties.

   ![Toepassings-id voor uw logische app](./media/logic-apps-custom-api-authentication/logic-app-identity-created.png)

5. Selecteer de nieuwe toepassingsidentiteit in de applijst registraties. Kopieer en sla de **toepassings-ID** als 'Client-ID' wilt gebruiken voor uw logische app in deel 3.

   ![Kopiëren en opslaan van toepassings-ID voor de logische app](./media/logic-apps-custom-api-authentication/logic-app-application-id.png)

6. Als de identity-instellingen van de toepassing niet weergegeven worden, kiest u **instellingen** of **alle instellingen**.

7. Onder **API-toegang**, kies **sleutels**. Onder **beschrijving**, Geef een naam voor uw sleutel. Onder **verloopt**, selecteert u een duur voor uw sleutel.

   De sleutel die u maakt, fungeert als de toepassings-id 'de geheime' of het wachtwoord voor uw logische app.

   ![Sleutel voor de id van logische app maken](./media/logic-apps-custom-api-authentication/create-logic-app-identity-key-secret-password.png)

8. Kies op de werkbalk **opslaan**. Onder **waarde**, uw sleutel wordt nu weergegeven. 
**Zorg ervoor dat kopiëren en opslaan van uw sleutel** voor later gebruik omdat de sleutel wordt verborgen wanneer u laat de **sleutels** pagina.

   Wanneer u uw logische app in deel 3 configureert, kunt u deze sleutel als de 'geheim' of het wachtwoord opgeven.

   ![Kopieer en sleutel opslaan voor later](./media/logic-apps-custom-api-authentication/logic-app-copy-key-secret-password.png)

<a name="powershell"></a>

**De toepassings-id voor uw logische app maken in PowerShell**

U kunt deze taak via Azure Resource Manager met PowerShell uitvoeren. Voer deze opdrachten uit in PowerShell:

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. Zorg ervoor dat u kopieert de **Tenant-ID** (GUID voor uw Azure AD-tenant), de **toepassings-ID**, en het wachtwoord dat u gebruikt.

Informatie voor meer informatie over hoe [een service-principal maken met PowerShell voor toegang tot bronnen](../azure-resource-manager/resource-group-authenticate-service-principal.md).

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a>Deel 2: Een Azure AD-toepassings-id voor uw web-app of API-app maken

Als uw web-app of API-app al is geïmplementeerd, kunt u verificatie inschakelen en de toepassings-id in de Azure portal maken. Anders kunt u [authentication inschakelen wanneer u met een Azure Resource Manager-sjabloon implementeert](#authen-deploy). 

**De toepassings-id maken en verificatie in de Azure-portal voor geïmplementeerde apps inschakelen**

1. In de [Azure-portal](https://portal.azure.com "https://portal.azure.com"), zoeken en selecteert u uw web-app of API-app. 

2. Onder **instellingen**, kies **verificatie/autorisatie**. Onder **verificatie van App Service**, schakelt u verificatie **op**. Onder **verificatieproviders**, kies **Azure Active Directory**.

   ![Authentication inschakelen](./media/logic-apps-custom-api-authentication/custom-web-api-app-authentication.png)

3. Maak nu een toepassings-id voor uw web-app of API-app zoals hier wordt weergegeven. Op de **Azure Active Directory-instellingen** pagina **beheermodus** naar **Express**. Kies **nieuwe AD-App maken**. Geef een naam op voor de toepassingsidentiteit van uw en kies **OK**. 

   ![Toepassings-id voor uw web-app of API-app maken](./media/logic-apps-custom-api-authentication/custom-api-application-identity.png)

4. Op de **verificatie / autorisatie** pagina **opslaan**.

U moet nu de client-ID en tenant-ID vinden voor de identiteit die is gekoppeld aan uw web-app of API-app. U gebruikt deze id in deel 3. Zo kunt u doorgaan met deze stappen voor de Azure-portal.

**De client-ID en het tenant-ID toepassings-id vinden voor uw web-app of API-app in de Azure portal**

1. Onder **verificatieproviders**, kies **Azure Active Directory**. 

   !['Azure Active Directory' kiezen](./media/logic-apps-custom-api-authentication/custom-api-app-identity-client-id-tenant-id.png)

2. Op de **Azure Active Directory-instellingen** pagina **beheermodus** naar **Geavanceerd**.

3. Kopieer de **Client-ID**, en sla deze GUID voor gebruik in deel 3.

   > [!TIP] 
   > Als **Client-ID** en **Url-verlener** niet worden weergegeven, vernieuw de Azure-portal en Herhaal stap 1.

4. Onder **Url-verlener**, kopiëren en opslaan van alleen de GUID voor deel 3. U kunt ook deze GUID gebruiken in uw web-app of sjabloon API-app-implementatie, indien nodig.

   Deze GUID is van de tenant van uw specifieke GUID ('tenant-ID') en moet worden weergegeven in deze URL:`https://sts.windows.net/{GUID}`

5. Zonder uw wijzigingen worden opgeslagen, sluit u de **Azure Active Directory-instellingen** pagina.

<a name="authen-deploy"></a>

**Verificatie inschakelen wanneer u met een Azure Resource Manager-sjabloon implementeren**

U moet nog steeds een Azure AD-toepassings-id voor uw web-app of API-app die verschilt van de identiteit van de app maken voor uw logische app. Volg de vorige stappen in deel 2 voor de Azure-portal voor het maken van de toepassings-id. 

U kunt ook de stappen in deel 1 maar zorg ervoor dat u het gebruik van uw web-app of API-app werkelijke `https://{URL}` voor **aanmeldings-URL** en **App ID URI**. U hebt de client-ID en de tenant-ID voor gebruik in uw app implementatiesjabloon en ook voor deel 3 opslaan van deze stappen.

> [!NOTE]
> Wanneer u de Azure AD-toepassings-id voor uw web-app of API-app maakt, moet u de Azure-portal, niet PowerShell gebruiken. De PowerShell-cmdlet instellen niet de vereiste machtigingen voor gebruikers zich aanmelden bij een website.

Nadat u de client-ID en tenant-ID hebt ontvangen, moet u deze id opnemen als een subresource van uw web-app of API-app in de implementatiesjabloon voor:

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

Automatisch implementeren een lege web-app en een logische app samen met Azure Active Directory-verificatie, [hier de volledige sjabloon weergeven](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), of klik op **implementeren in Azure** hier:

[![Implementeren in Azure](media/logic-apps-custom-api-authentication/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

#### <a name="part-3-populate-the-authorization-section-in-your-logic-app"></a>Deel 3: De autorisatie-sectie in uw logische app vullen

De vorige sjabloon is al in deze sectie autorisatie is ingesteld, maar als u rechtstreeks de logische app maakt, moet u de volledige autorisatie sectie opnemen.

Open de definitie van de logische app in de codeweergave, gaat u naar de **HTTP** sectie actie vindt de **autorisatie** sectie en deze regel:

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| Element | Vereist | Beschrijving | 
| ------- | -------- | ----------- | 
| Tenant | Ja | De GUID voor de Azure AD-tenant | 
| doelgroep | Ja | De GUID voor de doelresource die u openen wilt, die de client-id van de toepassings-id voor uw web-app of API-app | 
| clientId | Ja | De GUID voor de client aanvragen van toegang tot de client-id van de identiteit voor uw logische app | 
| Geheim | Ja | De sleutel of het wachtwoord van de toepassings-id voor de client die het toegangstoken aanvraagt | 
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

#### <a name="certificate-authentication"></a>Verificatie via certificaat

U kunt certificaten gebruiken voor het valideren van de binnenkomende aanvragen van uw logische app aan uw web-app of API-app. Als u uw code instelt, meer [TLS wederzijdse verificatie configureren](../app-service/app-service-web-configure-tls-mutual-auth.md).

In de **autorisatie** sectie, voeg deze regel: 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| Element | Vereist | Beschrijving | 
| ------- | -------- | ----------- | 
| type | Ja | Het verificatietype. Voor SSL-certificaten voor client, moet de waarde `ClientCertificate`. | 
| wachtwoord | Ja | Het wachtwoord voor het openen van het clientcertificaat (PFX-bestand) | 
| PFX | Ja | De base64-gecodeerde inhoud van het clientcertificaat (PFX-bestand) | 
|||| 

<a name="basic"></a>

#### <a name="basic-authentication"></a>Basisverificatie

U kunt basisverificatie, zoals een gebruikersnaam en wachtwoord gebruiken voor het valideren van binnenkomende aanvragen van uw logische app aan uw web-app of API-app. Basisverificatie is een algemene patroon en kunt u deze verificatie gebruiken in een andere taal gebruikt voor het ontwikkelen van uw web-app of API-app.

In de **autorisatie** sectie, voeg deze regel:

`{"type": "basic", "username": "username", "password": "password"}`.

| Element | Vereist | Beschrijving | 
| ------- | -------- | ----------- | 
| type | Ja | Het verificatietype dat u wilt gebruiken. Voor basisverificatie, moet de waarde `Basic`. | 
| gebruikersnaam | Ja | De gebruikersnaam die u wilt gebruiken voor verificatie | 
| wachtwoord | Ja | Het wachtwoord dat u wilt gebruiken voor verificatie | 
|||| 

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a>Azure Active Directory-verificatie via code

Standaard biedt de Azure AD-verificatie in de Azure portal inschakelen niet fijnmazig autorisatie. Deze verificatie vergrendelt bijvoorbeeld uw API op slechts een specifieke tenant, niet op een specifieke gebruiker of de app. 

Pak API om toegang te beperken aan uw logische app via de programmacode, de header die de JSON web token (JWT heeft). Controleer de aanroeper ID en aanvragen die niet overeenkomen met weigeren.

<!-- Going further, to implement this authentication entirely in your own code, 
and not use the Azure portal, learn how to 
[authenticate with on-premises Active Directory in your Azure app](../app-service/app-service-authentication-overview.md).

To create an application identity for your logic app and use that identity to call your API, 
you must follow the previous steps. -->

## <a name="next-steps"></a>Volgende stappen

* [Implementeren en het aanroepen van aangepaste API's vanuit logic app-werkstromen](../logic-apps/logic-apps-custom-api-host-deploy-call.md)