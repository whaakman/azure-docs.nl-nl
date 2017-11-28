---
title: Windows PowerShell-Scripts gebruiken om te publiceren naar de ontwikkeling en testomgevingen | Microsoft Docs
description: Informatie over het Windows PowerShell-scripts vanuit Visual Studio gebruiken om te publiceren naar de ontwikkeling en testen van omgevingen.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5fff1301-5469-4d97-be88-c85c30f837c1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 4e9409aac836a60e7ea01261840c084ff09e954e
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a>Windows PowerShell-scripts gebruiken om ontwikkel- en testomgevingen te publiceren

Wanneer u een webtoepassing in Visual Studio maakt, kunt u een Windows PowerShell-script dat u later gebruiken kunt voor het automatiseren van de publicatie van uw website naar Azure als een Web-App in Azure App Service- of een virtuele machine genereren. U kunt bewerken en de Windows PowerShell-script in de Visual Studio-editor om te voldoen aan uw vereisten uitbreiden of het script integreren met bestaande bouwen, testen en publiceren van scripts.

Deze scripts kunt u aangepaste versies (ook wel bekend als dev- en testomgevingen) van uw site voor tijdelijk gebruik inrichten. U kunt bijvoorbeeld een bepaalde versie van uw website op een virtuele machine van Azure of op de faseringssleuf op een website op een test-suite uitvoeren, reproduceren een bug, een bug correctie proefversie test een voorgestelde wijziging of instellen van een aangepaste omgeving voor een demo of presentatie instellen. Nadat u een script dat uw project publiceert hebt gemaakt, kunt u identieke omgevingen opnieuw door het script opnieuw naar behoefte of het script uitgevoerd met uw eigen build van uw webtoepassing te maken van een aangepaste omgeving voor het testen.

## <a name="prerequisites"></a>Vereisten

* Azure SDK 2.3 of hoger. Zie [Visual Studio-Downloads](http://go.microsoft.com/fwlink/?LinkID=624384). (U hoeft niet de Azure SDK met de scripts voor webprojecten genereren. Deze functie is voor webprojecten, niet webrollen in cloudservices.)
* Azure PowerShell 0.7.4 of hoger. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview).
* [Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) of hoger.

## <a name="additional-tools"></a>Extra hulpprogramma 's

Extra hulpprogramma's en bronnen voor het werken met PowerShell in Visual Studio voor het ontwikkelen van Azure zijn beschikbaar. Zie [PowerShell-Tools voor Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-the-publish-scripts"></a>Genereren van de scripts publiceren

U kunt de scripts publiceren voor een virtuele machine die als host fungeert voor uw website wanneer u een nieuw project met de volgende maakt genereren [deze instructies](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). U kunt ook [genereren van scripts voor web-apps publiceren in Azure App Service](app-service/app-service-web-get-started-dotnet.md).

## <a name="scripts-that-visual-studio-generates"></a>Scripts die door Visual Studio gegenereerd

Visual Studio-oplossing niveau map met de naam gegenereerd **PublishScripts** die bevat twee bestanden van Windows PowerShell, een script publiceren voor de virtuele machine of website en een module die functies die u kunt gebruiken bevat in de scripts. Visual Studio gegenereerd ook een bestand in de JSON-indeling die Hiermee geeft u de details van het project dat u implementeert.

### <a name="windows-powershell-publish-script"></a>Windows PowerShell publiceren script

Het script publiceren bevat specifieke Publiceren stappen voor het implementeren van een website of virtuele machine. Visual Studio biedt syntaxis kleuren voor de ontwikkeling van Windows PowerShell. Help voor de functies die beschikbaar is en u de functies in het script wilt aanpassen aan uw veranderende vereisten vrijelijk kunt bewerken.

### <a name="windows-powershell-module"></a>Windows PowerShell-module

De Windows PowerShell-module die Visual Studio gegenereerd bevat functies die gebruikmaakt van het script publiceren. Deze Azure PowerShell-functies zijn niet bedoeld om te worden gewijzigd. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview).

### <a name="json-configuration-file"></a>JSON-configuratiebestand
Het JSON-bestand wordt gemaakt in de **configuraties** map en bevat configuratiegegevens die Hiermee geeft u precies welke resources te implementeren in Azure. De naam van het bestand dat door Visual Studio gegenereerd is project-naam-WAWS-dev.json als u een website of project de naam van VM dev.json gemaakt als u een virtuele machine hebt gemaakt. Hier volgt een voorbeeld van een JSON-configuratiebestand die wordt gegenereerd wanneer u een website hebt gemaakt. De meeste van de waarden behoeven geen uitleg. Naam van de website wordt gegenereerd door Azure, zodat deze mogelijk niet overeen met de projectnaam van uw.

```json
{
    "environmentSettings": {
        "webSite": {
            "name": "WebApplication26632",
            "location": "West US"
        },
        "databases": [{
            "connectionStringName": "DefaultConnection",
            "databaseName": "WebApplication26632_db",
            "serverName": "YourDatabaseServerName",
            "user": "sqluser2",
            "password": "",
            "edition": "",
            "size": "",
            "collation": "",
            "location": "West US"
        }]
    }
}
```

Wanneer u een virtuele machine maakt, wordt het JSON-configuratiebestand lijkt op het volgende. Een cloudservice is gemaakt als een container voor de virtuele machine. De virtuele machine bevat de gebruikelijke eindpunten voor internettoegang via HTTP en HTTPS, evenals de eindpunten voor Web Deploy waarmee u vanuit uw lokale computer, extern bureaublad en Windows PowerShell publiceren naar de website.

```json
{
    "environmentSettings": {
        "cloudService": {
            "name": "myusernamevm1",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myusernamevm1",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [{
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [{
            "connectionStringName": "",
            "databaseName": "",
            "serverName": "",
            "user": "",
            "password": ""
        }],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

U kunt de JSON-configuratie om te wijzigen wat er gebeurt wanneer het uitvoeren van scripts publiceren bewerken. De `cloudService` en `virtualMachine` secties zijn vereist, maar u kunt verwijderen de `databases` sectie als u deze niet nodig. De eigenschappen die zijn leeg in het configuratiebestand dat Visual Studio gegenereerd zijn optioneel. Deze eigenschappen met waarden in het configuratiebestand zijn vereist.

Als u een website met meerdere implementatieomgevingen (bekend als sleuven) in plaats van een productiesite één in Azure hebt, kunt u de naam van de site op de naam van de website kunt opnemen in het JSON-configuratiebestand. Bijvoorbeeld, als er een website met de naam **persoonlijke site** en een site voor deze met de naam **testen** dan is de URI `mysite-test.cloudapp.net`, maar de juiste naam te gebruiken in het configuratiebestand is mysite(test). U kunt dit als de website alleen doen en sleuven bestaat al in uw abonnement. Als deze nog niet bestaan, maakt u de website door het script zonder op te geven van de sleuf, maak vervolgens de sleuf in de [Azure-portal](https://portal.azure.com/), en daarna het script uitgevoerd met de naam van de aangepaste website. Zie voor meer informatie over implementatiesites voor web-apps, [faseringsomgevingen voor web-apps in Azure App Service instellen](app-service/web-sites-staged-publishing.md).

## <a name="how-to-run-the-publish-scripts"></a>Het uitvoeren van de scripts publiceren

Als u een Windows PowerShell-script voordat nooit hebt uitgevoerd, moet u eerst het uitvoeringsbeleid voor het uitvoeren van scripts inschakelen instellen. Het beleid is een beveiligingsfunctie om te voorkomen dat gebruikers Windows PowerShell-scripts uitgevoerd als ze kwetsbaar voor schadelijke software en virussen die betrekking hebben op het uitvoeren van scripts.

### <a name="run-the-script"></a>Het script uitvoeren

1. Het Web Deploy-pakket voor uw project maken. Een Web Deploy-pakket is een gecomprimeerd archief (ZIP-bestand) met bestanden die u wilt kopiëren naar uw website of virtuele machine. U kunt Web Deploy-pakketten in Visual Studio maken voor een webtoepassing.

![Maken van webinhoud pakket implementeren](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Zie voor meer informatie [procedure: een Web-implementatiepakket maken in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx). U kunt ook het maken van uw pakket Web Deploy automatiseren zoals beschreven in [aanpassen en uitbreiden van de scripts[(#customizing-and-extending-publish-scripts) publiceren]

1. In **Solution Explorer**, opent u het snelmenu voor het script en kies vervolgens **openen met de PowerShell ISE**.
2. Als Windows PowerShell-scripts uitgevoerd op deze computer voor de eerste keer, open een opdrachtpromptvenster met Administrator-bevoegdheden en typ de volgende opdracht:

    ```powershell
    Set-ExecutionPolicy RemoteSigned
    ```

3. Aanmelden bij Azure met behulp van de volgende opdracht.

    ```powershell
    Add-AzureAccount
    ```

    Als u wordt gevraagd, geeft u uw gebruikersnaam en wachtwoord.

    Houd er rekening mee dat wanneer u het script automatiseren, deze methode van het bieden van Azure-referenties niet werkt. In plaats daarvan moet u de `.publishsettings` bestand referenties op te geven. Één keer alleen u de opdracht gebruiken **Get-AzurePublishSettingsFile** downloaden van het bestand in Azure, en daarna gebruiken **importeren AzurePublishSettingsFile** het bestand te importeren. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor gedetailleerde instructies.

4. (Optioneel) Als u maken van Azure-resources, zoals de virtuele machine wilt, database en een website zonder het publiceren van uw webtoepassing gebruikt het **publiceren WebApplication.ps1** opdracht met de **-configuratie**argument ingesteld op het JSON-configuratiebestand. Met deze opdrachtregel maakt gebruik van het JSON-configuratiebestand om te bepalen welke resources om te maken. Omdat deze gebruikmaakt van de standaardinstellingen voor andere opdrachtregelargumenten, maakt u de resources, maar uw webtoepassing niet publiceren. De uitgebreide optie vindt u meer informatie over wat er gebeurt.

    ```powershell
    Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json
    ```

5. Gebruik de **publiceren WebApplication.ps1** opdracht zoals weergegeven in een van de volgende voorbeelden voor het aanroepen van het script en publiceren van uw webtoepassing. Als u wilt overschrijven de standaardinstellingen voor een van de argumenten, zoals de naam van het abonnement, publiceren pakketnaam, de referenties van de virtuele machine of de referenties voor de database, kunt u deze parameters opgeven. Gebruik de **– uitgebreid** optie voor meer informatie over de voortgang van het publicatieproces.

    ```powershell
    Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
    –SubscriptionName Contoso `
    -WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
    -DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
    -Verbose
    ```

    Als u een virtuele machine maakt, wordt de opdracht ziet er als volgt. Dit voorbeeld toont ook de referenties voor meerdere databases opgeven. Voor de virtuele machines die deze scripts maken, is het SSL-certificaat niet uit een vertrouwde basiscertificeringsinstantie. Daarom moet u gebruiken de **– AllowUntrusted** optie.

    ```powershell
    Publish-WebApplication.ps1 `
    -Configuration C:\Path\ADVM-VM-test.json `
    -SubscriptionName Contoso `
    -WebDeployPackage C:\Path\ADVM.zip `
    -AllowUntrusted `
    -VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
    -DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
    -Verbose
    ```

    Het script databases kunt maken, maar het database-servers niet maken. Als u maken van een databaseserver wilt, kunt u de **nieuw AzureSqlDatabaseServer** functie in de Azure-module.

## <a name="customizing-and-extending-the-publish-scripts"></a>Aanpassen en uitbreiden van de scripts publiceren
U kunt het script voor publiceren en de JSON-configuratiebestand aanpassen. De functies in de Windows PowerShell-module **AzureWebAppPublishModule.psm1** zijn niet bedoeld om te worden gewijzigd. Als u alleen wilt opgeven van een andere database of enkele van de eigenschappen van de virtuele machine wijzigen, bewerkt u het JSON-configuratiebestand. Als u uitbreiden van de functies van het script wilt te maken en testen van uw project te automatiseren, kunt u de functie stubs in implementeren **publiceren WebApplication.ps1**.

Voor het automatiseren van uw project te bouwen, voeg code MSBuild naar roept `New-WebDeployPackage` zoals in dit codevoorbeeld. Het pad naar de MSBuild-opdracht is verschillend, afhankelijk van de versie van Visual Studio die u hebt geïnstalleerd. Als u het juiste pad, kunt u de functie **Get-MSBuildCmd**, zoals wordt weergegeven in dit voorbeeld.

### <a name="to-automate-building-your-project"></a>Voor het automatiseren van uw project te bouwen
1. Voeg de `$ProjectFile` parameter in de sectie globale param.

    ```powershell
    [Parameter(Mandatory = $false)]
    [ValidateScript({Test-Path $_ -PathType Leaf})]
    [String]
    $ProjectFile,
    ```

2. Kopieer de functie `Get-MSBuildCmd` in het scriptbestand.

    ```powershell
    function Get-MSBuildCmd
    {
            process
    {

                $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                    Sort-Object {[double]$_.PSChildName} -Descending |
                                    Select-Object -First 1 |
                                    Get-ItemProperty -Name MSBuildToolsPath |
                                    Select -ExpandProperty MSBuildToolsPath

                $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

            return Get-Item $path
        }
    }
    ```

3. Vervang `New-WebDeployPackage` met de volgende code en vervang de tijdelijke aanduidingen in het construeren van de regel `$msbuildCmd`. Deze code is voor Visual Studio 2017. Als u Visual Studio 2015 gebruikt, wijzigt u de **VisualStudioVersion** eigenschap `14.0` (`12.0` voor Visual Studio 2013).

    ```powershell
    function New-WebDeployPackage
    {
        #Write a function to build and package your web application
    ```

    Gebruik voor het bouwen van uw webtoepassing, MsBuild.exe. Zie voor informatie over MSBuild-Naslaggids op: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)

    ```powershell
    Write-VerboseWithTime 'Build-WebDeployPackage: Start'

    $msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=15.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory

    Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
    ```

### <a name="start-execution-of-the-build-command"></a>Uitvoering van de opdracht build starten

```powershell
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru

if ($job.ExitCode -ne 0) {
    throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName

#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName

#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir

Write-VerboseWithTime 'Build-WebDeployPackage: End'

return $WebDeployPackage
}
```

1. Roep de `New-WebDeployPackage` functie voordat deze regel: `$Config = Read-ConfigFile $Configuration` voor web-apps of `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` voor virtuele machines.

    ```powershell
    if($ProjectFile)
    {
    $WebDeployPackage = New-WebDeployPackage
    }
    ```

2. Aanroepen van het aangepaste script vanaf de opdrachtregel doorgegeven met de `$Project` argument, zoals in het volgende voorbeeld:

    ```powershell
    .\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
    -ProjectFile ..\WebApplication5\WebApplication5.csproj `
    -VMPassword @{Name="VMUser";Password="Test.123"} `
    -AllowUntrusted `
    -Verbose
    ```

    Voor het automatiseren van uw toepassing testen, voeg code toe aan `Test-WebApplication`. Zorg ervoor dat de regels in opmerking verwijderen **publiceren WebApplication.ps1** waar deze functies worden aangeroepen. Als u een implementatie niet opgeeft, kunt u handmatig bouwen van uw project met Visual Studio en voer het script publiceren om te publiceren naar Azure.

## <a name="publishing-function-summary"></a>Publishing functie samenvatting
Voor informatie over functies die u bij de opdrachtprompt van Windows PowerShell gebruiken kunt, gebruikt u de opdracht `Get-Help function-name`. De help bevat parameter help en voorbeelden. De dezelfde help-tekst is ook in de script-bronbestanden **AzureWebAppPublishModule.psm1** en **publiceren WebApplication.ps1**. Het script en de help worden in uw taal Visual Studio gelokaliseerd.

**AzureWebAppPublishModule**

| Functienaam | Beschrijving |
| --- | --- |
| Voeg AzureSQLDatabase |Maakt een nieuwe Azure SQL-database. |
| Voeg AzureSQLDatabases |Azure SQL-databases maakt van de waarden in het JSON-configuratiebestand dat Visual Studio gegenereerd. |
| -AzureVM |Azure een virtuele machine maakt en retourneert de URL van de geïmplementeerde virtuele machine. De functie stelt u de vereisten en roept vervolgens de **New-AzureVM** functie (Azure module) voor het maken van een nieuwe virtuele machine. |
| Voeg AzureVMEndpoints |Nieuwe invoereindpunten toegevoegd aan een virtuele machine en de virtuele machine met het nieuwe eindpunt retourneert. |
| Voeg AzureVMStorage |Maakt een nieuwe Azure storage-account in het huidige abonnement. De naam van het account begint met 'devtest' gevolgd door een unieke alfanumerieke tekenreeks. De functie retourneert de naam van het nieuwe opslagaccount. Geef een locatie of een affiniteitsgroep voor het nieuwe opslagaccount. |
| Voeg AzureWebsite |Hiermee maakt een website met de opgegeven naam en locatie. Deze functie roept de **nieuw AzureWebsite** functie in de Azure-module. Als het abonnement niet al een website met de opgegeven naam bevat, wordt deze functie wordt gemaakt van de website en een website-object geretourneerd. Anders is het resultaat `$null`. |
| Back-up-abonnement |Hiermee slaat u het huidige Azure-abonnement in de `$Script:originalSubscription` variabele in script-bereik. Deze functie slaat de huidige Azure-abonnement (verkregen met `Get-AzureSubscription -Current`) en de storage-account en het abonnement dat door dit script wordt gewijzigd (opgeslagen in de variabele `$UserSpecifiedSubscription`) en de opslagaccount in script-bereik. Als u de waarden opslaat, kunt u een functie, zoals `Restore-Subscription`, om te herstellen van de oorspronkelijke huidige abonnement en storage-account naar de huidige status als de huidige status is gewijzigd. |
| Zoek-AzureVM |Hiermee haalt u de opgegeven virtuele machine van Azure. |
| Indeling DevTestMessageWithTime |Voegt toe de datum en tijd aan een bericht. Deze functie is ontworpen voor berichten die naar de fout en uitgebreid stromen worden geschreven. |
| Get-AzureSQLDatabaseConnectionString |Ophaalprotocol een verbindingsreeks verbinding maken met een Azure SQL database. |
| Get-AzureVMStorage |Retourneert de naam van het eerste storage-account met het naampatroon ' devtest*' (hoofdlettergevoelig) in de opgegeven locatie of affiniteitsgroep. Als de "devtest*' storage-account komt niet overeen met de locatie of affiniteitsgroep, de functie negeert deze. Geef een locatie of een affiniteitsgroep. |
| Get-MSDeployCmd |Retourneert een opdracht voor het hulpprogramma MsDeploy.exe uitvoeren. |
| Nieuwe AzureVMEnvironment |Zoeken naar of maakt een virtuele machine in het abonnement dat overeenkomt met de waarden in het JSON-configuratiebestand. |
| Publiceren WebPackage |Maakt gebruik van MsDeploy.exe en een web-pakket niet publiceren. ZIP-bestand voor het implementeren van resources met een website. Deze functie biedt geen uitvoer gegenereerd. Als de aanroep van MSDeploy.exe mislukt, wordt de functie genereert een uitzondering. Als u meer gedetailleerde uitvoer, gebruikt de **-uitgebreide** optie. |
| Publiceren WebPackageToVM |Controleert of de parameterwaarden en roept vervolgens de **publiceren WebPackage** functie. |
| Lees ConfigFile |Valideert het JSON-configuratiebestand en retourneert een hashtabel met geselecteerde waarden. |
| Restore-abonnement |Hiermee stelt het huidige abonnement op het oorspronkelijke abonnement. |
| Test AzureModule |Retourneert `$true` als de versie van de geïnstalleerde Azure module 0.7.4 of hoger. Retourneert `$false` als de module is niet geïnstalleerd of een eerdere versie. Deze functie heeft geen parameters. |
| Test AzureModuleVersion |Retourneert `$true` als de versie van de Azure-module 0.7.4 is of hoger. Retourneert `$false` als de module is niet geïnstalleerd of een eerdere versie. Deze functie heeft geen parameters. |
| Test HttpsUrl |De invoer-URL wordt omgezet in een System.Uri-object. Retourneert `$True` als de URL absoluut is en het bijbehorende schema https is. Retourneert `$false` als de URL relatief is, het schema is niet HTTPS of de ingevoerde tekenreeks kan niet worden geconverteerd naar een URL. |
| Test-lid |Retourneert `$true` als een eigenschap of methode lid is van het object. Anders retourneert `$false`. |
| Schrijven ErrorWithTime |Schrijft een foutbericht dat wordt voorafgegaan door de huidige tijd. Deze functie roept de **indeling DevTestMessageWithTime** functie toevoegen van de tijd voordat het bericht schrijven naar de stroom van de fout aan. |
| Schrijven HostWithTime |Schrijft u een bericht naar het hostprogramma (**Write-Host**) voorafgegaan door de huidige tijd. Het effect van het schrijven naar het hostprogramma varieert. De meeste programma's die als host fungeren voor Windows PowerShell deze berichten schrijven naar de standaarduitvoer. |
| Schrijven VerboseWithTime |Schrijft een uitgebreid bericht voorafgegaan door de huidige tijd. Omdat deze wordt aangeroepen **Write-Verbose**, het bericht geeft alleen wanneer het script wordt uitgevoerd met de **uitgebreid** parameter of wanneer de **VerbosePreference** voorkeur is ingesteld op  **Doorgaan**. |

**Publiceren WebApplication**

| Functienaam | Beschrijving |
| --- | --- |
| Nieuwe AzureWebApplicationEnvironment |Azure-resources, zoals een website of virtuele machine maakt. |
| Nieuwe WebDeployPackage |Deze functie is niet geïmplementeerd. In deze functie om uw project te bouwen, kunt u opdrachten toevoegen. |
| Publiceren AzureWebApplication |Een webtoepassing naar Azure publiceert. |
| Publiceren WebApplication |Maakt en Web-Apps, virtuele machines, SQL-databases en storage-accounts voor een Visual Studio-webproject implementeert. |
| Test-WebApplication |Deze functie is niet geïmplementeerd. U kunt de opdrachten in deze functie voor het testen van uw toepassing toevoegen. |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over PowerShell-scripts door te lezen [met Windows PowerShell-scripts](https://technet.microsoft.com/library/bb978526.aspx) en Zie andere Azure PowerShell-scripts op de [Script Center](https://azure.microsoft.com/documentation/scripts/).
