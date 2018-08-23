---
title: 'Zelfstudie: Een Azure-webtoepassing configureren voor het lezen van een geheim uit Key Vault | Microsoft Docs'
description: 'Zelfstudie: Een ASP.Net Core-toepassing configureren voor het lezen van een geheim uit Key Vault'
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 05/17/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 91e2047998d6e743691821c631e15c94cd63cf15
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41918907"
---
# <a name="tutorial-configure-an-azure-web-application-to-read-a-secret-from-key-vault"></a>Zelfstudie: Een Azure-webtoepassing configureren voor het lezen van een geheim uit Key Vault

In deze zelfstudie gaat u de stappen uitvoeren die nodig om een Azure-webtoepassing met behulp van beheerde service-identiteiten gegevens te laten lezen uit Key Vault. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een sleutelkluis maken.
> * Een geheim opslaan in Key Vault.
> * Een Azure-webtoepassing maken.
> * Beheerde service-identiteiten inschakelen.
> * De vereiste machtigingen verlenen aan de toepassing om gegevens te lezen uit Key Vault.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie Azure CLI 2.0.4 of nieuwer uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

Gebruik de volgende opdracht om u aan te melden bij Azure met behulp van de CLI:

```azurecli
az login
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az-group-create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam *ContosoResourceGroup* in de locatie *eastus* gemaakt.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "ContosoResourceGroup" --location "East US"
```

De resourcegroep die u zojuist hebt gemaakt, wordt overal in deze zelfstudie gebruikt.

## <a name="create-an-azure-key-vault"></a>Een Azure Key Vault maken

Vervolgens maakt u een sleutelkluis in de resourcegroep die u in de vorige stap hebt gemaakt. Hoewel in deze zelfstudie 'ContosoKeyVault' wordt gebruikt als de naam voor de sleutelkluis, moet u een unieke naam gebruiken. Geef de volgende informatie op:

* Naam van kluis **ContosoKeyVault**
* Naam van resourcegroep **ContosoResourceGroup**.
* De locatie **US - oost**.

```azurecli
az keyvault create --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --location "East US"
```

De uitvoer van deze opdracht toont eigenschappen van de nieuw gemaakte sleutelkluis. Let op de onderstaande twee eigenschappen:

* **Vault Name**: in het voorbeeld is dit **ContosoKeyVault**. U gebruikt de naam van uw sleutelkluis voor alle Key Vault-opdrachten.
* **URI van kluis**: in het voorbeeld is dit https://<YourKeyVaultName>vault.azure.net/. Toepassingen die via de REST API gebruikmaken van uw kluis, moeten deze URI gebruiken.

>[!IMPORTANT]
> Als u de fout Parameter 'vault_name' must conform to the following pattern: '^[a-zA-Z0-9-]{3,24}$' krijgt, is de opgegeven naam voor de kluis niet uniek of bestaat deze niet uit een tekenreeks van 3 tot 24 alfanumerieke tekens.

Vanaf dit punt is uw Azure-account nu als enige gemachtigd om bewerkingen op deze nieuwe kluis uit te voeren.

## <a name="add-a-secret-to-key-vault"></a>Een geheim toevoegen aan Key Vault

We gaan een geheim toevoegen om te laten zien hoe dit werkt. U kunt een SQL-verbindingsreeks opslaan of andere gegevens die u veilig wilt bewaren, maar wel beschikbaar wilt stellen aan uw toepassing. In deze zelfstudie gebruiken we het geheim **AppSecret** en slaan we de waarde van **MySecret** op in het geheim.

Typ de onderstaande opdrachten om in Key Vault een geheim te maken met de naam **AppSecret** waarin de waarde van **MySecret** wordt opgeslagen:

```azurecli
az keyvault secret set --vault-name "ContosoKeyVault" --name "AppSecret" --value "MySecret"
```

Als u de waarde in het geheim als tekst zonder opmaak wilt weergeven:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "ContosoKeyVault"
```

Met deze opdracht vraagt u de geheime gegevens op, inclusief de URI. Als u deze stappen hebt uitgevoerd, beschikt u over een URI voor een geheim in een Azure-sleutelkluis. Noteer deze informatie. U hebt deze in een latere stap nog nodig.

## <a name="create-a-web-app"></a>Een webtoepassing maken

In deze sectie gaat u een ASP.NET MVC-toepassing maken en deze als een web-app implementeren in Azure. Zie [Overzicht van Web Apps](../app-service/app-service-web-overview.md) voor meer informatie over Azure Web Apps.

1. Maak in Visual Studio een project door **Bestand > Nieuw > Project** te selecteren. 

2. Selecteer **Visual C# > Web > ASP.NET Core-webtoepassing** in het dialoogvenster **Nieuw project**.

3. Geef de toepassing de naam **WebKeyVault** en selecteer vervolgens **OK**.
   >[!IMPORTANT]
   > U moet de app de naam WebKeyVault geven, zodat de code die u kopieert en plakt overeenkomt met de naamruimte. Als u een andere naam opgeeft, moet u de naam in de code aanpassen.

    ![Het dialoogvenster Nieuw ASP.NET-project](media/tutorial-web-application-keyvault/aspnet-dialog.png)

4. U kunt elk type ASP.NET Core-web-app implementeren in Azure. Voor deze zelfstudie selecteert u de sjabloon **Web Application** en stelt u de verificatiemethode in op **No Authentication**.

    ![ASP.NET-dialoogvenster met No Authentication](media/tutorial-web-application-keyvault/aspnet-noauth.png)

5. Selecteer **OK**.

6. Nadat het ASP.NET Core-project is gemaakt, wordt de welkomstpagina van ASP.NET Core weergegeven. Deze biedt talrijke links naar bronnen die u op weg helpen.

7. Selecteer in het menu **Fouten opsporen > Starten zonder foutopsporing** om de web-app lokaal uit te voeren.

## <a name="modify-the-web-app"></a>De web-app wijzigen

Er zijn twee NuGet-pakketten die moeten zijn geïnstalleerd voor uw webtoepassing. Ga als volgt te werk om de pakketten te installeren:

1. Klik in Solution Explorer met de rechtermuisknop op de naam van uw website.
2. Selecteer **Manage NuGet packages for solution...**
3. Schakel het selectievakje naast het zoekvak in, **Include prerelease**.
4. Zoek naar de twee onderstaande NuGet-pakketten en accepteer ze, zodat ze kunnen worden toegevoegd aan uw oplossing:

    * [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) - hiermee kunt u eenvoudig toegangstokens ophalen voor verificatiescenario's voor Service-naar-Azure-service. 
    * [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) - bevat methoden voor interactie met Key Vault.

5. Gebruik Solution Explorer om `Program.cs` te openen en vervang de inhoud van het bestand Program.cs met de volgende code. Vervang ```<YourKeyVaultName>``` door de naam van de sleutelkluis:

    ```csharp
    
    using Microsoft.AspNetCore;
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureKeyVault;
    
    namespace WebKeyVault
    {
       public class Program
       {
           public static void Main(string[] args)
           {
               BuildWebHost(args).Run();
           }

           public static IWebHost BuildWebHost(string[] args) =>
           WebHost.CreateDefaultBuilder(args)
               .ConfigureAppConfiguration((ctx, builder) =>
               {
                   var keyVaultEndpoint = GetKeyVaultEndpoint();
                   if (!string.IsNullOrEmpty(keyVaultEndpoint))
                   {
                       var azureServiceTokenProvider = new AzureServiceTokenProvider();
                       var keyVaultClient = new KeyVaultClient(
                           new KeyVaultClient.AuthenticationCallback(
                               azureServiceTokenProvider.KeyVaultTokenCallback));
                       builder.AddAzureKeyVault(
                           keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                   }
               }
            ).UseStartup<Startup>()
             .Build();

           private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
         }
    }
    ```

6. Ga in Solution Explorer naar de sectie **Pages** en open `About.cshtml`. Vervang de inhoud van **About.cshtml.cs** door de volgende code:

    ```csharp
    
    using Microsoft.AspNetCore.Mvc.RazorPages;
    using Microsoft.Extensions.Configuration;
    
    namespace WebKeyVault.Pages
    {
        public class AboutModel : PageModel
        {
            public AboutModel(IConfiguration configuration)
            {
                _configuration = configuration;
            }
    
            private readonly IConfiguration _configuration = null;
            public string Message { get; set; }
    
            public void OnGet()
            {
                Message = "My key val = " +  _configuration["AppSecret"];
            }
        }
    }
    
    ```

7. Kies in het hoofdmenu **Debug** > **Start without Debugging**. Wanneer de browser wordt weergegeven, gaat u naar de pagina **About**. De waarde voor AppSecret wordt weergegeven.

>[!IMPORTANT]
> Als u het bericht HTTP-fout 502.5 - Verwerkingsfout krijgt,
> > controleer dan de naam van de sleutelkluis die in `Program.cs` is opgegeven

## <a name="publish-the-web-application-to-azure"></a>De webtoepassing publiceren in Azure

1. Selecteer boven de editor **WebKeyVault**.
2. Selecteer **Publish** en daarna **Start**.
3. Maak een nieuwe **App Service** en selecteer **Publish**.
4. Selecteer **Maken**.

>[!IMPORTANT]
> Er wordt een browservenster geopend en u ziet het bericht 502.5 - Process Failure. Dit is normaal gedrag. U moet de toepassings-id namelijk rechten toekennen voor het lezen van geheimen uit Key Vault.

## <a name="enable-managed-service-identity"></a>Beheerde service-identiteit inschakelen

Azure Key Vault biedt een manier voor het veilig opslaan van referenties en andere sleutels en geheimen, maar uw code moet worden geverifieerd voor Key Vault om ze op te halen. Managed Service Identity (MSI) levert Azure-services met een automatisch beheerde identiteit in Azure Active Directory (Azure AD), waarmee dit probleem eenvoudiger kan worden opgelost. U kunt deze identiteit gebruiken voor verificatie bij alle services die ondersteuning bieden voor Azure AD-verificatie, inclusief Key Vault, zonder dat u referenties in uw code hoeft te hebben.

1. Ga terug naar de Azure CLI.
2. Voer de opdracht identity assign uit om de identiteit voor deze toepassing te maken:

```azurecli
az webapp identity assign --name "WebKeyVault" --resource-group "ContosoResourcegroup"
```

>[!NOTE]
>U kunt dit trouwens ook doen door naar de portal te gaan en **Managed service identity** op**On** te zetten in de eigenschappen van de webtoepassing.

## <a name="grant-rights-to-the-application-identity"></a>Rechten verlenen aan de toepassings-id

Ga in Azure Portal naar de regels voor toegangsbeleid van Key Vault en geef uzelf de machtiging Geheimenbeheer voor Key Vault. Hierdoor kunt u de toepassing uitvoeren op uw lokale ontwikkelcomputer.

1. Zoek naar uw sleutelkluis in het**zoekvak in** Azure Portal.
2. Selecteer **Toegangsbeleid**.
3. Selecteer **Nieuwe toevoegen** in de sectie **Geheime machtigingen** en selecteer vervolgens **Ophalen** en **Weergeven**.
4. Selecteer **Principal selecteren** en voeg de toepassings-id toe. Deze heeft dezelfde naam als de toepassing.
5. Kies **OK**.

Uw account in Azure en de toepassings-id hebben nu rechten om informatie te lezen uit Key Vault. Wanneer u de pagina vernieuwt, ziet u de startpagina van de site. Als u **Over** selecteert, ziet u de waarde die u in Key Vault hebt opgeslagen.

## <a name="clean-up-resources"></a>Resources opschonen

Voer de opdracht **az group delete** uit om de resourcegroep en alle bijbehorende resources te verwijderen.

  ```azurecli
  az group delete -n "ContosoResourceGroup"
  ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gids voor Azure Key Vault-ontwikkelaars](key-vault-developers-guide.md)