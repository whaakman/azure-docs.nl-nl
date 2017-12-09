---
title: Veilig opslaan van de geheime toepassingsinstellingen voor een webtoepassing | Microsoft Docs
description: Hoe veilig opslaan geheime toepassingsinstellingen zoals Azure-referenties of een derde partij API sleutels met ASP.NET core Key Vault Provider, gebruiker geheim of .NET 4.7.1 configuratie opbouwfuncties
services: visualstudio
documentationcenter: 
author: cawa
manager: paulyuk
editor: 
ms.assetid: 
ms.service: 
ms.workload: web, azure
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cawa
ms.openlocfilehash: 612b8d2c36e9b46e99452e78d1b30fda03474151
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="securely-save-secret-application-settings-for-a-web-application"></a>Geheime toepassingsinstellingen voor een webtoepassing veilig opslaan

## <a name="overview"></a>Overzicht
In dit artikel wordt beschreven hoe veilig opslaan geheime toepassing configuratie-instellingen voor Azure-toepassingen.

Traditioneel web alle instellingen worden opgeslagen in de configuratiebestanden zoals Web.config toepassingsconfiguratie. Deze oefening leidt tot het inchecken van geheime instellingen zoals Cloud referenties voor het openbare bronsystemen besturingselement zoals Github. Ondertussen kan het zijn moeilijk te volgen aanbevolen beveiligingsprocedure vanwege de overhead voor het wijzigen van de broncode en ontwikkeling instellingen opnieuw configureren.

Om er zeker van te zijn ontwikkelingsproces is beveiligd, tooling en framework bibliotheken gemaakt als geheime toepassingsinstellingen veilig met minimale of geen wijziging in de gegevensbron code wilt opslaan.

## <a name="aspnet-and-net-core-applications"></a>ASP.NET en .NET core-toepassingen

### <a name="save-secret-settings-in-user-secret-store-that-is-outside-of-source-control-folder"></a>Geheime instellingen opslaan in archief geheim van de gebruiker die buiten het besturingselement bronmap
Als u een snelle prototype uitvoert of u geen toegang tot internet hebt, kunt u beginnen met het verplaatsen van de instellingen van uw geheime buiten de bronmap van het besturingselement naar gebruiker geheim store. Geheime archief van de gebruiker is een bestand opgeslagen onder de map van de profiler gebruiker, zodat geheimen niet met resourcebeheer ingecheckt zijn. Het volgende diagram laat zien hoe [gebruiker geheim](https://docs.microsoft.com/aspnet/core/security/app-secrets?tabs=visual-studio#SecretManager) werkt.

![Gebruiker geheim houdt geheime-instellingen buiten bronbeheer](./media/vs-secure-secret-appsettings/aspnetcore-usersecret.PNG)

Als u .NET core-consoletoepassing uitvoert, gebruikt u de Sleutelkluis uw geheime veilig opslaan.

### <a name="save-secret-settings-in-azure-key-vault"></a>Geheime instellingen opslaan in Azure Sleutelkluis
Als u een teamproject ontwikkelt en moet broncode veilig delen, gebruikt u [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

1. Een Sleutelkluis maken in uw Azure-abonnement. Vul alle vereiste velden op de gebruikersinterface en op *maken* onderaan de blade

    ![Maken van Azure Sleutelkluis](./media/vs-secure-secret-appsettings/create-keyvault.PNG)

2. U en uw team leden toegang verlenen aan de Sleutelkluis. Als u een grote team hebt, kunt u een [Azure Active Directory-groep](https://docs.microsoft.com/azure/active-directory/active-directory-groups-create-azure-portal) en voeg die beveiliging groepstoegang toe aan de Sleutelkluis. In de *geheim machtigingen* vervolgkeuzelijst selectievakje *ophalen* en *lijst* onder *geheim beheerbewerkingen*.

    ![Sleutelkluis-beleid toevoegen](./media/vs-secure-secret-appsettings/add-keyvault-access-policy.png)

3. Uw geheime toevoegen aan Sleutelkluis in Azure portal. Voor geneste configuratie-instellingen, vervangt u ':' aan '--' zodat de geheime naam van de Sleutelkluis ongeldig is. ':' mag niet zijn in de naam van een Sleutelkluis geheim.

    ![Sleutelkluis geheim toevoegen](./media/vs-secure-secret-appsettings/add-keyvault-secret.png)

4. Installeer de [Azure Services verificatie-extensie voor Visual Studio](https://go.microsoft.com/fwlink/?linkid=862354). App toegankelijk via deze uitbreiding met behulp van Visual Studio-in de identiteit van de Sleutelkluis.

5. De volgende NuGet-pakketten aan uw project toevoegen:

    ```
    Microsoft.Azure.Services.AppAuthentication
    ```
6. Voeg de volgende code naar het bestand Program.cs:

    ```csharp
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
                    })
                    .UseStartup<Startup>()
                    .Build();

        private static string GetKeyVaultEndpoint() => Environment.GetEnvironmentVariable("KEYVAULT_ENDPOINT");
    ```
7. URL voor uw Sleutelkluis aan launchsettings.json bestand toevoegen. Naam van de omgevingsvariabele *KEYVAULT_ENDPOINT* is gedefinieerd in de code die u in stap 6 hebt toegevoegd.

    ![URL voor Sleutelkluis als een omgevingsvariabele project toevoegen](./media/vs-secure-secret-appsettings/add-keyvault-url.png)

8. Foutopsporing van het project te starten. Het moet is uitgevoerd.

## <a name="aspnet-and-net-applications"></a>ASP.NET en .NET-toepassingen

.NET 4.7.1 ondersteunt Sleutelkluis en geheim configuratie opbouwfuncties, waardoor geheimen kunnen worden verplaatst buiten de bronmap voor beheer zonder codewijzigingen.
Om door te gaan, [.NET 4.7.1 downloaden](https://www.microsoft.com/download/details.aspx?id=56115) en migreren van uw toepassing als een oudere versie van .NET framework wordt gebruikt.

### <a name="save-secret-settings-in-a-secret-file-that-is-outside-of-source-control-folder"></a>Geheime instellingen opslaan in een geheime die buiten het besturingselement bronmap
Als u een snelle prototype schrijft en Azure-resources inrichten niet wilt, gaat u met deze optie.

1. Het volgende NuGet-pakket installeren voor uw project
    ```
    Microsoft.Configuration.ConfigurationBuilders.Basic.1.0.0-alpha1.nupkg
    ```

2. Maak een bestand dat lijkt op het volgen. Deze onder een locatie buiten de projectmap opslaan.

    ```xml

       <root>
              <secrets ver="1.0">
                     <secret name="secret1" value="foo_one" />
                        <secret name="secret2" value="foo_two" />
                       </secrets>
      </root>
      ```

3. Definieer het geheime bestand om te worden van een opbouwfunctie configuratie in het Web.config-bestand. Deze sectie voordat plaatsen *appSettings* sectie.

    ```xml
    <configBuilders>
        <builders>
            <add name="Secrets"
                 secretsFile="C:\Users\AppData\MyWebApplication1\secret.xml" type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
                    Microsoft.Configuration.ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```

4. Geef de sectie appSettings met behulp van de opbouwfunctie voor het geheime configuratie. Zorg ervoor dat er een vermelding voor de geheime instelling met een dummy-waarde.

    ```xml
        <appSettings configBuilders="Secrets">
            <add key="webpages:Version" value="3.0.0.0" />
            <add key="webpages:Enabled" value="false" />
            <add key="ClientValidationEnabled" value="true" />
            <add key="UnobtrusiveJavaScriptEnabled" value="true" />
            <add key="secret" value="" />
        </appSettings>
    ```

5. Fouten opsporen in uw app. Het moet is uitgevoerd.

### <a name="save-secret-settings-in-an-azure-key-vault"></a>Geheime instellingen opslaan in een Azure Sleutelkluis
Volg de instructies van ASP.NET core sectie voor het configureren van een Sleutelkluis voor uw project.

1. Het volgende NuGet-pakket installeren voor uw project
```
Microsoft.Configuration.ConfigurationBuilders.Azure.1.0.0-alpha1.nupkg
```

2. Definieer Sleutelkluis configuratie builder in Web.config. Deze sectie voordat plaatsen *appSettings* sectie. Vervang *vaultName* moet de naam van de Sleutelkluis als uw Sleutelkluis in openbare Azure of de volledige URI als u van soevereine cloud gebruikmaakt.

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>
    <configBuilders>
        <builders>
            <add name="KeyVault" vaultName="Test911" type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```
3.  Geef de sectie appSettings met behulp van de opbouwfunctie voor Sleutelkluis-configuratie. Zorg ervoor dat er een vermelding voor de geheime instelling met een dummy-waarde.

    ```xml
    <appSettings configBuilders="AzureKeyVault">
        <add key="webpages:Version" value="3.0.0.0" />
        <add key="webpages:Enabled" value="false" />
        <add key="ClientValidationEnabled" value="true" />
        <add key="UnobtrusiveJavaScriptEnabled" value="true" />
        <add key="secret" value="" />
    </appSettings>
    ```

4. Foutopsporing van het project te starten. Het moet is uitgevoerd.
