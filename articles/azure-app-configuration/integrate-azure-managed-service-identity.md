---
title: Zelfstudie voor het integreren met Azure Managed Identities | Microsoft Docs
description: In deze zelfstudie leert u hoe u Azure Managed Identities gebruikt om te verifiëren met en toegang te krijgen tot Azure-appconfiguratie
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
ms.openlocfilehash: 874522b6b4ca3739e0736d4f70f76bb82cad25f9
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56957348"
---
# <a name="tutorial-integrate-with-azure-managed-identities"></a>Zelfstudie: Integreren met Azure Managed Identities

Azure Active Directory [Managed Identities](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) vereenvoudigt het beheer van geheimen voor uw cloudtoepassing. Met een beheerde identiteit kunt u uw code instellen met de service-principal die is gemaakt voor de Azure compute-service waarop deze wordt uitgevoerd, in plaats van een afzonderlijke referentie die is opgeslagen in Azure Key Vault of een lokale verbindingsreeks. Azure-appconfiguratie en de .NET Core, .NET en Java Spring-clientbibliotheken worden geleverd met ingebouwde MSI-ondersteuning. Hoewel u niet verplicht bent om deze te gebruiken, schakelt MSI de noodzaak voor een toegangstoken met geheimen uit. Uw code hoeft alleen het service-eindpunt voor een app-configuratiearchief te weten om er toegang toe te krijgen, en u kunt deze URL rechtstreeks insluiten in uw code zonder gevaar van het blootstellen van een geheim.

In deze zelfstudie leert hoe u kunt profiteren van MSI voor toegang tot App-configuratie. Dit is gebaseerd op de web-app die is geïntroduceerd in de quickstarts. Voltooi eerst [Een ASP.NET Core-app maken met App-configuratie](./quickstart-aspnet-core-app.md) voordat u verder gaat.

U kunt elke code-editor gebruiken om de stappen in deze quickstart uit te voeren. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie die beschikbaar is op de Windows-, macOS- en Linux-platforms.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een beheerde identiteit toegang verlenen tot App-configuratie
> * Uw app configureren voor het gebruik van een beheerde identiteit bij het verbinden met App-configuratie

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze zelfstudie te voltooien:

* [.NET Core-SDK](https://www.microsoft.com/net/download/windows)
* [Azure Cloud Shell geconfigureerd](https://docs.microsoft.com/azure/cloud-shell/quickstart)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="add-a-managed-identity"></a>Een beheerde identiteit toevoegen

Als u een beheerde identiteit in de portal instelt, moet u eerst een toepassing als normaal aanmaken en vervolgens de functie inschakelen.

1. Maak een app in de [Azure Portal](https://aka.ms/azconfig/portal) zoals u gewend bent. Navigeer ernaar in de portal.

2. Schuif omlaag naar de **Instellingen**-groep in het linkernavigatievenster en selecteer **Identiteit**.

3. Schakel binnen het tabblad **Systeem toegewezen** de **Status** naar **Aan** en klik op **Opslaan**.

    ![Stel beheerde identiteit in App Service in](./media/set-managed-identity-app-service.png)

## <a name="grant-access-to-app-configuration"></a>Verleen toegang tot de App-configuratie

1. Meld u aan bij de [Azure Portal](https://aka.ms/azconfig/portal), klik op **Alle resources** en het app-configuratiearchief dat u in de quickstart hebt gemaakt.

2. Klik op **Toegangsbeheer (IAM)**.

3. Binnen het tabblad **Toegang controleren** klikt u op **Toevoegen** in de **Een roltoewijzing toevoegen**-UI-kaart.

4. Stel **Rol** in als *Inzender* en **Toegang toewijzen aan** op *App Service* (onder *Systeemtoegewezen beheerde identiteit*).

5. Stel **Abonnement** in op uw Azure-abonnement en selecteer de resource App Service voor uw app.

6. Klik op **Opslaan**.

    ![Beheerde identiteit toevoegen](./media/add-managed-identity.png)

## <a name="use-a-managed-identity"></a>Een beheerde identiteit gebruiken

1. Open *appsettings.json*, voeg het volgende toe en vervang *<service_endpoint>* (inclusief punthaken) door de URL naar uw app-configuratiearchief:

    ```json
    "AppConfig": {
        "Endpoint": "<service_endpoint>"
    }
    ```

2. Open *Program.cs* en werk methode `CreateWebHostBuilder` bij door methode `config.AddAzureAppConfiguration()` toe te voegen.

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

De eenvoudigste manier om lokale Git-implementatie voor uw app met de Kudu-build-server in te schakelen, is met de Cloud Shell.

### <a name="configure-a-deployment-user"></a>Een implementatiegebruiker configureren

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Lokale Git met Kudu inschakelen

Als u lokale Git-implementatie voor uw app met de Kudu-build-server wilt inschakelen, voert u [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) in de Cloud Shell uit.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Voer voor het maken van een app met Git in plaats daarvan [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) in de Cloud Shell uit met de parameter `--deployment-local-git`.

```azurecli-interactive
az webapp create --name <app_name> --resource-group <group_name> --plan <plan_name> --deployment-local-git
```

De uitvoer na opdracht `az webapp create` moet er ongeveer uitzien zoals hieronder is weergegeven:

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

Push naar de externe Azure-instantie om uw app te implementeren met de volgende opdracht. Zorg er als u om een wachtwoord wordt gevraagd voor dat u het wachtwoord dat u hebt gemaakt bij [Een implementatiegebruiker configureren](#configure-a-deployment-user) gebruikt en niet het wachtwoord dat u gebruikt om u aan te melden bij Azure Portal.

```bash
git push azure master
```

U ziet mogelijk runtime-specifieke automatisering in de uitvoer, zoals MSBuild voor ASP.NET, `npm install` voor Node.js, en `pip install` voor Python.

### <a name="browse-to-the-azure-web-app"></a>Bladeren naar de Azure-web-app

Blader naar uw web-app met behulp van een browser om te controleren dat de inhoud wordt geïmplementeerd.

```bash
http://<app_name>.azurewebsites.net
```

![app die in App Service wordt uitgevoerd](../app-service/media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

## <a name="use-managed-identity-in-other-languages"></a>Beheerde identiteit gebruiken in andere talen

App-configuratie-providers voor .NET Framework en Java Spring hebben ook ingebouwde ondersteuning voor beheerde identiteit. In dergelijke gevallen gebruikt u gewoon het eindpunt van uw app-configuratiearchief in plaats van de volledige verbindingsreeks bij het configureren van een provider. Bijvoorbeeld, voor de .NET Framework-console-app die is gemaakt in de Quick Start geeft u de volgende instellingen op in het *App.config*-bestand:

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

In deze zelfstudie hebt u een beheerde Azure-service-identiteit toegevoegd om toegang tot App-configuratie te stroomlijnen en het beheer van referenties voor uw app te verbeteren. Voor meer informatie over het gebruik van App-configuratie, gaat u verder naar de Azure CLI-voorbeelden.

> [!div class="nextstepaction"]
> [CLI-voorbeelden](./cli-samples.md)
