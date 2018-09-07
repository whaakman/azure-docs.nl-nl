---
title: Opnieuw opbouwen van een Contoso-app voor on-premises naar Azure | Microsoft Docs
description: Meer informatie over hoe een app naar Azure met behulp van Azure App Services, de Kubernetes-service, CosmosDB, Azure Functions en Cognitive services in Contoso opnieuw opgebouwd.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: raynew
ms.openlocfilehash: ff195093f97f00fca54e4e1c5800b6b0ecf7605a
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44054458"
---
# <a name="contoso-migration-rebuild-an-on-premises-app-to-azure"></a>Migratie van Contoso: opnieuw opbouwen van een on-premises app naar Azure

In dit artikel ziet u hoe Contoso worden gemigreerd en worden de SmartHotel360-app in Azure. Contoso migreert van de app-front-end virtuele machine naar Azure App Services-Web-apps. De back-end van de app is gebouwd met behulp van microservices is geïmplementeerd in containers die worden beheerd door Azure Kubernetes Service (AKS). De site communiceert met Azure Functions om huisdier foto-functionaliteit te bieden. 

Dit document is een in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en scenario's die laten zien instellen van een infrastructuur voor migratie, beoordeling van de on-premises bronnen voor migratie en het uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit. Aanvullende artikelen gaan we toevoegen na verloop van tijd.


**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Overzicht van de serie artikelen, strategie voor de migratie van Contoso en de voorbeeld-apps die worden gebruikt in de reeks. | Beschikbaar
[Artikel 2: Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Contoso bereidt u de on-premises infrastructuur en de Azure-infrastructuur voor migratie. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen in de reeks. | Beschikbaar
[Artikel 3: Evalueer on-premises bronnen voor migratie naar Azure](contoso-migration-assessment.md)  | Contoso wordt uitgevoerd een evaluatie van de on-premises SmartHotel360-app die wordt uitgevoerd op VMware. Contoso beoordeelt virtuele machines van app met behulp van de Azure Migrate-service en de app SQL Server-database met behulp van Data Migration Assistant. | Beschikbaar
[Artikel 4: Opnieuw hosten van een app op een Azure-VM en het beheerde exemplaar van SQL Database](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso wordt een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel360-app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso de app-database migreert naar een Azure SQL Database Managed Instance met de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar   
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | Contoso migreert de VM's van de SmartHotel360-app naar Azure-VM's met behulp van de Site Recovery-service. | Beschikbare [artikel 5: een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | De VM's van de SmartHotel360-app migreert Contoso naar Azure-VM's met behulp van de Site Recovery-service. | Beschikbaar
[Artikel 6: Een app op Azure Virtual machines en in een SQL Server AlwaysOn-beschikbaarheidsgroep opnieuw hosten](contoso-migration-rehost-vm-sql-ag.md) | Contoso migreert de SmartHotel360-app. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app. De Database Migration Service wordt gebruikt voor het migreren van de app-database naar een SQL Server-cluster dat wordt beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbare [artikel 7: een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Contoso is een lift-and-shift-migratie van de Linux-osTicket-app naar Azure-VM's, met behulp van de Site Recovery-service voltooid. | Beschikbaar
[Artikel 8: Een Linux-app op Azure VM's en Azure Database for MySQL Rehost](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migreert de Linux-osTicket-app naar Azure-VM's met behulp van Site Recovery. Deze migreert de app-database naar Azure Database for MySQL via MySQL Workbench. | Beschikbaar
[Artikel 9: Een app in een Azure-web-app en Azure SQL Database herstructureren](contoso-migration-refactor-web-app-sql.md) | Contoso de SmartHotel360-app migreert naar een Azure-web-app en de app-database migreert naar een Azure SQL Server-exemplaar met de Database Migration Assistant. | Beschikbaar    
[Artikel 10: Een Linux-app in een Azure-web-app en de Azure Database for MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | De Linux-app osTicket migreert Contoso naar een Azure-web-app op meerdere Azure-regio's met behulp van Azure Traffic Manager, geïntegreerd met GitHub voor continue levering. Contoso migreert de app-database naar een Azure Database for MySQL-exemplaar. | Beschikbaar
[Artikel 11: Herstructureren Team Foundation Server in Visual Studio teamservices](contoso-migration-tfs-vsts.md) | Contoso migreert de on-premises Team Foundation Server-implementatie naar Visual Studio Team Services in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app in Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Contoso migreert de SmartHotel360-app naar Azure. Vervolgens rearchitects wordt de weblaag app als een Windows-container die wordt uitgevoerd in Azure Service Fabric en de database met Azure SQL Database. | Beschikbaar 
Artikel 13: Opnieuw opbouwen van een app in Azure | Contoso wordt opnieuw gemaakt van de SmartHotel360-app met behulp van een scala aan mogelijkheden van Azure en services, waaronder Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services en Azure Cosmos DB. | In dit artikel


## <a name="business-drivers"></a>Zakelijke drijfveren

De IT-team leiderschap heeft nauw samengewerkt met zakelijke partners om te begrijpen wat ze willen bereiken bij deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit en wil leveren van gedifferentieerde ervaringen voor klanten met een Contoso-websites.
- **Flexibiliteit**: Contoso moet in staat om te reageren sneller dan de wijzigingen in de marketplace, waarmee het succes in een globale economie zijn. 
- **Schaal**: wanneer het bedrijf is groeit, het Contoso IT-team kunnen groeien hetzelfde tempo systemen moet opgeven.
- **Kosten**: Contoso wil licentiekosten minimaliseren.

## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft vastgemaakt omlaag app-vereisten voor de migratie. Deze vereisten zijn gebruikt voor het bepalen van de beste migratiemethode:
 - De app in Azure blijft als kritiek omdat het is vandaag. Het moet ook uitvoeren en eenvoudig schalen.
 - De app al dan niet mogen IaaS-componenten gebruiken. Alles wat moet worden gemaakt voor het gebruik van PaaS of serverloze services.
 - De app-builds moeten worden uitgevoerd in de cloudservices en containers moeten zich bevinden in een persoonlijke bedrijfsbrede containerregister in de cloud.
 - De API-service die wordt gebruikt voor huisdier foto's moet nauwkeurige en betrouwbare in de praktijk omdat beslissingen door de app moeten worden uitgevoerd in hun hotels. Alle huisdier verleende toegang te blijven op de hotels is toegestaan.
 - Om te voldoen aan vereisten voor een DevOps-pijplijn, maakt Contoso gebruik van Visual Studio Team Services (VSTS) voor Source Code Management (SCM), met Git-opslagplaatsen.  Geautomatiseerde builds en releases wordt gebruikt om code te bouwen en deze implementeren in de Azure-Web-Apps, Azure Functions en AKS.
 - Andere CI/CD-pijplijnen zijn nodig voor microservices in de back-end, en voor de website op de front-end.
 - De back endservices hebben een verschillende versie van de front-end web-app-cyclus.  Om te voldoen aan deze vereiste, zullen ze twee verschillende DevOps-pijplijnen te implementeren.
 - Contoso management goedkeuring nodig is voor alle website-front-end-implementatie en het CI/CD-pijplijn moet dit opgeven.


## <a name="solution-design"></a>Het ontwerp van oplossing

Na het vastmaken omlaag doelstellingen en vereisten, Contoso ontwerpen van een implementatieoplossing controleren en identificeert het migratieproces, met inbegrip van de Azure-services die worden gebruikt voor de migratie.

### <a name="current-app"></a>Huidige app

- De on-premises SmartHotel360 app is gelaagd over twee VM's (WEBVM en SQLVM).
- De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5)
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een on-premises domeincontroller (**contosodc1**).
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.


### <a name="proposed-architecture"></a>Voorgestelde architectuur

- De front-end van de app wordt geïmplementeerd als een Azure App Services-Web-app in de primaire Azure-regio.
- Een Azure-functie biedt uploads van huisdier foto's en de site communiceert met deze functionaliteit.
- De functie huisdier foto maakt gebruik van Cognitive Services-Vision-API en cosmos DB.
- De back-end van de site maakt gebruik van microservices. Deze zijn geïmplementeerd in containers die worden beheerd in de Azure Kubernetes service (AKS).
- Containers zijn gebouwd met behulp van VSTS en gepusht naar Azure Container Registry (ACR).
- Voorlopig implementeert Contoso handmatig u de Web-app en functie-code met Visual Studio.
- Microservices worden geïmplementeerd met behulp van een PowerShell-script waarmee Kubernetes opdrachtregelprogramma's wordt aangeroepen.

    ![Scenario-architectuur](./media/contoso-migration-rebuild/architecture.png) 

  
### <a name="solution-review"></a>Beoordelingen van oplossing

Contoso evalueert het ontwerp van de voorgestelde door het samenstellen van een lijst met voor- en nadelen.

**Overweging** | **Details**
--- | ---
**Professionals** | Met behulp van PaaS en serverloze oplossingen voor de end-to-end implementatie aanzienlijk minder beheer hoelang Contoso moet opgeven.<br/><br/> Verplaatsen naar een microservice-architectuur, kunt Contoso eenvoudig uitbreiden van de oplossing na verloop van tijd.<br/><br/> Nieuwe functionaliteit kan online worden gebracht zonder dat een van de bestaande oplossingen codebases gegevenscentrum worden verstoord.<br/><br/> De Web-App worden geconfigureerd met meerdere exemplaren met geen enkel storingspunt.<br/><br/> Automatisch schalen worden ingeschakeld zodat de app kan verschillende verkeer volumes worden verwerkt.<br/><br/> Met de overgang naar de PaaS-services Contoso buiten gebruik stellen verouderde oplossingen die worden uitgevoerd op Windows Server 2008 R2-besturingssysteem.<br/><br/> Cosmos DB heeft ingebouwde fouttolerantie, waarvoor geen configuratie door Contoso. Dit betekent dat de gegevenslaag niet langer een single point of failover is.
**Nadelen** | Containers zijn complexer dan andere opties voor de migratie. Het leerproces is mogelijk een probleem voor Contoso.  Ze introduceren een nieuw niveau van complexiteit waarmee veel van de waarde er de curve.<br/><br/> Het operationele team bij Contoso moet aan de slag om te begrijpen en ondersteuning voor Azure, containers en microservices voor de app.<br/><br/> Contoso is niet volledig DevOps geïmplementeerd voor de gehele oplossing. Contoso heeft nodig om na te denken over die voor de implementatie van services met AKS, functies en App-Services.



### <a name="migration-process"></a>Migratieproces

1. Contoso wordt voorbereid en bepalingen van de services en de infrastructuur die ze nodig hebben:

 - Een beheerde Kubernetes-cluster met behulp van AKS en ACR.
 - Een Cosmos-DB voor huisdier foto's.
 - Een Azure-functie voor uploads van huisdier foto's.
 - Een storage-account voor het opslaan van containers voor huisdier foto's en instellingen van de front-end-app.
 - Computer Vision-API om te evalueren huisdier foto's voor de app.
 - Een Azure-Web-app.
 
2. Nadat de infrastructuur ingesteld is, worden de Contoso-sets van een DevOps bouwen en release-pijplijn te maken, push en voortdurend de containers naar het ACR integreren.  
3. Ten slotte Contoso VSTS-projecten voor de front-end van de site implementeert, en stelt u een pijplijn te pushen en integreer deze voortdurend de code in Azure.

    ![Migratieproces](./media/contoso-migration-rebuild/migration-process.png) 

### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[AKS](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Maakt eenvoudiger beheer, implementatie en bewerkingen van Kubernetes. Biedt een volledig beheerde Kubernetes-containerorganisatieservice.  | AKS is een gratis service.  Betaal voor alleen de virtuele machines, en de bijbehorende opslag en netwerkresources die worden verbruikt. [Meer informatie](https://azure.microsoft.com/pricing/details/kubernetes-service/).
[Azure Functions](https://azure.microsoft.com/services/functions/) | Versnelt de ontwikkeling met een gebeurtenisgestuurde, serverloze rekenervaring. Schalen op aanvraag.  | Betaal alleen voor de verbruikte resources. Plan, wordt in rekening gebracht op basis van per seconde resourceverbruik en uitvoeringen. [Meer informatie](https://azure.microsoft.com/pricing/details/functions/).
[Azure Container Registry](https://azure.microsoft.com/services/container-registry/) | Slaat de installatiekopieën voor alle typen containerimplementaties. | De kosten op basis van functies, opslag en duur van gebruik. [Meer informatie](https://azure.microsoft.com/pricing/details/container-registry/).
[Azure App Service](https://azure.microsoft.com/services/app-service/containers/) | Bouw, implementeer en schaal geavanceerde web-apps, mobiele apps en API-apps die op elk platform kunnen worden uitgevoerd. | Voor App Service-abonnementen worden de kosten per seconde in rekening gebracht. [Meer informatie](https://azure.microsoft.com/pricing/details/app-service/windows/).

## <a name="prerequisites"></a>Vereisten

Dit is wat Contoso nodig heeft voor dit scenario:

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | Contoso abonnementen tijdens een eerdere artikel gemaakt. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.
**Azure-infrastructuur** | [Informatie over hoe](contoso-migration-infrastructure.md) Contoso instellen van een Azure-infrastructuur.
**Developer-vereisten** | Contoso heeft de volgende hulpprogramma's op een werkstation ontwikkelaar nodig:<br/><br/> - [Visual Studio 2017 Community Edition: Versie 15.5](https://www.visualstudio.com/)<br/><br/> .NET-workload is ingeschakeld.<br/><br/> [Git](https://git-scm.com/)<br/><br/> [Azure PowerShell](https://azure.microsoft.com/downloads/)<br/><br/> [Azure-CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)<br/><br/> [Docker CE (Windows 10) of Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) ingesteld voor het gebruik van Windows-Containers.



## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso de migratie wordt uitgevoerd:

> [!div class="checklist"]
> * **Stap 1: Back-end-resources inrichten**: Contoso richt het beheerde Kubernetes-cluster met behulp van AKS en Azure container registry.
> * **Stap 2: Een pijplijn voor de back-end instellen**: Contoso stelt u een DevOps-werkstroom. Ze importeren van de git-opslagplaats in VSTS en maak een nieuwe build-pijplijn. Ze bouwen Docker-containers en ze naar de ACR gepusht. Na het bouwproces wordt de nieuwe opslagplaatsen (ingevuld met de containers) weergegeven in de ACR. Contoso implementeert vervolgens de rest van de back endinfrastructuur, voor het migreren van de back-end.
> * **Stap 3: Front-resources inrichten**: Contoso front-infrastructuur, met inbegrip van blob-opslag voor de huisdier foto's, de Cosmos-database voor het opslaan van documenten met huisdier informatie en de Vision-API voor de website implementeert.
> * **Stap 4: Een pijplijn voor de front-end instellen**: Contoso de SmartHotel360-app gepubliceerd naar de Azure App service en de PetCheck functie-app wordt geïmplementeerd. Dit kan worden opgelost in twee afzonderlijke pijplijnen om toe te staan voor verschillende introductiecycli.



## <a name="step-1-provision-back-end-resources"></a>Stap 1: Back-end-resources inrichten

Beheerders van Contoso implementatie een script voor het maken van de beheerde Kubernetes-cluster met behulp van AKS en de Azure Container Registry (ACR) uitvoeren.

- De instructies voor deze sectie de **SmartHotel360-Azure-back-end** opslagplaats.
- De **SmartHotel360-Azure-back-end** GitHub-opslagplaats bevat alle van de software voor dit onderdeel van de implementatie.

### <a name="prerequisites"></a>Vereisten

1. Voordat ze, Contoso-beheerders ervoor te zorgen dat alle prerequisitie software geïnstalleerd op de dev-machine dat wordt gebruikt voor de implementatie.
2. Ze kloon de opslagplaats die lokaal op de dev-machine met behulp van Git: **git-kloon https://github.com/Microsoft/SmartHotel360-Azure-backend.git**


### <a name="provision-aks-and-acr"></a>AKS en ACR inrichten

De Contoso-beheerders inrichten gaat als volgt:

1.  Ze opent u de map met behulp van Visual Studio Code en verplaatst naar de **/implementatie/k8s** directory waarin het script **gen-aks-env.ps1**.
2. Ze het script voor het maken van de beheerde Kubernetes-cluster, met behulp van AKS en ACR worden uitgevoerd.

    ![AKS](./media/contoso-migration-rebuild/aks1.png)
 
3.  Open het bestand, ze de parameter $location bijwerken **eastus2**, en sla het bestand.

    ![AKS](./media/contoso-migration-rebuild/aks2.png)

4. Ze klikt u op **weergave** > **Integrated Terminal** de geïntegreerde terminal te openen in de Code.

    ![AKS](./media/contoso-migration-rebuild/aks3.png)

5. In de PowerShell-geïntegreerde terminal, ze zich aanmelden bij Azure met behulp van de opdracht Connect-AzureRmAccount. [Meer informatie](https://docs.microsoft.com/powershell/azure/get-started-azureps) over aan de slag met PowerShell.

    ![AKS](./media/contoso-migration-rebuild/aks4.png)

6. Verificatie van Azure CLI door het uitvoeren van de **az login** opdracht, en de instructies volgen om te verifiëren met behulp van hun webbrowser. [Meer informatie](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) punt zich heeft aangemeld met Azure CLI.

    ![AKS](./media/contoso-migration-rebuild/aks5.png)

7. Ze worden uitgevoerd de volgende opdracht, de naam van de resourcegroep van ContosoRG, de naam van het AKS-cluster smarthotel-aks-eus2 en de nieuwe naam van het containerregister te geven.

    ```
    .\gen-aks-env.ps1  -resourceGroupName ContosoRg -orchestratorName smarthotelakseus2 -registryName smarthotelacreus2
    ```
    ![AKS](./media/contoso-migration-rebuild/aks6.png)

8. Azure maakt een andere resourcegroep, met de resources voor het AKS-cluster.

    ![AKS](./media/contoso-migration-rebuild/aks7.png)

9. Nadat de implementatie is voltooid, installatie van de **kubectl** opdrachtregel-hulpprogramma. Het hulpprogramma is al geïnstalleerd op de Azure-CloudShell.

    **az aks install-cli**

10. Ze controleren of de verbinding met het cluster door het uitvoeren van de **kubectl ophalen knooppunten** opdracht. Het knooppunt is dezelfde naam als de virtuele machine in de automatisch gemaakte resourcegroep.

    ![AKS](./media/contoso-migration-rebuild/aks8.png)

11. Ze worden uitgevoerd de volgende opdracht start het Kubernetes-Dashboard: 

    **AZ aks bladeren--resourcegroep ContosoRG--naam smarthotelakseus2**

12. Er wordt een browsertabblad geopend op het Dashboard. Dit is een via een tunnel verbinding met de Azure CLI. 

    ![AKS](./media/contoso-migration-rebuild/aks9.png)




## <a name="step-2-configure-the-back-end-pipeline"></a>Stap 2: Configureer de back-end-pijplijn

### <a name="create-a-vsts-project-and-build-the-pipeline"></a>Een VSTS-project maken en de pijplijn bouwen

Beheerders van Contoso een VSTS-project maken en configureren van een CI-bouwen voor het maken van de container en wordt vervolgens naar de ACR gepusht. De instructies in deze sectie de [SmartHotel360-Azure-back-end](https://github.com/Microsoft/SmartHotel360-Azure-backend) opslagplaats.

1. Van visualstudio.com, ze een nieuw account maken (**contosodevops360.visualstudio.com**), en configureer deze voor het gebruik van Git.

2. Ze een nieuw project maken (**SmartHotelBackend**) met behulp van Git voor versiebeheer en Agile voor de werkstroom.

    ![VSTS](./media/contoso-migration-rebuild/vsts1.png) 


3. Ze importeren de GitHub-opslagplaats.

    ![VSTS](./media/contoso-migration-rebuild/vsts2.png)
    
4. In **Build and Release**, maken van een nieuwe build-pijplijn met VSTS Git als een bron, vanuit de geïmporteerde **SmartHotelBackend** opslagplaats. 

    ![VSTS](./media/contoso-migration-rebuild/vsts3.png)

6. Ze selecteren om te beginnen met een lege pijplijn.

    ![VSTS](./media/contoso-migration-rebuild/vsts4.png)  

7. Ze selecteren **gehost Linux Preview** voor de build-definitie.

    ![VSTS](./media/contoso-migration-rebuild/vsts5.png) 
 
8. In **fase 1**, het toevoegen van een **Docker Compose** taak. Deze taak builds de Docker compose.

    ![VSTS](./media/contoso-migration-rebuild/vsts6.png) 

9. Ze herhalen en Voeg nog een **Docker Compose** taak. Deze pushes de containers naar ACR.

     ![VSTS](./media/contoso-migration-rebuild/vsts7.png) 

8. Ze selecteert de eerste taak (voor het samenstellen) en de build configureren met het Azure-abonnement, de autorisatie en de ACR. 

    ![VSTS](./media/contoso-migration-rebuild/vsts8.png)

9. Deze geeft u het pad van de **borderel compose.yaml** bestand, in de **src** map van de opslagplaats. Ze selecteren voor het bouwen van installatiekopieën van de service en de meest recente code bevatten. Wanneer de actie is gewijzigd in **maken van installatiekopieën van de service**, de naam van de VSTS-taak wordt gewijzigd in **automatisch met het bouwen van services**

    ![VSTS](./media/contoso-migration-rebuild/vsts9.png)

10. Nu, ze de tweede Docker taak configureren (push). Ze selecteert u het abonnement en de **smarthotelacreus2** ACR. 

    ![VSTS](./media/contoso-migration-rebuild/vsts10.png)

11. Nogmaals, ze voert u het bestand naar het docker-compose.yaml-bestand en selecteer **service installatiekopieën pushen** en de meest recente code bevatten. Wanneer de actie is gewijzigd in **service installatiekopieën pushen**, de naam van de VSTS-taak wordt gewijzigd in **automatisch Push-services**

    ![VSTS](./media/contoso-migration-rebuild/vsts11.png)

12. Met de VSTS-taken is geconfigureerd, beheerders van Contoso opslaan van de build-definitie en start het bouwproces.

    ![VSTS](./media/contoso-migration-rebuild/vsts12.png)

13. Ze klikt u op de build-taak om voortgang te controleren.

    ![VSTS](./media/contoso-migration-rebuild/vsts13.png)

14. Nadat de build is voltooid, ziet u de ACR de nieuwe opslagplaatsen worden ingevuld met de containers die worden gebruikt door de microservices.

    ![VSTS](./media/contoso-migration-rebuild/vsts14.png)


### <a name="deploy-the-back-end-infrastructure"></a>De back-end-infrastructuur implementeren

Met het AKS-cluster gemaakt en de Docker-installatiekopieën die is gebouwd, implementeren Contoso beheerders nu voor de rest van de infrastructuur die zal worden gebruikt door de back-end microservices.

- Instructies in de sectie de [SmartHotel360-Azure-back-end](https://github.com/Microsoft/SmartHotel360-Azure-backend) opslagplaats.
- In de **/deploy/k8s/arm** map, wordt er één script om alle items te maken. 

Ze implementeren als volgt:

1. Ze open een opdrachtprompt voor ontwikkelaars en de opdracht az aanmelding gebruiken voor het Azure-abonnement.
2. Ze het bestand deploy.cmd gebruiken voor het implementeren van de Azure-resources in de resourcegroep ContosoRG en EUS2 regio, door de volgende opdracht te typen:

    **.\deploy.cmd azuredeploy - c eastus2 ContosoRG**

    ![Back-end implementeren](./media/contoso-migration-rebuild/backend1.png)

2. In de Azure-portal, worden vastgelegd de verbindingsreeks voor elke database, moet later worden gebruikt.

    ![Back-end implementeren](./media/contoso-migration-rebuild/backend2.png)

### <a name="create-the-back-end-release-pipeline"></a>De back-end-release-pijplijn maken

Nu, Contoso-beheerders het volgende doen:

- Implementeer de NGINX-ingangscontroller om toe te staan van inkomend verkeer naar de services.
- Implementeert de microservices naar het AKS-cluster.
- Als eerste stap bijwerken ze de verbindingstekenreeksen naar de microservices met VSTS. Ze configureert u een nieuwe VSTS Release-pijplijn voor het implementeren van de microservices.
- De instructies in deze sectie de [SmartHotel360-Azure-back-end](https://github.com/Microsoft/SmartHotel360-Azure-backend) opslagplaats.
- Houd er rekening mee dat sommige van de configuratie-instellingen (bijvoorbeeld Active Directory B2C) worden niet behandeld in dit artikel. Lees meer informatie over deze instellingen in de opslagplaats.

Ze maken de pijplijn:

1. Met behulp van Visual Studio bijwerken de **/deploy/k8s/config_local.yml** -bestand met de database-verbindingsgegevens ze eerder hebt genoteerd.

    ![DB-verbindingen](./media/contoso-migration-rebuild/back-pipe1.png)

2. Deze VSTS opent, en in de SmartHotel360-project in **Releases**, klikt de gebruiker op **+ nieuwe pijplijn**.

    ![Nieuwe pijplijn](./media/contoso-migration-rebuild/back-pipe2.png)

3. Ze klikt u op **leeg taak** starten van de pijplijn zonder een sjabloon.

    ![Lege taak](./media/contoso-migration-rebuild/back-pipe3.png)

4. Beschikt over de namen van de omgeving en de pijplijn.

      ![Naam van de omgeving](./media/contoso-migration-rebuild/back-pipe4.png)

      ![Naam pijplijn](./media/contoso-migration-rebuild/back-pipe5.png)

5. Ze toevoegen een artefact.

     ![Artefact toevoegen](./media/contoso-migration-rebuild/back-pipe6.png)

6. Ze selecteren **Git** als de bron typt, en geef het project, de bron en de master-vertakking voor de SmartHotel360-app.

    ![Instellingen voor artefact](./media/contoso-migration-rebuild/back-pipe7.png)

7. Ze klikt u op de taakkoppeling.

    ![Taakkoppeling](./media/contoso-migration-rebuild/back-pipe8.png)

8. Ze toevoegen een nieuwe Auzre PowerShell-taak zodat ze een PowerShell-script in een Azure-omgeving uitvoeren kunnen.

    ![PowerShell in Azure](./media/contoso-migration-rebuild/back-pipe9.png)

9. Ze selecteert het Azure-abonnement voor de taak en selecteert u de **deploy.ps1** script uit de Git-opslagplaats.

    ![Script uitvoeren](./media/contoso-migration-rebuild/back-pipe10.png)


10. Ze toevoegen argumenten aan het script. het script alle inhoud van het cluster wordt verwijderd (met uitzondering van **inkomend** en **controller voor binnenkomend verkeer**), en implementeert de microservices.

    ![Scriptargumenten](./media/contoso-migration-rebuild/back-pipe11.png)

11. Ze stelt de gewenste versie van Azure PowerShell naar de meest recente en opslaan van de pijplijn.
12. Ze terug verplaatsen naar de **Release** pagina en maak handmatig een nieuwe versie.

    ![Nieuwe release](./media/contoso-migration-rebuild/back-pipe12.png)

13. Ze klikt u op de release nadat deze is gemaakt, en met **acties**, klikt de gebruiker op **implementeren**.

      ![Implementeren van release](./media/contoso-migration-rebuild/back-pipe13.png)  

14. Wanneer de implementatie voltooid is, worden ze uitgevoerd de volgende opdracht uit om de status van services, met behulp van de Azure Cloud Shell te controleren: **kubectl services ophalen**.


## <a name="step-3-provision-front-end-services"></a>Stap 3: Een front-end-services inrichten

Beheerders van Contoso moet de infrastructuur die wordt gebruikt door de front-end-apps implementeren. Maken van een blob storage-container voor het opslaan van huisdier afbeeldingen. de Cosmos-database voor het opslaan van documenten met de huisdier informatie; en de Vision-API voor de website. 

Instructies voor deze sectie de [SmartHotel360-openbare-web](https://github.com/Microsoft/SmartHotel360-public-web) opslagplaats.

### <a name="create-blob-storage-containers"></a>Blob Storage-containers maken

1.  Ze open in Azure portal het opslagaccount dat is gemaakt en klikt op **Blobs**.
2.  Ze een nieuwe container maken (**huisdieren**) waarbij het niveau van de openbare toegang is ingesteld op de container. Gebruikers wordt hun huisdier foto's uploaden naar deze container.

    ![Storage-blob](./media/contoso-migration-rebuild/blob1.png)

3. Maken van een tweede nieuwe container met de naam **instellingen**. Een bestand met de instellingen voor de app-front-end wordt in deze container worden geplaatst.

    ![Storage-blob](./media/contoso-migration-rebuild/blob2.png)

4. Het vastleggen van de toegangsgegevens voor het opslagaccount in een tekstbestand, voor toekomstig gebruik.

    ![Storage-blob](./media/contoso-migration-rebuild/blob2.png)

### <a name="provision-a-cosmos-database"></a>Een Cosmos-database inrichten

Beheerders van Contoso inrichten een Cosmos-database moet worden gebruikt voor huisdier informatie.

1. Het maken van een **Azure Cosmos DB** in de Azure Marketplace.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos1.png)

2. Ze een naam opgeven (**contosomarthotel**), selecteert u de SQL-API en plaats deze in de resourcegroep productie ContosoRG, in de regio VS-Oost 2.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos2.png)

3. Ze toevoegen een nieuwe verzameling aan de database met de standaardcapaciteit en doorvoer.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos3.png)


4. Ze Houd er rekening mee de verbindingsgegevens voor de database, voor toekomstig gebruik. 

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos4.png)


### <a name="provision-computer-vision"></a>Inrichten Computer Vision

Beheerders van Contoso inrichten van de Computer Vision-API. De API wordt aangeroepen door de functie, om te evalueren afbeeldingen geüpload door gebruikers.

1. Het maken van een **Computer Vision** exemplaar in de Azure Marketplace.

     ![Computer Vision](./media/contoso-migration-rebuild/vision1.png)

2. Het inrichten van de API (**smarthotelpets**) in de productieresource groep ContosoRG, in de regio VS-Oost 2.

    ![Computer Vision](./media/contoso-migration-rebuild/vision2.png)

3. Ze opslaan de verbindingsinstellingen voor de API in een tekstbestand voor later gebruik.

     ![Computer Vision](./media/contoso-migration-rebuild/vision3.png)


### <a name="provision-the-azure-web-app"></a>De Azure-Web-App inrichten

Beheerders van Contoso inrichten de web-app met behulp van de Azure portal.


1. Ze selecteren **Web-App** in de portal.

    ![Web-app](media/contoso-migration-rebuild/web-app1.png)

2. Ze bieden een app-naam (**smarthotelcontoso**), uitvoeren op Windows en plaats deze in de groep van de productie-resources **ContosoRG**. Ze maken een nieuwe Application Insights-instantie voor het bewaken van de app...

    ![Web-app-naam](media/contoso-migration-rebuild/web-app2.png)

3. Als ze klaar bent, worden ze, blader naar het adres van de app om te controleren of dat deze correct is gemaakt.

4. In Azure portal maakt ze nu een staging-site voor de code. de pijplijn wordt geïmplementeerd naar deze sleuf. Dit zorgt ervoor dat er code in productie is niet geplaatst totdat beheerders een release uitvoeren.

    ![Web-app staging-site](media/contoso-migration-rebuild/web-app3.png)



### <a name="provision-the-azure-function-app"></a>De Azure-functie-app inrichten

Beheerders van Contoso inrichten in Azure portal, de functie-App.

1. Ze selecteren **functie-App**.

    ![Functie-app maken](./media/contoso-migration-rebuild/function-app1.png)

2. Ze bieden een app-naam (**smarthotelpetchecker**). Ze de app plaatsen in de resourcegroep van de productie **ContosoRG**. Ze de hosting plaats ingesteld op **Verbruiksabonnement**, en plaatst u de app in de regio VS-Oost 2. Een nieuw opslagaccount is gemaakt, samen met een exemplaar van de Application Insights voor bewaking.

    ![Instellingen voor functie-apps](./media/contoso-migration-rebuild/function-app2.png)


3. Nadat de app is geïmplementeerd, worden ze, blader naar het adres van de app om te controleren of dat deze correct is gemaakt.


## <a name="step-4-set-up-the-front-end-pipeline"></a>Stap 4: De front-pijplijn instellen

Contoso-beheerders maken twee verschillende projecten voor de front-end-site. 

1. In VSTS, maken ze een project **SmartHotelFrontend**.

    ![Front-end-project](./media/contoso-migration-rebuild/function-app1.png)

2. Ze importeren de [SmartHotel360 front-end](https://github.com/Microsoft/SmartHotel360-public-web.git) Git-opslagplaats naar het nieuwe project.
3. Voor de functie-App maken van een ander VSTS-project (SmartHotelPetChecker) en importeer het [PetChecker](https://github.com/Microsoft/SmartHotel360-PetCheckerFunction ) Git-opslagplaats in dit project.

### <a name="configure-the-web-app"></a>De Web-App configureren

Nu configureren beheerders van Contoso de Web-App voor het gebruik van Contoso-resources.

1. Ze verbinding maken met de VSTS-project en kloon de opslagplaats lokaal naar de dev-machine.
2. Ze open de map om weer te geven van alle bestanden in de opslagplaats in Visual Studio.

    ![Bestanden van de opslagplaats](./media/contoso-migration-rebuild/configure-webapp1.png)

3. Ze bijwerken zo nodig wijzigingen in de configuratie.

    - Wanneer de web-app wordt gestart, wordt er gezocht naar de **SettingsUrl** app-instelling.
    - Deze variabele moet een URL die verwijst naar een configuratiebestand bevatten.
    - Standaard is de instelling die wordt gebruikt een openbaar eindpunt.

4.  Ze bijwerken het bestand /config-sample.json/sample.json.

    - Dit is het configuratiebestand voor het web wanneer u het openbare eindpunt.
    - Ze bewerken de **URL's** en **pets_config** secties met de waarden voor de AKS-API-eindpunten, de storage-accounts en de Cosmos-database.
    - De URL's moeten overeenkomen met de DNS-naam van de nieuwe web-app die Contoso maakt.
    - Dit is voor Contoso, **smarthotelcontoso.eastus2.cloudapp.azure.com**.

    ![JSON-instellingen](./media/contoso-migration-rebuild/configure-webapp2.png)

5. Nadat het bestand is bijgewerkt, ze wijzigen **smarthotelsettingsurl**, en dit uploaden naar de blog van storage ze eerder hebt gemaakt.

    ![Naam wijzigen en uploaden](./media/contoso-migration-rebuild/configure-webapp3.png)

6. Ze klikt u op het bestand om de URL te verkrijgen. De URL wordt gebruikt door de app wanneer deze wordt deze de configuratiebestanden.

    ![App-URL](./media/contoso-migration-rebuild/configure-webapp4.png)

7. In de **appsettings. Production.JSON** -bestand, dat ze werken de **SettingsURL** naar de URL van het nieuwe bestand.

    ![URL bijwerken](./media/contoso-migration-rebuild/configure-webapp5.png)

### <a name="deploy-the-website-to-the-azure-app-service"></a>De website kunt implementeren in Azure App Service

Contoso-beheerders kunnen nu de website publiceren.


1. Ze VSTS, openen en klik in de **SmartHotelFrontend** project in **van Builds en Releases**, klikt de gebruiker op **+ nieuwe pijplijn**.
2. Ze selecteren **VSTS Git** als een bron.

    ![Nieuwe pijplijn](./media/contoso-migration-rebuild/vsts-publishfront1.png)

3. Ze selecteert de **ASP.NET Core** sjabloon.
4. Ze controleren van de pijplijn en controleer of **webprojecten publiceren** en **gepubliceerd projecten Zip** zijn geselecteerd.

    ![De instellingen voor pijplijn](./media/contoso-migration-rebuild/vsts-publishfront2.png)

5. In **Triggers**, ze inschakelen van continue integratie en toevoegen van de master-vertakking. Dit zorgt ervoor dat elke tim de oplossing nieuwe code is doorgevoerd in de master-vertakking is, de build-pijplijn wordt gestart.

    ![Continue integratie](./media/contoso-migration-rebuild/vsts-publishfront3.png)

6. Ze klikt u op **opslaan en in de wachtrij** een build te starten.
7. Nadat de build is voltooid, ze configureren van een release-pijplijn met de **Azure App Service-implementatie**.
8. Ze bieden een omgevingsnaam **fasering**.

    ![Naam van de omgeving](./media/contoso-migration-rebuild/vsts-publishfront4.png)

9. Ze een artefact toevoegen en selecteer de build die ze zojuist hebt geconfigureerd.

     ![Artefact toevoegen](./media/contoso-migration-rebuild/vsts-publishfront5.png)

6. Ze klikt u op het bliksempictogram bolt op de artifcat en continue implementatie inschakelen.

    ![Doorlopende implementatie](./media/contoso-migration-rebuild/vsts-publishfront6.png)

7. In **omgeving**, klikt de gebruiker op **fase 1, 1 taak** onder **fasering**.
8. Na het selecteren van het abonnement en de appnaam, die ze openen de **Azure App Service implementeren** taak. De implementatie is geconfigureerd voor het gebruik van de **staging** implementatiesite. U maakt automatisch code voor controle en goedkeuring in deze sleuf.

     ![Sleuf](./media/contoso-migration-rebuild/vsts-publishfront7.png)

9. In de **nieuwe release-pijplijn**, het toevoegen van een nieuwe omgeving.

    ![Nieuwe omgeving](./media/contoso-migration-rebuild/vsts-publishfront8.png)

10. Ze selecteren **Azure App Service-implementatie met de sleuf**, en de naam van de omgeving **Prod**.

    ![Naam van de omgeving](./media/contoso-migration-rebuild/vsts-publishfront9.png)

11. Ze klikt u op **fase 1, 2 taken**, en selecteer het abonnement, de naam van app service, en **staging** sleuf.

    ![Naam van de omgeving](./media/contoso-migration-rebuild/vsts-publishfront10.png)

12. Ze verwijderen de **Azure App Service implementeren in sleuf** vanuit de pipeline. Het is er tijdens de vorige stappen geplaatst.

    ![Verwijderen uit de pijplijn](./media/contoso-migration-rebuild/vsts-publishfront11.png)

13. Sla ze op de pijplijn. Klik op de pijplijn die ze op in **na de implementatie voorwaarden**.

    ![Na de implementatie](./media/contoso-migration-rebuild/vsts-publishfront12.png)

14. Ze maken het mogelijk **na de implementatie goedkeuringen**, en voeg een lead ontwikkelen als de fiatteur.

    ![Na de implementatie goedkeuring](./media/contoso-migration-rebuild/vsts-publishfront13.png)

15. In de Build-pijplijn, ze handmatig een vliegende start een build. Dit activeert de nieuwe release-pijplijn, die de site in de staging-site implementeert. Voor Contoso, de URL voor de sleuf is **https://smarthotelcontoso-staging.azurewebsites.net/**.
16. Nadat de build is voltooid en de versie worden geïmplementeerd op de site, VSTS e-mailberichten de lead ontwikkelen voor goedkeuring.
17. De lead dev klikken **weergeven goedkeuring**, en kunnen goedkeuren of afwijzen van de aanvraag in het VSTS-portal.

    ![Goedkeuringse-mail](./media/contoso-migration-rebuild/vsts-publishfront14.png)

16. De potentiële klant wordt een opmerking en keurt deze goed. Hiermee start u de wisseling van het **staging** en **prod** sleuven en de build is verplaatst naar productie.

    ![Goedkeuren en wisselen](./media/contoso-migration-rebuild/vsts-publishfront15.png)

17. De pijplijn is voltooid de wisseling.

    ![Wisseling voltooien](./media/contoso-migration-rebuild/vsts-publishfront16.png)

18. De controles team de **prod** sleuf om te controleren of de web-app in productie **https://smarthotelcontoso.azurewebsites.net/**.


### <a name="deploy-the-petchecker-function-app"></a>De PetChecker functie-App implementeren

Beheerders van Contoso implementeren de app als volgt.

1. Ze kloon de opslagplaats lokaal naar de dev-machine door verbinding te maken aan het VSTS-project.
2. Ze open de map om weer te geven van alle bestanden in de opslagplaats in Visual Studio.
3. Ze opent de **src/PetCheckerFunction/local.settings.json** bestand en het toevoegen van de app-instellingen voor opslag, de Cosmos-database en de Computer Vision-API.

    ![De functie implementeren](./media/contoso-migration-rebuild/function5.png)

4. Ze doorvoeren van de code en deze terug naar VSTS, pushen hun wijzigingen synchroniseren.
5. Ze een nieuwe Build-pijplijn toevoegen en selecteer **VSTS Git** voor de bron.
6. Ze selecteert de **ASP.NET Core (.NET Framework)** sjabloon.
7. Ze accepteer de standaardwaarden voor de sjabloon.
8. In **Triggers**, selecteert u vervolgens aan **inschakelen van continue integratie**, en klikt u op **opslaan en in de wachtrij** een build te starten.
9. Nadat de build is voltooid, ze een Release-pijplijn bouwen toe te voegen de **Azure App Service-implementatie met de sleuf**.
10. Naam van de omgeving **Prod**, en selecteer het abonnement. Ze stelt de **App-type** naar **functie Azië en Stille Oceaan**, en de naam van de app service als **smarthotelpetchecker**.

    ![Function App](./media/contoso-migration-rebuild/petchecker2.png)

11. Het toevoegen van een artefact **bouwen**.

    ![Artefact](./media/contoso-migration-rebuild/petchecker3.png)

12. Ze maken het mogelijk **trigger voor continue implementatie**, en klikt u op **opslaan**.
13. Ze klikt u op **nieuwe build in de wachtrij** om uit te voeren van de volledige CI/CD-pijplijn.
14. Nadat de functie is geïmplementeerd, deze wordt weergegeven in de Azure-portal met de **met** status.

    ![De functie implementeren](./media/contoso-migration-rebuild/function6.png)


7. Ze bladeren naar de app om te testen of de app huisdier Checker werkt zoals verwacht op [ http://smarthotel360public.azurewebsites.net/Pets ](http://smarthotel360public.azurewebsites.net/Pets).
8. Ze klikt u op de avatar voor het uploaden van een afbeelding.

    ![De functie implementeren](./media/contoso-migration-rebuild/function7.png)

9. De eerste foto die ze willen controleren is van een kleine hond.

    ![De functie implementeren](./media/contoso-migration-rebuild/function8.png)

10. De app retourneert een bericht van acceptatie.

    ![De functie implementeren](./media/contoso-migration-rebuild/function9.png)






## <a name="review-the-deployment"></a>De implementatie controleren

Met de gemigreerde resources in Azure moet de Contoso nu volledig operationeel maken en de nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

- Contoso heeft nodig om ervoor te zorgen dat de nieuwe databases die beveiligd zijn. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- De app moet worden bijgewerkt voor het gebruik van SSL met certificaten. De containerexemplaar moet opnieuw worden geïmplementeerd om te reageren op 443.
- Contoso moet rekening houden met Key Vault met voor de beveiliging van geheimen voor hun Service Fabric-apps. [Meer informatie](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="backups-and-disaster-recovery"></a>Back-ups en herstel na noodgevallen

- Contoso moet voor een overzicht back-vereisten voor de Azure SQL-Database. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Contoso moet rekening houden met SQL-failover-groepen om regionale failover voor de database te implementeren. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Contoso kan gebruikmaken van geo-replicatie voor de ACR-premium SKU. [Meer informatie](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).
- Cosmos DB maakt automatisch een reservekopie van. Contoso kan [meer](https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore) over dit proces.

### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

- Nadat alle resources worden geïmplementeerd, Contoso Azure tags op basis van moet toewijzen hun [infrastructuur plannen](contoso-migration-infrastructure.md#set-up-tagging).
- Alle licenties zijn ingebouwd in de kosten van de PaaS-services die van Contoso gebruikmaakt al. Dit wordt afgetrokken van de EA.
- Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing die u helpt bij het gebruiken en beheren van Azure en andere cloudresources.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management.

## <a name="conclusion"></a>Conclusie

In dit artikel worden Contoso de SmartHotel360-app in Azure. De on-premises app die front-end virtuele machine is opnieuw opgebouwd op Azure App Services Web Apps. De back-end van de app is gebouwd met behulp van microservices is geïmplementeerd in containers die worden beheerd door Azure Kubernetes Service (AKS). Contoso uitgebreide functionaliteit van een app met een huisdier foto-app.




