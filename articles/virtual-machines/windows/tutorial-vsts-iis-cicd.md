---
title: 'Zelfstudie: een CI/CD-pijplijn te maken in Azure met Azure DevOps-Services | Microsoft Docs'
description: In deze zelfstudie leert u over het maken van een Azure-Services voor DevOps-pijplijn voor continue integratie en levering waarmee een web-app in IIS op een Windows-VM in Azure wordt geïmplementeerd.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: zr-msft
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: zarhoads
ms.custom: mvc
ms.openlocfilehash: 4b4d514ec8bfd78b303a7f51c2a4072507da5be9
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471455"
---
# <a name="tutorial-create-a-continuous-integration-pipeline-with-azure-devops-services-and-iis"></a>Zelfstudie: Een pijplijn voor continue integratie met Azure DevOps-Services en IIS maken
Voor het automatiseren van de build-, test- en implementatiefasen van toepassingsontwikkeling, kunt u een continue integratie en implementatie (CI/CD)-pijplijn. In deze zelfstudie maakt u een CI/CD-pijplijn met behulp van Azure DevOps-Services en een Windows virtuele machine (VM) in Azure waarop IIS wordt uitgevoerd. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een ASP.NET-webtoepassing op een Azure-Services voor DevOps-project publiceren
> * Maken van een build-pijplijn die wordt geactiveerd door code doorvoeringen
> * IIS installeren en configureren op een virtuele machine in Azure
> * De IIS-exemplaar toevoegen aan een implementatiegroep in Azure DevOps-Services
> * Maken van een release-pijplijn voor het publiceren van nieuwe web-pakketten implementeren in IIS
> * De CI/CD-pijplijn testen

Voor deze zelfstudie is moduleversie 5.7.0 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="create-a-project-in-azure-devops-services"></a>Een project maken in Azure DevOps-Services
Azure DevOps-Services kunnen eenvoudig samenwerking en ontwikkeling zonder het onderhouden van een on-premises-oplossing voor het beheer van code. Azure DevOps-Services biedt cloud code testen, build en application insights. U kunt een versie besturingselement opslagplaats en IDE die het beste past bij de ontwikkeling van uw code. Voor deze zelfstudie, kunt u een beschikbare organisatie om een standaard ASP.NET-web-app en CI/CD-pijplijn te maken. Als u nog geen een organisatie die Azure DevOps-Services, [maakt u er een](http://go.microsoft.com/fwlink/?LinkId=307137).

Voor het beheren van de code van het doorvoerproces, pijplijnen bouwen en pipelines vrijgeven, een project maken in Azure DevOps-Services als volgt:

1. Uw Services van Azure DevOps-dashboard in een webbrowser openen en kies **nieuw project**.
2. Voer *myWebApp* voor de **projectnaam**. Laat u alle overige standaardwaarden gebruiken *Git* versiebeheer en *Agile* proces van het werkitem.
3. Kies de optie **delen met** *teamleden*en selecteer vervolgens **maken**.
5. Nadat uw project is gemaakt, kiest u de optie voor het **initialiseren met een Leesmij-bestand of gitignore**, klikt u vervolgens **initialiseren**.
6. Kies in het nieuwe project **Dashboards** aan de bovenzijde, selecteert u vervolgens **openen in Visual Studio**.


## <a name="create-aspnet-web-application"></a>ASP.NET-webtoepassing maken
In de vorige stap hebt u een project in Azure DevOps Services gemaakt. De laatste stap wordt het nieuwe project geopend in Visual Studio. Beheren van uw code doorvoeringen in de **Team Explorer** venster. Maken van een lokale kopie van het nieuwe project en maak vervolgens een ASP.NET-webtoepassing met een sjabloon als volgt:

1. Selecteer **kloon** te maken van een lokale git-opslagplaats van uw Services van Azure DevOps-project.
    
    ![Kloon de opslagplaats van de Services van Azure DevOps-project](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. Onder **oplossingen**, selecteer **nieuw**.

    ![Oplossing voor webtoepassing maken](media/tutorial-vsts-iis-cicd/new_solution.png)

3. Selecteer **Web** sjablonen, en kies vervolgens de **ASP.NET-webtoepassing** sjabloon.
    1. Voer een naam voor uw toepassing, zoals *myWebApp*, en schakel het selectievakje voor **map maken voor oplossing**.
    2. Als de optie beschikbaar is, schakel het selectievakje aan **Application Insights toevoegen aan project**. Application Insights, moet u toestemming van uw webtoepassing met Azure Application Insights. Deze om eenvoudig te houden in deze zelfstudie, moet u dit proces overslaan.
    3. Selecteer **OK**.
4. Kies **MVC** uit de lijst met sjablonen.
    1. Selecteer **Change Authentication**, kiest u **geen verificatie**en selecteer vervolgens **OK**.
    2. Selecteer **OK** om uw oplossing te maken.
5. In de **Team Explorer** venster, kiest u **wijzigingen**.

    ![Voer lokale wijzigingen door in Azure-opslagplaatsen Git-opslagplaats](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. Voer in het tekstvak doorvoeren een bericht zoals *eerste doorvoering*. Kies **alle doorvoeren en synchroniseren** uit de vervolgkeuzelijst.


## <a name="create-build-pipeline"></a>Build-pijplijn maken
In Azure DevOps-Services, kunt u een build-pijplijn gebruiken om te beschrijven hoe uw toepassing moet worden gemaakt. In deze zelfstudie maken we een eenvoudige pijplijn die wordt onze broncode, bouwt de oplossing en maakt vervolgens web pakket die we gebruiken kunnen voor het uitvoeren van de web-app op een IIS-server implementeren.

1. Kies in het project Azure DevOps-Services, **Build & Release** aan de bovenzijde, selecteert u vervolgens **bouwt**.
3. Selecteer **+ New definition**.
4. Kies de **ASP.NET (PREVIEW)** sjabloon en selecteer **toepassen**.
5. Laat alle standaard waarden van de taak. Onder **bronnen ophalen**, zorg ervoor dat de *myWebApp* opslagplaats en *master* vertakking worden geselecteerd.

    ![Build-pijplijn in Services van Azure DevOps-project maken](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. Op de **Triggers** tabblad, verplaats de schuifregelaar voor **inschakelen van deze trigger** naar *ingeschakeld*.
7. De build-pijplijn en de wachtrij een nieuwe build opslaan door **opslaan en in de wachtrij** , klikt u vervolgens **opslaan en in de wachtrij** opnieuw. Laat de standaardwaarden en selecteer **wachtrij**.

Bekijk begint als de build is gepland op een host agent vervolgens te bouwen. De uitvoer lijkt op die in het volgende voorbeeld:

![Geslaagde build van de Services van Azure DevOps-project](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a>Virtuele machine maken
Voor een platform voor het uitvoeren van uw ASP.NET-web-app, moet u een virtuele machine van Windows waarop IIS wordt uitgevoerd. Azure DevOps-Services maakt gebruik van een agent om te communiceren met de IIS-exemplaar als u code doorvoert en builds worden geactiveerd.

Maak een Windows Server 2016-VM met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM* in de *VS-Oost* locatie. De resourcegroep *myResourceGroupAzureDevOpsServices* en ondersteunende netwerkbronnen ook worden gemaakt. Om toe te staan webverkeer te genereren, TCP-poort *80* wordt geopend met de virtuele machine. Geef desgevraagd een gebruikersnaam en wachtwoord moet worden gebruikt als de aanmeldingsreferenties voor de virtuele machine:

```powershell
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a virtual machine
New-AzureRmVM `
  -ResourceGroupName "myResourceGroupAzureDevOpsServices" `
  -Name "myVM" `
  -Location "East US" `
  -ImageName "Win2016Datacenter" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -SecurityGroupName "myNetworkSecurityGroup" `
  -PublicIpAddressName "myPublicIp" `
  -Credential $cred `
  -OpenPorts 80
```

Voor verbinding met uw virtuele machine, verkrijgen van het openbare IP-adres met [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) als volgt:

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select IpAddress
```

Een extern-bureaubladsessie met uw virtuele machine maken:

```cmd
mstsc /v:<publicIpAddress>
```

Open op de virtuele machine, een **beheerder PowerShell** opdrachtprompt. Installeer IIS en de vereiste .NET-functies als volgt:

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a>Implementatiegroep maken

Distribueren Internet pakket implementeren op de IIS-server, het definiëren van een implementatiegroep in Azure DevOps-Services. Deze groep kunt u opgeven welke servers het doel van de nieuwe builds worden als u code naar Services van Azure DevOps doorvoert en builds worden voltooid.

1. Kies in Azure DevOps-Services, **Build & Release** en selecteer vervolgens **implementatiegroepen**.
2. Kies **toevoegen implementatiegroep**.
3. Voer een naam voor de groep, zoals *myIIS*en selecteer vervolgens **maken**.
4. In de **machines registreren** sectie, controleert u of *Windows* is ingeschakeld, wordt het selectievakje **gebruiken een persoonlijk toegangstoken in het script voor verificatie**.
5. Selecteer **script naar Klembord kopiëren**.


### <a name="add-iis-vm-to-the-deployment-group"></a>IIS-VM toevoegen aan de implementatiegroep
Toevoegen aan de implementatiegroep is gemaakt, elk IIS-exemplaar aan de groep. Azure DevOps-Services genereert een script dat wordt gedownload en configureert u een agent op de virtuele machine die ontvangt van de nieuwe web-pakketten implementeren en vervolgens toegepast op IIS.

1. Klik in de **beheerder PowerShell** sessie op de virtuele machine, plakken en voer het script opgehaald uit Azure DevOps-Services.
2. Wanneer u hierom wordt gevraagd voor het configureren van tags voor de agent, kiest u *Y* en voer *web*.
3. Wanneer u hierom wordt gevraagd voor het gebruikersaccount op te geven, drukt u op *retourneren* naar de standaardwaarden te accepteren.
4. Wacht totdat het script dat moet worden afgerond met een bericht *vstsagent.account.computername Service is gestart*.
5. In de **implementatiegroepen** pagina van de **Build & Release** menu, open de *myIIS* implementatiegroep. Op de **Machines** tabblad, zorg ervoor dat uw virtuele machine wordt weergegeven.

    ![Virtuele machine is toegevoegd aan Azure DevOps Services implementatiegroep](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-pipeline"></a>Release-pijplijn maken
Voor het publiceren van uw builds, maakt u een release-pijplijn in Azure DevOps-Services. Deze pijplijn wordt automatisch geactiveerd door een build van uw toepassing. U hebt gekozen de implementatiegroep om uw web-pakket dat u wilt implementeren, en definieert u de juiste IIS-instellingen.

1. Kies **Build & Release**en selecteer vervolgens **bouwt**. Kies de build-pijplijn in een vorige stap hebt gemaakt.
2. Onder **onlangs voltooid**, kies de meest recente build, en selecteer vervolgens **Release**.
3. Kies **Ja** om een release-pijplijn te maken.
4. Kies de **leeg** sjabloon, selecteert u vervolgens **volgende**.
5. Controleer of de build-pijplijn project en de bron worden ingevuld met uw project.
6. Selecteer de **continue implementatie** selectievakje, en selecteer vervolgens **maken**.
7. Selecteer de vervolgkeuzelijst naast **+ toevoegen taken** en kies *toevoegen van een groep implementatiefase*.
    
    ![Taak voor de release-pijplijn in Azure DevOps-Services toevoegen](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. Kies **toevoegen** naast **IIS Web-App Deploy(Preview)** en selecteer vervolgens **sluiten**.
9. Selecteer de **uitvoeren op implementatiegroep** bovenliggende taak.
    1. Voor **Implementatiegroep**, selecteert u de implementatiegroep die u eerder hebt gemaakt, zoals *myIIS*.
    2. In de **Machine tags** Schakel **toevoegen** en kies de *web* tag.
    
    ![Release-pijplijn implementatie groepstaak voor IIS](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. Selecteer de **implementeren: IIS Web-App implementeren** taak voor het configureren van de IIS-exemplaar-instellingen als volgt te werk:
    1. Voor **Websitenaam**, voer *Default Web Site*.
    2. Laat u alle overige standaardinstellingen.
12. Kies **opslaan**en selecteer vervolgens **OK** twee keer.


## <a name="create-a-release-and-publish"></a>Een release te maken en publiceren
U kunt nu uw web-push implementeren pakket als een nieuwe versie. Deze stap communiceert met de agent op elke instantie die deel uitmaakt van de implementatiegroep, het web pushes pakket implementeren, en vervolgens configureert u IIS voor het uitvoeren van de bijgewerkte web-App.

1. Selecteer in de release-pijplijn **+ Release**, en kies vervolgens **Create Release**.
2. Controleer of de nieuwste build is geselecteerd in de vervolgkeuzelijst, samen met **geautomatiseerde implementatie: na het maken van release**. Selecteer **Maken**.
3. Kleine wordt een banner weergegeven aan de bovenkant van uw release-pijplijn, zoals *Release 'Release-1' is gemaakt*. Selecteer de koppeling release.
4. Open de **logboeken** tab om de voortgang van de release bekijken.
    
    ![Geslaagde release van Azure DevOps-Services en web implementeren pakket push](media/tutorial-vsts-iis-cicd/successful_release.png)

5. Nadat de release voltooid is, open een webbrowser en voer het openbare IPP-adres van uw virtuele machine. Uw ASP.NET-webtoepassing wordt uitgevoerd.

    ![ASP.NET-web-app die wordt uitgevoerd op IIS VM](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-the-whole-cicd-pipeline"></a>De volledige CI/CD-pijplijn testen
Met uw webtoepassing die wordt uitgevoerd op IIS, probeer het nu de volledige CI/CD-pijplijn. Nadat u een wijziging aanbrengt in Visual Studio en dat uw code, een build is geactiveerd en die vervolgens wordt er een versie van uw bijgewerkte web kunt u de pakket implementeren op IIS:

1. Open in Visual Studio, de **Solution Explorer** venster.
2. Navigeer naar en open *myWebApp | Weergaven | Start | Index.cshtml*
3. Regel 6 lezen bewerken:

    `<h1>ASP.NET with Azure DevOps Services and CI/CD!</h1>`

4. Sla het bestand op.
5. Open de **Team Explorer** venster de *myWebApp* project en kies vervolgens **wijzigingen**.
6. Voer een doorvoerbericht zoals *testen CI/CD-pijplijn*, en kies vervolgens **alles doorvoeren en synchroniseren** uit de vervolgkeuzelijst.
7. In de werkruimte van Azure DevOps-Services, wordt een nieuwe build van het doorvoeren van de code geactiveerd. 
    - Kies **Build & Release**en selecteer vervolgens **bouwt**. 
    - Kies uw build-pijplijn, en selecteer vervolgens de **in de wachtrij geplaatst & uitgevoerd** build te kijken de voortgang van de build.
9. Wanneer de build voltooid is, wordt een nieuwe versie wordt geactiveerd.
    - Kies **Build & Release**, klikt u vervolgens **Releases** om te zien van het web pakket gepusht naar uw IIS-VM implementeren. 
    - Selecteer de **vernieuwen** pictogram om de status te werken. Wanneer de *omgevingen* kolom ziet u een groen vinkje, de versie is geïmplementeerd in IIS.
11. Als u wilt zien van uw wijzigingen worden toegepast, vernieuwt u uw IIS-website in een browser.

    ![ASP.NET-web-app uitgevoerd op IIS VM van CI/CD-pijplijn](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een ASP.NET-toepassing in Azure DevOps-Services die zijn gemaakt en geconfigureerd build en release-pijplijnen voor het implementeren van nieuwe web-pakketten implementeren in IIS op elke doorvoercode. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een ASP.NET-webtoepassing op een Azure-Services voor DevOps-project publiceren
> * Maken van een build-pijplijn die wordt geactiveerd door code doorvoeringen
> * IIS installeren en configureren op een virtuele machine in Azure
> * De IIS-exemplaar toevoegen aan een implementatiegroep in Azure DevOps-Services
> * Maken van een release-pijplijn voor het publiceren van nieuwe web-pakketten implementeren in IIS
> * De CI/CD-pijplijn testen

Ga verder met de volgende zelfstudie voor informatie over het installeren van een SQL&#92;IIS&#92;.NET-stack op een combinatie van een Windows-VM's.

> [!div class="nextstepaction"]
> [SQL&#92;IIS&#92;.NET-stack](tutorial-iis-sql.md)
