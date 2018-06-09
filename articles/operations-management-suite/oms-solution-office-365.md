---
title: Office 365-oplossing in Azure | Microsoft Docs
description: Dit artikel bevat informatie over configuratie en gebruik van de Office 365-oplossing in Azure.  Het bevat een gedetailleerde beschrijving van de Office 365-records gemaakt in logboekanalyse.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: bwren
ms.openlocfilehash: e93860328ed6a31b7a06cc3420fb50ab4af9a00c
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236100"
---
# <a name="office-365-management-solution-in-azure-preview"></a>Office 365-oplossing in Azure (Preview)

![Office 365-logo](media/oms-solution-office-365/icon.png)

De Office 365-oplossing kunt u uw Office 365-omgeving in logboekanalyse bewaken.

- Controleren van gebruikersactiviteiten op uw Office 365-accounts te analyseren gebruikspatronen en trends voor gebruikersgedrag. U kunt bijvoorbeeld specifieke gebruiksscenario's, zoals bestanden die worden gedeeld buiten uw organisatie of de meest populaire SharePoint-sites ophalen.
- Activiteiten bijhouden van wijzigingen in de configuratie of hoge bevoegdheid bewerkingen beheerder bewaken.
- Detecteren en onderzoeken van ongewenste gebruikersgedrag die behoeften van uw organisatie kan worden aangepast.
- Controle en naleving demonstreren. U kunt bijvoorbeeld toegang bestandsbewerkingen op vertrouwelijke bestanden, wat u bij de controle en naleving waarborgen helpen kunnen bewaken.
- Uitvoeren van operationele problemen oplossen met behulp [Meld zoekopdrachten](../log-analytics/log-analytics-log-search.md) boven op Office 365-activiteitsgegevens van uw organisatie.

## <a name="prerequisites"></a>Vereisten
Het volgende is vereist voordat u deze oplossing wordt geïnstalleerd en geconfigureerd.

- Organisatie Office 365-abonnement.
- Referenties voor een gebruikersaccount dat een globale beheerder.
- Audit om gegevens te ontvangen, moet u [controle configureren](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&rs=en-US&ad=US#PickTab=Before_you_begin) in uw Office 365-abonnement.  Houd er rekening mee dat [Postvakcontrole](https://technet.microsoft.com/library/dn879651.aspx) wordt afzonderlijk geconfigureerd.  U kunt nog steeds installeren van de oplossing en andere gegevens verzamelen als controle is niet geconfigureerd.
 

## <a name="management-packs"></a>Management packs
Deze oplossing wordt niet geïnstalleerd voor management packs [verbonden beheergroepen](../log-analytics/log-analytics-om-agents.md).
  
## <a name="install-and-configure"></a>Installeren en configureren
Voeg eerst de [Office 365-oplossing aan uw abonnement](/monitoring/monitoring-solutions.md#install-a-management-solution). Nadat deze toegevoegd, moet u de configuratiestappen uitvoeren in deze sectie zodat deze toegang krijgt tot uw Office 365-abonnement.

### <a name="required-information"></a>Vereiste informatie
Voordat u deze procedure begint, moet u de volgende informatie verzamelen.

Vanuit de werkruimte voor logboekanalyse:

- Werkruimtenaam: de werkruimte waar de Office 365-gegevens worden verzameld.
- De naam van resourcegroep: de resourcegroep die de werkruimte bevat.
- Azure-abonnement-ID: het abonnement dat in de werkruimte bevat.

Via uw Office 365-abonnement:

- Gebruikersnaam: E-mailadres van een Administrator-account.
- Tenant-ID: De unieke ID voor Office 365-abonnement.
- Client-ID: 16 tekens bestaande tekenreeks met Office 365-clientupdates.
- Clientgeheim: Gecodeerde tekenreeks die nodig zijn voor verificatie.

### <a name="create-an-office-365-application-in-azure-active-directory"></a>Een Office 365-toepassing maken in Azure Active Directory
De eerste stap is het maken van een toepassing in Azure Active Directory die door het systeem wordt gebruikt voor toegang tot uw Office 365-oplossing.

1. Meld u aan bij de Azure-portal op [https://portal.azure.com](https://portal.azure.com/).
1. Selecteer **Azure Active Directory** en vervolgens **App registraties**.
1. Klik op **Nieuwe toepassing registreren**.

    ![Registratie van de app toevoegen](media/oms-solution-office-365/add-app-registration.png)
1. Voer een toepassing **naam** en **aanmeldings-URL**.  De naam moet beschrijvend zijn.  Gebruik _http://localhost_ voor de URL en houd _Web-app / API_ voor de **toepassingstype**
    
    ![App maken](media/oms-solution-office-365/create-application.png)
1. Klik op **maken** en informatie over de toepassing te valideren.

    ![Geregistreerde app](media/oms-solution-office-365/registered-app.png)

### <a name="configure-application-for-office-365"></a>Configureer de toepassing voor Office 365

1. Klik op **instellingen** openen de **instellingen** menu.
1. Selecteer **eigenschappen**. Wijziging **Multi-verpachte** naar _Ja_.

    ![Instellingen voor multitenant](media/oms-solution-office-365/settings-multitenant.png)

1. Selecteer **vereist machtigingen** in de **instellingen** menu en klik vervolgens op **toevoegen**.
1. Klik op **selecteert u een API** en vervolgens **Management-API's van Office 365**. Klik op **Office 365 Management-API's**. Klik op **Selecteren**.

    ![API selecteren](media/oms-solution-office-365/select-api.png)

1. Onder **machtigingen selecteren** de volgende opties beschikbaar voor zowel **Toepassingsmachtigingen** en **overgedragen machtigingen**:
    - Gegevens over de servicestatus van uw organisatie lezen
    - Activiteitengegevens van uw organisatie lezen
    - Activiteitenrapporten van uw organisatie lezen

    ![API selecteren](media/oms-solution-office-365/select-permissions.png)

1. Klik op **Selecteer** en vervolgens **gedaan**.
1. Klik op **machtigingen verlenen** en klik vervolgens op **Ja** wanneer u wordt gevraagd voor verificatie.

    ![Machtigingen verlenen](media/oms-solution-office-365/grant-permissions.png)

### <a name="add-a-key-for-the-application"></a>Een sleutel voor de toepassing toevoegen

1. Selecteer **sleutels** in de **instellingen** menu.
1. Typ in een **beschrijving** en **duur** voor de nieuwe sleutel.
1. Klik op **opslaan** en kopieer de **waarde** die wordt gegenereerd.

    ![Sleutels](media/oms-solution-office-365/keys.png)

### <a name="add-admin-consent"></a>Toestemming van de beheerder toevoegen
Als u wilt de Administrator-account voor het eerst inschakelt, moet u beheerdersrechten toestemming voor de toepassing opgeven. U kunt dit doen met een PowerShell-script. 

1. Sla het volgende script als *office365_consent.ps1*.

    ```
    param (
        [Parameter(Mandatory=$True)][string]$WorkspaceName,     
        [Parameter(Mandatory=$True)][string]$ResourceGroupName,
        [Parameter(Mandatory=$True)][string]$SubscriptionId
    )
    
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
        
    IF ($Subscription -eq $null)
        {Login-AzureRmAccount -ErrorAction Stop}
    $Subscription = (Select-AzureRmSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $Workspace = (Set-AzureRMOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
    $WorkspaceLocation= $Workspace.Location
    $WorkspaceLocationShort= $Workspace.PortalUrl.Split("/",$option)[1].Split(".",$option)[0]
    $WorkspaceLocation
    
    Function AdminConsent{
    
    $domain='login.microsoftonline.com'
    $mmsDomain = 'login.mms.microsoft.com'
    $WorkspaceLocationShort= $Workspace.PortalUrl.Split("/",$option)[1].Split(".",$option)[0]
    $WorkspaceLocationShort
    $WorkspaceLocation
    switch ($WorkspaceLocation.Replace(" ","").ToLower()) {
           "eastus"   {$OfficeAppClientId="d7eb65b0-8167-4b5d-b371-719a2e5e30cc"; break}
           "westeurope"   {$OfficeAppClientId="c9005da2-023d-40f1-a17a-2b7d91af4ede"; break}
           "southeastasia"   {$OfficeAppClientId="09c5b521-648d-4e29-81ff-7f3a71b27270"; break}
           "australiasoutheast"  {$OfficeAppClientId="f553e464-612b-480f-adb9-14fd8b6cbff8"; break}   
           "westcentralus"  {$OfficeAppClientId="98a2a546-84b4-49c0-88b8-11b011dc8c4e"; break}
           "japaneast"   {$OfficeAppClientId="b07d97d3-731b-4247-93d1-755b5dae91cb"; break}
           "uksouth"   {$OfficeAppClientId="f232cf9b-e7a9-4ebb-a143-be00850cd22a"; break}
           "centralindia"   {$OfficeAppClientId="ffbd6cf4-cba8-4bea-8b08-4fb5ee2a60bd"; break}
           "canadacentral"  {$OfficeAppClientId="c2d686db-f759-43c9-ade5-9d7aeec19455"; break}
           "eastus2"  {$OfficeAppClientId="7eb65b0-8167-4b5d-b371-719a2e5e30cc"; break}
           "westus2"  {$OfficeAppClientId="98a2a546-84b4-49c0-88b8-11b011dc8c4e"; break} #Need to check
           "usgovvirginia" {$OfficeAppClientId="c8b41a87-f8c5-4d10-98a4-f8c11c3933fe"; 
                             $domain='login.microsoftonline.us';
                             $mmsDomain = 'usbn1.login.oms.microsoft.us'; break} # US Gov Virginia
           default {$OfficeAppClientId="55b65fb5-b825-43b5-8972-c8b6875867c1";
                    $domain='login.windows-ppe.net'; break} #Int
        }
    
        $OfficeAppRedirectUrl="https://$($WorkspaceLocationShort).$($mmsDomain)/Office365/Authorize"
        $OfficeAppRedirectUrl
        $domain
        Start-Process -FilePath  "https://$($domain)/common/adminconsent?client_id=$($OfficeAppClientId)&state=12345&redirect_uri=$($OfficeAppRedirectUrl)"
    }
    
    AdminConsent -ErrorAction Stop
    ```

2. Voer het script met de volgende opdracht.
    ```
    .\office365_consent.ps1 -WorkspaceName <Workspace name> -ResourceGroupName <Resource group name> -SubscriptionId <Subscription ID>
    ```
    Voorbeeld:

    ```
    .\office365_consent.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631- yyyyyyyyyyyy'
    ```

1. U krijgt een lijkt op het onderstaande venster. Klik op **accepteren**.
    
    ![Toestemming van de beheerder](media/oms-solution-office-365/admin-consent.png)

### <a name="subscribe-to-log-analytics-workspace"></a>Abonneren op de werkruimte voor logboekanalyse
De laatste stap is om u te abonneren van de toepassing naar uw werkruimte voor logboekanalyse. U doen dit ook met een PowerShell-script.

1. Sla het volgende script als *office365_subscription.ps1*.

    ```
    param (
        [Parameter(Mandatory=$True)][string]$WorkspaceName,
        [Parameter(Mandatory=$True)][string]$ResourceGroupName,
        [Parameter(Mandatory=$True)][string]$SubscriptionId,
        [Parameter(Mandatory=$True)][string]$OfficeUsername,
        [Parameter(Mandatory=$True)][string]$OfficeTennantId,
        [Parameter(Mandatory=$True)][string]$OfficeClientId,
        [Parameter(Mandatory=$True)][string]$OfficeClientSecret
    )
    $line='#-------------------------------------------------------------------------------------------------------------------------------------------------------------------------'
    $line
    IF ($Subscription -eq $null)
        {Login-AzureRmAccount -ErrorAction Stop}
    $Subscription = (Select-AzureRmSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
    $Workspace = (Set-AzureRMOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
    $Workspace
    $WorkspaceLocation= $Workspace.Location
    $OfficeClientSecret =[uri]::EscapeDataString($OfficeClientSecret)
    
    # Client ID for Azure PowerShell
    $clientId = "1950a258-227b-4e31-a9cf-717495945fc2"
    # Set redirect URI for Azure PowerShell
    $redirectUri = "urn:ietf:wg:oauth:2.0:oob"
    $domain='login.microsoftonline.com'
    $adTenant = $Subscription[0].Tenant.Id
    $authority = "https://login.windows.net/$adTenant";
    $ARMResource ="https://management.azure.com/";
    $xms_client_tenant_Id ='55b65fb5-b825-43b5-8972-c8b6875867c1'
    
    switch ($WorkspaceLocation) {
           "USGov Virginia" { 
                             $domain='login.microsoftonline.us';
                              $authority = "https://login.microsoftonline.us/$adTenant";
                              $ARMResource ="https://management.usgovcloudapi.net/"; break} # US Gov Virginia
           default {
                    $domain='login.microsoftonline.com'; 
                    $authority = "https://login.windows.net/$adTenant";
                    $ARMResource ="https://management.azure.com/";break} 
                    }
    
    Function RESTAPI-Auth { 
    
    # Load ADAL Azure AD Authentication Library Assemblies
    $adal = "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Services\Microsoft.IdentityModel.Clients.ActiveDirectory.dll"
    $adalforms = "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Services\Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll"
    $null = [System.Reflection.Assembly]::LoadFrom($adal)
    $null = [System.Reflection.Assembly]::LoadFrom($adalforms)
     
    $global:SubscriptionID = $Subscription.SubscriptionId
    # Set Resource URI to Azure Service Management API
    $resourceAppIdURIARM=$ARMResource;
    # Authenticate and Acquire Token 
    # Create Authentication Context tied to Azure AD Tenant
    $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
    # Acquire token
    $global:authResultARM = $authContext.AcquireToken($resourceAppIdURIARM, $clientId, $redirectUri, "Auto")
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $authHeader
    }
    
    Function Failure {
    $line
    $formatstring = "{0} : {1}`n{2}`n" +
                    "    + CategoryInfo          : {3}`n" +
                    "    + FullyQualifiedErrorId : {4}`n"
    $fields = $_.InvocationInfo.MyCommand.Name,
              $_.ErrorDetails.Message,
              $_.InvocationInfo.PositionMessage,
              $_.CategoryInfo.ToString(),
              $_.FullyQualifiedErrorId
    
    $formatstring -f $fields
    $_.Exception.Response
    
    $line
    break
    }
    
    Function Connection-API
    {
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $ResourceName = "https://manage.office.com"
    $SubscriptionId   =  $Subscription[0].Subscription.Id
    
    $line
    $connectionAPIUrl = $ARMResource + 'subscriptions/' + $SubscriptionId + '/resourceGroups/' + $ResourceGroupName + '/providers/Microsoft.OperationalInsights/workspaces/' + $WorkspaceName + '/connections/office365connection_' + $SubscriptionId + $OfficeTennantId + '?api-version=2017-04-26-preview'
    $connectionAPIUrl
    $line
    
    $xms_client_tenant_Id ='1da8f770-27f4-4351-8cb3-43ee54f14759'
    
    $BodyString = "{
                    'properties': {
                                    'AuthProvider':'Office365',
                                    'clientId': '" + $OfficeClientId + "',
                                    'clientSecret': '" + $OfficeClientSecret + "',
                                    'Username': '" + $OfficeUsername   + "',
                                    'Url': 'https://$($domain)/" + $OfficeTennantId + "/oauth2/token',
                                  },
                    'etag': '*',
                    'kind': 'Connection',
                    'solution': 'Connection',
                   }"
    
    $params = @{
        ContentType = 'application/json'
        Headers = @{
        'Authorization'="$($authHeader)"
        'x-ms-client-tenant-id'=$xms_client_tenant_Id #Prod-'1da8f770-27f4-4351-8cb3-43ee54f14759'
        'Content-Type' = 'application/json'
        }
        Body = $BodyString
        Method = 'Put'
        URI = $connectionAPIUrl
    }
    $response = Invoke-WebRequest @params 
    $response
    $line
    
    }
    
    Function Office-Subscribe-Call{
    try{
    #----------------------------------------------------------------------------------------------------------------------------------------------
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $SubscriptionId   =  $Subscription[0].Subscription.Id
    $OfficeAPIUrl = $ARMResource + 'subscriptions/' + $SubscriptionId + '/resourceGroups/' + $ResourceGroupName + '/providers/Microsoft.OperationalInsights/workspaces/' + $WorkspaceName + '/datasources/office365datasources_' + $SubscriptionId + $OfficeTennantId + '?api-version=2015-11-01-preview'
    
    $OfficeBodyString = "{
                    'properties': {
                                    'AuthProvider':'Office365',
                                    'office365TenantID': '" + $OfficeTennantId + "',
                                    'connectionID': 'office365connection_" + $SubscriptionId + $OfficeTennantId + "',
                                    'office365AdminUsername': '" + $OfficeUsername + "',
                                    'contentTypes':'Audit.Exchange,Audit.AzureActiveDirectory'
                                  },
                    'etag': '*',
                    'kind': 'Office365',
                    'solution': 'Office365',
                   }"
    
    $Officeparams = @{
        ContentType = 'application/json'
        Headers = @{
        'Authorization'="$($authHeader)"
        'x-ms-client-tenant-id'=$xms_client_tenant_Id
        'Content-Type' = 'application/json'
        }
        Body = $OfficeBodyString
        Method = 'Put'
        URI = $OfficeAPIUrl
      }
    
    $officeresponse = Invoke-WebRequest @Officeparams 
    $officeresponse
    }
    catch{ Failure }
    }
    
    #GetDetails 
    RESTAPI-Auth -ErrorAction Stop
    Connection-API -ErrorAction Stop
    Office-Subscribe-Call -ErrorAction Stop
    ```

2. Voer het script met de volgende opdracht:
    ```
    .\office365_subscription.ps1 -WorkspaceName <Log Analytics workspace name> -ResourceGroupName <Resource Group name> -SubscriptionId <Subscription ID> -OfficeUsername <OfficeUsername> -OfficeTennantID <Tenant ID> -OfficeClientId <Client ID> -OfficeClientSecret <Client secret>
    ```
    Voorbeeld:

    ```
    .\office365_subscription.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631-yyyyyyyyyyyy' -OfficeUsername 'admin@contoso.com' -OfficeTennantID 'ce4464f8-a172-4dcf-b675-xxxxxxxxxxxx' -OfficeClientId 'f8f14c50-5438-4c51-8956-zzzzzzzzzzzz' -OfficeClientSecret 'y5Lrwthu6n5QgLOWlqhvKqtVUZXX0exrA2KRHmtHgQb='
    ```

### <a name="troublshooting"></a>Troublshooting

Mogelijk ziet u de volgende fout als u probeert te maken van een abonnement nadat het abonnement al bestaat.

```
Invoke-WebRequest : {"Message":"An error has occurred."}
At C:\Users\v-tanmah\Desktop\ps scripts\office365_subscription.ps1:161 char:19
+ $officeresponse = Invoke-WebRequest @Officeparams
+                   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (System.Net.HttpWebRequest:HttpWebRequest) [Invoke-WebRequest], WebException
    + FullyQualifiedErrorId : WebCmdletWebResponseException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand 
```

Mogelijk ziet u de volgende fout als ongeldige parameterwaarden worden geleverd.

```
Select-AzureRmSubscription : Please provide a valid tenant or a valid subscription.
At line:12 char:18
+ ... cription = (Select-AzureRmSubscription -SubscriptionId $($Subscriptio ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureRmContext], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.SetAzureRMContextCommand

```

## <a name="uninstall"></a>Verwijderen
U kunt de Office 365-oplossing met behulp van het proces in verwijderen [verwijderen van een beheeroplossing](../monitoring/monitoring-solutions.md#remove-a-management-solution). Dit wordt niet gegevens die worden verzameld vanuit Office 365 in logboekanalyse al gestopt. Volg de onderstaande procedure om te afmelden bij Office 365 en stoppen met het verzamelen van gegevens.

1. Sla het volgende script als *office365_unsubscribe.ps1*.

    ```
    param (
        [Parameter(Mandatory=$True)][string]$WorkspaceName,
        [Parameter(Mandatory=$True)][string]$ResourceGroupName,
        [Parameter(Mandatory=$True)][string]$SubscriptionId,
        [Parameter(Mandatory=$True)][string]$OfficeTennantId
    )
    $line='#-------------------------------------------------------------------------------------------------------------------------------------------------------------------------'
    
    $line
    IF ($Subscription -eq $null)
        {Login-AzureRmAccount -ErrorAction Stop}
    $Subscription = (Select-AzureRmSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
    $Workspace = (Set-AzureRMOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
    $Workspace
    $WorkspaceLocation= $Workspace.Location
    
    # Client ID for Azure PowerShell
    $clientId = "1950a258-227b-4e31-a9cf-717495945fc2"
    # Set redirect URI for Azure PowerShell
    $redirectUri = "urn:ietf:wg:oauth:2.0:oob"
    $domain='login.microsoftonline.com'
    $adTenant =  $Subscription[0].Tenant.Id
    $authority = "https://login.windows.net/$adTenant";
    $ARMResource ="https://management.azure.com/";
    $xms_client_tenant_Id ='55b65fb5-b825-43b5-8972-c8b6875867c1'
    
    switch ($WorkspaceLocation) {
           "USGov Virginia" { 
                             $domain='login.microsoftonline.us';
                              $authority = "https://login.microsoftonline.us/$adTenant";
                              $ARMResource ="https://management.usgovcloudapi.net/"; break} # US Gov Virginia
           default {
                    $domain='login.microsoftonline.com'; 
                    $authority = "https://login.windows.net/$adTenant";
                    $ARMResource ="https://management.azure.com/";break} 
                    }
    
    Function RESTAPI-Auth { 
    
    # Load ADAL Azure AD Authentication Library Assemblies
    $adal = "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Services\Microsoft.IdentityModel.Clients.ActiveDirectory.dll"
    $adalforms = "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Services\Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll"
    $null = [System.Reflection.Assembly]::LoadFrom($adal)
    $null = [System.Reflection.Assembly]::LoadFrom($adalforms)
     
    $global:SubscriptionID = $Subscription.SubscriptionId
    # Set Resource URI to Azure Service Management API
    $resourceAppIdURIARM=$ARMResource;
    # Authenticate and Acquire Token 
    # Create Authentication Context tied to Azure AD Tenant
    $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
    # Acquire token
    $global:authResultARM = $authContext.AcquireToken($resourceAppIdURIARM, $clientId, $redirectUri, "Auto")
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $authHeader
    }
    
    Function Office-UnSubscribe-Call{
    
    #----------------------------------------------------------------------------------------------------------------------------------------------
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $ResourceName = "https://manage.office.com"
    $SubscriptionId   = $Subscription[0].Subscription.Id
    $OfficeAPIUrl = $ARMResource + 'subscriptions/' + $SubscriptionId + '/resourceGroups/' + $ResourceGroupName + '/providers/Microsoft.OperationalInsights/workspaces/' + $WorkspaceName + '/datasources/office365datasources_'  + $SubscriptionId + $OfficeTennantId + '?api-version=2015-11-01-preview'
    
    $Officeparams = @{
        ContentType = 'application/json'
        Headers = @{
        'Authorization'="$($authHeader)"
        'x-ms-client-tenant-id'=$xms_client_tenant_Id
        'Content-Type' = 'application/json'
        }
        Method = 'Delete'
        URI = $OfficeAPIUrl
      }
    
    $officeresponse = Invoke-WebRequest @Officeparams 
    $officeresponse
    
    }
    
    #GetDetails 
    RESTAPI-Auth -ErrorAction Stop
    Office-UnSubscribe-Call -ErrorAction Stop
    ```

2. Voer het script met de volgende opdracht:

    ```
    .\office365_unsubscribe.ps1 -WorkspaceName <Log Analytics workspace name> -ResourceGroupName <Resource Group name> -SubscriptionId <Subscription ID> -OfficeTennantID <Tenant ID> 
    ```

    Voorbeeld:

    ```
    .\office365_unsubscribe.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631-yyyyyyyyyyyy' -OfficeTennantID 'ce4464f8-a172-4dcf-b675-xxxxxxxxxxxx'
    ```

## <a name="data-collection"></a>Gegevensverzameling
### <a name="supported-agents"></a>Ondersteunde agents
De Office 365-oplossing niet ophalen van gegevens uit een van de [OMS agents](../log-analytics/log-analytics-data-sources.md).  Het ophalen van gegevens rechtstreeks vanuit Office 365.

### <a name="collection-frequency"></a>Verzamelingsfrequentie
Het duurt enkele uren voor gegevens in eerste instantie worden verzameld. Zodra deze wordt gestart met het verzamelen van Office 365 verzendt een [webhook melding](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference#receiving-notifications) met gedetailleerde gegevens met logboekanalyse telkens wanneer een record wordt gemaakt. Deze record is beschikbaar in logboekanalyse binnen een paar minuten nadat de worden ontvangen.

## <a name="using-the-solution"></a>De oplossing gebruiken
Wanneer u de Office 365-oplossing aan uw werkruimte voor logboekanalyse toevoegt de **Office 365** tegel wordt toegevoegd aan uw dashboard. Deze tegel toont het aantal computers in uw omgeving, evenals een grafische voorstelling hiervan, en de bijbehorende updatenaleving.<br><br>
![Tegel samenvatting van Office 365](media/oms-solution-office-365/tile.png)  

Klik op de **Office 365** tegel openen de **Office 365** dashboard.

![Office 365-Dashboard](media/oms-solution-office-365/dashboard.png)  

Het dashboard bevat de kolommen in de volgende tabel. Elke kolom bevat de top tien waarschuwingen op het aantal die overeenkomt met de criteria die kolom voor het opgegeven bereik en tijdsbereik. U kunt een zoekopdracht logboek waarmee de volledige lijst door te klikken op Zie alle aan de onderkant van de kolom of door te klikken op de kolomkop uitvoeren.

| Kolom | Beschrijving |
|:--|:--|
| Bewerkingen | Bevat informatie over de actieve gebruikers van uw abonnementen op alle gecontroleerde Office 365. Ook is mogelijk om te zien van het aantal activiteiten die ontstaan gedurende een bepaalde periode.
| Exchange | U kunt de uitsplitsing van Exchange Server-activiteiten zoals de machtiging voor toevoegen of Set-postvak. |
| SharePoint | Toont de belangrijkste activiteiten dat gebruikers op de SharePoint-documenten uitvoeren. Wanneer u inzoomen op deze tegel de zoekpagina geeft de details van deze activiteiten, zoals het doeldocument en de locatie van deze activiteit. Bijvoorbeeld: voor een gebeurtenis bestand geopend, kunt u zich kunt zien van het document dat wordt geopend, de bijbehorende accountnaam en het IP-adres. |
| Azure Active Directory | Bovenste gebruikersactiviteiten, zoals gebruiker-wachtwoord opnieuw instellen en aanmeldingspogingen bevat. Wanneer u inzoomen, zich u kunt u de details van deze activiteiten zoals de resultaatstatus. Dit is vooral handig als u wilt bewaken van verdachte activiteiten op uw Azure Active Directory. |




## <a name="log-analytics-records"></a>Log Analytics-records

Alle records in de werkruimte voor logboekanalyse is gemaakt door de Office 365-oplossing een **Type** van **OfficeActivity**.  De **OfficeWorkload** eigenschap bepaalt welke Office 365-service de record naar - Exchange, AzureActiveDirectory, SharePoint of OneDrive verwijst.  De **RecordType** eigenschap geeft u het type bewerking.  De eigenschappen voor elk bewerkingstype varieert en worden weergegeven in de onderstaande tabellen.

### <a name="common-properties"></a>Algemene eigenschappen
De volgende eigenschappen gelden voor alle Office 365-records.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Type | *OfficeActivity* |
| ClientIP | Het IP-adres van het apparaat dat werd gebruikt toen de activiteit is geregistreerd. Het IP-adres wordt weergegeven in de notatie voor een IPv4- of IPv6-adres. |
| OfficeWorkload | Office 365-service die de record verwijst.<br><br>AzureActiveDirectory<br>Exchange<br>SharePoint|
| Bewerking | De naam van de activiteit van de gebruiker of beheerder.  |
| OrganizationId | De GUID voor Office 365-tenant van uw organisatie. Deze waarde is altijd hetzelfde zijn voor uw organisatie, ongeacht de Office 365-service waarin dit zich voordoet. |
| recordType | Type van een bewerking uitgevoerd. |
| ResultStatus | Hiermee wordt aangegeven of de actie (opgegeven in de eigenschap Operation) gelukt is. Mogelijke waarden zijn geslaagd, PartiallySucceded of mislukt. Voor Exchange-admin-activiteit is de waarde True of False. |
| UserId | De UPN (User Principal Name) van de gebruiker die de actie dat heeft geresulteerd in de record aan te melden; is uitgevoerd bijvoorbeeld: my_name@my_domain_name. Houd er rekening mee dat records voor de activiteit uitgevoerd door systeemaccounts (zoals SHAREPOINT\system of NTAUTHORITY\SYSTEM) ook opgenomen worden. | 
| UserKey | Een alternatieve ID voor de gebruiker in de eigenschap UserId geïdentificeerd.  Deze eigenschap wordt bijvoorbeeld gevuld met de passport unieke ID (PUID) voor gebeurtenissen die worden uitgevoerd door gebruikers in SharePoint, OneDrive voor bedrijven en Exchange. Deze eigenschap kan ook Geef dezelfde waarde als de eigenschap UserID voor gebeurtenissen in andere services en de gebeurtenissen die worden uitgevoerd door systeemaccounts|
| UserType | Het type van de gebruiker die de bewerking uitgevoerd.<br><br>Gemeente<br>Toepassing<br>DcAdmin<br>Reguliere<br>Gereserveerd<br>ServicePrincipal<br>Systeem |


### <a name="azure-active-directory-base"></a>Azure Active Directory-basis
De volgende eigenschappen gelden voor alle Azure Active Directory-records.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| recordType     | AzureActiveDirectory |
| AzureActiveDirectory_EventType | Het type van Azure AD-gebeurtenis. |
| extendedProperties | De uitgebreide eigenschappen van de Azure AD-gebeurtenis. |


### <a name="azure-active-directory-account-logon"></a>Aanmelding bij Azure Active Directory-Account
Deze records worden gemaakt wanneer u een Active Directory-gebruiker zich probeert aan te melden.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| recordType     | AzureActiveDirectoryAccountLogon |
| Toepassing | De toepassing die de gebeurtenis account aanmelden, zoals Office 15. |
| Client | Meer informatie over de client apparaat, een besturingssysteem van het apparaat en browser van een apparaat dat is gebruikt voor de van de account aanmelding-gebeurtenis. |
| loginStatus | Deze eigenschap is rechtstreeks uit OrgIdLogon.LoginStatus. De toewijzing van verschillende interessante aanmeldingsfouten kan worden uitgevoerd door waarschuwingen algoritmen. |
| UserDomain | De Tenant identiteitsgegevens (TII). | 


### <a name="azure-active-directory"></a>Azure Active Directory
Deze records worden gemaakt wanneer een wijziging of toevoegingen worden aangebracht in Azure Active Directory-objecten.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| recordType     | AzureActiveDirectory |
| AADTarget | De gebruiker die de actie (aangeduid met de eigenschap Operation) is uitgevoerd op. |
| Actor | De gebruiker of service-principal die de actie heeft uitgevoerd. |
| ActorContextId | De GUID van de organisatie die de actor behoort. |
| ActorIpAddress | De actor IP-adres in IPV4 of IPV6-adres-indeling. |
| InterSystemsId | De GUID die de acties voor onderdelen in de Office 365-service controleren. |
| IntraSystemId |   De GUID die wordt gegenereerd door Azure Active Directory voor het bijhouden van de actie. |
| SupportTicketId | De klant ondersteunt ticket-ID voor de actie in situaties 'act-op-andere gebruikers-of'. |
| TargetContextId | De GUID van de organisatie die de betreffende gebruiker behoort. |


### <a name="data-center-security"></a>Data Center-beveiliging
Deze records worden gemaakt van controlegegevens Center gegevensbeveiliging.  

| Eigenschap | Beschrijving |
|:--- |:--- |
| EffectiveOrganization | De naam van de tenant die de uitbreiding van bevoegdheden/cmdlet is gericht op. |
| ElevationApprovedTime | Het tijdstempel voor wanneer de uitbreiding is goedgekeurd. |
| ElevationApprover | De naam van een Microsoft-manager. |
| ElevationDuration | De duur waarvoor de uitbreiding van bevoegdheden actief was. |
| ElevationRequestId |  Een unieke id voor de uitbreiding van bevoegdheden-aanvraag. |
| ElevationRole | De rol van de uitbreiding is aangevraagd voor. |
| ElevationTime | De begintijd van de uitbreiding van bevoegdheden. |
| Start_Time | De begintijd van de uitvoering van cmdlets. |


### <a name="exchange-admin"></a>Exchange-beheerder
Deze records worden gemaakt wanneer er wijzigingen zijn aangebracht in de configuratie van Exchange.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | Exchange |
| recordType     | ExchangeAdmin |
| ExternalAccess |  Hiermee geeft u op of de cmdlet is uitgevoerd door een gebruiker in uw organisatie, door medewerkers van Microsoft-datacenter of in een datacenter-serviceaccount of door een gedelegeerde beheerder. De waarde False geeft aan dat de cmdlet is uitgevoerd door iemand die in uw organisatie. De waarde True geeft aan dat de cmdlet is uitgevoerd door datacenter personeel, een datacenter-serviceaccount of een gedelegeerde beheerder. |
| ModifiedObjectResolvedName |  Dit is de beschrijvende naam van het object dat door de cmdlet is gewijzigd. Dit is alleen geregistreerd als de cmdlet het object wijzigt. |
| Organisatienaam | De naam van de tenant. |
| OriginatingServer | De naam van de server van waaruit u de cmdlet is uitgevoerd. |
| Parameters | De naam en waarde voor alle parameters die zijn gebruikt met de cmdlet die wordt geïdentificeerd in de Operations-eigenschap. |


### <a name="exchange-mailbox"></a>Exchange-postvak
Deze records worden gemaakt wanneer wijzigingen of toevoegingen worden aangebracht in de Exchange-postvakken.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | Exchange |
| recordType     | ExchangeItem |
| ClientInfoString | Informatie over de e-mailclient die is gebruikt om de bewerking, zoals een browserversie, de Outlook-versie en de informatie van mobiele apparaten. |
| Client_IPAddress | Het IP-adres van het apparaat dat is gebruikt bij de bewerking werd geregistreerd. Het IP-adres wordt weergegeven in de notatie voor een IPv4- of IPv6-adres. |
| ClientMachineName | De naam van de machine die als host fungeert voor de Outlook-client. |
| ClientProcessName | De e-mailclient die is gebruikt voor toegang tot het postvak. |
| ClientVersion | De versie van de e-mailclient. |
| InternalLogonType | Gereserveerd voor intern gebruik. |
| Logon_Type | Geeft het type van de gebruiker die toegang tot het postvak en de bewerking die is vastgelegd uitgevoerd. |
| LogonUserDisplayName |    De beschrijvende naam van de gebruiker die de bewerking uitgevoerd. |
| LogonUserSid | De SID van de gebruiker die de bewerking uitgevoerd. |
| MailboxGuid | De Exchange-GUID van het postvak dat is geopend. |
| MailboxOwnerMasterAccountSid | Postvak eigenaarsaccount hoofdaccount SID. |
| MailboxOwnerSid | De SID van de eigenaar van het postvak. |
| MailboxOwnerUPN | Het e-mailadres van degene die eigenaar is van het postvak dat is geopend. |


### <a name="exchange-mailbox-audit"></a>Exchange-postvak Audit
Deze records worden gemaakt wanneer u een vermelding postvak wordt gemaakt.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | Exchange |
| recordType     | ExchangeItem |
| Item | Hiermee geeft u het item waarop de bewerking is uitgevoerd | 
| SendAsUserMailboxGuid | De Exchange-GUID van het postvak dat is geopend voor het verzenden van e-mailadres als. |
| SendAsUserSmtp | SMTP-adres van de gebruiker die wordt geïmiteerd. |
| SendonBehalfOfUserMailboxGuid | De Exchange-GUID van het postvak dat is geopend voor het verzenden van e-mail namens. |
| SendOnBehalfOfUserSmtp | SMTP-adres van de gebruiker namens wie het e-mailbericht wordt verzonden. |


### <a name="exchange-mailbox-audit-group"></a>Exchange-postvak Audit-groep
Deze records worden gemaakt wanneer wijzigingen of toevoegingen worden aangebracht in de Exchange-groepen.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | Exchange |
| OfficeWorkload | ExchangeItemGroup |
| AffectedItems | Informatie over elk item in de groep. |
| CrossMailboxOperations | Hiermee wordt aangegeven als de bewerking meer dan één postvak betrokken. |
| DestMailboxId | Alleen ingesteld als de parameter CrossMailboxOperations ingesteld op True is. Hiermee geeft u het doelpostvak GUID. |
| DestMailboxOwnerMasterAccountSid | Alleen ingesteld als de parameter CrossMailboxOperations ingesteld op True is. Hiermee geeft u de SID voor het hoofdaccount SID van de eigenaar van de doel-postvak. |
| DestMailboxOwnerSid | Alleen ingesteld als de parameter CrossMailboxOperations ingesteld op True is. Hiermee geeft u de SID van het doelpostvak. |
| DestMailboxOwnerUPN | Alleen ingesteld als de parameter CrossMailboxOperations ingesteld op True is. Hiermee geeft u de UPN van de eigenaar van het doelpostvak. |
| DestFolder | De doelmap voor bewerkingen, zoals verplaatsen. |
| Map | De map waarin een groep items zich bevindt. |
| Mappen |     Informatie over de bronmappen die zijn betrokken bij een bewerking. Als bijvoorbeeld mappen zijn geselecteerd en vervolgens verwijderd. |


### <a name="sharepoint-base"></a>SharePoint-basis
Deze eigenschappen gelden voor alle SharePoint-records.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| EventSource | Geeft aan dat een gebeurtenis is opgetreden in SharePoint. Mogelijke waarden zijn SharePoint of ObjectModel. |
| itemType | Het type object dat is geopend of gewijzigd. Zie de tabel ItemType voor meer informatie over de verschillende typen objecten. |
| MachineDomainInfo | Informatie over apparaat synchronisatiebewerkingen. Deze informatie wordt gerapporteerd als het aanwezig is in de aanvraag. |
| MachineId |   Informatie over apparaat synchronisatiebewerkingen. Deze informatie wordt gerapporteerd als het aanwezig is in de aanvraag. |
| Site_ | De GUID van de site waar het bestand of map gebruikt door de gebruiker zich bevindt. |
| Source_Name | De entiteit die de controlebewerking geactiveerd. Mogelijke waarden zijn SharePoint of ObjectModel. |
| UserAgent | Informatie over de client of de browser van de gebruiker. Deze informatie wordt verstrekt door de client of de browser. |


### <a name="sharepoint-schema"></a>SharePoint-Schema
Deze records worden gemaakt wanneer configuratiewijzigingen worden aangebracht in SharePoint.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| CustomEvent | Optionele tekenreeks voor aangepaste gebeurtenissen. |
| Event_Data |  De nettolading voor aangepaste gebeurtenissen is optioneel. |
| ModifiedProperties | De eigenschap is opgenomen voor admin-gebeurtenissen, zoals een gebruiker toe te voegen als lid van een site of een groep siteverzamelingen-beheerder. De eigenschap bevat de naam van de eigenschap die is aangepast (bijvoorbeeld de groep sitebeheerder), de nieuwe waarde van de aangepaste eigenschap (die de gebruiker die als een beheerder van de site is toegevoegd) en de vorige waarde van het gewijzigde object. |


### <a name="sharepoint-file-operations"></a>SharePoint-bestandsbewerkingen
Deze records worden gemaakt in reactie op bestandsbewerkingen in SharePoint.

| Eigenschap | Beschrijving |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePointFileOperation |
| DestinationFileExtension | De bestandsextensie van een bestand dat wordt verplaatst of gekopieerd. Deze eigenschap wordt alleen weergegeven voor FileCopied en FileMoved gebeurtenissen. |
| DestinationFileName | De naam van het bestand wordt gekopieerd of verplaatst. Deze eigenschap wordt alleen weergegeven voor FileCopied en FileMoved gebeurtenissen. |
| DestinationRelativeUrl | De URL van de doelmap waar een bestand wordt gekopieerd of verplaatst. De combinatie van de waarden voor parameters SiteURL, DestinationRelativeURL en DestinationFileName is hetzelfde als de waarde voor de object-id-eigenschap, die de volledige padnaam voor het bestand dat is gekopieerd. Deze eigenschap wordt alleen weergegeven voor FileCopied en FileMoved gebeurtenissen. |
| SharingType | Het type van de machtigingen die zijn toegewezen aan de gebruiker die de resource is gedeeld met delen. Deze gebruiker wordt geïdentificeerd door de parameter UserSharedWith. |
| Site_Url | De URL van de site waar het bestand of map gebruikt door de gebruiker zich bevindt. |
| SourceFileExtension | De bestandsextensie van het bestand dat door de gebruiker is geopend. Deze eigenschap is leeg als het object dat is geopend, een map is. |
| Bronbestandsnaam |  De naam van het bestand of map gebruikt door de gebruiker. |
| SourceRelativeUrl | De URL van de map waarin het bestand gebruikt door de gebruiker. De combinatie van de waarden voor de parameters SiteURL SourceRelativeURL en bronbestandsnaam is hetzelfde als de waarde voor de object-id-eigenschap, die de volledige padnaam voor het bestand gebruikt door de gebruiker. |
| UserSharedWith |  De gebruiker die met een bron is gedeeld. |




## <a name="sample-log-searches"></a>Voorbeeldzoekopdrachten in logboeken
De volgende tabel biedt voorbeeldzoekopdrachten in logboeken voor updaterecords die worden verzameld door deze oplossing.

| Query’s uitvoeren | Beschrijving |
| --- | --- |
|Telling van alle bewerkingen voor uw Office 365-abonnement |OfficeActivity &#124; count() door bewerking samenvatten |
|Informatie over het gebruik van SharePoint-sites|OfficeActivity &#124; waar OfficeWorkload = ~ 'sharepoint' &#124; count() door SiteUrl samenvatten | Aantal asc sorteren|
|Toegang bestandsbewerkingen door gebruikerstype|zoeken in (OfficeActivity) OfficeWorkload = ~ 'azureactivedirectory' en 'MyTest'|
|Zoeken met specifieke trefwoorden|Type = OfficeActivity OfficeWorkload = azureactivedirectory 'MyTest'|
|Externe acties van de monitor op Exchange|OfficeActivity &#124; waar OfficeWorkload = ~ 'exchange' en ExternalAccess == true|



## <a name="next-steps"></a>Volgende stappen
* Gebruik Logboekzoekopdrachten in [Log Analytics](../log-analytics/log-analytics-log-searches.md) om gedetailleerde updategegevens weer te geven.
* [Maak uw eigen dashboards](../log-analytics/log-analytics-dashboards.md) om uw favoriete Office 365 zoekquery's weer te geven.
* [Waarschuwingen maken](../log-analytics/log-analytics-alerts.md) proactief informeren van belangrijke Office 365-activiteiten.  
