---
title: Visual Studio Azure-resourcegroepprojecten | Microsoft Docs
description: Gebruik Visual Studio om een Azure-resourcegroepproject te maken en de resources in Azure te implementeren.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: multiple
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/02/2018
ms.author: tomfitz
ms.openlocfilehash: 48d23f9d6d79e89e24c808a59b73854952c93240
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54425438"
---
# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Azure-resourcegroepen maken en implementeren met Visual Studio
Met Visual Studio kunt u een project maken waarmee uw infrastructuur en code in Azure worden geïmplementeerd. U kunt bijvoorbeeld de webhost, website en database voor uw app opgeven en die infrastructuur samen met de code implementeren. Visual Studio biedt veel verschillende startsjablonen om te implementeren in algemene scenario's. In dit artikel gaat u een web-app en SQL Database implementeren.  

In dit artikel leest u hoe u [Visual Studio 2017 gebruikt met de workloads voor ontwikkeling en ASP.NET van Azure geïnstalleerd](/dotnet/azure/dotnet-tools). Als u Visual Studio 2015 Update 2 en Microsoft Azure SDK voor .NET 2.9 gebruikt, of Visual Studio 2013 met Azure SDK 2.9, is de ervaring in grote lijnen hetzelfde.

## <a name="create-azure-resource-group-project"></a>Een Azure-resourcegroepproject maken
In dit gedeelte maakt u een Azure Resource Group-project met de sjabloon **Web app + SQL**.

1. Kies in Visual Studio **File** en vervolgens **New Project**. Kies **C#** of **Visual Basic** (welke taal u kiest heeft geen invloed op de latere fasen, omdat deze projecten alleen JSON- en PowerShell-inhoud bevatten). Kies vervolgens **Cloud** en het project **Azure-resourcegroep**.
   
    ![Project voor cloudimplementatie](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)
2. Kies het sjabloon dat u wilt implementeren in Azure Resource Manager. Er zijn diverse opties beschikbaar op basis van het type project dat u wilt implementeren. Voor dit artikel kiest u de sjabloon **Web-app + SQL**.
   
    ![Een sjabloon selecteren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)
   
    Het sjabloon dat u kiest is slechts een beginpunt; u kunt resources toevoegen en verwijderen op basis van uw scenario.
   
   > [!NOTE]
   > Visual Studio haalt een lijst met beschikbare sjablonen online op. De lijst kan worden gewijzigd.
   > 
   > 
   
    Visual Studio maakt een project voor resourcegroepimplementatie voor de webtoepassing en SQL Database.
3. Kijk naar het knooppunt in het implementatieproject om te zien wat u hebt gemaakt.
   
    ![knooppunten weergegeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)
   
    Omdat er voor dit voorbeeld is gekozen voor de sjabloon Web-app + SQL, krijgt u de volgende bestanden te zien: 
   
   | Bestandsnaam | Beschrijving |
   | --- | --- |
   | Deploy-AzureResourceGroup.ps1 |Een PowerShell-script waarmee PowerShell-opdrachten worden uitgevoerd om te worden geïmplementeerd in Azure Resource Manager.<br />**Opmerking** Visual Studio gebruikt dit PowerShell-script voor het implementeren van uw sjabloon. Eventuele wijzigingen die u aanbrengt in dit script beïnvloeden de implementatie in Visual Studio, dus wees voorzichtig. |
   | WebSiteSQLDatabase.json |Het Resource Manager-sjabloon dat de infrastructuur definieert die u in Azure wilt implementeren en de parameters die u kunt opgeven tijdens de implementatie. Hiermee worden ook de afhankelijkheden tussen resources gedefinieerd zodat deze in de juiste volgorde worden geïmplementeerd. |
   | WebSiteSQLDatabase.parameters.json |Een parameterbestand dat de waarden bevat die nodig zijn voor de sjabloon. U geeft parameterwaarden door om elke implementatie aan te passen. |
   
    Alle implementatieprojecten voor resourcegroepen bevatten deze algemene bestanden. Andere projecten bevatten mogelijk extra bestanden ter ondersteuning van andere functies.

## <a name="customize-the-resource-manager-template"></a>Het Resource Manager-sjabloon aanpassen
U kunt een implementatieproject aanpassen door de JSON-sjablonen te wijzigen waarin wordt beschreven welke resources u wilt implementeren. JSON staat voor JavaScript Object Notation en is een geserialiseerde gegevensindeling waarmee eenvoudig te werken is. Voor JSON-bestanden wordt een schema gebruikt waarnaar u aan de bovenkant van elk bestand naar verwijst. U kunt het schema downloaden en het analyseren als u er meer inzicht in wilt krijgen. In het schema wordt gedefinieerd welke elementen zijn toegestaan, welke soorten en indelingen voor velden er zijn, en wat de mogelijke waarden voor een eigenschap zijn. Zie [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md) voor meer informatie over de onderdelen van een Resource Manager-sjabloon.

Als u aan dit sjabloon wilt werken, opent u **WebSiteSQLDatabase.json**.

De Visual Studio-editor biedt hulpprogramma's voor het bewerken van het Resource Manager-sjabloon. In het scherm **JSON-overzicht** ziet u eenvoudig welke elementen zijn gedefinieerd in het sjabloon.

![JSON-overzicht weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Wanneer u een element in het overzicht selecteert, gaat u naar dat deel van het sjabloon en wordt de bijbehorende JSON gemarkeerd.

![door JSON navigeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

U kunt een nieuwe resource toevoegen aan een sjabloon door op de knop **Resource toevoegen** te klikken boven aan het venster JSON Outline of door met de rechtermuisknop op **Resources** te klikken en **Nieuwe resource toevoegen** te selecteren.

![resource toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Voor deze zelfstudie selecteert u **Opslagaccount** en geeft u het een naam. Geef een naam op die niet meer dan elf tekens (alleen cijfers en kleine letters) omvat.

![opslag toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

U ziet dat niet alleen de resource is toegevoegd, maar ook een parameter voor het type opslagaccount en een variabele voor de naam van het opslagaccount.

![overzicht weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

De parameter **storageType** wordt vooraf gedefinieerd met de toegestane typen en een standaardtype. U kunt deze waarden laten staan of ze bewerken voor uw scenario. Als u niemand toestemming wilt geven om een **Premium_LRS**-opslagaccount te implementeren via deze sjabloon, verwijdert u de sjabloon uit de toegestane typen. 

```json
"storageType": {
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

Visual Studio biedt ook IntelliSense om u inzicht te geven in welke eigenschappen beschikbaar zijn bij het bewerken van het sjabloon. Als u bijvoorbeeld de eigenschappen bewerkt van uw App Service-plan, navigeert u naar de resource **HostingPlan** en voegt u een nieuwe waarde toe voor de **eigenschappen**. IntelliSense geeft de beschikbare waarden weer en biedt een beschrijving van die waarden.

![IntelliSense weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

U kunt **numberOfWorkers** instellen op 1.

```json
"properties": {
  "name": "[parameters('hostingPlanName')]",
  "numberOfWorkers": 1
}
```

## <a name="deploy-the-resource-group-project-to-azure"></a>Het resourcegroepproject implementeren in Azure
U bent nu klaar om uw project te implementeren. Wanneer u een Azure-resourcegroepsproject implementeert, implementeert u het in een Azure-resourcegroep. De resourcegroep is een logische groepering van resources die een gemeenschappelijke lifecycle delen.

1. Kies **Implementeren** > **Nieuw** in het snelmenu van het knooppunt van het implementatieproject.
   
    ![Het menu-item Implementeren, Nieuwe implementatie](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)
   
    Het dialoogvenster **Implementeren in resourcegroep** wordt weergegeven.
   
    ![Dialoogvenster Implementeren in resourcegroep](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)
2. In de vervolgkeuzelijst **Resourcegroep** kiest u een bestaande resourcegroep of maakt u een nieuwe. Als u een nieuwe resourcegroep wilt maken, opent u de vervolgkeuzelijst **Resourcegroep** en selecteert u **Nieuwe maken**.
   
    ![Dialoogvenster Implementeren in resourcegroep](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)
   
    Het dialoogvenster **Resourcegroep maken** wordt weergegeven. Geef uw groep een naam en locatie en selecteer de knop **Maken**.
   
    ![Dialoogvenster Resourcegroep maken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
3. U kunt de parameters voor de implementatie bewerken door de knop **Parameters bewerken** te selecteren.
   
    ![Knop Parameters bewerken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)
4. Geef waarden op voor de lege parameters en selecteer **Opslaan**. De lege parameters zijn **hostingPlanName**, **administratorLogin**, **administratorLoginPassword** en **databaseName**.
   
    Met **hostingPlanName** geeft u de naam op voor het te maken [App Service-plan](../app-service/overview-hosting-plans.md). 
   
    Met **administratorLogin** geeft u de gebruikersnaam op voor de SQL Server-beheerder. Gebruik geen veelvoorkomende beheerdersnamen als **sa** of **admin**. 
   
    Met **administratorLoginPassword** geeft u een wachtwoord op de SQL Server-beheerder. De optie **Wachtwoorden opslaan als tekst zonder opmaak in het parameterbestand** is niet beveiligd. Selecteer deze optie daarom niet. Omdat het wachtwoord niet als tekst zonder opmaak wordt opgeslagen, moet u dit wachtwoord opnieuw opgeven tijdens de implementatie. 
   
    Met **databaseName** geeft u een naam op voor de te maken database. 
   
    ![Dialoogvenster Parameters bewerken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
5. Klik op **Implementeren** om het project in Azure te implementeren. Een PowerShell-console wordt geopend buiten het Visual Studio-exemplaar. Voer het beheerderswachtwoord voor de SQL Server in de PowerShell-console in wanneer om het wachtwoord wordt gevraagd. **De PowerShell-console kan zijn verborgen achter andere items of geminimaliseerd op de taakbalk.** Zoek en selecteer deze console om het wachtwoord op te geven.
   
   > [!NOTE]
   > Visual Studio vraagt u mogelijk om de Azure PowerShell-cmdlets te installeren. U hebt de Azure PowerShell-cmdlets nodig om resourcegroepen te kunnen implementeren. Installeer ze als dit gevraagd wordt. Zie [Azure PowerShell installeren en configureren](/powershell/azure/azurerm/install-azurerm-ps) voor meer informatie.
   > 
   > 
6. De implementatie kan enkele minuten in beslag nemen. U kunt de voortgang van de implementatie bekijken in het venster **Uitvoer**. Wanneer de implementatie is voltooid, ziet u een laatste bericht dat de implementatie is geslaagd en dat er ongeveer als volgt uitziet:
   
        ... 
        18:00:58 - Successfully deployed template 'websitesqldatabase.json' to resource group 'DemoSiteGroup'.
7. Open in een browser de [Azure Portal](https://portal.azure.com/) en meld u aan bij uw account. Als u de resourcegroep wilt bekijken, selecteert u **Resourcegroepen** en vervolgens de resourcegroep waarin u hebt geïmplementeerd.
   
    ![groep selecteren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)
8. U ziet alle geïmplementeerde resources. De naam van het opslagaccount komt niet precies overeen met wat u hebt opgegeven bij het toevoegen van die resource. Het opslagaccount moet uniek zijn. De sjabloon voegt automatisch een tekenreeks toe aan de naam die u hebt opgegeven zodat dit een unieke naam wordt. 
   
    ![resources weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)
9. Als u wijzigingen aanbrengt en u het project opnieuw wilt implementeren, selecteert u de bestaande resourcegroep direct vanuit het snelmenu van het Azure-resourcegroepproject. In het snelmenu selecteert u **Implementeren** en vervolgens selecteert u de resourcegroep waarin u hebt geïmplementeerd.
   
    ![Azure-resourcegroep geïmplementeerd](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Code implementeren in uw infrastructuur
U hebt nu de infrastructuur geïmplementeerd voor uw app, maar er is nog geen code geïmplementeerd in uw project. In dit artikel leest u hoe u een webtoepassing en SQL Database-tabellen implementeert tijdens de implementatie. Als u een virtuele machine implementeert in plaats van een web-app, moet u als onderdeel van de implementatie bepaalde code uitvoeren op de machine. De processen voor het implementeren van code voor een webtoepassing en voor het instellen van een virtuele machine zijn bijna hetzelfde.

1. Voeg een project toe aan uw Visual Studio-oplossing. Klik met de rechtermuisknop op de oplossing en selecteer **Toevoegen** > **Nieuw Project**.
   
    ![project toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)
2. Voeg een **ASP.NET-webtoepassing** toe. 
   
    ![webtoepassing toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
3. Selecteer **MVC**.
   
    ![MVC selecteren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
4. Nadat Visual Studio uw web-app heeft gemaakt, ziet u beide projecten in de oplossing.
   
    ![projecten weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)
5. Nu moet u nagaan of het nieuwe project gekoppeld is aan uw resourcegroepproject. Ga terug naar uw resourcegroepproject (AzureResourceGroup1). Klik met de rechtermuisknop op **Verwijzingen** en selecteer **Verwijzing toevoegen**.
   
    ![verwijzing toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)
6. Selecteer het web-app-project dat u hebt gemaakt.
   
    ![verwijzing toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
   
    Wanneer u een verwijzing toevoegt, koppelt u het webtoepassingsproject aan het resourcegroepproject en worden automatisch drie sleuteleigenschappen ingesteld. U ziet deze eigenschappen in het venster **Eigenschappen** voor de verwijzing.
   
      ![verwijzing bekijken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
   
    De eigenschappen zijn:
   
   * **Extra eigenschappen** bevat de faseringslocatie van het webimplementatiepakket dat naar Azure Storage wordt gepusht. Noteer de map (ExampleApp) en het bestand (package.zip). U moet weten wat deze waarden zijn, omdat u ze als parameters moet opgeven tijdens het implementeren van de app. 
   * **Bestandspad toevoegen** bevat het pad waar het pakket wordt gemaakt. **Doelen toevoegen** bevat de opdracht die tijdens de implementatie wordt uitgevoerd. 
   * Met de standaardwaarde van **Build;Package** kan tijdens de implementatie een webtoepassingspakket worden gebouwd en gemaakt (pakket.zip).  
     
     Er is geen publicatieprofiel nodig omdat tijdens de implementatie de benodigde informatie voor het maken van het pakket wordt opgehaald uit de eigenschappen.
7. Ga terug naar WebSiteSQLDatabase.json en voeg een resource toe aan de sjabloon.
   
    ![resource toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)
8. Selecteer deze keer **Webimplementatie voor Web Apps**. 
   
    ![webimplementatie toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
9. Implementeer uw resourcegroepproject opnieuw in de resourcegroep. Deze keer zijn er enkele nieuwe parameters. U hoeft geen waarden op te geven voor **_artifactsLocation** of **_artifactsLocationSasToken** omdat Visual Studio deze waarden automatisch genereert. Stel de map- en bestandsnaam in op het pad dat het implementatiepakket bevat (weergegeven als **ExampleAppPackageFolder** en **ExampleAppPackageFileName** in de volgende afbeelding). Geef de waarden op die u eerder in de verwijzingseigenschappen hebt gezien (**ExampleApp** en **package.zip**).
   
    ![webimplementatie toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
   
    Voor het **artefactopslagaccount** kunt u het account selecteren dat is geïmplementeerd met deze resourcegroep.
10. Nadat de implementatie is voltooid, selecteert u uw web-app in de portal. Selecteer de URL om naar de site te bladeren.
    
     ![naar site bladeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)
11. De implementatie van de standaard ASP.NET-app is voltooid.
    
     ![geïmplementeerde app weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="add-an-operations-dashboard-to-your-deployment"></a>Een bewerkingsdashboard toevoegen aan uw implementatie
U bent niet beperkt tot alleen de resources die beschikbaar zijn via de Visual Studio-interface. U kunt uw implementatie aanpassen door een aangepaste resource toe te voegen aan uw sjabloon. Als u wilt zien hoe het toevoegen van een resource in zijn werk gaat, voegt u een operationeel dashboard toe voor het beheren van de resource die u hebt geïmplementeerd.

1. Open het bestand WebsiteSqlDeploy.json en voeg de volgende JSON toe, na de resource voor het opslagaccount maar voor het afsluitende `]` van de sectie resources.

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

2. Implementeer de resourcegroep opnieuw. Kijk naar het dashboard in Azure Portal en u ziet dat het gedeelde dashboard is toegevoegd aan uw lijst met mogelijkheden.

   ![Aangepast dashboard](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/view-custom-dashboards.png)

3. Selecteer het dashboard.

   ![Aangepast dashboard](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/Ops-DemoSiteGroup-dashboard.png)

U kunt de toegang tot het dashboard beheren met behulp van RBAC-groepen. U kunt ook het uiterlijk van het dashboard aanpassen zodra de implementatie is voltooid. Als u de resourcegroep echter opnieuw implementeert, wordt het dashboard opnieuw ingesteld op de standaardstatus in de sjabloon. Zie [Op programmatische wijze Azure-dashboards maken](../azure-portal/azure-portal-dashboards-create-programmatically.md) voor meer informatie over het maken van dashboards.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u sjablonen kunt maken en implementeren met Visual Studio. In de volgende zelfstudie leert u hoe u de informatie van de sjabloonverwijzing kunt vinden, zodat u een versleuteld Azure-opslagaccount kunt maken.

> [!div class="nextstepaction"]
> [Een versleuteld opslagaccount maken](./resource-manager-tutorial-create-encrypted-storage-accounts.md)
