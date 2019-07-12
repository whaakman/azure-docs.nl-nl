---
title: Azure Active Directory gebaseerde verificatie met Azure Cosmos DB
description: Informatie over het configureren van een Azure AD-identiteit voor verificatie op basis van certificaten voor toegangssleutels uit Azure Cosmos DB.
author: voellm
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: tvoellm
ms.reviewer: sngun
ms.openlocfilehash: cc39cc09259c1ae681e1fee070777575e2788323
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827847"
---
# <a name="certificate-based-authentication-for-an-azure-ad-identity-to-access-keys-from-an-azure-cosmos-db-account"></a>Verificatie op basis van certificaten voor een Azure AD-identiteit voor toegangssleutels uit een Azure Cosmos DB-account

Verificatie op basis van certificaten kan uw clienttoepassing moet worden geverifieerd met behulp van Azure Active Directory (Azure AD) met een clientcertificaat. U kunt certificaten gebaseerde verificatie uitvoeren op een computer waar u een identiteit, zoals een on-premises machine of virtuele machine in Azure nodig hebt. Uw toepassing kunt Azure Cosmos DB-sleutels lezen zonder dat de sleutels rechtstreeks in de toepassing. In dit artikel wordt beschreven hoe u een voorbeeld van Azure AD-toepassing maken, configureren voor verificatie op basis van certificaten, meld u aan bij Azure met behulp van de nieuwe toepassings-id en vervolgens de sleutels worden opgehaald uit uw Azure Cosmos-account. In dit artikel wordt Azure PowerShell gebruikt voor het instellen van de identiteiten en biedt een C# voorbeeld-app die wordt geverifieerd en krijgt toegang tot sleutels van uw Azure Cosmos-account.  

## <a name="prerequisites"></a>Vereisten

* Installeer de [meest recente versie](/powershell/azure/install-az-ps) van Azure PowerShell.

* Als u geen [Azure-abonnement](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing) hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.

## <a name="register-an-app-in-azure-ad"></a>Een app in Azure AD registreren

In deze stap registreert u een voorbeeld-web-App in uw Azure AD-account. Deze toepassing wordt later gebruikt voor het lezen van de sleutels van uw Azure Cosmos DB-account. Gebruik de volgende stappen uit om een toepassing te registreren: 

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).

1. Open de Azure **Active Directory** deelvenster, opent u het deelvenster van de App-registraties en selecteer **registratie van nieuwe**. 

   ![Nieuwe toepassing registreren in Active Directory](./media/certificate-based-authentication/new-app-registration.png)

1. Vul de **registreren van een toepassing** formulier met de volgende details:  

   * **Naam** – Geef een naam op voor uw toepassing, kan elke naam zijn, zoals 'sampleApp' zijn.
   * **Ondersteund accounttypen** – Kies **Accounts in deze organisatie-map alleen (standaarddirectory)** om toe te staan van bronnen in uw huidige directory voor toegang tot deze toepassing. 
   * **Omleidings-URL** – Kies toepassing van het type **Web** en geef een URL op waar uw toepassing wordt gehost, kan een URL zijn. In dit voorbeeld kunt u een test-URL, zoals opgeven `https://sampleApp.com` goed is, zelfs als de app bestaat niet.

   ![Een voorbeeld-web-App registreren](./media/certificate-based-authentication/register-sample-web-app.png)

1. Selecteer **registreren** nadat u het formulier hebt ingevuld.

1. Nadat de app is geregistreerd, maak een notitie van de **Application(client) ID** en **Object-ID**, gaat u deze gegevens in de volgende stappen. 

   ![De toepassing en object-id's ophalen](./media/certificate-based-authentication/get-app-object-ids.png)

## <a name="install-the-azuread-module"></a>De AzureAD-module installeren

In deze stap installeert u de Azure AD PowerShell-module. Deze module is vereist voor het ophalen van de ID van de toepassing die u in de vorige stap hebt geregistreerd en verbinden met een zelf-ondertekend certificaat bij die toepassing. 

1. Open Windows PowerShell ISE met administrator-rechten. Als u dit nog niet hebt gedaan, wordt de AZ-PowerShell-module installeren en verbinding maken met uw abonnement. Als u meerdere abonnementen hebt, kunt u de context van het huidige abonnement kunt instellen, zoals wordt weergegeven in de volgende opdrachten:

   ```powershell

   Install-Module -Name Az -AllowClobber
   Connect-AzAccount

   Get-AzSubscription
   $context = Get-AzSubscription -SubscriptionId <Your_Subscription_ID>
   Set-AzContext $context 
   ```

1. Installeren en te importeren de [AzureAD](/powershell/module/azuread/?view=azureadps-2.0) module

   ```powershell
   Install-Module AzureAD
   Import-Module AzureAD 
   ```

## <a name="sign-into-your-azure-ad"></a>Meld u aan bij uw Azure AD

Zich aanmelden bij uw Azure AD waar u de toepassing hebt geregistreerd. Gebruik de opdracht Connect-AzureAD Meld u aan bij uw account, geeft u de referenties van uw Azure-account in het pop-upvenster. 

```powershell
Connect-AzureAD 
```

## <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

Open een ander exemplaar van Windows PowerShell ISE en voer de volgende opdrachten om te maken van een zelfondertekend certificaat en de sleutel die is gekoppeld aan het certificaat niet lezen:

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "Cert:\CurrentUser\My" -Subject "CN=sampleAppCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData()) 
```

## <a name="create-the-certificate-based-credential"></a>De referentie op basis van certificaten maken 

Voer vervolgens de volgende opdrachten voor het ophalen van de object-ID van uw toepassing en de referentie op basis van certificaten te maken. In dit voorbeeld stellen we het certificaat verloopt na één jaar, kunt u dit instellen op eventuele vereiste einddatum valt.

```powershell
$application = Get-AzureADApplication -ObjectId <Object_ID_of_Your_Application>

New-AzureADApplicationKeyCredential -ObjectId $application.ObjectId -CustomKeyIdentifier "Key1" -Type AsymmetricX509Cert -Usage Verify -Value $keyValue -EndDate "2020-01-01"
```

De bovenstaande opdracht resulteert in de uitvoer is vergelijkbaar met de onderstaande schermafbeelding:

![Referentie op basis van certificaten maken-uitvoer](./media/certificate-based-authentication/certificate-based-credential-output.png)

## <a name="configure-your-azure-cosmos-account-to-use-the-new-identity"></a>Configureer uw Azure Cosmos-account voor het gebruik van de nieuwe identiteit

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).

1. Navigeer naar uw Azure Cosmos-account, open de **toegangsbeheer (IAM)** blade.

1. Selecteer **toevoegen** en **roltoewijzing toevoegen**. Toevoegen van de sampleApp die u hebt gemaakt in de vorige stap met **Inzender** rol zoals wordt weergegeven in de volgende schermafbeelding:

   ![Azure Cosmos-account voor het gebruik van de nieuwe identiteit configureren](./media/certificate-based-authentication/configure-cosmos-account-with-identify.png)

1. Selecteer **opslaan** nadat u het formulier invullen


## <a name="access-the-keys-from-powershell"></a>Toegang tot de sleutels van PowerShell

In deze stap wordt u melden bij Azure met behulp van de toepassing en het certificaat dat u hebt gemaakt en toegang tot uw Azure Cosmos-account. 

1. Schakel in eerste instantie van de Azure-account referenties die u aan te melden bij uw account hebt gebruikt. U kunt referenties wissen met behulp van de volgende opdracht uit:

   ```powershell
   Disconnect-AzAccount -Username <Your_Azure_account_email_id> 
   ```

1. Volgende valideren dat u kunt zich bij Azure portal aanmelden met behulp van de referenties van de toepassing en toegang de sleutels voor Azure Cosmos DB tot:

   ```powershell
   Login-AzAccount -ApplicationId <Your_Application_ID> -CertificateThumbprint $cert.Thumbprint -ServicePrincipal -Tenant <Tenant_ID_of_your_application>

   Invoke-AzResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDB/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <Resource_Group_Name_of_your_Azure_Cosmos_account> -ResourceName <Your_Azure_Cosmos_Account_Name> 
   ```

De vorige opdracht wordt de primaire en secundaire hoofdsleutels van uw Azure Cosmos-account weergegeven. U kunt het activiteitenlogboek van uw Azure Cosmos-account om te valideren dat de aanvraag van de sleutels get is voltooid en de gebeurtenis wordt gestart door de toepassing "sampleApp" weergeven. 
 
![Valideren van het aanroepen van de sleutels ophalen in de Azure AD](./media/certificate-based-authentication/activity-log-validate-results.png)


## <a name="access-the-keys-from-a-c-application"></a>Toegang tot de sleutels uit een C# toepassing 

U kunt dit scenario ook valideren door het openen van de sleutels uit een C# toepassing. De volgende C# consoletoepassing die toegang hebben tot Azure Cosmos DB-sleutels met behulp van de app geregistreerd in Active Directory. Zorg ervoor dat u het bijwerken van de tenant-id, clientID, certName, Resourcegroepnaam, abonnements-ID, Azure Cosmos-account naam gegevens voordat u de code uitvoeren. 

```csharp
using System;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Linq;
using System.Net.Http;
using System.Security.Cryptography.X509Certificates;
using System.Threading;
using System.Threading.Tasks;
 
namespace TodoListDaemonWithCert
{
    class Program
    {
        private static string aadInstance = "https://login.windows.net/";
        private static string tenantId = "<Your_Tenant_ID>";
        private static string clientId = "<Your_Client_ID>";
        private static string certName = "<Your_Certificate_Name>";
 
        private static int errorCode = 0;
        static int Main(string[] args)
        {
            MainAync().Wait();
            Console.ReadKey();
 
            return 0;
        } 
 
        static async Task MainAync()
        {
            string authContextURL = aadInstance + tenantId;
            AuthenticationContext authContext = new AuthenticationContext(authContextURL);
            X509Certificate2 cert = ReadCertificateFromStore(certName);
 
            ClientAssertionCertificate credential = new ClientAssertionCertificate(clientId, cert);
            AuthenticationResult result = await authContext.AcquireTokenAsync("https://management.azure.com/", credential);
            if (result == null)
            {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }
 
            string token = result.AccessToken;
            string subscriptionId = "<Your_Subscription_ID>";
            string rgName = "<ResourceGroup_of_your_Cosmos_account>";
            string accountName = "<Your_Cosmos_account_name>";
            string cosmosDBRestCall = $"https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{rgName}/providers/Microsoft.DocumentDB/databaseAccounts/{accountName}/listKeys?api-version=2015-04-08";
 
            Uri restCall = new Uri(cosmosDBRestCall);
            HttpClient httpClient = new HttpClient();
            httpClient.DefaultRequestHeaders.Remove("Authorization");
            httpClient.DefaultRequestHeaders.Add("Authorization", "Bearer " + token);
            HttpResponseMessage response = await httpClient.PostAsync(restCall, null);
 
            Console.WriteLine("Got result {0} and keys {1}", response.StatusCode.ToString(), response.Content.ReadAsStringAsync().Result);
        }
 
 
        /// <summary>
        /// Reads the certificate
        /// </summary>
        private static X509Certificate2 ReadCertificateFromStore(string certName)
        {
            X509Certificate2 cert = null;
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            store.Open(OpenFlags.ReadOnly);
            X509Certificate2Collection certCollection = store.Certificates;
 
            // Find unexpired certificates.
            X509Certificate2Collection currentCerts = certCollection.Find(X509FindType.FindByTimeValid, DateTime.Now, false);
 
            // From the collection of unexpired certificates, find the ones with the correct name.
            X509Certificate2Collection signingCert = currentCerts.Find(X509FindType.FindBySubjectName, certName, false);
 
            // Return the first certificate in the collection, has the right name and is current.
            cert = signingCert.OfType<X509Certificate2>().OrderByDescending(c => c.NotBefore).FirstOrDefault();
            store.Close();
            return cert;
        }
 
 
        /// <summary>
        /// Get an access token from Azure AD using client credentials.
        /// If the attempt to get a token fails because the server is unavailable, retry twice after 3 seconds each
        /// </summary>
        private static async Task<AuthenticationResult> GetAccessToken(AuthenticationContext authContext, string resourceUri, ClientAssertionCertificate cert)
        {
            //
            // Get an access token from Azure AD using client credentials.
            // If the attempt to get a token fails because the server is unavailable, retry twice after 3 seconds each.
            //
            AuthenticationResult result = null;
            int retryCount = 0;
            bool retry = false;
 
            do
            {
                retry = false;
                errorCode = 0;
 
                try
                {
                    result = await authContext.AcquireTokenAsync(resourceUri, cert);
                }
                catch (AdalException ex)
                {
                    if (ex.ErrorCode == "temporarily_unavailable")
                    {
                        retry = true;
                        retryCount++;
                        Thread.Sleep(3000);
                    }
 
                    Console.WriteLine(
                        String.Format("An error occurred while acquiring a token\nTime: {0}\nError: {1}\nRetry: {2}\n",
                        DateTime.Now.ToString(),
                        ex.ToString(),
                        retry.ToString()));
 
                    errorCode = -1;
                }
 
            } while ((retry == true) && (retryCount < 3));
            return result;
        }
    }
}
```

Dit script levert de primaire en secundaire hoofdsleutels zoals wordt weergegeven in de volgende schermafbeelding:

![uitvoer voor csharp-toepassing](./media/certificate-based-authentication/csharp-application-output.png)

Net als bij de vorige sectie, vindt u het activiteitenlogboek van uw Azure Cosmos-account te valideren dat de gebeurtenis get sleutels aanvraag wordt geïnitieerd door de toepassing 'sampleApp'. 


## <a name="next-steps"></a>Volgende stappen

* [Azure Cosmos-sleutels met behulp van Azure Key Vault beveiligen](access-secrets-from-keyvault.md)

* [Veiligheidskenmerken voor Azure Cosmos DB](cosmos-db-security-attributes.md)
