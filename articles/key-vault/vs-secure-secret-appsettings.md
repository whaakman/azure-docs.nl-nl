---
title: Veilig opslaan van geheime toepassingsinstellingen voor een web-App - Azure Key Vault | Microsoft Docs
description: Hoe veilig opslaan geheime toepassingsinstellingen, zoals Azure-referenties of van derden, API-sleutels met behulp van ASP.NET core Key Vault-Provider, gebruiker geheim of .NET 4.7.1 builders van configuratie
services: visualstudio
documentationcenter: ''
author: cawaMS
manager: paulyuk
editor: ''
ms.assetid: ''
ms.service: ''
ms.workload: web, azure
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: cawa
ms.openlocfilehash: 5ed0f861e17c646a66d9fa636e17ce0ba9c7c91a
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53999149"
---
# <a name="securely-save-secret-application-settings-for-a-web-application"></a>Geheime toepassingsinstellingen voor een webtoepassing veilig opslaan

## <a name="overview"></a>Overzicht
In dit artikel wordt beschreven hoe u veilig opslaan geheime toepassing configuratie-instellingen voor Azure-toepassingen.

Alle webservers traditioneel Toepassingsconfiguratie instellingen worden opgeslagen in de configuratiebestanden zoals Web.config. Met deze procedure leidt tot het inchecken van geheime instellingen zoals cloudreferenties naar openbare bronbeheersystemen zoals GitHub. Ondertussen kan het lastig zijn om te volgen best practice bij beveiliging wordt vanwege de overhead vereist voor het wijzigen van de broncode en opnieuw configureren van instellingen voor het ontwikkelen.

Hulpprogramma's en framework-bibliotheken worden om er zeker van te zijn ontwikkelingsproces is beveiligd, om op te slaan geheime toepassingsinstellingen veilig met minimale of geen wijziging in de code gegevensbron gemaakt.

## <a name="aspnet-and-net-core-applications"></a>ASP.NET en .NET core-toepassingen

### <a name="save-secret-settings-in-user-secret-store-that-is-outside-of-source-control-folder"></a>Geheime instellingen opslaan in archief geheim van de gebruiker die buiten de bron-versiebeheermap
Als u een snelle prototype uitvoert of u geen toegang tot internet hebt, kunt u beginnen met het verplaatsen van de geheime instellingen buiten de bron-versiebeheermap naar gebruiker geheim store. Geheime archief van de gebruiker is een bestand opgeslagen onder de map gebruiker profiler, zodat de geheimen zijn niet ingecheckt met resourcebeheer. Het volgende diagram laat zien hoe [gebruiker geheim](https://docs.microsoft.com/aspnet/core/security/app-secrets?tabs=visual-studio#SecretManager) werkt.

![Geheim van de gebruiker blijft geheime instellingen buiten het besturingselement](./media/vs-secure-secret-appsettings/aspnetcore-usersecret.PNG)

Als u .NET core-consoletoepassing uitvoert, moet u Key Vault gebruiken voor het veilig opslaan van uw geheim.

### <a name="save-secret-settings-in-azure-key-vault"></a>Geheime instellingen opslaan in Azure Key Vault
Als u hebt voor het veilig delen van de broncode en het ontwikkelen van een project, gebruikt u [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

1. Maak een Key Vault in uw Azure-abonnement. Vul alle vereiste velden in de gebruikersinterface en op *maken* aan de onderkant van de blade

    ![Azure Key Vault maken](./media/vs-secure-secret-appsettings/create-keyvault.PNG)

2. Verlenen u en uw teamleden toegang tot de Key Vault. Als u een grote team hebt, kunt u een [Azure Active Directory-groep](https://docs.microsoft.com/azure/active-directory/active-directory-groups-create-azure-portal) en toevoegen van die groepstoegang tot de Key Vault. In de *geheime machtigingen* vervolgkeuzelijst selectievakje *ophalen* en *lijst* onder *geheim beheerbewerkingen*.

    ![Key Vault-toegangsbeleid toevoegen](./media/vs-secure-secret-appsettings/add-keyvault-access-policy.png)

3. Uw geheim toevoegen aan Key Vault in Azure portal. Voor geneste configuratie-instellingen, vervangt u ':' met '--', zodat de naam van de Key Vault-geheim ongeldig is. ':' is niet toegestaan om te worden van de naam van een Key Vault secret.

    ![Key Vault-geheim toevoegen](./media/vs-secure-secret-appsettings/add-keyvault-secret.png)

4. Installeer de [verificatie voor Services van Azure-extensie voor Visual Studio](https://go.microsoft.com/fwlink/?linkid=862354). Via deze extensie app toegang heeft tot Key Vault met behulp van de identiteit van de aanmelding bij Visual Studio.

5. Voeg de volgende NuGet-pakketten toe aan uw project:

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
7. De URL voor Key Vault aan launchsettings.json bestand toevoegen. Naam van de omgevingsvariabele *KEYVAULT_ENDPOINT* is gedefinieerd in de code die u in stap 6 hebt toegevoegd.

    ![URL voor Key Vault als een omgevingsvariabele project toevoegen](./media/vs-secure-secret-appsettings/add-keyvault-url.png)

8. Het project voor de foutopsporing starten. Het moet is uitgevoerd.

## <a name="aspnet-and-net-applications"></a>ASP.NET en .NET-toepassingen

.NET 4.7.1 ondersteunt builders van de configuratie van Key Vault en -geheim, die garandeert dat geheimen kunnen worden verplaatst buiten de bron-versiebeheermap zonder codewijzigingen.
Om door te gaan, [.NET 4.7.1 downloaden](https://www.microsoft.com/download/details.aspx?id=56115) en migreren van uw toepassing als een oudere versie van .NET framework wordt gebruikt.

### <a name="save-secret-settings-in-a-secret-file-that-is-outside-of-source-control-folder"></a>Geheime instellingen opslaan in een geheime bestand die buiten de bron-versiebeheermap
Als u een snelle prototype ontwikkelt en niet wilt dat voor het inrichten van Azure-resources, gaat u met deze optie.

1. Het volgende NuGet-pakket installeren voor uw project
    ```
    Microsoft.Configuration.ConfigurationBuilders.Basic
    ```

2. Maak een bestand dat vergelijkbaar is met de volgende. Sla het op een locatie buiten de projectmap.

    ```xml
    <root>
        <secrets ver="1.0">
            <secret name="secret1" value="foo_one" />
            <secret name="secret2" value="foo_two" />
        </secrets>
    </root>
    ```

3. Definieer het geheime bestand om te worden van een opbouwfunctie voor configuratie in het Web.config-bestand. In deze sectie voordat u put *appSettings* sectie.

    ```xml
    <configBuilders>
        <builders>
            <add name="Secrets"
                 secretsFile="C:\Users\AppData\MyWebApplication1\secret.xml" type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
                    Microsoft.Configuration.ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```

4. Geef sectie appSettings met behulp van de opbouwfunctie voor geheime configuratie. Zorg ervoor dat er een vermelding voor de geheime instelling met een dummy-waarde.

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

### <a name="save-secret-settings-in-an-azure-key-vault"></a>Geheime instellingen opslaan in een Azure Key Vault
Volg de instructies van ASP.NET core sectie om te configureren van een Key Vault voor uw project.

1. Het volgende NuGet-pakket installeren voor uw project
```
Microsoft.Configuration.ConfigurationBuilders.UserSecrets
```

2. Opbouwfunctie voor Key Vault-configuratie definiëren in Web.config. In deze sectie voordat u put *appSettings* sectie. Vervang *vaultName* moet de naam van de Key Vault als uw Key Vault in openbare Azure, of de volledige URI als u van onafhankelijke Clouds gebruikmaakt.

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>
    <configBuilders>
        <builders>
            <add name="AzureKeyVault" vaultName="Test911" type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, ConfigurationBuilders, Version=1.0.0.0, Culture=neutral" />
        </builders>
    </configBuilders>
    ```
3.  Geef de sectie appSettings met behulp van de opbouwfunctie voor Key Vault-configuratie. Zorg ervoor dat er een vermelding voor de geheime instelling met een dummy-waarde.

    ```xml
    <appSettings configBuilders="AzureKeyVault">
        <add key="webpages:Version" value="3.0.0.0" />
        <add key="webpages:Enabled" value="false" />
        <add key="ClientValidationEnabled" value="true" />
        <add key="UnobtrusiveJavaScriptEnabled" value="true" />
        <add key="secret" value="" />
    </appSettings>
    ```

4. Het project voor de foutopsporing starten. Het moet is uitgevoerd.
