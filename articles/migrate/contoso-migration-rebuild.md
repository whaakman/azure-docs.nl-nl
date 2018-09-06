---
title: Opnieuw opbouwen van een Contoso-app voor on-premises naar Azure | Microsoft Docs
description: Meer informatie over hoe een app naar Azure met behulp van Azure App Services, de Kubernetes-service, CosmosDB, Azure Functions en Cognitive services in Contoso opnieuw opgebouwd.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: raynew
ms.openlocfilehash: 17212c076ef296a24021213b0aa887de930a44ac
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43783352"
---
# <a name="contoso-migration-rebuild-an-on-premises-app-to-azure"></a>Migratie van Contoso: opnieuw opbouwen van een on-premises app naar Azure

In dit artikel ziet u hoe Contoso worden gemigreerd en worden de SmartHotel360-app in Azure. Zij migreren van de app-front-end virtuele machine naar Azure App Services-Web-apps. De back-end van de app is gebouwd met behulp van microservices is geïmplementeerd in containers die worden beheerd door Azure Kubernetes Service (AKS). De site communiceert met Azure Functions biedt huisdier foto-functionaliteit. 

Dit document is een in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso de on-premises resources naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en scenario's die laten zien instellen van een infrastructuur voor migratie, beoordeling van de on-premises bronnen voor migratie en het uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit en aanvullende artikelen na verloop van tijd toegevoegd.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen. | Beschikbaar
[Artikel 3: On-premises resources evalueren](contoso-migration-assessment.md)  | Laat zien hoe een evaluatie van een on-premises twee lagen SmartHotel360 app waarop VMware wordt uitgevoerd in Contoso. Contoso beoordeelt de virtuele machines van een app met de [Azure Migrate](migrate-overview.md) -service en de SQL Server-database van de app met de [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Een app op Azure VM's en een beheerde SQL-exemplaar opnieuw hosten](contoso-migration-rehost-vm-sql-managed-instance.md) | Ziet u hoe Contoso een lift-and-shift-migratie naar Azure voor de SmartHotel360-app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database naar een SQL beheerd exemplaar, met behulp van de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | Ziet u hoe Contoso de SmartHotel360-app met behulp van Site Recovery alleen VM's migreren. | Beschikbaar
[Artikel 6: Een app op Azure VM's en SQL Server Always On Availability Group opnieuw hosten](contoso-migration-rehost-vm-sql-ag.md) | Ziet u hoe Contoso migreert de SmartHotel360-app. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app en de Database Migration service de app-database migreren naar een SQL Server-cluster dat is beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Ziet u hoe Contoso heeft een lift-and-shift-migratie van de Linux-osTicket-app op virtuele machines van Azure met Site Recovery | Beschikbaar
[Artikel 8: Een Linux-app op Azure VM's en Azure MySQL-Server opnieuw hosten](contoso-migration-rehost-linux-vm-mysql.md) | Ziet u hoe Contoso de osTicket Linux app overzet naar virtuele Azure-machines met behulp van Site Recovery en de app-database migreert naar een Azure MySQL-Server-exemplaar met behulp van MySQL Workbench. | Beschikbaar
[Artikel 9: Een app op Azure Web Apps en Azure SQL database herstructureren](contoso-migration-refactor-web-app-sql.md) | Ziet u hoe Contoso de SmartHotel360-app is gemigreerd naar een Azure-Web-App en de app-database migreert naar Azure SQL Server-exemplaar | Beschikbaar
[Artikel 10: Een Linux-app naar Azure WebApps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | Ziet u hoe Contoso de osTicket Linux app migreert naar Azure Web Apps op meerdere locaties, geïntegreerd met GitHub voor continue levering. Zij migreren de app-database naar een Azure MySQL-exemplaar. | Beschikbaar
[Artikel 11: Herstructureren TFS op VSTS](contoso-migration-tfs-vsts.md) | Ziet u hoe Contoso de on-premises Team Foundation Server (TFS)-implementatie door te migreren migreert het naar Visual Studio Team Services (VSTS) in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app naar Azure-containers en SQL Database](contoso-migration-rearchitect-container-sql.md) | Ziet u hoe Contoso migreert en rearchitects hun SmartHotel360-app naar Azure. Ze opnieuw ontwerpen voor de laag van de web-app als een Windows-container en de app-database in een Azure SQL Database. | Beschikbaar
Artikel 13: Opnieuw opbouwen van een app naar Azure | Ziet u hoe Contoso de SmartHotel360-app met een scala aan mogelijkheden van Azure en -services, waaronder App Services, Azure Kubernetes, Azure Functions, Cognitive services en Cosmos DB opnieuw. | In dit artikel.

In dit artikel worden de twee lagen Windows gemigreerd door Contoso. NET SmartHotel360-app die worden uitgevoerd op virtuele VMware-machines naar Azure. Als u wilt deze app wilt gebruiken, wordt geleverd als open source en u kunt downloaden via [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Zakelijke drijfveren

De IT-team leiderschap heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat ze willen bereiken bij deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit. Ze willen gedifferentieerde ervaringen bieden voor hun klanten op hun websites.
- **Flexibiliteit**: Contoso moet in staat om te reageren sneller dan de wijzigingen in de marketplace, waarmee het succes in een globale economie zijn. 
- **Schaal**: wanneer het bedrijf met succes groeit, Contoso IT-systemen die zijn kunnen groeien met de snelheid die dezelfde moet opgeven.
- **Kosten**: Contoso wil licentiekosten minimaliseren.

## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft vastgemaakt omlaag app-vereisten voor de migratie. Deze vereisten zijn gebruikt voor het bepalen van de beste migratiemethode:
 - De app in Azure blijft als kritiek omdat het is vandaag. Het moet ook uitvoeren en eenvoudig schalen.
 - De app al dan niet mogen IaaS-componenten gebruiken. Alles wat moet worden gemaakt voor het gebruik van PaaS of serverloze services.
 - De app-builds moeten worden uitgevoerd in de cloudservices en containers moeten zich bevinden in een persoonlijke bedrijfsbrede containerregister in de cloud.
 - De API-service die wordt gebruikt voor huisdier foto's moet nauwkeurige en betrouwbare in de praktijk omdat beslissingen door de app moeten worden uitgevoerd in hun hotels. Alle huisdier verleende toegang te blijven op de hotels is toegestaan.

## <a name="solution-design"></a>Het ontwerp van oplossing

Na het vastmaken omlaag hun doelstellingen en vereisten, Contoso ontwerpen van een implementatieoplossing controleren en identificeert het migratieproces, met inbegrip van de Azure-services die ze voor de migratie gaat gebruiken.

### <a name="current-app"></a>Huidige app

- De on-premises SmartHotel360 app is gelaagd over twee VM's (WEBVM en SQLVM).
- De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5)
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een on-premises domeincontroller (**contosodc1**).
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.


### <a name="proposed-architecture"></a>Voorgestelde architectuur

- De front-end van de app wordt geïmplementeerd als een Azure App Services-Web-app in de primaire regio.
- Een Azure-functie biedt uploads van huisdier foto's en de site communiceren met deze functionaliteit.
- De functie huisdier foto maakt gebruik van Cognitive Services-Vision-API en cosmos DB.
- De back-end van de site is gebouwd met behulp van microservices. Deze wordt geïmplementeerd op containers die worden beheerd in de Azure Kubernetes service (AKS).
- Containers worden gebouwd met behulp van VSTS, en gepusht naar Azure Container Registry (ACR).
- Op dit moment wordt de Web-app en functie-code met Visual Studio handmatig implementeren door Contoso.
- Microservices worden geïmplementeerd met behulp van een PowerShell-script waarmee Kubernetes opdrachtregelprogramma's wordt aangeroepen.

    ![Scenario-architectuur](./media/contoso-migration-rebuild/architecture.png) 

  
### <a name="solution-review"></a>Beoordelingen van oplossing
Contoso evalueert het voorgestelde ontwerp door het samenstellen van een lijst met voor- en nadelen.

**Overweging** | **Details**
--- | ---
**Professionals** | Met behulp van PaaS en serverloze oplossingen voor de end-to-end implementatie aanzienlijk minder beheer hoelang Contoso moet opgeven.<br/><br/> Verplaatsen naar een microservice-architectuur, kunt Contoso om uit te breiden eenvoudig hun oplossing na verloop van tijd.<br/><br/> Nieuwe functionaliteit kan online worden gebracht zonder dat een van de bestaande oplossingen codebases gegevenscentrum worden verstoord.<br/><br/> De Web-App worden geconfigureerd met meerdere exemplaren met geen enkel storingspunt.<br/><br/> Automatisch schalen worden ingeschakeld zodat de app kan verschillende verkeer volumes worden verwerkt.<br/><br/> Met de overgang naar de PaaS-services Contoso buiten gebruik stellen verouderde oplossingen die worden uitgevoerd op Windows Server 2008 R2-besturingssysteem.<br/><br/> Cosmos DB heeft ingebouwde fouttolerantie, waarvoor geen configuratie door Contoso. Dit betekent dat de gegevenslaag niet langer een single point of failover is.
**Nadelen** | Containers zijn complexer dan andere opties voor de migratie. Het leerproces is mogelijk een probleem voor Contoso.  Ze introduceren een nieuw niveau van complexiteit waarmee veel van de waarde er de curve.<br/><br/> Het operationele team bij Contoso moet aan de slag om te begrijpen en ondersteuning voor Azure, containers en microservices voor de app.<br/><br/> Contoso is niet volledig DevOps geïmplementeerd voor de gehele oplossing. Ze hoeven te denken over die voor de implementatie van services met AKS, functies en App-Services.



### <a name="migration-process"></a>Migratieproces

1. Contoso inrichten van de ACR, AKS en cosmos DB.
2. Het inrichten van de infrastructuur voor de implementatie, met inbegrip van de Azure-Web-App, opslagaccount, functie en API. 
3. Nadat de infrastructuur ingesteld is, wordt ze hun microservices bouwen met VSTS, waarbij de nieuwe versies naar het ACR-containerinstallatiekopieën.
4. Contoso implementeert deze microservices te vragen met behulp van een PowerShell-script.
5. Tot slot implementeert ze de Azure-functie en de Web-App.

    ![Migratieproces](./media/contoso-migration-rebuild/migration-process.png) 

### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[AKS](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Maakt eenvoudiger beheer, implementatie en bewerkingen van Kubernetes. Biedt een volledig beheerde Kubernetes-containerorganisatieservice.  | AKS is een gratis service.  Betaal voor alleen de virtuele machines, en de bijbehorende opslag en netwerkresources die worden verbruikt. [Meer informatie](https://azure.microsoft.com/pricing/details/kubernetes-service/).
[Azure Functions](https://azure.microsoft.com/services/functions/) | Versnelt de ontwikkeling met een gebeurtenisgestuurde, serverloze rekenervaring. Schalen op aanvraag.  | Betaal alleen voor de verbruikte resources. Plan, wordt in rekening gebracht op basis van per seconde resourceverbruik en uitvoeringen. [Meer informatie](https://azure.microsoft.com/pricing/details/functions/).
[Azure Container Registry](https://azure.microsoft.com/services/container-registry/) | Slaat de installatiekopieën voor alle typen containerimplementaties. | De kosten op basis van functies, opslag en duur van gebruik. [Meer informatie](https://azure.microsoft.com/pricing/details/container-registry/).
[Azure App Service](https://azure.microsoft.com/services/app-service/containers/) | Bouw, implementeer en schaal geavanceerde web-apps, mobiele apps en API-apps die op elk platform kunnen worden uitgevoerd. | Voor App Service-abonnementen worden de kosten per seconde in rekening gebracht. [Meer informatie](https://azure.microsoft.com/pricing/details/app-service/windows/).

## <a name="prerequisites"></a>Vereisten

Dit is wat u (en Contoso) nodig om uit te voeren in dit scenario:

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | U moet al een abonnement hebt gemaakt wanneer u de evaluatie uitgevoerd in de eerste artikel in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.
**Azure-infrastructuur** | [Informatie over hoe](contoso-migration-infrastructure.md) Contoso instellen van een Azure-infrastructuur.
**Developer-vereisten** | Contoso heeft de volgende hulpprogramma's op een werkstation ontwikkelaar nodig:<br/><br/> - [Visual Studio 2017 Community Edition: Versie 15.5](https://www.visualstudio.com/)<br/><br/> .NET-workload is ingeschakeld.<br/><br/> [Git](https://git-scm.com/)<br/><br/> [Azure PowerShell](https://azure.microsoft.com/downloads/)<br/><br/> [Azure-CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)<br/><br/> [Docker CE (Windows 10) of Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) ingesteld voor het gebruik van Windows-Containers.



## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso de migratie wordt uitgevoerd:

> [!div class="checklist"]
> * **Stap 1: Inrichten AKS en ACR**: Contoso richt de beheerde AKS-cluster en Azure-containerregister met behulp van PowerShell
> * **Stap 2: Bouw de Docker-containers**: ze CI voor Docker-containers met VSTS instellen en deze naar de ACR te pushen.
> * **Stap 3: Implementatie van back-end microservices**: implementatie van de rest van de infrastructuur die zal worden gebruikt door de back-end microservices.
> * **Stap 4: Een front-infrastructuur implementeren**: ze implementeren voor de front-end-infrastructuur, inclusief blob-opslag voor de huisdier telefoons, Cosmos DB en Vision-API.
> * **Stap 5: De back-end migreren**: ze microservices implementeren en uitvoeren in AKS, voor het migreren van de back-end.
> * **Stap 6: Publiceer de front-end**: ze de SmartHotel360-app publiceren naar de Azure App service en de functie-App die wordt aangeroepen door de huisdier-service.



## <a name="step-1-provision-an-aks-cluster-and-acr"></a>Stap 1: Een AKS-cluster en ACR inrichten

Contoso wordt uitgevoerd voor de implementatie van een script voor het maken van de beheerde Kubernetes-cluster met behulp van AKS en Azure Container Registry.

- De instructies voor deze sectie de **SmartHotel360-Azure-back-end** opslagplaats.
- De **SmartHotel360-Azure-back-end** GitHub-opslagplaats bevat alle van de software voor dit onderdeel van de implementatie.

Ze inrichten gaat als volgt:

1. Voordat ze, Contoso zorgt ervoor dat alle vereiste software is geïnstalleerd op de dev-machine dat wordt gebruikt. 
2. Ze kloon de opslagplaats lokaal naar de dev-machine met behulp van Git.

    **GIT-kloon https://github.com/Microsoft/SmartHotel360-Azure-backend.git**

3.  Contoso-Hiermee opent u de map met behulp van Visual Studio Code en wordt verplaatst naar de **/implementatie/k8s** directory waarin het script **gen-aks-env.ps1**.
4. Ze Voer het script voor het maken van de beheerde Kubernetes-cluster, met behulp van AKS en het Containerregister.

    ![AKS](./media/contoso-migration-rebuild/aks1.png)
 
5.  Open het bestand, ze de parameter $location bijwerken **eastus2**, en sla het bestand.

    ![AKS](./media/contoso-migration-rebuild/aks2.png)

6. Ze klikt u op **weergave** > **Integrated Terminal** de geïntegreerde terminal te openen in de Code.

    ![AKS](./media/contoso-migration-rebuild/aks3.png)

7. In de PowerShell-geïntegreerde terminal, ze zich aanmelden bij Azure met behulp van de opdracht Connect-AzureRmAccount. [Meer informatie](https://docs.microsoft.com/powershell/azure/get-started-azureps) over aan de slag met PowerShell.

    ![AKS](./media/contoso-migration-rebuild/aks4.png)

8. Verificatie van Azure CLI door het uitvoeren van de **az login** opdracht, en de instructies volgen om te verifiëren met behulp van hun webbrowser. [Meer informatie](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) punt zich heeft aangemeld met Azure CLI.

    ![AKS](./media/contoso-migration-rebuild/aks5.png)

9. Ze worden uitgevoerd de volgende opdracht, de naam van de resourcegroep van ContosoRG, de naam van het AKS-cluster smarthotel-aks-eus2 en de nieuwe naam van het containerregister te geven.

    ```
    .\gen-aks-env.ps1  -resourceGroupName ContosoRg -orchestratorName smarthotelakseus2 -registryName smarthotelacreus2
    ```

    ![AKS](./media/contoso-migration-rebuild/aks6.png)

10. Azure maakt een andere resourcegroep, met de resources voor het AKS-cluster.

    ![AKS](./media/contoso-migration-rebuild/aks7.png)

11. Nadat de implementatie is voltooid, installeert u Contoso de **kubectl** opdrachtregel-hulpprogramma. Het hulpprogramma is al geïnstalleerd op de Azure-CloudShell.

    **az aks install-cli**

12. Ze controleren of de verbinding met het cluster door het uitvoeren van de **kubectl ophalen knooppunten** opdracht. Het knooppunt is dezelfde naam als de virtuele machine in de automatisch gemaakte resourcegroep.

    ![AKS](./media/contoso-migration-rebuild/aks8.png)

13. Ze worden uitgevoerd de volgende opdracht start het Kubernetes-Dashboard: 

    **AZ aks bladeren--resourcegroep ContosoRG--naam smarthotelakseus2**

14. Er wordt een browsertabblad geopend op het Dashboard. Dit is een via een tunnel verbinding met de Azure CLI. 

    ![AKS](./media/contoso-migration-rebuild/aks9.png)




## <a name="step-2-build-a-docker-container"></a>Stap 2: Een Docker-container maken

### <a name="create-a-vsts-and-build"></a>Een VSTS maken en bouwen

Contoso een VSTS-project maakt en configureert u een CI-bouwen voor het maken van de container en wordt vervolgens naar de ACR gepusht. De instructies in deze sectie de [SmartHotel360-Azure-back-end](https://github.com/Microsoft/SmartHotel360-Azure-backend) opslagplaats.

1. Van visualstudio.com, ze een nieuw account maken (**contosodevops360.visualstudio.com**), en configureer deze voor het gebruik van Git.

2. Ze een nieuw project maken (**smarthotelrefactor**) met behulp van Git voor versiebeheer en Agile voor de werkstroom.

    ![VSTS](./media/contoso-migration-rebuild/vsts1.png) 


3. Ze importeren de GitHub-opslagplaats.

    ![VSTS](./media/contoso-migration-rebuild/vsts2.png)
    
4. In **Build and Release**, ze de definitie van een nieuwe VSTS Git gebruiken als een bron, vanuit de geïmporteerde opslagplaats maken. 

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

10. Contoso configureert nu de tweede taak Docker (te pushen). Ze selecteert u het abonnement en de **smarthotelacreus2** ACR. 

    ![VSTS](./media/contoso-migration-rebuild/vsts10.png)

11. Nogmaals, ze voert u het bestand naar het docker-compose.yaml-bestand en selecteer **service installatiekopieën pushen** en de meest recente code bevatten. Wanneer de actie is gewijzigd in **service installatiekopieën pushen**, de naam van de VSTS-taak wordt gewijzigd in **automatisch Push-services**

    ![VSTS](./media/contoso-migration-rebuild/vsts11.png)

12. Met de VSTS-taken is geconfigureerd, Contoso slaat de build-definitie en het bouwproces wordt gestart.

    ![VSTS](./media/contoso-migration-rebuild/vsts12.png)

13. Ze klikt u op de build-taak om voortgang te controleren.

    ![VSTS](./media/contoso-migration-rebuild/vsts13.png)

14. Nadat de build is voltooid, ziet u de ACR de nieuwe opslagplaatsen worden ingevuld met de containers die worden gebruikt door de microservices.

    ![VSTS](./media/contoso-migration-rebuild/vsts14.png)


## <a name="step-3-deploy-back-end-microservices"></a>Stap 3: Implementatie van back-end microservices

Met het AKS-cluster gemaakt en de Docker-installatiekopieën build implementeert Contoso nu de rest van de infrastructuur die zal worden gebruikt door de back-end microservices.

- Instructies in de sectie de [SmartHotel360-Azure-back-end](https://github.com/Microsoft/SmartHotel360-Azure-backend) opslagplaats.
- In de **/deploy/k8s/arm** map, wordt er één script om alle items te maken. 

Ze implementeren als volgt:

1. Contoso maakt gebruik van het bestand deploy.cmd voor het implementeren van de Azure-resources in de resourcegroep ContosoRG en EUS2 regio, door de volgende opdracht te typen:

    **. \deploy azuredeploy - c eastus2 ContosoRG**

    ![Back-end implementeren](./media/contoso-migration-rebuild/backend1.png)

2. Het vastleggen van de verbindingsreeks voor elke database, moet later worden gebruikt.

    ![Back-end implementeren](./media/contoso-migration-rebuild/backend2.png)

## <a name="step-4-deploy-front-end-infrastructure"></a>Stap 4: Een front-infrastructuur implementeren

Contoso moet de infrastructuur die wordt gebruikt door de front-end-apps te implementeren. Maken van een blob storage-container voor het opslaan van huisdier afbeeldingen. de Cosmos-database, voor het opslaan van documenten met de huisdier informatie; en de Vision-API voor de website. 

Instructies voor deze sectie de [SmartHotel-openbare-web](https://github.com/Microsoft/SmartHotel360-public-web) opslagplaats.

### <a name="create-storage-containers"></a>Storage-containers maken

1.  Contoso geopend in Azure portal het opslagaccount dat is gemaakt en klikt op **Blobs**.
2.  Ze een nieuwe container maken (**huisdieren**) waarbij het niveau van de openbare toegang is ingesteld op de container. Gebruikers wordt hun huisdier foto's uploaden naar deze container.

    ![Storage-blob](./media/contoso-migration-rebuild/blob1.png)

3. Maken van een tweede nieuwe container met de naam **instellingen**. Een bestand met de instellingen voor de app-front-end wordt in deze container worden geplaatst.

    ![Storage-blob](./media/contoso-migration-rebuild/blob2.png)

4. Het vastleggen van de toegangsgegevens voor het opslagaccount in een tekstbestand, voor toekomstig gebruik.

    ![Storage-blob](./media/contoso-migration-rebuild/blob2.png)

## <a name="provision-a-cosmos-database"></a>Een Cosmos-database inrichten

Contoso richt een Cosmos-database moet worden gebruikt voor huisdier informatie.

1. Het maken van een **Azure Cosmos DB** in de Azure Marketplace.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos1.png)

2. Ze een naam opgeven (**contosomarthotel**), selecteert u de SQL-API en plaats deze in de resourcegroep productie ContosoRG, in de regio VS-Oost 2.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos2.png)

3. Ze toevoegen een nieuwe verzameling aan de database met de standaardcapaciteit en doorvoer.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos3.png)


4. Ze Houd er rekening mee de verbindingsgegevens voor de database, voor toekomstig gebruik. 

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos4.png)


## <a name="provision-computer-vision"></a>Inrichten Computer Vision

Contoso voorziet de Computer Vision-API. De API wordt aangeroepen door de functie, om te evalueren afbeeldingen geüpload door gebruikers.

1. Het maken van een **Computer Vision** exemplaar in de Azure Marketplace.

     ![Computer Vision](./media/contoso-migration-rebuild/vision1.png)

2. Het inrichten van de API (**smarthotelpets**) in de productieresource groep ContosoRG, in de regio VS-Oost 2.

    ![Computer Vision](./media/contoso-migration-rebuild/vision2.png)

3. Ze opslaan de verbindingsinstellingen voor de API in een tekstbestand voor later gebruik.

     ![Computer Vision](./media/contoso-migration-rebuild/vision3.png)

## <a name="step-5-deploy-the-back-end-services-in-azure"></a>Stap 5: De back-end-services in Azure implementeren

Contoso moet nu de controller voor binnenkomend verkeer van NGINX voor het toestaan van binnenkomend verkeer naar de services en vervolgens implementeert de microservices op het AKS-cluster implementeren.

De instructies in deze sectie de [SmartHotel360-Azure-back-end](https://github.com/Microsoft/SmartHotel360-Azure-backend) opslagplaats.



1. Deze Visual Studio Code gebruiken om te werken de **/deploy/k8s/config_loal.yml** bestand. Ze werken de verschillende databaseverbindingen van informatie die ze eerder hebt genoteerd.

     ![Implementatie van microservices](./media/contoso-migration-rebuild/deploy-micro.png)

    > [!NOTE]
    > Enkele van de configuratie-instellingen (bijvoorbeeld Active Directory B2C) worden niet in dit artikel besproken. Er is meer informatie over deze instellingen in de opslagplaats.

2. Het bestand deploy.ps1 Hiermee verwijdert u alle inhoud van het cluster (met uitzondering van inkomend verkeer en controller voor binnenkomend verkeer), en implementeert de microservices. Ze voert deze als volgt uit:

  ```
  .\deploy.ps1 -configFile .\conf_local.yml -dockerUser smarthotelacreus2  -dockerPassword [passwordhere] -registry smarthotelacreus2.azurecr.io -imageTag latest -deployInfrastructure $false -buildImages $false -pushImages $false
  ```

3. Ze de volgende opdracht uit om te controleren of de status van services worden uitgevoerd:

    ```
    kubectl get services
    ```
4. Ze opent u het Kubernetes-dashboard om te controleren van de implementatie:

    ```
    az aks browse --resource-group ContosoRG --name smarthotelakseus2
    ```


## <a name="step-6-publish-the-frontend"></a>Stap 6: De front-end publiceren

Als laatste stap, wordt in Contoso de SmartHotel360-app gepubliceerd naar de Azure App Service en de functie-App die wordt aangeroepen door de Service huisdier.

De instructies in deze sectie de [SmartHotel-openbare-web-opslagplaats.](https://github.com/Microsoft/SmartHotel360-public-web) opslagplaats.

### <a name="set-up-web-app-to-use-contoso-resources"></a>Web-App ingesteld voor het gebruik van de Contoso-resources

1. Contoso kloont de opslagplaats lokaal naar de dev-machine met behulp van Git:

    **GIT-kloon https://github.com/Microsoft/SmartHotel360-public-web.git**

2. Ze open de map om weer te geven van alle bestanden in de opslagplaats in Visual Studio.

    ![Frontend publiceren](./media/contoso-migration-rebuild/front-publish1.png)

3. Deze maken wijzigingen in de configuratie die nodig zijn om de standaardinstelling te.

    - Wanneer de web-app wordt gestart, wordt er gezocht naar de **SettingsUrl** app-instelling.
    - Deze variabele moet een URL naar een configuratiebestand bevatten.
    - Standaard is de instelling die wordt gebruikt een openbaar eindpunt.

4. Deze update de **/config-sample.json/sample.json** bestand. Dit is het configuratiebestand voor het web wanneer u het openbare eindpunt.

    - Ze beide bewerken de **URL's** en **pets_config** secties, met de waarden voor de AKS-API-eindpunten, de storage-accounts en de Cosmos-database. 
    - De URL's moeten overeenkomen met de DNS-naam van de nieuwe web-app die Contoso maakt.
    - Dit is voor Contoso, **smarthotelcontoso.eastus2.cloudapp.azure.com**.

    ![Frontend publiceren](./media/contoso-migration-rebuild/front-publish2.png)

5. Nadat het bestand is bijgewerkt, ze Wijzig de naam **smarthotelsettingsurl**, en dit uploaden naar de opslag-blob ze eerder hebt gemaakt.

     ![Frontend publiceren](./media/contoso-migration-rebuild/front-publish3.png)

6. Ze klikt u op het bestand om de URL te verkrijgen. Deze URL wordt gebruikt door de app wanneer deze wordt gestart voor het ophalen van het configuratiebestand.

    ![Frontend publiceren](./media/contoso-migration-rebuild/front-publish4.png)

7. Ze werken **SettingsUrl** in de **appsettings. Production.JSON** bestanden naar de URL van het nieuwe bestand.

    ![Frontend publiceren](./media/contoso-migration-rebuild/front-publish5.png)


### <a name="deploy-the-website-to-the-azure-app-service"></a>De website kunt implementeren in Azure App Service

Contoso kan nu hun website publiceren.


1. Ze inschakelen in Application Insights-bewaking in Visual Studio 2017. Om dit te doen, selecteert ze de **PublicWeb** project in Solution Explorer en Ga naar **Application Insights toevoegen**. Ze de app te registreren met de Application Insights-exemplaar dat is gemaakt wanneer ze de infrastructuur geïmplementeerd.

    ![Website publiceren](./media/contoso-migration-rebuild/deploy-website1.png)

2. In Visual Studio ze verbinding maken met het project PublicWeb naar App Insights en bijwerken om weer te geven dat deze geconfigureerd.
 
    ![Website publiceren](./media/contoso-migration-rebuild/deploy-website2.png)

3. In Visual Studio, ze maken en publiceren van de web-app.

    ![Website publiceren](./media/contoso-migration-rebuild/deploy-website3.png)

5. Ze geven van een app-naam en plaats deze in de resourcegroep van de productie **ContosoRG**, in de regio VS-Oost 2.

    ![Website publiceren](./media/contoso-migration-rebuild/deploy-website4.png)

### <a name="deploy-the-function-to-azure"></a>De functie implementeren in Azure

1. Deze Visual Studio gebruiken om te maken en publiceren van de functie. U doet dit door ze met de rechtermuisknop op de **PetCheckerFunction** > **publiceren**. Vervolgens maken ze een nieuwe App Service-functie.

     ![De functie implementeren](./media/contoso-migration-rebuild/function1.png)

2. De naam van de opgegeven **smarthotelpetchecker**, en plaats deze in de resourcegroep ContosoRG en een nieuw app service-plan.

     ![De functie implementeren](./media/contoso-migration-rebuild/function2.png)

3. Selecteer de opslag-account en maakt u de functie.

    ![De functie implementeren](./media/contoso-migration-rebuild/function3.png)

4. De implementatie begint met het inrichten van de functie-app naar Azure. In **publiceren**, Contoso voegt de app-instellingen voor opslag, de Cosmos-database en de Computer Vision-API.

    ![De functie implementeren](./media/contoso-migration-rebuild/function4.png)

5. Als u wilt uitvoeren van de functie lokaal eerst, ze bijwerken van instellingen in **PetCheckerFunction/local.settings.json**.

    ![De functie implementeren](./media/contoso-migration-rebuild/function5.png)

6. Nadat de functie is geïmplementeerd, deze wordt weergegeven in de Azure-portal met de **met** status.

    ![De functie implementeren](./media/contoso-migration-rebuild/function6.png)


7. Ze bladeren naar de app om te testen of de app huisdier Checker werkt zoals verwacht op [ http://smarthotel360public.azurewebsites.net/Pets ](http://smarthotel360public.azurewebsites.net/Pets).
8. Ze klikt u op de avatar voor het uploaden van een afbeelding.

    ![De functie implementeren](./media/contoso-migration-rebuild/function7.png)

9. De eerste foto die ze willen controleren is van een kleine hond.

    ![De functie implementeren](./media/contoso-migration-rebuild/function8.png)

10. De app retourneert een bericht van acceptatie.

    ![De functie implementeren](./media/contoso-migration-rebuild/function9.png)




## <a name="review-the-deployment"></a>De implementatie controleren

Met de gemigreerde resources in Azure moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

- Contoso moet ervoor zorgen dat hun nieuwe databases beveiligd zijn. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- De app moet worden bijgewerkt voor het gebruik van SSL met certificaten. De containerexemplaar moet opnieuw worden geïmplementeerd om te reageren op 443.
- Ze moeten overwegen KeyVault-geheimen voor hun Service Fabric-apps beveiligen. [Meer informatie](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="backups-and-disaster-recovery"></a>Back-ups en herstel na noodgevallen

- Contoso moet voor een overzicht back-vereisten voor de Azure SQL-Database. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Ze moeten Overweeg de implementatie van SQL-failover-groepen als regionale failover voor de database. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Geo-replicatie voor de premium-SKU van de ACR kunnen gebruikmaken van. [Meer informatie](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication)

### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

- Nadat alle resources worden geïmplementeerd, Contoso Azure tags op basis van moet toewijzen hun [infrastructuur plannen](contoso-migration-infrastructure.md#set-up-tagging).
- Alle licenties zijn ingebouwd in de kosten van de PaaS-services die van Contoso gebruikmaakt al. Dit wordt afgetrokken van de EA.
- Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing die u helpt bij het gebruiken en beheren van Azure en andere cloudresources.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management.

## <a name="conclusion"></a>Conclusie

In dit artikel opnieuw Contoso de SmartHotel360-app in Azure. Ze opnieuw opgebouwd met de on-premises app front-end virtuele machine in Azure App Services-Web-apps. Ze de app back-end met behulp van microservices is geïmplementeerd in containers die worden beheerd door Azure Kubernetes Service (AKS) gebouwd. Deze uitgebreide functionaliteit van een app met een huisdier foto-app.




