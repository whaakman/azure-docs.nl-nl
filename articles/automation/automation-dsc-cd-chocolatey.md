---
title: Azure Automation State Configuration doorlopende implementatie met Chocolatey
description: DevOps continue implementatie met Statusconfiguratie van Azure Automation DSC en Chocolatey Pakketbeheer.  Voorbeeld met volledige JSON Resource Manager-sjabloon en PowerShell-bron.
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
ms.date: 08/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3eb68c4394afeb4719d92fb56d3ae9028d8566c9
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56456109"
---
# <a name="usage-example-continuous-deployment-to-virtual-machines-using-automation-state-configuration-and-chocolatey"></a>Voorbeeld van gebruik: Continue implementatie voor virtuele Machines met behulp van de configuratie van de Automation-status en Chocolatey

In een DevOps-wereld zijn er veel hulpprogramma's om u te helpen bij verschillende punten in de pijplijn voor continue integratie. Configuratie van de status van Azure Automation is een Welkom nieuwe toevoeging aan de opties die u van DevOps-teams gebruikmaken kunnen. In dit artikel ziet u de instelling van continue implementatie (CD) voor een Windows-computer. U kunt eenvoudig de techniek voor het aantal Windows-computers als nodig zijn in de rol (bijvoorbeeld een website) en van daaruit naar aanvullende functies ook uitbreiden.

![Continue implementatie voor IaaS-VM 's](./media/automation-dsc-cd-chocolatey/cdforiaasvm.png)

## <a name="at-a-high-level"></a>Op hoog niveau

Er is nogal hier aan, maar gelukkig deze kan worden onderverdeeld in twee belangrijkste processen:

- Code schrijven en testen, klikt u vervolgens maken en publiceren installatiepakketten voor primaire en secundaire versies van het systeem.
- Het maken en beheren van virtuele machines die zullen installeren en voer de code uit in de pakketten.  

Nadat de beide core processen zijn gemaakt, is een korte stap voor het automatisch bijwerken van het pakket naar nieuwe versies worden gemaakt en geïmplementeerd op een bepaalde virtuele machine wordt uitgevoerd.

## <a name="component-overview"></a>Onderdelenoverzicht

Pakket managers zoals [apt-get](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool) heel erg bekend zijn in de wereld van Linux, maar niet zoveel in de wereld van Windows.
[Chocolatey](https://chocolatey.org/) is die een ding en Scott Hanselman van [blog](https://www.hanselman.com/blog/IsTheWindowsUserReadyForAptget.aspx) op het onderwerp is een goede inleiding. Kortom, kunt Chocolatey u om pakketten te installeren vanuit een centrale opslagplaats van pakketten in een Windows-systeem via de opdrachtregel. U kunt maken en beheren van uw eigen opslagplaats en Chocolatey pakketten kunt installeren van een willekeurig aantal opslagplaatsen die u opgeeft.

Desired State Configuration (DSC) ([overzicht](/powershell/dsc/overview)) is een PowerShell-hulpprogramma waarmee u de configuratie die u wilt gebruiken voor een virtuele machine declareren. U kunt bijvoorbeeld, "Ik wil Chocolatey geïnstalleerd, ik wil dat IIS is geïnstalleerd, ik wil dat de poort 80 is geopend, ik wil dat versie 1.0.0 van mijn website geïnstalleerd." De DSC lokale Configuration Manager (LCM) implementeert dat de configuratie. Een DSC-Pull-Server bevat een opslagplaats van configuraties voor uw virtuele machines. De LCM op elke computer controleert periodiek op of de configuratie ervan overeenkomt met de opgeslagen configuratie. Het kan de status rapporteren of worden geprobeerd om de machine weer in overeenstemming met de opgeslagen configuratie. U kunt de opgeslagen configuratie op de pull-server om te leiden tot een machine of een set van machines komen in overeenstemming met de gewijzigde configuratie bewerken.

Azure Automation is een beheerde service in Microsoft Azure waarmee u verschillende taken automatiseren met runbooks, knooppunten, referenties, resources en -assets zoals schema's en globale variabelen.
Azure Automation State Configuration breidt deze automation-mogelijkheid om op te nemen van de PowerShell DSC-hulpprogramma's. Hier is een goed [overzicht](automation-dsc-overview.md).

Een DSC-Resource is een module van code die beschikt over specifieke mogelijkheden, zoals het beheren van netwerken, Active Directory of SQL Server. De Chocolatey DSC-Resource weet hoe u toegang tot een NuGet-Server (onder andere), -pakketten downloaden, installeren van pakketten, enzovoort. Er zijn veel andere DSC-Resources in de [PowerShell Gallery](https://www.powershellgallery.com/packages?q=dsc+resources&prerelease=&sortOrder=package-title).
Deze modules zijn geïnstalleerd in uw Azure Automation-staat configuratie Pull-Server (door u), zodat ze kunnen worden gebruikt door uw configuraties.

Resource Manager-sjablonen bieden een declaratieve manier voor het genereren van uw infrastructuur - handelingen zoals netwerken, subnetten, netwerkbeveiliging en routering, load balancers, NIC's en virtuele machines. Hier volgt een [artikel](../azure-resource-manager/resource-manager-deployment-model.md) die vergelijkt de Resource Manager-implementatiemodel (declaratief) met het implementatiemodel van Azure Service Management (ASM of klassiek) (imperatief), en bespreekt de resourceproviders core, Reken-, opslag en het netwerk.

Een belangrijke functie van een Resource Manager-sjabloon is de mogelijkheid voor het installeren van een VM-extensie in de virtuele machine als deze ingericht. Een VM-extensie heeft specifieke mogelijkheden, zoals een aangepast script is uitgevoerd, antivirussoftware installeren of uitvoeren van een script voor DSC-configuratie. Er zijn nog vele andere typen VM-extensies.

## <a name="quick-trip-around-the-diagram"></a>Snelle reis rond het diagram

Vanaf de bovenkant, schrijft u uw code bouwen, testen en vervolgens een installatiepakket maken.
Chocolatey kan verschillende soorten installatiepakketten, zoals een MSI-bestand, MSU, ZIP worden verwerkt. En u hebt de volledige kracht van PowerShell kunt u de werkelijke installatie doen als Chocolateys systeemeigen mogelijkheden zijn niet helemaal tot aan het. Het pakket in een bereikbaar – een pakketopslagplaats plaatsen. In dit voorbeeld gebruik gebruikmaakt van een openbare map in een Azure blob storage-account, maar deze kan overal worden. Chocolatey werkt systeemeigen met NuGet-servers en enkele andere voor het beheren van de pakketmetagegevens van het. [In dit artikel](https://github.com/chocolatey/choco/wiki/How-To-Host-Feed) worden de opties beschreven. In dit voorbeeld gebruik maakt gebruik van NuGet. Een Nuspec is metagegevens over uw pakketten. Van de Nuspec worden in de NuPkg "gecompileerd' en opgeslagen in een NuGet-server. Wanneer de configuratie van aanvragen van een pakket met de naam en verwijst naar een NuGet-server, wordt de Chocolatey DSC-Resource (nu beschikbaar op de virtuele machine) pakt het pakket en installeert deze voor u. U kunt ook een specifieke versie van een pakket aanvragen.

In het onderste links gedeelte van de afbeelding is er een Azure Resource Manager-sjabloon. In dit voorbeeld gebruik registreert de VM-extensie de virtuele machine met de Azure Automation-staat configuratie Pull-Server (dat wil zeggen, een pull-server) als een knooppunt. De configuratie is opgeslagen in de pull-server.
Dat deze twee keer worden opgeslagen: eenmaal als tekst zonder opmaak en zodra gecompileerd als een MOF-bestand (voor die dergelijke dingen weten.) In de portal is het MOF een 'knooppuntconfiguratie' (in plaats van gewoon ' configuration'). Het is de artefacten die is gekoppeld aan een knooppunt, zodat het knooppunt wordt de configuratie ervan weet. Details hieronder laten zien hoe de knooppuntconfiguratie toewijzen aan het knooppunt.

U bent de bits op de bovenkant of de meeste van deze waarschijnlijk al doet. Het maken van de nuspec, te compileren en in een NuGet-server op te slaan is een kleine ding. En u al beheert virtuele machines. Neemt de volgende stap met continue implementatie is vereist, instellen van de pull-server (één keer), registreert uw knooppunten (één keer), en het maken en opslaan van de configuratie er (in eerste instantie). Vernieuw vervolgens wanneer pakketten worden bijgewerkt en geïmplementeerd naar de opslagplaats, de configuratie en de configuratie van knooppunten in de pull-server (Herhaal indien nodig).

Als u begint niet met een Resource Manager-sjabloon, maar dat is ook OK. Er zijn PowerShell-cmdlets die zijn ontworpen om u te helpen u uw VM's registreren met de pull-server en alle van de rest. Raadpleeg dit artikel voor meer informatie: [Onboarding van machines voor beheer met Azure Automation State Configuration](automation-dsc-onboarding.md).

## <a name="step-1-setting-up-the-pull-server-and-automation-account"></a>Stap 1: Instellen van de pull-server en het automation-account

Op een geverifieerde (`Connect-AzureRmAccount`) PowerShell-opdrachtregel: (kan een paar minuten duren terwijl de pull-server is ingesteld)

```azurepowershell-interactive
New-AzureRmResourceGroup –Name MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES
New-AzureRmAutomationAccount –ResourceGroupName MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES –Name MY-AUTOMATION-ACCOUNT
```

U kunt uw automation-account in een van de volgende regio's (ook wel locatie) plaatsen: VS-Oost 2, VS Zuid-centraal, VS (overheid) Virginia, West-Europa, Zuidoost-Azië, Japan-Oost, centraal-India en Australië-Zuidoost, Canada centraal, Noord-Europa.

## <a name="step-2-vm-extension-tweaks-to-the-resource-manager-template"></a>Stap 2: VM-extensie correcties aan de Resource Manager-sjabloon

Details voor de VM-registratie (met behulp van de PowerShell DSC-VM-extensie) opgegeven in deze [Azure Quickstart-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/dsc-extension-azure-automation-pullserver).
Deze stap wordt de nieuwe virtuele machine geregistreerd met de pull-server in de lijst van status configureren-knooppunten. Onderdeel van deze registratie met het opgeven van de knooppuntconfiguratie moet worden toegepast op het knooppunt. De configuratie van deze knooppunten beschikt niet over nog bestaan in de pull-server, zodat u OK die stap 4 wanneer dit wordt gedaan voor de eerste keer. Maar hier in stap 2 u hoeft te hebben besloten de naam van het knooppunt en de naam van de configuratie. In dit voorbeeld gebruik het knooppunt is 'isvbox' en de configuratie is 'ISVBoxConfig'. De naam van de knooppuntconfiguratie (om te worden opgegeven in DeploymentTemplate.json) is dus 'ISVBoxConfig.isvbox'.

## <a name="step-3-adding-required-dsc-resources-to-the-pull-server"></a>Stap 3: Vereiste DSC-resources toe te voegen aan de pull-server

De PowerShell Gallery is geïnstrumenteerd voor het installeren van DSC-resources in uw Azure Automation-account.
Navigeer naar de resource die u wilt en klik op de knop 'Implementeren naar Azure Automation'.

![Voorbeeld van de PowerShell Gallery](./media/automation-dsc-cd-chocolatey/xNetworking.PNG)

Een andere methode die onlangs zijn toegevoegd aan de Azure-Portal kunt u nieuwe modules verzamelen of bestaande-modules bijwerken. Klik op de resource van het Automation-Account, de tegel Assets en ten slotte de tegel Modules. Het pictogram bladeren in galerie kunt u zien van de lijst met modules in de galerie, Inzoomen op details en uiteindelijk importeren in uw Automation-Account. Dit is een uitstekende manier om uw modules up-to-date te houden van tijd tot tijd. En de importfunctie wordt gecontroleerd of afhankelijkheden met andere modules om ervoor te zorgen dat er niets wordt niet gesynchroniseerd.

Of er is de handmatige methode. De mapstructuur van een PowerShell-integratie-Module voor een Windows-computer is enigszins verschillen van de mapstructuur die werd verwacht door de Azure Automation.
Hiervoor moet een beetje aanpassen aan uw kant. Maar het is niet moeilijk en dit wordt gedaan slechts één keer per resource (tenzij u in de toekomst een upgrade uitvoert.) Raadpleeg dit artikel voor meer informatie over het ontwerpen van integratiemodules PowerShell: [Integratiemodules ontwerpen voor Azure Automation](https://azure.microsoft.com/blog/authoring-integration-modules-for-azure-automation/)

- De module die u nodig hebt op uw werkstation, als volgt installeren:
  - Installeer [Windows Management Framework, v5](https://aka.ms/wmf5latest) (niet nodig voor Windows 10)
  - `Install-Module –Name MODULE-NAME`    <: pakt de module op basis van de PowerShell Gallery
- Kopieer de modulemap van `c:\Program Files\WindowsPowerShell\Modules\MODULE-NAME` naar een tijdelijke map
- Voorbeelden en documentatie van de belangrijkste map verwijderen
- ZIP-de hoofdmap, het ZIP-bestand een naam precies hetzelfde als de map 
- Het ZIP-bestand in een bereikbaar HTTP-locatie, zoals blob-opslag in een Azure Storage-Account geplaatst.
- Voer deze PowerShell:

  ```powershell
  New-AzureRmAutomationModule `
    -ResourceGroupName MY-AUTOMATION-RG -AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -Name MODULE-NAME –ContentLink 'https://STORAGE-URI/CONTAINERNAME/MODULE-NAME.zip'
  ```

De opgenomen voorbeeld voert deze stappen voor cChoco en xNetworking. Zie de [opmerkingen bij de](#notes) voor speciale handelingen voor cChoco.

## <a name="step-4-adding-the-node-configuration-to-the-pull-server"></a>Stap 4: De configuratie van de knooppunten toe te voegen aan de pull-server

Er zijn geen speciale over de eerste keer dat u de configuratie in de pull-server en de compilatie importeren. Alle volgende importeren/compileert met dezelfde configuratie er precies hetzelfde. Telkens wanneer u uw pakket bijwerken en pusht naar productie wilt uitvoert u deze stap nadat u hebt gecontroleerd van dat het configuratiebestand juist is, inclusief de nieuwe versie van het pakket. Dit is de configuratie van bestands- en PowerShell:

ISVBoxConfig.ps1:

```powershell
Configuration ISVBoxConfig
{
    Import-DscResource -ModuleName cChoco
    Import-DscResource -ModuleName xNetworking

    Node 'isvbox' {

        cChocoInstaller installChoco
        {
            InstallDir = 'C:\choco'
        }

        WindowsFeature installIIS
        {
            Ensure = 'Present'
            Name   = 'Web-Server'
        }

        xFirewall WebFirewallRule
        {
            Direction    = 'Inbound'
            Name         = 'Web-Server-TCP-In'
            DisplayName  = 'Web Server (TCP-In)'
            Description  = 'IIS allow incoming web site traffic.'
            Enabled       = 'True'
            Action       = 'Allow'
            Protocol     = 'TCP'
            LocalPort    = '80'
            Ensure       = 'Present'
        }

        cChocoPackageInstaller trivialWeb
        {
            Name      = 'trivialweb'
            Version   = '1.0.0'
            Source    = 'MY-NUGET-V2-SERVER-ADDRESS'
            DependsOn = '[cChocoInstaller]installChoco','[WindowsFeature]installIIS'
        }
    }
}
```

New-ConfigurationScript.ps1:

```powershell
Import-AzureRmAutomationDscConfiguration `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -SourcePath C:\temp\AzureAutomationDsc\ISVBoxConfig.ps1 `
    -Published –Force

$jobData = Start-AzureRmAutomationDscCompilationJob `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -ConfigurationName ISVBoxConfig

$compilationJobId = $jobData.Id

Get-AzureRmAutomationDscCompilationJob `
    -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT `
    -Id $compilationJobId
```

Het resultaat van deze stappen in een nieuwe configuratie van de knooppunten met de naam 'ISVBoxConfig.isvbox' worden geplaatst op de pull-server. De naam van de knooppuntconfiguratie is gebouwd als 'configurationName.nodeName'.

## <a name="step-5-creating-and-maintaining-package-metadata"></a>Stap 5: Het maken en onderhouden van pakketmetagegevens

Voor elk pakket dat u in de opslagplaats van het pakket hebt geplaatst, moet u een nuspec die wordt beschreven.
Deze nuspec moet worden gecompileerd en opgeslagen in de NuGet-server. Dit proces wordt beschreven [hier](https://docs.nuget.org/create/creating-and-publishing-a-package). U kunt MyGet.org gebruiken als een NuGet-server. Ze deze service verkopen, maar hebben een starter SKU die gratis is. Op NuGet.org vindt u instructies over het installeren van uw eigen NuGet-server voor uw persoonlijke pakketten.

## <a name="step-6-tying-it-all-together"></a>Stap 6: Alles bij elkaar verbinden

Telkens wanneer een versie QA is geslaagd en is goedgekeurd voor implementatie, het pakket wordt gemaakt, nuspec en nupkg bijgewerkt en worden geïmplementeerd voor de NuGet-server. Bovendien moet de configuratie (stap 4 hierboven) worden bijgewerkt akkoord gaat met het nieuwe versienummer. Het moet worden verzonden naar de pull-server en gecompileerd.
Vanaf dat moment op is het aan de virtuele machines die afhankelijk van dat de configuratie zijn voor het ophalen van de update en installeer deze. Elk van deze updates zijn eenvoudige - alleen een line- of twee van PowerShell. In het geval van Azure DevOps, zijn sommige van deze ingekapseld in de build-taken die u een worden samengevoegd in een build keten kunnen. Dit [artikel](https://www.visualstudio.com/docs/alm-devops-feature-index#continuous-delivery) vindt u meer informatie. Dit [GitHub-opslagplaats](https://github.com/Microsoft/vso-agent-tasks) details van de verschillende beschikbare build-taken.

## <a name="notes"></a>Opmerkingen

In dit voorbeeld gebruik begint met een VM op basis van een algemene Windows Server 2012 R2-afbeelding van de Azure-galerie. U kunt starten vanuit een opgeslagen afbeelding en vervolgens vanaf daar met de DSC-configuratie aanpassen.
Echter, is het veel moeilijker dan het dynamisch bijwerken van de configuratie met behulp van DSC-configuratie die is sparen wijzigen in een installatiekopie.

U hoeft niet te Resource Manager-sjabloon en de VM-extensie gebruiken deze techniek gebruiken met uw virtuele machines. En uw virtuele machines hoeft te zijn op Azure om te worden beheerd op de CD. Alles dat nodig is, is dat Chocolatey worden geïnstalleerd en de LCM geconfigureerd op de VM zodat deze waar de pull-server is.

Wanneer u een pakket op een virtuele machine die in productie bijwerkt, moet u natuurlijk, worden die virtuele machine uitgeschakeld terwijl de update is geïnstalleerd. Doet u als volgt varieert aanzienlijk. Bijvoorbeeld, met een VM uit achter een Load Balancer van Azure, kunt u toevoegen een aangepaste test. Tijdens het bijwerken van de virtuele machine, hebt u het eindpunt van de test een 400 geretourneerd. De bewerking wilt uitvoeren die nodig zijn om deze wijziging is in uw configuratie, zoals kan de bewerking wilt uitvoeren als u wilt het overschakelen naar een 200 retourneren nadat de update voltooid is.

Volledige bron voor dit voorbeeld gebruik is in [deze Visual Studio-project](https://github.com/sebastus/ARM/tree/master/CDIaaSVM) op GitHub.

## <a name="related-articles"></a>Gerelateerde artikelen
* [Overzicht van Azure Automation DSC](automation-dsc-overview.md)
* [Azure Automation DSC-cmdlets](https://docs.microsoft.com/powershell/module/azurerm.automation#automation)
* [Onboarding van machines voor beheer met Azure Automation DSC](automation-dsc-onboarding.md)

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [Statusconfiguratie van Azure Automation](automation-dsc-overview.md)
- Als u wilt beginnen, Zie [aan de slag met Azure Automation State Configuration](automation-dsc-getting-started.md)
- Zie voor meer informatie over het DSC-configuraties compileren zodat u ze aan de doelknooppunten toewijzen kunt, [-configuraties in Azure Automation-staat configuratie compileren](automation-dsc-compile.md)
- Zie voor naslagdocumentatie voor PowerShell-cmdlet, [Statusconfiguratie van Azure Automation-cmdlets](/powershell/module/azurerm.automation/#automation)
- Zie voor informatie over de prijzen [Statusconfiguratie van Azure Automation-prijzen](https://azure.microsoft.com/pricing/details/automation/)
- Zie voor een voorbeeld van het gebruik van Azure Automation State Configuration in een pijplijn voor continue implementatie [continue implementatie met behulp van Azure Automation Statusconfiguratie- en Chocolatey](automation-dsc-cd-chocolatey.md)
