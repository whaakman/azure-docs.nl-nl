---
title: Azure Automation DSC continue implementatie met Chocolatey | Microsoft Docs
description: DevOps continue implementatie met Azure Automation DSC en Chocolatey Pakketbeheer.  Voorbeeld met volledige JSON ARM-sjabloon en de PowerShell-bron.
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: c0baa411-eb76-4f91-8d14-68f68b4805b6
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 10/29/2016
ms.author: golive
ms.openlocfilehash: f9957d745ed910fbdcbeeee7d9ddb24a51da141b
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="usage-example-continuous-deployment-to-virtual-machines-using-automation-dsc-and-chocolatey"></a>Gebruik voorbeeld: Continue implementatie naar virtuele Machines met behulp van Automation DSC en Chocolatey
In een wereld DevOps zijn er veel hulpprogramma's om u te helpen bij verschillende punten in de pijplijn voor continue integratie.  Azure Automation gewenst State Configuration (DSC) is een Welkom nieuwe aanvulling op de opties die u van DevOps-teams gebruikmaken kunnen.  In dit artikel wordt gedemonstreerd instelling van continue implementatie (CD) voor een Windows-computer.  U kunt eenvoudig de techniek en op te nemen als veel Windows-computers, indien nodig in de rol (bijvoorbeeld een website) van daaruit naar aanvullende functies ook uitbreiden.

![Continue implementatie voor IaaS VM 's](./media/automation-dsc-cd-chocolatey/cdforiaasvm.png)

## <a name="at-a-high-level"></a>Op hoog niveau
Er is heel wat hier, maar gelukkig kunnen worden onderverdeeld in twee belangrijkste processen: 

* Schrijven van code testen, maken en publiceren van installatiepakketten voor de primaire en secundaire versies van het systeem. 
* Maken en beheren van virtuele machines die zullen installeren en voer de code in de pakketten.  

Als beide core processen uitgevoerd worden, is een korte stap automatisch bijwerken van het pakket naar nieuwe versies worden gemaakt en geïmplementeerd op een bepaalde virtuele machine wordt uitgevoerd.

## <a name="component-overview"></a>Onderdelenoverzicht van het
Pakket managers zoals [apt get-](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool) vrij bekend zijn in de Linux-wereld, maar niet zoveel in de Windows-wereld.  [Chocolatey](https://chocolatey.org/) is zo'n ding en van Scott Hanselman [blog](http://www.hanselman.com/blog/IsTheWindowsUserReadyForAptget.aspx) is een uitstekende inleiding in het onderwerp.  Kortom, kunt Chocolatey u pakketten installeren vanaf een centrale opslagplaats van pakketten in een Windows-systeem via de opdrachtregel.  U kunt maken en beheren van uw eigen opslagplaats en Chocolatey kunt pakketten installeren van een willekeurig aantal opslagplaatsen die u opgeeft.

Desired State Configuration (DSC) ([overzicht](https://technet.microsoft.com/library/dn249912.aspx)) is een PowerShell-hulpprogramma waarmee u de configuratie die u wilt dat voor een machine declareren.  U kunt bijvoorbeeld, "Ik wil Chocolatey geïnstalleerd, ik wil dat IIS is geïnstalleerd, ik wil dat de poort 80 is geopend, ik wil versie 1.0.0 van mijn website geïnstalleerd."  De DSC lokale Configuration Manager (LCM) implementeert dat de configuratie. Een DSC-Pull-Server bevat een opslagplaats van configuraties voor uw machines. De LCM op elke machine controleert regelmatig of de configuratie ervan overeenkomt met de opgeslagen configuratie. Deze kan status rapporteren of voert u de computer terugbrengen in uitlijning met de opgeslagen configuratie. U kunt de opgeslagen configuratie op de pull-server om te leiden tot een computer of een set computers moeten afkomstig zijn uitgelijnd met de gewijzigde configuratie bewerken.

Azure Automation is een beheerde service in Microsoft Azure waarmee u verschillende automatiseren met behulp van runbooks, knooppunten, referenties, bronnen en activa zoals schema's en globale variabelen. Azure Automation DSC breidt deze mogelijkheid automation PowerShell DSC-hulpprogramma's opnemen.  Hier volgt een geweldige [overzicht](automation-dsc-overview.md).

Een DSC-Resource is een module van code die de specifieke mogelijkheden, zoals het beheren van netwerken, Active Directory of SQL Server is.  De Chocolatey DSC-Resource weet hoe u toegang tot een NuGet-Server (onder andere), pakketten downloaden, installeren van pakketten, enzovoort.  Er zijn veel andere DSC-Resources in de [PowerShell Gallery](http://www.powershellgallery.com/packages?q=dsc+resources&prerelease=&sortOrder=package-title).  Deze modules zijn geïnstalleerd in uw Azure Automation DSC-Pull-Server (door u) zodat ze kunnen worden gebruikt door uw configuraties.

Resource Manager-sjablonen bieden een declaratieve manier voor het genereren van uw infrastructuur - items zoals netwerken, subnetten, netwerkbeveiliging en routering, load balancers, NIC's en virtuele machines.  Hier volgt een [artikel](../azure-resource-manager/resource-manager-deployment-model.md) die worden vergeleken met het Resource Manager-implementatiemodel (declaratieve) met de Azure Service Management (ASM of klassiek)-implementatie (imperatieve) model en de resourceproviders core, berekening, opslag en netwerk besproken.

Een belangrijke functie van een Resource Manager-sjabloon is de mogelijkheid voor het installeren van een VM-extensie in de virtuele machine als deze ingericht.  Een VM-extensie heeft specifieke mogelijkheden, zoals een aangepast script uitvoeren, antivirussoftware installeren of uitvoeren van een script DSC-configuratie.  Er zijn vele andere typen van VM-extensies.

## <a name="quick-trip-around-the-diagram"></a>Snelle reis rond het diagram
Vanaf de bovenkant, schrijft u uw code bouwen en testen en vervolgens een installatiepakket maken.  Chocolatey kan verschillende soorten installatiepakketten, zoals ZIP MSI, MSU, verwerken.  En u de kracht van PowerShell doen de werkelijke installatie als systeemeigen mogelijkheden van Chocolatey niet helemaal tot hebt.  Het pakket in een fout bereikbaar – een opslagplaats pakket geplaatst.  In dit voorbeeld gebruik van een openbare map in een Azure blob storage-account gebruikt, maar deze kan overal worden.  Chocolatey werkt systeemeigen met NuGet-servers en enkele andere voor het beheer van de pakketmetagegevens van het.  [In dit artikel](https://github.com/chocolatey/choco/wiki/How-To-Host-Feed) worden de opties beschreven.  In dit voorbeeld gebruik NuGet gebruikt.  Een Nuspec zijn metagegevens over uw pakketten.  De Nuspec zijn 'gecompileerd' in de NuPkg en opgeslagen in een NuGet-server.  Wanneer de configuratie van aanvragen van een pakket met de naam en verwijst naar een NuGet-server, wordt de Chocolatey DSC-Resource (nu op de virtuele machine) pakt het pakket en installeert deze voor u.  U kunt ook een specifieke versie van een pakket aanvragen.

Er is een Azure Resource Manager (ARM)-sjabloon in het linkergedeelte onderkant van de afbeelding.  In dit voorbeeld gebruik registreert de VM-extensie de virtuele machine met de Azure Automation DSC Pull-Server (dat wil zeggen, een pull-server) als een knooppunt.  De configuratie wordt opgeslagen in de pull-server.  Dat deze twee keer wordt opgeslagen: eenmaal als tekst zonder opmaak en wanneer gecompileerd als een MOF-bestand (voor die over zaken weten.)  In de portal is de MOF een 'knooppuntconfiguratie' (in plaats van gewoon "configuratie").  Het is de artefacten die is gekoppeld aan een knooppunt, zodat het knooppunt de configuratie ervan weet.  Details hieronder laten zien hoe de knooppuntconfiguratie toewijzen aan het knooppunt.

U doet waarschijnlijk al de bit boven of de meeste van deze.  Maken van de nuspec, compileren en opgeslagen in een NuGet-server is een kleine ding.  En u bent al het beheer van virtuele machines.  De volgende stap brengen naar continue implementatie vereist de pull-server (eenmaal) in te stellen, registreert uw knooppunten (één keer), en maken en opslaan van de configuratie er (in eerste instantie).  Vernieuw vervolgens pakketten worden bijgewerkt en geïmplementeerd naar de opslagplaats, de configuratie en configuratie van knooppunten in de pull-server (Herhaal indien nodig).

Als u begint niet met een ARM-sjabloon, maar dat is ook OK.  Er zijn ontworpen voor hulp bij het registreren van uw virtuele machines met de pull-server en alle van de rest van PowerShell-cmdlets. Zie voor meer informatie in dit artikel: [machines voorbereiden voor beheer door Azure Automation DSC](automation-dsc-onboarding.md)

## <a name="step-1-setting-up-the-pull-server-and-automation-account"></a>Stap 1: Het pull-server en de automation-account instellen
Op een geverifieerde (Add-AzureRmAccount) PowerShell-opdrachtregel: (kan enkele minuten duren terwijl de pull-server is ingesteld)

    New-AzureRmResourceGroup –Name MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES
    New-AzureRmAutomationAccount –ResourceGroupName MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES –Name MY-AUTOMATION-ACCOUNT 

U kunt uw automation-account in een van de volgende regio's (aka locatie) plaatsen: VS-Oost 2, Zuid-centraal VS, Gov ons Virginia, West-Europa, Zuidoost-Azië, Japan-Oost, India centraal en Australië-Zuidoost, Canada centraal, Noord-Europa.

## <a name="step-2-vm-extension-tweaks-to-the-arm-template"></a>Stap 2: VM-extensie trucs in de ARM-sjabloon
Details voor VM-registratie (met behulp van de PowerShell DSC-VM-extensie) opgegeven in deze [Snelstartsjabloon met de Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/dsc-extension-azure-automation-pullserver).  Deze stap registreert uw nieuwe virtuele machine met de pull-server in de lijst met DSC-knooppunten.  Onderdeel van deze registratie wordt de knooppuntconfiguratie moet worden toegepast op het knooppunt op te geven.  Deze knooppuntconfiguratie heeft geen nog bestaan in de pull-server, zodat deze OK die stap 4 wanneer dit wordt gedaan voor het eerst is.  Maar hier in stap 2 moet u hebt besloten de naam van het knooppunt en de naam van de configuratie.  In dit voorbeeld gebruik het knooppunt is 'isvbox' en de configuratie is 'ISVBoxConfig'.  De naam van de configuratie knooppunt (om te worden opgegeven in DeploymentTemplate.json) is daarom 'ISVBoxConfig.isvbox'.  

## <a name="step-3-adding-required-dsc-resources-to-the-pull-server"></a>Stap 3: Vereiste DSC-resources toevoegen aan de pull-server
De PowerShell-galerie is geïnstrumenteerd voor het installeren van DSC-resources in uw Azure Automation-account.  Navigeer naar de resource die u wilt en klik op de knop 'Implementeren naar Azure Automation'.

![Voorbeeld van PowerShell Gallery](./media/automation-dsc-cd-chocolatey/xNetworking.PNG)

Een andere methode die onlangs zijn toegevoegd aan de Azure Portal kunt u voor het ophalen van nieuwe modules of bijwerken van bestaande modules. Klik in de resource Automation-Account, de activa-tegel en ten slotte de tegel Modules.  Het pictogram voor het bladeren galerij kunt u de lijst met modules in de galerie, Inzoomen op gegevens en uiteindelijk importeren in uw Automation-Account. Dit is een uitstekende manier om uw modules up-to-date te houden van tijd tot tijd. En de functie importeren controleert afhankelijkheden met andere modules om ervoor te zorgen dat niets niet synchroon.

Of er is de handmatige methode.  De mapstructuur van een PowerShell-integratie-Module voor een Windows-computer verschilt enigszins van de mapstructuur werd verwacht door de Azure Automation.  Hiervoor moet een kleine aanpassingen van uw kant.  Het is niet moeilijk maar het voltooid maar één keer per resource (tenzij u in de toekomst een upgrade uitvoeren.)  Zie voor meer informatie over het ontwerpen van PowerShell integratiemodules in dit artikel: [Azure Automation-integratiemodules ontwerpen](https://azure.microsoft.com/blog/authoring-integration-modules-for-azure-automation/)

* Installeer de module die u nodig hebt op uw werkstation als volgt:
  * Installeer [Windows Management Framework, v5](http://aka.ms/wmf5latest) (niet nodig voor Windows 10)
  * `Install-Module –Name MODULE-NAME`<, pakt u de module op basis van de PowerShell-galerie 
* Kopieer de modulemap van `c:\Program Files\WindowsPowerShell\Modules\MODULE-NAME` naar een tijdelijke map 
* Voorbeelden en documentatie verwijderen uit de hoofdmap 
* De hoofdmap, de naam van het ZIP-bestand exact hetzelfde zijn als de map voor de ZIP 
* Plaats het ZIP-bestand in een bereikbaar HTTP-locatie, zoals de blob-opslag in een Azure Storage-Account.
* Voer deze PowerShell:
  
      New-AzureRmAutomationModule `
          -ResourceGroupName MY-AUTOMATION-RG -AutomationAccountName MY-AUTOMATION-ACCOUNT `
          -Name MODULE-NAME –ContentLink "https://STORAGE-URI/CONTAINERNAME/MODULE-NAME.zip"

De opgenomen voorbeeld voert deze stappen voor cChoco en xNetworking. Zie de [notities](#notes) voor speciale verwerking voor cChoco.

## <a name="step-4-adding-the-node-configuration-to-the-pull-server"></a>Stap 4: De knooppuntconfiguratie toevoegen aan de pull-server
Er zijn geen speciale over de eerste keer dat u uw configuratie in de pull-server en de compilatie importeren.  Alle volgende import/compileert met dezelfde configuratie er precies hetzelfde.  Telkens wanneer u het pakket bijwerkt en u moet doorgeven aan de productie doen u deze stap nadat u dat het configuratiebestand klopt – met inbegrip van de nieuwe versie van het pakket.  Dit is het configuratiebestand en PowerShell:

ISVBoxConfig.ps1:

    Configuration ISVBoxConfig 
    { 
        Import-DscResource -ModuleName cChoco 
        Import-DscResource -ModuleName xNetworking

        Node "isvbox" {   

            cChocoInstaller installChoco 
            { 
                InstallDir = "C:\choco" 
            }

            WindowsFeature installIIS 
            { 
                Ensure="Present" 
                Name="Web-Server" 
            }

            xFirewall WebFirewallRule 
            { 
                Direction = "Inbound" 
                Name = "Web-Server-TCP-In" 
                DisplayName = "Web Server (TCP-In)" 
                Description = "IIS allow incoming web site traffic." 
                DisplayGroup = "IIS Incoming Traffic" 
                State = "Enabled" 
                Access = "Allow" 
                Protocol = "TCP" 
                LocalPort = "80" 
                Ensure = "Present" 
            }

            cChocoPackageInstaller trivialWeb 
            {            
                Name = "trivialweb" 
                Version = "1.0.0" 
                Source = “MY-NUGET-V2-SERVER-ADDRESS” 
                DependsOn = "[cChocoInstaller]installChoco", 
                "[WindowsFeature]installIIS" 
            } 
        }    
    }

Nieuw-ConfigurationScript.ps1:

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

Het resultaat van deze stappen in een nieuwe configuratie van de knooppunten met de naam 'ISVBoxConfig.isvbox' op de pull-server wordt geplaatst.  De naam van de configuratie knooppunt is gebouwd als 'configurationName.nodeName'.

## <a name="step-5-creating-and-maintaining-package-metadata"></a>Stap 5: Het maken en onderhouden van pakketmetagegevens
Voor elk pakket dat u in de opslagplaats van het pakket zetten, moet u een nuspec die wordt beschreven.  Die nuspec moet worden gecompileerd en opgeslagen in de NuGet-server. Dit proces wordt beschreven [hier](http://docs.nuget.org/create/creating-and-publishing-a-package).  U kunt MyGet.org gebruiken als een NuGet-server.  Ze deze service verkocht, maar hebben een starter SKU is gratis.  NuGet.org vindt u instructies voor het installeren van uw eigen NuGet-server van uw persoonlijke pakketten is.

## <a name="step-6-tying-it-all-together"></a>Stap 6: Alles samenvoegen koppelende
Telkens wanneer een versie QA is geslaagd en is goedgekeurd voor implementatie, is het pakket gemaakt, nuspec en nupkg bijgewerkt en worden geïmplementeerd voor de NuGet-server.  Bovendien moet de configuratie (stap 4 hierboven) worden bijgewerkt Ga akkoord met het nieuwe versienummer.  Het moet worden verzonden naar de pull-server en gecompileerde.  Vanaf dat moment is het aan de virtuele machines die afhankelijk zijn van die configuratie pull-de update en installeren.  Elk van deze updates zijn eenvoudige - slechts een of twee regels van PowerShell.  In het geval van Visual Studio Team Services, zijn sommige van deze ingekapseld in de build-taken die u een worden samengesteld in een build keten kunnen.  Dit [artikel](https://www.visualstudio.com/en-us/docs/alm-devops-feature-index#continuous-delivery) biedt meer details.  Dit [GitHub-repo-](https://github.com/Microsoft/vso-agent-tasks) details van de verschillende beschikbare build-taken.

## <a name="notes"></a>Opmerkingen
In dit voorbeeld gebruik begint met een virtuele machine van een algemene Windows Server 2012 R2-afbeelding van de Azure-galerie.  U kunt starten vanuit een opgeslagen installatiekopie en vervolgens vanaf daar met de DSC-configuratie aanpassen.  Configuratie die is standaard uitbreidbaar wijzigen in een installatiekopie is echter veel moeilijker dan het gebruik van DSC configuratie dynamisch bijwerken.

U hoeft niet te gebruiken van een ARM-sjabloon en de VM-extensie voor het gebruik van deze techniek met uw virtuele machines.  En hebt u uw virtuele machines niet op Azure om te worden beheerd op de CD.  Alle die nodig is dat Chocolatey wordt geïnstalleerd en de LCM geconfigureerd op de virtuele machine, zodat deze waar de pull-server is.  

Wanneer u een pakket op een virtuele machine die in gebruik is genomen bijwerkt, moet u natuurlijk die VM buiten rotatie duren terwijl de update is geïnstalleerd.  Hoe u dit doen varieert.  Bijvoorbeeld, met een VM achter een Load Balancer van Azure, kunt u toevoegen een aangepaste test.  Tijdens het bijwerken van de virtuele machine hebben het retourneren van een 400 test-eindpunt.  De tweak moet u ervoor zorgen dat deze wijziging zijn binnen uw configuratie, kunt u de tweak om terug te keren naar een 200 retourneren nadat de update voltooid is.

Volledige bron voor dit voorbeeld gebruik is in [deze Visual Studio-project](https://github.com/sebastus/ARM/tree/master/CDIaaSVM) op GitHub.

## <a name="related-articles"></a>Verwante artikelen
* [Overzicht van Azure Automation DSC](automation-dsc-overview.md)
* [Azure Automation DSC-cmdlets](https://msdn.microsoft.com/library/mt244122.aspx)
* [Computers voorbereiden voor beheer door Azure Automation DSC](automation-dsc-onboarding.md)

