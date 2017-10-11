---
title: Apps, machtigingen en toestemming in Azure Active Directory | Microsoft Docs
description: "Azure AD Connect integreert uw on-premises adreslijsten met Azure Active Directory. Hiermee kunt u een algemene identiteit bieden voor Office 365-, Azure- en SaaS-toepassingen die zijn geïntegreerd met Azure AD."
keywords: inleiding tot Azure AD, apps, wat is Azure AD Connect, Active Directory installeren
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 25897cc4-7687-49b6-b0d5-71f51302b6b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/31/2017
ms.author: billmath
ms.reviewer: jesakowi
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 6f6baf5e1538fb280a899065c64ca5688473c04a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="apps-permissions-and-consent-in-azure-active-directory"></a>Apps, machtigingen en toestemming in Azure Active Directory
In Azure Active Directory kunt u toepassingen toevoegen aan uw directory.  De toepassingen kunnen variëren afhankelijk van het type toepassing.  Als u toepassingen in de klassieke portal wilt bekijken, selecteert u een directory en kiest u toepassingen.

![](media/active-directory-apps-permissions-consent/apps1.png)

> [!IMPORTANT]
> Microsoft raadt u aan Azure AD te beheren met het [Azure AD-beheercentrum](https://aad.portal.azure.com) in Azure Portal in plaats van de klassieke Azure portal waarnaar in dit artikel wordt verwezen.

## <a name="types-of-apps"></a>App-typen

1. **Apps met één tenant** </br>
    - **Apps met één tenant** - Ook vaak Line-of-Business-apps (LOB) genoemd. Dit is het geval wanneer iemand binnen uw organisatie een eigen app ontwikkelt en wilt dat gebruikers in de organisatie zich kunnen aanmelden bij de app.
    ![](media/active-directory-apps-permissions-consent/apps2.png)
    - **App Proxy-apps** - Wanneer u een on-premises toepassing gebruikt met Azure AD App Proxy wordt een app met één tenant geregistreerd in uw tenant (naast de App Proxy-service). Deze app vertegenwoordigt uw on-premises toepassing voor alle cloudinteracties (bijvoorbeeld verificatie). (App Proxy vereist Azure AD Basic of hoger.)


2. **Apps met meerdere tenants**
    - **Apps met meerdere tenants waar anderen toestemming voor kunnen geven** - Vergelijkbaar met 'apps met één tenant die uw organisatie ontwikkelt'. Het belangrijkste verschil (naast de logica in de app zelf) is dat gebruikers van andere tenants kunnen ook toestemming kunnen geven voor de app en zich erbij kunnen aanmelden.</br>
    ![](media/active-directory-apps-permissions-consent/apps4.png)
    - **Apps met meerdere tenants die anderen ontwikkelen en waar Contoso toestemming voor kan geven**. (Kort gezegd ook 'toegestane apps'.) Dit is het tegenovergestelde van 'apps met meerdere tenants die uw organisatie ontwikkelt'. Wanneer een andere organisatie een app met meerdere tenants ontwikkelt, kunnen de gebruikers in uw organisatie er toestemming voor geven en zich erbij aanmelden.
    - **Apps van Microsoft** - Apps die Microsoft-services vertegenwoordigen. U geeft toestemming wanneer u zich registreert voor de service. Er is soms sprake van speciale UX en logica voor apps van Microsoft. Vaak wordt hier gebruik van gemaakt voor het definiëren van beleid voor toegang tot de app.</br>
    ![](media/active-directory-apps-permissions-consent/apps3.png)
    - **Vooraf geïntegreerde apps** - Apps die beschikbaar zijn in de Azure AD App Gallery en die u aan uw directory kunt toevoegen voor eenmalige-aanmeldingsfunctionaliteit voor (en in sommige gevallen voor inrichting van) populaire SaaS-apps.
    - **Azure AD met eenmalige aanmelding** - 'Echte' eenmalige aanmelding voor apps die met Azure AD kunnen worden geïntegreerd via een ondersteund aanmeldingsprotocol zoals SAML 2.0 of OpenID Connect. De wizard begeleidt u bij het instellen.
    - **Wachtwoord voor eenmalige aanmelding**: Azure AD bewaart de referenties van de gebruiker voor de app op een veilige manier. De referenties worden door de Azure AD App Access-browserextensie in het aanmeldingsformulier geplaatst. Dit heet ook wel 'password vaulting'.

## <a name="permissions"></a>Machtigingen

Wanneer een app wordt geregistreerd, definieert de gebruiker die de app registreert (de ontwikkelaar) tot welke machtigingen en resources de app toegang nodig heeft. (De resources worden gedefinieerd als andere apps.) Iemand die bijvoorbeeld een e-mail-app bouwt, verklaart dat de app toegang nodig heeft tot de postvakken van de aangemelde gebruiker ('Postvakken van de aangemelde gebruiker openen'). Dit is een machtiging die beschikbaar is via de resource Office 365 Exchange Online:
    
![](media/active-directory-apps-permissions-consent/apps6.png)

Om het mogelijk te maken dat één app (de client) een bepaalde machtiging kan aanvragen bij een andere app (de resource), moet de ontwikkelaar van de resource-app de bestaande machtigingen definiëren. In ons voorbeeld heeft Microsoft, de eigenaar van de resource-app Office 365 Exchange Online, een machtiging met de naam 'Postvakken van de aangemelde gebruiker openen' gedefinieerd.

Bij het definiëren van machtigingen moet de app-ontwikkelaar opgeven of er toestemming voor de machtigingen kan worden gegeven en of een beheerder er toestemming voor moet geven. Op die manier kunnen ontwikkelaars gebruikers in staat stellen om zelf toestemming te geven voor apps die machtigingen met een lage gevoeligheid aanvragen. Beheerders moeten in dit geval wel nog toestemming geven voor machtigingen met een hogere gevoeligheid. De Azure Active Directory-resource-app is bijvoorbeeld zodanig ingesteld dat gebruikers toestemming kunnen geven voor het aanvragen van beperkte, alleen-lezen machtigingen.  Er is echter toestemming van een beheerder nodig voor volledige lees- en schrijftoegang.

Omdat systeemeigen clients niet worden geverifieerd, kan een app die is gedefinieerd als systeemeigen client-app alleen overgedragen machtigingen aanvragen. Dit betekent dat er altijd een daadwerkelijke gebruiker betrokken moet zijn bij het ophalen van een token. Web-apps en web-API's (vertrouwelijke clients) moeten altijd worden geverifieerd door Azure AD bij het ophalen van een toegangstoken. Dit betekent dat ze ook machtigingen die zich beperken tot apps kunnen aanvragen. Dat is bijvoorbeeld nodig als een back-endservice moet worden geverifieerd door een andere back-endservice. Toepassingen waarvoor machtigingen die zich beperken tot de app worden aangevraagd, moeten altijd worden goedgekeurd door een beheerder.

Samenvatting:



- In een app (client) is vastgelegd welke machtigingen nodig zijn voor andere apps (resources).
- In een app (resource) is vastgelegd welke machtigingen beschikbaar zijn voor andere apps (clients).
- Een machtiging kan zich beperken tot één app of kan overgedragen zijn.
- Een overgedragen machtiging kan worden gemarkeerd als 'gebruikerstoestemming toegestaan' of 'beheerderstoestemming vereist'.
- Een app kan functioneren als een client (door op te geven dat machtigingen voor een resource nodig zijn), als een resource (door op te geven welke machtigingen beschikbaar zijn) of als beide.

## <a name="controls"></a>Besturingselementen

Hier volgt een lijst van de verschillende besturingselementen voor beheerders die beschikbaar zijn voor deze zaken. De besturingselementen voor beheerders kunnen worden geopend in de klassieke portal. Ga daarvoor naar Configureren in de map.

![](media/active-directory-apps-permissions-consent/apps7.png)

In Azure Portal gaat u onder **Beheren** naar **Gebruikersinstellingen**.

![](media/active-directory-apps-permissions-consent/apps11.png)



- U bepaalt of gebruikers toestemming kunnen geven voor apps:

Selecteer in de klassieke portal de optie **Users may give applications permissions to access their data.**
![](media/active-directory-apps-permissions-consent/apps8.png) (Gebruikers mogen toepassingen machtigingen verlenen voor toegang tot hun gegevens.)

In Azure Portal selecteert u de optie **Gebruikers kunnen apps toegang tot de gegevens verlenen**.
![](media/active-directory-apps-permissions-consent/apps12.png)



- U bepaalt of gebruikers hun eigen LOB-apps met één tenant mogen registreren. In de klassieke portal selecteert u **Users may add integrated applications.**
![](media/active-directory-apps-permissions-consent/apps9.png) (Gebruikers mogen geïntegreerde toepassingen toevoegen.)

In Azure Portal selecteert u de optie **Gebruikers kunnen apps toegang tot de gegevens verlenen**.
![](media/active-directory-apps-permissions-consent/apps13.png)

>[!NOTE]
>Zelfs als u gebruikers toestaat om LOB-apps met één tenant te registreren, gelden er beperkingen voor wat er mag worden geregistreerd.  
>Dit geldt bijvoorbeeld voor ontwikkelaars die geen directorybeheerder zijn.
>
>- Gebruikers mogen van een app met één tenant geen app met meerdere tenants maken.
>- Bij het registreren van LOB-apps met één tenant mogen gebruikers geen machtigingen voor andere apps aanvragen.
>- Bij het registreren van LOB-apps met één tenant mogen gebruikers geen overgedragen machtigingen voor andere apps aanvragen als er toestemming van een beheerder voor nodig is.
>- Gebruikers mogen geen wijzigingen aanbrengen aan apps waar ze geen eigenaar van zijn.



- U bepaalt of gebruikers zelf vooraf geïntegreerde apps mogen toevoegen waarvoor eenmalige aanmelding met een wachtwoord wordt gebruikt ('password vaulting') ![](media/active-directory-apps-permissions-consent/apps10.png)



- U bepaalt onder welke omstandigheden toepassingen kunnen worden geopend (voorwaardelijke toegang). Dit geldt zowel voor de client-app als de resource-app. U stelt dus beleid voor voorwaardelijke toegang in waarmee u verklaart dat de app Office 365 Exchange Online alleen kan worden geopend vanaf apparaten die aan de voorwaarden voldoen.  Dit beleid wordt ook gehandhaafd wanneer een gebruiker probeert een client-app te gebruiken die machtigingen voor Exchange Online aanvraagt.



- U hebt inzicht in welke apps zijn goedgekeurd en in welke apps worden gebruikt.

1.  Wanneer een gebruiker toestemming heeft voor een app, wordt er een ServicePrincipal-object gemaakt in de tenant. In het controlerapport staat ook dat het ServicePrincipal-object is gemaakt.
2.  De gebruikersrapporten over aanmeldingsactiviteit staat bij welke apps de gebruiker zich aanmeldt. 

## <a name="example"></a>Voorbeeld

Als voorbeeld wordt de FabrikamMail for Office 365-app gebruikt. U hebt gemerkt dat gebruikers in uw tenant zich daarbij aanmelden. FabrikamMail is een e-mail-app voor gebruik op Android-apparaten, gepubliceerd door Fabrikam, Inc. Deze app valt in de categorie 'Apps met meerdere tenants die anderen ontwikkelen en waar Contoso toestemming voor kan geven'.

Als gebruikers toestemming mogen geven, krijgen ze bij de eerste keer aanmelden een toestemmingsprompt te zien: ![](media/active-directory-apps-permissions-consent/apps14.png)

'Uw postvakken openen' is de goedkeuringstekenreeks die aan de gebruiker wordt weergegeven voor de machtiging 'Postvakken van de aangemelde gebruiker openen' van Office 365 Exchange Online, Exchange in het kort.

U kunt de machtigingen bekijken door het ServicePrincipal-object voor Exchange (de resource) te zoeken dat is toegevoegd toen u zich registreerde voor Office 365. U kunt het ServicePrincipal-object zien als een exemplaar van de app in uw tenant dat wordt gebruikt voor het vastleggen van verschillende opties en configuraties.  U kunt het bekijken met behulp van de `Get-AzureADServicePrincipal` in PowerShell.

    PS C:\> Get-AzureADServicePrincipal -ObjectId 383f7b97-6754-4d3d-9474-3908ebcba1c6 | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : Office 365 Exchange Online
    AppId                     : 00000002-0000-0ff1-ce00-000000000000
    AppOwnerTenantId          : 
    AppRoleAssignmentRequired : False
    AppRoles                  : {...}
    DisplayName               : Microsoft.Exchange
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {...
                                , class OAuth2Permission {
                                  AdminConsentDescription : Allows the app to have the same access to mailboxes as the signed-in user via Exchange Web Services.
                                  AdminConsentDisplayName : Access mailboxes as the signed-in user via Exchange Web Services
                                  Id                      : 3b5f3d61-589b-4a3c-a359-5dd4b5ee5bd5
                                  IsEnabled               : True
                                  Type                    : User
                                  UserConsentDescription  : Allows the app full access to your mailboxes on your behalf.
                                  UserConsentDisplayName  : Access your mailboxes
                                  Value                   : full_access_as_user
                                },
                                ...}
    PasswordCredentials       : {}
    PublisherName             : 
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {00000002-0000-0ff1-ce00-000000000000/outlook.office365.com, 00000002-0000-0ff1-ce00-000000000000/mail.office365.com, 00000002-0000-0ff1-ce00-000000000000/outlook.com, 
                                00000002-0000-0ff1-ce00-000000000000/*.outlook.com...}
    Tags                      : {}

Er wordt toestemming gegeven wanneer de gebruiker op Accepteren klikt. Eerst wordt er in de tenant een ServicePrincipal-object gemaakt voor FabrikamMail for Office 365. De ServicePrincipal ziet er ongeveer als volgt uit:

    PS C:\> Get-AzureADServicePrincipal -SearchString "FabrikamMail for Office 365" | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : a8b16333-851d-42e8-acd2-eac155849b37
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : FabrikamMail for Office 365
    AppId                     : aba7c072-2267-4031-8960-e7a2db6e0590
    AppOwnerTenantId          : 4a4076e0-a70f-41c6-b819-6f9c4a86df89
    AppRoleAssignmentRequired : False
    AppRoles                  : {}
    DisplayName               : FabrikamMail for Office 365
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {}
    PasswordCredentials       : {}
    PublisherName             : Fabrikam, Inc.
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {aba7c072-2267-4031-8960-e7a2db6e0590}
    Tags                      : {WindowsAzureActiveDirectoryIntegratedApp}

Als u toestemming geeft voor een app, wordt er een Oauth2PermissionGrant-koppeling gemaakt tussen deze zaken:
  
- het gebruikersobject
- de ServicePrincipalName (SPN) van de client-app
- de ServicePrincipalName (SPN) van de resource-app
- machtigingen in de resource-app.  

In het geval van FabrikamMail, ziet dit er ongeveer als volgt uit:

    PS C:\> Get-AzureADUserOAuth2PermissionGrant -ObjectId ddiggle@aadpremiumlab.onmicrosoft.com | fl *
    
    ClientId    : a8b16333-851d-42e8-acd2-eac155849b37
    ConsentType : Principal
    ExpiryTime  : 05/15/2017 07:02:39 AM
    ObjectId    : M2OxqB2F6EKs0urBVYSbN5d7PzhUZz1NlH25COvLocbJjoxkUFfRQauryBKwBWet
    PrincipalId : 648c8ec9-5750-41d1-abab-c812b00567ad
    ResourceId  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    Scope       : full_access_as_user
    StartTime   : 01/01/0001 12:00:00 AM

(**ClientId** is de service-principal object-id van FabrikamMail (de id die zojuist is gemaakt), **PrincipalId** is de gebruikersobject-id (van de gebruiker die toestemming heeft gegeven), **ResourceId** is de service-principal object-id van Exchange en het bereik is de machtiging in Exchange waar toestemming voor is gegeven).

Als gebruikers geen toestemming mogen geven, krijgen ze een scherm te zien waarin staat dat machtiging is vereist.

