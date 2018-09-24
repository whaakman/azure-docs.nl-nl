---
title: Maak een edge-machine learning-oplossing met Azure en Azure Stack | Microsoft Docs
description: Informatie over het maken van een rand machine learning-oplossing met behulp van Python met Azure en Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 30dbdff584f1bea955072e96a5e0f03cfe4c92c1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963727"
---
# <a name="tutorial-create-an-edge-machine-learning-solution-with-azure-and-azure-stack"></a>Zelfstudie: Een edge-machine learning-oplossing met Azure en Azure Stack maken

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Informatie over het maken van een edge-machine learning-oplossing met Azure en Azure Stack.

In deze zelfstudie bouwt u een Voorbeeldomgeving:

> [!div class="checklist"]
> - Maak een opslagaccount en container schoon gegevens zich bevinden.
> - Een Ubuntu Data Science Virtual Machine (DSVM) maken in Azure portal.
> - Azure Machine Learning-Services in Azure maken en trainen van modellen implementeren.
> - Azure Machine Learning-services-accounts maken.
> - Implementeren en gebruiken van Azure Container Registry.
> - Een Kubernetes-cluster implementeren in Azure Stack.
> - Maak een Azure Stack-opslagaccount en de Storage-wachtrij voor gegevens.
> - Maak een nieuwe Azure Stack-functie voor het verplaatsen van de schoon gegevens uit Azure Stack in Azure.

**Wanneer u deze oplossing**

 -  Uw organisatie gebruikmaakt van een DevOps-methode, of heeft een gepland voor de nabije toekomst.
 -  Wilt u CI/CD-praktijken implementeren in uw Azure Stack-implementatie en de openbare cloud.
 -  Wilt u de CI/CD-pijplijn consolideren tussen cloud en on-premises omgevingen.
 -  Wilt u de mogelijkheid voor het ontwikkelen van toepassingen die gebruikmaken van de cloud of on-premises services.
 -  U wilt gebruiken voor consistente ontwikkelvaardigheden voor cloud en on-premises toepassingen.

> [!Tip]  
> ![hybride-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack is een uitbreiding van Azure. Azure Stack brengt de flexibiliteit en innovatie van cloud computing naar uw on-premises omgeving en de enige hybride cloud waarmee u waar dan ook hybride apps bouwen en implementeren in te schakelen.  
> 
> Het technische document [ontwerpoverwegingen voor hybride toepassingen](https://aka.ms/hybrid-cloud-applications-pillars) beoordelingen pijlers van softwarekwaliteit (plaatsing, schaalbaarheid, beschikbaarheid, tolerantie, beheerbaarheid en beveiliging) voor het ontwerpen, implementeren en besturingssysteem hybride toepassingen. De overwegingen bij het ontwerpen helpen bij het optimaliseren van hybride toepassingsontwerp, uitdagingen in een productieomgeving te minimaliseren.

## <a name="prerequisites"></a>Vereisten

Een aantal onderdelen zijn vereist voor het bouwen van deze use case en duurt enige tijd om voor te bereiden:

 -  Een Azure-Partner voor OEM/Hardware kan een productie Azure Stack te implementeren en alle gebruikers kunnen een ASDK implementeren

 -  Een Azure Stack-Operator moet ook de App Service implementeren, plannen en aanbiedingen maken, maakt u een tenantabonnement en de installatiekopie van Windows Server 2016 toevoegen

 -  Een App Service en hybride netwerk installatie vereist is (meer informatie over [App-integratie met VNets.](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet))

 -  De particuliere [Build and Release Agent](https://docs.microsoft.com/vsts/pipelines/agents/agents?view=vsts) voor VSTS-integratie moet worden ingesteld voorafgaand aan de implementatie (Zorg ervoor dat eventuele bestaande onderdelen gebruikt aan de vereisten voordat u begint.)

Kennis van Azure en Azure Stack is vereist. Beginnen met de volgende onderwerpen voor meer informatie voordat u doorgaat:

 -  [Inleiding tot Azure](https://azure.microsoft.com/overview/what-is-azure/)

 -  [Azure Stack-basisbegrippen](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

 -  [Azure Stack hybride CI/CD-handleiding](/azure-stack-solution-pipeline.md)

**Azure**

 -  Een Azure-abonnement (maken een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))

 -  De URL van een nieuwe Web-App die zijn gemaakt door [Web-App](https://docs.microsoft.com/azure/app-service/app-service-web-overview) in Azure

 -  Implementatie van [Azure Container Services (ACS) Kubernetes op Azure](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

 -  Implementatie van Azure Machine Learning-service (preview) [4-delige zelfstudie](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/tutorial-classifying-iris-part-1)

 -  Azure Machine Learning experimenten [account](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/experimentation-service-template)

**Azure Stack**

 -  Een geïntegreerde Azure Stack-systeem of de implementatie van Azure Stack Development Kit.

    - U vindt instructies voor het installeren van Azure Stack op [installeren van de Azure Stack Development Kit](/articles/azure-stack/asdk/asdk-install).
     - [https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1) Deze installatie is mogelijk een paar uur te voltooien.

 -  Implementatie van [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS-services met Azure Stack

 -  Een [-Plan/aanbiedingen](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) binnen de Azure Stack-omgeving

 -  Een [Tenant abonnement](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) binnen de Azure Stack-omgeving

 -  Een Ubuntu Server VM-installatiekopie (beschikbaar in [Azure Stack Marketplace](https://buildazure.com/2016/05/04/azure-marketplace-ubuntu-server-16-04-lts/)) deze virtuele machine in het tenantabonnement in de Azure Stack als de agent privéversie, evenals de Kubernetes-VM's worden samengesteld. Als deze afbeelding niet beschikbaar is, kan de Azure Stack-Operator om te controleren of dat deze is toegevoegd aan de omgeving helpen. (Gebruik niet de 18.04 build van ubuntu, omdat het momenteel niet wordt ondersteund.)

 -  Een Web-App binnen de tenantabonnement (Opmerking: de URL van de nieuwe Web-App voor later gebruik.)

 -  Implementatie van VSTS-op basis van Linux persoonlijke bouwen Agent virtuele Machine, binnen het tenantabonnement

 -  Implementatie van een [Azure Container Services (ACS) Kubernetes op Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

**Hulpprogramma's voor ontwikkelaars**

 -  [VSTS-werkruimte](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) (het aanmeldingsproces maakt een project met de naam "MyFirstProject")

 -  [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) installatie en [VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services) aanmelding

 -  [Lokale kloon](https://www.visualstudio.com/docs/git/gitquickstart) van project

 -  [Linux-subsysteem voor Windows 10](https://docs.microsoft.com/windows/wsl/install-win10) installatie (voor BASH en SSH)

 -  [Docker voor Windows](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe) installatie

 -  [Azure Machine Learning Workbench (preview)](https://aka.ms/azureml-wb-msi) installatie

 -  [Python](https://www.python.org/ftp/python/3.7.0/python-3.7.0rc1-amd64.exe) omgeving installatie

**VSTS**

 -  **VSTS-Account.** Snel instellen van continue integratie voor het bouwen, testen en implementeren. Zie voor meer informatie over het VSTS-accounts [het VSTS-Account maken](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student?view=vsts).

 -  **Codeopslagplaats.** Met bestaande code-opslagplaatsen in GitHub, BitBucket, DropBox, Onedrive en TFS, het VSTS-platform kan gebruikmaken van meerdere opslagplaatsen voor codes voor het stroomlijnen van de ontwikkelingspijplijn. Zie voor meer informatie over code opslagplaatsen [aan de slag met Git en VSTS](https://docs.microsoft.com/vsts/git/gitquickstart?view=vsts&tabs=visual-studio) zelfstudie.

 -  **Service-verbinding.** Verbinding maken met externe en externe services voor het uitvoeren van taken voor het testen, build of implementatie. Zie voor meer informatie over VSTS serviceverbindingen [Service-eindpunten voor Build and Release](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints?view=vsts).

 -  **Bouw definities.** Geautomatiseerde buildprocessen definiëren en stel een aantal taken via de taak catalogus. Zie voor meer informatie over build definities [maken van een Builddefinitie](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1?view=vsts) documentatie.

 -  **Release-definities.** Definieer het implementatieproces voor een verzameling van omgevingen waarin App-artefacten zijn geïmplementeerd. Zie voor meer informatie over release definities [Release-definitie maken](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1?view=vsts) documentatie.

 -  **Gehoste VSTS Build van de Linux-Agentpool.** Snel bouwen, testen en implementeren van toepassingen met behulp van een door Microsoft beheerd en onderhouden die worden gehost agent. Zie voor meer informatie over gehoste VSTS build agents [gehoste Agents](https://docs.microsoft.com/vsts/build-release/concepts/agents/hosted?view=vsts) documentatie.

## <a name="step-1-create-a-storage-account"></a>Stap 1: Een storage-account maken

Maak een opslagaccount en container schoon gegevens zich bevinden.

1.  Aanmelden bij de [ *Azure-portal*](https://portal.azure.com/).

2.  Vouw in de Azure-portal het menu aan de linkerkant op het menu met services te openen en kies **alle Services**. Schuif omlaag naar **opslag** en kies **opslagaccounts**. In de ** Opslagaccounts ** venster Kies **toevoegen**.

3.  Voer een naam voor het opslagaccount.

    > [!Note]  
    > Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. Naam van het opslagaccount moet uniek zijn binnen Azure. De Azure portal wordt aangegeven als de opslagaccountnaam die is geselecteerd al gebruikt wordt.

4.  Geef het implementatiemodel moet worden gebruikt: **Resource Manager**.

5.  Selecteer het type opslagaccount: **algemeen gebruik V1**, geeft u de prestatielaag: **Standard**.

6.  Selecteer de replicatieoptie voor het opslagaccount: **GRS**.

7.  Selecteer Nieuw abonnement op storage-account.

8.  Geef een nieuwe resourcegroep op of selecteer een bestaande resourcegroep.

9.  Selecteer de geografische locatie voor het opslagaccount.

10. Selecteer **Maken** om het opslagaccount te maken.

    ![Alternatieve tekst](\media\azure-stack-solution-machine-learning\image1.png)

11.  Kies de opslagaccount die onlangs zijn gemaakt.

12.  Selecteer op **Blobs**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image2.png)

13.  Selecteer op **+ Container** en selecteert u een op **Container**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image3.png)

14.  Geef de naam op voor de container **uploadeddata** en kies het toegangstype **Container**.

15.  Selecteer op **maken**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image4.png)

## <a name="step-2-create-a-data-science-virtual-machine"></a>Stap 2: Maak een virtuele Machine voor Datatechnologie

Maak een Ubuntu Data Science Virtual Machine (DSVM) in Azure portal.

1.  Meld u aan bij Azure-portal [https://portal.azure.com](https://portal.azure.com/)

2.  Selecteer op de **+ nieuw** koppeling en zoek naar 'Data Science Virtual Machine voor Linux Ubuntu CSP

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image5.png)

1.  Kies **Data Science Virtual Machine voor Linux (Ubuntu)** in de lijst en volg de aanwijzingen op het scherm instructies voor het maken van de DSVM.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image6.png)

> ! [Belangrijk]  
> **Kies** wachtwoord ** als de*verificatietype*.

Plaats de nieuwe DSVM in dezelfde resourcegroep bevinden als de zojuist gemaakte opslagaccount. Alle Edge ML-objecten worden geïmplementeerd in Azure in deze resourcegroep.

1.  In de instellingen configureert u optionele functies

    a.  Selecteer de **Opslagaccount** eerder hebt gemaakt.

    b.  Maak een nieuwe **Virtueelnetwerk**, **Subnet**, en **openbaar IP-adres** deze moet standaard maken een naam op basis van de naam van de resourcegroep.

    c.  Maak een nieuwe **NSG** wordt dit automatisch moet maken met de juiste regels die al is toegepast.

    d.  Voor de **Opslagaccount voor diagnostische gegevens**, selecteer het opslagaccount dat eerder hebt gemaakt.

    e.  Opmerking: Bij AAD ingeschakeld en geconfigureerd voor het Azure-abonnement, beheerde Service-identiteit kunnen ook worden ingeschakeld.

2.  Selecteer **OK**.

### <a name="connect-to-the-dsvm"></a>Verbinding maken met de DSVM

Zodra de DSVM is gemaakt, verbinden met de virtuele machine van Windows-subsysteem voor Linux.

```Bash  
    ssh <user>@<DSVM Public IP>
```

1.  Typ Ja wanneer u wordt gevraagd om te bevestigen van de VM-verbinding.

2.  Voer het wachtwoord voor de DSVM gemaakt.

### <a name="update-the-dsvm"></a>De DSVM bijwerken 

```Bash  
sudo su 
apt-get update 
apt-get -y upgrade 
apt-get -y dist-upgrade 
apt-get -y autoremove
```

## <a name="step-3-deploy-azure-machine-learning-services"></a>Stap 3: Azure Machine Learning-Services implementeren

Azure Machine Learning-Services in Azure implementeren.

Azure Machine Learning-services (preview) zijn een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse. Het helpt professionele gegevenswetenschappers gegevens voor te bereiden, experimenten te ontwikkelen en modellen te implementeren op cloudschaal.

Deze sectie wordt uitgelegd:

> [!div class="checklist"]
> - Serviceaccounts maken voor Azure Machine Learning-services
> - Azure Machine Learning Workbench installeren en erbij aanmelden.
> - Een project maken in Workbench
> - Een script in dat project uitvoeren
> - De CLI (opdrachtregelinterface) openen

Azure Machine Learning-services maken deel uit van het portfolio van Microsoft Azure en vereisen een Azure-abonnement. Als u een Azure-abonnement, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

Voldoende machtigingen zijn vereist om activa zoals resourcegroepen, virtuele Machines en andere items te maken.

De Azure Machine Learning Workbench-toepassing kan worden geïnstalleerd op de volgende besturingssystemen:

 -  Windows 10 of Windows Server 2016
 -  macOS Sierra of High Sierra

## <a name="step-4-create-azure-machine-learning-services"></a>Stap 4: Azure Machine Learning-services maken

Azure Machine Learning-services-accounts maken.

Gebruik de Azure-portal voor het inrichten van de Azure Machine Learning-accounts:

1.  Aanmelden bij de [Azure-portal](https://portal.azure.com/) met behulp van de referenties voor het Azure-abonnement moet worden gebruikt. Als u een Azure-abonnement, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image7.png)

1.  Selecteer de knop **Een resource maken** (+) in de linkerbovenhoek van de portal.

    ![Een resource maken in Azure Portal](media\azure-stack-solution-machine-learning\image8.png)

1.  Voer **Machine Learning** in de zoekbalk in. Selecteer het zoekresultaat met de naam **Machine Learning Experimenten (preview)**.

    ![Azure Machine Learning-zoekfunctie](media\azure-stack-solution-machine-learning\image9.png)

1.  In de **Machine Learning experimenten** deelvenster, schuif naar beneden en selecteer **maken** om te beginnen met het definiëren van het experimenten-account.

    ![Azure Machine Learning - Experimenten-account maken](media\azure-stack-solution-machine-learning\image10.png)

1.  In de **ML experimenten** deelvenster de Machine Learning experimenten-account configureren.

    | Instelling | Aanbevolen waarde voor zelfstudie | Beschrijving |
    |---------------------------------------|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Naam experimenteeraccount | Unieke naam | Voer een unieke naam ter identificatie van het account. Gebruik de naam van een afdeling of project die het beste het experiment identificeert. De naam moet minimaal 2 en maximaal 32 tekens lang zijn. De naam mag alleen alfanumerieke tekens en streepjes (-) bevatten. |
    | Abonnement | Het abonnement | Kies het Azure-abonnement moet worden gebruikt voor het experiment. Als er meerdere abonnementen bestaan, kiest u het abonnement waarin de resource wordt in rekening gebracht. |
    | Resourcegroep | De resourcegroep | Gebruik een bestaande resourcegroep in het abonnement of voer een naam in om een nieuwe resourcegroep voor dit experimenten-account te maken. |
    | Locatie | De regio het dichtst bij de gebruikers | Kies de locatie die het dichtst bij de gebruikers en de gegevensresources. |
    | Aantal seats | 2 | Voer het aantal seats in. Meer informatie over [de invloed van seats op prijzen](https://azure.microsoft.com/pricing/details/machine-learning/).<br><br>In deze Quick Start zijn slechts twee seats nodig. Seats kunnen naar behoefte worden toegevoegd of verwijderd in Azure Portal. |
    | Storage-account | Unieke naam | Selecteer **Nieuwe maken** en geef een naam op om een [Azure-opslagaccount](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal) te maken. De naam mag alleen 3 tot 24 alfanumerieke tekens en bevatten. U kunt ook selecteren **gebruik bestaande** en selecteert u het bestaande storage-account in de lijst. Het opslagaccount is vereist en wordt gebruikt voor het bewaren van projectartefacten en uitvoeringsgeschiedenisgegevens. |
    | Werkruimte voor Experimenten-account | IrisGarden<br>(naam gebruikt in zelfstudies) | Geef een naam op voor een werkruimte voor dit account. De naam moet minimaal 2 en maximaal 32 tekens lang zijn. De naam mag alleen alfanumerieke tekens en streepjes (-) bevatten. Deze werkruimte bevat hulpprogramma's die nodig zijn voor het maken, beheren en publiceren van experimenten. |
    | Eigenaar van de werkruimte toewijzen | Het account | Selecteer het eigen account als de eigenaar van de werkruimte. |
    | Modelbeheer-account maken | **controleren** | Maak een account voor Modelbeheer. Dit wordt gebruikt om te implementeren en beheren van de modellen als realtime webservices. <br><br>Hoewel dit optioneel is, wordt het maken van het Modelbeheer-account op hetzelfde moment als het experimenten-account aanbevolen. |
    | Accountnaam | Unieke naam | Kies een unieke naam ter identificatie van het Modelbeheer-account. Gebruik de afdelingen of naam van het project die het best het experiment identificeert. De naam moet minimaal 2 en maximaal 32 tekens lang zijn. De naam mag alleen alfanumerieke tekens en streepjes (-) bevatten. |
    | Prijscategorie voor Modelbeheer | **DEVTEST** | Selecteer **geen prijscategorie geselecteerd** om de prijscategorie voor de nieuwe Modelbeheer-account te geven. Selecteer om kosten te besparen, de DEVTEST-prijscategorie als deze beschikbaar op het abonnement (beperkte beschikbaarheid is). U kunt ook de prijscategorie S1 selecteren. Kies selecteren om op te slaan van de geselecteerde prijscategorie. |
    | Vastmaken aan dashboard | controleren | Selecteer de **vastmaken aan dashboard** optie voor het eenvoudig bijhouden van de Machine Learning experimenten-account op de eerste dashboardpagina van Azure portal. |

    ![Machine Learning Experimenten-accountconfiguratie](media\azure-stack-solution-machine-learning\image11.png)

1.  Selecteer **Maken** om te beginnen met het proces voor het maken van het Experimenten-account en het Modelbeheer-account.

    ![Machine Learning Experimenten-accountconfiguratie](media\azure-stack-solution-machine-learning\image12.png)

    Het duurt een paar seconden om een account te maken. Controleer de status van het implementatieproces door het selecteren van het meldingspictogram (de klok) op de werkbalk van de Azure portal.

    ![Meldingen in Azure-portal](media\azure-stack-solution-machine-learning\image13.png)

### <a name="install-and-log-in-to-workbench"></a>Installeren en meld u aan bij workbench 

Azure Machine Learning Workbench is beschikbaar voor Windows of macOS. Overzicht van [ondersteunde platforms](https://docs.microsoft.com/azure/machine-learning/service/quickstart-installation).

**Waarschuwing:** het installeren kan ongeveer een uur duren om uit te voeren.

1.  Download het meest recente Workbench-installatieprogramma en start het.

    > [!Important]  
    > Download het installatieprogramma volledig op schijf en voer het daarvandaan uit. Voer het niet rechtstreeks vanuit de downloadwidget van de browser.<br>**Op Windows:<br>**  een. Download [AmlWorkbenchSetup.msi](https://aka.ms/azureml-wb-msi).<br>b. Dubbelklik op het gedownloade installatieprogramma in Verkenner.

1.  Volg de aanwijzingen op het scherm instructies in het installatieprogramma is voltooid.

    ** De installatie kan zo veel 30 minuten duren. **
    
    `Windows: C:\\Users\\<user>\\AppData\\Local\\AmlWorkbench`
    
    Het installatieprogramma wordt gedownload en instellen van de vereiste afhankelijkheden, zoals Python, Miniconda en andere gerelateerde bibliotheken. Deze installatie omvat ook het Azure platformoverschrijdende opdrachtregelprogramma of Azure CLI.

1.  Start Workbench door de knop **Workbench starten** in het laatste scherm van het installatieprogramma te selecteren.

    Als het installatieprogramma wordt afgesloten, start met behulp van de **Machine Learning Workbench** snelkoppeling op het bureaublad.

1.  Selecteer **aanmelden bij Microsoft** om te verifiëren met Azure Machine Learning Workbench. Gebruik de dezelfde referenties gebruikt in de Azure-portal voor het maken van de accounts voor experimenten en Modelbeheer.

    Nadat u bent aangemeld, maakt Workbench gebruik van het eerste experimenten-account wordt gevonden in de Azure-abonnementen, en alle werkruimten en projecten die zijn gekoppeld aan dat account worden weergegeven.

    > [!Tip]  
    > Schakel over naar een ander experimentaccount met behulp van het pictogram in de linkerbenedenhoek van het toepassingsvenster van Workbench.

### <a name="create-a-new-project-in-workbench"></a>Maak een nieuw project in workbench

1.  Open de app Azure Machine Learning Workbench en meld u indien nodig.

    - Op Windows, start u met behulp van de **Machine Learning Workbench** snelkoppeling op het bureaublad.

    - Selecteer in macOS **Azure ML Workbench** in Launchpad.

1.  Klik in het deelvenster **PROJECTS** op het plusteken (+) om **New Project** te kiezen.

    ![Nieuwe werkruimte](media\azure-stack-solution-machine-learning\image14.png)

1.  Vul de formuliervelden in en selecteer de knop **Maken** om een nieuw project te maken in Workbench.

    | Veld | Aanbevolen waarde voor zelfstudie | Beschrijving |
    |-------------------------------------|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Projectnaam | myIris | Voer een unieke naam ter identificatie van het account. Gebruik de afdelingen of naam van het project die het best het experiment identificeert. De naam moet minimaal 2 en maximaal 32 tekens lang zijn. De naam mag alleen alfanumerieke tekens en streepjes (-) bevatten. |
    | Projectmap | c:\Temp\ | Selecteer de map waarin het project moet worden gemaakt. |
    | Projectbeschrijving | Leeg laten | Optioneel veld, nuttig voor het beschrijven van de projecten. |
    | URL van GIT-opslagplaats Visualstudio.com | Leeg laten | Optioneel veld. Een project koppelen aan een Git-opslagplaats in Visual Studio Team Services voor broncodebeheer en samenwerking. [Meer informatie over het instellen van een opslagplaats](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/using-git-ml-project). |
    | Geselecteerde werkruimte | IrisGarden (indien aanwezig) | Kies een werkruimte gemaakt voor de experimenten-account in Azure portal. <br>Met behulp van Quickstart, wordt de werkruimte met de naam IrisGarden vermeld. Gebruik anders de werkruimte met de naam van het experimenten-account of de naam van een voorkeur. |
    | Projectsjabloon | Iris classificeren | Sjablonen bevatten scripts en gegevens die worden gebruikt voor het verkennen van het product. Deze sjabloon bevat de scripts en gegevens die nodig zijn voor deze snelstart en andere zelfstudies op deze documentatiesite. |

    ![Nieuw project](media\azure-stack-solution-machine-learning\image15.png)

1.  Er wordt een nieuw project gemaakt en het projectdashboard wordt geopend met dat project. Verken de startpagina van project, gegevensbronnen, notitieblokken en broncodebestanden.

    ![Project openen](media\azure-stack-solution-machine-learning\image16.png)

### <a name="attach-a-dsvm-compute-target"></a>Een DSVM compute-doel toevoegen

Zodra de DSVM is gemaakt, koppelt u deze aan het project Azure ML.

1.  In de Azure ML Workbench-app, start de CLI van Azure ML Workbench door te selecteren **bestand**->**Open PowerShell**

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image17.png)

1.  Zodra de prompt is geopend PowerShell de volgende opdracht gebruiken:

    ```PowerShell  
        az login
    ```

1.  Het volgende bericht:

     ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image18.png)

1.  Blader naar de site zoals beschreven in de opdrachtprompt en voer de code die wordt geleverd.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image19.png)

1.  Selecteer Doorgaan wanneer hierom wordt gevraagd, en selecteer vervolgens het Azure-account dat het experimentele Azure ML-Account is gekoppeld.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image20.png)

1.  De Azure ML Workbench CLI stuurt vervolgens het volgende bericht:

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image21.png)

1.  Wanneer de aanmelding voor ML-Account en werkruimte wordt weergegeven als geslaagd, koppelt u de DSVM.

    ```PowerShell  
        az ml computetarget attach remotedocker --name <compute target name> --address <ip address or FQDN> --username <admin username> --password <admin password>
    ```

    De volgende melding wordt weergegeven:

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image22.png)

    ```PowerShell  
        # prepare the Docker image on the DSVM 
        az ml experiment prepare -c <compute target name>
    ```

Dit proces kan enige tijd duren en genereert een aanzienlijke hoeveelheid tekst omdat deze wordt deze verschillende docker-installatiekopieën, ze registreert en vervolgens van toepassing is de benodigde code en de toepassingen toe.

Experimenten kunnen nu worden uitgevoerd op deze DSVM.

### <a name="create-a-data-preparation-package"></a>Een gegevensvoorbereidingspakket maken

Vervolgens beginnen met het voorbereiden van de gegevens in Azure Machine Learning Workbench. Elke transformatie die wordt uitgevoerd in Workbench wordt opgeslagen in een JSON-indeling in een lokaal gegevensvoorbereidingspakket (\*.dprep-bestand). Dit gegevensvoorbereidingspakket is de primaire container voor de gegevensvoorbereidingswerkzaamheden in Workbench.

Dit pakket kan later worden doorgegeven aan een runtime, zoals local-C\#/CoreCLR, Scala/Spark of Scala/HDI.

1.  Selecteer het mappictogram om de bestandsweergave te openen en selecteer vervolgens **iris.csv** om dit bestand te openen.

    Dit bestand bevat een tabel met vijf kolommen en 50 rijen. Vier kolommen zijn kolommen voor numerieke functies. De vijfde kolom is een tekenreeks-doelkolom. Geen van de kolommen heeft een kopnaam.

    ![iris.csv](media\azure-stack-solution-machine-learning\image23.png)

1.  Klik in de **gegevensweergave** op het plusteken (**+**) om een nieuwe gegevensbron toe te voegen. De pagina **Add Data Source** (Gegevensbron toevoegen) wordt geopend.

    ![Gegevensweergave in Azure Machine Learning Workbench](media\azure-stack-solution-machine-learning\image24.png)

1.  Selecteer **tekstbestanden (\*CSV, \*.json, \*.txt.,...)** .

    ![Gegevensbron in Azure Machine Learning Workbench](media\azure-stack-solution-machine-learning\image25.png)

1.  Selecteer **Volgende**.

2.  Blader naar het bestand **iris.csv**, en selecteer **voltooien**. Hiermee worden standaardwaarden gebruikt voor parameters zoals het scheidingsteken en gegevenstypen.

    > [!Important]  
    > Selecteer de **iris.csv** -bestand uit vanuit de huidige projectmap voor deze oefening. Anders kunnen latere stappen mislukken.

    ![Iris selecteren](media\azure-stack-solution-machine-learning\image26.png)

1.  Een nieuw bestand met de naam `*iris-1.dsource` wordt gemaakt. Het bestand is een unieke naam met `-1` omdat het voorbeeldproject al wordt geleverd met een niet-genummerde **iris.dsource** bestand.

    Het bestand wordt geopend en de gegevens worden weergegeven. Een reeks kolomkoppen, van **Kolom1** naar **Column5**, wordt automatisch toegevoegd aan deze gegevensset. Ga naar de onderkant en u ziet dat de laatste rij van de gegevensset leeg is. De rij is leeg vanwege de extra regeleinde in het CSV-bestand.

    ![Gegevensweergave iris](media\azure-stack-solution-machine-learning\image27.png)

1.  Selecteer de knop **Metrics** (Metrische gegevens). Er worden histogrammen gegenereerd en weergegeven.

    Ga terug naar de gegevensweergave door de **gegevens** knop.

    ![Gegevensweergave iris](media\azure-stack-solution-machine-learning\image28.png)

1.  Bekijk de histogrammen. Er is voor elke kolom een volledige set statistieken berekend.

    ![Gegevensweergave iris](media\azure-stack-solution-machine-learning\image29.png)

1.  Maak eerst een gegevensvoorbereidingspakket door de knop **Prepare** te selecteren. Het **gelijknamige** dialoogvenster wordt geopend.

    Het voorbeeldproject bevat een **iris.dprep** gegevensvoorbereidingsbestand die standaard is geselecteerd.

    ![Gegevensweergave iris](media\azure-stack-solution-machine-learning\image30.png)

1.  Maak een nieuw pakket voor gegevensvoorbereiding door te selecteren **+ New Data Preparation Package** in het menu.

    ![Gegevensweergave iris](media\azure-stack-solution-machine-learning\image31.png)

1.  Voer een nieuwe waarde in voor de naam van het pakket (gebruik daarvoor **iris-1**) en selecteer **OK**.

    Een nieuw gegevensvoorbereidingspakket met de naam **iris-1.dprep** is gemaakt en geopend in de editor voor gegevensvoorbereiding.

    ![Gegevensweergave iris](media\azure-stack-solution-machine-learning\image32.png)

    Vervolgens is voorbereiden van gegevens vereist.

1.  Selecteer elke kolomkop om de koptekst te bewerken. Wijzig de naam van elke kolom als volgt:

    In deze volgorde **Petal Length**, **Petal Width**, **Petal Length**, **Petal Width**, en **soorten** voor de vijf kolommen respectievelijk.

    ![Kolomnamen wijzigen](media\azure-stack-solution-machine-learning\image33.png)

1.  Unieke waarden tellen:

    1.  Selecteer de kolom **Species**

    2.  Klik met de rechtermuisknop om deze te selecteren.

    3.  Selecteer **waarde telt** in het menu.

        Het deelvenster **Inspectors** wordt geopend onder de gegevens. Een histogram met vier balken wordt weergegeven. De doelkolom vier afzonderlijke waarden heeft: **iris_virginica**, **iris_versicolor**,**Iris-setosa**, en een **(null)** waarde.

    ![Selecteer Value Counts](media\azure-stack-solution-machine-learning\image34.png)

    ![Histogram met aantal waarden](media\azure-stack-solution-machine-learning\image35.png)

1.  Als u de null-waarden wilt filteren, selecteert u de balk "(null)" en selecteert u het minteken (**-**).

    De rij (null) wordt vervolgens grijs weergegeven om aan te geven dat deze is gefilterd.

    ![Null-waarden filteren](media\azure-stack-solution-machine-learning\image36.png)

1.  Bekijk de afzonderlijke gegevensvoorbereidingsstappen die worden beschreven in het deelvenster **STEPS**. Kolommen worden gewijzigd en rijen met null-waarden worden gefilterd, wordt elke actie vastgelegd als een stap van de voorbereiding van gegevens. Bewerk de afzonderlijke stappen voor het aanpassen van de instellingen, volgorde van de stappen veranderen en zelfs stappen verwijderen.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image37.png)

1.  Sluit de editor voor gegevensvoorbereiding. Selecteer het pictogram **x** op het tabblad **iris-1** bij het grafiekpictogram om het tabblad te sluiten. Het werk wordt automatisch opgeslagen in de **iris-1.dprep** bestand die wordt weergegeven onder de **Data Preparations** kop.

    ![Sluiten](media\azure-stack-solution-machine-learning\image38.png)

### <a name="generate-python-code-to-invoke-a-data-preparation-package"></a>Python-code voor het aanroepen van een gegevensvoorbereidingspakket genereren

De uitvoer van een gegevensvoorbereidingspakket kan rechtstreeks in Python of in een Jupyter Notebook worden gecontroleerd. De pakketten kunnen worden uitgevoerd op meerdere runtimes waaronder lokale Python, Spark (inclusief in Docker) en HDInsight.

1.  Zoek het bestand **iris 1.dprep** op het tabblad Data Preparations.

2.  Klik met de rechtermuisknop op het bestand **iris 1.dprep** en selecteer **Generate Data Access Code File** in het contextmenu.

    ![Code genereren](media\azure-stack-solution-machine-learning\image39.png)

    Een nieuw bestand met de naam **iris 1. PY** wordt geopend met de volgende regels met code voor het aanroepen van de logica die is gemaakt als een gegevensvoorbereidingspakket:

    ```Python
    # Use the Azure Machine Learning data preparation package
    from azureml.dataprep import package

    # Use the Azure Machine Learning data collector to log various metrics
    from azureml.logging import get_azureml_logger
    logger = get_azureml_logger()

    # This call will load the referenced package and return a DataFrame.
    # If run in a PySpark environment, this call returns a
    # Spark DataFrame. If not, it will return a Pandas DataFrame.
    df = package.run('iris-1.dprep', dataflow_idx=0)
    # Remove this line and add code that uses the DataFrame
    df.head(10)
    ```

    Afhankelijk van de context waarin deze code wordt uitgevoerd, vertegenwoordigt drep een ander soort gegevensframe:

    -  Bij het uitvoeren in een Python-runtime wordt een [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) gebruikt.

    -  Bij het uitvoeren in een Spark-context wordt een [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) gebruikt.

### <a name="review-irissklearnpy-and-the-configuration-files"></a>Iris_sklearn.py en de configuratiebestanden controleren

1.  Selecteer in het project hebt geopend, de **bestanden** knop (mappictogram) in het deelvenster uiterst links de lijst met bestanden in de projectmap te openen.

    ![Het project in de Azure Machine Learning Workbench openen](media\azure-stack-solution-machine-learning\image40.png)

1.  Selecteer het Python-scriptbestand **iris_sklearn.py**.

    ![Kies een script](media\azure-stack-solution-machine-learning\image41.png)

    De code wordt weergegeven op een nieuw tabblad van de teksteditor in Workbench.

    > [!Note]  
    > De code weergegeven mogelijk niet precies hetzelfde als de bovenstaande code omdat dit voorbeeldproject regelmatig wordt bijgewerkt.

    ![Een bestand openen](media\azure-stack-solution-machine-learning\image42.png)

1.  Bekijk de code van het Python-script om vertrouwd te raken met de stijl van coderen.

    Met het script **iris_sklearn.py** worden de volgende taken uitgevoerd:  

    -  Het standaard gegevensvoorbereidingspakket **iris.dprep** wordt geladen om een [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) te maken.

    -   Er worden willekeurige functies toegevoegd om het oplossen van het probleem te bemoeilijken. Randomness is nodig omdat Iris een kleine gegevensset is, die eenvoudig met bijna 100% nauwkeurigheid wordt geclassificeerd.

    -  Gebruikt thescikit learnmachine learning-bibliotheek voor het maken van een model voor logistieke regressie. Deze bibliotheek wordt standaard geleverd met Azure Machine Learning Workbench.

    -  Serialiseert het model door de [pickle](https://docs.python.org/3/library/pickle.html)-bibliotheek in te voegen in een bestand in de map `outputs`.

    -  Het geserialiseerde model wordt geladen en daarna weer in het geheugen gedeserialiseerd.

    -  Het gedeserialiseerde model wordt gebruikt om een voorspelling te geven voor een nieuwe record.

    -  Tekent twee grafieken, een verwarringsmatrix en een ROC-curve (RECEIVER) kenmerk besturingssysteem, met behulp van de [matplotlib](https://matplotlib.org/) bibliotheek, en deze vervolgens opslaan in theoutputsfolder. Deze bibliotheek installeren in de omgeving als dit niet zichtbaar.

    -  De nauwkeurigheid regularisatie en het model in de uitvoeringsgeschiedenis automatisch uitgezet. Het object `run_logger` wordt gebruikt om de regularisatiefrequentie en de nauwkeurigheid van het model in de logboeken vast te leggen.

### <a name="run-irissklearnpy-in-the-local-environment"></a>Iris_sklearn.py uitvoeren in de lokale omgeving

1.  Start de Azure Machine Learning-opdrachtregelinterface (CLI):

    1.  Start Azure Machine Learning Workbench.

    2.  Selecteer in het menu Workbench **File**> **Open Command Prompt**.

    Het venster Azure Machine Learning-opdrachtregelinterface (CLI) wordt gestart in de projectmap `C:\Temp\\myIris\` op Windows. Dit project is hetzelfde als de hebt gemaakt in deel 1 van de zelfstudie.

    > [!Important]  
    > Dit CLI-venster om uit te voeren van de volgende stappen gebruiken.

1.  In het CLI-venster installeert de Python-clientbibliotheek voor plotting **matplotlib**, is er nog niet is geïnstalleerd.

    Het script **iris_sklearn.py** is afhankelijk van twee Python-pakketten: **scikit-learn** en **matplotlib**. De **scikit-meer** pakket is geïnstalleerd in Azure Machine Learning Workbench voor het gemak. Nog steeds, installatie van **matplotlib** is mogelijk vereist.

    Als u doorgaat zonder te installeren **matplotlib**, de code in deze zelfstudie nog steeds met succes kunt uitvoeren. Het is voor de code echter niet mogelijk om de uitvoer van de verwarringsmatrix en de multiklasse ROC-curveplots die worden weergegeven in de geschiedenisvisualisaties te produceren.

    ```CLI
    pip install matplotlib
    python -m pip install --upgrade pip
    ```

    Deze installatie duurt ongeveer een minuut.

1.  Ga terug naar de Workbench-toepassing.

2.  Zoek het tabblad met de naam **iris_sklearn.py**.

    ![Zoek het tabblad met script](media\azure-stack-solution-machine-learning\image43.png)

1.  Selecteer in de werkbalk bovenaan het tabblad **lokale** als de uitvoeringsomgeving andiris_sklearn.pyas het script uit te voeren. Dit is mogelijk al ingeschakeld.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image44.png)

1.  Verplaatsen naar de rechterkant van de werkbalk en enter0.01in de **argumenten** veld.

    Deze waarde komt overeen met de regularisatiesnelheid van het logistieke regressiemodel.

    ![Lokale en scriptkeuze](media\azure-stack-solution-machine-learning\image45.png)

1.  Selecteer de knop **Run**. Er wordt direct een taak gepland. De taak wordt vermeld in het deelvenster **Jobs** aan de rechterkant van het Workbench-venster.

    ![Lokale en scriptkeuze](media\azure-stack-solution-machine-learning\image46.png)

    Na enkele ogenblikken verandert de status van de taak van **Submitting**naar **met**, en tot slot naar **voltooid**.

1.  Selecteer **Completed** in de taakstatustekst in het deelvenster **Jobs**.

    ![Sklearn van run](media\azure-stack-solution-machine-learning\image47.png)

    Een pop-upvenster wordt geopend en wordt de tekst standaarduitvoer (stdout) voor de uitvoerbewerking weergegeven. Als u wilt de stdout-tekst sluiten, selecteert u de **sluiten** (**x**) knop in de rechterbovenhoek van het pop-upvenster.

    ![Standaarduitvoer](media\azure-stack-solution-machine-learning\image48.png)

1.  In dezelfde taakstatus in het **taken** deelvenster, selecteert u de blauwe tekst **iris_sklearn.py \[n\] **(* n * is het uitvoeringsnummer) net boven de  **Voltooid** status en de begintijd. Het venster **Run Properties** wordt geopend en de volgende informatie wordt weergegeven voor de desbetreffende run:

    -  Informatie in **Run Properties**

    -  **Uitvoer**

    -  **Metrische gegevens**

    -  **Visualisaties**, indien aanwezig

    -  **Logboeken**

    Als de run is voltooid, bevat het pop-upvenster de volgende resultaten:

    > [!Note]  
    > Omdat de zelfstudie in de eerder randomisering hebben geïntroduceerd, wordt de exacte resultaten verschillen met de resultaten die hier worden weergegeven.

    ```Python  
        Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]

        Iris dataset shape: (150, 5)
        Regularization rate is 0.01
        LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
            intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
            penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
            verbose=0, warm_start=False)
        Accuracy is 0.6792452830188679

        ==========================================
        Serialize and deserialize using the outputs folder.

        Export the model to model.pkl
        Import the model from model.pkl
        New sample: [[3.0, 3.6, 1.3, 0.25]]
        Predicted class is ['Iris-setosa']
        Plotting confusion matrix...
        Confusion matrix in text:
        [[50  0  0]
        [ 1 37 12]
        [ 0  4 46]]
        Confusion matrix plotted.
        Plotting ROC curve....

        ROC curve plotted.
        Confusion matrix and ROC curve plotted. See them in Run History details pane.

    ```

1.  Sluit het tabblad **Run Properties** en ga vervolgens terug naar het tabblad **iris_sklearn.py**.

1.  Herhaal dit voor aanvullende runs.

Geef een reeks waarden op in het veld **Arguments**, variërend van `0.001` tot `10`. Selecteer **Run** om de code nog een paar keer uit te voeren. De argumentwaarde die telkens wanneer gewijzigd is ingevoerd in het model voor logistieke regressie in de code, waardoor de resultaten telkens.

### <a name="review-the-run-history-in-detail"></a>De uitvoeringsgeschiedenis in detail bekijken

In Azure Machine Learning Workbench wordt elke uitvoering van het script vastgelegd als een record van de uitvoeringsgeschiedenis. De uitvoeringsgeschiedenis van een bepaald script weergeven door het openen van de **uitvoeringen** weergeven.

1.  Als u de lijst met **Runs** wilt openen, selecteert u in de linkerwerkbalk de knop **Runs** (klokpictogram). Selecteer vervolgens **iris_sklearn.py** om weer te geven de **Run Dashboard** ofiris_sklearn.py.

    ![Weergave van run](media\azure-stack-solution-machine-learning\image49.png)

1.  Het tabblad **Run Dashboard** wordt geopend.

    Bekijk de statistieken die zijn vastgelegd voor de verschillende runs. Grafieken worden weergegeven in de rechterbovenhoek van het tabblad. Elke run heeft een volgnummer en de details van de uitvoering worden vermeld in de tabel aan de onderkant van het scherm.

    ![Dashboard van run](media\azure-stack-solution-machine-learning\image50.png)

1.  Filter de tabel en selecteer vervolgens een of meer grafieken om de status, duur, nauwkeurigheid en regularisatiefrequentie van elke run te bekijken.

2.  Schakel de selectievakjes in naast twee of meer uitvoeringen in de tabel **Runs**. Selecteer de knop **Compare** om een gedetailleerd vergelijkingsdeelvenster te openen. U kunt de gegevens van de verschillende runs nu naast elkaar bekijken.

3.  Selecteer in de linkerbovenhoek van het **vergelijkingsdeelvenster** de knop **Run List** om terug te keren naar het **Run Dashboard**.

    ![Ga terug naar Run list](media\azure-stack-solution-machine-learning\image51.png)

1.  Selecteer een afzonderlijke run om de detailweergave van de run te zien. Opmerking: de statistieken voor de geselecteerde run worden vermeld in de sectie **Run Properties**. De bestanden die worden weggeschreven naar de map voor uitvoer worden weergegeven in de **uitvoer** uit en download de bestanden van daaruit.

    ![Details van run](media\azure-stack-solution-machine-learning\image52.png)

De twee grafieken, de verwarringsmatrix en de ROC-curve met meerdere klassen worden weergegeven in de sectie **Visualizations**. Alle logboekbestanden zijn beschikbaar in de sectie **Logs**.

### <a name="run-scripts-in-the-azure-machine-learning-ml-workbench-cli-window"></a>Scripts uitvoeren in de Azure Machine Learning (ML) Workbench CLI-venster

1.  Start de Azure Machine Learning-opdrachtregelinterface (CLI):

    1.  Start Azure Machine Learning Workbench.

    2.  Selecteer in het menu Workbench **File** > **Open Command Prompt**.

    De CLI-prompt start in de projectmap `C:\\Temp\\myIris` op Windows. Dit is het project gemaakt in deel 1 van de zelfstudie.

    > [!Important]  
    > Dit CLI-venster om uit te voeren van de volgende stappen gebruiken.

1.  In het CLI-venster moet u zich aanmelden bij Azure. [Meer informatie over az-inloggen](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    Deze stap overslaan als u al aangemeld.

1.  Voer in de opdrachtregel in:

    ```CLI
        az login
    ```

    Met deze opdracht retourneert een code te gebruiken in de browser op [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin).

1.  Ga naar [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin) in de browser. Voer de code die u hebt ontvangen van de ontvangen in de CLI.

    De Workbench-app en de CLI gebruiken onafhankelijk referentiecaches voor verificatie bij Azure-resources. Verificatie is niet vereist opnieuw totdat het token in de cache verloopt.

1.  Als de organisatie heeft meerdere Azure-abonnementen (bedrijfsomgeving), stelt u het abonnement moet worden gebruikt. Het abonnement vinden, instellen met behulp van de abonnements-ID en test het vervolgens.

1.  Een overzicht van elk gebruikte Azure-abonnement met de volgende opdracht:

    ```CLI
        az account list -o table 
    ```

    De **az account list** opdracht retourneert de lijst met abonnementen aan de aanmelding. Als er meer dan één, Identificeer de abonnements-ID op voor het abonnement dat is gebruikt.

1.  De Azure-abonnement gebruikt als het standaardaccount instellen:

    ```CLI
        az account set -s <the-subscription-id
    ```

    Waarbij < de-abonnement-id > id-waarde voor het abonnement dat is gebruikt. Gebruik de haakjes niet.

1.  Bevestig de nieuwe abonnementsinstelling door de details voor het huidige abonnement aan te vragen.

    ```CLI
        az account show
    ```

1.  Verzend in het CLI-venster het script **iris_sklearn.py** als experiment.

    De uitvoering van iris_sklearn.py wordt uitgevoerd op de lokale compute-context.

1.  In Windows:

    ```CLI
        az ml experiment submit -c local .\\iris_sklearn.py
    ```

1.  In Mac OS:

    ```CLI
        az ml experiment submit -c local iris_sklearn.py
    ```

    De uitvoer is vergelijkbaar met: 

    ```
        RunId: myIris_1521077190506

    Executing user inputs .....
    ===========================

    Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  2 2016, 17:52:12) 
    [GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)]

    Iris dataset shape: (150, 5)
    Regularization rate is 0.01
    LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
            intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
            penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
            verbose=0, warm_start=False)
    Accuracy is 0.6792452830188679

    ==========================================
    Serialize and deserialize using the outputs folder.

    Export the model to model.pkl
    Import the model from model.pkl
    New sample: [[3.0, 3.6, 1.3, 0.25]]
    Predicted class is ['Iris-setosa']
    Plotting confusion matrix...
    Confusion matrix in text:
    [[50  0  0]
    [ 1 37 12]
    [ 0  4 46]]
    Confusion matrix plotted.
    Plotting ROC curve....
    ROC curve plotted.
    Confusion matrix and ROC curve plotted. See them in Run History details page.

    Execution Details
    =================
    RunId: myIris_1521077190506

    ```

6.  Controleer de uitvoer. Alle moet dezelfde uitvoer en resultaten als bij Workbench het script wordt uitgevoerd.

7.  Ga terug naar de Workbench en:

    Selecteer het pictogram van de map in het linkerdeelvenster voor een lijst met de projectbestanden.  Open het Python-script met de naam **run.py**. Met dit script is handig om verschillende regularisatiefrequenties. 

    ![Ga terug naar Run list](media\azure-stack-solution-machine-learning\image53.png)

1.  Voer het experiment meerdere keren met uit deze frequenties.

    Dit script wordt` aniris_sklearn.pyjob` met een snelheid regularisatie o `10.0` (een heel groot aantal). Het tarief voor het script vervolgens delen helft in de volgende run uit, enzovoort, totdat het tarief niet kleiner zijn dan is `0.005`. Het script bevat de volgende code:

    ![Ga terug naar Run list](media\azure-stack-solution-machine-learning\image54.png)

1.  U kunt het script **run.py** vanaf de opdrachtregel als volgt uitvoeren:

    ```CLI
        python run.py
    ```
Deze opdracht verzendt `iris_sklearn.py` meerdere keren met verschillende regularisatiefrequenties

Wanneer `run.py` is voltooid, grafieken van verschillende metrische gegevens worden weergegeven in de lijstweergave van de uitvoeringsgeschiedenis in workbench.

### <a name="run-scripts-in-an-ubuntu-based-data-science-virtual-machine-dsvm-on-azure"></a>Scripts uitvoeren in een op basis van een Ubuntu Data Science Virtual Machine (DSVM) op Azure

Voor het uitvoeren van het script in een Docker-container op een externe Linux-computer, wordt SSH-toegang (gebruikersnaam en wachtwoord) nodig om uit te voeren die externe computer. Bovendien moet op de computer een Docker-engine zijn geïnstalleerd en worden uitgevoerd.

1.  Bewerk het gegenereerde<your DSVM Name>.runconfigfile underaml_configand Wijzig het framework van de standaard-valuePySparktoPython:

    ```yaml  
    Framework: Python
    ```
1.  Dezelfde opdracht als voorheen in het CLI-venster met behulp van doel*<DSVM>* ditmaal iris_sklearn.py uitvoeren in een externe Docker-container: (Vervang de <DSVM> met de Data Science VM-naam, zonder de vierkante haken).

    ```CLI
        az ml experiment submit -c <DSVM> iris_sklearn.py
    ```

De opdracht wordt uitgevoerd alsof in adocker-pythonenvironment, behalve de uitvoering vindt plaats op de externe Linux-VM. Het CLI-venster bevat dezelfde uitvoergegevens.

### <a name="download-the-model-pickle-file"></a>Modelbestand van pickle downloaden

In het vorige deel van de zelfstudie werd het script **iris_sklearn.py** lokaal uitgevoerd in Machine Learning Workbench. Deze actie had tot doel het logistieke regressiemodel te serialiseren met behulp van het populaire Python-objectserialisatiepakket [pickle](https://docs.python.org/3/library/pickle.html).

1.  Open de toepassing Machine Learning Workbench. Open vervolgens de **myIris** project in de vorige gedeelten van deze reeks zelfstudies hebt gemaakt.

2.  Nadat het project geopend hebt, selecteert u de **bestanden** knop (mappictogram) in het linkerdeelvenster de lijst met bestanden in de projectmap te openen.

3.  Selecteer het bestand **iris_sklearn.py**. De Python-code wordt weergegeven op een nieuw tabblad van de teksteditor in Workbench.

4.  Controleer het bestand **iris_sklearn.py** om te zien waar het pickle-bestand is gegenereerd. Selecteer Ctrl+F om het dialoogvenster **Zoeken** te openen en zoek vervolgens het woord **pickle** in de Python-code.

Dit codefragment laat zien hoe het pickle-uitvoerbestand is gegenereerd. Op de schijf heeft het pickle-uitvoerbestand de naam **model.pkl**.

    ```Python
        print("Export the model to model.pkl")
        f = open('./outputs/model.pkl', 'wb')
        pickle.dump(clf1, f)
        f.close()

    ```

1.  Zoek het modelbestand van pickle tussen de uitvoerbestanden van een eerdere run.

    Wanneer de **iris_sklearn.py** script wordt uitgevoerd, wordt het modelbestand geschreven naar de **levert** map met de naam van de **model.pkl**. Deze map is aanwezig in de uitvoeringsomgeving gekozen het script uit te voeren en niet in de lokale projectmap. 

    1. Zoek het bestand, selecteer de **uitvoeringen** knop (klokpictogram) in het linkerdeelvenster om de lijst te openen **All Runs**.  

    2. Het tabblad **All Runs** wordt geopend. Selecteer in de tabel, een van de recente wordt uitgevoerd wanneer het doel is **lokale** en de scriptnaam **iris_sklearn.py**.  

    3. Het deelvenster **Run Properties** wordt geopend. In de rechterbovenhoek van het deelvenster ziet u de **uitvoer** sectie. d\. Als u wilt het pickle-bestand downloaden, schakel het selectievakje in naast de **model.pkl** bestand en selecteer vervolgens **downloaden**. Sla het bestand naar de hoofdmap van de projectmap. Het bestand is nodig bij de volgende stappen.  

    ![Pickle-bestand downloaden](media\azure-stack-solution-machine-learning\image55.png)

### <a name="get-scoring-script-and-schema-files"></a>Scoring-script en de schemabestanden ophalen

Scoring-script is voor het implementeren van de webservice het modelbestand nodig. (Optioneel) is een schema voor invoergegevens van de webservice vereist. Via het scoring-script wordt het bestand **model.pkl** uit de huidige map geladen en gebruikt voor het maken van nieuwe voorspellingen.

1.  Open de toepassing Machine Learning Workbench. Open vervolgens de **myIris** project gemaakt in het vorige deel van de serie zelfstudies.

2.  Nadat het project geopend hebt, selecteert u de **bestanden** knop (mappictogram) in het linkerdeelvenster de lijst met bestanden in de projectmap te openen.

3.  Selecteer het bestand **score_iris.py**. Het Python-script wordt geopend. Dit bestand wordt gebruikt als het scoring-bestand.

    ![Scoring-bestand](media\azure-stack-solution-machine-learning\image56.png)

1.  Voer het script uit om het schemabestand op te halen. Selecteer de omgeving **local** en het script **score_iris.py** in de opdrachtbalk en selecteer vervolgens **Run**.

    Dit script maakt u een JSON-bestand in de **uitvoer** sectie, met daarin het schema voor invoergegevens vereist voor het model.

1.  Aan de rechterkant van het **projectdashboard** wordt nu het deelvenster **taken** weergegeven. Wacht tot de meest recente ** score_iris.py**-taak om weer te geven van de groene **voltooid** status. Klik vervolgens op de hyperlink **iris-score.py** voor de laatst uitgevoerde taak om de details van de uitvoering te zien.

2.  Ga in het deelvenster **Run Properties** naar de sectie **Outputs** en selecteer het zojuist gemaakte bestand **service_schema.json**. Selecteer het selectievakje naast de bestandsnaam en selecteer vervolgens **Download**. Sla het bestand in de hoofdmap van het project.

3.  Ga terug naar het vorige tabblad en de **iris-score.PY** script. Met behulp van het verzamelen van gegevens, de modelinvoer en voorspellingen op basis van de webservice kunnen worden vastgelegd. De volgende stappen zijn van belang voor gegevensverzameling.

4.  Bekijk de code aan de bovenkant van het bestand, dat invoer van de klasse **ModelDataCollector**. De functionaliteit voor modelgegevensverzameling bevat:

    ```Python  
        from azureml.datacollector import ModelDataCollector
    ```

1.  Bekijk de volgende regels met code in de functie **init()** waarmee een instantie van **ModelDataCollector** wordt gemaakt:

    ```Python  
        global inputs_dc, prediction_dc
        inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
        prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
    ```

1.  Bekijk de volgende regels met code in de functie **run(input_df)** omdat hiermee de invoer- en voorspellingsgegevens worden verzameld:

    ```Python  
        inputs_dc.collect(input_df)
        prediction_dc.collect(pred)
    ```

Bereidt de omgeving om het model operationeel te maken.

## <a name="step-5-deploy-and-use-azure-container-registry"></a>Stap 5: Implementeren en gebruiken van Azure Container Registry

Implementeren en gebruiken van Azure Container Registry.

Maak een Azure Container Registry met de opdracht **az acr create**. De registernaam moet uniek zijn binnen Azure en mag 5 tot 50 alfanumerieke tekens bevatten. De resourcegroep is hetzelfde.

    ```CLI
        az acr create --resource-group <ResourceGroup> --name  <acrName> --sku Basic
    ```

### <a name="container-registry-login"></a>Aanmelden bij het containerregister

Gebruik de opdracht **az acr login** om u aan te melden bij het ACR-exemplaar. Geef de unieke naam op die u het containerregister hebt gegeven toen u het maakte.

    ```CLI
        az acr login --name <acrName>
    ```

De opdracht retourneert een ' aanmelding geslaagd bericht nadat deze is voltooid.

### <a name="prepare-to-operationalize-locally-for-development-and-testing-the-service"></a>Voorbereiden op operationeel maken van lokaal voor ontwikkelen en testen van de service

Gebruik *lokale modus* implementatie worden uitgevoerd in Docker-containers op de lokale computer, en voor het ontwikkelen en testen.

De Docker-engine moet lokaal worden uitgevoerd om de stappen te kunnen voltooien die nodig zijn om het model operationeel te maken. Gebruik de `-h` vlag aan het einde van elke opdracht om de bijbehorende help-bericht weer te geven.

    > [!Note]  
    > If Docker engine is not locally available, proceed by creating a cluster in Azure for deployment and keep the cluster for re-use, or delete it after the tutorial to avoid ongoing charges.

    > [!Note]  
    > Web services deployed locally do not appear in Azure Portal's list of services. They will be running in Docker on the local machine.

1.  Open de CLI (opdrachtregelinterface). Selecteer in de toepassing Machine Learning Workbench in het menu **File** de optie **Open Command Prompt**.

    De opdrachtregelprompt wordt geopend in de huidige map projectlocatie **c:\\temp\\myIris**.

1.  Zorg ervoor dat de Azure resourceprovider **Microsoft.ContainerRegistry** is geregistreerd in het abonnement. Deze resourceprovider registreren voordat u een omgeving maakt in stap 3. Controleren of deze al geregistreerd met behulp van de volgende opdracht uit:

    ```CLI
        az provider list --query "\[\].{Provider:namespace, Status:registrationState}" --out table
    ```

    Deze uitvoer weergeven:

    ```CLI
        Provider                                    Status 
        --------                                    --------
        Microsoft.Authorization                     Registered 
        Microsoft.ContainerRegistry                 Registered 
        microsoft.insights                          Registered 
        Microsoft.MachineLearningExperimentation    Registered 
    ```

    Als **Microsoft.ContainerRegistry** is niet geregistreerd gebruik de volgende opdracht uit:

    ```CLI
    az provider register --namespace Microsoft.ContainerRegistry
    ```

    Registreren kan enkele minuten duren. Van registratiestatus controleren met de vorige **az provider list** opdracht of de volgende opdracht:

    ```CLI
    az provider show -n Microsoft.ContainerRegistry
    ```

    De derde regel van de uitvoer bestaat uit **'registrationState': 'Registering'**. Wacht even en Herhaal de **weergeven** opdracht totdat u de uitvoer **'registrationState': 'Registered.**

1.  Maak de omgeving. Deze stap eenmaal per omgeving uitvoeren.

    De volgende installatieopdracht moet Inzender-toegang tot het abonnement. Inzender-toegang tot de implementatie naar resourcegroep is vereist. Naam van de resourcegroep opgeven als onderdeel van de installatieopdracht via de gflag.

    ```CLI
    az ml env setup -n <new deployment environment name> --location <e.g. eastus2> -g <existing resource group name>
    ```

    Volg de aanwijzingen op het scherm voor het inrichten van een opslagaccount om Docker-installatiekopieën op te slaan, een Azure Container Registry om Docker-installatiekopieën te vermelden en een Azure Application Insights-account om telemetrie te verzamelen. **Als de cswitch, de opdracht bovendien een Container Service-cluster maakt**.

    Naam van het cluster is een manier om de omgeving te identificeren. De locatie moet hetzelfde zijn als de locatie van het modelbeheeraccount gemaakt op basis van de Azure-portal.

    Als u er zeker van wilt zijn dat de omgeving goed is ingesteld, voert u de volgende opdracht uit om de status te controleren:

    ```CLI
    az ml env show -n <deployment environment name> -g <existing resource group name>
    ```

    Zorg ervoor dat 'Provisioning State' heeft de waarde 'Succeeded', zoals wordt weergegeven, voordat het instellen van de omgeving in stap 5:

    ![Inrichtingsstatus](media\azure-stack-solution-machine-learning\image57.png)

1.  Stel de omgeving in.

    Als de installatie is voltooid, gebruikt u de volgende opdracht om de omgevingsvariabelen in te stellen die zijn vereist om de omgeving operationeel te maken. Gebruik dezelfde omgevingsnaam eerder hebt gebruikt in stap 3. Gebruik dezelfde naam voor de resourcegroep die als uitvoer wordt weergegeven in het opdrachtvenster wanneer het installatieproces is voltooid.

    ```CLI
        az ml env set -n <deployment environment name> -g <existing resource group name>
    ```

1.  Als u wilt controleren of de juiste configuratie van de omgeving voor implementatie van de lokale webservice, voer de volgende opdracht:

    ```CLI
    az ml env show
    ```

    Maak nu de realtime webservice.

    > [!Note]  
    > De Modelbeheer-account en de omgeving voor volgende webservice-implementaties opnieuw kunt gebruiken. Er is niet nodig om ze te maken voor elke webservice. Aan een account of een omgeving kunnen meerdere webservices zijn gekoppeld.

### <a name="create-a-real-time-web-service-by-using-separate-commands"></a>Een realtime webservice maken met behulp van afzonderlijke opdrachten

Als alternatief voor de **az ml service maken realtime** opdracht eerder weergegeven, kunnen deze stappen ook afzonderlijk worden uitgevoerd.

Registreer eerst het model. Genereer vervolgens het manifest, bouw de Docker-installatiekopie en maak de webservice. Deze stapsgewijze aanpak biedt meer flexibiliteit bij elke stap. Bovendien kunnen de entiteiten die in de vorige stappen zijn gegenereerd worden hergebruikt.

1. Registreer het model door de naam van het pickle-bestand op te geven.

    ```CLI
    az ml model register --model model.pkl --name model.pkl
    ```

    Er wordt nu een id voor het model gegenereerd.

2.  Een manifest maken.

    Gebruik de volgende opdracht en geeft de model-ID in de vorige stap voor het maken van een manifest: 
    
    ```CLI
    az ml manifest create --manifest-name <new manifest name> -f score_iris.py -r python -i <model ID> -s service_schema.json
    ```

    Er wordt nu een id voor het manifest gegenereerd.

3.  Een Docker-installatiekopie maken.

    Voor het maken van een Docker-installatiekopie, gebruikt u de volgende opdracht en geeft u het manifest van de waarde ID van de vorige stap. Conda-afhankelijkheden kunnen ook worden gebruikt de `-c` overschakelen. 
    
    ```CLI
    az ml image create -n irisimage --manifest-id <manifest ID> -c aml_config\conda_dependencies.yml
    ```
    
    Er wordt nu een id voor de Docker-installatiekopie gegenereerd.

2.  De service maken.

    Gebruik de volgende opdracht en geeft u de uitvoer van de installatiekopie-ID van de vorige stap voor het maken van een service: 
    
    ```CLI
    az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
    ```
    
    Er wordt nu een id voor de webservice gegenereerd.
    
    Voer vervolgens de web-service.

## <a name="step-6-deploy-a-kubernetes-cluster-to-azure-stack"></a>Stap 6: Een Kubernetes-cluster implementeren op Azure Stack

Een Kubernetes-cluster implementeren in Azure Stack.

Kubernetes kan worden geïnstalleerd met behulp van Azure Resource Manager-sjablonen die zijn gegenereerd door de Engine voor Azure Container Services (ACS) op Azure Stack. [*Kubernetes* ](https://kubernetes.io/) is een open-source systeem voor het automatiseren van implementatie, schalen en beheren van toepassingen in containers. Een [ *container* ](https://www.docker.com/what-container) is opgenomen in een afbeelding, vergelijkbaar met een virtuele machine. In tegenstelling tot een virtuele machine bevat de containerinstallatiekopie de benodigde resources beschikken om uit te voeren van een toepassing, zoals de code, runtime voor het uitvoeren van de code, specifieke bibliotheken en -instellingen.

Kubernetes voor gebruiken:

 -  Ontwikkel toepassingen met grote aantallen schaalbare en kan worden bijgewerkt, die kunnen worden geïmplementeerd in een paar seconden.

 -  Vereenvoudig het ontwerp van de toepassing en de betrouwbaarheid verbeteren door verschillende Helm-toepassingen. [*Helm* ](https://github.com/kubernetes/helm) is een open-source-pakketten hulpprogramma waarmee u kunt installeren en beheren van de levenscyclus van toepassingen met Kubernetes.

 -  Eenvoudig bewaken en de status van de toepassingen met schaal opsporen en functionaliteit van een upgrade uitvoert.

> [!Note]  
> Installatie van het cluster duurt ongeveer een uur. plan op de juiste wijze.

### <a name="prerequisites-for-kubernetes-cluster-deployment"></a>Vereisten voor implementatie van Kubernetes-cluster

Bevestig om te beginnen, machtigingen en gereedheid voor Azure Stack:

1.  Valideren dat de **Kubernetes-Cluster maken (Preview)** Item is beschikbaar in de Azure Stack Marketplace. Als dit item ontbreekt, wordt deze samen met een Azure Stack-operators voor het toevoegen van dit item in de Azure Stack-omgeving.

2.  Controleer of de mogelijkheid om toepassingen te maken in de tenant Azure Active Directory (Azure AD). Machtigingen zijn vereist voor Kubernetes-implementatie.

    Zie voor instructies over het controleren van de machtigingen [ *machtigingen voor Azure Active Directory controleren*](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).

3.  Een SSH openbare en persoonlijke sleutelpaar genereren om aan te melden bij de Linux-VM in Azure Stack. De openbare sleutel is nodig bij het maken van het cluster. Zie voor instructies [een verificatiesleutel te genereren voor SSH](#generate-an-authenticatio-key-for-ssh).

4.  Controleer of het abonnement in de portal voor Azure Stack-tenant geldig is en er onvoldoende openbare IP zijn-adressen beschikbaar om toe te voegen nieuwe toepassingen.

    Het cluster kan niet worden geïmplementeerd naar een Azure Stack **beheerder** abonnement. Gebruik een **gebruiker** abonnement.

###  <a name="generate-an-authentication-key-for-ssh"></a>Een verificatiesleutel te genereren voor SSH

Gebruik de volgende opdrachten in het Windows-subsysteem voor Linux-sessie voor het genereren van een verificatiesleutel: 

1. Type:

    ```Bash  
    ssh-keygen -t rsa
    ```
    
2. Selecteer `id_rsa` wanneer hierom wordt gevraagd. 
3. Maak een wachtwoordzin wanneer hierom wordt gevraagd. Het is belangrijk te weten van dit wachtwoord op voor later gebruik. 
    De uitvoer ziet eruit als hieronder: 
    
    ```Bash  
    Your identification has been saved in `id_rsa`.
    Your public key has been saved in `id_rsa.pub`. 
    The key fingerprint is: SHA256:lUtUUjzaqWqGeolEPKeBmsnrhcNGM9Dn2OxYatt05SE  <user>@<machine-name>
    The key's randomart image is:  
    ```
    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image58.png)

4. Na het genereren van de sleutel, plak de gegevens van de sleutel met de volgende opdrachten: 
    ```Bash
    cat id_rsa.pub
    ```
    De uitvoer ziet eruit als hieronder: 
    ```Bash
    ssh-rsa  AAAAB3NzaC1yc2EAAAADAQABAAABAQDHaWrAktR3BlQ356T8Qvv8O2Q/U/hsXQwS9xJbuduuc9lkJwddzgmNCyM9PooZWYtGVXyHU6SC3YH1XkwqGtKhtPb03d24hmhX1euAaqIqHHSp4WgUS5s1gNsp37L8QCSGNCnF31FWavI8bnjOjccUkMowKl8iyGN++5UyQgNuynEVUbFJjrntoDL66HUu88xDxTcVB7rqDr2QKFwYJkg4HSoHYpezJd7W8kcmv33eh0xs8nlDA7Dzu7zXpyVc54bH9XtPR6EUXaQa+UqKaNlQNiJqEs+1X/zNuK9V6NLVNiO0h3jCHI3K8o4VnZyRKHCQProasiiPLUUJ6SF/RTLN  <user>@<machine-name>
    ```
    
5. Kopieer de gegenereerde sleutel naar het SSH-sleutelveld publiek.

### <a name="create-a-service-principal-in-azure-ad"></a>Een service-principal maken in Azure AD

1.  Meld u aan de globale [ *Azure-portal*](http://www.poartal.azure.com/).

2.  Meld u aan met behulp van de Azure AD-tenant die is gekoppeld aan de Azure Stack-exemplaar.

3.  Maak een Azure AD-toepassing.

    1. Selecteer **Azure Active Directory** > **+ App-registraties**> **nieuwe toepassing registreren**.
    2. Voer een **naam** van de toepassing. 
    3. Selecteer **Web-app / API**. 
    4. Voer `http://localhost` voor de **aanmeldings-URL**. 
    5. Selecteer **Maken**.

1.  Noteer de **toepassings-id**. Deze ID is vereist wanneer het cluster is gemaakt en wordt verwezen als **Client-ID Service-Principal**.

2.  Selecteer **instellingen** > **sleutels**.

    1. Voer de **beschrijving**. 
    2. Selecteer **verloopt nooit** voor **verloopt**. 
    3. Selecteer **Opslaan**. Houd er rekening mee de belangrijkste tekenreeks maken De belangrijkste tekenreeks is die nodig zijn bij het maken van het cluster waarnaar wordt verwezen als de **Clientgeheim Service-Principal**.

### <a name="give-the-service-principal-access"></a>De service-principal toegang verlenen

De service-principal toegang geven tot het abonnement, zodat deze resources kan maken.

1.  Aanmelden bij de [beheerportal](https://adminportal.local.azurestack.external/).

2.  Selecteer **meer services** > ** gebruikersabonnementen ** > **+ toevoegen**.

3.  Selecteer het abonnement dat is gemaakt.

4.  Selecteer **toegangsbeheer (IAM)** > Selecteer **+ toevoegen**.

5.  Selecteer de **eigenaar** rol.

6.  Selecteer de naam van de toepassing gemaakt voor de service principal. Typ de naam in het zoekvak, indien nodig.

7.  Selecteer **Opslaan**.

8.  Open de [Azure Stack-portal](https://portal.local.azurestack.external).

9.  Selecteer **+ nieuw** > **Compute** > **Kubernetes-Cluster**. Selecteer **Maken**.

    ![Oplossingssjabloon implementeren](media\azure-stack-solution-machine-learning\image59.png)

10\. Selecteer **basisbeginselen** in het Kubernetes-Cluster maken.

    ![Deploy Solution Template](media\azure-stack-solution-machine-learning\image60.png)

11. Voer de **Linux VM-Beheerdersgebruikersnaam**. De naam van de gebruiker voor de virtuele Linux-Machines die deel van het Kubernetes-cluster uitmaken en DVM.

12. Voer de **openbare SSH-sleutel** gebruikt voor verificatie op alle Linux-machines gemaakt als onderdeel van de Kubernetes-cluster en DVM.

13. Voer de **Master profiel DNS-voorvoegsel** die uniek is voor de regio. Dit moet een regio-unieke naam, zoals `ask8s-12345`. Probeer te kiezen gelijk zijn aan de resourcegroep een naam als best practice.

    > [!Note]  
    > Gebruik een master profiel van nieuwe en unieke DNS-voorvoegsel voor elk cluster.

14. Voer de **Agentcount Pool profiel**. Het aantal bevat het aantal agents in het cluster. Er mag uit 1 tot en met 4.

15. Voer de **Service-Principal ClientId** dit wordt gebruikt door de Kubernetes Azure-cloud-provider.

16. Voer de **Clientgeheim Service-Principal** gemaakt bij het maken van service-principal-toepassing.

17. Voer de **Kubernetes Azure Cloud Provider-versie**. Dit is de versie van de Kubernetes Azure-provider. Azure Stack brengt een aangepaste Kubernetes-build voor elke Azure Stack-versie.

18. Selecteer de **abonnement** -id.

19. Voer de naam van een nieuwe resourcegroep of Selecteer een bestaande resourcegroep. De resourcenaam moet alleen alfanumerieke tekens en kleine letters.

20. Selecteer de **locatie** van de resourcegroep. Dit is de regio die is gekozen voor de installatie van Azure Stack.

### <a name="specify-the-azure-stack-settings"></a>Geef de instellingen op Azure Stack

1.  Selecteer de **instellingen voor Azure Stack-stempel**.

    ![Oplossingssjabloon implementeren](media\azure-stack-solution-machine-learning\image61.png)

2.  Voer de **Tenant Azure Resource Manager-eindpunt**. Dit is de Azure Resource Manager-eindpunt verbinding maken met de resourcegroep voor het Kubernetes-cluster maken. Het eindpunt van de Azure Stack-operator is nodig voor een geïntegreerd systeem. Voor de Azure Stack Development Kit (ASDK), gebruik `https://management.local.azurestack.external`.

3.  Voer de **Tenant-ID** voor de tenant. Zie [ *tenant-ID ophalen* ](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) te vinden van de tenant id-waarde.

### <a name="install-kubectl-on-windows-powershell-environment"></a>Kubectl installeren op Windows PowerShell-omgeving

Voer de volgende opdrachten voor het installeren van kubectl in de omgeving WSL binnen de omgeving WSL uit.

```PowerShell  
Install-script -name install-kubectl -scope CurrentUser -force
Install-kubectl.ps1 -downloadlocation “C:\Users\<Current User>\Documents\Kube
```

### <a name="install-kubectl-on-the-windows-subsystem-for-linux-environment"></a>Kubectl installeren op het Windows-subsysteem voor Linux-omgeving

Voer de volgende opdrachten voor het installeren van kubectl in de omgeving WSL binnen de omgeving WSL uit.

```Bash  
    apt-get update && apt-get install -y apt-transport-https
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
    cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
    deb http://apt.kubernetes.io/ kubernetes-xenial main
    EOF
    apt-get update
    apt-get install -y kubectl
```

### <a name="configure-kubectl"></a>Kubectl configureren

Kubectl vinden en openen van een Kubernetes-cluster, zodat een*kubeconfig bestand* nodig is. Dit wordt automatisch gemaakt wanneer een cluster is gemaakt met behulp van kube-up.sh of u een cluster Minikube implementeert. Zie de [ *aan de slag handleidingen* ](https://kubernetes.io/docs/setup/) voor meer informatie over het maken van clusters. Zie voor toegang tot een cluster dat is gemaakt door een andere gebruiker, de [ *toegang tot het Cluster delen document*](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/). Standaard configuratie van kubectl bevindt zich in **~.kube/config**.

### <a name="check-the-kubectl-configuration"></a>Controleer de configuratie van kubectl

Controleer dat kubectl correct is geconfigureerd met het ophalen van de clusterstatus:

```Bash  
kubectl cluster-info
```

kubectl is juist geconfigureerd voor toegang tot het cluster wanneer het antwoord van de url wordt weergegeven.

kubectl is niet correct geconfigureerd of kan geen verbinding maken met een Kubernetes-cluster als het volgende bericht wordt weergegeven:

```Bash  
The connection to the server <server-name:port> was refused -  did you specify the right host or port?
```

Bijvoorbeeld, wanneer een Kubernetes-cluster wordt uitgevoerd op een lokale laptop, een hulpprogramma dat kan ook nodig zijn (minikube of vergelijkbaar) de bovenstaande opdrachten opnieuw uitvoeren.

Als u kubectl cluster-info wordt het url-antwoord geretourneerd, maar het cluster nog steeds niet toegankelijk is, Controleer voor de juiste configuratie met behulp van:

```Bash  
> kubectl cluster-info dump
```

### <a name="enable-shell-autocompletion"></a>Automatisch aanvullen shell inschakelen

kubectl bevat ondersteuning voor automatisch aanvullen, waardoor shell inschakelen voor snelle en eenvoudige.

Het script is voltooid, zelf wordt gegenereerd door kubectl en toegankelijk is vanaf het profiel.

### <a name="connect-to-the-kubernetes-cluster"></a>Verbinding maken met het Kubernetes-Cluster

Verbinding maken met het cluster, de Kubernetes-opdrachtregelclient (**kubectl**) nodig is. Vindt u instructies voor het verbinden met en beheren van het cluster in [documentatie voor Azure Container Services.](https://docs.microsoft.com/azure/container-service/dcos-swarm/)

Een SSH-Client kan worden gebruikt om verbinding te maken van het Kubernetes-cluster. Windows-subsysteem voor Linux (WSL) wordt aanbevolen. De computer die verbinding maakt met het Kubernetes-Cluster worden weergegeven in de resourcegroep gemaakt voor het cluster en beginnen met het voorvoegsel van vmd-edge-ml-stack.

```Bash  
ssh <user>@vmd-edge-ml-stack.<FQDN of Kubernetes Machine in  Azure Stack>
```

Eenmaal verbinding hebben met de Kubernetes-Machine, voer de volgende machine voor het ophalen van de configuratie van het Kubernetes-bestand.

```Bash  
sudo find / -name \*kubeconfig\* -type f
```

De uitvoer wordt ziet er ongeveer als volgt uit:

```Bash  
/var/lib/waagent/custom-script/download/0/acs-engine/_output/edgemlstack/kubeconfig/kubeconfig.<regionname>.json
```

Deze bestanden pad gegevens kopiëren en voer vervolgens de volgende opdracht te plakken in het pad van het kubeconfig gekopieerd hierboven:

```Bash  
sudo cat  /var/lib/waagent/custom-script/download/0/acs-engine/_output/edgemlstack/kubeconfig/kubeconfig.<regionname>.json
```

De uitvoer is een grote blok van tekst, de onbewerkte JSON-inhoud. Kopieer deze uitvoertekst en plak deze code in een Visual Studio-bestand opslaan als een JSON-bestand. Dit resulteert in een lokaal opgeslagen kubeconfig.json-bestand. (opslaannaar/mnt/c/gebruikers/ <current user> /documenten/Kube-map als kubeconfig.json)

### <a name="configure-the-kubernetes-cluster"></a>Het Kubernetes-Cluster configureren

Als u het lokale JSON-bestand is verkregen, in een nieuwe sessie WSL, gebruik de volgende opdrachten om het Cluster te configureren.

```Bash  
    cd /mnt/c/users/<current user>/documents/Kube
    kubectl proxy
    kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
    kubectl proxy
    set KUBECONFIG=”/mnt/c/users/<current user>/documents/Kube/kubeconfig.json”
    kubectl.exe config view
```

Kubernetes-configuratie-instellingen worden gedefinieerd (Zie de onderstaande uitvoer).

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image62.png)

Start de lokale proxyservice:

```Bash  
kubectl proxy
```

Blader naar de kubernetes-cluster UI op het volgende adres: `https://localhost:8001`.

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image63.png)

U hebt nu een plaats voor het implementeren van de container en een container die zich in de cloud die u on-premises kunt zien.

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image64.png)

Aanpassen de **iris_deployment.yaml** bestand (zich in /*mnt/c/gebruikers/<current user>/documenten/Kube directory*), zodat **webservicename** en containers  **Afbeelding** en **naam** overeenkomen met de implementatie, met een code-editor naar keuze.

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image65.png)

Stel de poort van de container in op **5001.**

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image66.png)

En maak vervolgens de **imagePullSecret**:

### <a name="create-a-secret-in-the-cluster-that-holds-the-authorization-token"></a>Een geheim maken in het cluster waarin het Autorisatietoken

Een Kubernetes-cluster maakt gebruik van het geheim van **docker-register** type om te verifiëren met een container registry voor het ophalen van een persoonlijke installatiekopie.

Maken van dit geheim, naam **azuremlcr**:

```Bash  
kubectl create secret docker-registry azuremlcr --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password= "<your-pword>" --docker-email=<your-email>
```

Zoek als volgt:

- **<-Register-server >** is de Azure Container Registry **aanmeldingsserver**.
- **< uw-naam >** is de Azure Container Registry **gebruikersnaam**.
- **< uw-pword >** is de Azure Container Registry **wachtwoord**. Zorg ervoor dat het wachtwoord is tussen aanhalingstekens.
- **< uw-e-mailadres >** is de gebruiker dat R/W toegang tot de container heeft.
- Deze informatie vinden op de **Azure Container** **register** onder **toegangssleutels**.
- Docker-referenties zijn nu ingesteld in het cluster, zoals een geheim genoemd **azuremlcr**.

Sla de **iris_deployment.yaml** bestand (zich in /*mnt/c/gebruikers/<current user>/documenten/Kube directory*).

### <a name="create-the-kubernetes-container"></a>De Kubernetes-Container maken

```Bash  
kubectl.exe create -f /mnt/c/users/<current  user>/documents/Kube/iris_deployment.yaml
```

    ![Alt text](media\azure-stack-solution-machine-learning\image67.png)

Controleer de status van de implementatie:

```Bash  
Kubectl get deployments
```

    ![Alt text](media\azure-stack-solution-machine-learning\image68.png)

De implementatie kan enige tijd duren.

### <a name="configure-visual-studio-team-services-to-deploy-automatically"></a>Visual Studio Team Services automatische implementatie configureren

#### <a name="create-a-team-project"></a>Maak een teamproject

1.  Zorg ervoor dat [Project verzameling Administrators groepslidmaatschap.](https://docs.microsoft.com/vsts/organizations/security/set-project-collection-level-permissions?view=vsts) Teamprojecten, maken **maken van nieuwe projecten** machtiging moet worden ingesteld op **toestaan**.

2.  Selecteer in de pagina projecten **nieuw Project**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image69.png)

1.  Noem het project **HybridMLIris**.

2.  Selecteer het eerste besturingselement van het brontype als **Git**

3.  Selecteer een proces en selecteer **maken**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image70.png)

### <a name="import-some-code--create-repository"></a>Importeren van code opslagplaats maken

Een Git-opslagplaats voor YAML-code is vereist.

#### <a name="add-user-to-the-git-repo"></a>Gebruiker toevoegen aan de GIT-opslagplaats

1.  Selecteer in het projectdashboard standaard genereren Git-referenties.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image71.png)

1.  Voer wachtwoord indien nodig en sla de Git-referenties.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image72.png)

1.  Initialiseren van de opslagplaats door het selecteren van de **initialiseren** knop en het maken van een **Leesmij-bestand** bestand.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image73.png)

#### <a name="clone-the-git-repository-locally-and-upload-the-code"></a>De Git-opslagplaats lokaal klonen en de code te uploaden. 

1.  Een map maken in machine op `c:\\users\\<User>\\source\\repos\\hybridMLIris`, en kloon de opslagplaats

    ```Bash  
    sudo mkdir /mnt/c/users/<User>/source sudo mkdir /mnt/c/users/<User>/source/repos sudo mkdir /mnt/c/users/<User>/source/repos/hybridMLIris cd /mnt/c/users/<User>/source/repos/hybridMLIris sudo git clone  https://<yourvstssite>.visualstudio.com/HybridMLIris/_git/HybridMLIris
    ```

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image74.png)

1.  Navigeer naar de nieuwe gekloonde opslagplaats:

    ```Bash  
    ls
    cd ./HybridMLIris
    ```
    
    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image75.png)

1.  Kopieer de **iris_deployment.yaml** -bestand in de opslagplaats.

    ```Bash  
    cp /mnt/c/users/<User>/documents/Kube/iris_deployment.yaml  /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/iris_deployment.yaml
    ``` 

1.  Voer de wijziging in GIT

    ```Bash  
    git add . git commit -m Added Deployment YAML git push
    ```

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image76.png)

### <a name="prepare-the-private-build-and-release-agent-for-vsts-integration"></a>Voorbereiden van de Privéversie en Release-Agent voor integratie met VSTS

#### <a name="prerequisites"></a>Vereisten

VSTS wordt geverifieerd op basis van Azure Resource Manager met behulp van een Service-Principal. Voor VSTS om het inrichten van resources in een Azure Stack-abonnement te kunnen hiervoor Inzender status. \ **volgen de stappen op hoog niveau die moeten worden geconfigureerd als dergelijke verificatie wilt inschakelen:**

1.  Service-Principal moet worden gemaakt of een bestaande kan worden gebruikt.

2.  Verificatiesleutels moeten worden gemaakt voor de Service-Principal.

3.  Azure Stack-abonnement moet worden geverifieerd via toegangsbeheer op basis van rollen om toe te staan de SPN deel uitmaken van de rol van bijdragers.

4.  De definitie van een nieuwe Service in VSTS moet worden gemaakt met de Azure Stack-eindpunten, evenals de SPN-informatie.

#### <a name="service-principal-creation"></a>Service-Principal maken

Raadpleeg de [instructies voor het maken van Service-Principal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) te maken van een service-principal en de Web-App/API optie voor het Type toepassing.

**Sleutel voor toegang tot de maken**

Een Service-Principal is een sleutel vereist voor verificatie, volgt u de stappen in deze sectie om een sleutel te genereren.

1.  Van **App-registraties** in Azure Active Directory, selecteert u de toepassing.

    ![toepassing selecteren](media\azure-stack-solution-machine-learning\image77.png)

1.  Noteer de waarde van **toepassings-ID. De waarde wordt gebruikt bij het configureren van het service-eindpunt in VSTS.**

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image78.png)

1.  Selecteer **Instellingen** om een verificatiesleutel te genereren.

    ![instellingen selecteren](media\azure-stack-solution-machine-learning\image79.png)

1.  Selecteer **Sleutels**.

    ![sleutels selecteren](media\azure-stack-solution-machine-learning\image80.png)

1.  Geef een beschrijving op van de sleutel en geef de duur van de sleutel op. Selecteer **Opslaan** wanneer u klaar bent.

    ![sleutel opslaan](media\azure-stack-solution-machine-learning\image81.png)

Na het opslaan van de sleutel wordt de waarde van de sleutel weergegeven. Kopieer deze waarde, zoals deze later nodig is. De **sleutelwaarde** met de toepassing-ID is vereist voor aanmelding bij als de toepassing. De waarde van de sleutel waar de toepassing deze kan ophalen Store.

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image82.png)

#### <a name="get-tenant-id"></a>Tenant-ID ophalen

Als onderdeel van de configuratie van de service-eindpunt, VSTS vereist de **Tenant-ID** die overeenkomt met de Azure Stack-stempel is geïmplementeerd voor de AAD-Directory. Volg de stappen in deze sectie voor het verzamelen van de Tenant-id.

1.  Selecteer **Azure Active Directory**.

    ![azure active directory selecteren](media\azure-stack-solution-machine-learning\image83.png)

1.  De tenant-ID, selecteer **eigenschappen** voor de Azure AD-tenant.

    ![Azure AD-eigenschappen selecteren](media\azure-stack-solution-machine-learning\image84.png)

1.  Kopieer de **Map-id**. Deze waarde is de tenant-ID.

    ![tenant-id](media\azure-stack-solution-machine-learning\image85.png)

De Service-Principal-rechten voor het implementeren van resources in de Azure Stack-abonnement verlenen

Voor toegang tot resources in het abonnement, de toepassing toewijzen aan een rol. Bepaal welke rol staat voor de juiste machtigingen voor de toepassing. Zie voor meer informatie over de beschikbare rollen, [RBAC: ingebouwde rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Stel het bereik op het niveau van het abonnement, resourcegroep of resource. Machtigingen worden overgenomen op lagere niveaus van bereik. Een toepassing toevoegt aan de rol van lezer voor een resourcegroep kan bijvoorbeeld het lezen van de resourcegroep en alle resources die deze bevat.

1.  Ga naar het gewenste niveau van bereik om de toepassing te wijzen. Selecteer bijvoorbeeld het volgende om te wijzen aan een rol op het abonnementsbereik, **abonnementen**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image86.jpeg)

1.  Selecteer de **abonnement** (resourcegroep of resource) om de toepassing te wijzen.

    ![abonnement voor toewijzing selecteren](media\azure-stack-solution-machine-learning\image87.png)

1.  Selecteer **toegangsbeheer (IAM)**.

    ![Selecteer toegang](media\azure-stack-solution-machine-learning\image88.png)

1.  Selecteer **Toevoegen**.

    ![Selecteer toevoegen](media\azure-stack-solution-machine-learning\image89.png)

1.  Selecteer de rol om de toepassing te wijzen. De volgende afbeelding toont de **eigenaar** rol.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image90.png)

1.  Azure Active Directory-toepassingen worden niet standaard weergegeven in de beschikbare opties. Om de toepassing te zoeken **geeft u de naam** in het zoekvak en selecteer deze.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image91.png)

1.  Selecteer **opslaan** voltooien van de rol toe te wijzen. De toepassing wordt weergegeven in de lijst met gebruikers die zijn toegewezen aan een rol voor deze scope.

### <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

Azure Role-Based Access Control (RBAC) kunt over Geavanceerd toegangsbeheer voor Azure en Azure Stack. Met RBAC kunt kan alleen de mate van toegang die gebruikers nodig hebben om hun werk te worden verleend. Zie voor meer informatie over Role-Based Access Control [toegang beheren tot de Resources voor Azure-abonnement](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

**VSTS-Agentpools**

In plaats van elke agent afzonderlijk beheren, agents zijn ingedeeld in **agentpools**. Een agentpool definieert de delen grens voor alle agents in die groep. Agentpools zijn in VSTS gericht op het VSTS-account, dus kunnen worden gedeeld met teamprojecten. Zie voor meer informatie en een zelfstudie over het maken van VSTS agentpools [Agentpools maken en wachtrijen.](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts)

**APersonal toegangstoken (PAT) voor Azure Stack toevoegen**

 -  Meld u aan bij de VSTS-account en selecteert u op **accountprofiel** naam.

 -  Selecteer **beveiliging beheren** access token maken-pagina.

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image92.png)

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image93.jpeg)

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image94.jpeg)

> [!Note]  
> De informatie over beveiligingstoken verkrijgen. Het wordt niet opnieuw na het verlaten van dit scherm worden weergegeven.

1.  Kopieer de **token**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image95.png)

#### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>Installatie van die de VSTS build-agent op de Azure Stack gehoste Server bouwen

1.  Verbinding maken met de **Server bouwen** geïmplementeerd op de Azure Stack-host.

    > [!Note]  
    > Zorg ervoor dat Azure Stack gehoste bouwen Server is toegankelijk vanaf het openbare Internet. Als dat niet het geval is, werken met een Azure Stack-operators om toegang te krijgen.

    ```Bash  
    ssh <user>@<buildserver.publicip>
    ```

2.  De Server voor het bouwen van Ubuntu een upgrade naar de nieuwste versie (18.04):

    ```Bash  
    sudo su
    apt-get update
    apt-get upgrade
    apt-get dist-upgrade
    apt-get autoremove
    do-release-upgrade -d
    ```

    > [!Note]  
    > Met deze bewerking zal enige tijd duren.

2.  Toepassingen van de vereisten voor de buildserver installeren. Vanuit de bash shell van Ubuntu op basis van Build-Server uitgevoerd de volgende opdrachten:

    ```Bash  
    wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
    sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
    wget -q https://packages.microsoft.com/config/ubuntu/18.04/prod.list 
    sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
    sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
    sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
    sudo apt-get install apt-transport-https
    sudo apt-get update
    sudo apt-get install liblttng-ust0 libcurl3 libssl1.0.0 curl lsb-release libkrb5-3 zlib1g libicu60 aspnetcore-runtime-2.1 dotnet-sdk-2.1
    wget https://github.com/PowerShell/PowerShell/releases/download/v6.1.0-preview.3/powershell-preview_6.1.0-preview.3-1.ubuntu.18.04_amd64.deb
    sudo dpkg -i powershell-preview_6.1.0-preview.3-1.ubuntu.18.04_amd64.deb
    sudo apt-get install -f
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
        sudo tee /etc/apt/sources.list.d/azure-cli.list
    curl -L https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add –
    sudo apt-get update && sudo apt-get install azure-cli
    ```

2.  Downloaden en implementeren van de build-agent als een service met een **persoonlijk toegangstoken (PAT)** en uit te voeren als het account van de VM-beheer.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image96.png)

    ```Bash  
        cd \home\<user>
        sudo mkdir myagent && cd myagent
        wget https://vstsagentpackage.azureedge.net/agent/2.134.2/vsts-agent-linux-x64-2.134.2.tar.gz
        sudo tar zxvf vsts-agent-linux-x64-2.134.2.tar.gz
    ```

2.  Ga naar de map van de uitgepakte build-agent. Voer de volgende code.

    ```Bash  
        cd ..
        sudo chmod -R 777 myagent
        cd myagent
        ./config.sh
    ```

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image97.png)

2.  Na **./config.sh**is voltooid, voert de volgende code om in te schakelen van de service bij het opstarten van de server en start de service:

    ```Bash  
    sudo ./svc.sh install
    sudo ./svc.sh start
    ```

De agent is nu zichtbaar in VSTS-map.

#### <a name="endpoint-creation-permissions"></a>Machtigingen voor het eindpunt maken

Gebruikers kunnen eindpunten maken zodat VSTO builds Azure Service-apps naar de stack implementeren kunnen. VSTS verbindt met de build-agent, die vervolgens verbinding met Azure Stack maakt.

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image98.png)

1.  Op de **instellingen** in het menu **Security**.

2.  In de **VSTS groepen** lijst aan de linkerkant, selecteer **makers van eindpunt**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image99.png)

3.  Op de **tabblad leden** selecteert u de **+ toevoegen**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image100.png)

1.  Type **gebruikersnaam** en selecteert u de gebruikersnaam in de lijst.

2.  Selecteer **wijzigingen opslaan**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image101.png)

3.  In de **VSTS groepen** lijst aan de linkerkant, selecteer **eindpunt beheerders**.

4.  Op de **tabblad leden** selecteert u de **+ toevoegen**.

5.  Typ een **gebruikersnaam** en selecteert u die gebruiker in de lijst.

6.  Selecteer **wijzigingen opslaan.**

    ![buchatech](media\azure-stack-solution-machine-learning\image102.jpeg)

    De build-agent in Azure Stack krijgt aanwijzingen van VSTS, die vervolgens endpoint-informatie voor de communicatie met de Azure Stack brengt.

    VSTS aan Azure Stack-verbinding is nu gereed.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image103.png)

### <a name="configure-build-and-release-definitions"></a>Configureren van Build and Release-definities

Nu dat de verbindingen tot stand worden gebracht, wordt u handmatig toewijzen van het gemaakte Azure-eindpunt, AKS en Azure Container Registry naar de versie en release-definities.

#### <a name="create-the-build-definition-for-the-yaml-code"></a>De definitie van de bouwen voor het YAML-code maken

1.  Selecteer de sectie bouwt onder de Build and Release-hub en de definitie van een nieuwe maken.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image104.png)

1.  VSTS Git selecteren en selecteer de opslagplaats die eerder hebt gemaakt.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image105.png)

1.  Selecteer lege pijplijn als de sjabloon

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image106.png)

1.  Naam van de Build **kopie artefact** en selecteert u de Azure Stack Build-Server voor de Agent-wachtrij.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image107.png)

1.  Fase 1 selecteert in de processen en wijzig de naam **kopie artefact**, klikt u vervolgens **toevoegen van een taak** met de fase:

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image108.png)

1.  Selecteer **Build-artefacten publiceren** uit de **hulpprogramma** lijst en selecteer **toevoegen**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image109.png)

1.  Selecteer de **pad om te publiceren** en selecteer de **iris_deployment.yaml** bestand.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image110.png)

1.  Naam van het artefact **iris_deployment** en selecteer de locatie voor publiceren moet **Visual Studio Team Services/TFS**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image111.png)

1.  Selecteer **opslaan en in de wachtrij**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image112.png)

1.  Controleer de status van build van het door het selecteren van de build-ID.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image113.png)

Succes ziet er ongeveer als volgt uit:

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image114.png)

#### <a name="create-the-release-definition-for-the-yaml-code"></a>De definitie van de Release voor de YAML-code maken

1.  Selecteer de sectie Releases onder de Build and Release-hub, de definitie van een nieuwe

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image115.png)

1.  Selecteer lege pijplijn als sjabloon.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image106.png)

1.  De naam van de omgeving Azure Stack.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image116.png)

1.  Een nieuwe artefact toevoegen door te selecteren **artefacten** en **+ toevoegen**

2.  Selecteer Build als het brontype en **HybridMLIris** als het project.

3.  Selecteer vervolgens de Build-definitie eerder hebt gemaakt voor de bron.

4.  Selecteer vervolgens **toevoegen**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image117.png)

1.  Selecteer Azure Stack in omgevingen, en vervolgens een nieuwe taak toevoegen aan Azure Stack

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image118.png)


1.  Ingesteld op de Agent-fase wordt de wachtrij van de Agent op de Azure Stack die wordt gehost bouwen Server.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image119.png)

1.  Een nieuwe taak toevoegen aan deze fase, selecteert u de implementeren in Kubernetes taak onder implementeren en selecteer toevoegen.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image120.png)


1.  Geef het de naam **Kubectl toepassen** (standaardnaam) en selecteert u de opdracht toepassen.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image121.png)

    Maak nu een nieuwe verbinding van de Kubernetes-Service.

#### <a name="create-kubernetes-service-endpoint"></a>Kubernetes-Service-eindpunt maken

1.  Selecteer onder een verbinding met Kubernetes, de **+ nieuw** en selecteer**Kubernetes**in de lijst. u kunt dit eindpunt gebruiken om verbinding te maken de**VSTS**en de**Azure Container Service (AKS)**.

2.  **Verbindingsnaam**: Geef de naam van de verbinding.

3.  **Server-URL**: het adres van de container in de formathttp bevatten: / / {API-serveradres}

4.  **Kubeconfig**: als u de waarde Kubeconfig, kunt u de volgende Azure opdrachten uitvoeren in een opdrachtprompt geopend waarin de Administrator-bevoegdheden.

    > [!Important]  
    > Dit CLI-venster om uit te voeren van de volgende stappen gebruiken.

6.  In het CLI-venster moet u zich aanmelden bij Azure. [Meer informatie over az-inloggen](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

7.  Voer in de opdrachtregel in:

    ```CLI
        az login
    ```

10. Met deze opdracht retourneert een code te gebruiken in de browser op <https://aka.ms/devicelogin>.

11. Ga naar <https://aka.ms/devicelogin> in de browser. Wanneer u hierom wordt gevraagd, typt u de code in de CLI, ontvangen in de browser.

    ![Kubernetes-Service-eindpunt](media\azure-stack-solution-machine-learning\image122.png)

1.  Typ de volgende opdracht uit achter de opdrachtprompt om op te halen van de referenties voor toegang voor het Kubernetes-cluster.

### <a name="azure-ml-workbench-cli"></a>Azure ML Workbench CLI

AZ aks get-credentials resource-group <yourResourceGroup> naam <yourazurecontainerservice>

![Kubernetes-Service-eindpunt](media\azure-stack-solution-machine-learning\image123.png)

1.  Navigeer naar de **.kube**map onder de basismap (bijvoorbeeld: C:\\gebruikers\\<user>\\documenten\\Kube)

2.  Kopieer de inhoud van de**config**-bestand en plak deze in het venster verbinding met Kubernetes. Selecteer de**OK**knop.

    ![Kubernetes-Service-eindpunt](media\azure-stack-solution-machine-learning\image124.png)
    

3.  Zodra het Kubernetes-eindpunt is gemaakt en ingeschakeld, het selectievakje Gebruik configuratie bestanden voor het toevoegen van een configuratiebestand. Blader vervolgens naar het bestand iris_deployment.yaml in de gekoppelde artefacten.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image125.png)

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image126.png)

4.  Sla de release-definitie.

#### <a name="check-the-status-of-the-release-definition"></a>Controleer de status van de release-definitie. 

Nadat het is opgeslagen, moet een versie automatisch de release-definitie de VSTS Vliegende.

Controleer de status van de implementatie door het uitvoeren van een snelle opdracht in het opdrachtpromptvenster WSL en controleren van de UI van Kubernetes.

```Bash  
kubectl get deployments
```

De uitvoer moet er ongeveer als volgt, terwijl in het proces implementeren.

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image127.png)

```Bash  
kubectl proxy
```

Zodra de kubernetes UI wordt uitgevoerd, bladert u naar de implementatie op [ **https://localhost:8001/** ](https://localhost:8001/) navigeer vervolgens naar **Workloads-replicasets >**.

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image128.png)

### <a name="deploy-the-yaml-service"></a>Het YAML-Service implementeren

#### <a name="upload-the-irisserviceyaml-to-the-repository-and-sync-changes"></a>De iris_service.yaml uploaden naar de opslagplaats en synchronisatie van wijzigingen

1.  Navigeer naar de nieuwe gekloonde opslagplaats:

    ```Bash  
    cd /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/
    ```

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image75.png)

1.  Kopieer de **iris_service.yaml** -bestand in de opslagplaats.

    ```Bash  
    cp /mnt/c/users/<User>/documents/Kube/iris_service.yaml  /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/iris_service.yaml
    ```

1.  Voer de wijziging in GIT

    ```Bash  
    git add .
    git commit -m “Added Service YAML” 
    git push
    ```

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image129.png)

#### <a name="update-the-build-definition-for-the-yaml-code"></a>De Build-definitie voor de YAML-code bijwerken

1.  Selecteer de sectie bouwt onder de Build and Release-hub en selecteer de definitie van de eerder hebt gemaakt.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image130.png)

2.  Selecteer de knop bewerken om te bewerken van de definitie.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image131.png)

3.  **Een taak toevoegen** met de fase. Selecteer **Build-artefacten publiceren** uit de **hulpprogramma** lijst en selecteer **toevoegen**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image108.png)

4.  Geef het de naam **Kubectl toepassen** (standaardnaam) en selecteert u de opdracht toepassen.



#### <a name="update-the-release-definition-for-the-yaml-code"></a>De Release-definitie voor de YAML-code bijwerken

1.  Selecteer de sectie theReleases onder de Build and Release-hub en selecteert u de releasedefinitie eerder hebt gemaakt. Selecteer vervolgens de koppeling bewerken.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image132.png)

1.  Selecteer de omgeving **Azure Stack** vervolgens een nieuwe taak toevoegen aan Azure Stack.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image133.png)

1.  Toevoegen een **nieuwe taak** naar deze fase, selecteert u de **implementeren in Kubernetes** taak onder **implementeren** en selecteer **toevoegen**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image134.png)

1.  Geef het de naam **Kubectl toepassen** (standaardnaam) en selecteert u de opdracht toepassen.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image109.png)

1.  De verbinding Kubernates Service ingesteld op de Azure Stack-verbinding eerder hebt gemaakt en selecteer vervolgens de **configuratiebestanden gebruiken** selectievakje in om toe te voegen een configuratiebestand. Blader naar het bestand iris_service.yaml in de gekoppelde artefacten.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image135.png)


    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image136.png)

1.  Sla de release-definitie.

#### <a name="check-the-status-of-the-release-definition"></a>Controleer de status van de release-definitie

Nadat het is opgeslagen, moet een versie automatisch de release-definitie de VSTS Vliegende.

Controleer de status van de implementatie door het uitvoeren van een snelle opdracht in het opdrachtpromptvenster WSL en controleren van de UI van Kubernetes.

```Bash  
kubectl get deployments
```

De uitvoer moet er ongeveer als volgt, terwijl in het proces implementeren.

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image127.png)


```Bash  
kubectl proxy
```

Zodra de kubernetes UI wordt uitgevoerd, bladert u naar de implementatie op [ **https://localhost:8001/** ](https://localhost:8001/) navigeer vervolgens naar **Workloads-replicasets >**.

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image137.png)


### <a name="kubernetes-scoring-and-validation"></a>Kubernetes scoren en validatie

Start de gebruikersinterface van Kubernetes

```Bash  
kubectl proxy
```

Blader naar de Kubernetes-UI, en ga vervolgens naar **implementaties** -> **Iris-implementatie** -> **nieuwe replicaset**  ->  **Iris-implementatie-xxxxxxxxx** (waar de kruisjes zijn voor de implementatie-ID).

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image138.png)

Navigeer vervolgens naar **Services** en selecteer de **Extern eindpunt** van de Service om te valideren functioneert.

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image139.png)

Een van die vergelijkbaar is met het volgende validatiebericht weergegeven:

![Alternatieve tekst](media\azure-stack-solution-machine-learning\image140.png)

#### <a name="create-azure-stack-scoring-function-app-in-the-azure-stack-portal"></a>Azure Stack Scoring-functie-App in de Azure Stack-Portal maken

Een functie-app is vereist voor het hosten van de uitvoering van elke functie. Een functie-app kunt gebruiken als een logische eenheid voor eenvoudiger beheer, implementatie en delen van resources groeperen.

1.  Selecteer in de gebruikersportal van Azure Stack, de **+ nieuw** knop gevonden in de linkerbovenhoek, selecteer vervolgens**Web en mobiel** >**functie-App**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image141.png)

1.  Noem de functie **gegevensfuncties** en plaats deze in dezelfde resourcegroep met de resterende Machine Learning-inhoud. Laat het hulpprogramma automatisch een nieuwe app service-plan voor het maken en gebruiken van de storage-account eerder hebt gemaakt voor de app-opslag.

    ![Nieuwe functie app-instellingen definiëren](media\azure-stack-solution-machine-learning\image142.png)

1.  Selecteer**maken**inrichten en implementeren van de functie-app.

2.  Selecteer het meldingspictogram in de rechterbovenhoek van de portal en zoek de**implementatie is voltooid** bericht.

    ![Instellingen voor nieuwe functie-apps definiëren](media\azure-stack-solution-machine-learning\image143.png)

1.  Selecteer**naar de resource gaan** om de nieuwe functie-app weer te geven.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image144.png)

1.  Een nieuwe functie maken die door het selecteren van **functies**, dan zal de **+ nieuwe functie** knop.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image145.png)

1.  HTTP-Trigger selecteren

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image146.png)

1.  Selecteer **C\#**  als de taal en de naam van de functie: **clean-score-data**, en stelt u het autorisatieniveau op **anoniem**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image147.png)

1.  De inhoud van de voorbeeld-code voor opschonen-score-gegevens in de functie kopiëren en plakken

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image148.png)

#### <a name="use-postman-to-validate-functions"></a>Postman gebruiken om te valideren van functies

Om te controleren of dat u hebt ingesteld de functies en Kbernetes kunt correct u de gratis Postman-app testen en valideren van schema's en functies. U start dit proces, moet u eerst wat informatie verzamelen van uw Kubernetes-instantie.

1.  Blader naar de Kubernetes-UI, en ga vervolgens naar **implementaties** -> **Iris-implementatie** -> **nieuwe replicaset**  ->  **Iris-implementatie-xxxxxxxxx** (waar de kruisjes zijn voor de implementatie-ID)

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image138.png)

1.  Navigeer vervolgens naar **Services** en kopieer de **Extern eindpunt**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image149.png)

1.  Download en installeer de Postman-app [hier](https://www.getpostman.com/apps) indien nodig.

2.  Meld u aan bij de Postman-App en sluit het dialoogvenster voor het nieuwe bestand.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image150.png)

1.  Selecteer in bericht in de postman-app...

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image151.png)

1.  Plak de **Extern eindpunt** URL in de postman-app onder de **aanvraag-URL** toe te voegen  **\\score** aan het einde van de URL, zoals hieronder weergegeven.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image152.png)

1.  Selecteer de **hoofdtekst** tabblad en klik vervolgens op de gegevens typt als **onbewerkte**, klikt u vervolgens **JSON**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image153.png)

1.  Navigeer via een webbrowser naar **Extern eindpunt**. Het volgende toe te voegen aan de URL **/swagger.json** dit leidt tot de Services Swagger-bestand gebruikt om instellingen te controleren.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image154.png)

1.  Kopieert u het voorbeeld weergegeven in de **Swagger.JSON** bestand.

2.  In de Postman-App in het voorbeeld in de hoofdtekst van het bericht te plakken en selecteer **verzenden**. Er moet een waarde die vergelijkbaar is met het voorbeeld hieronder retourneren.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image155.png)

## <a name="step-7-create-an-azure-stack-storage-account-and-storage-queue"></a>Stap 7: Een Azure Stack-opslagaccount en de Storage-wachtrij maken

Maak een Azure Stack-opslagaccount en de Storage-wachtrij voor gegevens.

1.  Meld u aan bij de Gebruikersportal van Azure Stack. (Elke Azure Stack is een unieke portal URL)

2.  Vouw het menu aan de linkerkant op het menu met services te openen en kies in de gebruikersportal van Azure Stack **alle Services**. Schuif omlaag naar **opslag** en kies **opslagaccounts**. In de **Opslagaccounts** venster Kies **toevoegen**.

3.  Voer een naam voor het opslagaccount.

4.  Selecteer de replicatieoptie voor het opslagaccount: **LRS**.

5.  Geef een nieuwe resourcegroep op of selecteer een bestaande resourcegroep.

6.  Selecteer **lokale** voor de locatie voor het opslagaccount.

7.  Selecteer**maken**om het opslagaccount te maken.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image156.png)

1.  Kies de opslagaccount die onlangs zijn gemaakt.

2.  Selecteer op**wachtrijen**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image157.png)

1.  Selecteer op **+ wachtrij** en de wachtrij en selecteert u de naam **OK.**

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image158.png)

1.  Krijgen de **Connection String** voor de Storage-wachtrij en deze te kopiëren.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image159.png)

1.  Navigeer naar de Azure-functie-App en selecteer vervolgens **toepassingsinstellingen**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image160.png)

1.  In binnen de toepassingsinstellingen van de functie-App, bladert u omlaag naar de instellingen van toepassingen, en selecteer **+ nieuwe instelling toevoegen**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image161.png)

1.  Voer de naam van het opslagaccount in de **naam** veld toe te voegen aan het einde; _opslag

Hierdoor kan de toepassing om te begrijpen dat dit een eindpunt van de storage-account is.

1.  Plak de verbindingsreeks in de **waarde** veld.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image162.png)

1.  Schuif omlaag naar de bovenkant van de instellingen van de toepassing en selecteer **opslaan**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image163.png)

### <a name="update-the-scoring-function-to-use-storage-queue"></a>De Scoring-functie voor het gebruik van Storage-wachtrij bijwerken

1.  Selecteer op **integreren** op de functie, en schakelt de **ophalen** optie.

2.  Selecteer **Opslaan**.

3.  Selecteer vervolgens **+ nieuwe uitvoer** uit de uitvoer.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image164.png)

1.  Selecteer vervolgens **Azure Queue Storage** en kies **Selecteer**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image165.png)

1.  Update de **wachtrijnaam** naar de Opslagwachtrij eerder hebt gemaakt, en stel de **Opslagaccountverbinding** aan de Storage-accountverbinding gemaakt eerdere en selecteer **opslaan.**

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image166.png)

## <a name="step-8-create-a-function-to-handle-clean-data"></a>Stap 8: Maak een functie voor het afhandelen van nieuwe gegevens

Maak een nieuwe Azure Stack-functie voor het verplaatsen van de schoon gegevens uit Azure Stack in Azure.

1.  Een nieuwe functie maken die door het selecteren van **functies**, dan zal de **+ nieuwe functie** knop.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image167.png)

1.  Selecteer **Timertrigger**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image168.png)

1.  Selecteer **C\#**  als de taal en de naam van de functie: **uploaden naar azure** en stel de planning op **0 0 \*/1 \* \* \***  die in CRON notatie wordt eenmaal per uur.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image169.png)

### <a name="get-the-connection-string-to-the-azure-hosted-storage-account"></a>De verbindingsreeks naar het Azure gehoste Opslagaccount ophalen

1.  Blader naar <https://portal.azure.com> en selecteer vervolgens de **Azure Storage-Account** eerder hebt gemaakt.

2.  Selecteer **toegangssleutels**, kopieer vervolgens de **Connection String** voor het Opslagaccount.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image170.png)

### <a name="update-the-upload-to-azure-function-to-use-the-azure-hosted-storage"></a>Het uploaden naar azure te gebruiken van de Azure-opslag die wordt gehost functie bijwerken

1.  Navigeer naar de Azure-functie-App en selecteer vervolgens **toepassingsinstellingen**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image171.png)

1.  In binnen de toepassingsinstellingen van de functie-App, bladert u omlaag naar de instellingen van toepassingen, en selecteer **+ nieuwe instelling toevoegen**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image172.png)

1.  Voer de naam van het opslagaccount in de **naam** veld toe te voegen aan het einde; _opslag

Hierdoor kan de toepassing om te begrijpen dat dit een eindpunt van de storage-account is.

1.  Plak de Azure gehost Storage Account Connection String in het **waarde** veld.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image173.png)

1.  Schuif omlaag naar de bovenkant van de instellingen van de toepassing en selecteer **opslaan**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image174.png)

1.  Ga terug naar de **uploaden naar azure** functie.

2.  Selecteer op **integreren** op de functie.

3.  Selecteer vervolgens **+ nieuwe uitvoer** uit de uitvoer.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image175.png)

1.  Selecteer vervolgens **Azure Blob Storage** en kies **Selecteer**.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image176.png)

1.  Update de **pad** naar storage-container gemaakt eerder in de volgende indeling: **uploadeddata / {rand-guid} .txt**, en stel de **Opslagaccountverbinding** naar de Storage-accountverbinding met Azure gemaakt eerdere en selecteer **opslaan.**

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image177.png)

1.  Kopiëren en plakken die de inhoud van de voorbeeld-code voor **uploaden naar azure** in de functie.

2.  Wijzig zo nodig om te verwijzen naar de verbindingsreeks van de Storage-Accounts.

3.  Sla op en voer de Code uit.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image178.png)

1.  Controleer het gehoste Storage-account om de gegevens te zien is geparseerd naar de cloud van Azure: geslaagd ziet eruit als het hieronder.

    ![Alternatieve tekst](media\azure-stack-solution-machine-learning\image179.png)

De gegevens is opgeschoond van gevoelige gegevens door de Azure Stack die wordt gehost Kubernetes Machine Learning en geüpload naar de openbare Cloud van Azure van de on-premises Azure Stack, via de Azure Stack host functie-Apps, en de gegevens voor het uploaden van bestanden in een edge/verbroken kunt voorbereiden scenario.

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over Azure Cloud-patronen, [Cloudontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).