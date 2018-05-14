---
title: Een Azure-web-toepassing om te lezen van een geheim van de sleutelkluis configureren | Microsoft Docs
description: Zelfstudie een ASP.Net core toepassing configureren voor het lezen van een geheim van de sleutelkluis
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: identity
ms.topic: article
ms.date: 04/16/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: b4e317a82b93513c6161d9da0c55883e99580cbb
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="tutorial-configure-an-azure-web-application-to-read-a-secret-from-key-vault"></a>Zelfstudie: Een Azure-web-toepassing om te lezen van een geheim van de Sleutelkluis configureren

In deze zelfstudie gaat u over de benodigde stappen voor een Azure-webtoepassing om informatie te lezen uit met beheerde service-identiteiten sleutelkluis. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een Sleutelkluis maken.
> * Sla een geheim in de Sleutelkluis.
> * Een Azure-webtoepassing maken.
> * Beheerde service-identiteiten inschakelen
> * De vereiste machtigingen voor de toepassing gegevens lezen uit de sleutelkluis.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie Azure CLI 2.0.4 of nieuwer uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

Voor het aanmelden bij de Azure met behulp van de CLI, kunt u het volgende typen:

```azurecli
az login
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az_group_create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli
az group create --name ContosoResourceGroup --location eastus
```

De resourcegroep die u zojuist hebt gemaakt wordt in deze zelfstudie gebruikt.

## <a name="create-an-azure-key-vault"></a>Een Azure Key Vault maken

Vervolgens maakt u een Sleutelkluis in de resourcegroep in de vorige stap hebt gemaakt. Bepaalde informatie, moet u opgeven:

>[!NOTE]
> Hoewel 'ContosoKeyVault' als naam voor onze Sleutelkluis in deze zelfstudie wordt gebruikt, moet u een unieke naam op.

* De kluisnaam van de **ContosoKeyVault**.
* Naam van resourcegroep **ContosoResourceGroup**.
* De locatie **VS - oost**.

```azurecli
az keyvault create --name '<YourKeyVaultName>' --resource-group ContosoResourceGroup --location eastus
```

De uitvoer van deze opdracht worden de eigenschappen van de nieuwe Sleutelkluis. Let op de onderstaande twee eigenschappen:

* **Vault Name**: in het voorbeeld is dit **ContosoKeyVault**. U gebruikt de naam van uw Sleutelkluis voor alle Sleutelkluis-opdrachten.
* **Vault URI**: In het voorbeeld is dit https://<YourKeyVaultName>.vault.azure.net/. Toepassingen die via de REST API gebruikmaken van uw kluis, moeten deze URI gebruiken.

>[!IMPORTANT]
> Als u de fout Parameter 'vault_name krijgt' aan het volgende patroon volgen voldoen moet: ' ^ [een-zA-Z0 - 9-]{3,24}$'-naam param waarde is niet uniek of voldoet niet aan voor een tekenreeks die bestaat uit alfanumerieke tekens van 3 tot 24 lang.

Vanaf dit punt is uw Azure-account nu als enige gemachtigd om bewerkingen op deze nieuwe kluis uit te voeren.

## <a name="add-a-secret-to-key-vault"></a>Een geheim toevoegen aan sleutelkluis

We toevoegen een geheim te verduidelijken hoe dit werkt als u wilt. U kan een SQL-verbindingsreeks of andere informatie die u veilig behouden maar beschikbaar stellen voor uw toepassing moet worden opgeslagen. In deze zelfstudie het wachtwoord moet worden aangeroepen **AppSecret** en slaat de waarde van **MySecret** erin.

Typ de onderstaande opdrachten voor het maken van een geheim in de Sleutelkluis aangeroepen **AppSecret** die de waarde wordt opgeslagen **MySecret**:

```azurecli
az keyvault secret set --vault-name '<YourKeyVaultName>' --name 'AppSecret' --value 'MySecret'
```

Als u de waarde in het geheim als tekst zonder opmaak wilt weergeven:

```azurecli
az keyvault secret show --name 'AppSecret' --vault-name '<YourKeyVaultName>'
```

Deze opdracht geeft de geheime informatie, met inbegrip van de URI. Na het voltooien van deze stappen hebt u een URI naar een geheim in een Azure Sleutelkluis. Noteer deze informatie. U moet een latere stap.

## <a name="create-a-web-app"></a>Een webtoepassing maken

In deze sectie een ASP.NET MVC-toepassing maken en deze implementeren in Azure als een Web-App. Zie voor meer informatie over Azure Web Apps [overzicht van Web-Apps](../app-service/app-service-web-overview.md).

1. Maak in Visual Studio een project door **Bestand > Nieuw > Project** te selecteren. 

2. Selecteer **Visual C# > Web > ASP.NET Core-webtoepassing** in het dialoogvenster **Nieuw project**.

3. Naam van de toepassing **WebKeyVault**, en selecteer vervolgens **OK**.
   >[!IMPORTANT]
   > U moet de app WebKeyVault naam zodat de code die u kopieert en plakt komt overeen met de naamruimte. Als u de naam van de site iets anders moet u de code zodat deze overeenkomen met de naam van de site te wijzigen.

    ![Het dialoogvenster Nieuw ASP.NET-project](media/tutorial-web-application-keyvault/aspnet-dialog.png)

4. U kunt elk type ASP.NET Core-web-app implementeren in Azure. Voor deze zelfstudie selecteert u de **webtoepassing** sjabloon, en zorg ervoor dat de verificatie is ingesteld op **geen verificatie**.

    ![ASPNET geen verificatiedialoogvenster](media/tutorial-web-application-keyvault/aspnet-noauth.png)

5. Selecteer **OK**.

6. Nadat het ASP.NET Core-project is gemaakt, wordt de welkomstpagina van ASP.NET Core weergegeven. Deze biedt talrijke links naar bronnen die u op weg helpen.

7. Selecteer in het menu **Fouten opsporen > Starten zonder foutopsporing** om de web-app lokaal uit te voeren.

## <a name="modify-the-web-app"></a>De web-app wijzigen

Er zijn twee NuGet-pakketten dat uw webtoepassing moet zijn geïnstalleerd. Als u wilt installeren stappen ze de volgende:

1. In solution explorer met de rechtermuisknop op de websitenaam van uw.
2. Selecteer **beheren NuGet-pakketten voor oplossing...**
3. Selecteer het selectievakje in naast het zoekvak. **Voorlopige versie opnemen**
4. Zoeken naar de twee NuGet-pakketten die hieronder worden vermeld en accepteer ze kunnen worden toegevoegd aan uw oplossing:

    * [Microsoft.Azure.Services.AppAuthentication (preview)](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) -kunt u gemakkelijk toegangstokens voor verificatie scenario's voor Service-naar-Azure-Service ophalen. 
    * [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault/2.4.0-preview) -methoden voor interactie met Sleutelkluis bevat.

5. Solution Explorer gebruiken om te openen `Program.cs` en vervang de inhoud van het bestand Program.cs met de volgende code. Vervang ```<YourKeyVaultName>``` met de naam van de sleutelkluis:

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
             )
                .UseStartup<Startup>()
                .Build();
    
            private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
        }
        }
    ```

6. Solution Explorer gebruiken om te navigeren naar de **pagina's** sectie en open `About.cshtml`. Vervang de inhoud van **About.cshtml.cs** met de volgende code:

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

7. Kies in het hoofdmenu **Debug** > **starten zonder foutopsporing**. Wanneer de browser wordt weergegeven, gaat u naar de **over** pagina. De waarde voor de AppSecret wordt weergegeven.

>[!IMPORTANT]
> Als u krijgt een HTTP-fout 502.5 - proces foutbericht Controleer de naam van de Sleutelkluis die is opgegeven in `Program.cs`

## <a name="publish-the-web-application-to-azure"></a>Publiceer de webtoepassing naar Azure

1. Selecteer boven de editor **WebKeyVault**.
2. Selecteer **publiceren**.
3. Selecteer **publiceren** opnieuw.
4. Selecteer **maken**.

>[!IMPORTANT]
> Een browservenster wordt geopend en u een 502.5 - fout in het bericht te zien. Dit is normaal. U moet de toepassing identiteit rechten toekennen voor het lezen van geheimen van de Sleutelkluis.

## <a name="enable-managed-service-identity"></a>Beheerde Service-identiteit inschakelen

Azure Sleutelkluis biedt een manier voor het veilig opslaan van referenties en andere sleutels en geheimen, maar uw code moet worden geverifieerd voor Sleutelkluis om op te halen ze. Beheerde Service-identiteit (MSI) maakt het oplossen van dit probleem eenvoudiger door middel van een identiteit automatisch beheerde Azure-services in Azure Active Directory (Azure AD). U kunt deze identiteit gebruiken om alle services die ondersteuning biedt voor Azure AD-verificatie, met inbegrip van de Sleutelkluis, zonder dat u geen referenties hoeft in uw code te verifiëren.

1. Ga terug naar de Azure CLI
2. Voer de opdracht toewijzen identiteit voor het maken van de identiteit voor deze toepassing:

```azurecli
az webapp assign-identity --name WebKeyVault --resource-group ContosoResourcegroup
```

>[!NOTE]
>Dit is het equivalent van de portal gaan en overschakelingen van **beheerde service-identiteit** naar **op** in eigenschappen van de webtoepassing.

## <a name="grant-rights-to-the-application-identity"></a>Verleen machtigingen voor de toepassings-id

Met de Azure portal, Ga naar de Sleutelkluis-beleid en zelf geheim Management toegang verlenen tot de Sleutelkluis. Hierdoor kunt u de toepassing uitvoeren op uw lokale ontwikkelcomputer.

1. Zoeken naar uw Sleutelkluis in de **zoeken in Resources** dialoogvenster in de Azure-portal.
2. Selecteer **toegangsbeleid**.
3. Selecteer **nieuwe toevoegen**, in de **geheime machtigingen** Selecteer sectie **ophalen** en **lijst**.
4. Selecteer **Principal selecteren**, en voeg de toepassings-id. Deze heeft dezelfde naam als de toepassing.
5. Kies **Ok**

Uw account in Azure en de toepassings-id hebt nu rights om informatie te lezen uit de Sleutelkluis. Als u de pagina vernieuwt, ziet u moet de startpagina van de site. Als u selecteert **over**. Ziet u de waarde die u in de Sleutelkluis opgeslagen.

## <a name="clean-up-resources"></a>Resources opschonen

U een resourcegroep en de bijhorende resources verwijderen met de **az groep verwijderen** opdracht.

  ```azurecli
  az group delete -n ContosoResourceGroup
  ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ontwikkelaarshandleiding Azure Sleutelkluis](key-vault-developers-guide.md)
