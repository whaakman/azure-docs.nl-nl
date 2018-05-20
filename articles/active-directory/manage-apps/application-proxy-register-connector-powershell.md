---
title: Een installatie zonder toezicht toepassingsproxy van Azure AD-connector | Microsoft Docs
description: Bevat informatie over het uitvoeren van een installatie zonder toezicht van Azure AD Connector voor toepassingsproxy om te bieden veilige externe toegang tot uw lokale apps.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 93036b3b473de8bb0026ec0759691c20ec38401a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="create-an-unattended-installation-script-for-the-azure-ad-application-proxy-connector"></a>Een script voor installatie zonder toezicht voor de Azure AD-toepassingsproxy-connector maken

Dit onderwerp helpt u bij het maken van een Windows PowerShell-script waarmee de installatie zonder toezicht en registratie voor uw Azure AD-toepassingsproxy-connector.

Deze mogelijkheid is handig als u wilt:

* De connector installeren op Windows-servers hebt die geen gebruikersinterface is ingeschakeld of dat u kunt openen met extern bureaublad.
* Installeren en veel connectors in één keer te registreren.
* De installatie van connector en de registratie als onderdeel van een andere procedure integreren.
* Maak een Standard van SQL server-installatiekopie die de connector bits bevat maar is niet geregistreerd.

Voor de [Application Proxy connector](application-proxy-connectors.md) om te werken, deze worden geregistreerd met uw Azure AD-directory met een globale beheerder en het wachtwoord heeft. Normaal gesproken deze informatie wordt ingevoerd tijdens de installatie van de Connector in een pop-updialoogvenster, maar u kunt PowerShell gebruiken dit proces te automatiseren in plaats daarvan.

Er zijn twee stappen voor een installatie zonder toezicht. Installeer eerst de connector. Ten tweede, registreert u de connector met Azure AD. 

## <a name="install-the-connector"></a>De connector installeren
Gebruik de volgende stappen voor het installeren van de connector zonder het registreren van deze:

1. Open een opdrachtprompt.
2. Voer de volgende opdracht, waarbij de /q stille installatie betekent. Een stille installatie niet wordt gevraagd u accepteert de gebruiksrechtovereenkomst.
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-the-connector-with-azure-ad"></a>Registreert u de connector met Azure AD
Er zijn twee methoden die kunt u de connector te registreren:

* De connector met behulp van een Windows PowerShell-referentieobject registreren
* Registreert u de connector met behulp van een token offline gemaakt

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>De connector met behulp van een Windows PowerShell-referentieobject registreren
1. Maak een Windows PowerShell-referenties object `$cred` die een administratieve gebruikersnaam en wachtwoord voor uw directory bevat. Voer de volgende opdracht vervangen *\<gebruikersnaam\>* en  *\<wachtwoord\>*:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. Ga naar **C:\Program Files\Microsoft AAD App Proxy Connector** en voer de volgende script uit via de `$cred` -object dat u hebt gemaakt:
   
        .\RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred -Feature ApplicationProxy

### <a name="register-the-connector-using-a-token-created-offline"></a>Registreert u de connector met behulp van een token offline gemaakt
1. Maken van een offline-token met behulp van de AuthenticationContext klasse met de waarden in deze codefragment of onderstaande PowerShell-cmdlets:

    **Werken met C#:**

        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.microsoftonline.com/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }

    **Met behulp van PowerShell:**

        # Locate AzureAD PowerShell Module
        # Change Name of Module to AzureAD after what you have installed
        $AADPoshPath = (Get-InstalledModule -Name AzureAD).InstalledLocation
        # Set Location for ADAL Helper Library
        $ADALPath = $(Get-ChildItem -Path $($AADPoshPath) -Filter Microsoft.IdentityModel.Clients.ActiveDirectory.dll -Recurse ).FullName | Select-Object -Last 1
        
        # Add ADAL Helper Library
        Add-Type -Path $ADALPath
        
        #region constants
        
        # The AAD authentication endpoint uri
        [uri]$AadAuthenticationEndpoint = "https://login.microsoftonline.com/common/oauth2/token?api-version=1.0/" 
        
        # The application ID of the connector in AAD
        [string]$ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489"
        
        # The reply address of the connector application in AAD
        [uri]$ConnectorRedirectAddress = "urn:ietf:wg:oauth:2.0:oob" 
        
        # The AppIdUri of the registration service in AAD
        [uri]$RegistrationServiceAppIdUri = "https://proxy.cloudwebappproxy.net/registerapp"
        
        #endregion
        
        #region GetAuthenticationToken
        
        # Set AuthN context
        $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $AadAuthenticationEndpoint
        
        # Build platform parameters
        $promptBehavior = [Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Always
        $platformParam = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.PlatformParameters" -ArgumentList $promptBehavior
        
        # Do AuthN and get token
        $authResult = $authContext.AcquireTokenAsync($RegistrationServiceAppIdUri.AbsoluteUri, $ConnectorAppId, $ConnectorRedirectAddress, $platformParam).Result
        
        # Check AuthN result
        If (($authResult) -and ($authResult.AccessToken) -and ($authResult.TenantId) ) {
        $token = $authResult.AccessToken
        $tenantId = $authResult.TenantId
        }
        Else {
        Write-Output "Authentication result, token or tenant id returned are null"
        }
        
        #endregion

2. Zodra u het token hebt, maakt u een SecureString met behulp van het token:

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. Voer de volgende Windows PowerShell-opdracht vervangen \<tenant-GUID\> aan uw directory-ID:

   `.\RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID> -Feature ApplicationProxy`

## <a name="next-steps"></a>Volgende stappen 
* [Toepassingen publiceren met uw eigen domeinnaam](application-proxy-configure-custom-domain.md)
* [Eenmalige aanmelding inschakelen](application-proxy-configure-single-sign-on-with-kcd.md)
* [Oplossen van problemen met toepassingsproxy](../active-directory-application-proxy-troubleshoot.md)


