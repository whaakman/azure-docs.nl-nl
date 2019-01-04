---
title: Inrichten en implementeren van microservices zoals verwacht - Azure App Service
description: Informatie over het implementeren van een toepassing die bestaat uit microservices in Azure App Service als één eenheid en op een voorspelbare wijze met behulp van JSON-resourcegroepsjablonen en PowerShell-scripts.
services: app-service
documentationcenter: ''
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
ms.custom: seodec18
ms.openlocfilehash: 657211378d7b38b88ccd40aa31a175058e1ad67c
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54015553"
---
# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Inrichten en implementeren van microservices zoals verwacht in Azure
Deze zelfstudie laat zien hoe u kunt inrichten en implementeren van een toepassing die bestaat uit [microservices](https://en.wikipedia.org/wiki/Microservices) in [Azure App Service](https://azure.microsoft.com/services/app-service/) als één eenheid en op een voorspelbare wijze met behulp van JSON-resourcegroepsjablonen en PowerShell-scripts. 

Bij het inrichten en implementeren van grootschalige toepassingen die bestaan uit maximaal losgekoppeld zijn microservices, herhaalbaarheid en voorspelbaarheid essentieel voor succes. [Azure App Service](https://azure.microsoft.com/services/app-service/) kunt maken van microservices met web-apps, mobiele back-ends en API apps. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kunt u alle microservices beheren als een eenheid, samen met afhankelijkheden van resources, zoals de database en controle-instellingen van de gegevensbron. U kunt nu ook een toepassing met behulp van JSON-sjablonen en eenvoudige PowerShell-scripts implementeren. 

## <a name="what-you-will-do"></a>Wat u doet
In de zelfstudie implementeert u een toepassing die gebruikmaakt van:

* Twee App Service-apps (dat wil zeggen twee microservices)
* Een back-end van de SQL-Database
* App-instellingen, verbindingsreeksen en bronbeheer
* Application insights, waarschuwingen en instellingen voor automatisch schalen

## <a name="tools-you-will-use"></a>Hulpprogramma's die u wilt gebruiken
In deze zelfstudie gebruikt u de volgende hulpprogramma's. Omdat het geen uitgebreide discussie over de hulpprogramma's, ga ik Houd u aan de end-to-end-scenario alleen te geven en u een korte inleiding voor elke en waar u meer informatie over deze kan vinden. 

### <a name="azure-resource-manager-templates-json"></a>Azure Resource Manager-sjablonen (JSON)
Telkens wanneer u een app in Azure App Service maken, bijvoorbeeld een JSON-sjabloon door Azure Resource Manager gebruikt om de hele resourcegroep met de component-resources maken. Een complexe sjabloon van de [Azure Marketplace](/azure/marketplace) kunt opnemen de database, storage-accounts, het App Service-plan, de app zelf, regels voor waarschuwingen, app-instellingen, instellingen voor automatisch schalen en meer en alle deze sjablonen zijn beschikbaar voor u via PowerShell. Zie voor meer informatie over het downloaden en gebruiken van deze sjablonen [met behulp van Azure PowerShell met Azure Resource Manager](../powershell-azure-resource-manager.md).

Zie voor meer informatie over de Azure Resource Manager-sjablonen [Azure Resource Manager-sjablonen ontwerpen](../azure-resource-manager/resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Azure SDK 2.6 voor Visual Studio
De nieuwste SDK bevat verbeteringen voor de sjabloonondersteuning van de Resource Manager-in de JSON-editor. U kunt dit gebruiken om snel een resourcegroepsjabloon helemaal zelf maken of openen van een bestaande JSON-sjabloon (zoals een met gedownloade galeriesjabloon) voor de wijziging, vul in het parameterbestand en zelfs implementeren de resourcegroep die u rechtstreeks vanuit een Azure-Resource Groep-oplossing.

Zie voor meer informatie, [Azure SDK 2.6 voor Visual Studio](https://azure.microsoft.com/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Azure PowerShell 0.8.0 of hoger
Vanaf versie 0.8.0, omvat de Azure PowerShell-installatie de Azure Resource Manager-module naast de Azure-module. Deze nieuwe module kunt u om een script van de implementatie van resourcegroepen.

Zie voor meer informatie, [met behulp van Azure PowerShell met Azure Resource Manager](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure Resource Explorer
Dit [voorbeeldfunctie](https://resources.azure.com) kunt u de JSON-definities van de resourcegroepen in uw abonnement en de afzonderlijke resources ontdekken. In het hulpprogramma kunt u de JSON-definities van een resource bewerken, verwijderen van de gehele hiërarchie van resources en nieuwe bronnen maken.  De informatie die direct beschikbaar maken in dit hulpprogramma is zeer nuttig zijn voor het ontwerpen van een sjabloon omdat hier ziet u welke eigenschappen u wilt instellen voor een bepaald type resource, de juiste waarden, enzovoort. U kunt zelfs maken uw resourcegroep in de [Azure Portal](https://portal.azure.com/), Controleer de JSON-definities in de Verkenner kunt u sjablonen gebruiken voor de resourcegroep.

### <a name="deploy-to-azure-button"></a>Implementeren op Azure-knop
Als u GitHub voor broncodebeheer, plaatst u een [implementeren op Azure-knop](https://azure.microsoft.com/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) in de Leesmij-bestand. MD, waarmee de implementatie van een gebruiksklare gebruikersinterface naar Azure. Terwijl u dit voor een eenvoudige app doen kunt, kunt u deze om in te schakelen voor het implementeren van een hele resourcegroep met het plaatsen van een bestand azuredeploy.json in de hoofdmap van de opslagplaats kunt uitbreiden. Deze JSON-bestand waarin de resourcegroepsjabloon, zal worden gebruikt door de implementeren op Azure-knop om de resourcegroep te maken. Zie voor een voorbeeld: de [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) voorbeeld, waarin u in deze zelfstudie.

## <a name="get-the-sample-resource-group-template"></a>De voorbeeld-resourcegroepsjabloon ophalen
Nu gaan we Ga meteen naar deze.

1. Navigeer naar de [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) voorbeeld van App Service.
2. Klik in het readme.md, **implementeren in Azure**.
3. Er wordt de [implementeren naar azure](https://deploy.azure.com) site en wordt gevraagd voor het invoeren van de implementatieparameters. U ziet dat de meeste van de velden zijn ingevuld met de naam van de opslagplaats en sommige willekeurige tekenreeksen voor u. U kunt alle velden wijzigen als u wilt, maar alleen wat u moet invoeren zijn de administratieve SQL Server-aanmelding en het wachtwoord en klik vervolgens op **volgende**.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)
4. Klik vervolgens op **implementeren** om het implementatieproces te starten. Nadat het proces uitgevoerd tot ze voltooid, klikt u op de http://todoapp *XXXX*. azurewebsites.net koppeling naar de geïmplementeerde toepassing bladeren. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)
   
   De gebruikersinterface zou een beetje traag worden als u eerst naar het niet doorzoeken, omdat de apps alleen worden gestart, maar zelf te overtuigen dat het is een volledig functionele toepassing.
5. Klik in de pagina implementeren op de **beheren** koppeling om te zien van de nieuwe toepassing in Azure Portal.
6. In de **Essentials** vervolgkeuzelijst, klik op de koppeling van de groep resource. U ziet ook dat de app al is verbonden met de GitHub-opslagplaats onder **extern Project**. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
7. Op de blade van de resourcegroep, houd er rekening mee dat er nog twee apps en een SQL-Database in de resourcegroep.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)

Alles wat u hebt zojuist gezien in een paar korte minuten is een volledig geïmplementeerde twee-microservice-toepassing, met alle onderdelen, afhankelijkheden, instellingen, database en continue publicatie, ingesteld door een geautomatiseerde in Azure Resource Manager-indeling. Dit is gedaan door twee dingen:

* De implementeren op Azure-knop
* azuredeploy.JSON in de hoofdmap van de opslagplaats

U kunt deze dezelfde toepassing implementeren tientallen, honderden of duizenden keren en exact dezelfde configuratie elke keer hebben. De herhaalbaarheid en de voorspelbaarheid van deze benadering kunt u zeer schaalbare toepassingen implementeren met gemak en vertrouwen.

## <a name="examine-or-edit-azuredeployjson"></a>Bekijk AZUREDEPLOY (of bewerken). JSON
Nu gaan we kijken hoe de GitHub-opslagplaats is ingesteld. U de JSON-editor gebruikt in de Azure .NET SDK, dus als u dit nog niet hebt geïnstalleerd [Azure .NET SDK 2.6](https://azure.microsoft.com/downloads/), dat nu doen.

1. Kloon de [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) opslagplaats met behulp van uw favoriete git-hulpprogramma. In de onderstaande schermafbeelding heb ik dit doen in Team Explorer in Visual Studio 2013.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)
2. Open in de hoofdmap van de opslagplaats, azuredeploy.json in Visual Studio. Als u het JSON-overzicht deelvenster niet ziet, moet u de Azure .NET SDK installeren.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Ik ga niet om te beschrijven van elk detail van de JSON-indeling, maar de [meer bronnen](#resources) sectie bevat koppelingen voor het leren van de taal van de resource-group-sjabloon. Ik ga hier alleen om weer te geven de interessante functies die u kunnen helpen bij het maken van uw eigen aangepaste sjabloon voor app-implementatie aan de slag.

### <a name="parameters"></a>Parameters
Raadpleeg de parametersectie om te zien dat de meeste van deze parameters wat zijn de **implementeren in Azure** knop vraagt u om in te voeren. De site achter de **implementeren in Azure** knop vult de invoer met behulp van de gedefinieerde parameters in azuredeploy.json gebruikersinterface. Deze parameters worden gebruikt in de resourcedefinities, zoals namen, eigenschapswaarden, enzovoort.

### <a name="resources"></a>Resources
In het knooppunt resources, kunt u zien dat 4 op het hoogste niveau resources zijn gedefinieerd, met inbegrip van een exemplaar van SQL Server, een App Service-plan en twee apps. 

#### <a name="app-service-plan"></a>App Service-plan
Laten we beginnen met een eenvoudige op hoofdniveau-resource in de JSON. Klik op de App Service-plan met de naam in de JSON-overzicht **[hostingPlanName]** markeren van de bijbehorende JSON-code. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Houd er rekening mee dat de `type` element geeft de tekenreeks voor een App Service-plan (het is wel een serverfarm een lang, lange tijd geleden), en andere elementen en de eigenschappen zijn ingevuld met behulp van de gedefinieerde parameters in het JSON-bestand, en deze resource niet beschikt over een genest resources.

> [!NOTE]
> U ziet ook dat de waarde van `apiVersion` vertelt Azure welke versie van de REST-API voor het gebruik van de resourcedefinitie JSON met en kan invloed hebben op hoe de bron moet worden geformatteerd in de `{}`. 
> 
> 

#### <a name="sql-server"></a>SQL Server
Klik op de SQL Server-resource met de naam **SQLServer** in de JSON-overzicht.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)

Houd rekening met de volgende met betrekking tot de gemarkeerde JSON-code:

* Het gebruik van parameters zorgt ervoor dat de gemaakte resources zijn met de naam en geconfigureerd op een begrijpelijke manier kunt u ze consistent zijn met elkaar.
* De SQL Server-resource heeft twee ingesloten resources, elk een andere waarde voor `type`.
* De resources van het geneste in `“resources”: […]`, waarbij de database en de firewall-regels zijn gedefinieerd, hebben een `dependsOn` element dat Hiermee geeft u de resource-ID van de resource van de SQL Server op hoofdniveau. Dit vertelt Azure Resource Manager 'voordat u deze resource, die andere resource moet al bestaan; maken en als die andere resource in de sjabloon is gedefinieerd, maakt u een eerste'.
  
  > [!NOTE]
  > Voor gedetailleerde informatie over het gebruik van de `resourceId()` functie, Zie [Azure Resource Manager-sjabloonfuncties](../azure-resource-manager/resource-group-template-functions-resource.md#resourceid).
  > 
  > 
* Het effect van de `dependsOn` -element is dat Azure Resource Manager weet welke resources parallel kunnen worden gemaakt en welke resources moeten opeenvolgend worden gemaakt. 

#### <a name="app-service-app"></a>App Service-app
Nu gaan we verder met de werkelijke apps zelf die gecompliceerder zijn. Klik op de app [variables('apiSiteName')] in de JSON-overzicht naar de JSON-code markeren. U zult zien dat dingen interessanter krijgen. Voor dit doel, moet ik meer vragen stellen over de functies die één voor één:

##### <a name="root-resource"></a>Basis-resource
De app, is afhankelijk van twee verschillende bronnen. Dit betekent dat Azure Resource Manager de app maakt alleen als zowel de App Service-plan en de SQL Server-exemplaar worden gemaakt.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>App-instellingen
De app-instellingen zijn ook gedefinieerd als een geneste resource.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

In de `properties` -element voor `config/appsettings`, hebt u twee app-instellingen in de indeling `"<name>" : "<value>"`.

* `PROJECT` is een [KUDU instelling](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) waarin Azure-implementatie welk project moet worden gebruikt in een Visual Studio-oplossing voor meerdere project staat. Ziet u hoe broncodebeheer is geconfigureerd, maar omdat de code ToDoApp zich in een Visual Studio-oplossing voor meerdere project, moet deze instelling.
* `clientUrl` is gewoon een app-instelling die de toepassingscode wordt gebruikt.

##### <a name="connection-strings"></a>Verbindingsreeksen
De verbindingsreeksen zijn ook gedefinieerd als een geneste resource.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

In de `properties` -element voor `config/connectionstrings`, elke verbindingsreeks ook is gedefinieerd als een combinatie van naam: waarde, met de specifieke indeling van `"<name>" : {"value": "…", "type": "…"}`. Voor de `type` -element, mogelijke waarden zijn `MySql`, `SQLServer`, `SQLAzure`, en `Custom`.

> [!TIP]
> Voer de volgende opdracht in Azure PowerShell voor een definitieve lijst van de tekenreeks verbindingstypen: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.websites.Services.WebEntities.DatabaseType")
> 
> 

##### <a name="source-control"></a>Broncodebeheer
De instellingen voor het beheer van bron zijn ook gedefinieerd als een geneste resource. Azure Resource Manager maakt gebruik van deze resource continue publicatie configureren (Zie voorbehoud op `IsManualIntegration` later) en ook voor een vliegende start de implementatie van toepassingscode automatisch tijdens de verwerking van het JSON-bestand.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl` en `branch` moet vrij intuïtief en moet verwijzen naar de Git-opslagplaats en de naam van de vertakking om te publiceren vanaf. Nogmaals, deze worden gedefinieerd door de invoerparameters die zijn opgegeven. 

Houd er rekening mee de `dependsOn` element dat naast de resource voor de app zelf, `sourcecontrols/web` ook afhankelijk van `config/appsettings` en `config/connectionstrings`. Dit komt doordat zodra `sourcecontrols/web` is geconfigureerd, het Azure-implementatie-proces wordt automatisch geprobeerd te implementeren, bouwen en start de toepassingscode. Daarom kunt u ervoor zorgen dat de toepassing toegang tot de vereiste app-instellingen en verbindingsreeksen heeft voordat de toepassingscode wordt uitgevoerd met deze afhankelijkheid invoegen. 

> [!NOTE]
> Merk ook op dat `IsManualIntegration` is ingesteld op `true`. Deze eigenschap is in deze zelfstudie nodig omdat u de GitHub-opslagplaats niet daadwerkelijk in bezit, en dus kan niet daadwerkelijk machtigingen voor Azure voor het configureren van continue publicatie vanuit verlenen [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) (dat wil zeggen push automatische opslagplaats updates voor Azure). U kunt de standaardwaarde `false` voor de opgegeven opslagplaats alleen als u eigenaar van de GitHub-referenties in hebt geconfigureerd de [Azure-portal](https://portal.azure.com/) voordat. Met andere woorden, als u hebt ingesteld met broncodebeheer naar GitHub of BitBucket voor een app in de [Azure Portal](https://portal.azure.com/) eerder, met behulp van de gebruiker referenties, en vervolgens Azure worden de referenties onthouden en deze gebruiken wanneer u een app vanuit implementeren GitHub of BitBucket in de toekomst. Echter, als u dit nog niet hebt gedaan, implementatie van de JSON-sjabloon mislukken wanneer Azure Resource Manager probeert te configureren van instellingen voor beheer van de app-bron, omdat deze niet kan op GitHub of BitBucket met de referenties van de eigenaar van de opslagplaats aanmelden.
> 
> 

## <a name="compare-the-json-template-with-deployed-resource-group"></a>De JSON-sjabloon met de geïmplementeerde resourcegroep vergelijken
Hier kunt u doornemen van de app-blades in de [Azure Portal](https://portal.azure.com/), maar er is een ander hulpmiddel dat is net zo kan handig zijn, als niet meer. Ga naar de [Azure Resource Explorer](https://resources.azure.com) voorbeeldfunctie, waardoor u een JSON-weergave van alle resourcegroepen in uw abonnementen, omdat ze daadwerkelijk in de Azure back-end bestaat. U kunt ook zien hoe de resourcegroep JSON hiërarchie in Azure komt overeen met de hiërarchie in de sjabloonbestand dat wordt gebruikt om deze te maken.

Bijvoorbeeld, als ik ga naar de [Azure Resource Explorer](https://resources.azure.com) hulpprogramma en vouw de knooppunten in de Verkenner, kan ik zien de resourcegroep en de op hoofdniveau-resources die worden verzameld onder hun respectievelijke resourcetypen.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Als u op een app inzoomen, zou het mogelijk voor app-configuratie-informatie die vergelijkbaar is met de onderstaande schermafbeelding:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Nogmaals, de ingesloten resources moeten een vergelijkbaar met die in uw JSON-sjabloonbestand hiërarchie hebt en ziet u de app-instellingen, verbindingsreeksen, enzovoort, juist weergegeven in het deelvenster met JSON. Instellingen hier kan duiden op een probleem met uw JSON-bestand en kunt u uw JSON-sjabloonbestand op te lossen.

## <a name="deploy-the-resource-group-template-yourself"></a>Implementatie van de resource-group-sjabloon
De **implementeren in Azure** knop is erg handig, maar kunt u de resource-groep om sjabloon te implementeren in azuredeploy.json alleen als u al azuredeploy.json naar GitHub hebt gepusht. De Azure .NET SDK biedt ook de hulpprogramma's voor het implementeren van een JSON-sjabloonbestand rechtstreeks vanuit uw lokale computer. U doet dit door de volgende stappen uit te voeren:

1. Klik in Visual Studio op **File** > **New** > **Project**.
2. Klik op **Visual C#** > **Cloud** > **Azure-resourcegroep**, klikt u vervolgens op **OK**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)
3. In **Azure-sjabloon selecteren**, selecteer **lege sjabloon** en klikt u op **OK**.
4. Sleep azuredeploy.json in de **sjabloon** map van het nieuwe project.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)
5. Open in Solution Explorer de gekopieerde azuredeploy.json.
6. Alleen voor de demonstratie, gaan we enkele standard Application Insights resources toevoegen aan onze JSON-bestand door te klikken op **Resource toevoegen**. Als u alleen geïnteresseerd bent in het JSON-bestand implementeren, gaat u naar de stappen implementeren.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)
7. Selecteer **Application Insights voor Web-Apps**, Controleer of een bestaande App Service-plan en een app is geselecteerd en klik vervolgens op **toevoegen**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)
   
   U gaat nu mogelijk om te zien van verschillende nieuwe resources die, afhankelijk van de bron en wat het doet, zijn afhankelijk van de App Service-plan of de app. Deze resources niet zijn ingeschakeld in hun bestaande definitie en u wilt wijzigen.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
8. Klik in de JSON-overzicht op **appInsights voor automatisch schalen** markeren van de JSON-code. Dit is de instelling vergroten/verkleinen voor uw App Service-plan.
9. Zoek in de gemarkeerde JSON-code, de `location` en `enabled` eigenschappen en stel ze in zoals hieronder wordt weergegeven.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)
10. Klik in de JSON-overzicht op **CPUHigh appInsights** markeren van de JSON-code. Dit is een waarschuwing.
11. Zoek de `location` en `isEnabled` eigenschappen en stel ze in zoals hieronder wordt weergegeven. Doe hetzelfde voor de andere drie waarschuwingen (paarse bollen).
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)
12. U bent nu klaar om te implementeren. Met de rechtermuisknop op het project en selecteer **implementeren** > **nieuwe implementatie**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)
13. Meld u aan bij uw Azure-account als u dat nog niet hebt gedaan.
14. Selecteer een bestaande resourcegroep in uw abonnement of maak een nieuwe één, selecteer **azuredeploy.json**, en klik vervolgens op **Parameters bewerken**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)
    
    U gaat nu moeten kunnen bewerken van de parameters die zijn gedefinieerd in het sjabloonbestand in een mooie tabel. Parameters die standaardwaarden definiëren beschikt al over de standaardwaarden en parameters die een lijst van toegestane waarden definieert worden weergegeven als de vervolgkeuzelijsten.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)
15. Vul de lege parameters en gebruik de [adres van de GitHub-opslagplaats voor ToDoApp](https://github.com/azure-appservice-samples/ToDoApp.git) in **repoUrl**. Klik vervolgens op **opslaan**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)
    
    > [!NOTE]
    > Automatisch schalen is een functie die wordt aangeboden in **Standard** laag of hoger, plan op het niveau en waarschuwingen zijn functies die worden aangeboden **Basic** laag of hoger gebruikt, moet u om in te stellen de **sku** parameter naar **Standard** of **Premium** als u wilt bekijken van al uw nieuwe App Insights-resources belicht.
    > 
    > 
16. Klik op **implementeren**. Als u hebt geselecteerd **wachtwoorden opslaan**, het wachtwoord wordt opgeslagen in het parameterbestand **in tekst zonder opmaak**. Anders wordt u gevraagd voor het invoeren van het wachtwoord voor de database tijdens het implementatieproces.

Dat is alles. Nu u alleen hoeft te gaat u naar de [Azure Portal](https://portal.azure.com/) en de [Azure Resource Explorer](https://resources.azure.com) hulpprogramma om te zien van de nieuwe waarschuwingen en de instellingen voor automatisch schalen is toegevoegd aan uw JSON-toepassing geïmplementeerd.

De stappen in deze sectie voornamelijk het volgende gedaan:

1. Het sjabloonbestand voorbereid
2. Een parameterbestand te gaan met het sjabloonbestand gemaakt
3. Het sjabloonbestand met het parameterbestand geïmplementeerd

De laatste stap is eenvoudig uitgevoerd door een PowerShell-cmdlet. Als u wilt zien wat Visual Studio gedaan wanneer uw toepassing is geïmplementeerd, opent u Scripts\Deploy AzureResourceGroup.ps1. Er is een veel code er echter alleen ik Markeer de relevante code moet u de sjabloonbestand met de parameterbestand implementeren.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

De laatste cmdlet `New-AzureResourceGroup`, is de sleutel die daadwerkelijk de actie uitvoert. Dit moet worden aangetoond dat u dat met behulp van hulpprogramma's, het is betrekkelijk eenvoudig implementeren van uw cloudtoepassing zoals verwacht. Telkens wanneer u de cmdlet op dezelfde sjabloon met de dezelfde parameter-bestand uitvoert, gaat u hetzelfde resultaat.

## <a name="summary"></a>Samenvatting
Zijn de sleutels voor een succesvolle implementatie van een zeer schaalbare toepassing die uit microservices bestaan in DevOps, herhaalbaarheid en voorspelbaarheid. In deze zelfstudie maakt hebt u een toepassing twee-microservices in Azure als één resourcegroep met de Azure Resource Manager-sjabloon geïmplementeerd. Het opgegeven Hopelijk heeft de kennis die u nodig hebt om te beginnen met het converteren van uw toepassing in Azure in een sjabloon en kunt inrichten en implementeren deze zoals verwacht. 

<a name="resources"></a>

## <a name="more-resources"></a>Meer bronnen
* [Taal van Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md)
* [Functies van Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-template-functions.md)
* [Een toepassing implementeren met Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure PowerShell gebruiken met Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)
* [Problemen met implementaties van de resourcegroep in Azure oplossen](../azure-resource-manager/resource-manager-common-deployment-errors.md)

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de JSON-syntaxis en de eigenschappen voor het woord brontypen geïmplementeerd in dit artikel, Zie:

* [Microsoft.Sql/servers](/azure/templates/microsoft.sql/servers)
* [Microsoft.Sql/servers/databases](/azure/templates/microsoft.sql/servers/databases)
* [Microsoft.Sql/servers/firewallRules](/azure/templates/microsoft.sql/servers/firewallrules)
* [Microsoft.Web/serverfarms](/azure/templates/microsoft.web/serverfarms)
* [Microsoft.Web/sites](/azure/templates/microsoft.web/sites)
* [Microsoft.Web/sites/slots](/azure/templates/microsoft.web/sites/slots)
* [Microsoft.Insights/autoscalesettings](/azure/templates/microsoft.insights/autoscalesettings)