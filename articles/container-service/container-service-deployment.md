---
title: Een Docker-containercluster implementeren in Azure | Microsoft Docs
description: Een Azure Container Service-cluster implementeren met behulp van Azure Portal of een Azure Resource Manager-sjabloon.
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
ms.date: 02/21/2017
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 2a381431acb6436ddd8e13c69b05423a33cd4fa6
ms.openlocfilehash: b9be92498f9daf1d2f964cc689bacb2358b237be


---
# <a name="deploy-an-azure-container-service-cluster"></a>Een Azure Container Service-cluster implementeren



Azure Container Service biedt een snelle implementatie van populaire open-sourceoplossingen voor containerclustering en -orchestration. In dit document vindt u instructies voor het implementeren van een Azure Container Service-cluster met behulp van Azure Portal of een Azure Resource Manager-snelstartsjabloon. 

U kunt ook een Azure Container Service-cluster implementeren met behulp van de [Azure CLI 2.0](container-service-create-acs-cluster-cli.md) of de Azure Container Service-API's.



## <a name="prerequisites"></a>Vereisten

* **Azure-abonnement**: als u nog geen abonnement hebt, kunt u zich registreren voor een [gratis proefversie](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

* **Openbare SSH RSA-sleutel**: wanneer u implementeert via de portal of een van de snelstartsjablonen van Azure, moet u de openbare sleutel opgeven voor verificatie bij de virtuele machines van Azure Container Service. Als u SSH RSA-sleutels (Secure Shell) wilt maken, ziet u de richtlijnen voor [OS X en Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) of [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md). 

* **Service-principal-client-id en -geheim** (alleen Kubernetes): zie [Informatie over de service-principal voor een Kubernetes-cluster](container-service-kubernetes-service-principal.md) voor meer informatie over en richtlijnen voor het maken van een service-principal.



## <a name="create-a-cluster-by-using-the-azure-portal"></a>Een cluster maken met Azure Portal
1. Meld u aan bij Azure Portal, selecteer **New** en zoek in Azure Marketplace naar **Azure Container Service**.

    ![Azure Container Service in Marketplace](media/container-service-deployment/acs-portal1.png)  <br />

2. Selecteer **Azure Container Service** en klik op **Create**.

    ![Een Container Service maken](media/container-service-deployment/acs-portal2.png)  <br />

3. Voer de volgende informatie in:

    * **User name**: de gebruikersnaam voor een account op elk van de virtuele machines en virtuele-machineschaalsets in het Azure Container Service-cluster.
    * **Subscription**: selecteer een Azure-abonnement.
    * **Resource group**: selecteer een bestaande resourcegroep of maak een nieuwe. Als best practice gebruikt u een nieuwe resourcegroep voor elke implementatie.
    * **Location**: selecteer een Azure-regio voor de Azure Container Service-implementatie.
    * **SSH RSA public key**: voeg de openbare sleutel toe die wordt gebruikt voor verificatie bij de virtuele machines voor Azure Container Service. Het is belangrijk dat deze sleutel geen regeleinden bevat, maar wel het voorvoegsel `ssh-rsa`. Het achtervoegsel `username@domain` is optioneel. De sleutel moet er ongeveer zo uitzien: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**. 

4. Klik op **OK** wanneer u klaar bent om door te gaan.

    ![Basisinstellingen](media/container-service-deployment/acs-portal3.png)  <br />

5. Op de blade **Framework-configuratie** selecteert u een **orchestrator-configuratie**. De opties zijn:

  * **DC/OS**: implementeert een DC/OS-cluster.
  * **Swarm**: implementeert een Docker Swarm-cluster.
  * **Kubernetes**: implementeert een Kubernetes-cluster.


6. Klik op **OK** wanneer u klaar bent om door te gaan.

    ![Een orchestrator kiezen](media/container-service-deployment/acs-portal4-new.png)  <br />

7. Als in de vervolgkeuzelijst de optie **Kubernetes** is geselecteerd, moet u de client-id ook wel appId genoemd) en het clientgeheim (het wachtwoord) van de service-principal invoeren. Zie [Over de service-principal voor een Kubernetes-cluster](container-service-kubernetes-service-principal.md) voor meer informatie.

    ![Service-principal voor Kubernetes invoeren](media/container-service-deployment/acs-portal10.png)  <br />

7. Voer de volgende gegevens in op de blade **Azure Container Service-instellingen**:

    * **Master count**: het aantal masters in het cluster.
    * **Agent count**: voor Docker Swarm en Kubernetes is dit het oorspronkelijke aantal agents in de agentschaalset. Voor DC/OS is dit het oorspronkelijke aantal agents in een persoonlijke schaalset. Bovendien wordt een openbare schaalset gemaakt voor DC/OS, die een vooraf bepaald aantal agents bevat. Het aantal agents in deze openbare schaalset wordt bepaald door het aantal masters dat in het cluster is gemaakt: één openbare agent voor één master en twee openbare agents voor drie of vijf masters.
    * **Agent virtual machine size**: de grootte van de virtuele machines van de agent.
    * **DNS prefix**: een wereldwijd unieke naam die wordt gebruikt om sleutelonderdelen toe te voegen voor de volledig gekwalificeerde domeinnamen voor de service.
    * **VM diagnostics**: bij sommige orchestrators kunt u ervoor kiezen om VM-diagnostiek in te schakelen.

8. Klik op **OK** wanneer u klaar bent om door te gaan.

    ![Container Service-instellingen](media/container-service-deployment/acs-portal5.png)  <br />

9. Klik op **OK** nadat de servicevalidatie is voltooid.

    ![Validatie](media/container-service-deployment/acs-portal6.png)  <br />

10. Lees de voorwaarden. Klik op **Purchase** om het implementatieproces te starten.

    ![Kopen](media/container-service-deployment/acs-portal7.png)  <br />

    Als u ervoor gekozen hebt om de implementatie aan Azure Portal vast te maken, kunt u de status van de implementatie zien.

    ![Implementatiestatus](media/container-service-deployment/acs-portal8.png)  <br />

Het duurt enkele minuten om de implementatie te voltooien. Daarna is het Azure Container Service-cluster klaar voor gebruik.



## <a name="create-a-cluster-by-using-a-quickstart-template"></a>Een cluster maken met het snelstartsjabloon
Er zijn Azure-snelstartsjablonen beschikbaar voor het implementeren van clusters in Azure Container Service. De verstrekte snelstartsjablonen kunnen worden aangepast om aanvullende of geavanceerde Azure-configuratie toe te voegen. Als u een Azure Container Service-cluster wilt maken met een Azure-snelstartsjabloon, hebt u een Azure-abonnement nodig. Als u er geen hebt, kunt u zich [registreren voor een gratis proefversie](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). 

Volg deze stappen voor het implementeren van een cluster middels een sjabloon en Azure CLI 2.0 (zie de [instructies voor installeren en instellen](/cli/azure/install-az-cli2.md)).

> [!NOTE] 
> Als u een Windows-systeem gebruikt, kunt u vergelijkbare stappen volgen voor het implementeren van een sjabloon, maar dan via Azure PowerShell. De stappen worden verderop in dit gedeelte beschreven. U kunt sjablonen ook implementeren via de [portal](../azure-resource-manager/resource-group-template-deploy-portal.md) of op een andere manier.

1. Selecteer een van de beschikbare snelstartsjablonen van GitHub om een DC/OS-, Docker Swarm- of Kubernetes-cluster te implementeren. Hierna volgt een gedeeltelijke lijst. De DC/OS- en Swarm-sjablonen zijn hetzelfde, behalve voor wat betreft de standaardselectie voor de orchestrator.

    * [DC/OS-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [Swarm-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [Kubernetes-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. Meld u aan bij uw Azure-account (`az login`) en zorg ervoor dat de Azure CLI is gekoppeld aan uw Azure-abonnement. U kunt het standaardabonnement bekijken met behulp van de volgende opdracht:

    ```azurecli
    az account show
    ```
    
    Als u meer dan één abonnement hebt en u een ander standaardabonnement wilt instellen, voert u `az account set --subscription` uit en geeft u de id of de naam van het abonnement op.

3. Als best practice kunt u een nieuwe resourcegroep gebruiken voor de implementatie. Voor het maken van een resourcegroep gebruikt u de opdracht `az group create` en geeft u een resourcegroepnaam en -locatie op: 

    ```azurecli
    az group create --name "RESOURCE_GROUP" --location "LOCATION"
    ```

4. Maak een JSON-bestand met de vereiste sjabloonparameters. Download het parameterbestand met de naam `azuredeploy.parameters.json` dat bij de Azure Container Service-sjabloon `azuredeploy.json` staat in GitHub. Voer de vereiste parameterwaarden voor uw cluster. 

    Als u bijvoorbeeld de [DC/OS-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos) wilt gebruiken, geeft u parameterwaarden op voor `dnsNamePrefix` en `sshRSAPublicKey`. Bekijk de beschrijvingen in `azuredeploy.json` en de opties voor andere parameters.  
 

5. Maak een Container Service-cluster door de volgende opdracht te gebruiken voor het bestand met implementatieparameters:

    * **RESOURCE_GROUP** is de naam is van de resourcegroep die u hebt gemaakt tijdens de vorige stap.
    * **DEPLOYMENT_NAME** (optioneel) is de naam die u de implementatie geeft.
    * **TEMPLATE_URI** is de locatie van het implementatiebestand `azuredeploy.json`. Deze URI moet het RAW-bestand zijn en geen verwijzing naar de GitHub-gebruikersinterface. U vindt deze URI door het bestand `azuredeploy.json` te selecteren in GitHub en op de knop **Raw** te klikken.  

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters @azuredeploy.parameters.json
    ```

    U kunt in de opdrachtregel ook parameters opgeven als een tekenreeks met JSON-indeling. Gebruik een opdracht die hierop lijkt:

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters "{ \"param1\": {\"value1\"} … }"
    ```

    > [!NOTE]
    > Het duurt enkele minuten om de implementatie te voltooien.
    > 

### <a name="equivalent-powershell-commands"></a>Vergelijkbare PowerShell-opdrachten
U kunt een Azure Container Service-clustersjabloon ook implementeren met PowerShell. Dit document is gebaseerd op versie 1.0 van de [Azure PowerShell-module](https://azure.microsoft.com/blog/azps-1-0/).

1. Selecteer een van de beschikbare snelstartsjablonen van GitHub om een DC/OS-, Docker Swarm- of Kubernetes-cluster te implementeren. Hierna volgt een gedeeltelijke lijst. De DC/OS- en Swarm-sjablonen zijn hetzelfde, behalve voor wat betreft de standaardselectie voor de orchestrator.

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

4. Als best practice kunt u een nieuwe resourcegroep gebruiken voor de implementatie. Voor het maken van een resourcegroep gebruikt u de opdracht `New-AzureRmResourceGroup` en geeft u een resourcegroepnaam en bestemmingsregio op:

    ```powershell
    New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
    ```

5. Nadat u een resourcegroep hebt gemaakt, kunt u uw cluster maken met de volgende opdracht. De URI van de gewenste sjabloon wordt opgegeven voor de `-TemplateUri`-parameter. Wanneer u deze opdracht uitvoert, vraagt PowerShell u de parameterwaarden voor de implementatie op te geven.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
    ```

#### <a name="provide-template-parameters"></a>Sjabloonparameters opgeven
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



<!--HONumber=Feb17_HO4-->


