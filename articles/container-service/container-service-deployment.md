---
title: Een Azure Container Service-cluster implementeren | Microsoft Docs
description: Een Azure Container Service-cluster implementeren met behulp van Azure Portal, de Azure CLI of PowerShell.
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Mesos, Azure, dcos, swarm, kubernetes, azure container service, acs
ms.assetid: 696a736f-9299-4613-88c6-7177089cfc23
ms.service: container-service
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: dc7ce9f0524567b861a40940f02cd07e0b7b22bf
ms.openlocfilehash: 52331c92a4e3e254c044c9cba85a937b6ba95011


---
# <a name="deploy-an-azure-container-service-cluster"></a>Een Azure Container Service-cluster implementeren
Azure Container Service biedt een snelle implementatie van populaire open-sourceoplossingen voor containerclustering en -orchestration. Met behulp van Azure Container Service kunt u DC/OS-, Kubernetes- en Docker Swarm-clusters implementeren met Azure Resource Manager-sjablonen of Azure Portal. U implementeert deze clusters met behulp van Azure Virtual Machine-schaalsets, en de clusters profiteren van het Azure-aanbod voor netwerken en opslag. U hebt een Azure-abonnement nodig om toegang te krijgen tot Azure Container Service. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefabonnement](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

> [!NOTE]
> Ondersteuning voor Kubernetes in Azure Container Service is momenteel in de preview-fase.
>

In dit document vindt u instructies voor het implementeren van een Azure Container Service-cluster met behulp van [Azure Portal](#creating-a-service-using-the-azure-portal), de [Azure-opdrachtregelinterface (CLI)](#creating-a-service-using-the-azure-cli) en de [Azure PowerShell-module](#creating-a-service-using-powershell).  

## <a name="create-a-service-by-using-the-azure-portal"></a>Een service maken met Azure Portal
1. Meld u aan bij Azure Portal, selecteer **New** en zoek in Azure Marketplace naar **Azure Container Service**.

    ![Implementatie maken - 1](media/acs-portal1.png)  <br />

2. Selecteer **Azure Container Service** en klik op **Create**.

    ![Implementatie maken - 2](media/acs-portal2.png)  <br />

3. Voer de volgende informatie in:

    * **User name**: dit is de gebruikersnaam die wordt gebruikt voor een account op elk van de virtuele machines en virtuele-machineschaalsets in het Azure Container Service-cluster.
    * **Subscription**: selecteer een Azure-abonnement.
    * **Resource group**: selecteer een bestaande resourcegroep of maak een nieuwe.
    * **Location**: selecteer een Azure-regio voor de Azure Container Service-implementatie.
    * **SSH public key**: voeg de openbare sleutel toe die wordt gebruikt voor verificatie bij de virtuele machines voor Azure Container Service. Het is heel belangrijk dat deze sleutel geen regeleinden bevat, maar wel de prefix 'ssh-rsa' en de postfix 'username@domain'. Het moet er ongeveer zo uitzien: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**. Zie de artikelen voor [Linux](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) en [Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/) voor informatie over het maken van SSH-sleutels.

4. Klik op **OK** wanneer u klaar bent om door te gaan.

    ![Implementatie maken - 3](media/acs-portal3.png)  <br />

5. Selecteer een Orchestration-type. De opties zijn:

    * **DC/OS**: implementeert een DC/OS-cluster.
    * **Swarm**: implementeert een Docker Swarm-cluster.
    * **Kubernetes**: implementeert een Kubernetes-cluster.

6. Klik op **OK** wanneer u klaar bent om door te gaan.

    ![Implementatie maken - 4](media/acs-portal4-new.png)  <br />

7. Als in de vervolgkeuzelijst de optie **Kubernetes** is geselecteerd, moet u de client-id en het clientgeheim van de service-principal invoeren. Zie [Over de service-principal voor een Kubernetes-cluster](container-service-kubernetes-service-principal.md) voor meer informatie.

    ![Implementatie maken - 4.5](media/acs-portal10.PNG)  <br />

7. Voer de volgende gegevens in op de blade **Azure Container Service**-instellingen:

    * **Master count**: het aantal masters in het cluster. Als Kubernetes is geselecteerd, wordt het aantal masters ingesteld op een standaardwaarde van 1
    * **Agent count**: voor Docker Swarm en Kubernetes is dit het oorspronkelijke aantal agents in de agentschaalset. Voor DC/OS is dit het oorspronkelijke aantal agents in een persoonlijke schaalset. Bovendien wordt een openbare schaalset gemaakt, die een vooraf bepaald aantal agents bevat. Het aantal agents in deze openbare schaalset wordt bepaald door het aantal masters dat in het cluster is gemaakt: één openbare agent voor één master en twee openbare agents voor drie of vijf masters.
    * **Agent virtual machine size**: de grootte van de virtuele machines van de agent.
    * **DNS prefix**: een wereldwijd unieke naam die wordt gebruikt om sleutelonderdelen toe te voegen voor de volledig gekwalificeerde domeinnamen voor de service.

8. Klik op **OK** wanneer u klaar bent om door te gaan.

    ![Implementatie maken - 5](media/acs-portal5.png)  <br />

9. Klik op **OK** nadat de servicevalidatie is voltooid.

    ![Implementatie maken - 6](media/acs-portal6.png)  <br />

10. Klik op **Purchase** om het implementatieproces te starten.

    ![Implementatie maken - 7](media/acs-portal7.png)  <br />

    Als u ervoor gekozen hebt om de implementatie aan Azure Portal vast te maken, kunt u de status van de implementatie zien.

    ![Implementatie maken - 8](media/acs-portal8.png)  <br />

Wanneer de implementatie is voltooid, is het Azure Container Service-cluster klaar voor gebruik.

## <a name="create-a-service-by-using-the-azure-cli"></a>Een service maken met behulp van de Azure CLI
Wanneer u een Azure Container Service-exemplaar wilt maken met behulp van de opdrachtregel, hebt u een Azure-abonnement nodig. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefabonnement](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). Ook moet de Azure CLI [geïnstalleerd](../xplat-cli-install.md) en [geconfigureerd](../xplat-cli-connect.md) zijn.

1. Selecteer een van de volgende sjablonen van GitHub om een DC/OS-, Docker Swarm- of Kubernetes-cluster te implementeren. 

    * [DC/OS-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [Swarm-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [Kubernetes-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. Controleer vervolgens of de Azure CLI is verbonden met een Azure-abonnement. U gebruikt hiervoor de volgende opdracht:

    ```bash
    azure account show
    ```
    Als er geen Azure-account wordt geretourneerd, gebruikt u de volgende opdracht om de CLI aan te melden bij Azure.

    ```bash
    azure login -u user@domain.com
    ```

3. Configureer de Azure CLI-hulpprogramma's voor het gebruik van Azure Resource Manager.

    ```bash
    azure config mode arm
    ```

4. Maak een Azure-resourcegroep- en Container Service-cluster met de volgende opdracht, waarbij:

    * **RESOURCE_GROUP** de naam is van de resourcegroep die u wilt gebruiken voor deze service.
    * **LOCATION** de Azure-regio is waar de resourcegroep en Azure Container Service-implementatie wordt gemaakt.
    * **TEMPLATE_URI** de locatie is van het implementatiebestand. Dit moet het Raw-bestand zijn, niet een verwijzing naar de GitHub-gebruikersinterface. U vindt deze URL door het bestand azuredeploy.json te selecteren in GitHub en op de knop **Raw** te klikken.

    > [!NOTE]
    > Wanneer u deze opdracht uitvoert, vraagt de shell u parameterwaarden op te geven voor de implementatie.
    > 

    ```bash
    azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
    ```

### <a name="provide-template-parameters"></a>Sjabloonparameters opgeven
Voor deze versie van de opdracht moet u parameters interactief definiëren. Als u parameters wilt opgeven, zoals een tekenreeks in JSON-indeling, kunt u dit doen met behulp van de `-p`-switch. Bijvoorbeeld:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

U kunt ook parameters in JSON-indeling opgeven met behulp van de `-e`-switch:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

U kunt een voorbeeldparameterbestand met de naam `azuredeploy.parameters.json` bekijken door dit in GitHub op te zoeken met de Azure Container Service-sjablonen.

## <a name="create-a-service-by-using-powershell"></a>Een service maken met PowerShell
U kunt een Azure Container Service-cluster ook implementeren met PowerShell. Dit document is gebaseerd op versie 1.0 van de [Azure PowerShell-module](https://azure.microsoft.com/blog/azps-1-0/).

1. Selecteer een van de volgende sjablonen om een DC/OS-, Docker Swarm- of Kubernetes-cluster te implementeren. Deze sjablonen zijn hetzelfde, behalve de standaardselectie voor orchestrator.

    * [DC/OS-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [Swarm-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [Kubernetes-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. Controleer voordat u een cluster maakt in uw Azure-abonnement, of uw PowerShell-sessie is aangemeld bij Azure. U kunt dit doen met de opdracht `Get-AzureRMSubscription`:

    ```powershell
    Get-AzureRmSubscription
    ```

3. Als u zich bij Azure wilt aanmelden, gebruikt u de opdracht `Login-AzureRMAccount`:

    ```powershell
    Login-AzureRmAccount
    ```

4. Als u naar een nieuwe resourcegroep implementeert, moet u eerst de resourcegroep maken. Voor het maken van een nieuwe resourcegroep gebruikt u de opdracht `New-AzureRmResourceGroup` en geeft u een resourcegroep en bestemmingsregio op:

    ```powershell
    New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
    ```

5. Nadat u een resourcegroep hebt gemaakt, kunt u uw cluster maken met de volgende opdracht. De URI van de gewenste sjabloon wordt opgegeven voor de `-TemplateUri`-parameter. Wanneer u deze opdracht uitvoert, vraagt PowerShell u de parameterwaarden voor de implementatie op te geven.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
    ```

### <a name="provide-template-parameters"></a>Sjabloonparameters opgeven
Als u bekend bent met PowerShell, weet u dat u door de beschikbare parameters voor een cmdlet kunt lopen door een minteken (-) te typen en dan op de tabtoets te drukken. Dezelfde functionaliteit werkt ook met parameters die u in de sjabloon definieert. Zodra u de sjabloonnaam gaat typen, haalt de cmdlet de sjabloon op, parseert de parameters en voegt de sjabloonparameters dynamisch aan de opdracht toe. Dit maakt het heel eenvoudig om sjabloonparameterwaarden op te geven. En als u de waarde van een vereiste parameter bent vergeten, vraagt PowerShell u om de waarde.

Hieronder vindt u de volledige opdracht, inclusief parameters. U kunt uw eigen waarden opgeven voor de namen van de resources.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Volgende stappen
Nu u een werkend cluster hebt, kunt u deze documenten lezen voor meer informatie over verbinding en beheer:

* [Verbinding maken met een Azure Container Service-cluster](container-service-connect.md)
* [Werken met de Azure Container Service en DC/OS](container-service-mesos-marathon-rest.md)
* [Werken met de Azure Container Service en Docker Swarm](container-service-docker-swarm.md)
* [Werken met de Azure Container Service en Kubernetes](container-service-kubernetes-walkthrough.md)




<!--HONumber=Dec16_HO3-->


