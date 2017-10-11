---
title: Een CI/CD-pijplijn maken in Azure met Team Services | Microsoft Docs
description: Informatie over het maken van een pijplijn Visual Studio Team Services voor continue integratie en levering die een web-app in IIS op een virtuele machine van Windows implementeert
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: a587f58fad2ec74c7633823c4d34f900e7c01f7e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-continuous-integration-pipeline-with-visual-studio-team-services-and-iis"></a>Een continue integratie-pijplijn maken met Visual Studio Team Services en IIS
Voor het automatiseren van de build-, test- en implementatie fasen van de ontwikkeling van toepassingen, kunt u een continue integratie en implementatie (CI/CD) pijplijn. In deze zelfstudie maakt u een CI/CD-pijplijn met Visual Studio Team Services en virtuele Windows-machine (VM) in Azure waarop IIS wordt uitgevoerd. Procedures voor:

> [!div class="checklist"]
> * ASP.NET-webtoepassing op een project Team Services publiceren
> * Een build-definitie die wordt geactiveerd door doorvoeracties code maken
> * IIS installeren en configureren op een virtuele machine in Azure
> * Het IIS-exemplaar toevoegen aan een implementatiegroep in Team Services
> * Maken van een release definitie voor het publiceren van nieuwe web implementeren van pakketten naar IIS
> * De pijplijn CI/CD testen

Voor deze zelfstudie is moduleversie 3,6 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u upgraden wilt, Zie [Installeer Azure PowerShell-module](/powershell/azure/install-azurerm-ps).


## <a name="create-project-in-team-services"></a>Project maken in een Team Services
Visual Studio Team Services kunt u eenvoudig samenwerking en ontwikkeling zonder het onderhouden van een on-premises-oplossing voor het beheer van code. Teamservices biedt cloud code testen, build en application insights. U kunt een besturingselement-repo-versie en IDE die het beste past bij de ontwikkeling van uw code. Voor deze zelfstudie kunt u een gratis account om een eenvoudige ASP.NET-web-app en CI/CD-pijplijn te maken. Als u nog geen een Team Services-account [maken van een](http://go.microsoft.com/fwlink/?LinkId=307137).

Het proces van de commit code beheren, definities bouwen en definities release, in een project maakt Team Services als volgt:

1. Open uw dashboard Team Services in een webbrowser en kies **nieuw project**.
2. Voer *myWebApp* voor de **projectnaam**. Alle andere standaardwaarden gebruiken laat *Git* versiebeheer en *Agile* proces werkitem.
3. Kies de optie **delen met** *teamleden*, selecteer daarna **maken**.
5. Wanneer uw project is gemaakt, kiest u de optie voor het **initialiseren met een Leesmij-bestand of gitignore**, klikt u vervolgens **initialiseren**.
6. Kies in het nieuwe project **Dashboards** aan de bovenkant Selecteer **openen in Visual Studio**.


## <a name="create-aspnet-web-application"></a>ASP.NET-webtoepassing maken
In de vorige stap hebt u een project in Team Services gemaakt. De laatste stap wordt het nieuwe project in Visual Studio geopend. Beheren van uw code doorvoeringen in de **Team Explorer** venster. Maken van een lokale kopie van het nieuwe project en maak vervolgens een ASP.NET-webtoepassing vanuit een sjabloon als volgt:

1. Selecteer **kloon** naar een lokale git-opslagplaats van uw Team Services-project maken.
    
    ![De opslagplaats klonen uit Team Services-project](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. Onder **oplossingen**, selecteer **nieuw**.

    ![Oplossing voor webtoepassing maken](media/tutorial-vsts-iis-cicd/new_solution.png)

3. Selecteer **Web** sjablonen, en kies vervolgens de **ASP.NET-webtoepassing** sjabloon.
    1. Voer een naam voor uw toepassing, zoals *myWebApp*, en schakel het selectievakje uit voor **map voor oplossing maken**.
    2. Als de optie beschikbaar is, schakelt u het selectievakje **Application Insights toevoegen aan project**. Application Insights moet u uw webtoepassing met Azure Application Insights autoriseren. Het gemak in deze zelfstudie houden, moet u dit proces overslaan.
    3. Selecteer **OK**.
4. Kies **MVC** uit de lijst met sjablonen.
    1. Selecteer **verificatie wijzigen**, kies **geen verificatie**, selecteer daarna **OK**.
    2. Selecteer **OK** om uw oplossing te maken.
5. In de **Team Explorer** venster kiezen **wijzigingen**.

    ![Lokale wijzigingen doorvoeren aan Team Services git-opslagplaats](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. Voer in het tekstvak commit een bericht zoals *initiële doorvoeren*. Kies **alle doorvoeren en Sync** uit de vervolgkeuzelijst.


## <a name="create-build-definition"></a>Build-definitie maken
In het Team Services gebruikt u de definitie van een build als overzicht van hoe uw toepassing moet worden samengesteld. In deze zelfstudie maken we een basisdefinitie die vergt onze broncode, bouwt de oplossing en maakt vervolgens web pakket die we gebruiken kunnen voor het uitvoeren van de web-app op een IIS-server implementeren.

1. Kies in het project Team Services **bouwen & Release** bovenaan, selecteer **Builds**.
3. Selecteer **+ nieuwe definitie**.
4. Kies de **ASP.NET (PREVIEW)** sjabloon en selecteer **toepassen**.
5. Laat de standaardwaarde taak waarden. Onder **bronnen ophalen**, zorg ervoor dat de *myWebApp* opslagplaats en *master* vertakking worden geselecteerd.

    ![Definitie van de build in Team Services-project maken](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. Op de **Triggers** tabblad, verplaats de schuifregelaar voor **inschakelen van deze trigger** naar *ingeschakeld*.
7. De definitie van de build en de wachtrij een nieuwe build opslaan door **opslaan en wachtrij** , vervolgens **opslaan en wachtrij** opnieuw. Laat de standaardwaarden en selecteer **wachtrij**.

Bekijk begint als de build is gepland op een gehoste agent vervolgens om samen te stellen. De uitvoer lijkt op die in het volgende voorbeeld:

![Geslaagde build van het Team Services-project](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a>Virtuele machine maken
Als u een platform voor het uitvoeren van uw ASP.NET-web-app maken, moet u een virtuele Windows-machine waarop IIS wordt uitgevoerd. Teamservices maakt gebruik van een agent om te communiceren met het IIS-exemplaar als u code doorvoert en builds worden geactiveerd.

Maak een VM van Windows Server 2016 met [dit voorbeeldscript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json). Het duurt enkele minuten duren voordat het script uitvoeren en de virtuele machine maken. Zodra de virtuele machine is gemaakt, opent u poort 80 voor internetverkeer met [toevoegen AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) als volgt:

```powershell
Get-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $resourceGroup `
  -Name "myNetworkSecurityGroup" | `
Add-AzureRmNetworkSecurityRuleConfig `
  -Name "myNetworkSecurityGroupRuleWeb" `
  -Protocol "Tcp" `
  -Direction "Inbound" `
  -Priority "1001" `
  -SourceAddressPrefix "*" `
  -SourcePortRange "*" `
  -DestinationAddressPrefix "*" `
  -DestinationPortRange "80" `
  -Access "Allow" | `
Set-AzureRmNetworkSecurityGroup
```

Verkrijgen voor verbinding met uw virtuele machine, het openbare IP-adres met [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) als volgt:

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup | Select IpAddress
```

Een extern-bureaubladsessie met uw virtuele machine maken:

```cmd
mstsc /v:<publicIpAddress>
```

Open op de virtuele machine, een **beheerder PowerShell** opdrachtprompt. IIS en de vereiste .NET-functies als volgt installeren:

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a>Implementatiegroep maken
Voor de push-uit het web pakket implementeren op de IIS-server, het definiëren van een implementatiegroep in een Team Services. Deze groep kunt u opgeven welke servers zijn het doel van de nieuwe builds u code doorvoert naar Team Services en builds zijn voltooid.

1. Kies in het Team Services, **bouwen & Release** en selecteer vervolgens **implementatiegroepen**.
2. Kies **implementatie toevoegen groep**.
3. Voer een naam voor de groep, zoals *myIIS*, selecteer daarna **maken**.
4. In de **machines registreren** sectie, moet u zorgen *Windows* is ingeschakeld, wordt het selectievakje **gebruiken een persoonlijk toegangstoken in het script voor verificatie**.
5. Selecteer **script kopiëren naar Klembord**.


### <a name="add-iis-vm-to-the-deployment-group"></a>IIS VM toevoegen aan de implementatiegroep
Toevoegen aan de implementatiegroep is gemaakt, elke IIS-exemplaar aan de groep. Teamservices genereert een script dat downloadt en configureert u een agent op de virtuele machine die nieuw web ontvangt pakketten implementeren en vervolgens toegepast op IIS.

1. Terug in de **beheerder PowerShell** sessie op de virtuele machine, plakken en voer het script dat is opgehaald uit het Team Services.
2. Wanneer u wordt gevraagd voor het configureren van de labels voor de agent, kies *Y* en voer *web*.
3. Wanneer u daarom wordt gevraagd voor het gebruikersaccount op te geven, drukt u op *retourneren* de standaardwaarden accepteren.
4. Wacht totdat het script voltooid met een bericht *vstsagent.account.computername Service is gestart*.
5. In de **implementatiegroepen** pagina van de **bouwen & Release** menu openen de *myIIS* implementatiegroep. Op de **Machines** tabblad, controleert u of uw virtuele machine wordt weergegeven.

    ![Virtuele machine is toegevoegd aan het Team Services, implementatiegroep](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-definition"></a>Release-definitie maken
Voor het publiceren van uw builds, maakt u een definitie van een release in Team Services. Deze definitie wordt automatisch geactiveerd door een geslaagde build van uw toepassing. Kies van de implementatiegroep voor de push-uw web-pakket te implementeren en de juiste IIS-instellingen te definiëren.

1. Kies **bouwen & Release**, selecteer daarna **Builds**. Kies de definitie van de build in de vorige stap hebt gemaakt.
2. Onder **onlangs voltooid**, kiest u de meest recente build en selecteer vervolgens **Release**.
3. Kies **Ja** een release-definitie maken.
4. Kies de **leeg** sjabloon, selecteer vervolgens **volgende**.
5. Controleer of de definitie van de build-project en de bron zijn gevuld met uw project.
6. Selecteer de **continue implementatie** in en selecteer **maken**.
7. Selecteer de vervolgkeuzelijst naast **+ taken toevoegen** en kies *toevoegen van een groep implementatiefase*.
    
    ![Taak release definitie in Team Services toevoegen](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. Kies **toevoegen** naast **IIS Web-App Deploy(Preview)**, selecteer daarna **sluiten**.
9. Selecteer de **uitvoeren op implementatiegroep** bovenliggende taak.
    1. Voor **Implementatiegroep**, selecteert u de implementatiegroep die u eerder hebt gemaakt, zoals *myIIS*.
    2. In de **Machine labels** de optie **toevoegen** en kies de *web* label.
    
    ![De definitie van implementatie groepstaak vrijgeven voor IIS](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. Selecteer de **implementeren: IIS Web-App implementeren** taak voor het configureren van de IIS-exemplaar-instellingen als volgt:
    1. Voor **Websitenaam**, voer *standaardwebsite*.
    2. Laat alle andere standaardinstellingen.
12. Kies **opslaan**, selecteer daarna **OK** twee keer.


## <a name="create-a-release-and-publish"></a>Een releaserecord maken en publiceren
U kunt nu uw web-push pakket als een nieuwe release implementeren. Deze stap communiceert met de agent op elke instantie die deel uitmaakt van de implementatiegroep, stuurt het web pakket implementeren en vervolgens configureert u IIS voor het uitvoeren van de bijgewerkte webtoepassing.

1. Selecteer in de definitie van de release **+ Release**, en kies vervolgens **maken Release**.
2. Controleer of de laatste build is geselecteerd in de vervolgkeuzelijst samen met **geautomatiseerde implementatie: na het maken van de release**. Selecteer **Maken**.
3. Een kort weergegeven dat aan de bovenkant van de definitie van de release, zoals *Release Release-1 is aangemaakt*. Selecteer de koppeling release.
4. Open de **logboeken** tabblad bekijkt u de voortgang van de release.
    
    ![Geslaagde teamservices release en web implementeren push pakket](media/tutorial-vsts-iis-cicd/successful_release.png)

5. Nadat de release voltooid is, open een webbrowser en voer het openbare IPP-adres van uw virtuele machine. Uw ASP.NET-webtoepassing wordt uitgevoerd.

    ![ASP.NET-web-app uitgevoerd op IIS VM](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-the-whole-cicd-pipeline"></a>Testen van de hele CI/CD-pipeline
Bij uw webtoepassing die wordt uitgevoerd op IIS, probeert u nu de hele CI/CD-pijplijn. Nadat u een wijziging aanbrengt in Visual Studio en doorvoeren uw code, een build wordt geactiveerd, die vervolgens een versie van uw web-bijgewerkte activeert, pakket implementeren naar IIS:

1. Open in Visual Studio de **Solution Explorer** venster.
2. Blader naar en open *myWebApp | Weergaven | Start | Index.cshtml*
3. Regel 6 lezen bewerken:

    `<h1>ASP.NET with VSTS and CI/CD!</h1>`

4. Sla het bestand op.
5. Open de **Team Explorer** Selecteer de *myWebApp* project en kies vervolgens **wijzigingen**.
6. Voer een bericht doorvoeren, zoals *testen CI/CD-pijplijn*, en kies vervolgens **alle doorvoeren en Sync** uit de vervolgkeuzelijst.
7. In de werkruimte Team Services, wordt een nieuwe build van het code-doorvoeren geactiveerd. 
    - Kies **bouwen & Release**, selecteer daarna **Builds**. 
    - Kies de definitie van de build en selecteer vervolgens de **in de wachtrij geplaatst & uitgevoerd** build moet letten tijdens de voortgang van de build.
9. Wanneer de build voltooid is, wordt een nieuwe release wordt geactiveerd.
    - Kies **bouwen & Release**, klikt u vervolgens **Releases** om te zien van het web naar uw VM IIS gepusht pakket implementeren. 
    - Selecteer de **vernieuwen** pictogram om de status te werken. Wanneer de *omgevingen* kolom ziet u een groen vinkje, de versie is geïmplementeerd naar IIS.
11. Overzicht van uw wijzigingen zijn toegepast, uw IIS-website in een browser te vernieuwen.

    ![ASP.NET-web-app uitgevoerd op IIS VM vanuit CI/CD-pipeline](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een ASP.NET-webtoepassing in een Team Services gemaakt en geconfigureerd build en release definities voor het implementeren van nieuwe web implementeren van pakketten naar IIS op elke doorvoer code. U leert hoe naar:

> [!div class="checklist"]
> * ASP.NET-webtoepassing op een project Team Services publiceren
> * Een build-definitie die wordt geactiveerd door doorvoeracties code maken
> * IIS installeren en configureren op een virtuele machine in Azure
> * Het IIS-exemplaar toevoegen aan een implementatiegroep in Team Services
> * Maken van een release definitie voor het publiceren van nieuwe web implementeren van pakketten naar IIS
> * De pijplijn CI/CD testen

Ga naar de volgende zelfstudie voor meer informatie over het beveiligen van een webserver met SSL-certificaten.

> [!div class="nextstepaction"]
> [Webserver met SSL beveiligde](tutorial-secure-web-server.md)