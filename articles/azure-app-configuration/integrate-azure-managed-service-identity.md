---
title: Zelfstudie voor het integreren met Azure beheerde identiteiten | Microsoft Docs
description: In deze zelfstudie leert u hoe u het gebruik van Azure beheerde identiteiten te verifiëren met toegang tot Azure App-configuratie
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: na
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: be19d37900acb8201922fa61fda61cc884d4c933
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226008"
---
# <a name="tutorial-integrate-with-azure-managed-identities"></a>Zelfstudie: Integreren met Azure beheerde identiteiten

Azure Active Directory [beheerde identiteiten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) vereenvoudigt het beheer van geheimen voor uw cloudtoepassing. Met een beheerde identiteit, kunt u uw code instellen met de service-principal die is gemaakt voor de Azure compute-service waarop het wordt uitgevoerd. U kunt een beheerde identiteit gebruiken in plaats van een afzonderlijke referenties die zijn opgeslagen in Azure Key Vault of een lokale verbindingsreeks. 

Azure App-configuratie en de .NET Core, .NET en Java Spring-clientbibliotheken worden geleverd met ondersteuning voor beheerde service-identiteiten (MSI) die is ingebouwd in deze. Hoewel u niet verplicht om deze te gebruiken, hoeft MSI u voor een toegangstoken met geheimen. Uw code heeft alleen het service-eindpunt voor een appconfiguratie opslaan voor toegang tot het kennen. U kunt deze URL insluiten in uw code rechtstreeks zonder de gevolgen van het blootstellen van een geheim.

In deze zelfstudie leert hoe u kunt profiteren van MSI voor toegang tot App-configuratie. Dit is gebaseerd op de web-app die is geïntroduceerd in de quickstarts. Voordat u doorgaat, voltooien [maken van een ASP.NET Core-app met App-configuratie](./quickstart-aspnet-core-app.md) eerste.

Een code-editor kunt u de stappen in deze zelfstudie doet. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie beschikbaar is op Windows, macOS en Linux-platforms.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een beheerde identiteit toegang verlenen aan App-configuratie.
> * Uw app configureren voor een beheerde identiteit gebruiken wanneer u verbinding met App-configuratie maken.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze zelfstudie te voltooien:

* [.NET Core SDK](https://www.microsoft.com/net/download/windows).
* [Azure Cloud Shell geconfigureerd](https://docs.microsoft.com/azure/cloud-shell/quickstart).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="add-a-managed-identity"></a>Een beheerde identiteit toevoegen

Als u een beheerde identiteit in de portal instelt, moet u eerst maken van een toepassing als normale en schakel vervolgens de functie.

1. Maak een app in de [Azure-portal](https://aka.ms/azconfig/portal) zoals u gewend bent. Ga naar het in de portal.

2. Schuif omlaag naar de **instellingen** groep in het linkerdeelvenster en selecteer **identiteit**.

3. Op de **systeem toegewezen** tabblad **Status** naar **op** en selecteer **opslaan**.

    ![Stel beheerde identiteit in App Service in](./media/set-managed-identity-app-service.png)

## <a name="grant-access-to-app-configuration"></a>Verleen toegang tot de App-configuratie

1. In de [Azure-portal](https://aka.ms/azconfig/portal), selecteer **alle resources** en selecteert u de app-configuratiearchief, dat u in de Quick Start hebt gemaakt.

2. Klik op **Toegangsbeheer (IAM)**.

3. Op de **toegang controleren** tabblad **toevoegen** in de **roltoewijzing toevoegen** UI-kaart.

4. Onder **rol**, selecteer **Inzender**. Onder **toegang toewijzen aan**, selecteer **App Service** onder **door het systeem toegewezen beheerde identiteit**.

5. Onder **abonnement**, selecteer uw Azure-abonnement. Selecteer de resource App Service voor uw app.

6. Selecteer **Opslaan**.

    ![Een beheerde identiteit toevoegen](./media/add-managed-identity.png)

## <a name="use-a-managed-identity"></a>Een beheerde identiteit gebruiken

1. Open *appsettings.json*, en voeg het volgende script toe. Vervang *< service_endpoint >*, met inbegrip van de vierkante haken, de URL naar het opslaan van de app-configuratie:

    ```json
    "AppConfig": {
        "Endpoint": "<service_endpoint>"
    }
    ```

2. Open *Program.cs*, en werk de `CreateWebHostBuilder` methode door het vervangen van de `config.AddAzureAppConfiguration()` methode.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(o => o.ConnectWithManagedIdentity(settings["AppConfig:Endpoint"]));
            })
            .UseStartup<Startup>();
    ```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-from-local-git"></a>Implementeren vanuit lokale Git

De eenvoudigste manier om in te schakelen van lokale Git-implementatie voor uw app met de Kudu-build-server is met Azure Cloud Shell.

### <a name="configure-a-deployment-user"></a>Een implementatiegebruiker configureren

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Lokale Git met Kudu inschakelen

Als u lokale Git-implementatie voor uw app met de Kudu-build-server, voert u [ `az webapp deployment source config-local-git` ](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) in Cloud Shell.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Voer voor het maken een app met Git in plaats daarvan [ `az webapp create` ](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) in Cloud Shell met de `--deployment-local-git` parameter.

```azurecli-interactive
az webapp create --name <app_name> --resource-group <group_name> --plan <plan_name> --deployment-local-git
```

De `az webapp create` opdracht biedt u iets die vergelijkbaar is met de volgende uitvoer:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="deploy-your-project"></a>Uw project implementeren

Voeg, eenmaal terug in het _lokale terminalvenster_, een externe Azure-instantie toe aan uw lokale Git-opslagplaats. Vervang _\<url >_ met de URL van de externe Git-instantie die u hebt verkregen via [Git inschakelen voor uw app](#enable-local-git-with-kudu).

```bash
git remote add azure <url>
```

Push naar de externe Azure-instantie om uw app te implementeren met de volgende opdracht. Wanneer u wordt gevraagd om een wachtwoord, typt u het wachtwoord die u hebt gemaakt in [een implementatiegebruiker configureren](#configure-a-deployment-user). Gebruik niet het wachtwoord dat u zich aanmeldt bij Azure portal gebruikt.

```bash
git push azure master
```

Runtime-specifieke automatisering in de uitvoer, zoals MSBuild voor ASP.NET, ziet u mogelijk `npm install` voor Node.js, en `pip install` voor Python.

### <a name="browse-to-the-azure-web-app"></a>Bladeren naar de Azure-web-app

Blader naar uw web-app met behulp van een browser om te controleren dat de inhoud wordt geïmplementeerd.

```bash
http://<app_name>.azurewebsites.net
```

![App die wordt uitgevoerd in App Service](../app-service/media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

## <a name="use-managed-identity-in-other-languages"></a>Beheerde identiteit gebruiken in andere talen

App-configuratie-providers voor .NET Framework en Java Spring hebben ook ingebouwde ondersteuning voor beheerde identiteit. In dergelijke gevallen gebruikt u uw app opslaan van de configuratie van URL-eindpunt in plaats van de volledige verbindingsreeks bij het configureren van een provider. Geef bijvoorbeeld de volgende instellingen in voor de .NET Framework-consoletoepassing in de Quick Start hebt gemaakt, de *App.config* bestand:

```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" endpoint="${Endpoint}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="Endpoint" value ="Set via an environment variable - for example, dev, test, staging, or production endpoint." />
    </appSettings>
```

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een beheerde Azure-service-identiteit toegevoegd om toegang tot app-configuratie te stroomlijnen en het beheer van referenties voor uw app te verbeteren. Voor meer informatie over het gebruik van App-configuratie, gaat u naar de Azure CLI-voorbeelden.

> [!div class="nextstepaction"]
> [CLI-voorbeelden](./cli-samples.md)
