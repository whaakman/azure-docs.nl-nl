---
title: Inrichten en microservices zoals verwacht in Azure implementeren
description: "Informatie over het implementeren van een toepassing die bestaat uit microservices in Azure App Service als één eenheid en in een voorspelbare wijze met JSON resource group-sjablonen en PowerShell-scripts."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: bb51e565-e462-4c60-929a-2ff90121f41d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2016
ms.author: cephalin
ms.openlocfilehash: 3719e037f1564411a8f94d1ca962ba1ef6b5d435
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Inrichten en microservices zoals verwacht in Azure implementeren
Deze zelfstudie laat zien hoe inrichten en implementeren van een toepassing die bestaat uit [microservices](https://en.wikipedia.org/wiki/Microservices) in [Azure App Service](/services/app-service/) als één eenheid en in een voorspelbare wijze JSON resource group-sjablonen en PowerShell-scripts. 

Bij het inrichten en implementeren van toepassingen met hoge schaalbaarheid die bestaan uit maximaal losgekoppeld zijn microservices, herhaalbaarheid en voorspelbaarheid essentieel voor succes. [Azure App Service](/services/app-service/) kunt u microservices met web-apps, mobiele apps, API-apps en logic apps maken. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kunt u alle microservices beheren als één eenheid, samen met bronafhankelijkheden zoals database en controle-instellingen van de gegevensbron. U kunt nu ook dergelijke toepassing met behulp van JSON-sjablonen en eenvoudig PowerShell-scripts implementeren. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Wat u doet
In de zelfstudie implementeert u een toepassing die gebruikmaakt van:

* Twee web-apps (dat wil zeggen twee microservices)
* Een back-end SQL-Database
* App-instellingen, verbindingsreeksen en bronbeheer
* Application insights, waarschuwingen, instellingen voor automatisch schalen

## <a name="tools-you-will-use"></a>Hulpprogramma's die u wilt gebruiken
In deze zelfstudie gebruikt u de volgende hulpprogramma's. Omdat het geen uitgebreide bespreking van de hulpprogramma's, ga ik blijven op het end-to-end-scenario en net bieden een korte inleiding tot elk, en vindt u meer informatie over deze. 

### <a name="azure-resource-manager-templates-json"></a>Azure Resource Manager-sjablonen (JSON)
Telkens wanneer u een web-app in Azure App Service maakt, Azure Resource Manager gebruikt bijvoorbeeld een JSON-sjabloon maken van de hele resourcegroep met de onderdeel-resources. Een complexe sjabloon van de [Azure Marketplace](/marketplace) zoals de [schaalbare WordPress](/marketplace/partners/wordpress/scalablewordpress/) app kan bevatten de MySQL-database, storage-accounts, het App Service-abonnement, WebApp zelf, waarschuwingsregels, app-instellingen instellingen voor automatisch schalen en meer en alle deze sjablonen zijn beschikbaar via PowerShell. Zie voor meer informatie over het downloaden en gebruiken van deze sjablonen [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md).

Zie voor meer informatie over de Azure Resource Manager-sjablonen [Azure Resource Manager-sjablonen ontwerpen](../azure-resource-manager/resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>2.6 Azure SDK voor Visual Studio
De nieuwste SDK bevat verbeteringen in de sjabloonondersteuning voor de Resource Manager-in de JSON-editor. Gebruik deze optie om snel te maken van een sjabloon van de groep resource vanaf het begin of open een bestaande JSON-sjabloon (zoals een gedownloade gallery-sjabloon) voor de wijziging, het parameterbestand vullen en zelfs de resourcegroep rechtstreeks vanuit een Azure-Resource implementeren Groep oplossing.

Zie voor meer informatie [2.6 van de Azure SDK voor Visual Studio](https://azure.microsoft.com/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Azure PowerShell 0.8.0 of hoger
Vanaf versie 0.8.0, omvat de Azure PowerShell-installatie de module Azure Resource Manager naast de Azure-module. Deze nieuwe module kunt u de implementatie van resourcegroepen met een script.

Zie voor meer informatie [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure Resource Explorer
Dit [voorbeeldfunctie](https://resources.azure.com) kunt u de definities van de JSON van de resourcegroepen in uw abonnement en de afzonderlijke resources verkennen. In het hulpprogramma kunt u de JSON-definities van een resource bewerken, verwijderen van de gehele hiërarchie van resources en maken van nieuwe resources.  De informatie die direct beschikbaar is in dit hulpprogramma is erg handig voor het ontwerpen van een sjabloon omdat er wordt aangegeven welke eigenschappen u moet instellen voor een bepaald type resource, de juiste waarden, enzovoort. U kunt zelfs maken met de resourcegroep in de [Azure Portal](https://portal.azure.com/), Controleer de definities van JSON in het hulpprogramma explorer kunt u de resourcegroep templatize.

### <a name="deploy-to-azure-button"></a>Implementeren naar Azure-knop
Als u GitHub voor broncodebeheer gebruikt, kunt u plaatsen een [implementeren in Azure knop](https://azure.microsoft.com/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) in de Leesmij-bestand. MD, waardoor de implementatie van een directe UI naar Azure. Terwijl u dit voor een eenvoudige web-app doen kunt, kunt u deze optie als u wilt inschakelen voor het implementeren van een hele resourcegroep de toevoeging van een bestand azuredeploy.json in de hoofdmap van de opslagplaats kunt uitbreiden. Deze JSON-bestand dat de resource-group-sjabloon bevat, wordt gebruikt door de implementeren in Azure-knop om de resourcegroep te maken. Zie voor een voorbeeld de [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) voorbeeld, gaat u in deze zelfstudie.

## <a name="get-the-sample-resource-group-template"></a>Ophalen van de voorbeeldsjabloon voor de resource-groep
Nu gaan we direct aan het.

1. Navigeer naar de [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) voorbeeld-App Service.
2. Klik in de readme.md, **implementeren in Azure**.
3. U hebt uitgevoerd om de [implementeren naar azure](https://deploy.azure.com) site en wordt gevraagd implementatie invoerparameters. U ziet dat de meeste velden zijn gevuld met de naam van de opslagplaats en sommige willekeurige tekenreeksen voor u. U kunt alle velden wijzigen als u wilt, maar alleen wat u moet invoeren zijn de aanmelding van SQL Server-beheerdersrechten en het wachtwoord en klik vervolgens op **volgende**.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)
4. Klik vervolgens op **implementeren** starten van het implementatieproces. Nadat het proces wordt uitgevoerd worden voltooid, klikt u op de http://todoapp*XXXX*. azurewebsites.net koppeling naar de geïmplementeerde toepassing bladeren. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)
   
   De gebruikersinterface zou een beetje traag zijn wanneer u eerst het niet doorzoeken, omdat de apps alleen worden gestart, maar zelf te overtuigen dat het een toepassing volledig functioneel is.
5. Klik in de pagina implementeren op de **beheren** koppeling om te zien van de nieuwe toepassing in de Azure Portal.
6. In de **Essentials** vervolgkeuzelijst, klik op de koppeling van de groep resource. U ziet ook dat de web-app al is verbonden met de GitHub-opslagplaats onder **extern Project**. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
7. Houd er rekening mee dat er al twee web-apps en één SQL-Database in de resourcegroep zijn in de resourcegroepblade.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)

Alles wat u hebt gezien in enkele korte minuten is een volledig geïmplementeerde twee-microservice-toepassing, met alle onderdelen, afhankelijkheden, instellingen, database en continue publicatie ingesteld door een geautomatiseerde orchestration in Azure Resource Manager. Dit alles is uitgevoerd door twee dingen:

* De implementeren in Azure-knop
* azuredeploy.JSON in de hoofdmap van de opslagplaats

U kunt deze dezelfde toepassing implementeren tientallen, honderden of duizenden malen en de exacte dezelfde configuratie elke keer hebben. De herhaalbaarheid en de voorspelbaarheid van deze benadering kunt u hoge schaalbaarheid toepassingen implementeren met gemak en betrouwbaarheid.

## <a name="examine-or-edit-azuredeployjson"></a>Bekijk AZUREDEPLOY (of bewerken). JSON
Nu gaan we kijken hoe de GitHub-opslagplaats is ingesteld. U de JSON-editor gebruikt in de Azure .NET SDK, dus als u nog niet hebt geïnstalleerd [Azure .NET SDK 2.6](https://azure.microsoft.com/downloads/), dat nu doen.

1. Kloon de [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) opslagplaats met uw favoriete git-hulpprogramma. In de onderstaande schermafbeelding ben ik dit doen in het Team Explorer in Visual Studio 2013.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)
2. Open in de hoofdmap van de opslagplaats azuredeploy.json in Visual Studio. Als u het overzichtsvenster van JSON niet ziet, moet u de Azure .NET SDK installeren.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Ik ga niet te beschrijven elk detail van de JSON-indeling, maar de [meer bronnen](#resources) sectie bevat koppelingen voor het leren van de sjabloontaal van de resource-groep. Hier Ga alleen ik om weer te geven de interessante functies die u kunnen helpen bij het maken van uw eigen aangepaste sjabloon voor app-implementatie aan de slag.

### <a name="parameters"></a>Parameters
Bekijk het gedeelte parameters om te zien dat de meeste van deze parameters wat zijn de **implementeren in Azure** knop vraagt u om in te voeren. De site achter de **implementeren in Azure** knop vult de gebruikersinterface van de gedefinieerde parameters in azuredeploy.json met behulp van de invoer. Deze parameters worden gebruikt in de resourcedefinities zoals resourcenamen, eigenschapswaarden, enz.

### <a name="resources"></a>Resources
U ziet in het knooppunt resources dat 4 op het hoogste niveau resources zijn gedefinieerd, met inbegrip van een SQL Server-exemplaar, een App Service-plan en twee web-apps. 

#### <a name="app-service-plan"></a>App Service-plan
Laten we beginnen met een eenvoudige hoofdniveau resource in de JSON. Klik op de App Service-abonnement met de naam in de JSON-overzicht **[hostingPlanName]** Markeer de bijbehorende JSON-code. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Houd er rekening mee dat de `type` element geeft de tekenreeks voor een App Service-abonnement (het is wel een serverfarm een lange, lange tijd geleden), en andere elementen en eigenschappen worden ingevuld met de parameters die zijn gedefinieerd in het JSON-bestand en deze bron niet beschikt over een geneste bronnen.

> [!NOTE]
> U ziet ook dat de waarde van `apiVersion` vertelt Azure welke versie van de REST-API voor het gebruik van de resourcedefinitie JSON met, maar hoe de bron moet zijn geformatteerd beïnvloeden kan in de `{}`. 
> 
> 

#### <a name="sql-server"></a>SQL Server
Klik op de SQL Server-resource met de naam **SQLServer** in het JSON-overzicht.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)

Houd rekening met de volgende met betrekking tot de gemarkeerde JSON-code:

* Het gebruik van parameters zorgt ervoor dat de gemaakte resources met de naam en geconfigureerd op een manier die zorgt ervoor dat ze consistent zijn met elkaar.
* De SQL Server-bron heeft twee ingesloten resources, elk een andere waarde voor `type`.
* De ingesloten resources binnen `“resources”: […]`, waarbij de database en de firewallregels zijn gedefinieerd, hebben een `dependsOn` element waarmee de resource-ID van de bron van de SQL Server op hoofdniveau. Dit vertelt Azure Resource Manager 'voordat u deze resource die andere resources moet al bestaan; maken en als die andere resource vervolgens in de sjabloon is gedefinieerd maken dat een eerst'.
  
  > [!NOTE]
  > Voor gedetailleerde informatie over het gebruik van de `resourceId()` werkt, Zie [Azure Resource Manager-sjabloonfuncties](../azure-resource-manager/resource-group-template-functions-resource.md#resourceid).
  > 
  > 
* Het effect van de `dependsOn` -element is dat Azure Resource Manager weten kunt welke resources parallel kunnen worden gemaakt en welke resources moeten opeenvolgend worden gemaakt. 

#### <a name="web-app"></a>Web-app
Nu gaan we naar de werkelijke web-apps zelf, die gecompliceerder zijn. Klik op de web-app [variables('apiSiteName')] in het JSON-overzicht markeren de JSON-code. U zult zien dat dingen zijn het veel meer interessant. Voor dit doel leert ik over de functies één voor één:

##### <a name="root-resource"></a>Basis-resource
De web-app, is afhankelijk van twee verschillende bronnen. Dit betekent dat dat Azure Resource Manager de web-app maakt alleen als zowel de App Service-abonnement als de SQL Server-exemplaar worden gemaakt.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>App-instellingen
De app-instellingen zijn ook gedefinieerd als een geneste resource.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

In de `properties` element voor `config/appsettings`, hebt u twee appinstellingen in de indeling `“<name>” : “<value>”`.

* `PROJECT`is een [KUDU instelling](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) die vertelt Azure-implementatie project moet worden gebruikt in een meerdere project Visual Studio-oplossing. Leest u hoe broncodebeheer is geconfigureerd, maar omdat de code ToDoApp zich in een meerdere project Visual Studio-oplossing, moet deze instelling.
* `clientUrl`is gewoon een instelling die de toepassingscode app gebruikt.

##### <a name="connection-strings"></a>Verbindingsreeksen
De verbindingsreeksen zijn ook gedefinieerd als een geneste resource.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

In de `properties` element voor `config/connectionstrings`, elke verbindingsreeks ook is gedefinieerd als een combinatie van naam: waarde, met de specifieke notatie `“<name>” : {“value”: “…”, “type”: “…”}`. Voor de `type` element mogelijke waarden zijn `MySql`, `SQLServer`, `SQLAzure`, en `Custom`.

> [!TIP]
> Voor een definitieve lijst van de tekenreeks verbindingstypen, kunt u de volgende opdracht uitvoeren in Azure PowerShell: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
> 
> 

##### <a name="source-control"></a>Resourcebeheer
De instellingen voor bronbeheer zijn ook gedefinieerd als een geneste resource. Azure Resource Manager gebruikt deze bron voor het configureren van continue publicatie (Zie voorbehoud op `IsManualIntegration` later) en ook ere van de implementatie van toepassingscode automatisch tijdens de verwerking van het JSON-bestand.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`en `branch` moet vrij intuïtieve en moet verwijzen naar de Git-opslagplaats en de naam van de vertakking vanuit publiceren. Deze worden opnieuw, gedefinieerd door de invoerparameters. 

Houd er rekening mee de `dependsOn` element dat naast de webbron app zelf, `sourcecontrols/web` ook afhankelijk van `config/appsettings` en `config/connectionstrings`. Dit is omdat zodra `sourcecontrols/web` is geconfigureerd, het proces van het Azure-implementatie wordt automatisch geprobeerd te implementeren, bouwen en de toepassingscode te starten. Daarom kunt u ervoor zorgen dat de toepassing toegang tot de vereiste app-instellingen en verbindingsreeksen heeft voordat de toepassingscode wordt uitgevoerd voor het invoegen van deze afhankelijkheid. 

> [!NOTE]
> U ziet ook dat `IsManualIntegration` is ingesteld op `true`. Deze eigenschap is in deze zelfstudie nodig omdat u de GitHub-opslagplaats niet daadwerkelijk in bezit en dus kan niet daadwerkelijk machtigingen aan Azure toekennen voor het configureren van continue publiceren vanaf [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) (dat wil zeggen push automatische opslagplaats bijwerken naar Azure). U kunt de standaardwaarde `false` voor de opgegeven bibliotheek alleen als u de GitHub-referenties van de eigenaar in hebt geconfigureerd de [Azure-portal](https://portal.azure.com/) voordat. Met andere woorden, als u hebt ingesteld met broncodebeheer GitHub of BitBucket voor een app in de [Azure Portal](https://portal.azure.com/) voorheen werd door gebruik te maken van uw referenties, vervolgens Azure worden de referenties onthouden en ze gebruiken wanneer u een app van implementeren GitHub of BitBucket in de toekomst. Echter, als u dit nog niet hebt gedaan, implementatie van het JSON-sjabloon mislukken wanneer Azure Resource Manager probeert te configureren van instellingen voor de web-app bronbeheer omdat deze niet kan op GitHub of BitBucket met referenties van de eigenaar van de opslagplaats aanmelden.
> 
> 

## <a name="compare-the-json-template-with-deployed-resource-group"></a>De JSON-sjabloon met de geïmplementeerde resourcegroep vergelijken
Hier, doorloopt u de blades alle de van web-app in de [Azure Portal](https://portal.azure.com/), maar er is een ander hulpprogramma dat net zoals handig als niet meer. Ga naar de [Azure Resource Explorer](https://resources.azure.com) voorbeeldfunctie waarmee u een JSON-weergave van de resourcegroepen in uw abonnementen, omdat ze werkelijk bestaan in de Azure back-end. U ziet ook hoe de resourcegroep JSON hiërarchie in Azure komt overeen met de hiërarchie in de sjabloonbestand dat wordt gebruikt om deze te maken.

Bijvoorbeeld, wanneer ik ga naar de [Azure Resource Explorer](https://resources.azure.com) hulpprogramma en vouw de knooppunten in de Verkenner, ik ziet u de resourcegroep en de bronnen op hoofdniveau die worden verzameld onder hun respectieve resourcetypen.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Als u een web-app bereiken, moet u kunnen voor een overzicht van web-app-configuratiedetails vergelijkbaar met de onderstaande schermafbeelding:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Opnieuw ingesloten resources moeten een vergelijkbaar met die in het JSON-sjabloonbestand hiërarchie hebt en ziet u de app-instellingen, verbindingsreeksen, enzovoort, juist weergegeven in het deelvenster JSON. Het ontbreken van instellingen hier kan wijzen op een probleem met uw JSON-bestand en kunt u uw JSON-sjabloonbestand oplossen.

## <a name="deploy-the-resource-group-template-yourself"></a>Implementatie van de resource-group-sjabloon
De **implementeren in Azure** knop is een goede maar kunt u voor het implementeren van de resource-group-sjabloon in azuredeploy.json alleen als u al azuredeploy.json met GitHub gepusht hebt. De Azure .NET SDK biedt ook de hulpprogramma's voor u voor het implementeren van een bestand JSON-sjabloon rechtstreeks vanuit uw lokale computer. U doet dit door de volgende stappen uit te voeren:

1. Klik in Visual Studio **bestand** > **nieuw** > **Project**.
2. Klik op **Visual C#** > **Cloud** > **Azure-resourcegroep**, klikt u vervolgens op **OK**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)
3. In **Azure-sjabloon selecteren**, selecteer **lege sjabloon** en klik op **OK**.
4. Sleep azuredeploy.json in de **sjabloon** map van het nieuwe project.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)
5. Open de gekopieerde azuredeploy.json in Solution Explorer.
6. Alleen voor de demonstratie we enkele standaard Application Insight resources toevoegen aan onze JSON-bestand door te klikken op **Resource toevoegen**. Als u alleen geïnteresseerd bent in het JSON-bestand te implementeren, gaat u naar de stappen implementeren.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)
7. Selecteer **Application Insights voor Web-Apps**, Controleer of een bestaande App Service-plan en web app is geselecteerd en klik vervolgens op **toevoegen**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)
   
   U hebt nu mogelijk om te bekijken van verschillende nieuwe resources die, afhankelijk van de bron en wat het doet, zijn afhankelijk van de App Service-abonnement of de web-app. Deze resources niet zijn ingeschakeld in hun bestaande definitie en gaat u deze instelling wijzigen.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
8. Klik in de JSON-overzicht op **appInsights automatisch schalen** Markeer de JSON-code. Dit is de instelling voor vergroten/verkleinen voor uw App Service-abonnement.
9. Zoek in de gemarkeerde JSON-code, de `location` en `enabled` eigenschappen en ze te stellen zoals hieronder wordt weergegeven.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)
10. Klik in de JSON-overzicht op **CPUHigh appInsights** Markeer de JSON-code. Dit is een waarschuwing.
11. Zoek de `location` en `isEnabled` eigenschappen en ze te stellen zoals hieronder wordt weergegeven. Doe hetzelfde voor de andere drie waarschuwingen (paarse bollen).
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)
12. U bent nu klaar om te implementeren. Met de rechtermuisknop op het project en selecteer **implementeren** > **nieuwe implementatie**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)
13. Meld u aan bij uw Azure-account als u dat nog niet hebt gedaan.
14. Selecteer een bestaande resourcegroep in uw abonnement of maak een nieuwe één, selecteer **azuredeploy.json**, en klik vervolgens op **Parameters bewerken**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)
    
    U hebt nu mogelijk om te bewerken van de parameters die zijn gedefinieerd in het sjabloonbestand in een tabel nice. Parameters die standaardwaarden definiëren al hun standaardwaarden en parameters die een lijst van toegestane waarden te definiëren als opgegeven waarin dropdowns worden weergegeven.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)
15. Vul de lege parameters en gebruik de [GitHub-repo-adres voor ToDoApp](https://github.com/azure-appservice-samples/ToDoApp.git) in **repoUrl**. Klik vervolgens op **opslaan**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)
    
    > [!NOTE]
    > Automatisch schalen is een functie die wordt aangeboden op **standaard** laag of hoger en plan niveau waarschuwingen zijn functies die worden aangeboden **Basic** servicetier of hoger, moet u om in te stellen de **sku** parameter naar **standaard** of **Premium** om te zien van al uw nieuwe App Insights resources lichte up.
    > 
    > 
16. Klik op **implementeren**. Als u hebt geselecteerd **wachtwoorden opslaan**, het wachtwoord wordt opgeslagen in het parameterbestand **als tekst zonder opmaak**. Anders wordt u gevraagd het wachtwoord invoeren tijdens de implementatie.

Dat is alles. Nu u alleen hoeft te gaat u naar de [Azure Portal](https://portal.azure.com/) en de [Azure Resource Explorer](https://resources.azure.com) hulpprogramma om te zien van de nieuwe waarschuwingen en de instellingen voor automatisch schalen toegevoegd aan uw JSON-toepassing geïmplementeerd.

De stappen in deze sectie doen hoofdzakelijk het volgende:

1. Het sjabloonbestand voorbereid
2. Een parameterbestand te gaan met het sjabloonbestand gemaakt
3. Het sjabloonbestand met het parameterbestand geïmplementeerd

De laatste stap wordt eenvoudig uitgevoerd door een PowerShell-cmdlet. Als u wilt zien wat Visual Studio gedaan wanneer uw toepassing is geïmplementeerd, open Scripts\Deploy AzureResourceGroup.ps1. Er is veel code er, maar ik Ga net Markeer de relevante code moet u het sjabloonbestand met de parameterbestand te implementeren.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

De laatste cmdlet `New-AzureResourceGroup`, is het adres waarmee de actie uitvoert. Dit alles moet aan u aantonen dat met behulp van tooling, relatief eenvoudig is voor het implementeren van uw cloudtoepassing zoals verwacht. Telkens wanneer u de cmdlet op dezelfde sjabloon met de dezelfde parameter-bestand uitvoert, gaat u hetzelfde resultaat.

## <a name="summary"></a>Samenvatting
In de DevOps zijn herhaalbaarheid en voorspelbaarheid sleutels voor een geslaagde implementatie van een toepassing met hoge schaalbaarheid van microservices bestaan. In deze zelfstudie maakt hebt u een twee-microservice-toepassing in Azure als één resourcegroep met de Azure Resource Manager-sjabloon geïmplementeerd. Het gegeven in het ideale geval, heeft de kennis die u nodig hebt om te beginnen met het converteren van uw toepassing in Azure in een sjabloon en kunt inrichten en zoals verwacht implementeren. 

<a name="resources"></a>

## <a name="more-resources"></a>Meer bronnen
* [Taal van Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager-sjablonen ontwerpen](../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager-sjabloonfuncties](../azure-resource-manager/resource-group-template-functions.md)
* [Implementeer een toepassing met Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure PowerShell gebruiken met Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)
* [Implementaties van de resourcegroep in Azure oplossen](../azure-resource-manager/resource-manager-common-deployment-errors.md)

