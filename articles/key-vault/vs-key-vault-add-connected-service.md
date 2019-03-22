---
title: Ondersteuning voor Key Vault toevoegen aan uw ASP.NET-project met Visual Studio - Azure Key Vault | Microsoft Docs
description: Gebruik deze zelfstudie voor meer informatie over hoe u Key Vault-ondersteuning toevoegt aan een ASP.NET- of ASP.NET Core-webtoepassing.
services: key-vault
author: ghogen
manager: jillfra
ms.prod: visual-studio
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: ghogen
ms.openlocfilehash: d95bd114be712953b79ef5afbb0915173f6de26c
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58339275"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Key Vault toevoegen aan uw web-App met behulp van Visual Studio verbonden Services

In deze zelfstudie leert u hoe eenvoudig u alles wat die u nodig hebt om te starten met Azure Key Vault voor het beheren van uw geheimen voor webprojecten in Visual Studio, of u van ASP.NET Core of elk type ASP.NET-project gebruikmaakt toevoegt. U kunt met behulp van de functie voor Services die zijn verbonden in Visual Studio, Visual Studio automatisch toevoegen van de NuGet-pakketten en configuratie-instellingen die u wilt verbinding maken met Key Vault in Azure hebben. 

Zie voor meer informatie over de wijzigingen Connected Services voor het inschakelen van Key Vault maakt in uw project, [Key Vault Connected Service - wat is er gebeurd met mijn ASP.NET 4.7.1 project](#how-your-aspnet-framework-project-is-modified) of [Key Vault Connected Service - wat is er gebeurd met mijn ASP.NET Core-project](#how-your-aspnet-core-project-is-modified).

## <a name="prerequisites"></a>Vereisten

- **Een Azure-abonnement**. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio 2019** of **Visual Studio 2017 versie 15,7** met de **webontwikkeling** werkbelasting geïnstalleerd. [Download nu](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- Voor ASP.NET (niet-Core) met Visual Studio 2017 moet u de .NET Framework 4.7.1 of hoger ontwikkelprogramma's, die niet standaard worden geïnstalleerd. Als u wilt installeren, start het installatieprogramma van Visual Studio, kiest u **wijzigen**, en kies vervolgens **afzonderlijke onderdelen**, vouw vervolgens aan de rechterkant **ASP.NET en webontwikkeling**, en kies **.NET Framework 4.7.1 ontwikkelprogramma's**.
- Een ASP.NET-4.7.1 of hoger, of ASP.NET Core 2.0-webproject openen.

## <a name="add-key-vault-support-to-your-project"></a>Key Vault-ondersteuning aan uw project toevoegen

1. In **Solution Explorer** kiest u **Connected Service** > **Toevoegen**.
   De Connected Service-pagina wordt weergegeven met services die u aan uw project kunt toevoegen.
1. Kies in het menu van de beschikbare services, **beveiligde geheimen met Azure Key Vault**.

   ![Kies "Beveiligde geheimen met Azure Key Vault"](media/vs-key-vault-add-connected-service/KeyVaultConnectedService1.PNG)

   Als u bent aangemeld bij Visual Studio en een Azure-abonnement hebt dat is gekoppeld aan uw account, wordt een pagina weergegeven met een vervolgkeuzelijst met uw abonnementen. Zorg ervoor dat u bent aangemeld bij Visual Studio, en dat het account dat u bent aangemeld met hetzelfde account dat u voor uw Azure-abonnement gebruiken.

1. Selecteer het abonnement dat u wilt gebruiken, en kies vervolgens een nieuwe of bestaande Key Vault of kies de koppeling bewerken om te wijzigen van de automatisch gegenereerde naam.

   ![Selecteer uw abonnement](media/vs-key-vault-add-connected-service/KeyVaultConnectedService3.PNG)

1. Typ de naam die u wilt gebruiken voor de Key Vault.

   ![Wijzig de naam van de Key Vault en kies een resourcegroep](media/vs-key-vault-add-connected-service/KeyVaultConnectedService-Edit.PNG)

1. Selecteer een bestaande resourcegroep of maak een nieuw bestand met een automatisch gegenereerde unieke naam.  Als u een nieuwe groep maken met een andere naam wilt, kunt u de [Azure Portal](https://portal.azure.com), en de pagina te sluiten en opnieuw opstarten om opnieuw te laden van de lijst met resourcegroepen.
1. Kies de regio waarin u wilt maken van de Key Vault. Als uw webtoepassing wordt gehost in Azure, kiest u de regio die als host fungeert voor de web-App voor optimale prestaties.
1. Kies een prijsmodel. Zie voor meer informatie, [prijzen van Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).
1. Klik op OK om te accepteren van de configuratie-opties.
1. Kies **toevoegen** te maken van de Key Vault. Het proces maken kan mislukken als u ervoor kiest een naam die is gebruikt.  Als dit gebeurt, gebruikt u de **bewerken** koppeling naar de naam van de Key Vault en probeer het opnieuw.

   ![Verbonden service toevoegen aan project](media/vs-key-vault-add-connected-service/KeyVaultConnectedService4.PNG)

1. Nu u een geheim toevoegen in uw Key Vault in Azure. Als u naar de juiste plaats in de portal, klik op de koppeling voor beheren geheimen die zijn opgeslagen in deze Key Vault. Als u de pagina of het project hebt gesloten, u kunt navigeren in de [Azure-portal](https://portal.azure.com) door te kiezen **alle Services**onder **Security**, kiest u **Key Vault**, en kies vervolgens de Sleutelkluis die u hebt gemaakt.

   ![Navigeren naar de portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. In de sectie Key Vault voor de key vault u hebt gemaakt, kiest u **geheimen**, klikt u vervolgens **genereren/importeren**.

   ![Een geheim genereren/importeren](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Voer een geheim, zoals "MySecret" en wijs hieraan een string-waarde als een test en kies vervolgens de **maken** knop.

   ![Een geheim maken](media/vs-key-vault-add-connected-service/create-a-secret.jpg)

1. (optioneel) Voer een andere geheim, maar deze keer plaatsen deze in een categorie door de naam van het 'Geheimen--MySecret'. Deze syntaxis Hiermee geeft u een categorie 'Geheimen' met een geheim "MySecret."
 
U kunt nu toegang tot uw geheimen in de code. De volgende stappen zijn verschillend, afhankelijk van of u van ASP.NET 4.7.1 of ASP.NET Core gebruikmaakt.

## <a name="access-your-secrets-in-code"></a>Toegang tot uw geheimen in de code

1. Deze twee nuget-pakketten installeren [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) en [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) NuGet-bibliotheken.

2. Open het bestand Program.cs en de code bijwerken met de volgende code: 
   ```
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
   ```

3. Naast About.cshtml.cs bestand te openen en de volgende code schrijft:
   1. Een verwijzing naar Microsoft.Extensions.Configuration invloed opnemen met de instructie:

       ```csharp
       using Microsoft.Extensions.Configuration
       ```

   1. Deze constructor toevoegen:

       ```csharp
       public AboutModel(IConfiguration configuration)
       {
           _configuration = configuration;
       }
       ```

   1. De methode OnGet update. De tijdelijke aanduidingswaarde wordt hier getoond met de naam van de geheime die u hebt gemaakt in de bovenstaande opdrachten wordt bijgewerkt.

       ```csharp
       public void OnGet()
       {
           //Message = "Your application description page.";
           Message = "My key val = " + _configuration["<YourSecretNameThatWasCreatedAbove>"];
       }
       ```

De app lokaal uitvoeren door te bladeren naar de pagina over. U ziet uw geheime waarde die is opgehaald.

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de resourcegroep als u deze niet meer nodig hebt. Hiermee verwijdert u de Key Vault en de gerelateerde resources. De resourcegroep verwijderen via de portal:

1. Voer de naam van uw resourcegroep in het vak Zoeken bovenaan de portal in. Wanneer u de in deze snelstart gebruikte resourcegroep in de zoekresultaten ziet, selecteert u deze.
2. Selecteer **Resourcegroep verwijderen**.
3. Typ in het vak **TYP DE NAAM VAN DE RESOURCEGROEP** de naam van de resourcegroep en selecteer **Verwijderen**.

## <a name="how-your-aspnet-core-project-is-modified"></a>Hoe uw ASP.NET Core-project is gewijzigd

Deze sectie vindt u de exacte wijzigingen aangebracht in een ASP.NET-project bij het toevoegen van de Key Vault verbonden service met behulp van Visual Studio.

### <a name="added-references"></a>Toegevoegde verwijzingen

Is van invloed op de projectverwijzingen bestand .NET en de verwijzingen naar NuGet-pakket.

| Type | Referentie |
| --- | --- |
| NuGet | Microsoft.AspNetCore.AzureKeyVault.HostingStartup |

### <a name="added-files"></a>Toegevoegde bestanden

- ConnectedService.json toegevoegd, welke records enkele gegevens over de Connected Service-provider, versie en een koppeling van de documentatie.

### <a name="project-file-changes"></a>Wijzigingen in bestanden

- Het bestand verbonden Services ItemGroup en ConnectedServices.json toegevoegd.

### <a name="launchsettingsjson-changes"></a>launchsettings.JSON wijzigingen

- De volgende omgeving variabele vermeldingen toegevoegd aan zowel de IIS Express-profiel als het profiel dat overeenkomt met de naam van uw web-project:

    ```json
      "environmentVariables": {
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED": "true",
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT": "<your keyvault URL>"
      }
    ```

### <a name="changes-on-azure"></a>Wijzigingen in Azure

- Een resourcegroep gemaakt (of een bestaande resourcegroep gebruikt).
- Een Key Vault in de opgegeven resourcegroep gemaakt.

## <a name="how-your-aspnet-framework-project-is-modified"></a>Hoe uw Framework van ASP.NET-project is gewijzigd

Deze sectie vindt u de exacte wijzigingen aangebracht in een ASP.NET-project bij het toevoegen van de Key Vault verbonden service met behulp van Visual Studio.

### <a name="added-references"></a>Toegevoegde verwijzingen

Is van invloed op de projectverwijzingen voor bestand .NET en `packages.config` (NuGet-verwijzingen).

| Type | Referentie |
| --- | --- |
| .NET; NuGet | Microsoft.Azure.KeyVault |
| .NET; NuGet | Microsoft.Azure.KeyVault.WebKey |
| .NET; NuGet | Microsoft.Rest.ClientRuntime |
| .NET; NuGet | Microsoft.Rest.ClientRuntime.Azure |

### <a name="added-files"></a>Toegevoegde bestanden

- ConnectedService.json toegevoegd, welke records enkele gegevens over de Connected Service-provider, versie en een koppeling naar de documentatie.

### <a name="project-file-changes"></a>Wijzigingen in bestanden

- Het bestand verbonden Services ItemGroup en ConnectedServices.json toegevoegd.
- Verwijzingen naar de .NET-assembly's die worden beschreven in de [referenties toegevoegd](#added-references) sectie.

### <a name="webconfig-or-appconfig-changes"></a>Web.config of app.config wijzigingen

- De volgende configuratie-items toegevoegd:

    ```xml
    <configSections>
      <section
           name="configBuilders"
           type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" 
           restartOnExternalChanges="false"
           requirePermission="false" />
    </configSections>
    <configBuilders>
      <builders>
        <add 
             name="AzureKeyVault"
             vaultName="vaultname"
             type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Azure, Version=1.0.0.0, Culture=neutral" 
             vaultUri="https://vaultname.vault.azure.net" />
      </builders>
    </configBuilders>
    ```

### <a name="changes-on-azure"></a>Wijzigingen in Azure

- Een resourcegroep gemaakt (of een bestaande resourcegroep gebruikt).
- Een Key Vault in de opgegeven resourcegroep gemaakt.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het ontwikkelen van Key Vault vindt de [Ontwikkelaarshandleiding voor Key Vault](key-vault-developers-guide.md)