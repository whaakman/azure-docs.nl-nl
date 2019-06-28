---
title: Maken en implementeren van Azure van Visual Studio-resourcegroepprojecten
description: Gebruik Visual Studio om een Azure-resourcegroepproject te maken en de resources in Azure te implementeren.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: quickstart
ms.date: 06/20/2019
ms.author: tomfitz
ms.openlocfilehash: 8677d906375853bdde5c192c86dacc7479f2e31e
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67311329"
---
# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Azure-resourcegroepen maken en implementeren met Visual Studio

Met Visual Studio kunt u een project maken waarmee uw infrastructuur en code in Azure worden geïmplementeerd. U kunt bijvoorbeeld de webhost, website en code voor de website implementeren. Visual Studio biedt veel verschillende startsjablonen om te implementeren in algemene scenario's. In dit artikel implementeert u een web-app.  

In dit artikel ziet u hoe u [Visual Studio 2019 of hoger met de Azure-ontwikkeling en de ASP.NET-workloads geïnstalleerd](/visualstudio/install/install-visual-studio?view=vs-2019). Als u Visual Studio 2017 gebruikt, uw ervaring is grotendeels hetzelfde.

## <a name="create-azure-resource-group-project"></a>Een Azure-resourcegroepproject maken

In deze sectie maakt u een Azure-resourcegroep-project met een **Web-app** sjabloon.

1. Kies in Visual Studio **bestand**, **nieuw**, en **Project**. Selecteer de **Azure-resourcegroep** projectsjabloon, maken en **volgende**.

    ![Project maken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Geef uw project een naam. De andere standaard instellingen waarschijnlijk in orde zijn, maar ze kunnen bekijken die ze werken voor uw omgeving. Wanneer u klaar bent, selecteert u **Maken**.

    ![Project maken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/name-project.png)

1. Kies het sjabloon dat u wilt implementeren in Azure Resource Manager. Er zijn diverse opties beschikbaar op basis van het type project dat u wilt implementeren. Kies voor dit artikel, de **Web-app** sjabloon en **OK**.

    ![Een sjabloon selecteren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Het sjabloon dat u kiest is slechts een beginpunt; u kunt resources toevoegen en verwijderen op basis van uw scenario.

1. Visual Studio maakt een project voor resourcegroepimplementatie voor de web-app. Als u wilt zien van de bestanden voor uw project, kijken naar het knooppunt in het implementatieproject.

    ![knooppunten weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Omdat u de Web-appsjabloon is gekozen, ziet u de volgende bestanden:

   | Bestandsnaam | Description |
   | --- | --- |
   | Deploy-AzureResourceGroup.ps1 |Een PowerShell-script waarmee PowerShell-opdrachten worden uitgevoerd om te worden geïmplementeerd in Azure Resource Manager. Visual Studio gebruikt dit PowerShell-script om uw sjabloon te implementeren. |
   | WebSite.json |Het Resource Manager-sjabloon dat de infrastructuur definieert die u in Azure wilt implementeren en de parameters die u kunt opgeven tijdens de implementatie. Hiermee worden ook de afhankelijkheden tussen resources gedefinieerd zodat deze in de juiste volgorde worden geïmplementeerd. |
   | WebSite.parameters.json |Een parameterbestand dat de waarden bevat die nodig zijn voor de sjabloon. U geeft parameterwaarden door om elke implementatie aan te passen. |

    Alle implementatieprojecten voor resourcegroepen bevatten deze algemene bestanden. Andere projecten bevatten mogelijk extra bestanden ter ondersteuning van andere functies.

## <a name="customize-resource-manager-template"></a>Resource Manager-sjabloon aanpassen

U kunt een implementatieproject aanpassen door het wijzigen van de Resource Manager-sjabloon die worden beschreven van de resources die u wilt implementeren. Zie [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md) voor meer informatie over de onderdelen van een Resource Manager-sjabloon.

1. Als u wilt werken met de sjabloon, open **WebSite.json**.

1. De Visual Studio-editor biedt hulpprogramma's voor het bewerken van het Resource Manager-sjabloon. In het scherm **JSON-overzicht** ziet u eenvoudig welke elementen zijn gedefinieerd in het sjabloon.

   ![JSON-overzicht weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

1. Selecteer een element in het overzicht te gaan naar dat deel van de sjabloon.

   ![door JSON navigeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

1. U kunt een nieuwe resource toevoegen aan een sjabloon door op de knop **Resource toevoegen** te klikken boven aan het venster JSON Outline of door met de rechtermuisknop op **Resources** te klikken en **Nieuwe resource toevoegen** te selecteren.

   ![Resource toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

1. Selecteer **Opslagaccount** en wijs hieraan een naam. Geef een naam op die niet meer dan elf tekens (alleen cijfers en kleine letters) omvat.

   ![Opslag toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

1. U ziet dat niet alleen de resource is toegevoegd, maar ook een parameter voor het type opslagaccount en een variabele voor de naam van het opslagaccount.

   ![overzicht weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

1. De parameter voor het type opslagaccount is vooraf gedefinieerd met de toegestane typen en een standaardtype. U kunt deze waarden laten staan of ze bewerken voor uw scenario. Als u niemand toestemming wilt geven om een **Premium_LRS**-opslagaccount te implementeren via deze sjabloon, verwijdert u de sjabloon uit de toegestane typen.

   ```json
   "demoaccountType": {
     "type": "string",
     "defaultValue": "Standard_LRS",
     "allowedValues": [
       "Standard_LRS",
       "Standard_ZRS",
       "Standard_GRS",
       "Standard_RAGRS"
     ]
   }
   ```

1. Visual Studio biedt ook IntelliSense om u inzicht te geven in welke eigenschappen beschikbaar zijn bij het bewerken van de sjabloon. Als u bijvoorbeeld de eigenschappen bewerkt van uw App Service-plan, navigeert u naar de resource **HostingPlan** en voegt u een nieuwe waarde toe voor de **eigenschappen**. IntelliSense geeft de beschikbare waarden weer en biedt een beschrijving van die waarden.

   ![intellisense weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

   U kunt instellen **numberOfWorkers** op 1, en sla het bestand.

   ```json
   "properties": {
     "name": "[parameters('hostingPlanName')]",
     "numberOfWorkers": 1
   }
   ```

1. Open de **WebSite.parameters.json** bestand. De parameterbestand wordt gebruikt om door te geven tijdens de implementatie waarden aanpassen van de resource wordt geïmplementeerd. Geef een naam op voor het hostingabonnement en sla het bestand.

   ```json
   {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "hostingPlanName": {
         "value": "demoHostPlan"
       }
     }
   }
   ```

## <a name="deploy-project-to-azure"></a>Project implementeren in Azure

U kunt nu uw project implementeren in een resourcegroep.

Het PowerShell-script (implementeren-AzureResourceGroup.ps1) in het project maakt standaard gebruik van de AzureRM-module. Als u nog steeds de AzureRM-module geïnstalleerd hebt en wilt blijven gebruiken, kunt u dit Standaardscript gebruiken. Met dit script, kunt u de Visual Studio-interface gebruiken om uw oplossing te implementeren.

Echter, als u hebt gemigreerd naar de nieuwe [Az module](/powershell/azure/new-azureps-module-az), moet u een nieuw script toevoegen aan uw project. U voegt een script dat gebruikmaakt van de Az-module, Kopieer de [implementeren AzTemplate.ps1](https://github.com/Azure/azure-quickstart-templates/blob/master/Deploy-AzTemplate.ps1) script en voeg deze toe aan uw project. Als u wilt gebruiken met dit script voor implementatie, moet u het uitvoeren van een PowerShell-console, in plaats van met behulp van Visual Studio-implementatie-interface.

Beide methoden worden weergegeven in dit artikel. In dit artikel verwijst naar de Standaardscript als de AzureRM-module-script en het nieuwe script op als het script Az-module.

### <a name="az-module-script"></a>AZ module-script

Open een PowerShell-console voor het script van de module Az en uitvoeren:

```powershell
.\Deploy-AzTemplate.ps1 -ArtifactStagingDirectory . -Location centralus -TemplateFile WebSite.json -TemplateParametersFile WebSite.parameters.json
```

### <a name="azurerm-module-script"></a>Script voor AzureRM-module

Gebruik Visual Studio voor het script AzureRM-module:

1. Kies **Implementeren** > **Nieuw** in het snelmenu van het knooppunt van het implementatieproject.

    ![Nieuwe implementatie menu-item](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

1. Het dialoogvenster **Implementeren in resourcegroep** wordt weergegeven. In de vervolgkeuzelijst **Resourcegroep** kiest u een bestaande resourcegroep of maakt u een nieuwe. Selecteer **Implementeren**.

    ![Implementeren in het dialoogvenster van de resource-groep](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. U kunt de voortgang van de implementatie bekijken in het venster **Uitvoer**. Wanneer de implementatie is voltooid, ziet u een laatste bericht dat de implementatie is geslaagd en dat er ongeveer als volgt uitziet:

   ```output
   18:00:58 - Successfully deployed template 'website.json' to resource group 'ExampleAppDeploy'.
   ```

## <a name="view-deployed-resources"></a>Geïmplementeerde resources weergeven

Laten we de resultaten controleren.

1. Open in een browser de [Azure Portal](https://portal.azure.com/) en meld u aan bij uw account. Als u de resourcegroep wilt bekijken, selecteert u **Resourcegroepen** en vervolgens de resourcegroep waarin u hebt geïmplementeerd.

1. U ziet alle geïmplementeerde resources. De naam van het opslagaccount komt niet precies overeen met wat u hebt opgegeven bij het toevoegen van die resource. Het opslagaccount moet uniek zijn. De sjabloon voegt automatisch een reeks tekens aan de naam die u hebt opgegeven voor het maken van een unieke naam.

    ![resources weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

## <a name="add-code-to-project"></a>Voeg code toe aan het project

U hebt nu de infrastructuur geïmplementeerd voor uw app, maar er is nog geen code geïmplementeerd in uw project.

1. Voeg een project toe aan uw Visual Studio-oplossing. Klik met de rechtermuisknop op de oplossing en selecteer **Toevoegen** > **Nieuw Project**.

    ![project toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. Voeg een **ASP.NET Core-webtoepassing**.

    ![web-app toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)

1. Geef een naam op voor uw web-app en selecteer **maken**.

    ![De naam van web-app](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/name-web-app.png)

1. Selecteer **webtoepassing** en **maken**.

    ![Selecteer de Web-App](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project-type.png)

1. Nadat Visual Studio uw web-app heeft gemaakt, ziet u beide projecten in de oplossing.

    ![projecten weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Nu moet u nagaan of het nieuwe project gekoppeld is aan uw resourcegroepproject. Ga terug naar uw resourcegroepproject (ExampleAppDeploy). Klik met de rechtermuisknop op **Verwijzingen** en selecteer **Verwijzing toevoegen**.

    ![Verwijzing toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Selecteer het web-app-project dat u hebt gemaakt.

   ![Verwijzing toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)

   Door een verwijzing toe te voegen kunt u de web-app-project koppelen aan het resourcegroepproject en sommige eigenschappen automatisch ingesteld. U ziet deze eigenschappen in het venster **Eigenschappen** voor de verwijzing. **Bestandspad toevoegen** bevat het pad waar het pakket wordt gemaakt. Noteer de map (ExampleApp) en het bestand (package.zip). U moet weten wat deze waarden zijn, omdat u ze als parameters moet opgeven tijdens het implementeren van de app.

   ![Zie de naslaginformatie](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)

1. Ga terug naar uw sjabloon (WebSite.json) en een resource toevoegen aan de sjabloon.

    ![Resource toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Selecteer deze keer **Webimplementatie voor Web Apps**. 

    ![toevoegen van web implementeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)

   Sla uw sjabloon.

1. Er zijn enkele nieuwe parameters in uw sjabloon. Ze zijn toegevoegd in de vorige stap. U hoeft niet te waarden opgeven voor **_artifactsLocation** of **_artifactsLocationSasToken** omdat deze waarden worden automatisch gegenereerd. U moet echter de map en bestandsnaam ingesteld op het pad dat het implementatiepakket bevat. De namen van deze parameters eindigen met **PackageFolder** en **PackageFileName**. Het eerste deel van de naam is de naam van de Web Deploy resource die u hebt toegevoegd. In dit artikel, ze heten **ExampleAppPackageFolder** en **ExampleAppPackageFileName**. 

   Open **Website.parameters.json** en stelt u deze parameters op de waarden die u in de verwijzingseigenschappen hebt gezien. Stel **ExampleAppPackageFolder** op de naam van de map. Stel **ExampleAppPackageFileName** op de naam van het zip-bestand.

   ```json
   {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "hostingPlanName": {
         "value": "demoHostPlan"
       },
       "ExampleAppPackageFolder": {
         "value": "ExampleApp"
       },
       "ExampleAppPackageFileName": {
         "value": "package.zip"
       }
     }
   }
   ```

## <a name="deploy-code-with-infrastructure"></a>Code implementeren via de infrastructuur

Omdat u code toegevoegd aan het project, is uw implementatie enigszins anders deze tijd. Tijdens de implementatie, kunt u artefacten voorbereiden voor uw project op een plaats die toegang hebben tot Resource Manager. De artefacten, gefaseerd binnengebracht naar een opslagaccount.

### <a name="az-module-script"></a>AZ module-script

Er is een kleine wijziging die u aanbrengen in uw sjabloon moet als u het script Az-module. Met dit script voegt een schuine streep naar de locatie van de artefacten, maar de sjabloon wordt niet verwacht dat schuine streep. Open WebSite.json en de eigenschappen vinden voor de MSDeploy-extensie. Er is een eigenschap met de naam **packageUri**. Verwijder de slash tussen de locatie van de artefacten en de pakketmap.

Er moet uitzien:

```json
"packageUri": "[concat(parameters('_artifactsLocation'), parameters('ExampleAppPackageFolder'), '/', parameters('ExampleAppPackageFileName'), parameters('_artifactsLocationSasToken'))]",
```

U ziet in het voorgaande voorbeeld er is geen `'/',` tussen **parameters('_artifactsLocation')** en **parameters('ExampleAppPackageFolder')** .

Bouw het project opnieuw. Het bouwen van het project kunt u ervoor dat de bestanden die u wilt implementeren naar de map met tijdelijke bestanden worden toegevoegd.

Nu, open een PowerShell-console en voer:

```powershell
.\Deploy-AzTemplate.ps1 -ArtifactStagingDirectory .\bin\Debug\staging\ExampleAppDeploy -Location centralus -TemplateFile WebSite.json -TemplateParametersFile WebSite.parameters.json -UploadArtifacts -StorageAccountName <storage-account-name>
```

### <a name="azurerm-module-script"></a>Script voor AzureRM-module

Gebruik Visual Studio voor het script AzureRM-module:

1. Als u wilt implementeren, kies **implementeren**, en de resourcegroep die u eerder hebt geïmplementeerd.

    ![Project opnieuw implementeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

1. Selecteer het opslagaccount dat u hebt geïmplementeerd met deze resourcegroep voor de **opslagaccount voor artefact**.

   ![Opnieuw implementeren van web implementeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy-web-app.png)

## <a name="view-web-app"></a>Web-app weergeven

1. Nadat de implementatie is voltooid, selecteert u uw web-app in de portal. Selecteer de URL om naar de site te bladeren.

   ![site bladeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. De implementatie van de standaard ASP.NET-app is voltooid.

   ![geïmplementeerde app weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="add-operations-dashboard"></a>Bewerkingsdashboard toevoegen

U bent niet beperkt tot alleen de resources die beschikbaar zijn via de Visual Studio-interface. U kunt uw implementatie aanpassen door een aangepaste resource toe te voegen aan uw sjabloon. Als u wilt zien hoe het toevoegen van een resource in zijn werk gaat, voegt u een operationeel dashboard toe voor het beheren van de resource die u hebt geïmplementeerd.

1. Open het bestand WebSite.json en voeg de volgende JSON toe na de opslagaccountresource maar voor de afsluitende `]` van de sectie met resources.

   ```json
    ,{
      "properties": {
        "lenses": {
          "0": {
            "order": 0,
            "parts": {
              "0": {
                "position": {
                  "x": 0,
                  "y": 0,
                  "colSpan": 4,
                  "rowSpan": 6
                },
                "metadata": {
                  "inputs": [
                    {
                      "name": "resourceGroup",
                      "isOptional": true
                    },
                    {
                      "name": "id",
                      "value": "[resourceGroup().id]",
                      "isOptional": true
                    }
                  ],
                  "type": "Extension/HubsExtension/PartType/ResourceGroupMapPinnedPart"
                }
              },
              "1": {
                "position": {
                  "x": 4,
                  "y": 0,
                  "rowSpan": 3,
                  "colSpan": 4
                },
                "metadata": {
                  "inputs": [],
                  "type": "Extension[azure]/HubsExtension/PartType/MarkdownPart",
                  "settings": {
                    "content": {
                      "settings": {
                        "content": "__Customizations__\n\nUse this dashboard to create and share the operational views of services critical to the application performing. To customize simply pin components to the dashboard and then publish when you're done. Others will see your changes when you publish and share the dashboard.\n\nYou can customize this text too. It supports plain text, __Markdown__, and even limited HTML like images <img width='10' src='https://portal.azure.com/favicon.ico'/> and <a href='https://azure.microsoft.com' target='_blank'>links</a> that open in a new tab.\n",
                        "title": "Operations",
                        "subtitle": "[resourceGroup().name]"
                      }
                    }
                  }
                }
              }
            }
          }
        },
        "metadata": {
          "model": {
            "timeRange": {
              "value": {
                "relative": {
                  "duration": 24,
                  "timeUnit": 1
                }
              },
              "type": "MsPortalFx.Composition.Configuration.ValueTypes.TimeRange"
            }
          }
        }
      },
      "apiVersion": "2015-08-01-preview",
      "name": "[concat('ARM-',resourceGroup().name)]",
      "type": "Microsoft.Portal/dashboards",
      "location": "[resourceGroup().location]",
      "tags": {
        "hidden-title": "[concat('OPS-',resourceGroup().name)]"
      }
    }
   ```

1. Opnieuw implementeren uw project.

1. Nadat de implementatie is voltooid, kunt u uw dashboard bekijken in de portal. Selecteer **Dashboard** en kies de versie die u hebt geïmplementeerd.

   ![Aangepast dashboard](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/view-custom-dashboards.png)

1. U ziet het aangepaste dashboard.

   ![Aangepast dashboard](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/Ops-DemoSiteGroup-dashboard.png)

U kunt de toegang tot het dashboard beheren met behulp van RBAC-groepen. U kunt ook het uiterlijk van het dashboard aanpassen zodra de implementatie is voltooid. Als u de resourcegroep echter opnieuw implementeert, wordt het dashboard opnieuw ingesteld op de standaardstatus in de sjabloon. Zie [Op programmatische wijze Azure-dashboards maken](../azure-portal/azure-portal-dashboards-create-programmatically.md) voor meer informatie over het maken van dashboards.

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer in de Azure-portal **resourcegroepen** in het menu links.

1. Selecteer de naam van de resourcegroep.

1. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u sjablonen kunt maken en implementeren met Visual Studio. In de volgende zelfstudie leert u hoe u de informatie van de sjabloonverwijzing kunt vinden, zodat u een versleuteld Azure-opslagaccount kunt maken.

> [!div class="nextstepaction"]
> [Een versleuteld opslagaccount maken](./resource-manager-tutorial-create-encrypted-storage-accounts.md)
