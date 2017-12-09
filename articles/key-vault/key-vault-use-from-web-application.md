---
title: Azure Sleutelkluis in een webtoepassing gebruiken | Microsoft Docs
description: Met deze zelfstudie kunt u informatie over het gebruik van Azure Sleutelkluis in een webtoepassing.
services: key-vault
author: adhurwit
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.topic: article
ms.date: 09/15/2017
ms.author: adhurwit
ms.openlocfilehash: 107be940b4c105056c63f793fb0111b03469bf66
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="use-azure-key-vault-from-a-web-application"></a>Azure Sleutelkluis in een webtoepassing gebruiken

## <a name="introduction"></a>Inleiding

Met deze zelfstudie kunt u informatie over het gebruik van Azure Sleutelkluis in een webtoepassing in Azure. Dit helpt u bij het proces voor het openen van een geheim van een Azure Sleutelkluis, zodat deze kan worden gebruikt in uw webtoepassing.

**Geschatte duur:** 15 minuten

Zie [Wat is Azure Sleutelkluis?](key-vault-whatis.md) voor algemene informatie over Azure Sleutelkluis.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze zelfstudie te voltooien:

* Een URI naar een geheim in een Azure Sleutelkluis
* Een Client-ID en een Clientgeheim voor een webtoepassing die zijn geregistreerd bij Azure Active Directory die toegang tot uw Sleutelkluis heeft
* Een webtoepassing. We worden de stappen voor een ASP.NET MVC-toepassing geïmplementeerd in Azure als een Web-App weergegeven.

>[!IMPORTANT]
>* Dit voorbeeld is afhankelijk van een oudere manier voor het leveren van AAD-identiteiten handmatig. Er is momenteel een nieuwe functie in preview aangeroepen [beheerde Service identiteit (MSI)](https://docs.microsoft.com/azure/active-directory/msi-overview), die automatisch AAD identiteiten kunt inrichten. Raadpleeg het volgende voorbeeld over [GitHub](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet/) voor meer informatie.

> [!NOTE]
>* Het is essentieel dat u de stappen in hebt voltooid [aan de slag met Azure Key Vault](key-vault-get-started.md) voor deze zelfstudie zodat u de URI moet een geheim en de Client-ID en Clientgeheim voor een webtoepassing hebt.


De webtoepassing die toegang krijgen de Sleutelkluis tot is het adres dat is geregistreerd in Azure Active Directory en toegang heeft gekregen tot uw Sleutelkluis. Als dit niet het geval is, gaat u terug naar Register een toepassing in de zelfstudie aan de slag en Herhaal de stappen die worden vermeld.

Deze zelfstudie is ontworpen voor webontwikkelaars die de basisbeginselen van het maken van webtoepassingen in Azure. Zie voor meer informatie over Azure Web Apps [overzicht van Web-Apps](../app-service/app-service-web-overview.md).

## <a id="packages"></a>NuGet-pakketten toevoegen

Er zijn twee pakketten die u moet uw webtoepassing hebt geïnstalleerd.

* Active Directory Authentication Library - bevat methoden voor interactie met Azure Active Directory en het beheer van gebruikersidentiteiten
* Azure Key Vault Library - bevat methoden voor interactie met Azure Key Vault

Beide van deze pakketten kunnen worden geïnstalleerd met behulp van de Package Manager-Console met de opdracht Install-Package.

```
// this is currently the latest stable version of ADAL
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202
Install-Package Microsoft.Azure.KeyVault
```

## <a id="webconfig"></a>Web.Config wijzigen

Er zijn drie toepassingsinstellingen die moeten worden toegevoegd aan het bestand web.config als volgt.

```
    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />
```

Als u geen randvoorwaarden voor het hosten van uw toepassing als een Azure-Web-App, moet u de werkelijke waarden van ClientId, Clientgeheim en Secret URI toevoegen aan het bestand web.config. Laat deze dummy-waarden anders omdat we de werkelijke waarden in de Azure-portal voor een extra beveiligingsniveau toevoegen.

## <a id="gettoken"></a>Methode voor een toegangstoken ophalen toevoegen

Als u wilt gebruikmaken van de kluis-API-sleutel moet u een toegangstoken. De Sleutelkluis Client aanroepen naar de kluis-API-sleutel verwerkt, maar moet u deze met een functie die het toegangstoken ontvangt uit opgeven.  

Hieronder volgt de code voor een toegangstoken ophalen uit Azure Active Directory. Deze code overal in uw toepassing. Ik wil graag een klasse Utils of EncryptionHelper toevoegen.  

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
```

> [!NOTE]
>* Momenteel is de nieuwe MSI-functie (beheerde service-identiteit) de eenvoudigste manier om te verifiëren. Klik op de volgende koppeling naar het voorbeeld met [Key Vault met MSI in een toepassing in .NET](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet/) en de gerelateerde [zelfstudie voor MSI met App Service en Functions](https://docs.microsoft.com/azure/app-service/app-service-managed-service-identity) voor meer informatie. 
>* Client-ID en Clientgeheim is een andere manier om te verifiëren van een Azure AD-toepassing. En u deze in uw webtoepassing kunt u een scheiding van taken en meer controle over uw Sleutelbeheer. Maar deze maakt gebruik van de Client-Secret als in uw configuratie-instellingen die voor bepaalde als riskant als het plaatsen van het geheim dat u wilt beveiligen in uw configuratie-instellingen kunnen worden. Zie de volgende voor informatie over het gebruik van een Client-ID en het certificaat in plaats van de Client-ID en Clientgeheim om te verifiëren van de Azure AD-toepassing.

## <a id="appstart"></a>Ophalen van het geheim op toepassing starten

Er moet nu code roept u de kluis-API-sleutel en het geheim ophalen. De volgende code kan overal worden geplaatst, zolang deze wordt aangeroepen voordat u wilt gebruiken. Ik hebben deze code in de toepassing gestart gebeurtenis in het bestand Global.asax plaatsen, zodat er wordt één keer worden uitgevoerd op start en het geheim voor de toepassing beschikbaar wordt.

```cs
//add these using statements
using Microsoft.Azure.KeyVault;
using System.Web.Configuration;

// I put my GetToken method in a Utils class. Change for wherever you placed your method.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));
var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

//I put a variable in a Utils class to hold the secret for general  application use.
Utils.EncryptSecret = sec.Value;
```

## <a id="portalsettings"></a>App-instellingen toevoegen in de Azure portal (optioneel)

Als u een Azure-Web-App hebt, kunt u nu de werkelijke waarden toevoegen voor de AppSettings in de Azure portal. Op deze manier worden de werkelijke waarden worden niet in de web.config maar beveiligd via de Portal waar u afzonderlijke access control mogelijkheden hebt. Deze waarden wordt vervangen voor de waarden die u hebt ingevoerd in de web.config. Zorg ervoor dat de namen hetzelfde zijn.

![Toepassingsinstellingen weergegeven in de Azure-portal][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Verifiëren met een certificaat in plaats van een Clientgeheim

Er is een andere manier om te verifiëren van een Azure AD-toepassing met behulp van een Client-ID en een certificaat in plaats van een Client-ID en Clientgeheim. Hieronder volgen de stappen voor het gebruik van een certificaat in een Azure-Web-App:

1. Maken of een certificaat maken
2. Het certificaat koppelen aan een Azure AD-toepassing
3. Voeg code toe aan uw Web-App om het certificaat te gebruiken
4. Een certificaat toevoegen aan uw Web-App

### <a name="get-or-create-a-certificate"></a>Maken of een certificaat maken

Voor onze toepassing maken we een testcertificaat. Hier volgen een aantal opdrachten die u in een opdrachtprompt voor ontwikkelaars kunt gebruiken om een certificaat te maken. Wijzig de directory aan waar u het certificaat dat bestanden die zijn gemaakt.  Gebruik ook, voor het begin- en einddatum van het certificaat, de huidige datum plus 1 jaar.

```
makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2017 -e 07/31/2018 -r
pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123
```

Noteer de einddatum en het wachtwoord voor de .pfx (in dit voorbeeld: 07/31/2017 en test123). U moet ze hieronder.

Zie voor meer informatie over het maken van een testcertificaat [hoe: uw eigen testen certificaat maken](https://msdn.microsoft.com/library/ff699202.aspx)

### <a name="associate-the-certificate-with-an-azure-ad-application"></a>Het certificaat koppelen aan een Azure AD-toepassing

Nu u een certificaat hebt, moet u deze koppelen aan een Azure AD-toepassing. De Azure-portal ondersteunt momenteel kan niet in deze werkstroom; Dit kan worden uitgevoerd via PowerShell. Voer de volgende opdrachten voor assoicate het certificaat met de Azure AD-toepassing:

```ps
$x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$x509.Import("C:\data\KVWebApp.cer")
$credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())


$adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $x509.NotBefore -EndDate $x509.NotAfter


$sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId


Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName "http://kvwebapp" -PermissionsToSecrets all -ResourceGroupName 'contosorg'

# get the thumbprint to use in your app settings
$x509.Thumbprint
```

Nadat u deze opdrachten hebt uitgevoerd, ziet u de toepassing in Azure AD. Wanneer u zoekt, zorg ervoor dat u selecteert u 'Toepassingen mijn bedrijf eigenaar is van' in plaats van 'Toepassingen mijn bedrijf gebruikt' in het Zoekdialoogvenster.

Zie voor meer informatie over Azure AD-toepassing en ServicePrincipal-objecten, [toepassingsobjecten en Service-Principal objecten](../active-directory/active-directory-application-objects.md).

### <a name="add-code-to-your-web-app-to-use-the-certificate"></a>Voeg code toe aan uw Web-App om het certificaat te gebruiken

Er wordt nu code toevoegen aan uw Web-App voor toegang tot het certificaat en deze gebruiken voor verificatie.

Er is eerst code voor toegang tot het certificaat.

```cs
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

Houd er rekening mee dat de StoreLocation CurrentUser in plaats van de hoofdmap van LocalMachine is. En dat we 'false' aan de methode vinden levert als u een test-certificaat.

Hierna volgt code die gebruikmaakt van de CertificateHelper en maakt een ClientAssertionCertificate die nodig is voor verificatie.

```cs
public static ClientAssertionCertificate AssertionCert { get; set; }

public static void GetCert()
{
    var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
    AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
}
```

Hier wordt de nieuwe code voor het ophalen van het toegangstoken. Hiermee vervangt u de methode GetToken in het voorgaande voorbeeld. Deze hebt ik op een andere naam voor het gemak gegeven.

```cs
public static async Task<string> GetAccessToken(string authority, string resource, string scope)
{
    var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
    var result = await context.AcquireTokenAsync(resource, AssertionCert);
    return result.AccessToken;
}
```

Alle deze code hebt ik geplaatst in mijn Web-App-project Utils-klasse voor eenvoudig te gebruiken.

De laatste codewijziging is in de methode Application_Start. Eerst moet de GetCert() methode aanroepen om de ClientAssertionCertificate laden. En wijzig vervolgens we de retouraanroepmethode die we bij het maken van een nieuwe KeyVaultClient leveren. Houd er rekening mee dat Hiermee vervangt u de code die we hebben als in het voorgaande voorbeeld.

```cs
Utils.GetCert();
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));
```

### <a name="add-a-certificate-to-your-web-app-through-the-azure-portal"></a>Een certificaat toevoegen aan uw Web-App via de Azure portal

Een certificaat toevoegen aan uw Web-App is een eenvoudig proces. Eerst, gaat u naar de Azure-portal en navigeer naar uw Web-App. Klik op de vermelding voor 'aangepaste domeinen en SSL' in de blade instellingen voor uw Web-App. Op de blade dat openen dat u zich voor het uploaden van het certificaat dat u hebt gemaakt in het voorgaande voorbeeld, KVWebApp.pfx, zorg ervoor dat onthouden u het wachtwoord voor de pfx.

![Een certificaat toevoegen aan een Web-App in de Azure portal][2]

De laatste stap die u moet doen is een toepassingsinstelling toevoegen aan uw Web-App met de naam WEBSITE\_LOAD\_certificaten en een waarde van *. Dit zorgt ervoor dat alle certificaten zijn geladen. Als u laden, alleen de certificaten die u hebt geüpload wilt, kunt u een door komma's gescheiden lijst met de vingerafdrukken invoeren.

Zie voor meer informatie over het toevoegen van een certificaat aan een Web-App, [met behulp van certificaten in toepassingen met Azure Websites](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)

### <a name="add-a-certificate-to-key-vault-as-a-secret"></a>Een certificaat als een geheim aan Sleutelkluis toevoegen

U kunt in plaats van uw certificaat rechtstreeks naar de service-Web-App geüpload, opslaan in de Sleutelkluis als een geheim en het implementeren van daaruit. Dit is een proces in twee stappen die wordt beschreven in het volgende blogbericht [Azure Web App certificaat implementeren via Sleutelkluis](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)

## <a id="next"></a>Volgende stappen

Zie voor het programmeren van verwijzingen [Azure Key Vault C#-Client API Reference](https://msdn.microsoft.com/library/azure/dn903628.aspx).

<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
