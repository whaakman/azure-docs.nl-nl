---
title: Visual Studio Azure-resourcegroepprojecten | Microsoft Docs
description: Gebruik Visual Studio om een Azure-resourcegroepproject te maken en de resources in Azure te implementeren.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 4bd084c8-0842-4a10-8460-080c6a085bec
ms.service: azure-resource-manager
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: tomfitz
ms.openlocfilehash: d647206b882059e0651223dc84f2ad2a314f8a87
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Azure-resourcegroepen maken en implementeren met Visual Studio
Met Visual Studio en de [Azure SDK](https://azure.microsoft.com/downloads/) kunt u een project maken waarmee uw infrastructuur en code in Azure worden geïmplementeerd. U kunt bijvoorbeeld de webhost, website en database voor uw app opgeven en die infrastructuur samen met de code implementeren. U kunt ook een virtuele machine, een virtueel netwerk en een opslagaccount opgeven en die infrastructuur implementeren in combinatie met een script dat wordt uitgevoerd op de virtuele machine. Met het implementatieproject voor de **Azure-resourcegroep** kunt u alle vereiste resources met één herhaalbare bewerking implementeren. Zie voor meer informatie over het implementeren en beheren van uw resources [Overzicht van Azure Resource Manager](resource-group-overview.md).

Azure-resourcegroepprojecten bevatten Azure Resource Manager JSON-sjablonen, die de resources definiëren die u implementeert in Azure. Zie [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md) voor meer informatie over de onderdelen van een Resource Manager-sjabloon. Met Visual Studio kunt u deze sjablonen bewerken. Visual Studio voorziet ook in hulpprogramma's die het eenvoudiger maken om met sjablonen te werken.

In dit artikel gaat u een web-app en SQL Database implementeren. De stappen zijn echter bijna hetzelfde zijn voor elk type resource. U kunt net zo eenvoudig een Virtuele Machine en de bijbehorende resources implementeren. Visual Studio biedt veel verschillende startsjablonen om te implementeren in algemene scenario's.

In dit artikel wordt Visual Studio 2017 gebruikt. Als u Visual Studio 2015 Update 2 en Microsoft Azure SDK voor .NET 2.9 gebruikt, of Visual Studio 2013 met Azure SDK 2.9, is de ervaring in grote lijnen hetzelfde. U kunt Azure SDK versie 2.6 en hoger gebruiken, maar dan is uw ervaring mogelijk anders dan beschreven in dit artikel. Het is raadzaam om de nieuwste versie van de [Azure SDK](https://azure.microsoft.com/downloads/) te installeren voordat u met de stappen begint. 

## <a name="create-azure-resource-group-project"></a>Een Azure-resourcegroepproject maken
In deze procedure maakt u een Azure Resource Group-project met het sjabloon **Web app + SQL**.

1. Kies in Visual Studio **Bestand** en vervolgens **Nieuw Project**. Selecteer **C#** of **Visual Basic** (welke taal u kiest heeft geen invloed op de latere stadia, omdat deze projecten alleen JSON- en PowerShell-inhoud bevatten). Kies vervolgens **Cloud** en het project **Azure-resourcegroep**.
   
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
   
    Omdat er voor dit voorbeeld is gekozen voor het sjabloon Web-app + SQL, krijgt u de volgende bestanden te zien: 
   
   | Bestandsnaam | Beschrijving |
   | --- | --- |
   | Deploy-AzureResourceGroup.ps1 |Een PowerShell-script waarmee PowerShell-opdrachten worden aangeroepen om te worden geïmplementeerd in Azure Resource Manager.<br />**Opmerking** Visual Studio gebruikt dit PowerShell-script voor het implementeren van uw sjabloon. Eventuele wijzigingen die u aanbrengt in dit script beïnvloeden de implementatie in Visual Studio, dus wees voorzichtig. |
   | WebSiteSQLDatabase.json |Het Resource Manager-sjabloon dat de infrastructuur definieert die u in Azure wilt implementeren en de parameters die u kunt opgeven tijdens de implementatie. Hiermee worden ook de afhankelijkheden tussen resources gedefinieerd zodat deze in de juiste volgorde worden geïmplementeerd. |
   | WebSiteSQLDatabase.parameters.json |Een parameterbestand dat de waarden bevat die nodig zijn voor het sjabloon. U geeft parameterwaarden door om elke implementatie aan te passen. |
   
    Alle resourcegroepimplementatieprojecten bevatten deze algemene bestanden. Andere projecten bevatten mogelijk extra bestanden ter ondersteuning van andere functies.

## <a name="customize-the-resource-manager-template"></a>Het Resource Manager-sjabloon aanpassen
U kunt een implementatieproject aanpassen door de JSON-sjablonen te wijzigen waarin wordt beschreven welke resources u wilt implementeren. JSON staat voor JavaScript Object Notation en is een geserialiseerde gegevensindeling waarmee eenvoudig te werken is. Voor JSON-bestanden wordt een schema gebruikt waarnaar u aan de bovenkant van elk bestand naar verwijst. U kunt het schema downloaden en het analyseren als u er meer inzicht in wilt krijgen. In het schema wordt gedefinieerd welke elementen zijn toegestaan, welke soorten en indelingen voor velden er zijn, wat de mogelijke opsommingswaarden zijn, enzovoort. Zie [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md) voor meer informatie over de onderdelen van een Resource Manager-sjabloon.

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

De parameter **storageType** wordt vooraf gedefinieerd met de toegestane typen en een standaardtype. U kunt deze waarden laten staan of ze bewerken voor uw scenario. Als u niet iedereen wilt toestaan om een **Premium_LRS**-opslagaccount te implementeren via dit sjabloon, verwijdert u dit uit de toegestane typen. 

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
   
    Met **hostingPlanName** geeft u de naam op voor het te maken [App Service-plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 
   
    Met **administratorLogin** geeft u de gebruikersnaam op voor de SQL Server-beheerder. Gebruik geen veelvoorkomende beheerdersnamen als **sa** of **admin**. 
   
    Met **administratorLoginPassword** geeft u een wachtwoord op de SQL Server-beheerder. De optie **Wachtwoorden opslaan als tekst zonder opmaak in het parameterbestand** is niet beveiligd. Selecteer deze optie daarom niet. Omdat het wachtwoord niet als tekst zonder opmaak wordt opgeslagen, moet u dit wachtwoord opnieuw opgeven tijdens de implementatie. 
   
    Met **databaseName** geeft u een naam op voor de te maken database. 
   
    ![Dialoogvenster Parameters bewerken](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
5. Klik op **Implementeren** om het project in Azure te implementeren. Een PowerShell-console wordt geopend buiten het Visual Studio-exemplaar. Voer het beheerderswachtwoord voor de SQL Server in de PowerShell-console in wanneer om het wachtwoord wordt gevraagd. **De PowerShell-console kan zijn verborgen achter andere items of geminimaliseerd op de taakbalk.** Zoek en selecteer deze console om het wachtwoord op te geven.
   
   > [!NOTE]
   > Visual Studio vraagt u mogelijk om de Azure PowerShell-cmdlets te installeren. U hebt de Azure PowerShell-cmdlets nodig om resourcegroepen te kunnen implementeren. Installeer ze als dit gevraagd wordt.
   > 
   > 
6. De implementatie kan enkele minuten in beslag nemen. U kunt de voortgang van de implementatie bekijken in het venster **Uitvoer**. Wanneer de implementatie is voltooid, ziet u een laatste bericht dat de implementatie is geslaagd en dat er ongeveer als volgt uitziet:
   
        ... 
        18:00:58 - Successfully deployed template 'websitesqldatabase.json' to resource group 'DemoSiteGroup'.
7. Open in een browser de [Azure Portal](https://portal.azure.com/) en meld u aan bij uw account. Als u de resourcegroep wilt bekijken, selecteert u **Resourcegroepen** en vervolgens de resourcegroep waarin u hebt geïmplementeerd.
   
    ![groep selecteren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)
8. U ziet alle geïmplementeerde resources. Merk op dat de naam van het opslagaccount niet precies overeenkomt met wat u hebt opgegeven bij het toevoegen van die resource. Het opslagaccount moet uniek zijn. De sjabloon voegt automatisch een tekenreeks toe aan de naam die u hebt opgegeven zodat dit een unieke naam wordt. 
   
    ![resources weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)
9. Als u wijzigingen aanbrengt en u het project opnieuw wilt implementeren, selecteert u de bestaande resourcegroep direct vanuit het snelmenu van het Azure-resourcegroepproject. In het snelmenu selecteert u **Implementeren** en vervolgens selecteert u de resourcegroep waarin u hebt geïmplementeerd.
   
    ![Azure-resourcegroep geïmplementeerd](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Code implementeren in uw infrastructuur
U hebt nu de infrastructuur geïmplementeerd voor uw app, maar er is nog geen code geïmplementeerd in uw project. In dit artikel leest u hoe u een webtoepassing en SQL Database-tabellen implementeert tijdens de implementatie. Als u een virtuele machine implementeert in plaats van een webtoepassing, moet u als onderdeel van de implementatie code uitvoeren op de machine. De processen voor het implementeren van code voor een webtoepassing en voor het instellen van een virtuele machine zijn bijna hetzelfde.

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
     
     Er is geen publicatieprofiel nodig omdat tijdens de implementatie de benodigde informatie voor het maken van het pakket wordt verkregen uit de eigenschappen.
7. Ga terug naar WebSiteSQLDatabase.json en voeg een resource toe aan de sjabloon.
   
    ![resource toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)
8. Selecteer deze keer **Webimplementatie voor Web Apps**. 
   
    ![webimplementatie toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
9. Implementeer uw resourcegroepproject opnieuw in de resourcegroep. Deze keer zijn er enkele nieuwe parameters. U hoeft geen waarden op te geven voor **_artifactsLocation** en **_artifactsLocationSasToken** omdat Visual Studio deze automatisch gegenereert. Stel de map- en bestandsnaam in op het pad dat het implementatiepakket bevat (weergegeven als **ExampleAppPackageFolder** en **ExampleAppPackageFileName** in de volgende afbeelding). Geef de waarden op die u eerder in de verwijzingseigenschappen hebt gezien (**ExampleApp** en **package.zip**).
   
    ![webimplementatie toevoegen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
   
    Voor het **artefactopslagaccount** kunt u het account selecteren dat is geïmplementeerd met deze resourcegroep.
10. Nadat de implementatie is voltooid, selecteert u uw web-app in de portal. Selecteer de URL om naar de site te bladeren.
    
     ![naar site bladeren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)
11. Merk op dat u de standaard ASP.NET-app hebt geïmplementeerd.
    
     ![geïmplementeerde app weergeven](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Volgende stappen
* Voor meer informatie over het beheren van resources via de portal raadpleegt u [Azure Portal gebruiken om uw Azure-resources te beheren](resource-group-portal.md).
* Zie [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md) voor meer informatie over sjablonen.

