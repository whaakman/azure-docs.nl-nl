---
title: Continue leveringsmethode voor cloud services met TFS in Azure | Microsoft Docs
description: Informatie over het instellen van continue leveringsmethode voor Azure cloud-apps. Codevoorbeelden voor opdrachtregelprogramma MSBuild-instructies en PowerShell-scripts.
services: cloud-services
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4f3c93c6-5c82-4779-9d19-7404a01e82ca
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/12/2017
ms.author: kraigb
ms.openlocfilehash: 0979722b9ec715e91825c7aba74657451df6e83f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Continue leveringsmethode voor Cloudservices in Azure
De procedure beschreven in dit artikel wordt beschreven hoe u voor het instellen van continue leveringsmethode voor Azure cloud-apps. Met dit proces kunt u automatisch pakketten maken en het pakket implementeren op Azure nadat de afzonderlijke code is ingecheckt. Het pakket buildproces beschreven in dit artikel is gelijk aan de **pakket** opdracht in Visual Studio en de publicatie stappen gelijk zijn aan de **publiceren** opdracht in Visual Studio.
Het artikel bevat informatie over de methoden die u gebruiken wilt voor het maken van een installatieserver opdrachtregelprogramma MSBuild-instructies en Windows PowerShell-scripts en ook laat zien hoe Configureer desgewenst Visual Studio Team Foundation Server - definities Team maken gebruik van de MSBuild-opdrachten en PowerShell-scripts. Het proces kan worden aangepast voor uw build-omgeving en de doel-Azure-omgevingen.

U kunt ook Visual Studio Team Services, een versie van TFS die wordt gehost in Azure, eenvoudiger hiervoor gebruiken. 

Voordat u begint, moet u uw toepassing vanuit Visual Studio publiceren.
Dit zorgt ervoor dat alle resources beschikbaar en geïnitialiseerd zijn wanneer u probeert om de publicatieproces te automatiseren.

## <a name="1-configure-the-build-server"></a>1: de Buildserver configureren
Voordat u een Azure-pakket maken kunt met behulp van MSBuild, moet u de vereiste software en hulpprogramma's installeren op de buildserver.

Visual Studio is niet vereist om te worden geïnstalleerd op de buildserver. Als u gebruiken van Team Foundation bouwen Service wilt voor het beheren van uw buildserver, volg de instructies in de [Team Foundation bouwen Service] [ Team Foundation Build Service] documentatie.

1. Installeer op de buildserver de [.NET Framework 4.5.2][.NET Framework 4.5.2], waaronder MSBuild.
2. Installeer de meest recente [Authoring hulpprogramma's van Azure voor .NET](https://azure.microsoft.com/develop/net/).
3. Installeer de [Azure-bibliotheken voor .NET](http://go.microsoft.com/fwlink/?LinkId=623519).
4. Kopieer het bestand Microsoft.WebApplication.targets van een installatie voor Visual Studio met de buildserver.

   Op een computer met Visual Studio is geïnstalleerd, wordt dit bestand zich in de map C:\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications. Kopieer deze naar dezelfde map op de buildserver.
5. Installeer de [Azure-hulpprogramma's voor Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: een pakket met MSBuild-opdrachten maken
Deze sectie beschrijft het maken van een MSBuild-opdracht die wordt gemaakt van een Azure-pakket. Deze stap uitvoeren op de server maken om te controleren of alles correct is geconfigureerd en dat de MSBuild-opdracht biedt u de gewenste te doen. U kunt met deze opdrachtregel voor installatie zonder toezicht toevoegen aan bestaande build scripts op de buildserver of kunt u de opdrachtregel in een definitie TFS bouwen, zoals beschreven in de volgende sectie. Zie voor meer informatie over opdrachtregelparameters en MSBuild [MSBuild opdrachtregel verwijzing](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

1. Als Visual Studio is geïnstalleerd op de buildserver, zoekt en kies **Visual Studio-opdrachtprompt** in de **Visual Studio Tools** map in Windows.

   Als Visual Studio niet is geïnstalleerd op de buildserver, open een opdrachtprompt en zorg ervoor dat MSBuild.exe toegankelijk is op het pad. MSBuild is geïnstalleerd met de .NET Framework in het pad % WINDIR %\\Microsoft.NET\\Framework\\*versie*. Bijvoorbeeld als u wilt toevoegen aan de omgevingsvariabele PATH MSBuild.exe wanneer u .NET Framework 4 geïnstalleerd hebt, typt u de volgende opdracht achter de opdrachtprompt:

       set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"
2. Navigeer naar de map met het Azure-project-bestand dat u wilt maken bij de opdrachtprompt.
3. MSBuild uitvoeren met de/target: publicatieoptie zoals in het volgende voorbeeld:

       MSBuild /target:Publish

   Deze optie kan worden afgekort tot /t: publiceren. De optie /t:Publish in MSBuild Verwar niet met de opdrachten publiceren in Visual Studio beschikbaar wanneer u de Azure-SDK geïnstalleerd hebt. De /t: optie alleen builds publiceren de Azure-pakketten. Dit biedt de pakketten niet implementeren als de opdrachten publiceren in Visual Studio.

   U kunt eventueel de projectnaam opgeven als een MSBuild-parameter. Als niet wordt opgegeven, wordt de huidige map gebruikt. Zie voor meer informatie over de opdrachtregelopties MSBuild [MSBuild opdrachtregel verwijzing](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).
4. Zoek de uitvoer. Met deze opdracht maakt standaard een map ten opzichte van de hoofdmap voor het project, zoals *ProjectDir*\\bin\\*configuratie*\\app.publish \\. Wanneer u een Azure-project maakt, genereert u twee bestanden: het pakketbestand zelf en het bijbehorende configuratiebestand:

   * Project.cspkg
   * Serviceconfiguration zijn. *TargetProfile*.cscfg

   Standaard elke Azure-project bevat één serviceconfiguratiebestand (.cscfg-bestand) voor de lokale (foutopsporing) builds en een andere voor builds cloud (fasering of productie), maar u kunt toevoegen of verwijderen van service configuratiebestanden zo nodig. Wanneer u een pakket vanuit Visual Studio maakt, wordt u gevraagd welke serviceconfiguratiebestand om op te nemen samen met het pakket.
5. Geef het configuratiebestand van de service. Als u een pakket met behulp van MSBuild bouwen, wordt het configuratiebestand van de lokale service standaard opgenomen. Als u wilt opnemen in een andere service-configuratiebestand, stelt u de eigenschap TargetProfile de MSBuild-opdracht:

       MSBuild /t:Publish /p:TargetProfile=Cloud
6. Geef de locatie voor de uitvoer. Het pad worden ingesteld met behulp van de /p:PublishDir =*Directory* \\ optie, met inbegrip van het afsluitende backslash scheidingsteken, zoals in het volgende voorbeeld:

       MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

   Nadat u hebt gemaakt en een juiste MSBuild-opdrachtregel voor het bouwen van uw projecten en ze combineren in een Azure-pakket getest, kunt u met deze opdrachtregel toevoegen aan uw build-scripts. Als uw buildserver gebruikmaakt van aangepaste scripts, wordt dit proces afhankelijk van de details van uw aangepaste buildproces. Als u TFS gebruikt als een build-omgeving, kunt u de instructies in de volgende stap de Azure-pakket build toevoegen aan uw buildproces volgen.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: bouwen van een pakket met TFS-Team maken
Als u hebt Team Foundation Server (TFS) ingesteld als een controller bouwen en de buildserver instellen als een TFS-build-machine en u kunt desgewenst een geautomatiseerde build instellen voor uw Azure-pakket op. Zie voor meer informatie over het instellen en Team Foundation server gebruiken als een build-systeem [Scale-out uw build-systeem][Scale out your build system]. Met name de volgende procedure wordt ervan uitgegaan dat u uw buildserver hebt geconfigureerd zoals beschreven in [implementeren en configureren van een buildserver][Deploy and configure a build server], en dat u hebt gemaakt dat een teamproject gemaakt een cloud Service-project in het teamproject.

Voer de volgende stappen uit voor het configureren van TFS om samen te stellen Azure pakketten:

1. Kies in Visual Studio op uw ontwikkelcomputer, in het menu Beeld **Team Explorer**, of kies Ctrl +\\, Ctrl + M. Vouw in het Team Explorer-venster, de **Builds** knooppunt of kies de **Builds** pagina en kies **nieuwe bouwen definitie**.

   ![Nieuwe bouwen definitie-optie][0]
2. Kies de **Trigger** tabblad en geef de gewenste voorwaarden voor als u wilt dat het pakket kunnen worden gebouwd. Geef bijvoorbeeld **continue integratie** optreedt voor het bouwen van het pakket wanneer een bron inchecken beheren.
3. Kies de **broninstellingen** tabblad en zorg ervoor dat de projectmap wordt vermeld in de **bronmap van het besturingselement** kolom en de status **Active**.
4. Kies de **bouwen standaard** tabblad en controleer of de naam van de buildserver onder Build-controller.  Ook de optie **kopie bouwen uitvoer voor de volgende doelmap** en geef de gewenste doellocatie.
5. Kies de **proces** tabblad. Kies op het tabblad proces de standaardsjabloon onder **bouwen**, kiest u het project als deze nog niet is geselecteerd, en vouw de **Geavanceerd** sectie het **bouwen** sectie van het raster.
6. Kies **MSBuild-argumenten**, en stel de juiste MSBuild-opdrachtregelargumenten, zoals beschreven in stap 2 hierboven. Voer bijvoorbeeld **/t: publiceren /p:PublishDir =\\\\MijnServer\\zakt\\**  het bouwen van een pakket en kopieer de pakketbestanden naar de locatie \\ \\ MijnServer\\zakt\\:

   ![MSBuild-argumenten][2]

   > [!NOTE]
   > De bestanden zijn gekopieerd naar een openbare share gemakkelijker handmatig implementeren van de pakketten van uw ontwikkelcomputer.
7. Het succes van uw build-stap testen door te controleren in een wijziging in uw project, of een nieuwe build in de wachtrij. Als u wilt een nieuwe build, in de Explorer-Team in de wachtrij met de rechtermuisknop op **alle bouwen definities** en kies vervolgens **wachtrij nieuwe bouwen**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: een pakket met een PowerShell-Script publiceren
Deze sectie wordt beschreven hoe een Windows PowerShell-script die de uitvoer van de Cloud-app-pakket worden gepubliceerd naar Azure met optionele parameters. Dit script kan worden aangeroepen na de build stap in uw aangepaste build automation. Het kan ook worden aangeroepen vanuit de werkstroomactiviteiten processjabloon in Visual Studio TFS Team bouwen.

1. Installeer de [Azure PowerShell-cmdlets] [ Azure PowerShell cmdlets] (v0.6.1 of hoger).
   Kiezen tijdens de installatiefase cmdlet te installeren als een module. Houd er rekening mee dat deze officieel ondersteunde versie wordt vervangen door de oudere versie die worden aangeboden via CodePlex, hoewel de vorige versies 2.x.x nummer.
2. Azure PowerShell via het menu Start begint of pagina. Als u op deze manier wordt gestart, wordt de Azure PowerShell-cmdlets worden geladen.
3. Controleren of de PowerShell-cmdlets zijn geladen met de gedeeltelijke opdracht achter de PowerShell-prompt `Get-Azure` en drukt u op de Tab-toets voor de instructie was voltooid.

   Als u herhaaldelijk op de Tab-toets drukt, ziet u diverse Azure PowerShell-opdrachten.
4. Controleer of dat u verbinding met uw Azure-abonnement maken kunt met uw abonnementsgegevens importeren uit de .publishsettings-bestand.

   `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

   Voer de opdracht

   `Get-AzureSubscription`

   Dit toont informatie over uw abonnement. Controleer of alles juist is.
5. Sla de script-sjabloon opgegeven aan het einde van dit artikel om uw scriptmap als c:\\scripts\\WindowsAzure\\**PublishCloudService.ps1**.
6. Bekijk het gedeelte parameters van het script. Toevoegen of wijzigen van de standaardwaarden. Deze waarden kunnen altijd worden genegeerd door door te geven in expliciete parameters.
7. Zorg dat er geldige cloud service- en storage accounts worden gemaakt in uw abonnement die kan worden gericht door het script publiceren. Het opslagaccount (blobopslag) wordt gebruikt om te uploaden en de implementatie van pakket en config-bestand tijdelijk op te slaan tijdens de implementatie wordt gemaakt.

   * Voor het maken van een nieuwe cloudservice, kunt u dit script aanroepen of de [Azure-portal](https://portal.azure.com). De naam van de cloud-service wordt gebruikt als een voorvoegsel in een volledig gekwalificeerde domeinnaam en daarom moet uniek zijn.

         New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
   * Als u wilt een nieuw opslagaccount maakt, kunt u dit script aanroepen of de [Azure-portal](https://portal.azure.com). Naam van het opslagaccount wordt gebruikt als een voorvoegsel in een volledig gekwalificeerde domeinnaam en daarom moet uniek zijn. U kunt met dezelfde naam als de cloudservice.

         New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
8. Aanroepen van het script rechtstreeks vanuit de Azure PowerShell, of op wire dit script op uw host build automation na het opbouwen van het pakket.

   > [!IMPORTANT]
   > Het script wordt altijd Verwijder of vervang de bestaande implementaties standaard als ze worden gedetecteerd. Dit is nodig om in te schakelen continue van automatisering geen gebruiker wordt gevraagd wanneer mogelijk is.
   >
   >

   **Voorbeeldscenario 1:** continue implementatie bij de faseringsomgeving van een service:

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   Dit is doorgaans opgevolgd door testuitvoering verificatie en geen VIP's wisselen. Het wisselen van VIP kan worden uitgevoerd via de [Azure-portal](https://portal.azure.com) of met behulp van de cmdlet Move-implementatie.

   **Voorbeeldscenario 2:** continue implementatie naar de productieomgeving van een speciale test-service

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   **Extern bureaublad:**

   Als Extern bureaublad is ingeschakeld in uw Azure-project u aanvullende eenmalige stappen moet om te controleren of dat de juiste Cloud Service-certificaat is geüpload naar alle cloudservices waarop dit script uitvoeren.

   Zoek de certificaat vingerafdruk waarden door uw rollen wordt verwacht. De vingerafdruk-waarden zijn zichtbaar in de sectie certificaten van het configuratiebestand van de cloud (dat wil zeggen ServiceConfiguration.Cloud.cscfg). Het is ook zichtbaar in het dialoogvenster configuratie van extern bureaublad in Visual Studio wanneer u opties en bekijk het geselecteerde certificaat.

       <Certificates>
             <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
       </Certificates>

   Extern bureaublad-certificaten uploaden als een eenmalige instellingsstap met behulp van de volgende cmdlet-script:

       Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

   Bijvoorbeeld:

       Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

   U kunt ook het certificaatbestand PFX met persoonlijke sleutel exporteren en certificaten uploaden naar elk doel cloud service met behulp van de [Azure-portal](https://portal.azure.com).

   <!---
   Fixing broken links for Azure content migration from ACOM to DOCS. I'm unable to find a replacement links, so I'm commenting out this reference for now. The author can investigate in the future. "Read the following article to learn more: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx.
   -->
   **Upgraden versus van de implementatie. Verwijderen van implementatie -\> nieuwe implementatie**

   Het script wordt standaard uitvoeren een Upgrade-implementatie ($enableDeploymentUpgrade = 1) als er is geen parameter is doorgegeven, of de waarde 1 expliciet wordt doorgegeven. Dit heeft het voordeel van het minder tijd nodig dan een volledige implementatie voor enkele exemplaren. Voor exemplaren waarvoor hoge beschikbaarheid, dit ook het voordeel heeft van het verlaten van sommige gevallen terwijl anderen kunnen worden bijgewerkt (roulatie van uw updatedomein), plus de VIP wordt niet verwijderd.

   Upgrade-implementatie kan worden uitgeschakeld in het script ($enableDeploymentUpgrade = 0) of door het doorgeven van *- enableDeploymentUpgrade 0* als een parameter die het gedrag van het script moet eerst alle bestaande implementatie verwijderen en maak vervolgens een nieuwe wordt gewijzigd de implementatie.

   > [!IMPORTANT]
   > Het script wordt altijd Verwijder of vervang de bestaande implementaties standaard als ze worden gedetecteerd. Dit is nodig om in te schakelen continue van automatisering wanneer er geen gebruiker/operator vragen mogelijk is.
   >
   >

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: een pakket met behulp van TFS-Team bouwen publiceren
Deze optionele stap maakt verbinding met TFS-Team bouwen aan het script dat is gemaakt in stap 4, die het publiceren van de build van het pakket naar Azure verwerkt. Dit houdt in dat de sjabloon die wordt gebruikt door de definitie van de build zodat deze wordt uitgevoerd een activiteit publiceren aan het einde van de werkstroom wijzigen. De activiteit publiceren wordt uw PowerShell-opdracht wordt doorgegeven in parameters van de build uitgevoerd. Uitvoer van de MSBuild-doelen en publiceren van script zal worden doorgesluisd naar de standaard build-uitvoer.

1. Bewerk de definitie bouwen die verantwoordelijk is voor continue implementeren.
2. Selecteer de **proces** tabblad.
3. Ga als volgt [deze instructies](http://msdn.microsoft.com/library/dd647551.aspx) om toe te voegen in een activiteit-project voor de build processjabloon, downloaden de standaardsjabloon, toe te voegen aan het project en inchecken in. Geeft de build processjabloon een nieuwe naam, zoals AzureBuildProcessTemplate.
4. Ga terug naar de **proces** tabblad en gebruik **Details weergeven** om een lijst met beschikbare build processjablonen weer te geven. Kies de **nieuwe...**  knop en navigeer naar het project dat u zojuist hebt toegevoegd en ingecheckt. Zoek de sjabloon die u zojuist hebt gemaakt en kies **OK**.
5. Open de geselecteerde sjabloon proces voor het bewerken. U kunt rechtstreeks in de Workflow designer of in de XML-editor om te werken met de XAML openen.
6. De volgende lijst met nieuwe argumenten toevoegen als afzonderlijke regels op het tabblad argumenten van de workflow designer. Alle argumenten moet richting = In en typ = tekenreeks. Deze wordt gebruikt voor parameters van de stroom van de definitie van de build in de werkstroom, die vervolgens gebruikt ophalen voor het aanroepen van het script publiceren.

       SubscriptionName
       StorageAccountName
       CloudConfigLocation
       PackageLocation
       Environment
       SubscriptionDataFileLocation
       PublishScriptLocation
       ServiceName

   ![Lijst met argumenten][3]

   De bijbehorende XAML ziet er als volgt:

       <Activity  _ />
         <x:Members>
           <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
           <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
           <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
           <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
           <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
           <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
           <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
           <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
           <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
           <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
           <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
           <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
           <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
           <x:Property Name="GetVersion" Type="InArgument(x:String)" />
           <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
           <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
           <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
           <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
           <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
           <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
           <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
           <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
           <x:Property Name="Environment" Type="InArgument(x:String)" />
           <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
           <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
           <x:Property Name="ServiceName" Type="InArgument(x:String)" />
         </x:Members>

         <this:Process.MSBuildArguments>
7. Een nieuwe reeks toevoegen aan het einde van de Agent uitvoeren op:

   1. Start op het toevoegen van een activiteit If-instructie om te controleren op een geldige scriptbestand. De voorwaarde op deze waarde wordt ingesteld:

          Not String.IsNullOrEmpty(PublishScriptLocation)
   2. Wanneer de vervolgens van de instructie als een nieuwe Sequence-activiteit toevoegen. De weergavenaam 'Start publiceren' instellen
   3. Met het begin publiceren reeks nog steeds is geselecteerd, voeg de volgende lijst met nieuwe variabelen als afzonderlijke regels op het tabblad variabelen van de workflow designer. Alle variabelen moet type variabele = tekenreeks en bereik = Start publiceren. Deze wordt gebruikt voor parameters van de stroom van de definitie van de build in de werkstroom, die vervolgens gebruikt ophalen voor het aanroepen van het script publiceren.

      * SubscriptionDataFilePath van het type tekenreeks
      * PublishScriptFilePath van het type tekenreeks

        ![Nieuwe variabelen][4]
   4. Als u TFS 2012 of ouder gebruikt, moet u een ConvertWorkspaceItem-activiteit toevoegen aan het begin van de nieuwe volgorde. Als u TFS 2013 of later gebruikt, moet u een GetLocalPath-activiteit toevoegen aan het begin van de nieuwe volgorde. Voor een ConvertWorkspaceItem als volgt de eigenschappen instellen: richting = ServerToLocal, DisplayName = 'Converteren publiceren script filename', invoer = PublishScriptLocation, resultaat = PublishScriptFilePath, werkruimte = 'Werkruimte'. Stel de eigenschap IncomingPath naar 'PublishScriptLocation' en het resultaat 'PublishScriptFilePath' voor een activiteit GetLocalPath. Deze activiteit zet het pad naar het script publiceren van TFS-serverlocaties (indien van toepassing) naar een pad standaard lokale schijf.
   5. Als u TFS 2012 of ouder gebruikt, moet u een andere ConvertWorkspaceItem activiteit toevoegen aan het einde van de nieuwe volgorde. Richting = ServerToLocal, DisplayName = 'Converteren abonnement filename', invoer = SubscriptionDataFileLocation, resultaat = SubscriptionDataFilePath, werkruimte = 'Werkruimte'. Als u TFS 2013 of later gebruikt, voegt u een andere GetLocalPath. IncomingPath = 'SubscriptionDataFileLocation' en resultaat = 'SubscriptionDataFilePath'.
   6. Een activiteit InvokeProcess toevoegen aan het einde van de nieuwe volgorde.
      Deze activiteit roept PowerShell.exe met de argumenten doorgegeven aan de definitie bouwen.

      + Argumenten String.Format = ('-'' {0}' ' - serviceName {1} - storageAccountName {2} - packageLocation '' {3}' ' - cloudConfigLocation '' {4}' ' - subscriptionDataFile '' {5}' ' - selectedSubscription {6} bestand-omgeving '' {7}' ' ",  PublishScriptFilePath, ServiceName, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, SubscriptionName, omgeving)
      + DisplayName = Execute script publiceren
      + FileName = 'PowerShell' (inclusief de aanhalingstekens)
      + OutputEncoding System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage) =
   7. In de **standaarduitvoer verwerken** sectie textbox van de InvokeProcess, stelt u de waarde van het tekstvak naar 'data'. Dit is een variabele voor het opslaan van de standaarduitvoer.
   8. Toevoegen van een activiteit WriteBuildMessage NET hieronder de **standaarduitvoer verwerken** sectie. De urgentie = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' en het bericht = 'data'. Hierdoor worden de standaarduitvoer van het script wordt weggeschreven naar de builduitvoer.
   9. In de **foutuitvoer verwerken** sectie textbox van de InvokeProcess, stelt u de waarde van het tekstvak naar 'data'. Dit is een variabele voor het opslaan van de standaardfout-gegevens.
   10. Toevoegen van een activiteit WriteBuildError NET hieronder de **foutuitvoer verwerken** sectie. Het bericht instellen = 'data'. Hierdoor worden dat de standaard-fouten van het script wordt weggeschreven naar de uitvoer van de build-fout.
   11. Corrigeer eventuele fouten, aangegeven door blauw uitroepteken aanhalingstekens. Beweeg de muisaanwijzer over de merken uitroepteken ophalen van een aanwijzing over de fout. De workflow om te wissen fouten niet opslaan.

   In de ontwerpfunctie voor er het uiteindelijke resultaat van de werkstroomactiviteiten publiceren als volgt:

   ![Workflow-activiteiten][5]

   Het uiteindelijke resultaat van de werkstroomactiviteiten publiceren ziet er als volgt in XAML:

       <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
           <If.Then>
             <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
               <Sequence.Variables>
                 <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                 <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
               </Sequence.Variables>
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
               <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                 <mtbwa:InvokeProcess.ErrorDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.ErrorDataReceived>
                 <mtbwa:InvokeProcess.OutputDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.OutputDataReceived>
               </mtbwa:InvokeProcess>
             </Sequence>
           </If.Then>
         </If>
       </Sequence>
8. De werkstroom van sjabloon build en controleren In dit bestand opslaan.
9. Bewerk de build-definitie (sluit als het al geopend is), en selecteer de **nieuw** knop als u de nieuwe sjabloon in de lijst met processjablonen niet nog ziet.
10. De parameter eigenschapswaarden in de sectie div als volgt instellen:

    1. CloudConfigLocation ='c:\\zakt\\app.publish\\ServiceConfiguration.Cloud.cscfg' *deze waarde is afgeleid van: ($PublishDir)ServiceConfiguration.Cloud.cscfg*
    2. PackageLocation = ' c:\\zakt\\app.publish\\ContactManager.Azure.cspkg' *deze waarde is afgeleid van: ($PublishDir)($ProjectName) .cspkg*
    3. PublishScriptLocation = ' c:\\scripts\\WindowsAzure\\PublishCloudService.ps1'
    4. ServiceName = 'mycloudservicename' *hier de naam van de juiste cloud-service gebruiken*
    5. Omgeving = 'Fasering'
    6. StorageAccountName = 'mystorageaccountname' *hier de naam van het juiste opslagaccount gebruiken*
    7. SubscriptionDataFileLocation = ' c:\\scripts\\WindowsAzure\\Subscription.xml'
    8. SubscriptionName = 'default'

    ![Eigenschap parameterwaarden][6]
11. Sla de wijzigingen aan de definitie bouwen.
12. Een Build voor het uitvoeren van zowel de pakket-build en publiceren van de verzendwachtrij. Als u een trigger die is ingesteld op doorlopende integratie hebt, kunt u dit gedrag wordt uitgevoerd bij elke controle.

### <a name="publishcloudserviceps1-script-template"></a>PublishCloudService.ps1 script sjabloon
```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Volgende stappen
Zie inschakelen foutopsporing op afstand bij gebruik van doorlopende levering [foutopsporing op afstand inschakelen wanneer publiceren naar Azure met behulp van continue levering](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

[Team Foundation Build Service]: https://msdn.microsoft.com/library/ee259687.aspx
[.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
[.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
[.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
[Scale out your build system]: https://msdn.microsoft.com/library/dd793166.aspx
[Deploy and configure a build server]: https://msdn.microsoft.com/library/ms181712.aspx
[Azure PowerShell cmdlets]: /powershell/azureps-cmdlets-docs
[the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
[0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
[2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
[3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
[4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
[5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
[6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
