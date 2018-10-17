---
title: 'Zelfstudie: Azure Key Vault gebruiken vanuit een webtoepassing | Microsoft Docs'
description: In deze zelfstudie leert u hoe u Azure Key Vault gebruikt vanuit een webtoepassing.
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 10/09/2018
ms.author: barclayn
ms.openlocfilehash: b66c9912ba0b6508c2beb786d2327efa779c6645
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079460"
---
# <a name="tutorial-use-azure-key-vault-from-a-web-application"></a>Zelfstudie: Azure Key Vault gebruiken vanuit een webtoepassing

In deze zelfstudie leert u hoe u Azure Key Vault gebruikt vanuit een webtoepassing in Azure. U ziet hoe u vanuit een Azure Key Vault toegang krijgt tot een geheim voor gebruik in een webtoepassing. De zelfstudie bouwt vervolgens voort op dit proces en gebruikt een certificaat in plaats van een clientgeheim. Deze zelfstudie is ontworpen voor webontwikkelaars die bekend zijn met de basisbeginselen van het maken van webtoepassingen in Azure.

In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
> * Toepassingsinstellingen toevoegen aan het bestand web.config
> * Een methode toevoegen om een toegangstoken te verkrijgen
> * Het token ophalen in de gebeurtenis Toepassing starten
> * Verifiëren met een certificaat

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om deze zelfstudie te voltooien:

* Een URI naar een geheim in een Azure Key Vault
* Een client-id en een clientgeheim voor een webtoepassing die is geregistreerd bij Azure Active Directory die toegang heeft tot uw Key Vault
* Een webtoepassing. Deze zelfstudie toont de stappen voor een ASP.NET MVC-toepassing die als een webtoepassing is geïmplementeerd in Azure.

Voer de stappen uit in [Aan de slag met Azure Key Vault](key-vault-get-started.md) om de URI naar een geheim, de client-id en het clientgeheim op te halen en om de toepassing te registreren. De webtoepassing opent de kluis en moet zijn geregistreerd in Azure Active Directory. De toepassing moet ook toegangsrechten hebben voor Key Vault. Als dat niet het geval is, gaat u terug naar Een toepassing registreren in de zelfstudie Aan de slag en herhaalt u de weergegeven stappen. Zie [Overzicht van Web Apps](../app-service/app-service-web-overview.md) voor meer informatie over Azure Web Apps.

Dit voorbeeld is afhankelijk van een handmatige inrichting van Azure Active Directory-identiteiten. Maar u moet [beheerde identiteiten voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) gebruiken, aangezien Azure AD-identiteiten dan automatisch worden ingericht. Zie voor meer informatie het [voorbeeld op GitHub](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet/) en de verwante [zelfstudie over App Service en Functions](https://docs.microsoft.com/azure/app-service/app-service-managed-service-identity). U kunt ook deze zelfstudie voor Key Vault doornemen: [Een Azure-webtoepassing configureren voor het lezen van een geheim uit Key Vault](tutorial-web-application-keyvault.md).

## <a id="packages"></a>NuGet-pakketten toevoegen

Er zijn twee pakketten die moeten zijn geïnstalleerd voor uw webtoepassing.

* Active Directory Authentication Library: bevat methoden voor interactie met Azure Active Directory en het beheer van gebruikers-id's
* Azure Key Vault-bibliotheek: bevat methoden voor interactie met Azure Key Vault

Deze pakketten kunnen beide worden geïnstalleerd met de opdracht Install-Package van de Package Manager Console.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory 
Install-Package Microsoft.Azure.KeyVault
```

## <a id="webconfig"></a>Web.config wijzigen

Er zijn drie toepassingsinstellingen die als volgt moeten worden toegevoegd aan het bestand web.config. We voegen de werkelijke waarden in Azure Portal toe voor een extra beveiligingsniveau.

```xml
    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />
    <!-- If you aren't hosting your app as an Azure Web App, then you should use the actual ClientId, Client Secret, and Secret URI values -->
```



## <a id="gettoken"></a>Een methode toevoegen om een toegangstoken te verkrijgen

Om de API van Key Vault te gebruiken, hebt u een toegangstoken nodig. De Key Vault-client handelt aanroepen naar de API van Key Vault af, maar u moet deze voorzien van een functie om het toegangstoken op te halen. Hier volgt een voorbeeld van code om een toegangstoken op te halen uit Azure Active Directory. U kunt deze code overal in uw toepassing invoegen. Ik wil een Utils- of EncryptionHelper-klasse toevoegen.  

```cs
//add these using statements
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Threading.Tasks;
using System.Web.Configuration;

//this is an optional property to hold the secret after it is retrieved
public static string EncryptSecret { get; set; }

//the method that will be provided to the KeyVaultClient
public static async Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                WebConfigurationManager.AppSettings["ClientSecret"]);
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
// Using Client ID and Client Secret is a way to authenticate an Azure AD application.
// Using it in your web application allows for a separation of duties and more control over your key management. 
// However, it does rely on putting the Client Secret in your configuration settings.
// For some people, this can be as risky as putting the secret in your configuration settings.
```

## <a id="appstart"></a>Het geheim ophalen in de gebeurtenis Toepassing starten

Nu hebben we de code nodig om de API van Key Vault aan te roepen en de geheime sleutel op te halen. De volgende code kan overal worden ingevoegd, zolang deze wordt aangeroepen voordat u deze moet gebruiken. Ik heb deze code in de gebeurtenis Toepassing starten in Global.asax geplaatst, zodat deze eenmaal wordt uitgevoerd bij het starten en het geheim beschikbaar is voor de toepassing.

```cs
//add these using statements
using Microsoft.Azure.KeyVault;
using System.Web.Configuration;

// I put my GetToken method in a Utils class. Change for wherever you placed your method.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));
var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

//I put a variable in a Utils class to hold the secret for general application use.
Utils.EncryptSecret = sec.Value;
```

## <a id="portalsettings"></a>App-instellingen toevoegen in Azure Portal

In de Azure Web App kunt u nu de werkelijke waarden voor de AppSettings toevoegen in Azure Portal. Door deze stap uit te voeren, staan de werkelijke waarden niet in het bestand web.config, maar worden ze beveiligd via de Portal, waar u afzonderlijke mogelijkheden hebt voor toegangsbeheer. Deze waarden worden vervangen worden door de waarden die u hebt ingevoerd in web.config. Controleer of de namen hetzelfde zijn.

![Toepassingsinstellingen weergegeven in Azure Portal][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Verifiëren met een certificaat in plaats van een clientgeheim

Nu u weet hoe u een Azure AD-app verifieert met behulp van een client-id en een clientgeheim, gaat u een client-id en een certificaat gebruiken. Gebruik de volgende stappen om een certificaat te gebruiken in een Azure Web App:

1. Een certificaat verkrijgen of maken
2. Het certificaat koppelen aan een Azure AD-toepassing
3. Code aan uw webtoepassing toevoegen om het certificaat te gebruiken
4. Een certificaat toevoegen aan uw webtoepassing

### <a name="get-or-create-a-certificate"></a>Een certificaat verkrijgen of maken

 Voor deze zelfstudie maken we een testcertificaat. Hier volgt een script voor het maken van een zelfondertekend certificaat. Wijzig de map in de map waarin u de certificaatbestanden wilt maken.  Voor de begin- en einddatum van het certificaat kunt u de huidige datum plus één jaar gebruiken.

```powershell
#Create self-signed certificate and export pfx and cer files 
$PfxFilePath = 'KVWebApp.pfx'
$CerFilePath = 'KVWebApp.cer'
$DNSName = 'MyComputer.Contoso.com'
$Password = 'MyPassword"'

$StoreLocation = 'CurrentUser' #be aware that LocalMachine requires elevated privileges
$CertBeginDate = Get-Date
$CertExpiryDate = $CertBeginDate.AddYears(1)

$SecStringPw = ConvertTo-SecureString -String $Password -Force -AsPlainText 
$Cert = New-SelfSignedCertificate -DnsName $DNSName -CertStoreLocation "cert:\$StoreLocation\My" -NotBefore $CertBeginDate -NotAfter $CertExpiryDate -KeySpec Signature
Export-PfxCertificate -cert $Cert -FilePath $PFXFilePath -Password $SecStringPw 
Export-Certificate -cert $Cert -FilePath $CerFilePath 
```

Noteer de einddatum en het wachtwoord voor het PFX-bestand (in dit voorbeeld: 15 mei 2019 en mijnwachtwoord). U hebt deze nodig voor het onderstaande script. 
### <a name="associate-the-certificate-with-an-azure-ad-application"></a>Het certificaat koppelen aan een Azure AD-toepassing

Nu u een certificaat hebt, moet u dit koppelen aan een Azure AD-toepassing. De koppeling kan worden uitgevoerd via PowerShell. Voer de volgende opdrachten uit om het certificaat te koppelen aan de Azure AD-toepassing:

```powershell
$x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$x509.Import("C:\data\KVWebApp.cer")
$credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())


$adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $x509.NotBefore -EndDate $x509.NotAfter


$sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId


Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName "http://kvwebapp" -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge -ResourceGroupName 'contosorg'

# get the thumbprint to use in your app settings
$x509.Thumbprint
```

Nadat u deze opdrachten hebt uitgevoerd, ziet u de toepassing in Azure AD. Zorg er bij het zoeken naar de toepassingsregistraties voor dat u in het zoekvenster **Mijn apps** selecteert in plaats van 'Alle apps'. 

### <a name="add-code-to-your-web-app-to-use-the-certificate"></a>Code aan uw webtoepassing toevoegen om het certificaat te gebruiken

Nu gaat u code aan uw webtoepassing toevoegen om toegang te krijgen tot het certificaat en dit te gebruiken voor verificatie. 

Hier volgt eerst de code voor toegang tot het certificaat. StoreLocation is CurrentUser in plaats van LocalMachine. We hebben bovendien 'false' aan de methode Find toegevoegd omdat we een testcertificaat gebruiken.

```cs
//Add this using statement
using System.Security.Cryptography.X509Certificates;  

public static class CertificateHelper
{
    public static X509Certificate2 FindCertificateByThumbprint(string findValue)
    {
        X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
        try
        {
            store.Open(OpenFlags.ReadOnly);
            X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                findValue, false); // Don't validate certs, since the test root isn't installed.
            if (col == null || col.Count == 0)
                return null;
            return col[0];
        }
        finally
        {
            store.Close();
        }
    }
}
```



Hieronder volgt code die gebruikmaakt van de CertificateHelper en die een ClientAssertionCertificate maakt dat nodig is voor verificatie.

```cs
public static ClientAssertionCertificate AssertionCert { get; set; }

public static void GetCert()
{
    var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
    AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
}
```

Hier is de nieuwe code voor het ophalen van het toegangstoken. Deze code vervangt de methode GetToken in het voorgaande voorbeeld. Ik heb deze voor de duidelijkheid een andere naam gegeven. Ik heb alle code voor het gemak in de Utils-klasse van mijn Web App-project geplaatst.

```cs
public static async Task<string> GetAccessToken(string authority, string resource, string scope)
{
    var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
    var result = await context.AcquireTokenAsync(resource, AssertionCert);
    return result.AccessToken;
}
```



De laatste codewijziging is in de methode Application_Start. Eerst moet u de methode GetCert() aanroepen om het ClientAssertionCertificate aan te roepen. Daarna moet u de callback-methode wijzigen die we hebben opgegeven bij het maken van een nieuwe KeyVaultClient. Deze code vervangt de code in het voorgaande voorbeeld.

```cs
Utils.GetCert();
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));
```

### <a name="add-a-certificate-to-your-web-app-through-the-azure-portal"></a>Een certificaat aan uw webtoepassing toevoegen via Azure Portal

Het toevoegen van een certificaat aan uw webtoepassing is een eenvoudig proces van twee stappen. Ga eerst naar Azure Portal en navigeer naar uw webtoepassing. Klik in Instellingen voor uw webtoepassing en klik op de vermelding voor **SSL-instellingen**. Wanneer dit onderdeel wordt geopend, uploadt u het certificaat dat u in het voorgaande voorbeeld hebt gemaakt, KVWebApp.pfx. Zorg dat u het wachtwoord voor de pfx onthoudt.

![Een certificaat aan een webtoepassing toevoegen in Azure Portal][2]

Tot slot moet u een toepassingsinstelling met de naam WEBSITE\_LOAD\_CERTIFICATES en de waarde * aan uw webtoepassing toevoegen. Met deze stap zorgt u ervoor dat alle certificaten worden geladen. Als u alleen de certificaten wilt laden die u hebt geüpload, kunt u een door komma's gescheiden lijst van hun vingerafdrukken invoeren.


## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u deze resources niet meer nodig hebt, verwijdert u de toepassingsservice, sleutelkluis en Azure AD-toepassing die u voor de zelfstudie hebt gebruikt.  


## <a id="next"></a>Volgende stappen
> [!div class="nextstepaction"]
>[API-naslag voor Azure Key Vault Management](/dotnet/api/overview/azure/keyvault/management).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
 